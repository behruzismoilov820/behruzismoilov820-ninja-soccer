<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ninja Hard Soccer 1v1 - Ultimate One File</title>
    <style>
        body {
            margin: 0;
            background: #0f0c20;
            font-family: 'Arial Black', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            color: white;
            overflow: hidden;
            user-select: none;
        }

        /* 1. Bosh Menyu va 10 ta Qahramon Tanlash */
        #menuContainer {
            display: flex;
            position: absolute;
            width: 100%;
            height: 100%;
            background: #130f26;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 10;
            overflow-y: auto;
            padding: 20px 0;
        }
        .menu-title {
            font-size: 28px;
            text-shadow: 0 0 10px #ff0055;
            margin-bottom: 15px;
        }
        .mode-selection {
            margin-bottom: 20px;
        }
        .mode-btn {
            background: #211c42;
            color: white;
            border: 2px solid #00f0ff;
            padding: 10px 25px;
            font-size: 16px;
            margin: 0 10px;
            border-radius: 8px;
            cursor: pointer;
        }
        .mode-btn.active {
            background: #00f0ff;
            color: #000;
            box-shadow: 0 0 15px #00f0ff;
        }
        .char-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 12px;
            margin-bottom: 25px;
            max-width: 650px;
            width: 90%;
        }
        .char-card {
            background: #1c163a;
            border: 3px solid #3d356b;
            border-radius: 12px;
            padding: 10px 5px;
            text-align: center;
            cursor: pointer;
            transition: 0.2s;
        }
        .char-card.selected-p1 { border-color: #ff9900; box-shadow: 0 0 15px #ff9900; }
        .char-card.selected-p2 { border-color: #00f0ff; box-shadow: 0 0 15px #00f0ff; }
        .char-name { font-size: 12px; margin-top: 6px; }

        .start-game-btn {
            background: #ff0055;
            color: white;
            border: 3px solid #fff;
            padding: 12px 45px;
            font-size: 22px;
            border-radius: 12px;
            cursor: pointer;
            box-shadow: 0 5px 0 #990033;
        }
        .start-game-btn:active { transform: translateY(4px); box-shadow: 0 1px 0 #990033; }

        /* 2. O'yin oynasi, Taymer va Maydon */
        #gameWrapper {
            display: none;
            position: relative;
            flex-direction: column;
            align-items: center;
        }
        #topPanel {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 10px;
        }
        #scoreBoard {
            font-size: 24px;
            background: rgba(0,0,0,0.5);
            padding: 8px 25px;
            border-radius: 20px;
            border: 2px solid #333;
        }
        #timerDisplay {
            font-size: 24px;
            background: #ff0055;
            padding: 8px 20px;
            border-radius: 20px;
            border: 2px solid #fff;
            box-shadow: 0 0 10px #ff0055;
            font-weight: bold;
            min-width: 60px;
            text-align: center;
        }
        canvas {
            background: linear-gradient(to bottom, #161233, #1e3d23);
            border: 5px solid #fff;
            border-radius: 12px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.7);
        }

        /* 3. Sensorli tugmalar */
        #touchControls {
            display: none;
            width: 100%;
            max-width: 800px;
            justify-content: space-between;
            margin-top: 15px;
            padding: 0 20px;
            box-sizing: border-box;
        }
        .nav-group, .action-group { display: flex; gap: 20px; }
        .touch-btn {
            width: 65px;
            height: 65px;
            background: rgba(255, 255, 255, 0.15);
            border: 3px solid #fff;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 26px;
            color: white;
        }
        .touch-btn:active { background: rgba(255, 255, 255, 0.5); }

        /* 4. Defeat / Victory Oynasi */
        #overlayScreen {
            display: none;
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(5, 3, 13, 0.9);
            z-index: 50;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        #overlayText {
            font-size: 50px;
            text-align: center;
            margin-bottom: 25px;
            letter-spacing: 3px;
        }
    </style>
</head>
<body>

    <!-- 1. Bosh o'yin menyusi (10 ta qahramonlik grid) -->
    <div id="menuContainer">
        <h1 class="menu-title">QAHRAMONNI TANLANG</h1>
        
        <div class="mode-selection">
            <button class="mode-btn active" id="mode1P" onclick="setMode('1P')">1P (Botga qarshi)</button>
            <button class="mode-btn" id="mode2P" onclick="setMode('2P')">2P (Do'st bilan)</button>
        </div>

        <div class="char-grid" id="charactersContainer">
            <!-- 10 ta qahramon shu yerda chiqadi -->
        </div>

        <button class="start-game-btn" onclick="initGame()">O'YINNI BOSHLASH</button>
    </div>

    <!-- 2. O'yin oynasi -->
    <div id="gameWrapper">
        <div id="topPanel">
            <div id="scoreBoard">
                <span id="p1Label" style="color:#ff9900">P1</span>: <span id="p1Score">0</span> 
                | 
                <span id="p2Label" style="color:#00f0ff">P2</span>: <span id="p2Score">0</span>
            </div>
            <div id="timerDisplay">60</div>
        </div>
        <canvas id="gameCanvas" width="800" height="400"></canvas>
        
        <!-- Sensorli ekran elementlari -->
        <div id="touchControls">
            <div class="nav-group">
                <div class="touch-btn" id="tLeft">←</div>
                <div class="touch-btn" id="tRight">→</div>
            </div>
            <div class="action-group">
                <div class="touch-btn" id="tJump">↑</div>
            </div>
        </div>
    </div>

    <!-- 3. Natija ekrani -->
    <div id="overlayScreen">
        <div id="overlayText">DEFEAT<br>Yutqazdingiz!</div>
        <button class="start-game-btn" onclick="backToMenu()">KAYTA TANLASH</button>
    </div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        let currentMode = '1P';
        let selectedP1Index = 0;
        let selectedP2Index = 1;
        let gameTimer = null;
        let timeLeft = 60;
        let isGameActive = false;

        const heroDatabase = [
            { name: "Naruto", body: "#ff9900", head: "#fcc", hair: "#ffee00", eye: "#00aaff" },
            { name: "Sasuke", body: "#2e3b8f", head: "#fdd", hair: "#1a1a2e", eye: "#ff0033" },
            { name: "Kakashi", body: "#34495e", head: "#e0e0e0", hair: "#d5dbdb", eye: "#333" },
            { name: "Itachi", body: "#4a0e17", head: "#fcc", hair: "#2c3e50", eye: "#ff0000" },
            { name: "Gaara", body: "#962d2d", head: "#fbb", hair: "#800000", eye: "#40e0d0" },
            { name: "Madara", body: "#1a1a1a", head: "#fcc", hair: "#111111", eye: "#ff0011" },
            { name: "Jiraiya", body: "#7d1212", head: "#fcc", hair: "#ffffff", eye: "#222" },
            { name: "Orochimaru", body: "#5b32a8", head: "#eef2e6", hair: "#050505", eye: "#e1ff00" },
            { name: "Obito", body: "#2c3e50", head: "#d5dbdb", hair: "#222222", eye: "#ff0055" },
            { name: "Minato", body: "#2980b9", head: "#fcc", hair: "#f1c40f", eye: "#3498db" }
        ];

        const grid = document.getElementById("charactersContainer");
        function renderMenu() {
            grid.innerHTML = "";
            heroDatabase.forEach((hero, idx) => {
                const card = document.createElement("div");
                let activeClass = "";
                if (idx === selectedP1Index) activeClass = "selected-p1";
                else if (currentMode === '2P' && idx === selectedP2Index) activeClass = "selected-p2";
                
                card.className = `char-card ${activeClass}`;
                card.id = `char-${idx}`;
                card.setAttribute("onclick", `selectCharacter(${idx})`);
                card.innerHTML = `
                    <div style="width:40px; height:40px; background:${hero.body}; margin:0 auto; border-radius:6px; border:2px solid ${hero.hair}"></div>
                    <div class="char-name">${hero.name}</div>
                `;
                grid.appendChild(card);
            });
        }

        let p1Score = 0, p2Score = 0;
        const gravity = 0.5;
        const pH = 75, pW = 55;

        const p1 = { x: 100, y: 325, vx: 0, vy: 0, isGrounded: false };
        const p2 = { x: 650, y: 325, vx: 0, vy: 0, isGrounded: false };
        const ball = { x: 400, y: 120, radius: 15, vx: 0, vy: 0, bounce: 0.75 };

        const goalW = 15, goalH = 140;
        const leftGoal = { x: 0, y: 400 - goalH, w: goalW, h: goalH };
        const rightGoal = { x: 800 - goalW, y: 400 - goalH, w: goalW, h: goalH };

        const keys = {};
        window.addEventListener("keydown", e => keys[e.key] = true);
        window.addEventListener("keyup", e => keys[e.key] = false);

        function bindTouch(id, key) {
rightGoal.w, rightGoal.h);
drawBoxCharacter(p1, heroDatabase[selectedP1Index]);
drawBoxCharacter(p2, heroDatabase[selectedP2Index]);
ctx.fillStyle = "#fff";
ctx.beginPath(); ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2); ctx.fill();
ctx.strokeStyle = "#111"; ctx.lineWidth = 3; ctx.stroke();
}
function animate() {
if (isGameActive) {
update();
draw();
requestAnimationFrame(animate);
}
}
renderMenu();
