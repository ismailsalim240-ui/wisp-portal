<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Wisp | Command Portal</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --gold: #D4AF37;
            --deep-blue: #020617;
            --glass-bg: rgba(255, 255, 255, 0.03);
            --glass-border: rgba(212, 175, 55, 0.15);
            --text-main: #E2E8F0;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background-color: var(--deep-blue);
            color: var(--text-main);
            font-family: 'Montserrat', sans-serif;
            display: flex; flex-direction: column; align-items: center;
            background-image: radial-gradient(circle at 50% -20%, #1e293b 0%, var(--deep-blue) 80%);
            min-height: 100vh;
            overflow-x: hidden;
            scroll-behavior: smooth;
        }

        /* --- Water Motion Keyframes --- */
        @keyframes drift {
            0% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-10px) rotate(0.5deg); }
            100% { transform: translateY(0px) rotate(0deg); }
        }

        @keyframes ripple-glow {
            0% { border-color: var(--glass-border); box-shadow: 0 0 0px rgba(212,175,55,0); }
            50% { border-color: var(--gold); box-shadow: 0 0 20px rgba(212,175,55,0.1); }
            100% { border-color: var(--glass-border); box-shadow: 0 0 0px rgba(212,175,55,0); }
        }

        /* --- Layout --- */
        .container { 
            width: 92%; 
            max-width: 460px; 
            padding-bottom: 120px; 
            opacity: 0; 
            transform: translateY(30px); 
            transition: all 1.5s cubic-bezier(0.22, 1, 0.36, 1); /* Liquid entrance */
        }
        
        .container.visible { opacity: 1; transform: translateY(0); }

        .glass-card { 
            background: var(--glass-bg); 
            border: 1px solid var(--glass-border); 
            border-radius: 24px; 
            padding: 25px; 
            backdrop-filter: blur(20px); 
            margin-bottom: 20px; 
            transition: 0.8s cubic-bezier(0.4, 0, 0.2, 1);
            animation: drift 8s ease-in-out infinite; /* Constant slow water drift */
        }

        .glass-card:hover {
            animation: ripple-glow 3s infinite;
            transform: scale(1.01);
        }

        header { text-align: center; padding: 60px 20px 20px 20px; animation: drift 10s ease-in-out infinite; }
        .logo-aura { font-size: 3.5rem; text-shadow: 0 0 30px rgba(212, 175, 55, 0.3); margin-bottom: 10px; }

        /* --- Performance UI --- */
        .yield-val { font-size: 2rem; font-weight: 700; color: #2ecc71; text-shadow: 0 0 15px rgba(46, 204, 113, 0.3); }
        .timer-display { font-size: 2.2rem; font-weight: 300; margin: 15px 0; color: #fff; letter-spacing: 2px; }
        
        /* --- Buttons --- */
        .action-btn { 
            display: block; width: 100%; padding: 20px; border-radius: 15px; 
            text-align: center; text-decoration: none; font-weight: 700; 
            text-transform: uppercase; letter-spacing: 3px; font-size: 0.7rem; 
            transition: 0.5s; margin-top: 15px; border: none; cursor: pointer;
        }
        .btn-gold { background: var(--gold); color: #000; box-shadow: 0 10px 30px rgba(212,175,55,0.15); }
        .btn-gold:active { transform: scale(0.96); opacity: 0.8; }
        .btn-locked { background: rgba(255,255,255,0.03); color: rgba(255,255,255,0.2); pointer-events: none; border: 1px solid rgba(255,255,255,0.05); }

        /* --- Ticker --- */
        .ticker-wrap { width: 100%; overflow: hidden; background: rgba(0,0,0,0.2); padding: 12px 0; font-size: 0.6rem; color: var(--gold); letter-spacing: 2px; border-bottom: 1px solid var(--glass-border); }
        .ticker { display: inline-block; white-space: nowrap; animation: marquee 40s linear infinite; opacity: 0.6; }
        @keyframes marquee { 0% { transform: translateX(100%); } 100% { transform: translateX(-100%); } }

        /* --- Assistant --- */
        .chat-trigger { 
            position: fixed; bottom: 30px; right: 30px; width: 65px; height: 65px; 
            background: var(--gold); border-radius: 50%; display: flex; 
            align-items: center; justify-content: center; font-size: 1.5rem; 
            box-shadow: 0 15px 35px rgba(0,0,0,0.4); z-index: 1000;
            transition: 0.5s;
        }
        .chat-trigger:active { transform: scale(0.8); }

        .chat-window { 
            display: none; position: fixed; bottom: 105px; right: 30px; 
            width: 320px; background: #050a18; border: 1px solid var(--glass-border); 
            border-radius: 25px; flex-direction: column; overflow: hidden; 
            box-shadow: 0 25px 60px rgba(0,0,0,0.8); z-index: 1000;
        }

        /* --- Progress --- */
        .progress-track { height: 4px; background: rgba(255,255,255,0.05); border-radius: 20px; overflow: hidden; margin: 15px 0; }
        .progress-fill { height: 100%; background: linear-gradient(90deg, var(--gold), #fff); width: 0%; transition: width 1.2s cubic-bezier(0.16, 1, 0.3, 1); }

    </style>
</head>
<body>

    <div class="ticker-wrap">
        <div class="ticker">
            &bull; SILENT MANAGEMENT ACTIVE &nbsp;&nbsp; &bull; MARKET PULSE: STABLE &nbsp;&nbsp; &bull; VENTURE ARCHITECTURE: ETHICAL &nbsp;&nbsp; &bull; ASSETS DEPLOYED &nbsp;&nbsp;
        </div>
    </div>

    <header>
        <div class="logo-aura">🌙</div>
        <h1 style="letter-spacing: 8px; font-size: 1.1rem; opacity: 0.9;">WISP COMMAND</h1>
    </header>

    <div class="container" id="main-ui">
        
        <div class="glass-card">
            <div style="display: flex; justify-content: space-between; align-items: center;">
                <div>
                    <div id="yield-display" class="yield-val">+12.8%</div>
                    <div style="font-size: 0.55rem; text-transform: uppercase; letter-spacing: 2px; opacity: 0.5;">Weekly Growth</div>
                </div>
                <div style="text-align: right; font-size: 0.65rem; line-height: 1.8;">
                    <div style="color: var(--gold);">● AI INFRASTRUCTURE</div>
                    <div style="opacity: 0.6;">● WEB ARCHITECTURE</div>
                </div>
            </div>
        </div>

        <div class="glass-card" style="text-align: center;">
            <div id="status" style="font-size: 0.55rem; letter-spacing: 3px; opacity: 0.6; margin-bottom: 10px;">PORTAL SECURED</div>
            <div id="timer" class="timer-display">0D 0H 0M 0S</div>
            <div class="action-btn btn-locked" id="action-link">Portal Locked</div>
        </div>

        <div class="glass-card">
            <div style="font-size: 0.6rem; letter-spacing: 2px; margin-bottom: 15px; opacity: 0.7;">DAILY DISCIPLINE FLOW</div>
            <div class="progress-track"><div id="p-bar" class="progress-fill"></div></div>
            <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px;">
                <button onclick="runProgress()" style="background: rgba(255,255,255,0.02); border: 1px solid var(--glass-border); color: white; padding: 12px; border-radius: 12px; font-size: 0.55rem; cursor: pointer;">PRAYER</button>
                <button onclick="runProgress()" style="background: rgba(255,255,255,0.02); border: 1px solid var(--glass-border); color: white; padding: 12px; border-radius: 12px; font-size: 0.55rem; cursor: pointer;">SKILL</button>
                <button onclick="runProgress()" style="background: rgba(255,255,255,0.02); border: 1px solid var(--glass-border); color: white; padding: 12px; border-radius: 12px; font-size: 0.55rem; cursor: pointer;">GIVING</button>
            </div>
        </div>

    </div>

    <div class="chat-trigger" onclick="toggleChat()">✨</div>
    <div id="chatWindow" class="chat-window">
        <div style="background: var(--gold); color: #000; padding: 15px; font-weight: 700; font-size: 0.75rem; display: flex; justify-content: space-between;">
            <span>INTELLIGENCE</span><span onclick="toggleChat()" style="cursor:pointer">×</span>
        </div>
        <div id="chatBody" style="padding: 20px; max-height: 300px; overflow-y: auto; font-size: 0.75rem;">
            <div style="border-left: 2px solid var(--gold); padding: 10px; background: rgba(255,255,255,0.05); border-radius: 0 10px 10px 0;">Salam. Management standing by for your instructions.</div>
        </div>
    </div>

    <footer>&copy; 2026 SCINTILLATING WISP</footer>

    <script>
        // Liquid Reveal on Load
        window.onload = () => {
            setTimeout(() => {
                document.getElementById('main-ui').classList.add('visible');
            }, 300);
        };

        // Progress Math
        let score = parseInt(localStorage.getItem('wisp-score') || 0);
        function runProgress() {
            score = (score >= 100) ? 0 : score + 11.2;
            document.getElementById('p-bar').style.width = score + '%';
            localStorage.setItem('wisp-score', score);
        }
        document.getElementById('p-bar').style.width = score + '%';

        function toggleChat() {
            const win = document.getElementById('chatWindow');
            win.style.display = (win.style.display === 'flex') ? 'none' : 'flex';
        }

        // Timer Logic
        function refresh() {
            const now = new Date();
            const day = now.getDay(), hrs = now.getHours();
            const btn = document.getElementById('action-link'), pill = document.getElementById('status'), timer = document.getElementById('timer');

            if (day === 5 && hrs >= 3 && hrs < 10) {
                pill.innerText = "WINDOW ACTIVE"; pill.style.color = "#2ecc71";
                btn.className = "action-btn btn-gold"; btn.innerText = "INITIALIZE";
                timer.innerText = "LIVE";
            } else {
                let next = new Date();
                let dDays = (5 - now.getDay() + 7) % 7;
                if(now.getDay() === 5 && now.getHours() >= 10) dDays = 7;
                next.setDate(now.getDate() + dDays); next.setHours(3,0,0,0);
                let ms = next - now;
                let d = Math.floor(ms / 86400000), h = Math.floor((ms / 3600000) % 24), m = Math.floor((ms / 60000) % 60), s = Math.floor((ms / 1000) % 60);
                timer.innerText = `${d}D ${h}H ${m}M ${s}S`;
            }
        }
        setInterval(refresh, 1000); refresh();
    </script>
</body>
</html>
