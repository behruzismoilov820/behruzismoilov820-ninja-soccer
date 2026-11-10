<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>🥷 NINJA SOCCER 3D ⚽</title>
    <style>
        * { box-sizing: border-box; user-select: none; -webkit-user-select: none; }
        body {
            margin: 0; padding: 0;
            background-color: #0f172a;
            color: #fff;
            font-family: 'Segoe UI', sans-serif;
            overflow: hidden;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            min-height: 100vh;
            touch-action: manipulation;
        }
        h1 { position: absolute; top: 15px; color: #38bdf8; z-index: 10; margin: 0; font-size: 26px; text-shadow: 0 0 10px rgba(56, 189, 248, 0.5); }
        #canvas3d { width: 100vw; height: 100vh; display: block; }
        
        /* Mobil boshqaruv tugmalari */
        #mobileControls {
            position: absolute; bottom: 30px; width: 100%;
            display: flex; justify-content: space-between; padding: 0 40px;
            z-index: 10; pointer-events: none;
        }
        .control-group { display: flex; gap: 15px; pointer-events: auto; }
        .touch-btn {
            width: 65px; height: 65px; background: rgba(255,255,255,0.15);
            border: 2px solid rgba(255,255,255,0.4); border-radius: 50%; color: white;
            font-size: 22px; font-weight: bold; display: flex; align-items: center; justify-content: center;
            touch-action: none;
        }
        .touch-btn:active { background: rgba(56, 189, 248, 0.6); border-color: #38bdf8; transform: scale(0.95); }
        .action-group .touch-btn { background: rgba(234, 179, 8, 0.2); border-color: rgba(234, 179, 8, 0.4); }
        .action-group .touch-btn:active { background: rgba(234, 179, 8, 0.6); }
    </style>
    <!-- Uch o'lchamli grafika uchun Three.js kutubxonasi -->
    <script src="https://cloudflare.com"></script>
</head>
<body>

    <h1>🥷 NINJA SOCCER 3D ⚽</h1>
    
    <!-- 3D O'yin ekrani -->
    <div id="canvas3d"></div>

    <!-- Mobil telefonlar uchun ekran ustidagi tugmalar -->
    <div id="mobileControls">
        <div class="control-group">
            <button class="touch-btn" id="btnLeft">←</button>
            <button class="touch-btn" id="btnUp">↑</button>
            <button class="touch-btn" id="btnRight">→</button>
        </div>
        <div class="control-group action-group">
            <button class="touch-btn" id="btnPass" style="font-size: 14px;">PASS</button>
            <button class="touch-btn" id="btnShot" style="font-size: 14px; background: rgba(239,68,68,0.3); border-color: rgba(239,68,68,0.5);">SHOT</button>
        </div>
    </div>

    <script>
        // 1. 3D Sahna, Kamera va Renderni yaratish
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x0f172a);

        const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.getElementById('canvas3d').appendChild(renderer.domElement);

        // Kamerani o'yinni yon tomondan chuqurlik bilan ko'rsatadigan burchakka o'rnatish
        camera.position.set(0, 5, 14);
        camera.lookAt(0, 2, 0);

        // 2. Chiroqlar (Yorug'lik va soyalar uchun)
        const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
        scene.add(ambientLight);
        const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
        dirLight.position.set(10, 20, 10);
        scene.add(dirLight);

        // 3. 3D Geometrik ob'ektlarni yaratish
        // Yashil futbol maydoni (3D platforma)
        const fieldGeo = new THREE.BoxGeometry(20, 0.5, 10);
        const fieldMat = new THREE.MeshStandardMaterial({ color: 0x10b981, roughness: 0.5 });
        const field = new THREE.Mesh(fieldGeo, fieldMat);
        field.position.y = -0.25;
        scene.add(field);

        // P1 (Qizil o'yinchi kubi)
        const p1Geo = new THREE.BoxGeometry(1, 1.8, 1);
        const p1Mat = new THREE.MeshStandardMaterial({ color: 0xef4444 });
        const p1Mesh = new THREE.Mesh(p1Geo, p1Mat);
        p1Mesh.position.set(-6, 0.9, 0);
        scene.add(p1Mesh);

        // P2 (Ko'k o'yinchi - Bot kubi)
        const p2Geo = new THREE.BoxGeometry(1, 1.8, 1);
        const p2Mat = new THREE.MeshStandardMaterial({ color: 0x3b82f6 });
        const p2Mesh = new THREE.Mesh(p2Geo, p2Mat);
        p2Mesh.position.set(6, 0.9, 0);
        scene.add(p2Mesh);

        // To'p (Sariq 3D shar)
        const ballGeo = new THREE.SphereGeometry(0.4, 32, 32);
        const ballMat = new THREE.MeshStandardMaterial({ color: 0xfacc15, roughness: 0.2 });
        const ballMesh = new THREE.Mesh(ballGeo, ballMat);
        ballMesh.position.set(0, 4, 0);
        scene.add(ballMesh);

        // 4. Fizika parametrlari
        let p1 = { x: -6, y: 0.9, vx: 0, vy: 0, jump: false };
        let ball = { x: 0, y: 4, vx: 0, vy: 0 };
        const gravity = -0.015;
        let keys = {};

        // Kompyuter uchun klaviatura boshqaruvi
        window.addEventListener("keydown", e => keys[e.key.toLowerCase()] = true);
        window.addEventListener("keyup", e => keys[e.key.toLowerCase()] = false);

        // Telefonlar uchun mobil sensorli boshqaruv
        function bindTouch(id, key) {
            const btn = document.getElementById(id);
            btn.addEventListener("touchstart", (e) => { e.preventDefault(); keys[key] = true; });
            btn.addEventListener("touchend", (e) => { e.preventDefault(); keys[key] = false; });
        }
        bindTouch("btnLeft", "a");
        bindTouch("btnRight", "d");
        bindTouch("btnUp", "w");
        bindTouch("btnPass", "s");
        bindTouch("btnShot", "e");

        // Faqat sensorli ekranlarda mobil boshqaruv tugmalarini chiqarish
        if (!('ontouchstart' in window || navigator.maxTouchPoints > 0)) {
            document.getElementById('mobileControls').style.display = 'none';
        }

        // 5. O'yin mantig'i va hisob-kitoblar (Update)
        function update() {
            // P1 (Foydalanuvchi) harakati
            if (keys["a"]) p1.vx = -0.12;
            else if (keys["d"]) p1.vx = 0.12;
            else p1.vx = 0;

            // Sakrash
            if (keys["w"] && !p1.jump) {
                p1.vy = 0.3;
                p1.jump = true;
            }

            // P1 Fizikasini qo'llash
            p1.vy += gravity; p1.y += p1.vy; p1.x += p1.vx;

            if (p1.y <= 0.9) { 
                p1.y = 0.9; p1.vy = 0; p1.jump = false;
            }
            if (p1.x < -9.5) p1.x = -9.5;
            if (p1.x > 0) p1.x = 0; 

            p1Mesh.position.x = p1.x;
            p1Mesh.position.y = p1.y;

            // P2 (Sun'iy intellekt / Bot) harakati - to'p ketidan yuradi
            if (ballMesh.position.x > p2Mesh.position.x + 0.3) p2Mesh.position.x += 0.08;
            else if (ballMesh.position.x < p2Mesh.position.x - 0.3) p2Mesh.position.x -= 0.08;
            if (p2Mesh.position.x > 9.5) p2Mesh.position.x = 9.5;
            if (p2Mesh.position.x < 0.5) p2Mesh.position.x = 0.5;

            // To'p harakati va tortishish kuchi
            ball.vy += gravity; ball.y += ball.vy; ball.x += ball.vx;

            // To'p yerga urilganda sakrashi (bouncing)
            if (ball.y <= 0.4) { 
                ball.y = 0.4;
                ball.vy = -ball.vy * 0.65; // Qaytish kuchi
                ball.vx *= 0.98; // Ishqalanish sabab sekinlashish
            }

            // Chap va o'ng devorlardan to'pning qaytishi
            if (ball.x < -9.6 || ball.x > 9.6) ball.vx *= -0.8;

            ballMesh.position.x = ball.x;
            ballMesh.position.y = ball.y;

            // To'qnashuvni aniqlash (O'yinchi to'pga tegsa)
            let distP1 = p1Mesh.position.distanceTo(ballMesh.position);
            if (distP1 < 1.2) {
                ball.vx = (ball.x - p1.x) * 0.25 + p1.vx * 0.5;
                ball.vy = (ball.y - p1.y) * 0.2 + 0.1;
                
                // PASS va SHOT tugmalari bosilgandagi qo'shimcha kuch
                if (keys["s"]) { ball.vx = 0.22; ball.vy = 0.15; }
                if (keys["e"]) { ball.vx = 0.45; ball.vy = 0.25; }
            }
        }

        // 6. Doimiy render va animatsiya sikli (Loop)
        function animate() {
            requestAnimationFrame(animate);
            update();
            renderer.render(scene, camera);
        }

        animate();

        // Ekran o'lchami o'zgarganda tasvirni buzmasdan moslashtirish
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
    </script>
</body>
</html>
