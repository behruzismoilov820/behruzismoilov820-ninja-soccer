HTML


<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🥷 HEAD SOCCER - NARUTO EDITION ⚽</title>
    <style>
        * { box-sizing: border-box; touch-action: manipulation; }
        body {
            margin: 0; padding: 10px;
            background: #0f172a;
            color: #fff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            min-height: 100vh; overflow: hidden;
        }
        h1 { margin: 5px 0; color: #38bdf8; font-size: 26px; text-align: center; }
        #gameContainer {
            background: #1e293b;
            padding: 15px; border-radius: 20px;
            border: 1px solid rgba(255,255,255,0.15);
            display: flex; flex-direction: column; align-items: center;
            width: 100%; max-width: 720px; position: relative;
        }
        #ninjaMenu, #charSelectMenu { text-align: center; padding: 10px; width: 100%; }
        #charSelectMenu { display: none; }
        
        .btn {
            display: block; width: 230px; padding: 14px; margin: 12px auto;
            background: #2563eb; color: white; border: none; border-radius: 12px;
            font-size: 16px; font-weight: bold; cursor: pointer; transition: 0.2s;
        }
        .btn:hover { background: #1d4ed8; transform: scale(1.03); }

        .char-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin: 15px 0;
            max-height: 280px;
            overflow-y: auto;
        }
        .char-btn {
            padding: 12px;
            border: none;
            border-radius: 12px;
            color: white;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            transition: 0.2s;
        }
        .char-btn:hover { transform: scale(1.02); }

        .menu-btn {
            position: absolute; top: 15px; left: 15px;
            background: #ef4444; color: white; border: none;
            padding: 8px 14px; font-size: 14px; font-weight: bold;
            border-radius: 8px; cursor: pointer; display: none; z-index: 10;
        }

        /* G'oliblik oynasi */
        #winModal {
            display: none;
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(15, 23, 42, 0.95);
            border-radius: 20px;
            flex-direction: column; justify-content: center; align-items: center;
            z-index: 20;
        }
        #winModal h2 { font-size: 32px; color: #facc15; margin-bottom: 10px; text-shadow: 0 0 10px rgba(250, 204, 21, 0.5); }
        #winText { font-size: 20px; margin-bottom: 25px; color: #fff; font-weight: bold; text-align: center; }

        canvas {
            border: 3px solid rgba(56, 189, 248, 0.4); border-radius: 16px;
            background: #0f172a; display: none; width: 100%; height: auto;
        }
        .controls-info { margin-top: 10px; color: #94a3b8; font-size: 13px; text-align: center; line-height: 1.4; }

        .touch-controls {
            display: none;
            width: 100%;
            margin-top: 15px;
            justify-content: space-between;
            gap: 8px;
        }
        .touch-group { display: flex; gap: 6px; }
        .t-btn {
            background: #334155; color: white;
            border: 2px solid rgba(56, 189, 248, 0.4);
            padding: 14px 18px; font-size: 16px; font-weight: bold;
            border-radius: 12px; cursor: pointer; user-select: none; outline: none;
        }
        .t-btn:active { background: #2563eb; transform: scale(0.95); }

        @media (max-width: 768px) {
            .touch-controls { display: flex; }
        }
    </style>
</head>
<body>
    <h1>🥷 HEAD SOCCER - NARUTO ⚽</h1>
    <div id="gameContainer">
        <button class="menu-btn" id="menuBtn" onclick="returnToMenu()">🏠 Menu</button>

        <!-- Menyu -->
        <div id="ninjaMenu">
            <h2>O'yin Rejimini Tanlang</h2>
            <button class="btn" onclick="selectMode(1)">1 Kishi (Botga qarshi)</button>
            <button class="btn" style="background: #10b981;" onclick="selectMode(2)">2 Kishilik (Do'st bilan)</button>
            <div class="controls-info">
                <p><b>Vaqt:</b> O'yin 1 daqiqa (60 soniya) davom etadi!</p>
                <p><b>Kompyuter:</b> P1 (W,A,D | Pass: S | Shot: E) | P2 (I,J,L | Pass: K | Shot: O)</p>
                <p><b>Telefon:</b> Ekrandagi sensorli tugmalar ishlatiladi!</p>
            </div>
        </div>

        <!-- Naruto Qahramonlarini Tanlash -->
        <div id="charSelectMenu">
            <h2 id="selectTitle" style="color: #38bdf8; margin-bottom: 5px;">Qahramonni Tanlang</h2>
            <div class="char-grid" id="charGrid"></div>
        </div>

        <canvas id="gameCanvas" width="700" height="400"></canvas>

        <!-- WIN Oynasi -->
        <div id="winModal">
            <h2 id="winTitle">🏆 G'OLIB!</h2>
            <div id="winText">G'olib Bo'ldi!</div>
            <button class="btn" style="background: #10b981;" onclick="restartGame()">🔄 Qayta O'ynash</button>
            <button class="btn" style="background: #ef4444;" onclick="returnToMenu()">🏠 Menyu</button>
        </div>

        <!-- Sensorli pult -->
        <div class="touch-controls" id="touchPanel">
            <div class="touch-group">
                <button class="t-btn" ontouchstart="tState.left=true" ontouchend="tState.left=false" onmousedown="tState.left=true" onmouseup="tState.left=false">◀️</button>
                <button class="t-btn" ontouchstart="tState.right=true" ontouchend="tState.right=false" onmousedown="tState.right=true" onmouseup="tState.right=false">▶️</button>
            </div>
            <div class="touch-group">
                <button class="t-btn" style="background:#059669;" ontouchstart="tState.pass=true" ontouchend="tState.pass=false" onmousedown="tState.pass=true" onmouseup="tState.pass=false">Pass</button>
                <button class="t-btn" style="background:#dc2626;" ontouchstart="tState.shot=true" ontouchend="tState.shot=false" onmousedown="tState.shot=true" onmouseup="tState.shot=false">Shot</button>
                <button class="t-btn" style="background:#2563eb;" ontouchstart="tState.jump=true" ontouchend="tState.jump=false" onmousedown="tState.jump=true" onmouseup="tState.jump=false">⬆️</button>
            </div>
        </div>
    </div>

    <script>
        const characters = [
            { name: "🦊 Naruto Uzumaki", color: "#f97316" },
            { name: "💜 Sasuke Uchiha", color: "#8b5cf6" },
            { name: "🤍 Kaguya Otsutsuki", color: "#38bdf8" },
            { name: "🖤 Madara Uchiha", color: "#991b1b" },
            { name: "🌀 Obito Uchiha", color: "#0284c7" },
            { name: "💚 Hashirama Senju", color: "#22c55e" },
            { name: "🔴 Nagato (Pain)", color: "#dc2626" },
            { name: "❤️ Itachi Uchiha", color: "#e11d48" },
            { name: "⚡ Minato Namikaze", color: "#eab308" },
            { name: "🔥 Might Guy", color: "#10b981" }
        ];

        let canvas = document.getElementById('gameCanvas');
        let ctx = canvas.getContext("2d");
        let ninjaMenu = document.getElementById('ninjaMenu');
        let charSelectMenu = document.getElementById('charSelectMenu');
        let selectTitle = document.getElementById('selectTitle');
        let charGrid = document.getElementById('charGrid');
        let touchPanel = document.getElementById('touchPanel');
        let menuBtn = document.getElementById('menuBtn');
        let winModal = document.getElementById('winModal');
        let winTitle = document.getElementById('winTitle');
        let winText = document.getElementById('winText');

        let gameMode = 1;
        let selectStep = 1;
        let score1 = 0, score2 = 0;
        let timeLeft = 60;
        let timerInterval = null;

        let p1 = { x: 100, y: 275, r: 22, w: 26, h: 45, vx: 0, vy: 0, color: "#f97316", name: "Naruto Uzumaki" };
        let p2 = { x: 560, y: 275, r: 22, w: 26, h: 45, vx: 0, vy: 0, color: "#8b5cf6", name: "Sasuke Uchiha" };
        let ball = { x: 350, y: 150, r: 14, vx: 0, vy: 0, color: "#facc15" };
        let keys = {};
        let tState = { left: false, right: false, jump: false, pass: false, shot: false };
        let gameRunning = false;

        window.addEventListener("keydown", e => { keys[e.code] = true; keys[e.key.toLowerCase()] = true; });
        window.addEventListener("keyup", e => { keys[e.code] = false; keys[e.key.toLowerCase()] = false; });

        function selectMode(mode) {
            gameMode = mode;
            selectStep = 1;
            ninjaMenu.style.display = 'none';
            charSelectMenu.style.display = 'block';
            buildCharGrid();
        }

        function buildCharGrid() {
            charGrid.innerHTML = "";
            if (gameMode === 1) {
                selectTitle.innerText = "P1: O'zingizga qahramon tanlang";
            } else {
                selectTitle.innerText = selectStep === 1 ? "1-O'yinchi uchun qahramon" : "2-O'yinchi uchun qahramon";
            }

            characters.forEach(char => {
                let btn = document.createElement("button");
                btn.className = "char-btn";
                btn.style.background = char.color;
                btn.innerText = char.name;
                btn.onclick = () => chooseCharacter(char);
                charGrid.appendChild(btn);
            });
        }

        function chooseCharacter(char) {
            if (gameMode === 1) {
                p1 = { ...p1, color: char.color, name: char.name };
                let botChars = characters.filter(c => c.name !== char.name);
                let randomBot = botChars[Math.floor(Math.random() * botChars.length)];
                p2 = { ...p2, color: randomBot.color, name: randomBot.name };
                startNinjaGame();
            } else {
                if (selectStep === 1) {
                    p1 = { ...p1, color: char.color, name: char.name };
                    selectStep = 2;
                    buildCharGrid();
                } else {
                    p2 = { ...p2, color: char.color, name: char.name };
                    startNinjaGame();
                }
            }
        }

        function startNinjaGame() {
            charSelectMenu.style.display = 'none';
            winModal.style.display = 'none';
            canvas.style.display = 'block';
            touchPanel.style.display = 'flex';
            menuBtn.style.display = 'block';
            score1 = 0; score2 = 0;
            timeLeft = 60;
            
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                if (gameRunning) {
                    timeLeft--;
                    if (timeLeft <= 0) {
                        endGame();
                    }
                }
            }, 1000);

            resetPositions();
            gameRunning = true;
            loop();
        }

        function restartGame() {
            startNinjaGame();
        }

        function endGame() {
            gameRunning = false;
            clearInterval(timerInterval);
            winModal.style.display = 'flex';

            if (score1 > score2) {
                winTitle.innerText = "🎉 WINNER! 🎉";
                winText.innerText = `${p1.name} G'olib Bo'ldi!\nHisob: ${score1} : ${score2}`;
                winText.style.color = p1.color;
            } else if (score2 > score1) {
                winTitle.innerText = "🎉 WINNER! 🎉";
                winText.innerText = `${p2.name} G'olib Bo'ldi!\nHisob: ${score2} : ${score1}`;
                winText.style.color = p2.color;
            } else {
                winTitle.innerText = "🤝 DURANG!";
                winText.innerText = `O'yin tenglik bilan tugadi!\nHisob: ${score1} : ${score2}`;
                winText.style.color = "#facc15";
            }
        }

        function returnToMenu() {
            gameRunning = false;
            clearInterval(timerInterval);
            canvas.style.display = 'none';
            touchPanel.style.display = 'none';
            menuBtn.style.display = 'none';
            charSelectMenu.style.display = 'none';
            winModal.style.display = 'none';
            ninjaMenu.style.display = 'block';
        }

        function resetPositions() {
            p1.x = 100; p1.y = 275; p1.vx = 0; p1.vy = 0;
            p2.x = 560; p2.y = 275; p2.vx = 0; p2.vy = 0;
            ball.x = 350; ball.y = 100; ball.vx = 0; ball.vy = 0;
        }

        function update() {
            if (!gameRunning) return;

            if (keys["KeyA"] || keys["a"] || tState.left) p1.vx = -5;
            else if (keys["KeyD"] || keys["d"] || tState.right) p1.vx = 5;
            else p1.vx = 0;
            
            if ((keys["KeyW"] || keys["w"] || tState.jump) && p1.y >= 275) p1.vy = -13;

            if (gameMode === 2) {
                if (keys["KeyJ"] || keys["j"]) p2.vx = -5;
                else if (keys["KeyL"] || keys["l"]) p2.vx = 5;
                else p2.vx = 0;
                if ((keys["KeyI"] || keys["i"]) && p2.y >= 275) p2.vy = -13;
            } else {
                if (ball.x > p2.x + 15) p2.vx = 3.6;
                else if (ball.x < p2.x - 15) p2.vx = -3.6;
                else p2.vx = 0;
                if (ball.y < p2.y && p2.y >= 275 && Math.random() < 0.04) p2.vy = -12;
            }

            [p1, p2].forEach(p => {
                p.vy += 0.6; p.y += p.vy; p.x += p.vx;
                if (p.y > 275) { p.y = 275; p.vy = 0; }
                if (p.x < 15) p.x = 15; if (p.x > 665) p.x = 665;
            });

            if (p1.x > 310) p1.x = 310;
            if (p2.x < 350) p2.x = 350;

            let p1HeadX = p1.x + p1.w/2, p1HeadY = p1.y + 10;
            let p1Dist = Math.hypot(ball.x - p1HeadX, ball.y - p1HeadY);
            if (p1Dist < ball.r + p1.r) {
                if (keys["KeyS"] || keys["s"] || tState.pass) { ball.vx = 8; ball.vy = -5; }
                if (keys["KeyE"] || keys["e"] || tState.shot) { ball.vx = 15; ball.vy = -9; }
                else { ball.vx = (ball.x - p1HeadX) * 0.3 + p1.vx * 0.5; ball.vy = (ball.y - p1HeadY) * 0.3; }
            }

            let p2HeadX = p2.x + p2.w/2, p2HeadY = p2.y + 10;
            let p2Dist = Math.hypot(ball.x - p2HeadX, ball.y - p2HeadY);
            if (p2Dist < ball.r + p2.r) {
                if (keys["KeyK"] || keys["k"]) { ball.vx = -8; ball.vy = -5; }
                if (keys["KeyO"] || keys["o"]) { ball.vx = -15; ball.vy = -9; }
                else { ball.vx = (ball.x - p2HeadX) * 0.3 + p2.vx * 0.5; ball.vy = (ball.y - p2HeadY) * 0.3; }
            }

            ball.vy += 0.45; ball.x += ball.vx; ball.y += ball.vy;

            if (ball.x - ball.r < 0) { ball.x = ball.r; ball.vx *= -0.8; }
            if (ball.x + ball.r > 700) { ball.x = 700 - ball.r; ball.vx *= -0.8; }
            if (ball.y - ball.r < 0) { ball.y = ball.r; ball.vy *= -0.8; }
            if (ball.y + ball.r > 350) {
                ball.y = 350 - ball.r; ball.vy *= -0.7; ball.vx *= 0.95;
            }

            if (ball.x - ball.r < 50 && ball.y > 210) { score2++; resetPositions(); }
            if (ball.x + ball.r > 650 && ball.y > 210) { score1++; resetPositions(); }
        }

        function drawHeadPlayer(p) {
            let cx = p.x + p.w / 2;
            let cy = p.y;

            ctx.fillStyle = p.color;
            ctx.fillRect(p.x, cy + 22, p.w, p.h);

            ctx.beginPath();
            ctx.arc(cx, cy + 10, p.r, 0, Math.PI * 2);
            ctx.fill();

            ctx.fillStyle = "#fff";
            ctx.beginPath();
            ctx.arc(cx + (p.color === p1.color ? 6 : -6), cy + 6, 4, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = "#000";
            ctx.beginPath();
            ctx.arc(cx + (p.color === p1.color ? 7 : -7), cy + 6, 2, 0, Math.PI * 2);
            ctx.fill();
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            ctx.strokeStyle = "rgba(255,255,255,0.3)"; ctx.lineWidth = 4;
            ctx.strokeRect(0, 210, 50, 140); ctx.strokeRect(650, 210, 50, 140);
            ctx.fillStyle = "#334155"; ctx.fillRect(0, 350, 700, 50);

            ctx.fillStyle = "#fff"; ctx.font = "bold 24px Segoe UI"; ctx.textAlign = "center";
            ctx.fillText(`${score1} : ${score2}`, 350, 45);

            ctx.fillStyle = "#facc15"; ctx.font = "bold 18px Segoe UI";
            ctx.fillText(`⏱️ 00:${timeLeft < 10 ? '0' + timeLeft : timeLeft}`, 350, 20);

            ctx.font = "14px Segoe UI";
            ctx.fillStyle = p1.color; ctx.fillText(p1.name, 120, 25);
            ctx.fillStyle = p2.color; ctx.fillText(p2.name, 580, 25);

            drawHeadPlayer(p1);
            drawHeadPlayer(p2);
            
            ctx.fillStyle = ball.color; ctx.beginPath(); ctx.arc(ball.x, ball.y, ball.r, 0, Math.PI*2); ctx.fill();
        }

        function loop() {
            if (!gameRunning) return;
            update(); draw(); requestAnimationFrame(loop);
        }
    </script>
</body>
</html>
