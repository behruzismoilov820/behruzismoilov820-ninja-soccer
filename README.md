HTML


<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🥷 NINJA SOCCER - HEAD EDITION ⚽</title>
    <style>
        * { box-sizing: border-box; touch-action: manipulation; }
        body {
            margin: 0; padding: 10px;
            background: #020617;
            color: #fff;
            font-family: 'Trebuchet MS', 'Segoe UI', sans-serif;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            min-height: 100vh; overflow: hidden;
        }
        
        .logo-container {
            display: flex; align-items: center; justify-content: center; gap: 12px;
            margin-bottom: 10px;
        }
        .game-logo-img {
            width: 48px; height: 48px; border-radius: 50%;
            border: 2px solid #fbbf24; background: #fff; object-fit: cover;
            box-shadow: 0 0 12px rgba(251, 191, 36, 0.6);
        }
        .game-logo {
            font-size: 30px; font-weight: 900; text-transform: uppercase;
            font-style: italic; letter-spacing: 2px;
            background: linear-gradient(180deg, #ffe58f 0%, #faad14 50%, #d48806 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 4px 15px rgba(250, 173, 20, 0.4);
            margin: 0; text-align: center;
        }

        #gameContainer {
            background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%);
            padding: 15px; border-radius: 24px;
            border: 2px solid rgba(251, 191, 36, 0.4);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.9), 0 0 30px rgba(234, 179, 8, 0.2);
            display: flex; flex-direction: column; align-items: center;
            width: 100%; max-width: 740px; position: relative;
        }

        /* 2 soniya turadigan Splash Screen */
        #splashScreen {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            width: 100%; height: 400px; text-align: center;
        }
        #splashScreen img {
            max-width: 100%; max-height: 350px; border-radius: 16px;
            border: 2px solid rgba(251, 191, 36, 0.5);
            box-shadow: 0 0 20px rgba(0,0,0,0.8);
        }

        #ninjaMenu, #charSelectMenu { text-align: center; padding: 10px; width: 100%; display: none; }
        
        .btn {
            display: block; width: 250px; padding: 14px; margin: 12px auto;
            background: linear-gradient(180deg, #3b82f6 0%, #1d4ed8 100%); color: white; border: 2px solid #60a5fa; border-radius: 14px;
            font-size: 17px; font-weight: 900; cursor: pointer; transition: 0.15s ease;
            text-shadow: 0 2px 4px rgba(0,0,0,0.5); box-shadow: 0 6px 0 #1e40af, 0 8px 15px rgba(0,0,0,0.4);
        }
        .btn:active { transform: translateY(4px); box-shadow: 0 2px 0 #1e40af; }
        .btn-green {
            background: linear-gradient(180deg, #22c55e 0%, #15803d 100%);
            border-color: #4ade80; box-shadow: 0 6px 0 #166534;
        }

        .char-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px; margin: 15px 0; max-height: 270px;
            overflow-y: auto; padding: 5px;
        }
        .char-btn {
            display: flex; align-items: center; gap: 12px;
            padding: 10px 14px; border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 16px; color: white; font-weight: 800;
            font-size: 14px; cursor: pointer; transition: 0.15s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.4); text-shadow: 0 1px 3px rgba(0,0,0,0.8);
        }
        .char-btn:hover { transform: scale(1.03); border-color: #fff; }
        .char-icon {
            width: 42px; height: 42px; border-radius: 50%;
            border: 2px solid #fff; background: #fff; object-fit: cover;
            box-shadow: 0 0 8px rgba(0,0,0,0.5);
        }

        .menu-btn {
            position: absolute; top: 15px; left: 15px;
            background: #ef4444; color: white; border: 2px solid #fca5a5;
            padding: 8px 14px; font-size: 14px; font-weight: bold;
            border-radius: 10px; cursor: pointer; display: none; z-index: 10;
        }

        #winModal {
            display: none; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(2, 6, 23, 0.95); border-radius: 20px;
            flex-direction: column; justify-content: center; align-items: center; z-index: 20;
        }
        #winModal h2 { font-size: 36px; color: #facc15; margin-bottom: 5px; text-shadow: 0 0 15px rgba(250, 204, 21, 0.6); }
        #winText { font-size: 22px; margin-bottom: 25px; color: #fff; font-weight: 800; text-align: center; line-height: 1.4; }

        canvas {
            border: 3px solid rgba(251, 191, 36, 0.5); border-radius: 18px;
            background: #0f172a; display: none; width: 100%; height: auto;
            box-shadow: 0 0 25px rgba(0,0,0,0.8);
        }
        .controls-info { margin-top: 10px; color: #94a3b8; font-size: 13px; text-align: center; }

        .touch-controls {
            display: none; width: 100%; margin-top: 12px;
            justify-content: space-between; gap: 8px;
        }
        .touch-group { display: flex; gap: 6px; }
        .t-btn {
            background: #334155; color: white; border: 2px solid rgba(56, 189, 248, 0.5);
            padding: 14px 18px; font-size: 16px; font-weight: bold;
            border-radius: 14px; cursor: pointer; user-select: none; outline: none;
            box-shadow: 0 4px 0 #1e293b;
        }
        .t-btn:active { background: #2563eb; transform: translateY(2px); box-shadow: 0 1px 0 #1e293b; }

        @media (max-width: 768px) {
            .touch-controls { display: flex; }
        }
    </style>
</head>
<body>
    <div class="logo-container">
        <img src="https://api.dicebear.com/7.x/bottts/svg?seed=naruto" class="game-logo-img" alt="Logo">
        <div class="game-logo">NINJA SOCCER ⚽</div>
    </div>
    
    <div id="gameContainer">
        <button class="menu-btn" id="menuBtn" onclick="returnToMenu()">🏠 Menu</button>

        <!-- 2 Sekund Turadigan Kirish Rasmi (Splash Screen) -->
        <div id="splashScreen">
            <img src="watermarked_img_1437238758356918555.jpg" alt="Ninja Soccer Intro">[cite: 2]
            <p style="color: #facc15; margin-top: 10px; font-weight: bold; font-size: 15px;">Yuklanmoqda...</p>
        </div>

        <!-- Mode Select Menu -->
        <div id="ninjaMenu">
            <h2>O'YIN REJIMINI TANLANG</h2>
            <button class="btn" onclick="selectMode(1)">1 KISHI (VS BOT)</button>
            <button class="btn btn-green" onclick="selectMode(2)">2 KISHILIK (2 PLAYER)</button>
            <div class="controls-info">
                <p><b>Vaqt:</b> 1 Daqiqa (60 Soniyalik match)</p>
                <p><b>Boshqaruv (PC):</b> P1 (W,A,D | Pass: S | Shot: E) | P2 (I,J,L | Pass: K | Shot: O)</p>
            </div>
        </div>

        <!-- Character Select Menu -->
        <div id="charSelectMenu">
            <h2 id="selectTitle" style="color: #facc15; margin-bottom: 5px;">QAHRAMONNI TANLANG</h2>
            <div class="char-grid" id="charGrid"></div>
        </div>

        <canvas id="gameCanvas" width="700" height="400"></canvas>

        <!-- Win Modal -->
        <div id="winModal">
            <h2 id="winTitle">🏆 G'OLIB!</h2>
            <div id="winText">G'olib Bo'ldi!</div>
            <button class="btn btn-green" onclick="restartGame()">🔄 QAYTA O'YNASH</button>
            <button class="btn" style="background: #ef4444; border-color: #fca5a5;" onclick="returnToMenu()">🏠 MENYU</button>
        </div>

        <!-- Sensorli Panel -->
        <div class="touch-controls" id="touchPanel">
            <div class="touch-group">
                <button class="t-btn" ontouchstart="tState.left=true" ontouchend="tState.left=false" onmousedown="tState.left=true" onmouseup="tState.left=false">◀️</button>
                <button class="t-btn" ontouchstart="tState.right=true" ontouchend="tState.right=false" onmousedown="tState.right=true" onmouseup="tState.right=false">▶️</button>
            </div>
            <div class="touch-group">
                <button class="t-btn" style="background:#059669; border-color:#34d399;" ontouchstart="tState.pass=true" ontouchend="tState.pass=false" onmousedown="tState.pass=true" onmouseup="tState.pass=false">Pass</button>
                <button class="t-btn" style="background:#dc2626; border-color:#f87171;" ontouchstart="tState.shot=true" ontouchend="tState.shot=false" onmousedown="tState.shot=true" onmouseup="tState.shot=false">Shot</button>
                <button class="t-btn" style="background:#2563eb; border-color:#60a5fa;" ontouchstart="tState.jump=true" ontouchend="tState.jump=false" onmousedown="tState.jump=true" onmouseup="tState.jump=false">⬆️</button>
            </div>
        </div>
    </div>

    <script>
        setTimeout(() => {
            document.getElementById('splashScreen').style.display = 'none';
            document.getElementById('ninjaMenu').style.display = 'block';
        }, 2000);

        // Naruto olamidagi kuchli 10 ta qahramon (Maxsus tashqi ko'rinish va ranglar bilan)
        const characters = [
            { name: "Naruto Uzumaki", shirtColor: "#f97316", hairColor: "#eab308", headBand: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=naruto" },
            { name: "Sasuke Uchiha", shirtColor: "#3b82f6", hairColor: "#1e1b4b", headBand: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=sasuke" },
            { name: "Kakashi Hatake", shirtColor: "#1e293b", hair: "#94a3b8", mask: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=kakashi" },
            { name: "Madara Uchiha", shirtColor: "#991b1b", hair: "#0f172a", spikyHair: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=madara" },
            { name: "Itachi Uchiha", shirtColor: "#0f172a", hair: "#18181b", redCloud: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=itachi" },
            { name: "Hashirama Senju", shirtColor: "#15803d", hair: "#14532d", traditional: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=hashirama" },
            { name: "Minato Namikaze", shirtColor: "#facc15", hair: "#fef08a", cape: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=minato" },
            { name: "Pain (Nagato)", shirtColor: "#b91c1c", hair: "#ff7675", rinnegan: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=pain" },
            { name: "Obito Uchiha", shirtColor: "#475569", hair: "#000000", orangeMask: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=obito" },
            { name: "Might Guy", color: "#10b981", shirtColor: "#16a34a", greenSuit: true, img: "https://api.dicebear.com/7.x/bottts/svg?seed=guy" }
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
        let winModal = document.getElementById('winModal');
        let winTitle = document.getElementById('winTitle');
        let winText = document.getElementById('winText');

        let gameMode = 1;
        let selectStep = 1;
        let score1 = 0, score2 = 0;
        let timeLeft = 60;
        let timerInterval = null;

        let p1 = { x: 100, y: 260, r: 30, w: 24, h: 32, vx: 0, vy: 0, shirtColor: "#f97316", hairColor: "#eab308", name: "Naruto Uzumaki", anim: 0 };
        let p2 = { x: 560, y: 260, r: 30, w: 24, h: 32, vx: 0, vy: 0, shirtColor: "#3b82f6", hairColor: "#1e1b4b", name: "Sasuke Uchiha", anim: 0 };
        let ball = { x: 350, y: 150, r: 14, vx: 0, vy: 0, color: "#facc15" };
        let keys = {};
        let tState = { left: false, right: false, jump: false, pass: false, shot: false };
        let gameRunning = false;

        window.addEventListener("keydown", e => { keys[e.code] = true; keys[e.key.toLowerCase()] = true; });
        window.addEventListener("keyup", e => { keys[e.code] = false; keys[e.key.toLowerCase()] = false; });

        function selectMode(mode) {
            gameMode = mode; selectStep = 1;
            ninjaMenu.style.display = 'none';
            charSelectMenu.style.display = 'block';
            buildCharGrid();
        }

        function buildCharGrid() {
            charGrid.innerHTML = "";
            selectTitle.innerText = gameMode === 1 ? "P1: QAHRAMONNI TANLANG" : (selectStep === 1 ? "1-O'YINCHI QAHRAMONI" : "2-O'YINCHI QAHRAMONI");
            characters.forEach(char => {
                let btn = document.createElement("button");
                btn.className = "char-btn";
                btn.style.background = `linear-gradient(135deg, ${char.shirtColor}, #0f172a)`;
                btn.innerHTML = `<img src="${char.img}" class="char-icon"> <span>${char.name}</span>`;
                btn.onclick = () => chooseCharacter(char);
                charGrid.appendChild(btn);
            });
        }

        function chooseCharacter(char) {
            if (gameMode === 1) {
                p1 = { ...p1, shirtColor: char.shirtColor, hairColor: char.hairColor || "#333", name: char.name };
                let botChars = characters.filter(c => c.name !== char.name);
                let randomBot = botChars[Math.floor(Math.random() * botChars.length)];
                p2 = { ...p2, shirtColor: randomBot.shirtColor, hairColor: randomBot.hairColor || "#333", name: randomBot.name };
                startNinjaGame();
            } else {
                if (selectStep === 1) {
                    p1 = { ...p1, shirtColor: char.shirtColor, hairColor: char.hairColor || "#333", name: char.name };
                    selectStep = 2; buildCharGrid();
                } else {
                    p2 = { ...p2, shirtColor: char.shirtColor, hairColor: char.hairColor || "#333", name: char.name };
                    startNinjaGame();
                }
            }
        }

        function startNinjaGame() {
            charSelectMenu.style.display = 'none'; winModal.style.display = 'none';
            canvas.style.display = 'block'; touchPanel.style.display = 'flex'; menuBtn.style.display = 'block';
            score1 = 0; score2 = 0; timeLeft = 60;
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                if (gameRunning) {
                    timeLeft--;
                    if (timeLeft <= 0) endGame();
                }
            }, 1000);
            resetPositions(); gameRunning = true; loop();
        }

        function restartGame() { startNinjaGame(); }

        function endGame() {
            gameRunning = false; clearInterval(timerInterval); winModal.style.display = 'flex';

            if (score1 > score2) {
                winTitle.innerText = "🏆 WINNER!";
                winTitle.style.color = "#facc15";
                winText.innerText = `${p1.name} G'olib Bo'ldi!\nHisob: ${score1} : ${score2}`;
                winText.style.color = p1.shirtColor;
            } else if (score2 > score1) {
                if (gameMode === 1) {
                    winTitle.innerText = "❌ YUTQAZDINGIZ!";
                    winTitle.style.color = "#ef4444";
                    winText.innerText = `Siz mag'lub bo'ldingiz!\nBot (${p2.name}) yutdi: ${score2} : ${score1}`;
                    winText.style.color = "#ef4444";
                } else {
                    winTitle.innerText = "🏆 WINNER!";
                    winTitle.style.color = "#facc15";
                    winText.innerText = `${p2.name} G'olib Bo'ldi!\nHisob: ${score2} : ${score1}`;
                    winText.style.color = p2.shirtColor;
                }
            } else {
                winTitle.innerText = "🤝 DURANG!";
                winTitle.style.color = "#facc15";
                winText.innerText = `O'yin tenglik bilan tugadi!\nHisob: ${score1} : ${score2}`;
                winText.style.color = "#facc15";
            }
        }

        function returnToMenu() {
            gameRunning = false; clearInterval(timerInterval);
            canvas.style.display = 'none'; touchPanel.style.display = 'none'; menuBtn.style.display = 'none';
            charSelectMenu.style.display = 'none'; winModal.style.display = 'none';
            ninjaMenu.style.display = 'block';
        }

        function resetPositions() {
            p1.x = 100; p1.y = 260; p1.vx = 0; p1.vy = 0;
            p2.x = 560; p2.y = 260; p2.vx = 0; p2.vy = 0;
            ball.x = 350; ball.y = 100; ball.vx = 0; ball.vy = 0;
        }

        function update() {
            if (!gameRunning) return;

            if (keys["KeyA"] || keys["a"] || tState.left) p1.vx = -5;
            else if (keys["KeyD"] || keys["d"] || tState.right) p1.vx = 5;
            else p1.vx = 0;
            if ((keys["KeyW"] || keys["w"] || tState.jump) && p1.y >= 260) p1.vy = -13;

            if (gameMode === 2) {
                if (keys["KeyJ"] || keys["j"]) p2.vx = -5;
                else if (keys["KeyL"] || keys["l"]) p2.vx = 5;
                else p2.vx = 0;
                if ((keys["KeyI"] || keys["i"]) && p2.y >= 260) p2.vy = -13;
            } else {
                if (ball.x > p2.x + 15) p2.vx = 3.6;
                else if (ball.x < p2.x - 15) p2.vx = -3.6;
                else p2.vx = 0;
                if (ball.y < p2.y && p2.y >= 260 && Math.random() < 0.04) p2.vy = -12;
            }

            [p1, p2].forEach(p => {
                p.vy += 0.6; p.y += p.vy; p.x += p.vx;
                if (p.vx !== 0) p.anim += 0.25; else p.anim = 0;
                if (p.y > 260) { p.y = 260; p.vy = 0; }
                if (p.x < 15) p.x = 15; if (p.x > 665) p.x = 665;
            });

            if (p1.x > 310) p1.x = 310;
            if (p2.x < 350) p2.x = 350;

            let p1HeadX = p1.x + p1.w/2, p1HeadY = p1.y + 12;
            let p1Dist = Math.hypot(ball.x - p1HeadX, ball.y - p1HeadY);
            if (p1Dist < ball.r + p1.r) {
                if (keys["KeyS"] || keys["s"] || tState.pass) { ball.vx = 8.5; ball.vy = -5; }
                if (keys["KeyE"] || keys["e"] || tState.shot) { ball.vx = 16; ball.vy = -9.5; }
                else { ball.vx = (ball.x - p1HeadX) * 0.35 + p1.vx * 0.5; ball.vy = (ball.y - p1HeadY) * 0.35; }
            }

            let p2HeadX = p2.x + p2.w/2, p2HeadY = p2.y + 12;
            let p2Dist = Math.hypot(ball.x - p2HeadX, ball.y - p2HeadY);
            if (p2Dist < ball.r + p2.r) {
                if (keys["KeyK"] || keys["k"]) { ball.vx = -8.5; ball.vy = -5; }
                if (keys["KeyO"] || keys["o"]) { ball.vx = -16; ball.vy = -9.5; }
                else { ball.vx = (ball.x - p2HeadX) * 0.35 + p2.vx * 0.5; ball.vy = (ball.y - p2HeadY) * 0.35; }
            }

            ball.vy += 0.45; ball.x += ball.vx; ball.y += ball.vy;

            if (ball.x - ball.r < 0) { ball.x = ball.r; ball.vx *= -0.8; }
            if (ball.x + ball.r > 700) { ball.x = 700 - ball.r; ball.vx *= -0.8; }
            if (ball.y - ball.r < 0) { ball.y = ball.r; ball.vy *= -0.8; }
            if (ball.y + ball.r > 350) { ball.y = 350 - ball.r; ball.vy *= -0.7; ball.vx *= 0.95; }

            if (ball.x - ball.r < 50 && ball.y > 210) { score2++; resetPositions(); }
            if (ball.x + ball.r > 650 && ball.y > 210) { score1++; resetPositions(); }
        }

        function drawStadium() {
            let bgGrad = ctx.createLinearGradient(0, 0, 0, 400);
            bgGrad.addColorStop(0, "#020617");
            bgGrad.addColorStop(0.6, "#0f172a");
            bgGrad.addColorStop(1, "#1e293b");
            ctx.fillStyle = bgGrad; ctx.fillRect(0, 0, 700, 400);

            ctx.fillStyle = "rgba(255, 255, 255, 0.05)";
            ctx.beginPath(); ctx.moveTo(0, 0); ctx.lineTo(250, 350); ctx.lineTo(100, 350); ctx.fill();
            ctx.beginPath(); ctx.moveTo(700, 0); ctx.lineTo(450, 350); ctx.lineTo(600, 350); ctx.fill();

            let grass = ctx.createLinearGradient(0, 350, 0, 400);
            grass.addColorStop(0, "#16a34a"); grass.addColorStop(1, "#15803d");
            ctx.fillStyle = grass; ctx.fillRect(0, 350, 700, 50);

            ctx.strokeStyle = "rgba(255,255,255,0.6)"; ctx.lineWidth = 4;
            ctx.strokeRect(0, 210, 50, 140); ctx.strokeRect(650, 210, 50, 140);
            ctx.beginPath(); ctx.moveTo(350, 0); ctx.lineTo(350, 350); ctx.stroke();
            ctx.beginPath(); ctx.arc(350, 175, 45, 0, Math.PI * 2); ctx.stroke();
        }

        // Head Soccer uslubidagi katta bosh, to'rtburchak tana, mitti qo'l-oyoqlar va Naruto qahramonlari dizayni
        function drawHeadPlayer(p, isP1) {
            let cx = p.x + p.w / 2;
            let cy = p.y;
            let swing = Math.sin(p.anim) * 10;

            // Oyoqlar (Sneakers)
            ctx.fillStyle = "#ffffff";
            ctx.beginPath(); ctx.arc(cx - 7 + swing, cy + 62, 5, 0, Math.PI * 2); ctx.fill();
            ctx.beginPath(); ctx.arc(cx + 7 - swing, cy + 62, 5, 0, Math.PI * 2); ctx.fill();

            // To'rtburchak Tana (Shirt)
            ctx.fillStyle = p.shirtColor;
            ctx.beginPath(); ctx.roundRect(cx - 12, cy + 32, 24, 26, 6); ctx.fill();

            // Qo'llar
            ctx.strokeStyle = p.shirtColor; ctx.lineWidth = 5; ctx.lineCap = "round";
            ctx.beginPath();
            ctx.moveTo(cx - 12, cy + 38); ctx.lineTo(cx - 20 + swing/2, cy + 50);
            ctx.moveTo(cx + 12, cy + 38); ctx.lineTo(cx + 20 - swing/2, cy + 50);
            ctx.stroke();

            // Katta Bosh (Giant Head)
            ctx.fillStyle = "#ffedd5";
            ctx.beginPath(); ctx.arc(cx, cy + 10, p.r, 0, Math.PI * 2); ctx.fill();

            // Qahramon Sochi yoki Elementlari
            ctx.fillStyle = p.hairColor;
            ctx.beginPath(); ctx.arc(cx, cy + 2, p.r + 2, Math.PI * 0.8, Math.PI * 2.2); ctx.fill();

            // Nindza Peshonabog'i (Headband)
            ctx.fillStyle = "#334155";
            ctx.fillRect(cx - p.r + 3, cy + 2, p.r * 2 - 6, 7);
            ctx.fillStyle = "#cbd5e1";
            ctx.fillRect(cx - 6, cy + 3, 12, 5);

            // Katta Cartoon Ko'zlar
            let eyeX = isP1 ? cx + 8 : cx - 8;
            ctx.fillStyle = "#ffffff";
            ctx.beginPath(); ctx.arc(eyeX, cy + 14, 5.5, 0, Math.PI * 2); ctx.fill();
            ctx.fillStyle = "#000000";
            ctx.beginPath(); ctx.arc(eyeX + (isP1 ? 1 : -1), cy + 14, 2.5, 0, Math.PI * 2); ctx.fill();

            // Tabassum
            ctx.strokeStyle = "#000"; ctx.lineWidth = 2;
            ctx.beginPath(); ctx.arc(eyeX - (isP1 ? 2 : -2), cy + 22, 3.5, 0, Math.PI); ctx.stroke();
        }

        function draw() {
            drawStadium();

            ctx.fillStyle = "#facc15"; ctx.font = "bold 24px Trebuchet MS"; ctx.textAlign = "center";
            ctx.fillText(`${score1} : ${score2}`, 350, 45);

            ctx.fillStyle = "#38bdf8"; ctx.font = "bold 16px Trebuchet MS";
            ctx.fillText(`⏱️ 00:${timeLeft < 10 ? '0' + timeLeft : timeLeft}`, 350, 20);

            ctx.font = "bold 13px Trebuchet MS";
            ctx.fillStyle = p1.shirtColor; ctx.fillText(p1.name, 120, 25);
            ctx.fillStyle = p2.shirtColor; ctx.fillText(p2.name, 580, 25);

            drawHeadPlayer(p1, true);
            drawHeadPlayer(p2, false);
            
            ctx.shadowBlur = 10; ctx.shadowColor = "#facc15";
            ctx.fillStyle = ball.color; ctx.beginPath(); ctx.arc(ball.x, ball.y, ball.r, 0, Math.PI*2); ctx.fill();
            ctx.shadowBlur = 0;
        }

        function loop() {
            if (!gameRunning) return;
            update(); draw(); requestAnimationFrame(loop);
        }
    </script>
</body>
</html>
