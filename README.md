## Impact Measurement Data Product
#### Sales data to Activity Mapping [DE]
---
##### Objective:
This document outlines the approach for mapping Sales data with Promotional Activity data. It provides a structured methodology to integrate sales data with various promotional activities, such as Calls, Emails, Events, and Newsletters by aligning these datasets at the sub-territory, segment, and month level.

> **Pre-requisite** - Users should have access to `AZ_DE_PROD.DSCI_RAW`, `AZ_DE_PROD.DSCI_ANALYTICS`  & `AZ_DE_PROD.LOCAL_BI_DSCI_GERMANY` schema to access the tables mentioned below. To raise access, please submit a ticket on [go/it4u](https://bayersi.service-now.com/sp)

Data Sets: Following are the data sets that we are using-

##### 1. Sales Data:
This dataset contains sales data prepared by country team (DSCI-Germany), structured at the sub-territory and month level. It includes multiple key fields capturing sales performance, product details and time information.The data enables comprehensive analysis of sales trends and territory-level performance at a brand level.

---
- Sales Table Name: `AZ_DE_PROD.DSCI_RAW.VIEW_REGIONAL_DATA`
- Key fields & description - 



| **Field Name**        | **Description**                                                                 |
|-----------------------|---------------------------------------------------------------------------------|
| `COUNTRY_CD`         | Country code (ISO Alpha-2 format)                                               |
| `RGNL_CLUSTER`       | Regional cluster identifier                                                      |
| `SALE_DIV`           | Sales division                                                                  |
| `SKU_NM`             | Stock Keeping Unit (SKU) name                                                    |
| `SOURCE_PROD_ID`     | Unique identifier for the source product                                         |
| `BRAND`              | Brand name associated with the product                                           |
| `GEO_LVL_1_ID`       | Primary geographic identifier (Sub-territory)                                   |
| `GEO_LVL_1_NM`       | Name of the primary geographic location (Sub-territory)                         |
| `GEO_LVL_1_TYP_NM`   | Type of the geographic area (e.g., Sub-territory)                               |
| `TM_PD`              | Reporting time period                                                           |
| `TM_PD_DT`           | Date representing the reporting period                                           |
| `TRANS_DATA_CATG_NM` | Transaction data category (e.g., Prescriptions, SHI/PHI Prescriptions)          |
| `DOSAGE_CNT`         | Total dosage count                                                               |
| `CTNG_UNIT_CNT`      | Counting unit total                                                              |
| `UNIT_CNT`           | Total units sold                                                                 |
| `AMT_LCL_CURR`       | Sales amount in local currency                                                   |
| `CVRT_UNIT`          | Conversion unit value                                                            |
| `CONVERSION_FACTOR`  | Factor applied for unit conversion                                               |

Sample query to pull Kerendia sales data from table at a sub-territory level:
This query extracts **Kerendia** sales data from the `AZ_DE_PROD.DSCI_RAW.VIEW_REGIONAL_DATA` table, aggregated at the **sub-territory** and **month** level, focusing on **Prescriptions** from **January 1, 2024** onward.

```sql
SELECT 
    geo_lvl_1_id,            -- Sub-territory ID
    geo_lvl_1_nm,            -- Sub-territory name
    geo_lvl_1_typ_nm,        -- Type of geographic area (e.g., Subterritory)
    brand,                   -- Brand name
    tm_pd,                   -- Reporting time period
    tm_pd_dt,                -- Reporting date
    SUM(unit_cnt) AS unit_cnt,                -- Total units sold
    SUM(amt_lcl_curr) AS amt_lcl_curr         -- Total sales amount (local currency)
FROM 
    az_de_prod.dsci_raw.view_regional_data
WHERE 
    geo_lvl_1_typ_nm = 'Subterritory'
    AND brand LIKE '%KERENDIA%'
    AND tm_pd_dt >= '2024-01-01'
    AND trans_data_catg_nm = 'Prescriptions'
GROUP BY 
    geo_lvl_1_id,
    geo_lvl_1_nm,
    geo_lvl_1_typ_nm,
    brand,
    tm_pd,
    tm_pd_dt;
```

##### 2. Promotional Activity Data Summary:
This dataset captures promotional activity touchpoints conducted across various channels, including Calls, Emails, Events, and Newsletters. The data is structured at the segment, sub-territory, and month level, providing a detailed view of engagement trends. Additionally, it includes key attributes such as Brand, Segments, Product Adoption, KV, and State level insights. 
 This dataset enables a comprehensive analysis of promotional effectiveness, customer engagement patterns, and the impact of marketing initiatives across different geographies and product categories, supporting strategic decision-making and performance optimization.
 Activity data contains HCP, Brand & Geography related fields.

---

**Table Name:** `AZ_DE_PROD.LOCAL_BI_DSCI_GERMANY.TABLE_TP_DETAIL`  

The following key fields are included in the dataset:

| **Field Name**       | **Description**                                                               |
|----------------------|-------------------------------------------------------------------------------|
| `COUNTRY_CODE`       | Country code (ISO Alpha-2 format)                                             |
| `CONTACT_ID`         | Unique identifier for the account or contact                                  |
| `BRAND_NAME`         | Name of the associated brand                                                  |
| `SEGMENT`            | Customer segment classification                                               |
| `SUBTERRITORY`       | Subdivision within a sales territory                                          |
| `STATE`             | State or province of the contact/account                                       |
| `ACTIVITY_TYPE`      | Type of engagement or marketing activity (e.g., Call, Event, Email)           |
| `DATE`              | Exact date of the activity or interaction                                      |
| `MONTH_DATE`         | Month-level representation of the activity date                               |
| `BUSINESS_FUNCTION`  | Business function associated with the activity (e.g., Commercial, Medical)    |
| `TOUCHPOINTS`        | Number of customer interactions or engagement instances                       |


Sample query to pull commercial activity data at Brand level:
```sql
select 
    brand_name,
    sum(case when activity_type='Call' then touchpoints end) as f2f_calls,
    sum(case when activity_type='Phone' then touchpoints end) as phone_calls,
    sum(case when activity_type='Approved Email' then touchpoints end) as ae_opened,
    sum(case when activity_type='Event' then touchpoints end) as event_attendees,
    sum(case when activity_type='Email Newsletter' then touchpoints end) as nl_opened
from AZ_DE_PROD.LOCAL_BI_DSCI_GERMANY.TABLE_TP_DETAIL
where brand_name in ('Kerendia','Nubeqa','Eylea 8mg') 
    and business_function='commercial' 
    and date like '%2024%'
group by 1;
```

##### Promotional Activity Data Overview  

The table `TABLE_TP_DETAIL` serves as the **primary source** for capturing **promotional activity data**, curated and maintained by the **DSCI-Germany team**.

> **Note:** There is no formal documentation available for `TABLE_TP_DETAIL`.  
> To ensure accuracy and understand the underlying logic, the business rules and filters were backtracked from the view **`VIEW_TP_DETAIL`**.

##### Applied Filters for Activity Data Extraction:  
The following business rules are applied to extract valid promotional activity records across different channels (such as **Calls**, **Emails**, **Events**, and **Newsletters**):

- Filter definitions were derived by tracing the logic and transformations applied within `VIEW_TP_DETAIL`.
- Filters ensure only **submitted, relevant, and validated activities** are captured.
- Channel-specific criteria (defined in detail in subsequent sections) refine the data to include only meaningful touchpoints.

##### F2F Calls - 
**Source Table:** `CPH_DB_PROD.ANALYTICS.FACT_CALL`

The following criteria are applied to identify valid **Face-to-Face (F2F)** call activities:

| **#** | **Condition**                                              | **Description**                                |
|-------|-------------------------------------------------------------|------------------------------------------------|
| 1     | `call_status = 'Submitted'`                                 | Only include calls that have been submitted.  |
| 2     | `nc_record_type LIKE '%Face2Face%'`                         | Include records classified as Face-to-Face.   |
| 3     | `nc_record_type NOT LIKE '%Non_Face2Face%'`                 | Exclude records marked as Non-Face-to-Face.   |
| 4     | `OR nc_record_type LIKE '%Remote%'`                         | Alternatively, include records marked Remote. |

    
##### Phone Calls  
**Source Table:** `CPH_DB_PROD.ANALYTICS.FACT_CALL`

The following filters are applied to identify valid **Phone Call** activities:

| **#** | **Condition**                                    | **Description**                                |
|-------|-------------------------------------------------|------------------------------------------------|
| 1     | `call_status = 'Submitted'`                    | Include only calls that have been submitted.  |
| 2     | `call_med_discussion_type = 'Phone'`           | Filter for calls classified as Phone discussions. |
| 3     | `nc_record_type LIKE '%Non_Face2Face%'`        | Ensure the call is marked as a Non-Face-to-Face interaction. |


##### Approved Emails  
**Source Table:** `CPH_DB_PROD.ANALYTICS.FACT_APPROVED_EMAIL`
The following criteria are applied to identify valid **Approved Email** interactions:

| **#** | **Condition**                                                           | **Description**                                 |
|-------|--------------------------------------------------------------------------|-------------------------------------------------|
| 1     | `approved_doc_record_type = 'Email Template'`                          | Filters for communications based on approved email templates. |
| 2     | `(email_opened_flag = TRUE OR email_clicked_flag = TRUE)`              | Includes emails where the recipient either opened or clicked the email. |


##### Events  
**Source Tables:**  
- `CPH_DB_PROD.ANALYTICS.DIM_EVENT`  
- `CPH_DB_PROD.ANALYTICS.FACT_EVENT_ATTENDEE`  

The following filters are applied to identify valid **Event** participation records:

| **#** | **Condition**                                                                            | **Description**                                        |
|-------|------------------------------------------------------------------------------------------|--------------------------------------------------------|
| 1     | `event_status IN ('Closed', 'Approved')`                                                 | Includes only events that are completed and approved.  |
| 2     | `event_attendee_deleted_flag = 0`                                                        | Ensures only active (non-deleted) attendee records.   |
| 3     | `event_attendee_status IN ('Attended', 'Cleared Signature', 'Signed')`                  | Filters for attendees who were confirmed as present.  |
| 4     | `event_user_emp_key IS NULL`                                                             

##### Newsletters  
**Source Table:**  
- `CPH_DB_PROD.ANALYTICS.FACT_PERSONAL_CHANNEL_ACTIVITY`

The following filters are applied to identify valid **Newsletter** engagements:

| **#** | **Condition**                                      | **Description**                                                   |
|-------|----------------------------------------------------|-------------------------------------------------------------------|
| 1     | `eventtype IN ('Open', 'Click')`                  | Includes interactions where the newsletter was opened or clicked. |
| 2     | `country NOT IN ('XX', '<UNDEFINED>')`            | Excludes records with invalid or placeholder country codes.       |
| 3     | `activity_status_type IN ('Open', 'Click')`       | Confirms the activity status reflects an actual engagement.       |





#### 3. Mapping Approach
---
#####   Datasets Overview:
##### 1. Sales Data

##### Sales Data Overview

- **Granularity:**  
  Country-level sales data aggregated at the **Sub-territory** and **Month** levels.

- **Key Fields:**  
  - **Sub-territory** (`GEO_LVL_1_ID`): Unique identifier for the sales region.  
  - **Time Period** (`TM_PD_DT` / `TM_PD`): Represents the reporting period of the sales data.  
  - **Sales Metrics:**  
    - **Dosage Count** (`DOSAGE_CNT`) 
    - **Unit Count** (`UNIT_CNT`)  
    - **Amount in Local Currency** (`AMT_LCL_CURR`)  



##### Promotional Activity Data Overview

- **Granularity:**  
  Promotional activities aggregated at the **Segment**, **Sub-territory**, and **Month** levels.

- **Key Fields:**  
  - **Account Information:**  
    - **Account ID:** Unique identifier for the customer or HCP.  
    - **Account Flags:** Indicators or attributes related to the account.  

  - **Brand:**  
    The associated product or brand (e.g., Kerendia, Nubeqa).  

  - **Activity Type:**  
    Types of promotional engagements, including:  
    - Calls
    - Emails
    - Events 
    - Newsletters

  - **Geography:**  
    Detailed location attributes such as:  
    - Segment
    - Sub-territory
    - District
    - Region 
    - KV 
    - State  


#### Mapping & Join Strategy

##### Common Join Keys

- **Sub-territory** (`GEO_LVL_1_ID` / `Subterritory`): 
  This field exists in both datasets and serves as the primary geographic key for joining the sales and promotional activity data.

- **Time Period** (`TM_PD` / `MONTH`): 
  Both datasets capture data at the **month** level. This field ensures that the records are temporally aligned during the mapping process.

##### Join Process

- **Join Type Selection:**

  - **Inner Join:**  
    Use this when the analysis focuses on records that are present in **both** the Sales and Promotional Activity datasets. This ensures only overlapping data is included.

  - **Left Join:**  
    Use this when prioritizing the **Sales data** as the primary dataset. All Sales records will be retained, and corresponding Promotional Activity data will be added where available.

