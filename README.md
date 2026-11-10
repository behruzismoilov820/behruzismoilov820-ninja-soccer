HTML


<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🥷 NINJA SOCCER 3D ULTRA ⚽</title>
    <style>
        * { box-sizing: border-box; touch-action: manipulation; }
        body {
            margin: 0; padding: 5px;
            background: #020617; color: #fff;
            font-family: 'Trebuchet MS', 'Segoe UI', sans-serif;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            min-height: 100vh; overflow: hidden;
        }

        #gameContainer {
            background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%);
            padding: 10px; border-radius: 20px;
            border: 2px solid rgba(251, 191, 36, 0.4);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.9), 0 0 30px rgba(234, 179, 8, 0.2);
            display: flex; flex-direction: column; align-items: center;
            width: 100%; max-width: 1060px; position: relative;
        }

        #splashScreen {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            width: 100%; height: 420px; text-align: center;
        }
        #splashScreen img {
            max-width: 100%; max-height: 360px; border-radius: 18px;
            border: 2px solid rgba(251, 191, 36, 0.6);
            box-shadow: 0 0 25px rgba(0,0,0,0.9);
        }

        #ninjaMenu, #charSelectMenu, #tournamentModal { text-align: center; padding: 10px; width: 100%; display: none; }
        
        .btn {
            display: block; width: 260px; padding: 11px; margin: 8px auto;
            background: linear-gradient(180deg, #3b82f6 0%, #1d4ed8 100%); color: white; border: 2px solid #60a5fa; border-radius: 12px;
            font-size: 15px; font-weight: 900; cursor: pointer; transition: 0.15s ease;
            text-shadow: 0 2px 4px rgba(0,0,0,0.5); box-shadow: 0 5px 0 #1e40af, 0 6px 12px rgba(0,0,0,0.4);
        }
        .btn:active { transform: translateY(4px); box-shadow: 0 1px 0 #1e40af; }
        .btn-green {
            background: linear-gradient(180deg, #22c55e 0%, #15803d 100%);
            border-color: #4ade80; box-shadow: 0 5px 0 #166534;
        }
        .btn-gold {
            background: linear-gradient(180deg, #eab308 0%, #ca8a04 100%);
            border-color: #fef08a; box-shadow: 0 5px 0 #854d0e; text-shadow: 0 2px 4px rgba(0,0,0,0.8);
        }

        .char-grid {
            display: grid; grid-template-columns: repeat(2, 1fr);
            gap: 8px; margin: 10px 0; max-height: 220px;
            overflow-y: auto; padding: 5px;
        }
        .char-btn {
            display: flex; align-items: center; gap: 8px;
            padding: 6px 10px; border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 12px; color: white; font-weight: 800;
            font-size: 12px; cursor: pointer; transition: 0.15s;
            box-shadow: 0 3px 6px rgba(0,0,0,0.4);
        }
        .char-btn:hover { transform: scale(1.02); border-color: #fff; }
        .char-icon {
            width: 32px; height: 32px; border-radius: 50%;
            border: 2px solid #fff; background: #fff; object-fit: cover;
        }

        .menu-btn {
            position: absolute; top: 10px; left: 10px;
            background: #ef4444; color: white; border: 2px solid #fca5a5;
            padding: 5px 10px; font-size: 12px; font-weight: bold;
            border-radius: 8px; cursor: pointer; display: none; z-index: 10;
        }

        #winModal, #tournamentModal {
            display: none; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(2, 6, 23, 0.95); border-radius: 20px;
            flex-direction: column; justify-content: center; align-items: center; z-index: 20;
        }
        #winModal h2 { font-size: 30px; color: #facc15; margin-bottom: 5px; text-shadow: 0 0 15px rgba(250, 204, 21, 0.6); }
        #winText { font-size: 18px; margin-bottom: 15px; color: #fff; font-weight: 800; text-align: center; line-height: 1.4; }

        canvas {
            border: 3px solid rgba(251, 191, 36, 0.5); border-radius: 14px;
            background: #0f172a; display: none; width: 100%; height: auto;
            box-shadow: 0 0 20px rgba(0,0,0,0.8);
        }

        .touch-controls {
            display: none; width: 100%; margin-top: 8px;
            justify-content: space-between; gap: 6px; flex-wrap: wrap;
        }
        .touch-group { display: flex; gap: 4px; }
        .t-btn {
            background: #334155; color: white; border: 2px solid rgba(56, 189, 248, 0.5);
            padding: 8px 12px; font-size: 13px; font-weight: bold;
            border-radius: 10px; cursor: pointer; user-select: none; outline: none;
            box-shadow: 0 3px 0 #1e293b;
        }
        .t-btn:active { background: #2563eb; transform: translateY(2px); box-shadow: 0 1px 0 #1e293b; }
        .super-btn {
            background: linear-gradient(180deg, #eab308 0%, #ca8a04 100%);
            border-color: #fef08a; animation: pulse 1s infinite alternate;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.08); box-shadow: 0 0 15px #eab308; }
        }

        @media (max-width: 768px) {
            .touch-controls { display: flex; }
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <button class="menu-btn" id="menuBtn" onclick="returnToMenu()">🏠 Menu</button>

        <div id="splashScreen">
            <img src="watermarked_img_1437238758356918555.jpg" alt="Ninja Soccer Intro">
            <p style="color: #facc15; margin-top: 10px; font-weight: bold; font-size: 14px;">Yuklanmoqda...</p>
        </div>

        <div id="ninjaMenu">
            <h2 style="color: #facc15; font-size: 22px; margin-bottom: 12px;">O'YIN REJIMINI TANLANG (3D 11 vs 11)</h2>
            <button class="btn" onclick="selectMode(1)">1 KISHI (VS BOT)</button>
            <button class="btn btn-green" onclick="selectMode(2)">2 KISHILIK (2 PLAYER)</button>
            <button class="btn btn-gold" onclick="selectMode(3)">🏆 TURNIR (NINJA CUP)</button>
            <div style="margin-top: 10px; font-size: 12px; color: #94a3b8; line-height: 1.4;">
                <p><b>P1 Boshqaruv:</b> A, D (Yurish) | W (Sakrash) | S (Pass / 2 marta: Almashtirish) | Q (Shot / Jutsu)</p>
                <p><b>P2 Boshqaruv:</b> J, L (Yurish) | I (Sakrash) | K (Pass / 2 marta: Almashtirish) | U (Shot / Jutsu)</p>
                <p><b>O'yinchini almashtirish:</b> Tab (PC), "Almashtir" (Mobil) yoki S/K ni 2 marta bosish</p>
            </div>
        </div>

        <div id="charSelectMenu">
            <h2 id="selectTitle" style="color: #facc15; margin-bottom: 5px; font-size: 20px;">SARDOR QAHRAMONNI TANLANG</h2>
            <div class="char-grid" id="charGrid"></div>
        </div>

        <div id="tournamentModal">
            <h2 style="color: #facc15;">🏆 NINJA CUP TURNIRI</h2>
            <div id="bracketText" style="font-size: 16px; margin: 12px 0; color: #e2e8f0; line-height: 1.5;"></div>
            <button class="btn btn-green" onclick="startTournamentMatch()">⚔️ O'YINNI BOSHLASH</button>
        </div>

        <canvas id="gameCanvas" width="1040" height="460"></canvas>

        <div id="winModal">
            <h2 id="winTitle">🏆 G'OLIB!</h2>
            <div id="winText">G'olib Bo'ldi!</div>
            <button class="btn btn-green" id="nextMatchBtn" onclick="handleWinNext()">🔄 DAVOM ETISH</button>
            <button class="btn" style="background: #ef4444; border-color: #fca5a5;" onclick="returnToMenu()">🏠 MENYU</button>
        </div>

        <div class="touch-controls" id="touchPanel">
            <div class="touch-group">
                <button class="t-btn" ontouchstart="tState.left=true" ontouchend="tState.left=false">◀️</button>
                <button class="t-btn" ontouchstart="tState.right=true" ontouchend="tState.right=false">▶️</button>
                <button class="t-btn" ontouchstart="tState.up=true" ontouchend="tState.up=false">⬆️</button>
                <button class="t-btn" ontouchstart="tState.down=true" ontouchend="tState.down=false">⬇️</button>
            </div>
            <div class="touch-group">
                <button class="t-btn" style="background:#8b5cf6;" onclick="switchActivePlayer(1)">🔄 Almashtir</button>
                <button class="t-btn super-btn" ontouchstart="triggerSuperOrShot(team1[activeP1], true)">⚡ JUTSU/SHOT</button>
                <button class="t-btn" style="background:#059669;" ontouchstart="tState.pass=true" ontouchend="tState.pass=false">Pass</button>
                <button class="t-btn" style="background:#dc2626;" ontouchstart="tState.shot=true" ontouchend="tState.shot=false">Shot</button>
            </div>
        </div>
    </div>

    <script>
        setTimeout(() => {
            document.getElementById('splashScreen').style.display = 'none';
            document.getElementById('ninjaMenu').style.display = 'block';
        }, 2000);

        const availableChars = [
            { name: "Naruto Uzumaki", shirtColor: "#f97316", hairColor: "#eab308", img: "https://api.dicebear.com/7.x/bottts/svg?seed=naruto" },
            { name: "Sasuke Uchiha", shirtColor: "#3b82f6", hairColor: "#1e1b4b", img: "https://api.dicebear.com/7.x/bottts/svg?seed=sasuke" },
            { name: "Kakashi Hatake", shirtColor: "#1e293b", hairColor: "#94a3b8", img: "https://api.dicebear.com/7.x/bottts/svg?seed=kakashi" },
            { name: "Madara Uchiha", shirtColor: "#991b1b", hairColor: "#0f172a", img: "https://api.dicebear.com/7.x/bottts/svg?seed=madara" },
            { name: "Itachi Uchiha", shirtColor: "#0f172a", hairColor: "#18181b", img: "https://api.dicebear.com/7.x/bottts/svg?seed=itachi" },
            { name: "Hashirama Senju", shirtColor: "#15803d", hairColor: "#14532d", img: "https://api.dicebear.com/7.x/bottts/svg?seed=hashirama" },
            { name: "Minato Namikaze", shirtColor: "#facc15", hairColor: "#fef08a", img: "https://api.dicebear.com/7.x/bottts/svg?seed=minato" },
            { name: "Pain (Nagato)", shirtColor: "#b91c1c", hairColor: "#ff7675", img: "https://api.dicebear.com/7.x/bottts/svg?seed=pain" },
            { name: "Obito Uchiha", shirtColor: "#475569", hairColor: "#000000", img: "https://api.dicebear.com/7.x/bottts/svg?seed=obito" },
            { name: "Might Guy", shirtColor: "#16a34a", hairColor: "#000000", img: "https://api.dicebear.com/7.x/bottts/svg?seed=guy" }
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
        let tournamentModal = document.getElementById('tournamentModal');
        let bracketText = document.getElementById('bracketText');

        let gameMode = 1;
        let tournamentStage = 1;
        let selectStep = 1;
        let score1 = 0, score2 = 0;
        let timeLeft = 60;
        let timerInterval = null;
        let particles = [];
        let ballTrail = [];

        let team1 = [];
        let team2 = [];
        let activeP1 = 0; 
        let activeP2 = 0; 

        let lastSPressTime = 0;
        let lastKPressTime = 0;

        let ball = { x: 520, y: 240, z: 0, r: 10, vx: 0, vy: 0, vz: 0, color: "#facc15", super: false };
        let keys = {};
        let tState = { left: false, right: false, up: false, down: false, pass: false, shot: false };
        let gameRunning = false;

        window.addEventListener("keydown", e => {
            keys[e.code] = true; keys[e.key.toLowerCase()] = true;
            
            if (e.code === "KeyS" || e.key.toLowerCase() === "s") {
                let now = Date.now();
                if (now - lastSPressTime < 300) {
                    switchActivePlayer(1);
                    lastSPressTime = 0;
                } else {
                    lastSPressTime = now;
                }
            }

            if ((e.code === "KeyK" || e.key.toLowerCase() === "k") && gameMode === 2) {
                let now = Date.now();
                if (now - lastKPressTime < 300) {
                    switchActivePlayer(2);
                    lastKPressTime = 0;
                } else {
                    lastKPressTime = now;
                }
            }

            if (e.code === "KeyQ") {
                triggerSuperOrShot(team1[activeP1], true);
            }
            if (e.code === "KeyU" && gameMode === 2) {
                triggerSuperOrShot(team2[activeP2], false);
            }
            if (e.code === "Tab") { e.preventDefault(); switchActivePlayer(1); }
        });

        window.addEventListener("keyup", e => { keys[e.code] = false; keys[e.key.toLowerCase()] = false; });

        function switchActivePlayer(teamNum) {
            if (teamNum === 1) {
                activeP1 = (activeP1 + 1) % team1.length;
            } else {
                activeP2 = (activeP2 + 1) % team2.length;
            }
        }

        function triggerSuperOrShot(p, isTeam1) {
            if (p.chakra >= 100) {
                p.chakra = 0;
                ball.super = true;
                let dir = isTeam1 ? 1 : -1;
                ball.vx = dir * 22;
                ball.vz = -4;
                createExplosion(ball.x, ball.y, p.shirtColor);
            } else {
                let dir = isTeam1 ? 1 : -1;
                let dist = Math.hypot(ball.x - p.x, ball.y - p.y);
                if (dist < 45) {
                    ball.vx = dir * 14;
                    ball.vz = -5;
                    ball.vy = (Math.random() - 0.5) * 4;
                }
            }
        }

        function createExplosion(x, y, color) {
            for (let i = 0; i < 20; i++) {
                particles.push({
                    x: x, y: y,
                    vx: (Math.random() - 0.5) * 10,
                    vy: (Math.random() - 0.5) * 10,
                    r: Math.random() * 4 + 1,
                    color: color, life: 25
                });
            }
        }

        function selectMode(mode) {
            gameMode = mode; selectStep = 1; tournamentStage = 1;
            ninjaMenu.style.display = 'none';
            charSelectMenu.style.display = 'block';
            buildCharGrid();
        }

        function buildCharGrid() {
            charGrid.innerHTML = "";
            selectTitle.innerText = gameMode === 1 || gameMode === 3 ? "SARDOR QAHRAMONNI TANLANG" : (selectStep === 1 ? "1-JAMOASINING SARDORI" : "2-JAMOASINING SARDORI");
            availableChars.forEach(char => {
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
                team1 = createTeam(char, true);
                let remaining = availableChars.filter(c => c.name !== char.name);
                let botCaptain = remaining[Math.floor(Math.random() * remaining.length)];
                team2 = createTeam(botCaptain, false);
                activeP1 = 0; activeP2 = 0;
                startNinjaGame();
            } else if (gameMode === 2) {
                if (selectStep === 1) {
                    team1 = createTeam(char, true);
                    selectStep = 2; buildCharGrid();
                } else {
                    team2 = createTeam(char, false);
                    activeP1 = 0; activeP2 = 0;
                    startNinjaGame();
                }
            } else if (gameMode === 3) {
                team1 = createTeam(char, true);
                setupTournamentMatch();
            }
        }

        function createTeam(captainChar, isLeft) {
            let team = [];
            let startX = isLeft ? 180 : 600;
            let dir = isLeft ? 1 : -1;

            let gkX = isLeft ? 80 : 960;
            team.push({ x: gkX, y: 240, z: 0, homeX: gkX, homeY: 240, r: 15, vx: 0, vy: 0, shirtColor: captainChar.shirtColor, hairColor: captainChar.hairColor, name: "GK: " + captainChar.name, chakra: 100, anim: 0, role: 'GK' });

            for (let i = 0; i < 4; i++) {
                let rnd = availableChars[Math.floor(Math.random() * availableChars.length)];
                let hX = startX + (i % 2) * 55 * dir;
                let hY = i < 2 ? 160 : 320;
                team.push({ x: hX, y: hY, z: 0, homeX: hX, homeY: hY, r: 14, vx: 0, vy: 0, shirtColor: rnd.shirtColor, hairColor: rnd.hairColor, name: rnd.name, chakra: 100, anim: 0, role: 'DEF' });
            }

            for (let i = 0; i < 4; i++) {
                let rnd = availableChars[Math.floor(Math.random() * availableChars.length)];
                let hX = startX + 200 + (i % 2) * 65 * dir;
                let hY = i < 2 ? 140 : 340;
                team.push({ x: hX, y: hY, z: 0, homeX: hX, homeY: hY, r: 14, vx: 0, vy: 0, shirtColor: rnd.shirtColor, hairColor: rnd.hairColor, name: rnd.name, chakra: 100, anim: 0, role: 'MID' });
            }

            let fwdX1 = startX + 360 * dir;
            team.push({ x: fwdX1, y: 190, z: 0, homeX: fwdX1, homeY: 190, r: 15, vx: 0, vy: 0, shirtColor: captainChar.shirtColor, hairColor: captainChar.hairColor, name: "FWD: " + captainChar.name, chakra: 100, anim: 0, role: 'FWD' });
            
            let rndF = availableChars[Math.floor(Math.random() * availableChars.length)];
            let fwdX2 = startX + 360 * dir;
            team.push({ x: fwdX2, y: 290, z: 0, homeX: fwdX2, homeY: 290, r: 14, vx: 0, vy: 0, shirtColor: rndF.shirtColor, hairColor: rndF.hairColor, name: rndF.name, chakra: 100, anim: 0, role: 'FWD' });

            return team;
        }

        function setupTournamentMatch() {
            charSelectMenu.style.display = 'none';
            tournamentModal.style.display = 'flex';
            let remaining = availableChars.filter(c => c.name !== team1[0].name);
            let botCaptain = remaining[Math.floor(Math.random() * remaining.length)];
            team2 = createTeam(botCaptain, false);
            activeP1 = 0; activeP2 = 0;

            let stageTitle = tournamentStage === 1 ? "1/4 FINAL" : (tournamentStage === 2 ? "YARIM FINAL" : "🔥 KATTA FINAL 🔥");
            bracketText.innerHTML = `<b>BOSHQICH: ${stageTitle}</b><br><br><span style="color:${team1[0].shirtColor}">${team1[0].name} (Sardor)</span> jamoasi VS <span style="color:${team2[0].shirtColor}">${botCaptain.name} (Sardor)</span> jamoasi`;
        }

        function startTournamentMatch() {
            tournamentModal.style.display = 'none';
            startNinjaGame();
        }

        function startNinjaGame() {
            charSelectMenu.style.display = 'none'; winModal.style.display = 'none'; tournamentModal.style.display = 'none';
            canvas.style.display = 'block'; touchPanel.style.display = 'flex'; menuBtn.style.display = 'block';
            score1 = 0; score2 = 0; timeLeft = 60;
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                if (gameRunning) {
                    timeLeft--;
                    [...team1, ...team2].forEach(p => { if (p.chakra < 100) p.chakra += 3; });
                    if (timeLeft <= 0) endGame();
                }
            }, 1000);
            resetPositions(); gameRunning = true; loop();
        }

        function restartGame() { startNinjaGame(); }

        function handleWinNext() {
            if (gameMode === 3 && score1 > score2) {
                if (tournamentStage < 3) {
                    tournamentStage++;
                    setupTournamentMatch();
                } else {
                    returnToMenu();
                }
            } else {
                restartGame();
            }
        }

        function endGame() {
            gameRunning = false; clearInterval(timerInterval); winModal.style.display = 'flex';
            let nextBtn = document.getElementById("nextMatchBtn");

            if (score1 > score2) {
                if (gameMode === 3) {
                    if (tournamentStage < 3) {
                        winTitle.innerText = "🎉 G'ALABA!";
                        winText.innerText = `Jamoangiz navbatdagi bosqichga o'tdi!\nHisob: ${score1} : ${score2}`;
                        nextBtn.innerText = "➡️ KEYINGI BOSHQICH";
                    } else {
                        winTitle.innerText = "🏆 NINJA CUP CHEMPIONI! 🏆";
                        winText.innerText = `🌟 TABRIKLAYMIZ! 🌟\nSizning jamoangiz Katta Finalda g'olib bo'lib Kubokni qo'lga kiritdi! 🏆`;
                        nextBtn.innerText = "🏠 MENYUGA QAYTISH";
                    }
                } else {
                    winTitle.innerText = "🏆 WINNER!";
                    winText.innerText = `1-Jamoa G'olib Bo'ldi!\nHisob: ${score1} : ${score2}`;
                    nextBtn.innerText = "🔄 QAYTA O'YNASH";
                }
                winTitle.style.color = "#facc15";
                winText.style.color = team1[0].shirtColor;
                createExplosion(520, 240, "#facc15");
            } else if (score2 > score1) {
                if (gameMode === 1 || gameMode === 3) {
                    winTitle.innerText = "❌ YUTQAZDINGIZ!";
                    winTitle.style.color = "#ef4444";
                    winText.innerText = `Turnirdan chiqib ketdingiz!\nRaقیb jamoa yutdi: ${score2} : ${score1}`;
                    nextBtn.innerText = "🔄 QAYTA URINIB KO'RISH";
                } else {
                    winTitle.innerText = "🏆 WINNER!";
                    winTitle.style.color = "#facc15";
                    winText.innerText = `2-Jamoa G'olib Bo'ldi!\nHisob: ${score2} : ${score1}`;
                    nextBtn.innerText = "🔄 QAYTA O'YNASH";
                }
            } else {
                winTitle.innerText = "🤝 DURANG!";
                winTitle.style.color = "#facc15";
                winText.innerText = `O'yin tenglik bilan tugadi!\nHisob: ${score1} : ${score2}`;
                nextBtn.innerText = "🔄 QAYTA O'YNASH";
            }
        }

        function returnToMenu() {
            gameRunning = false; clearInterval(timerInterval);
            canvas.style.display = 'none'; touchPanel.style.display = 'none'; menuBtn.style.display = 'none';
            charSelectMenu.style.display = 'none'; winModal.style.display = 'none'; tournamentModal.style.display = 'none';
            ninjaMenu.style.display = 'block';
        }

        function resetPositions() {
            team1.forEach(p => { p.x = p.homeX; p.y = p.homeY; p.z = 0; p.vx = 0; p.vy = 0; });
            team2.forEach(p => { p.x = p.homeX; p.y = p.homeY; p.z = 0; p.vx = 0; p.vy = 0; });
            ball.x = 520; ball.y = 240; ball.z = 0; ball.vx = 0; ball.vy = 0; ball.vz = 0; ball.super = false;
        }

        function update() {
            if (!gameRunning) return;

            team1.forEach((p, i) => {
                if (i === activeP1) {
                    let moveX = 0;
                    if (keys["KeyA"] || keys["a"] || tState.left) moveX = -3.8;
                    if (keys["KeyD"] || keys["d"] || tState.right) moveX = 3.8;
                    if ((keys["KeyW"] || keys["w"] || tState.up) && p.z === 0) p.vz = -7.5;
                    p.vx = moveX;
                } else {
                    if (ball.x > p.x + 8) p.vx = 1.8;
                    else if (ball.x < p.x - 8) p.vx = -1.8;
                    else p.vx = 0;
                    if (ball.y > p.y + 8) p.vy = 1.5;
                    else if (ball.y < p.y - 8) p.vy = -1.5;
                    else p.vy = 0;
                }
            });

            team2.forEach((p, i) => {
                if (gameMode === 2 && i === activeP2) {
                    let moveX = 0;
                    if (keys["KeyJ"] || keys["j"]) moveX = -3.8;
                    if (keys["KeyL"] || keys["l"]) moveX = 3.8;
                    if ((keys["KeyI"] || keys["i"]) && p.z === 0) p.vz = -7.5;
                    p.vx = moveX;
                } else {
                    if (ball.x > p.x + 8) p.vx = 2;
                    else if (ball.x < p.x - 8) p.vx = -2;
                    else p.vx = 0;
                    if (ball.y > p.y + 8) p.vy = 1.5;
                    else if (ball.y < p.y - 8) p.vy = -1.5;
                    else p.vy = 0;
                    if (p.chakra >= 100 && Math.hypot(ball.x - p.x, ball.y - p.y) < 70) triggerSuperOrShot(p, false);
                }
            });

            [...team1, ...team2].forEach(p => {
                p.z += p.vz; p.vz += 0.4;
                if (p.z > 0) { p.z = 0; p.vz = 0; }
                p.x += p.vx; p.y += p.vy;
                if (p.vx !== 0 || p.vy !== 0) p.anim += 0.2; else p.anim = 0;

                if (p.x < 50) p.x = 50; if (p.x > 990) p.x = 990;
                if (p.y < 100) p.y = 100; if (p.y > 380) p.y = 380;
            });

            [...team1, ...team2].forEach((p, idx) => {
                let dist = Math.hypot(ball.x - p.x, ball.y - p.y);
                if (dist < ball.r + p.r + 5 && Math.abs(ball.z - p.z) < 20) {
                    ball.super = false;
                    let isTeam1 = idx < 11;
                    let passKey = isTeam1 ? (keys["KeyS"] || keys["s"] || tState.pass) : (keys["KeyK"] || keys["k"]);
                    
                    if (passKey) {
                        ball.vx = isTeam1 ? 8.5 : -8.5; ball.vy = (Math.random() - 0.5) * 4; ball.vz = -3.5;
                    } else {
                        ball.vx = (ball.x - p.x) * 0.45;
                        ball.vy = (ball.y - p.y) * 0.45;
                    }
                }
            });

            ball.z += ball.vz; ball.vz += 0.35;
            ball.x += ball.vx; ball.y += ball.vy;

            ballTrail.push({ x: ball.x, y: ball.y, z: ball.z, super: ball.super });
            if (ballTrail.length > 8) ballTrail.shift();

            if (ball.x - ball.r < 30) { ball.x = 30; ball.vx *= -0.8; }
            if (ball.x + ball.r > 1010) { ball.x = 1010 - ball.r; ball.vx *= -0.8; }
            if (ball.y - ball.r < 80) { ball.y = 80 + ball.r; ball.vy *= -0.8; }
            if (ball.y + ball.r > 400) { ball.y = 400 - ball.r; ball.vy *= -0.8; }
            if (ball.z > 0) { ball.z = 0; ball.vz *= -0.6; ball.vx *= 0.95; ball.vy *= 0.95; }

            if (ball.x < 60 && ball.y > 160 && ball.y < 320 && ball.z > -35) {
                score2++; createExplosion(ball.x, ball.y, "#ef4444"); resetPositions();
            }
            if (ball.x > 980 && ball.y > 160 && ball.y < 320 && ball.z > -35) {
                score1++; createExplosion(ball.x, ball.y, "#22c55e"); resetPositions();
            }

            for (let i = particles.length - 1; i >= 0; i--) {
                let pt = particles[i];
                pt.x += pt.vx; pt.y += pt.vy; pt.life--;
                if (pt.life <= 0) particles.splice(i, 1);
            }
        }

        function drawStadium() {
            let bgGrad = ctx.createLinearGradient(0, 0, 0, 460);
            bgGrad.addColorStop(0, "#020617");
            bgGrad.addColorStop(0.5, "#0f172a");
            bgGrad.addColorStop(1, "#1e293b");
            ctx.fillStyle = bgGrad; ctx.fillRect(0, 0, 1040, 460);

            let pitchGrad = ctx.createLinearGradient(0, 80, 0, 420);
            pitchGrad.addColorStop(0, "#15803d");
            pitchGrad.addColorStop(1, "#16a34a");
            ctx.fillStyle = pitchGrad;
            ctx.beginPath();
            ctx.moveTo(100, 80); ctx.lineTo(940, 80);
            ctx.lineTo(1020, 420); ctx.lineTo(20, 420);
            ctx.closePath();
            ctx.fill();

            ctx.strokeStyle = "rgba(255,255,255,0.7)"; ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.moveTo(100, 80); ctx.lineTo(940, 80);
            ctx.lineTo(1020, 420); ctx.lineTo(20, 420);
            ctx.closePath();
            ctx.stroke();

            ctx.beginPath(); ctx.moveTo(520, 80); ctx.lineTo(520, 420); ctx.stroke();
            ctx.beginPath(); ctx.ellipse(520, 250, 60, 30, 0, 0, Math.PI * 2); ctx.stroke();

            ctx.strokeStyle = "#ffffff"; ctx.lineWidth = 4;
            ctx.strokeRect(20, 160, 40, 180);
            ctx.fillStyle = "rgba(255,255,255,0.1)";
            ctx.fillRect(20, 160, 40, 180);

            ctx.strokeStyle = "#ffffff"; ctx.lineWidth = 4;
            ctx.strokeRect(980, 160, 40, 180);
            ctx.fillStyle = "rgba(255,255,255,0.1)";
            ctx.fillRect(980, 160, 40, 180);
        }

        function drawHeadPlayer(p, isActive, isTeam1) {
            let cx = p.x;
            let cy = p.y + p.z;
            let swing = Math.sin(p.anim) * 4;

            ctx.fillStyle = "rgba(0,0,0,0.5)";
            ctx.beginPath(); ctx.ellipse(p.x, p.y + 16, 14, 7, 0, 0, Math.PI * 2); ctx.fill();

            ctx.fillStyle = "#0f172a";
            ctx.fillRect(cx - 6 + swing, cy + 9, 5, 8);
            ctx.fillRect(cx + 1 - swing, cy + 9, 5, 8);

            ctx.fillStyle = p.shirtColor;
            ctx.beginPath();
            ctx.roundRect(cx - 9, cy - 6, 18, 17, 4);
            ctx.fill();
            ctx.strokeStyle = "#ffffff";
            ctx.lineWidth = 1.2;
            ctx.stroke();

            ctx.fillStyle = "#ffedd5";
            ctx.beginPath(); ctx.arc(cx, cy - 16, 13, 0, Math.PI * 2); ctx.fill();
            ctx.strokeStyle = isActive ? "#facc15" : "#000000"; 
            ctx.lineWidth = isActive ? 3 : 1.5; 
            ctx.stroke();

            ctx.fillStyle = p.hairColor;
            ctx.beginPath(); ctx.arc(cx, cy - 20, 12, Math.PI, Math.PI * 2); ctx.fill();

            if (isActive) {
                ctx.fillStyle = "#facc15";
                ctx.beginPath();
                ctx.moveTo(cx, cy - 34); 
                ctx.lineTo(cx - 7, cy - 44); 
                ctx.lineTo(cx + 7, cy - 44);
                ctx.closePath();
                ctx.fill();
                ctx.strokeStyle = "#000";
                ctx.lineWidth = 1.2;
                ctx.stroke();
            }

            ctx.fillStyle = "rgba(0,0,0,0.8)"; 
            ctx.fillRect(cx - 15, cy - 50, 30, 5);
            ctx.fillStyle = p.chakra >= 100 ? "#facc15" : "#3b82f6";
            ctx.fillRect(cx - 15, cy - 50, (p.chakra / 100) * 30, 5);
        }

        function draw() {
            drawStadium();

            let allEntities = [
                ...team1.map(p => ({ ...p, isPlayer: true, isTeam1: true })),
                ...team2.map(p => ({ ...p, isPlayer: true, isTeam1: false })),
                { ...ball, isBall: true, y: ball.y + ball.z }
            ];

            allEntities.sort((a, b) => a.y - b.y);

            allEntities.forEach(ent => {
                if (ent.isPlayer) {
                    let realP = ent.isTeam1 ? team1.find(p => p.name === ent.name) : team2.find(p => p.name === ent.name);
                    let isActive = (ent.isTeam1 && team1.indexOf(realP) === activeP1) || (!ent.isTeam1 && team2.indexOf(realP) === activeP2);
                    drawHeadPlayer(realP, isActive, ent.isTeam1);
                } else if (ent.isBall) {
                    ctx.fillStyle = "rgba(0,0,0,0.5)";
                    ctx.beginPath(); ctx.ellipse(ball.x, ball.y + 8, ball.r, ball.r/2, 0, 0, Math.PI * 2); ctx.fill();

                    ballTrail.forEach((t, index) => {
                        ctx.fillStyle = t.super ? `rgba(250, 204, 21, ${index / 10})` : `rgba(255, 255, 255, ${index / 12})`;
                        ctx.beginPath(); ctx.arc(t.x, t.y + t.z, ball.r * (index / 8), 0, Math.PI * 2); ctx.fill();
                    });

                    ctx.shadowBlur = ball.super ? 18 : 10;
                    ctx.shadowColor = ball.super ? "#ef4444" : "#facc15";
                    ctx.fillStyle = ball.color;
                    ctx.beginPath(); ctx.arc(ball.x, ball.y + ball.z, ball.r, 0, Math.PI * 2); ctx.fill();
                    ctx.shadowBlur = 0;
                }
            });

            particles.forEach(pt => {
                ctx.fillStyle = pt.color;
                ctx.beginPath(); ctx.arc(pt.x, pt.y, pt.r, 0, Math.PI * 2); ctx.fill();
            });

            ctx.fillStyle = "#facc15"; ctx.font = "bold 24px Trebuchet MS"; ctx.textAlign = "center";
            ctx.fillText(`${score1} : ${score2}`, 520, 35);

            ctx.fillStyle = "#38bdf8"; ctx.font = "bold 16px Trebuchet MS";
            ctx.fillText(`⏱️ 00:${timeLeft < 10 ? '0' + timeLeft : timeLeft}`, 520, 15);
        }

        function loop() {
            if (!gameRunning) return;
            update(); draw(); requestAnimationFrame(loop);
        }
    </script>
</body>
</html>
