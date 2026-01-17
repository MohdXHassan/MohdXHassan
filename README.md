<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hassan - Creative Developer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0a0a0a;
            color: #fff;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
            overflow-x: hidden;
            cursor: none;
        }

        /* Custom Cursor */
        .cursor {
            width: 20px;
            height: 20px;
            border: 2px solid #00ff88;
            border-radius: 50%;
            position: fixed;
            pointer-events: none;
            z-index: 9999;
            transition: all 0.1s ease;
            transform: translate(-50%, -50%);
            mix-blend-mode: difference;
        }

        .cursor-follower {
            width: 40px;
            height: 40px;
            border: 1px solid rgba(0, 255, 136, 0.3);
            border-radius: 50%;
            position: fixed;
            pointer-events: none;
            z-index: 9998;
            transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            transform: translate(-50%, -50%);
            mix-blend-mode: difference;
        }

        /* Animated Background Grid */
        .grid-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(rgba(0, 255, 136, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 255, 136, 0.03) 1px, transparent 1px);
            background-size: 50px 50px;
            animation: gridMove 20s linear infinite;
            z-index: 0;
        }

        @keyframes gridMove {
            0% { transform: perspective(500px) rotateX(60deg) translateY(0); }
            100% { transform: perspective(500px) rotateX(60deg) translateY(50px); }
        }

        /* Gradient Orbs */
        .orb {
            position: fixed;
            border-radius: 50%;
            filter: blur(80px);
            opacity: 0.4;
            animation: float 25s infinite ease-in-out;
            z-index: 1;
        }

        .orb1 {
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, #00ff88, transparent);
            top: -10%;
            left: -10%;
            animation-delay: 0s;
        }

        .orb2 {
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, #0088ff, transparent);
            bottom: -10%;
            right: -10%;
            animation-delay: 5s;
        }

        .orb3 {
            width: 350px;
            height: 350px;
            background: radial-gradient(circle, #ff0088, transparent);
            top: 50%;
            left: 50%;
            animation-delay: 10s;
        }

        @keyframes float {
            0%, 100% { transform: translate(0, 0) scale(1); }
            33% { transform: translate(100px, -100px) scale(1.1); }
            66% { transform: translate(-100px, 100px) scale(0.9); }
        }

        .container {
            position: relative;
            z-index: 2;
            max-width: 1400px;
            margin: 0 auto;
            padding: 40px 20px;
        }

        /* Hero Section */
        .hero {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            position: relative;
        }

        .hero h1 {
            font-size: clamp(3rem, 10vw, 8rem);
            font-weight: 900;
            line-height: 1;
            margin-bottom: 20px;
            background: linear-gradient(135deg, #00ff88 0%, #0088ff 50%, #ff0088 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: textShine 3s ease-in-out infinite;
            background-size: 200% 200%;
        }

        @keyframes textShine {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        .hero .subtitle {
            font-size: clamp(1.2rem, 3vw, 2rem);
            color: rgba(255, 255, 255, 0.6);
            margin-bottom: 40px;
            animation: fadeInUp 1s ease 0.3s backwards;
        }

        .wave-emoji {
            display: inline-block;
            animation: wave 2.5s ease-in-out infinite;
            transform-origin: 70% 70%;
        }

        @keyframes wave {
            0%, 100% { transform: rotate(0deg); }
            10%, 30% { transform: rotate(20deg); }
            20% { transform: rotate(-10deg); }
            40%, 60% { transform: rotate(15deg); }
            50% { transform: rotate(-15deg); }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .scroll-indicator {
            position: absolute;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            animation: bounce 2s infinite;
        }

        .scroll-indicator::before {
            content: '';
            width: 2px;
            height: 60px;
            background: linear-gradient(transparent, #00ff88);
            display: block;
            margin: 0 auto;
        }

        @keyframes bounce {
            0%, 100% { transform: translateX(-50%) translateY(0); }
            50% { transform: translateX(-50%) translateY(20px); }
        }

        /* Skills Section */
        .skills-section {
            margin: 100px 0;
            animation: fadeInUp 1s ease 0.6s backwards;
        }

        .section-title {
            font-size: clamp(2rem, 5vw, 4rem);
            font-weight: 800;
            text-align: center;
            margin-bottom: 60px;
            background: linear-gradient(135deg, #fff, rgba(255, 255, 255, 0.5));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .skills-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 20px;
            perspective: 1000px;
        }

        .skill-card {
            background: rgba(255, 255, 255, 0.02);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 255, 136, 0.1);
            border-radius: 20px;
            padding: 30px 20px;
            text-align: center;
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }

        .skill-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 255, 136, 0.1), transparent);
            transition: left 0.5s;
        }

        .skill-card:hover::before {
            left: 100%;
        }

        .skill-card:hover {
            transform: translateY(-10px) scale(1.05);
            border-color: #00ff88;
            box-shadow: 0 20px 60px rgba(0, 255, 136, 0.3);
        }

        .skill-card h3 {
            font-size: 1.2rem;
            color: #00ff88;
            margin: 0;
        }

        /* Info Cards */
        .info-section {
            margin: 100px 0;
        }

        .info-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
        }

        .info-card {
            background: rgba(255, 255, 255, 0.02);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 24px;
            padding: 40px;
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }

        .info-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: conic-gradient(from 0deg, transparent, rgba(0, 255, 136, 0.2), transparent 30%);
            animation: rotate 6s linear infinite;
            opacity: 0;
            transition: opacity 0.4s;
        }

        .info-card:hover::before {
            opacity: 1;
        }

        @keyframes rotate {
            100% { transform: rotate(360deg); }
        }

        .info-card-content {
            position: relative;
            z-index: 1;
        }

        .info-card:hover {
            transform: translateY(-10px);
            border-color: rgba(0, 255, 136, 0.5);
            box-shadow: 0 30px 80px rgba(0, 255, 136, 0.2);
        }

        .info-card h3 {
            font-size: 1.8rem;
            margin-bottom: 15px;
            color: #00ff88;
        }

        .info-card p, .info-card li {
            color: rgba(255, 255, 255, 0.7);
            line-height: 1.8;
            font-size: 1.1rem;
        }

        .info-card ul {
            list-style: none;
            padding: 0;
        }

        .info-card li {
            padding-left: 25px;
            position: relative;
            margin: 12px 0;
        }

        .info-card li::before {
            content: '→';
            position: absolute;
            left: 0;
            color: #00ff88;
            font-weight: bold;
        }

        /* Social Links */
        .social-section {
            margin: 100px 0;
            text-align: center;
        }

        .social-grid {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
            margin-top: 40px;
        }

        .social-link {
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(0, 255, 136, 0.2);
            color: #00ff88;
            padding: 15px 35px;
            border-radius: 50px;
            text-decoration: none;
            font-weight: 600;
            font-size: 1.1rem;
            transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }

        .social-link::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(0, 255, 136, 0.2);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        .social-link:hover::before {
            width: 300px;
            height: 300px;
        }

        .social-link:hover {
            transform: translateY(-5px);
            border-color: #00ff88;
            box-shadow: 0 10px 40px rgba(0, 255, 136, 0.4);
        }

        .social-link span {
            position: relative;
            z-index: 1;
        }

        /* Stats Section */
        .stats-section {
            margin: 100px 0;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 30px;
            margin-top: 40px;
        }

        .stat-card {
            text-align: center;
            padding: 40px 20px;
            background: rgba(255, 255, 255, 0.02);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            transition: all 0.4s ease;
        }

        .stat-card:hover {
            transform: scale(1.05);
            border-color: #00ff88;
        }

        .stat-number {
            font-size: 4rem;
            font-weight: 900;
            background: linear-gradient(135deg, #00ff88, #0088ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .stat-label {
            color: rgba(255, 255, 255, 0.6);
            font-size: 1.1rem;
            margin-top: 10px;
        }

        /* Footer */
        .footer {
            text-align: center;
            padding: 60px 20px;
            margin-top: 100px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .footer p {
            color: rgba(255, 255, 255, 0.5);
            font-size: 1.1rem;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .grid-bg { background-size: 30px 30px; }
            .orb { display: none; }
            body { cursor: auto; }
            .cursor, .cursor-follower { display: none; }
        }
    </style>
</head>
<body>
    <div class="cursor"></div>
    <div class="cursor-follower"></div>
    <div class="grid-bg"></div>
    <div class="orb orb1"></div>
    <div class="orb orb2"></div>
    <div class="orb orb3"></div>

    <div class="container">
        <section class="hero">
            <h1>Hi there <span class="wave-emoji">👋</span></h1>
            <p class="subtitle">I'm <strong>Hassan</strong> — Creative Developer</p>
            <p class="subtitle" style="font-size: 1.2rem;">Welcome to my world of code and innovation</p>
            <div class="scroll-indicator"></div>
        </section>

        <section class="skills-section">
            <h2 class="section-title">Tech Arsenal</h2>
            <div class="skills-grid">
                <div class="skill-card"><h3>C / C++</h3></div>
                <div class="skill-card"><h3>Java</h3></div>
                <div class="skill-card"><h3>Python</h3></div>
                <div class="skill-card"><h3>Protopie</h3></div>
                <div class="skill-card"><h3>Docker</h3></div>
                <div class="skill-card"><h3>Selenium</h3></div>
            </div>
        </section>

        <section class="info-section">
            <h2 class="section-title">What I'm Up To</h2>
            <div class="info-grid">
                <div class="info-card">
                    <div class="info-card-content">
                        <h3>🔭 Current Project</h3>
                        <p>Working on Belle 2 - pushing boundaries in particle physics research and computational analysis</p>
                    </div>
                </div>
                <div class="info-card">
                    <div class="info-card-content">
                        <h3>🌱 Learning Journey</h3>
                        <p>Deep diving into Selenium automation, mastering test frameworks and web scraping techniques</p>
                    </div>
                </div>
                <div class="info-card">
                    <div class="info-card-content">
                        <h3>😄 About Me</h3>
                        <ul>
                            <li>Pronouns: He/Him</li>
                            <li>Passionate Problem Solver</li>
                            <li>Code Enthusiast</li>
                            <li>Continuous Learner</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>

        <section class="stats-section">
            <h2 class="section-title">GitHub Journey</h2>
            <div class="stats-grid">
                <div class="stat-card">
                    <div class="stat-number">500+</div>
                    <div class="stat-label">Commits</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">25+</div>
                    <div class="stat-label">Projects</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">∞</div>
                    <div class="stat-label">Ideas</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">100%</div>
                    <div class="stat-label">Dedication</div>
                </div>
            </div>
        </section>

        <section class="social-section">
            <h2 class="section-title">Let's Connect</h2>
            <div class="social-grid">
                <a href="https://www.linkedin.com/in/mohammad-hassan-769083183/" class="social-link">
                    <span>LinkedIn</span>
                </a>
                <a href="mailto:mdh73053@gmail.com" class="social-link">
                    <span>Email</span>
                </a>
                <a href="https://github.com/MohdXHassan" class="social-link">
                    <span>GitHub</span>
                </a>
                <a href="https://leetcode.com/ZARIX1" class="social-link">
                    <span>LeetCode</span>
                </a>
                <a href="https://codeforces.com/profile/AyanSanger" class="social-link">
                    <span>Codeforces</span>
                </a>
            </div>
        </section>

        <footer class="footer">
            <p>Thanks for stopping by! Let's build something amazing together 🚀</p>
        </footer>
    </div>

    <script>
        // Custom Cursor
        const cursor = document.querySelector('.cursor');
        const follower = document.querySelector('.cursor-follower');

        let mouseX = 0, mouseY = 0;
        let followerX = 0, followerY = 0;

        document.addEventListener('mousemove', (e) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
            cursor.style.left = mouseX + 'px';
            cursor.style.top = mouseY + 'px';
        });

        function animateFollower() {
            followerX += (mouseX - followerX) * 0.1;
            followerY += (mouseY - followerY) * 0.1;
            follower.style.left = followerX + 'px';
            follower.style.top = followerY + 'px';
            requestAnimationFrame(animateFollower);
        }
        animateFollower();

        // Hover effects for interactive elements
        const interactiveElements = document.querySelectorAll('a, .skill-card, .info-card');
        interactiveElements.forEach(el => {
            el.addEventListener('mouseenter', () => {
                cursor.style.width = '40px';
                cursor.style.height = '40px';
                cursor.style.borderColor = '#0088ff';
            });
            el.addEventListener('mouseleave', () => {
                cursor.style.width = '20px';
                cursor.style.height = '20px';
                cursor.style.borderColor = '#00ff88';
            });
        });

        // Smooth scroll reveal animations
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -100px 0px'
        };

        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);

        document.querySelectorAll('.skill-card, .info-card, .stat-card').forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'translateY(30px)';
            el.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
            observer.observe(el);
        });
    </script>
</body>
</html>
