HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>🥷 NINJA SOCCER 2026 - TEAM EDITION ⚽</title>
    <style>
        * { box-sizing: border-box; touch-action: manipulation; }
        body {
            margin: 0; padding: 10px;
            background: #020617; color: #fff;
            font-family: 'Trebuchet MS', 'Segoe UI', sans-serif;
            display: flex; flex-direction: column;
            align-items: center; justify-content: center;
            min-height: 100vh; overflow: hidden;
        }

        #gameContainer {
            background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%);
            padding: 15px; border-radius: 24px;
            border: 2px solid rgba(251, 191, 36, 0.4);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.9), 0 0 30px rgba(234, 179, 8, 0.2);
            display: flex; flex-direction: column; align-items: center;
            width: 100%; max-width: 860px; position: relative;
        }

        #splashScreen {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            width: 100%; height: 420px; text-align: center;
        }
        #splashScreen img {
            max-width: 100%; max-height: 370px; border-radius: 18px;
            border: 2px solid rgba(251, 191, 36, 0.6);
            box-shadow: 0 0 25px rgba(0,0,0,0.9);
        }

        #ninjaMenu, #charSelectMenu, #tournamentModal { text-align: center; padding: 10px; width: 100%; display: none; }
        
        .btn {
            display: block; width: 260px; padding: 13px; margin: 10px auto;
            background: linear-gradient(180deg, #3b82f6 0%, #1d4ed8 100%); color: white; border: 2px solid #60a5fa; border-radius: 14px;
            font-size: 16px; font-weight: 900; cursor: pointer; transition: 0.15s ease;
            text-shadow: 0 2px 4px rgba(0,0,0,0.5); box-shadow: 0 6px 0 #1e40af, 0 8px 15px rgba(0,0,0,0.4);
        }
        .btn:active { transform: translateY(4px); box-shadow: 0 2px 0 #1e40af; }
        .btn-green {
            background: linear-gradient(180deg, #22c55e 0%, #15803d 100%);
            border-color: #4ade80; box-shadow: 0 6px 0 #166534;
        }
        .btn-gold {
            background: linear-gradient(180deg, #eab308 0%, #ca8a04 100%);
            border-color: #fef08a; box-shadow: 0 6px 0 #854d0e; text-shadow: 0 2px 4px rgba(0,0,0,0.8);
        }

        .char-grid {
            display: grid; grid-template-columns: repeat(2, 1fr);
            gap: 10px; margin: 12px 0; max-height: 250px;
            overflow-y: auto; padding: 5px;
        }
        .char-btn {
            display: flex; align-items: center; gap: 10px;
            padding: 8px 12px; border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 14px; color: white; font-weight: 800;
            font-size: 13px; cursor: pointer; transition: 0.15s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.4);
        }
        .char-btn:hover { transform: scale(1.02); border-color: #fff; }
        .char-icon {
            width: 38px; height: 38px; border-radius: 50%;
            border: 2px solid #fff; background: #fff; object-fit: cover;
        }

        .menu-btn {
            position: absolute; top: 12px; left: 12px;
            background: #ef4444; color: white; border: 2px solid #fca5a5;
            padding: 6px 12px; font-size: 13px; font-weight: bold;
            border-radius: 10px; cursor: pointer; display: none; z-index: 10;
        }

        #winModal, #tournamentModal {
            display: none; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(2, 6, 23, 0.95); border-radius: 20px;
            flex-direction: column; justify-content: center; align-items: center; z-index: 20;
        }
        #winModal h2 { font-size: 34px; color: #facc15; margin-bottom: 5px; text-shadow: 0 0 15px rgba(250, 204, 21, 0.6); }
        #winText { font-size: 20px; margin-bottom: 20px; color: #fff; font-weight: 800; text-align: center; line-height: 1.4; }

        canvas {
            border: 3px solid rgba(251, 191, 36, 0.5); border-radius: 18px;
            background: #0f172a; display: none; width: 100%; height: auto;
            box-shadow: 0 0 25px rgba(0,0,0,0.8);
        }

        .touch-controls {
            display: none; width: 100%; margin-top: 10px;
            justify-content: space-between; gap: 6px; flex-wrap: wrap;
        }
        .touch-group { display: flex; gap: 5px; }
        .t-btn {
            background: #334155; color: white; border: 2px solid rgba(56, 189, 248, 0.5);
            padding: 10px 14px; font-size: 14px; font-weight: bold;
            border-radius: 12px; cursor: pointer; user-select: none; outline: none;
            box-shadow: 0 4px 0 #1e293b;
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
            <h2 style="color: #facc15; font-size: 24px; margin-bottom: 15px;">O'YIN REJIMINI TANLANG (11 vs 11)</h2>
            <button class="btn" onclick="selectMode(1)">1 KISHI (VS BOT)</button>
            <button class="btn btn-green" onclick="selectMode(2)">2 KISHILIK (2 PLAYER)</button>
            <button class="btn btn-gold" onclick="selectMode(3)">🏆 TURNIR (NINJA CUP)</button>
            <div style="margin-top: 15px; font-size: 13px; color: #94a3b8; line-height: 1.5;">
                <p><b>O'yinchini almashtirish:</b> Tab (PC) yoki "Switch" tugmasi (Mobil)</p>
                <p><b>Super Zarba:</b> P1 (Q) | P2 (U)</p>
                <p><b>Boshqaruv:</b> P1 (W,A,D | S:Pass | E:Shot) | P2 (I,J,L | K:Pass | O:Shot)</p>
            </div>
        </div>

        <div id="charSelectMenu">
            <h2 id="selectTitle" style="color: #facc15; margin-bottom: 5px;">SARDOR QAHRAMONNI TANLANG</h2>
            <div class="char-grid" id="charGrid"></div>
        </div>

        <div id="tournamentModal">
            <h2 style="color: #facc15;">🏆 NINJA CUP TURNIRI</h2>
            <div id="bracketText" style="font-size: 18px; margin: 15px 0; color: #e2e8f0; line-height: 1.6;"></div>
            <button class="btn btn-green" onclick="startTournamentMatch()">⚔️ O'YINNI BOSHLASH</button>
        </div>

        <canvas id="gameCanvas" width="820" height="420"></canvas>

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
            </div>
            <div class="touch-group">
                <button class="t-btn" style="background:#8b5cf6;" onclick="switchActivePlayer(1)">🔄 Almashtir</button>
                <button class="t-btn super-btn" ontouchstart="triggerSuper(team1[activeP1])">⚡ JUTSU</button>
                <button class="t-btn" style="background:#059669;" ontouchstart="tState.pass=true" ontouchend="tState.pass=false">Pass</button>
                <button class="t-btn" style="background:#dc2626;" ontouchstart="tState.shot=true" ontouchend="tState.shot=false">Shot</button>
                <button class="t-btn" style="background:#2563eb;" ontouchstart="tState.jump=true" ontouchend="tState.jump=false">⬆️</button>
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
        let tournamentStage = 1; // 1: 1/4 Final, 2: Yarim Final, 3: Katta Final
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

        let ball = { x: 410, y: 150, r: 12, vx: 0, vy: 0, color: "#facc15", super: false };
        let keys = {};
        let tState = { left: false, right: false, jump: false, pass: false, shot: false };
        let gameRunning = false;

        window.addEventListener("keydown", e => {
            keys[e.code] = true; keys[e.key.toLowerCase()] = true;
            if (e.code === "KeyQ") triggerSuper(team1[activeP1]);
            if (e.code === "KeyU" && gameMode === 2) triggerSuper(team2[activeP2]);
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

        function triggerSuper(p) {
            if (p.chakra >= 100) {
                p.chakra = 0;
                ball.super = true;
                let dir = (team1.includes(p)) ? 1 : -1;
                ball.vx = dir * 22;
                ball.vy = -6;
                createExplosion(ball.x, ball.y, p.shirtColor);
            }
        }

        function createExplosion(x, y, color) {
            for (let i = 0; i < 25; i++) {
                particles.push({
                    x: x, y: y,
                    vx: (Math.random() - 0.5) * 12,
                    vy: (Math.random() - 0.5) * 12,
                    r: Math.random() * 5 + 2,
                    color: color, life: 30
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
            let startX = isLeft ? 50 : 500;
            let dir = isLeft ? 1 : -1;

            team.push({ x: isLeft ? 30 : 770, y: 270, r: 20, w: 18, h: 22, vx: 0, vy: 0, shirtColor: captainChar.shirtColor, hairColor: captainChar.hairColor, name: "GK: " + captainChar.name, chakra: 100, anim: 0, role: 'GK' });

            for (let i = 0; i < 4; i++) {
                let rnd = availableChars[Math.floor(Math.random() * availableChars.length)];
                team.push({ x: startX + i * 35 * dir, y: 270, r: 18, w: 16, h: 20, vx: 0, vy: 0, shirtColor: rnd.shirtColor, hairColor: rnd.hairColor, name: rnd.name, chakra: 100, anim: 0, role: 'DEF' });
            }

            for (let i = 0; i < 4; i++) {
                let rnd = availableChars[Math.floor(Math.random() * availableChars.length)];
                team.push({ x: startX + 160 + i * 35 * dir, y: 270, r: 18, w: 16, h: 20, vx: 0, vy: 0, shirtColor: rnd.shirtColor, hairColor: rnd.hairColor, name: rnd.name, chakra: 100, anim: 0, role: 'MID' });
            }

            team.push({ x: startX + 310 * dir, y: 270, r: 20, w: 18, h: 22, vx: 0, vy: 0, shirtColor: captainChar.shirtColor, hairColor: captainChar.hairColor, name: "FWD: " + captainChar.name, chakra: 100, anim: 0, role: 'FWD' });
            
            let rndF = availableChars[Math.floor(Math.random() * availableChars.length)];
            team.push({ x: startX + 350 * dir, y: 270, r: 18, w: 16, h: 20, vx: 0, vy: 0, shirtColor: rndF.shirtColor, hairColor: rndF.hairColor, name: rndF.name, chakra: 100, anim: 0, role: 'FWD' });

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
                createExplosion(410, 200, "#facc15");
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
            team1.forEach((p, i) => {
                if(p.role === 'GK') { p.x = 30; p.y = 270; }
                else if(p.role === 'DEF') { p.x = 100 + (i * 35); p.y = 270; }
                else if(p.role === 'MID') { p.x = 250 + (i * 30); p.y = 270; }
                else { p.x = 380 + (i * 20); p.y = 270; }
                p.vx = 0; p.vy = 0;
            });
            team2.forEach((p, i) => {
                if(p.role === 'GK') { p.x = 770; p.y = 270; }
                else if(p.role === 'DEF') { p.x = 700 - (i * 35); p.y = 270; }
                else if(p.role === 'MID') { p.x = 550 - (i * 30); p.y = 270; }
                else { p.x = 440 - (i * 20); p.y = 270; }
                p.vx = 0; p.vy = 0;
            });
            ball.x = 410; ball.y = 120; ball.vx = 0; ball.vy = 0; ball.super = false;
        }

        function update() {
            if (!gameRunning) return;

            team1.forEach((p, i) => {
                if (i === activeP1) {
                    if (keys["KeyA"] || keys["a"] || tState.left) p.vx = -4.5;
                    else if (keys["KeyD"] || keys["d"] || tState.right) p.vx = 4.5;
                    else p.vx = 0;
                    if ((keys["KeyW"] || keys["w"] || tState.jump) && p.y >= 270) p.vy = -12;
                } else {
                    if (ball.x > p.x + 15) p.vx = 2;
                    else if (ball.x < p.x - 15) p.vx = -2;
                    else p.vx = 0;
                    if (ball.y < p.y && p.y >= 270 && Math.random() < 0.015) p.vy = -10;
                }
            });

            team2.forEach((p, i) => {
                if (gameMode === 2 && i === activeP2) {
                    if (keys["KeyJ"] || keys["j"]) p.vx = -4.5;
                    else if (keys["KeyL"] || keys["l"]) p.vx = 4.5;
                    else p.vx = 0;
                    if ((keys["KeyI"] || keys["i"]) && p.y >= 270) p.vy = -12;
                } else {
                    if (ball.x > p.x + 15) p.vx = 2.2;
                    else if (ball.x < p.x - 15) p.vx = -2.2;
                    else p.vx = 0;
                    if (ball.y < p.y && p.y >= 270 && Math.random() < 0.02) p.vy = -10;
                    if (p.chakra >= 100 && Math.abs(ball.x - p.x) < 70) triggerSuper(p);
                }
            });

            [...team1, ...team2].forEach(p => {
                p.vy += 0.6; p.y += p.vy; p.x += p.vx;
                if (p.vx !== 0) p.anim += 0.2; else p.anim = 0;
                if (p.y > 270) { p.y = 270; p.vy = 0; }
                if (p.x < 15) p.x = 15; if (p.x > 795) p.x = 795;
            });

            [...team1, ...team2].forEach((p, idx) => {
                let headX = p.x + p.w/2, headY = p.y + 10;
                let dist = Math.hypot(ball.x - headX, ball.y - headY);
                if (dist < ball.r + p.r) {
                    ball.super = false;
                    let isTeam1 = idx < 11;
                    let passKey = isTeam1 ? (keys["KeyS"] || keys["s"] || tState.pass) : false;
                    let shotKey = isTeam1 ? (keys["KeyE"] || keys["e"] || tState.shot) : true;
                    
                    if (passKey) {
                        ball.vx = isTeam1 ? 8 : -8; ball.vy = -5;
                    } else if (shotKey) {
                        ball.vx = isTeam1 ? 14 : -14; ball.vy = -8;
                    } else {
                        ball.vx = (ball.x - headX) * 0.4 + p.vx * 0.5;
                        ball.vy = (ball.y - headY) * 0.4;
                    }
                }
            });

            ball.vy += 0.45; ball.x += ball.vx; ball.y += ball.vy;

            ballTrail.push({ x: ball.x, y: ball.y, super: ball.super });
            if (ballTrail.length > 8) ballTrail.shift();

            if (ball.x - ball.r < 0) { ball.x = ball.r; ball.vx *= -0.8; }
            if (ball.x + ball.r > 820) { ball.x = 820 - ball.r; ball.vx *= -0.8; }
            if (ball.y - ball.r < 0) { ball.y = ball.r; ball.vy *= -0.8; }
            if (ball.y + ball.r > 370) { ball.y = 370 - ball.r; ball.vy *= -0.7; ball.vx *= 0.95; }

            if (ball.x - ball.r < 45 && ball.y > 220) {
                score2++; createExplosion(ball.x, ball.y, "#ef4444"); resetPositions();
            }
            if (ball.x + ball.r > 775 && ball.y > 220) {
                score1++; createExplosion(ball.x, ball.y, "#22c55e"); resetPositions();
            }

            for (let i = particles.length - 1; i >= 0; i--) {
                let pt = particles[i];
                pt.x += pt.vx; pt.y += pt.vy; pt.life--;
                if (pt.life <= 0) particles.splice(i, 1);
            }
        }

        function drawStadium() {
            let bgGrad = ctx.createLinearGradient(0, 0, 0, 420);
            bgGrad.addColorStop(0, "#020617");
            bgGrad.addColorStop(0.6, "#0f172a");
            bgGrad.addColorStop(1, "#1e293b");
            ctx.fillStyle = bgGrad; ctx.fillRect(0, 0, 820, 420);

            ctx.fillStyle = "rgba(255, 255, 255, 0.05)";
            ctx.beginPath(); ctx.moveTo(0, 0); ctx.lineTo(300, 370); ctx.lineTo(120, 370); ctx.fill();
            ctx.beginPath(); ctx.moveTo(820, 0); ctx.lineTo(520, 370); ctx.lineTo(700, 370); ctx.fill();

            let grass = ctx.createLinearGradient(0, 370, 0, 420);
            grass.addColorStop(0, "#16a34a"); grass.addColorStop(1, "#15803d");
            ctx.fillStyle = grass; ctx.fillRect(0, 370, 820, 50);

            ctx.strokeStyle = "rgba(255,255,255,0.6)"; ctx.lineWidth = 4;
            ctx.strokeRect(0, 220, 45, 150); ctx.strokeRect(775, 220, 45, 150);
            ctx.beginPath(); ctx.moveTo(410, 0); ctx.lineTo(410, 370); ctx.stroke();
            ctx.beginPath(); ctx.arc(410, 185, 50, 0, Math.PI * 2); ctx.stroke();
        }

        function drawHeadPlayer(p, isActive, isTeam1) {
            let cx = p.x + p.w / 2;
            let cy = p.y;
            let swing = Math.sin(p.anim) * 6;

            ctx.fillStyle = "#1e293b";
            ctx.fillRect(cx - 6 + swing, cy + 42, 5, 6);
            ctx.fillRect(cx + 1 - swing, cy + 42, 5, 6);

            ctx.fillStyle = p.shirtColor;
            ctx.fillRect(cx - 9, cy + 22, 18, 20);

            ctx.fillStyle = "#ffedd5";
            ctx.beginPath(); ctx.roundRect(cx - 15, cy - 8, 30, 30, 6); ctx.fill();
            ctx.strokeStyle = isActive ? "#facc15" : "#000"; ctx.lineWidth = isActive ? 2.5 : 1; ctx.stroke();

            ctx.fillStyle = p.hairColor;
            ctx.beginPath(); ctx.roundRect(cx - 17, cy - 12, 34, 12, [6, 6, 2, 2]); ctx.fill();

            if (isActive) {
                ctx.fillStyle = "#facc15";
                ctx.beginPath();
                ctx.moveTo(cx, cy - 16); ctx.lineTo(cx - 5, cy - 24); ctx.lineTo(cx + 5, cy - 24);
                ctx.fill();
            }

            ctx.fillStyle = "rgba(0,0,0,0.6)"; ctx.fillRect(cx - 14, cy - 30, 28, 4);
            ctx.fillStyle = p.chakra >= 100 ? "#facc15" : "#3b82f6";
            ctx.fillRect(cx - 14, cy - 30, (p.chakra / 100) * 28, 4);
        }

        function draw() {
            drawStadium();

            ballTrail.forEach((t, index) => {
                ctx.fillStyle = t.super ? `rgba(250, 204, 21, ${index / 10})` : `rgba(255, 255, 255, ${index / 12})`;
                ctx.beginPath(); ctx.arc(t.x, t.y, ball.r * (index / 8), 0, Math.PI * 2); ctx.fill();
            });

            particles.forEach(pt => {
                ctx.fillStyle = pt.color;
                ctx.beginPath(); ctx.arc(pt.x, pt.y, pt.r, 0, Math.PI * 2); ctx.fill();
            });

            ctx.fillStyle = "#facc15"; ctx.font = "bold 24px Trebuchet MS"; ctx.textAlign = "center";
            ctx.fillText(`${score1} : ${score2}`, 410, 45);

            ctx.fillStyle = "#38bdf8"; ctx.font = "bold 16px Trebuchet MS";
            ctx.fillText(`⏱️ 00:${timeLeft < 10 ? '0' + timeLeft : timeLeft}`, 410, 20);

            team1.forEach((p, i) => drawHeadPlayer(p, i === activeP1, true));
            team2.forEach((p, i) => drawHeadPlayer(p, i === activeP2, false));
            
            ctx.shadowBlur = ball.super ? 20 : 10;
            ctx.shadowColor = ball.super ? "#ef4444" : "#facc15";
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
