<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ninja Hard Soccer 1v1 - Ultimate Edition</title>
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

        /* 1. Kirish Ekrani (Splash Screen - 5 sekund) */
        #splashScreen {
            position: absolute;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, #241442 0%, #090514 100%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 100;
            transition: opacity 0.5s ease;
        }
        .splash-banner {
            width: 85%;
            max-width: 650px;
            background: linear-gradient(135deg, #120c1f, #251b40);
            border: 4px solid #00f0ff;
            box-shadow: 0 0 30px #00f0ff;
            border-radius: 20px;
            padding: 30px;
            text-align: center;
        }
        .splash-title {
            font-size: 38px;
            color: #fff;
            text-shadow: 0 0 15px #ff0055, 3px 3px 0 #000;
            margin: 0 0 20px 0;
            letter-spacing: 2px;
        }
        .splash-vs-art {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 20px 0;
        }
        .ninja-face {
            width: 100px;
            height: 100px;
            border: 4px solid #fff;
            border-radius: 15px;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            text-shadow: 2px 2px 0 #000;
        }
        .n-orange { background: #ff9900; box-shadow: 0 0 15px #ff9900; color: #000; }
        .s-blue { background: #2e3b8f; box-shadow: 0 0 15px #2e3b8f; color: #fff; }
        .chakra-ball-art {
            width: 70px;
            height: 70px;
            background: #fff;
            border: 4px solid #000;
            border-radius: 50%;
            box-shadow: 0 0 25px #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #000;
            font-size: 12px;
            animation: spin 3s linear infinite;
        }
        @keyframes spin { 100% { transform: rotate(360deg); } }

        /* 2. Bosh Menyu va 10 ta Qahramon Tanlash */
        #menuContainer {
            display: none;
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

        /* 3. O'yin oynasi va Maydon */
        #gameWrapper {
            display: none;
            position: relative;
            flex-direction: column;
            align-items: center;
        }
        #scoreBoard {
            font-size: 24px;
            margin-bottom: 10px;
            background: rgba(0,0,0,0.5);
            padding: 8px 25px;
            border-radius: 20px;
            border: 2px solid #333;
        }
        canvas {
            background: linear-gradient(to bottom, #161233, #1e3d23);
            border: 5px solid #fff;
            border-radius: 12px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.7);
        }

        /* 4. Sensorli tugmalar */
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

        /* 5. Defeat / Victory Oynasi */
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

    <!-- 1. Kirish Splash ekrani (5 soniya ko'rsatiladi) -->
    <div id="splashScreen">
        <div class="splash-banner">
            <h1 class="splash-title">NINJA HARD SOCCER</h1>
            <div class="splash-vs-art">
                <div class="ninja-face n-orange">NARUTO</div>
                <div class="chakra-ball-art">⚽<br>CHAKRA</div>
                <div class="ninja-face s-blue">SASUKE</div>
            </div>
            <div style="color: #888; font-size: 14px; margin-top: 15px;">Yuklanmoqda...</div>
        </div>
    </div>

    <!-- 2. Bosh o'yin menyusi (10 ta qahramonlik grid) -->
    <div id="menuContainer">
        <h1 class="menu-title">QAHRAMONNI TANLANG</h1>
        
        <div class="mode-selection">
            <button class="mode-btn active" id="mode1P" onclick="setMode('1P')">1P (Botga qarshi)</button>
            <button class="mode-btn" id="mode2P" onclick="setMode('2P')">2P (Do'st bilan)</button>
        </div>

        <div class="char-grid" id="charactersContainer">
            <!-- Dinamik 10 ta qahramon JavaScript orqali yuklanadi -->
        </div>

        <button class="start-game-btn" onclick="initGame()">O'YINNI BOSHLASH</button>
    </div>

    <!-- 3. O'yin oynasi -->
    <div id="gameWrapper">
        <div id="scoreBoard">
            <span id="p1Label" style="color:#ff9900">P1</span>: <span id="p1Score">0</span> 
            | 
            <span id="p2Label" style="color:#00f0ff">P2</span>: <span id="p2Score">0</span>
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

    <!-- 4. Natija ekrani -->
    <div id="overlayScreen">
        <div id="overlayText">DEFEAT<br>Yutqazdingiz!</div>
        <button class="start-game-btn" onclick="backToMenu()">KAYTA TANLASH</button>
    </div>

    <script>
        // 5 soniyadan keyin splash ekranni yopish va menyuni ochish
        setTimeout(() => {
            const splash = document.getElementById("splashScreen");
            splash.style.opacity = 0;
            setTimeout(() => {
                splash.style.display = "none";
                document.getElementById("menuContainer").style.display = "flex";
            }, 500);
        }, 5000);

        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        let currentMode = '1P';
        let selectedP1Index = 0;
        let selectedP2Index = 1;

        // 10 ta eng mashhur va zo'r qahramonlar ma'lumotlar bazasi
        const heroDatabase = [
            { name: "Naruto", body: "#ff9900", head: "#fcc", hair: "#ffee00", eye: "#00aaff" },
