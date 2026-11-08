HTML


<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🥷 NINJA SOCCER 2D - NARUTO EDITION ⚽</title>
    <style>
        * { box-sizing: border-box; touch-action: manipulation; }
        body {
            margin: 0; padding: 10px;
            background: radial-gradient(circle at center, #1e1b4b, #0f172a, #020617);
            color: #fff;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            min-height: 100vh; overflow: hidden;
        }
        h1 {
            margin: 5px 0; color: #fbbf24; font-size: 28px;
            text-shadow: 0 0 15px rgba(251, 191, 36, 0.6);
            text-align: center; font-weight: 900; letter-spacing: 1px;
        }
        #gameContainer {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(12px);
            padding: 15px; border-radius: 24px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.8), 0 0 20px rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(255,255,255,0.15);
            display: flex; flex-direction: column; align-items: center;
            width: 100%; max-width: 740px; position: relative;
        }
        #ninjaMenu, #charSelectMenu { text-align: center; padding: 10px; width: 100%; }
        #charSelectMenu { display: none; }
        
        .btn {
            display: block; width: 240px; padding: 14px; margin: 12px auto;
            background: linear-gradient(135deg, #2563eb, #1d4ed8); color: white; border: none; border-radius: 14px;
            font-size: 16px; font-weight: bold; cursor: pointer; transition: 0.2s ease;
            box-shadow: 0 4px 15px rgba(37, 99, 235, 0.4);
        }
        .btn:hover { transform: scale(1.04); box-shadow: 0 6px 20px rgba(37, 99, 235, 0.6); }

        /* Qahramonlar gridi */
        .char-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin: 15px 0;
            max-height: 300px;
            overflow-y: auto;
            padding: 5px;
        }
        .char-btn {
            display: flex; align-items: center; gap: 10px;
            padding: 10px 14px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 16px;
            color: white;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.2s ease;
            box-shadow: 0 4px 12px rgba(0,0,0,0.4);
            text-shadow: 0 1px 3px rgba(0,0,0,0.8);
        }
        .char-btn:hover { transform: translateY(-2px) scale(1.02); border-color: #fff; }
        .char-avatar {
            width: 40px; height: 40px; border-radius: 50%;
            border: 2px solid #fff; object-fit: cover;
            box-shadow: 0 0 8px rgba(0,0,0,0.5);
        }

        /* O'yin ichidagi menyu tugmasi */
        .menu-btn {
            position: absolute; top: 15px; left: 15px;
            background: #ef4444; color: white; border: none;
            padding: 8px 14px; font-size: 14px; font-weight: bold;
            border-radius: 10px; cursor: pointer; display: none; z-index: 10;
            box-shadow: 0 2px 10px rgba(239, 68, 68, 0.4);
        }
        .menu-btn:hover { background: #dc2626; }

        canvas {
            border: 3px solid rgba(251, 191, 36, 0.4); border-radius: 18px;
            background: #0f172a; display: none; width: 100%; height: auto;
            box-shadow: 0 0 30px rgba(0,0,0,0.7);
        }
        .controls-info { margin-top: 10px; color: #94a3b8; font-size: 13px; text-align: center; line-height: 1.5; }

        /* Telefon uchun sensorli tugmalar */
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
            border-radius: 14px; cursor: pointer; user-select: none; outline: none;
        }
        .t-btn:active { background: #2563eb; transform: scale(0.95); }

        @media (max-width: 768px) {
            .touch-controls { display: flex; }
        }
    </style>
</head>
<body>
    <h1>🥷 NINJA SOCCER ⚽</h1>
    <div id="gameContainer">
        <button class="menu-btn" id="menuBtn" onclick="returnToMenu()">🏠 Menu</button>

        <!-- Boshlang'ich Menyu -->
        <div id="ninjaMenu">
            <h2>O'yin Rejimini Tanlang</h2>
            <button class="btn" onclick="selectMode(1)">1 Kishi (Botga qarshi)</button>
            <button class="btn" style="background: linear-gradient(135deg, #10b981, #059669);" onclick="selectMode(2)">2 Kishilik (Do'st bilan)</button>
            <div class="controls-info">
                <p><b>Kompyuter:</b> P1 (W,A,D | Pass: S | Shot: E) | P2 (I,J,L | Pass: K | Shot: O)</p>
                <p><b>Telefon:</b> Ekrandagi sensorli tugmalar yordamida o'ynaladi!</p>
            </div>
        </div>

        <!-- Qahramon tanlash menyusi -->
        <div id="charSelectMenu">
            <h2 id="selectTitle" style="color: #fbbf24; margin-bottom: 5px;">Qahramonni Tanlang</h2>
            <div class="char-grid" id="charGrid"></div>
        </div>

        <canvas id="gameCanvas" width="700" height="400"></canvas>

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
            { name: "Naruto Uzumaki", color: "#f97316", img: "https://api.dicebear.com/7.x/bottts/svg?seed=naruto" },
            { name: "Sasuke Uchiha", color: "#8b5cf6", img: "https://api.dicebear.com/7.x/bottts/svg?seed=sasuke" },
            { name: "Kaguya Otsutsuki", color: "#e2e8f0", img: "https://api.dicebear.com/7.x/bottts/svg?seed=kaguya" },
            { name: "Madara Uchiha", color: "#991b1b", img: "https://api.dicebear.com/7.x/bottts/svg?seed=madara" },
            { name: "Obito Uchiha", color: "#0284c7", img: "https://api.dicebear.com/7.x/bottts/svg?seed=obito" },
            { name: "Hashirama Senju", color: "#22c55e", img: "https://api.dicebear.com/7.x/bottts/svg?seed=hashirama" },
            { name: "Nagato (Pain)", color: "#dc2626", img: "https://api.dicebear.com/7.x/bottts/svg?seed=pain" },
            { name: "Itachi Uchiha", color: "#334155", img: "https://api.dicebear.com/7.x/bottts/svg?seed=itachi" },
            { name: "Minato Namikaze", color: "#eab308", img: "https://api.dicebear.com/7.x/bottts/svg?seed=minato" },
            { name: "Might Guy", color: "#ef4444", img: "https://api.dicebear.com/7.x/bottts/svg?seed=guy" }
        ];

        let loadedImages = {};
        characters.forEach(c => {
            let img = new Image();
            img.src = c.img;
            loadedImages[c.name] = img;
        });

        let canvas = document.getElementById('gameCanvas');
        let ctx = canvas.getContext("2d");
        let ninjaMenu = document.getElementById('ninjaMenu');
        let charSelectMenu = document.getElementById('charSelectMenu');
        let selectTitle = document.getElementById('selectTitle');
        let charGrid = document.getElementById('charGrid');
        let touchPanel = document.getElementById('touchPanel');
        let menuBtn = document.getElementById('menuBtn');

        let gameMode = 1;
        let selectStep = 1;
        let score1 = 0, score2 = 0;
        let p1 = { x: 100, y: 280, w: 42, h: 70, vx: 0, vy: 0, color: "#f97316", name: "Naruto Uzumaki" };
        let p2 = { x: 560, y: 280, w: 42, h: 70, vx: 0, vy: 0, color: "#8b5cf6", name: "Sasuke Uchiha" };
        let ball = { x: 350, y: 150, r: 14, vx: 0, vy: 0, color: "#fbbf24" };
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
                btn.style.background = `linear-gradient(135deg, ${char.color}, #0f172a)`;
                btn.innerHTML = `<img src="${char.img}" class="char-avatar"> <span>${char.name}</span>`;
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
            canvas.style.display = 'block';
            touchPanel.style.display = 'flex';
            menuBtn.style.display = 'block';
            score1 = 0; score2 = 0;
            resetPositions();
            gameRunning = true;
            loop();
        }

        function returnToMenu() {
            gameRunning = false;
            canvas.style.display = 'none';
            touchPanel.style.display = 'none';
            menuBtn.style.display = 'none';
            charSelectMenu.style.display = 'none';
            ninjaMenu.style.display = 'block';
        }

        function resetPositions() {
            p1.x = 100; p1.y = 280; p1.vx = 0; p1.vy = 0;
            p2.x = 560; p2.y = 280; p2.vx = 0; p2.vy = 0;
            ball.x = 350; ball.y = 100; ball.vx = 0; ball.vy = 0;
        }

        function update() {
            if (!gameRunning) return;

            if (keys["KeyA"] || keys["a"] || tState.left) p1.vx = -4.5;
            else if (keys["KeyD"] || keys["d"] || tState.right) p1.vx = 4.5;
            else p1.vx = 0;
            
            if ((keys["KeyW"] || keys["w"] || tState.jump) && p1.y >= 280) p1.vy = -12;

            if (gameMode === 2) {
                if (keys["KeyJ"] || keys["j"]) p2.vx = -4.5;
                else if (keys["KeyL"] || keys["l"]) p2.vx = 4.5;
                else p2.vx = 0;
                if ((keys["KeyI"] || keys["i"]) && p2.y >= 280) p2.vy = -12;
            } else {
                if (ball.x > p2.x + 15) p2.vx = 3.2;
                else if (ball.x < p2.x - 15) p2.vx = -3.2;
                else p2.vx = 0;
                if (ball.y < p2.y && p2.y >= 280 && Math.random() < 0.03) p2.vy = -11;
            }

            [p1, p2].forEach(p => {
                p.vy += 0.55; p.y += p.vy; p.x += p.vx;
                if (p.y > 280) { p.y = 280; p.vy = 0; }
                if (p.x < 0) p.x = 0; if (p.x > 658) p.x = 658;
            });

            if (p1.x > 310) p1.x = 310;
            if (p2.x < 350) p2.x = 350;

            let p1Dist = Math.hypot(ball.x - (p1.x + p1.w/2), ball.y - (p1.y + p1.h/2));
            if (p1Dist < 45) {
                if (keys["KeyS"] || keys["s"] || tState.pass) { ball.vx = 7; ball.vy = -4; }
                if (keys["KeyE"] || keys["e"] || tState.shot) { ball.vx = 14; ball.vy = -8; }
            }

            let p2Dist = Math.hypot(ball.x - (p2.x + p2.w/2), ball.y - (p2.y + p2.h/2));
            if (p2Dist < 45) {
                if (keys["KeyK"] || keys["k"]) { ball.vx = -7; ball.vy = -4; }
                if (keys["KeyO"] || keys["o"]) { ball.vx = -14; ball.vy = -8; }
            }

            ball.vy += 0.4; ball.x += ball.vx; ball.y += ball.vy;

            if (ball.x - ball.r < 0) { ball.x = ball.r; ball.vx *= -0.8; }
            if (ball.x + ball.r > 700) { ball.x = 700 - ball.r; ball.vx *= -0.8; }
            if (ball.y - ball.r < 0) { ball.y = ball.r; ball.vy *= -0.8; }
            if (ball.y + ball.r > 350) {
                ball.y = 350 - ball.r; ball.vy *= -0.7; ball.vx *= 0.95;
            }

            if (ball.x - ball.r < 50 && ball.y > 210) { score2++; resetPositions(); }
            if (ball.x + ball.r > 650 && ball.y > 210) { score1++; resetPositions(); }

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

        function drawField() {
            // Tungi stadion va osmon foni
            let grad = ctx.createLinearGradient(0, 0, 0, 400);
            grad.addColorStop(0, "#090d16");
            grad.addColorStop(0.7, "#1e293b");
            grad.addColorStop(1, "#0f172a");
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, 700, 400);

            // Chim (Maydon gazi)
            let grass = ctx.createLinearGradient(0, 350, 0, 400);
            grass.addColorStop(0, "#15803d");
            grass.addColorStop(1, "#166534");
            ctx.fillStyle = grass;
            ctx.fillRect(0, 350, 700, 50);

            // Darvozalar va chiziqlar
            ctx.strokeStyle = "rgba(255,255,255,0.4)"; ctx.lineWidth = 3;
            ctx.strokeRect(0, 210, 50, 140);
            ctx.strokeRect(650, 210, 50, 140);
            ctx.beginPath(); ctx.moveTo(350, 0); ctx.lineTo(350, 350); ctx.stroke();
            ctx.beginPath(); ctx.arc(350, 175, 50, 0, Math.PI * 2); ctx.stroke();
        }

        function drawPlayer(p) {
            // Aura (Porlash)
            ctx.shadowBlur = 15;
            ctx.shadowColor = p.color;

            // Tanasi
            ctx.fillStyle = p.color;
            ctx.fillRect(p.x, p.y + 15, p.w, p.h - 15);

            // Bosh / Avatar koti
            let img = loadedImages[p.name];
            if (img && img.complete) {
                ctx.save();
                ctx.beginPath();
                ctx.arc(p.x + p.w / 2, p.y + 12, 16, 0, Math.PI * 2);
                ctx.clip();
                ctx.drawImage(img, p.x + p.w / 2 - 16, p.y - 4, 32, 32);
                ctx.restore();
            } else {
                ctx.beginPath(); ctx.arc(p.x + p.w / 2, p.y + 12, 14, 0, Math.PI * 2); ctx.fill();
            }

            ctx.shadowBlur = 0; // Shadow qaytarish
        }

        function draw() {
            drawField();

            // Hisob va ismlar
            ctx.fillStyle = "#fbbf24"; ctx.font = "bold 24px Segoe UI"; ctx.textAlign = "center";
            ctx.fillText(`${score1} : ${score2}`, 350, 40);

            ctx.font = "bold 13px Segoe UI";
            ctx.fillStyle = p1.color; ctx.fillText(p1.name, 120, 25);
            ctx.fillStyle = p2.color; ctx.fillText(p2.name, 580, 25);

            // O'yinchilar
            drawPlayer(p1);
            drawPlayer(p2);

            // To'p
            ctx.shadowBlur = 12; ctx.shadowColor = "#fbbf24";
            ctx.fillStyle = ball.color;
            ctx.beginPath(); ctx.arc(ball.x, ball.y, ball.r, 0, Math.PI * 2); ctx.fill();
            ctx.shadowBlur = 0;
        }

        function loop() {
            if (!gameRunning) return;
            update(); draw(); requestAnimationFrame(loop);
        }
    </script>
</body>
</html>
