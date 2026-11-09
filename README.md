<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ninja Soccer - Original 1v1</title>
    <style>
        body {
            margin: 0;
            background: linear-gradient(135deg, #111, #222);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            color: white;
            overflow: hidden;
        }
        #scoreBoard {
            display: flex;
            gap: 30px;
            font-size: 28px;
            font-weight: bold;
            margin-bottom: 15px;
            background: rgba(0, 0, 0, 0.6);
            padding: 10px 30px;
            border-radius: 30px;
            border: 2px solid #ff007f;
            box-shadow: 0 0 15px #ff007f;
        }
        .score-box {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        #playerScore { color: #00f0ff; }
        #botScore { color: #ff007f; }
        canvas {
            background: linear-gradient(to bottom, #112233, #224411); /* Kechki stadion ko'rinishi */
            border: 4px solid #fff;
            box-shadow: 0px 0px 30px rgba(0, 240, 255, 0.3);
            border-radius: 12px;
        }
        .hint {
            margin-top: 15px;
            font-size: 14px;
            color: #00f0ff;
            text-transform: uppercase;
            letter-spacing: 1px;
            text-shadow: 0 0 5px #00f0ff;
        }
    </style>
</head>
<body>

    <div id="scoreBoard">
        <div class="score-box">NINJA (Siz): <span id="playerScore">0</span></div>
        <div>VS</div>
        <div class="score-box">SAMURAY (Bot): <span id="botScore">0</span></div>
    </div>
    
    <canvas id="ninjaCanvas" width="900" height="450"></canvas>
    
    <div class="hint">
        <b>Boshqarish:</b> Chap/O'ng o'qlari yoki <b>A, D</b> — Harakatlanish | Tepaga o'q yoki <b>W</b> — Sakrash
    </div>

    <script>
        const canvas = document.getElementById("ninjaCanvas");
        const ctx = canvas.getContext("2d");

        let pScore = 0;
        let bScore = 0;

        const gravity = 0.6;

        // Ninja (O'yinchi)
        const ninja = {
            x: 150,
            y: 350,
            radius: 25,
            vx: 0,
            vy: 0,
            speed: 6,
            jumpForce: -13,
            isGrounded: false,
            color: "#00f0ff"
        };

        // Samuray (Bot)
        const bot = {
            x: 750,
            y: 350,
            radius: 25,
            vx: 0,
            vy: 0,
            speed: 5,
            jumpForce: -13,
            isGrounded: false,
            color: "#ff007f"
        };

        // Koptok
        const ball = {
            x: 450,
            y: 150,
            radius: 16,
            vx: 0,
            vy: 0,
            bounce: 0.75,
            gravity: 0.4,
            friction: 0.99
        };

        // Darvozalar o'lchami va pozitsiyasi
        const goalW = 20;
        const goalH = 160;
        const leftGoal = { x: 0, y: 450 - goalH, w: goalW, h: goalH };
        const rightGoal = { x: 900 - goalW, y: 450 - goalH, w: goalW, h: goalH };

        const keys = {};
        window.addEventListener("keydown", e => keys[e.key.toLowerCase()] = true);
        window.addEventListener("keyup", e => keys[e.key.toLowerCase()] = false);

        function resetRound() {
            ninja.x = 150; ninja.y = 350; ninja.vx = 0; ninja.vy = 0;
            bot.x = 750; bot.y = 350; bot.vx = 0; bot.vy = 0;
            ball.x = 450; ball.y = 150; ball.vx = 0; ball.vy = 0;
        }

        function resolveCollision(player, ballObj) {
            let dx = ballObj.x - player.x;
            let dy = ballObj.y - player.y;
            let distance = Math.sqrt(dx * dx + dy * dy);

            if (distance < player.radius + ballObj.radius) {
                // To'qnashuv yo'nalishini aniqlash va koptokni siljitish
                let overlap = (player.radius + ballObj.radius) - distance;
                ballObj.x += (dx / distance) * overlap;
                ballObj.y += (dy / distance) * overlap;

                // Dinamik tepish kuchi va fizika hisobi
                let speed = Math.sqrt(player.vx * player.vx + player.vy * player.vy) + 7;
                ballObj.vx = (dx / distance) * speed;
                ballObj.vy = (dy / distance) * speed - 2; // Tepaga biroz yo'nalish berish
            }
        }

        function update() {
            // Ninja boshqaruvi (A, D, W yoki O'qlar)
            if (keys["a"] || keys["arrowleft"]) ninja.vx = -ninja.speed;
            else if (keys["d"] || keys["arrowright"]) ninja.vx = ninja.speed;
            else ninja.vx = 0;

            if ((keys["w"] || keys["arrowup"]) && ninja.isGrounded) {
                ninja.vy = ninja.jumpForce;
                ninja.isGrounded = false;
            }

            // Fizika va gravitatsiya - Ninja
            ninja.vy += gravity;
            ninja.x += ninja.vx;
            ninja.y += ninja.vy;

            // Chegaralar - Ninja
            if (ninja.y >= 450 - ninja.radius) {
                ninja.y = 450 - ninja.radius;
                ninja.vy = 0;
                ninja.isGrounded = true;
            }
            ninja.x = Math.max(ninja.radius + goalW, Math.min(900 - ninja.radius - goalW, ninja.x));

            // Kuchaytirilgan AI (Bot) Mantiqi
            let distToBall = ball.x - bot.x;
            if (Math.abs(distToBall) < 250) {
                if (ball.x < bot.x) bot.vx = -bot.speed;
                else bot.vx = bot.speed;

                // Agar koptok balandda va yaqin bo'lsa sakrash
                if (ball.y < bot.y - 40 && Math.abs(distToBall) < 80 && bot.isGrounded) {
                    bot.vy = bot.jumpForce;
                    bot.isGrounded = false;
                }
            } else {
                // Pozitsiyasini markazroqqa to'g'irlash
                if (bot.x < 700) bot.vx = bot.speed;
                else if (bot.x > 750) bot.vx = -bot.speed;
                else bot.vx = 0;
            }

            // Fizika va gravitatsiya - Bot
            bot.vy += gravity;
            bot.x += bot.vx;
            bot.y += bot.vy;

            if (bot.y >= 450 - bot.radius) {
                bot.y = 450 - bot.radius;
                bot.vy = 0;
                bot.isGrounded = true;
            }
            bot.x = Math.max(bot.radius + goalW, Math.min(900 - bot.radius - goalW, bot.x));

            // Koptok Harakati va Fizikasi
            ball.vy += ball.gravity;
            ball.vx *= ball.friction;
            ball.x += ball.vx;
            ball.y += ball.vy;

            // Devor va pol to'qnashuvi - Koptok
            if (ball.y >= 450 - ball.radius) {
                ball.y = 450 - ball.radius;
                ball.vy = -ball.vy * ball.bounce;
            }
            if (ball.y <= ball.radius) {
                ball.y = ball.radius;
                ball.vy = -ball.vy * ball.bounce;
            }

            // Chap tomondagi darvoza va uning ustuni
            if (ball.x <= goalW + ball.radius) {
                if (ball.y > leftGoal.y) {
                    bScore++;
                    document.getElementById("botScore").innerText = bScore;
                    resetRound();
                } else {
                    ball.x = goalW + ball.radius;
                    ball.vx = -ball.vx * ball.bounce;
                }
            }

            // O'ng tomondagi darvoza va uning ustuni
            if (ball.x >= 900 - goalW - ball.radius) {
                if (ball.y > rightGoal.y) {
                    pScore++;
                    document.getElementById("playerScore").innerText = pScore;
                    resetRound();
                } else {
                    ball.x = 900 - goalW - ball.radius;
                    ball.vx = -ball.vx * ball.bounce;
                }
            }

            // To'qnashuvlarni tekshirish va ishga tushirish
            resolveCollision(ninja, ball);
            resolveCollision(bot, ball);
        }

        function draw() {
            ctx.clearRect(0, 0, 900, 450);

            // Stadion maydoni o'rtasidagi neon chiziqlar
            ctx.strokeStyle = "rgba(255, 255, 255, 0.15)";
            ctx.lineWidth = 5;
            ctx.beginPath();
            ctx.moveTo(450, 0);
            ctx.lineTo(450, 450);
            ctx.stroke();

            // Chap Darvoza (Neon yashil/oq)
            ctx.fillStyle = "#fff";
            ctx.fillRect(leftGoal.x, leftGoal.y, leftGoal.w, leftGoal.h);
            
            // O'ng Darvoza
            ctx.fillRect(rightGoal.x, rightGoal.y, rightGoal.w, rightGoal.h);

            // Ninja chizish (Ko'z qismi effekti bilan)
            ctx.fillStyle = ninja.color;
            ctx.beginPath();
            ctx.arc(ninja.x, ninja.y, ninja.radius, 0, Math.PI * 2);
            ctx.fill();
            // Ninja tasmasi (maska effekti)
            ctx.fillStyle = "#111";
            ctx.fillRect(ninja.x - 15, ninja.y - 8, 30, 8);

            // Samuray botini chizish
            ctx.fillStyle = bot.color;
            ctx.beginPath();
            ctx.arc(bot.x, bot.y, bot.radius, 0, Math.PI * 2);
            ctx.fill();
            // Samuray ko'z qismi
            ctx.fillStyle = "#111";
            ctx.fillRect(bot.x - 15, bot.y - 8, 30, 8);

            // Koptokni chizish (Futbol dizayni elementlari)
            ctx.fillStyle = "#fff";
            ctx.beginPath();
            ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = "#111";
            ctx.lineWidth = 2;
            ctx.stroke();
        }

        function loop() {
            update();
            draw();
            requestAnimationFrame(loop);
        }


            
