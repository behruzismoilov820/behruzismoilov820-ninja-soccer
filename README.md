HTML


<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulyator Ombori</title>
    <style>
        * {
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            width: 100vw;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #0f172a, #1e293b, #3b82f6);
            background-size: cover;
            background-attachment: fixed;
            color: #fff;
            overflow: hidden;
        }

        /* Kalkulyator Qismi */
        .calc-container {
            background: rgba(30, 41, 59, 0.85);
            backdrop-filter: blur(15px);
            padding: 35px;
            border-radius: 24px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6);
            width: 400px;
            max-width: 90vw;
            border: 1px solid rgba(255, 255, 255, 0.15);
        }
        #display {
            width: 100%;
            height: 70px;
            background: #0f172a;
            color: #38bdf8;
            font-size: 34px;
            font-weight: bold;
            text-align: right;
            padding: 10px 20px;
            box-sizing: border-box;
            border: 2px solid rgba(56, 189, 248, 0.3);
            margin-bottom: 25px;
            border-radius: 16px;
        }
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
        }
        button {
            padding: 20px;
            font-size: 22px;
            font-weight: bold;
            cursor: pointer;
            border: none;
            border-radius: 16px;
            background: #334155;
            color: #fff;
            transition: all 0.2s ease;
        }
        button:hover {
            background: #475569;
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.4);
        }
        button.operator { background: #f59e0b; color: white; }
        button.operator:hover { background: #d97706; }
        button.clear { background: #ef4444; color: white; }
        button.clear:hover { background: #dc2626; }

        /* Yashirin Ombor Oynasi */
        .vault-wrapper {
            display: none;
            width: 95vw;
            height: 90vh;
            gap: 20px;
        }

        /* Chap Menyu */
        .vault-sidebar {
            width: 320px;
            min-width: 280px;
            height: 100%;
            background: rgba(30, 41, 59, 0.9);
            backdrop-filter: blur(15px);
            padding: 25px;
            border-radius: 24px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6);
            border: 1px solid rgba(255, 255, 255, 0.15);
            display: flex;
            flex-direction: column;
            overflow-y: auto;
        }
        .vault-sidebar h2 {
            margin-top: 0;
            color: #4ade80;
            font-size: 24px;
            text-align: center;
        }
        .app-btn {
            display: block;
            width: 100%;
            padding: 14px;
            margin-bottom: 10px;
            background: #2563eb;
            color: white;
            border: none;
            border-radius: 14px;
            font-size: 16px;
            font-weight: bold;
            text-align: center;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
        }
        .app-btn:hover {
            transform: translateY(-2px);
            opacity: 0.95;
        }

        /* O'ng Tomondagi Kontent Maydoni */
        .vault-content {
            flex: 1;
            height: 100%;
            background: rgba(15, 23, 42, 0.9);
            border-radius: 24px;
            border: 1px solid rgba(255, 255, 255, 0.15);
            overflow: hidden;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }
        .placeholder-text {
            color: #94a3b8;
            font-size: 18px;
            text-align: center;
            padding: 20px;
        }

        /* Saytlar uchun Iframe */
        #siteViewer {
            width: 100%;
            height: 100%;
            border: none;
            background: #fff;
            display: none;
        }

        /* Ninza Futboli O'yin CSS qismi */
        #gameArea {
            display: none;
            width: 100%;
            height: 100%;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            background: linear-gradient(to bottom, #1e293b, #0f172a);
        }
        #ninjaMenu {
            text-align: center;
        }
        .game-btn {
            display: block;
            width: 220px;
            padding: 12px;
            margin: 10px auto;
            background: #2563eb;
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
        }
        .game-btn:hover { background: #1d4ed8; }
        canvas {
            border: 2px solid rgba(56, 189, 248, 0.3);
            border-radius: 12px;
            background: #0f172a;
            display: none;
        }

        .controls-info {
            margin-top: 15px;
            color: #94a3b8;
            font-size: 13px;
            text-align: center;
            line-height: 1.5;
        }

        .lock-btn {
            background: #ef4444;
            color: white;
            padding: 14px;
            border-radius: 14px;
            border: none;
            cursor: pointer;
            font-weight: bold;
            font-size: 16px;
            width: 100%;
            margin-top: auto;
        }

        .counter-footer {
            margin-top: 20px;
            text-align: center;
        }
    </style>
</head>
<body>

    <!-- Kalkulyator Qismi -->
    <div class="calc-container" id="calculator">
        <input type="text" id="display" readonly placeholder="0">
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button onclick="press('/')">/</button>
            <button onclick="press('*')">*</button>
            <button class="operator" onclick="press('-')">-</button>
            
            <button onclick="press('7')">7</button>
            <button onclick="press('8')">8</button>
            <button onclick="press('9')">9</button>
            <button class="operator" onclick="press('+')">+</button>
            
            <button onclick="press('4')">4</button>
            <button onclick="press('5')">5</button>
            <button onclick="press('6')">6</button>
            <button class="operator" onclick="calculate()">=</button>
            
            <button onclick="press('1')">1</button>
            <button onclick="press('2')">2</button>
            <button onclick="press('3')">3</button>
            <button onclick="press('0')">0</button>
        </div>
        <div class="counter-footer">
            <img src="https://komarev.com/ghpvc/?username=behruzismoilov820-calcualtor&label=Tashriflar&color=blue&style=flat" alt="Tashriflar soni">
        </div>
    </div>

    <!-- Yashirin Ombor Oynasi -->
    <div class="vault-wrapper" id="vault">
        <!-- Chap Menyu -->
        <div class="vault-sidebar">
            <h2>🔒 Ombor</h2>
            
            <button class="app-btn" style="background: #10b981;" onclick="openNinjaGame()">🥷 Ninza Soccer</button>
            <button class="app-btn" style="background: #334155;" onclick="openSite('https://play-cs.com/')">🎮 CS 1.6 Web</button>
            <button class="app-btn" style="background: #fc3f1d;" onclick="openSite('https://yandex.com/games')">🕹️ Yandex Games</button>
            <button class="app-btn" style="background: #2563eb;" onclick="openSite('https://www.bing.com')">🌐 Qidiruv (Google)</button>
            <button class="app-btn" style="background: #cc181e;" onclick="openSite('https://www.youtube-nocookie.com/embed?listType=playlist')">▶️ YouTube</button>
            <button class="app-btn" style="background: #24292e;" onclick="openSite('https://github.com/behruzismoilov820')">🐙 GitHub</button>
            
            <button onclick="lockVault()" class="lock-btn">Qulflash</button>
        </div>

        <!-- O'ng tomonda o'yin va saytlar ochiladigan joy -->
        <div class="vault-content">
            <div id="placeholder" class="placeholder-text">
                <h3>👈 Chap tomondan xohlagan ilovani tanlang</h3>
                <p>Barchasi to'g'ridan-to'g'ri shu yerda ochiladi!</p>
            </div>

            <!-- Saytlar uchun Iframe -->
            <iframe id="siteViewer" src=""></iframe>

            <!-- Ninza Futboli O'yin Maydoni -->
            <div id="gameArea">
                <div id="ninjaMenu">
                    <h2 style="color: #38bdf8; margin-bottom: 10px;">🥷 NINJA SOCCER ⚽</h2>
                    <button class="game-btn" onclick="startNinja(1)">1 Kishi (Bot)</button>
                    <button class="game-btn" style="background: #10b981;" onclick="startNinja(2)">2 Kishilik</button>
                    <div class="controls-info">
                        <p><b>P1 (Qizil):</b> Yurish: <b>W, A, D</b> | Pass: <b>S</b> | Shot: <b>E</b></p>
                        <p><b>P2 (Ko'k):</b> Yurish: <b>I, J, L</b> | Pass: <b>K</b> | Shot: <b>O</b></p>
                    </div>
                </div>
                <canvas id="gameCanvas" width="600" height="350"></canvas>
            </div>
        </div>
    </div>

    <script>
        const USER_PASSWORD = "1995"; 

        let display = document.getElementById('display');
        let calculator = document.getElementById('calculator');
        let vault = document.getElementById('vault');
        let placeholder = document.getElementById('placeholder');
        let siteViewer = document.getElementById('siteViewer');
        let gameArea = document.getElementById('gameArea');
        let ninjaMenu = document.getElementById('ninjaMenu');
        let canvas = document.getElementById('gameCanvas');
        let ctx = canvas.getContext("2d");

        let gameMode = 1;
        let score1 = 0, score2 = 0;
        let p1 = { x: 100, y: 240, w: 35, h: 60, vx: 0, vy: 0, color: "#ef4444" };
        let p2 = { x: 465, y: 240, w: 35, h: 60, vx: 0, vy: 0, color: "#3b82f6" };
        let ball = { x: 300, y: 150, r: 12, vx: 0, vy: 0, color: "#facc15" };
        let keys = {};
        let gameRunning = false;

        function press(val) { display.value += val; }
        function clearDisplay() { display.value = ""; }

        function calculate() {
            if (display.value === USER_PASSWORD) {
                calculator.style.display = 'none';
                vault.style.display = 'flex';
                clearDisplay();
                return;
            }
            try { display.value = eval(display.value); } catch (e) { display.value = "Xato"; }
        }

        function openSite(url) {
            gameRunning = false;
            gameArea.style.display = 'none';
            placeholder.style.display = 'none';
            siteViewer.style.display = 'block';
            siteViewer.src = url;
        }

        function openNinjaGame() {
            siteViewer.style.display = 'none';
            siteViewer.src = "";
            placeholder.style.display = 'none';
            gameArea.style.display = 'flex';
            ninjaMenu.style.display = 'block';
            canvas.style.display = 'none';
            gameRunning = false;
        }

        window.addEventListener("keydown", e => { keys[e.key] = true; keys[e.key.toLowerCase()] = true; keys[e.key.toUpperCase()] = true; });
        window.addEventListener("keyup", e => { keys[e.key] = false; keys[e.key.toLowerCase()] = false; keys[e.key.toUpperCase()] = false; });

        function startNinja(mode) {
            gameMode = mode;
            ninjaMenu.style.display = 'none';
            canvas.style.display = 'block';
            score1 = 0; score2 = 0;
            resetPositions();
            gameRunning = true;
            loop();
        }

        function resetPositions() {
            p1.x = 100; p1.y = 240; p1.vx = 0; p1.vy = 0;
            p2.x = 465; p2.y = 240; p2.vx = 0; p2.vy = 0;
            ball.x = 300; ball.y = 100; ball.vx = 0; ball.vy = 0;
        }

        function update() {
            if (!gameRunning) return;

            // P1 Boshqaruvi
            if (keys["a"]) p1.vx = -4;
            else if (keys["d"]) p1.vx = 4;
            else p1.vx = 0;
            if (keys["w"] && p1.y >= 240) p1.vy = -11;

            // P2 yoki Bot Boshqaruvi
            if (gameMode === 2) {
                if (keys["j"]) p2.vx = -4;
                else if (keys["l"]) p2.vx = 4;
                else p2.vx = 0;
                if (keys["i"] && p2.y >= 240) p2.vy = -11;
            } else {
                if (ball.x > p2.x + 15) p2.vx = 3;
                else if (ball.x < p2.x - 15) p2.vx = -3;
                else p2.vx = 0;
                if (ball.y < p2.y && p2.y >= 240 && Math.random() < 0.03) p2.vy = -10;
            }

            [p1, p2].forEach(p => {
                p.vy += 0.5;
                p.y += p.vy;
                p.x += p.vx;
                if (p.y > 240) { p.y = 240; p.vy = 0; }
                if (p.x < 0) p.x = 0;
                if (p.x > 565) p.x = 565;
            });

            if (p1.x > 260) p1.x = 260;
            if (p2.x < 300) p2.x = 300;

            // PASS va SHOT mexanikasi
            let p1Dist = Math.hypot(ball.x - (p1.x + p1.w/2), ball.y - (p1.y + p1.h/2));
            if (p1Dist < 45) {
                if (keys["s"]) { ball.vx = 7; ball.vy = -4; } // P1 Pass (S)
                if (keys["e"]) { ball.vx = 14; ball.vy = -8; } // P1 Shot (E)
            }

            let p2Dist = Math.hypot(ball.x - (p2.x + p2.w/2), ball.y - (p2.y + p2.h/2));
            if (p2Dist < 45) {
                if (keys["k"]) { ball.vx = -7; ball.vy = -4; } // P2 Pass (K)
                if (keys["o"]) { ball.vx = -14; ball.vy = -8; } // P2 Shot (O)
            }

            ball.vy += 0.35;
            ball.x += ball.vx;
            ball.y += ball.vy;

            if (ball.x - ball.r < 0) { ball.x = ball.r; ball.vx *= -0.8; }
            if (ball.x + ball.r > 600) { ball.x = 600 - ball.r; ball.vx *= -0.8; }
            if (ball.y - ball.r < 0) { ball.y = ball.r; ball.vy *= -0.8; }
            if (ball.y + ball.r > 300) {
                ball.y = 300 - ball.r;
                ball.vy *= -0.7;
                ball.vx *= 0.95;
            }

            // Gollar
            if (ball.x - ball.r < 40 && ball.y > 180) { score2++; resetPositions(); }
            if (ball.x + ball.r > 560 && ball.y > 180) { score1++; resetPositions(); }

            [p1, p2].forEach(p => {
                let cx = Math.max(p.x, Math.min(ball.x, p.x + p.w));
                let cy = Math.max(p.y, Math.min(ball.y, p.y + p.h));
                let dx = ball.x - cx, dy = ball.y - cy;
                if (Math.sqrt(dx*dx + dy*dy) < ball.r) {
                    ball.vx = (ball.x - (p.x + p.w/2)) * 0.2 + p.vx * 0.4;
                    ball.vy = (ball.y - (p.y + p.h/2)) * 0.2 + p.vy * 0.4;
                }
            });
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Darvozalar
            ctx.strokeStyle = "rgba(255,255,255,0.3)";
            ctx.lineWidth = 3;
            ctx.strokeRect(0, 180, 40, 120);
            ctx.strokeRect(560, 180, 40, 120);

            // Yer
            ctx.fillStyle = "#334155";
            ctx.fillRect(0, 300, 600, 50);

            // Hisob
            ctx.fillStyle = "#fff";
            ctx.font = "bold 24px Segoe UI";
            ctx.textAlign = "center";
            ctx.fillText(`${score1} : ${score2}`, 300, 40);

            // P1
            ctx.fillStyle = p1.color;
            ctx.fillRect(p1.x, p1.y, p1.w, p1.h);
            ctx.beginPath(); ctx.arc(p1.x + p1.w/2, p1.y + 12, 12, 0, Math.PI*2); ctx.fill();

            // P2
            ctx.fillStyle = p2.color;
            ctx.fillRect(p2.x, p2.y, p2.w, p2.h);
            ctx.beginPath(); ctx.arc(p2.x + p2.w/2, p2.y + 12, 12, 0, Math.PI*2); ctx.fill();

            // Top
            ctx.fillStyle = ball.color;
            ctx.beginPath(); ctx.arc(ball.x, ball.y, ball.r, 0, Math.PI*2); ctx.fill();
        }

        function loop() {
            if (!gameRunning) return;
            update();
            draw();
            requestAnimationFrame(loop);
        }

        function lockVault() {
            gameRunning = false;
            vault.style.display = 'none';
            calculator.style.display = 'block';
            gameArea.style.display = 'none';
            siteViewer.style.display = 'none';
            siteViewer.src = "";
            placeholder.style.display = 'block';
            clearDisplay();
        }
    </script>
</body>
</html>
