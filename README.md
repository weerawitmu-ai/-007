<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>THE PROBABILITY: CURSED EDITION</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Creepster&family=Sarabun:wght@400;700&display=swap');

        body {
            margin: 0;
            overflow: hidden;
            background-color: #000;
            color: #d00000;
            font-family: 'Sarabun', sans-serif;
            user-select: none;
        }

        /* --- BACKGROUND PULSE & HEARTBEAT --- */
        #bg-pulse {
            position: fixed;
            top: -10%;
            left: -10%;
            width: 120%;
            height: 120%;
            background: radial-gradient(circle, #3a0000 0%, #000000 70%);
            z-index: -1;
            animation: heartbeat-bg 1.2s infinite cubic-bezier(0.215, 0.61, 0.355, 1);
        }

        @keyframes heartbeat-bg {
            0% { transform: scale(1); opacity: 0.8; }
            10% { transform: scale(1.05); opacity: 1; } /* ตุบ */
            20% { transform: scale(1); opacity: 0.8; }
            30% { transform: scale(1.02); opacity: 0.9; } /* ตับ */
            100% { transform: scale(1); opacity: 0.8; }
        }

        /* --- CRT & GRAIN EFFECTS --- */
        .scanlines {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: linear-gradient(
                to bottom,
                rgba(255,255,255,0),
                rgba(255,255,255,0) 50%,
                rgba(0,0,0,0.2) 50%,
                rgba(0,0,0,0.2)
            );
            background-size: 100% 4px;
            pointer-events: none;
            z-index: 99;
        }

        .grain {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            pointer-events: none;
            z-index: 98;
            opacity: 0.15;
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
            animation: grain-flicker 0.2s infinite;
        }

        .vignette-flicker {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, transparent 50%, black 100%);
            pointer-events: none;
            z-index: 97;
            animation: flicker-light 4s infinite;
        }

        @keyframes grain-flicker {
            0% { transform: translate(0,0); }
            10% { transform: translate(-5%,-5%); }
            20% { transform: translate(-10%,5%); }
            30% { transform: translate(5%,-10%); }
            40% { transform: translate(-5%,15%); }
            50% { transform: translate(-10%,5%); }
            60% { transform: translate(15%,0); }
            70% { transform: translate(0,10%); }
            80% { transform: translate(-15%,0); }
            90% { transform: translate(10%,5%); }
            100% { transform: translate(5%,0); }
        }

        @keyframes flicker-light {
            0% { opacity: 0.8; }
            5% { opacity: 0.5; }
            10% { opacity: 0.8; }
            15% { opacity: 0.1; background: rgba(255,0,0,0.1); }
            20% { opacity: 0.8; }
            100% { opacity: 0.8; }
        }

        /* --- UI & LAYOUT --- */
        #game-container {
            position: relative;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        canvas {
            border: 2px solid #500000;
            box-shadow: 0 0 20px #ff000040;
            background-color: rgba(0,0,0,0.7);
            position: relative;
            z-index: 10;
        }

        .overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 20;
            text-align: center;
        }

        /* --- GLITCH TITLE --- */
        h1.glitch-title {
            font-family: 'Creepster', cursive;
            font-size: 4rem;
            color: #fff;
            position: relative;
            text-shadow: 2px 2px 0px #ff0000;
            animation: glitch-anim 2s infinite linear alternate-reverse;
        }

        h1.glitch-title::before, h1.glitch-title::after {
            content: attr(data-text);
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            opacity: 0.8;
        }

        h1.glitch-title::before {
            color: #0ff;
            z-index: -1;
            animation: glitch-anim-2 0.4s infinite;
        }

        h1.glitch-title::after {
            color: #f0f;
            z-index: -2;
            animation: glitch-anim-3 0.4s infinite;
        }

        @keyframes glitch-anim {
            0% { transform: translate(0); }
            20% { transform: translate(-2px, 2px); }
            40% { transform: translate(-2px, -2px); }
            60% { transform: translate(2px, 2px); }
            80% { transform: translate(2px, -2px); }
            100% { transform: translate(0); }
        }
        @keyframes glitch-anim-2 {
            0% { clip-path: inset(0 0 0 0); transform: translate(2px,0); }
            20% { clip-path: inset(20% 0 60% 0); transform: translate(-2px,0); }
            40% { clip-path: inset(80% 0 5% 0); transform: translate(2px,0); }
            100% { clip-path: inset(10% 0 40% 0); transform: translate(-2px,0); }
        }
        @keyframes glitch-anim-3 {
            0% { clip-path: inset(0 0 0 0); transform: translate(-2px,0); }
            20% { clip-path: inset(10% 0 80% 0); transform: translate(2px,0); }
            40% { clip-path: inset(50% 0 10% 0); transform: translate(-2px,0); }
            100% { clip-path: inset(0 0 0 0); transform: translate(2px,0); }
        }

        /* --- BUTTONS --- */
        .btn {
            padding: 15px 40px;
            font-size: 1.5rem;
            background: #000;
            color: #d00000;
            border: 2px solid #d00000;
            cursor: pointer;
            font-family: 'Creepster', cursive;
            transition: 0.2s;
            margin-top: 20px;
            text-transform: uppercase;
            letter-spacing: 2px;
            box-shadow: 0 0 10px #d00000;
            position: relative;
            overflow: hidden;
        }

        .btn:hover {
            background: #d00000;
            color: #000;
            box-shadow: 0 0 30px #d00000, inset 0 0 10px #000;
            transform: scale(1.05);
        }

        .btn:active {
            transform: scale(0.95);
        }

        /* --- JUMPSCARE --- */
        #scare-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: black;
            z-index: 9999;
            align-items: center;
            justify-content: center;
        }

        #scare-img {
            width: 100%; height: 100%;
            object-fit: cover;
            /* รูปผีสยองขวัญ */
            content: url('https://images.unsplash.com/photo-1519074069444-1ba4fff66d16?q=80&w=1974&auto=format&fit=crop');
            filter: contrast(150%) grayscale(100%) brightness(120%);
            animation: jump-scare-anim 0.2s infinite;
        }

        @keyframes jump-scare-anim {
            0% { transform: scale(1) translate(0,0); filter: invert(0); }
            25% { transform: scale(1.2) translate(10px, -10px); filter: invert(1); }
            50% { transform: scale(1.1) translate(-10px, 10px); filter: invert(0); }
            75% { transform: scale(1.3) translate(5px, 5px); filter: red(100%); }
            100% { transform: scale(1); }
        }

        /* Quiz & Misc */
        #quiz-modal { display: none; background: #000; border: 4px double #d00000; padding: 20px; width: 85%; max-width: 500px; box-shadow: 0 0 50px #ff000040; }
        input[type="text"] { width: 90%; padding: 10px; font-size: 1.2rem; background: #111; border: 1px solid #555; color: #fff; text-align: center; margin-bottom: 10px; font-family: 'Sarabun'; }
        #controls-hint { position: absolute; bottom: 20px; color: #666; font-size: 0.8rem; pointer-events: none; z-index: 15; text-shadow: 0 0 5px black; }
        
        .blood-drip {
            color: red;
            animation: drip 2s infinite;
        }
    </style>
</head>
<body>

<div id="bg-pulse"></div>
<div class="scanlines"></div>
<div class="grain"></div>
<div class="vignette-flicker"></div>

<div id="game-container">
    <canvas id="gameCanvas"></canvas>
    
    <div id="controls-hint">ลากเพื่อหนี... อย่าหยุด...</div>

    <div id="start-screen" class="overlay">
        <h1 class="glitch-title" data-text="THE PROBABILITY">THE PROBABILITY</h1>
        <p style="color: #aaa; max-width: 600px; font-size: 1.1rem; text-shadow: 0 0 5px #ff0000;">
            ความน่าจะเป็นแห่งความตาย...<br>
            จงพา "ดวงวิญญาณ" ไปยังจุดหมาย<br>
            <br>
            <span style="color: #d00000;">คำเตือน:</span> มีแสงวูบวาบและเสียงดัง<br>
            (กรุณาเปิดเสียงเพื่อความหลอนสูงสุด)
        </p>
        <button class="btn" onclick="startGame()">ENTER THE VOID</button>
    </div>

    <div id="quiz-modal" class="overlay">
        <h2 style="font-family: 'Creepster'; color: #ff0000; font-size: 2.5rem; margin: 0;">GATEKEEPER</h2>
        <div id="level-indicator" style="color: #666; margin-bottom: 20px;">Soul Level: 1</div>
        <p id="question-text" style="font-size: 1.2rem; margin-bottom: 20px;">Loading...</p>
        <input type="text" id="answer-input" placeholder="ตอบคำถาม..." autocomplete="off">
        <button class="btn" onclick="submitAnswer()">SACRIFICE ANSWER</button>
    </div>

    <div id="game-over-screen" class="overlay" style="display: none;">
        <h1 class="glitch-title" data-text="YOU DIED" style="font-size: 5rem; color: #d00000;">YOU DIED</h1>
        <p>วิญญาณของคุณแตกสลาย...</p>
        <button class="btn" onclick="resetGameFull()">TRY AGAIN</button>
    </div>
    
    <div id="win-screen" class="overlay" style="display: none;">
        <h1 class="glitch-title" data-text="SURVIVED" style="color: #00ff00;">SURVIVED</h1>
        <p>คุณรอดพ้นจากนรกขุมนี้แล้ว...</p>
        <button class="btn" onclick="location.reload()">REINCARNATE</button>
    </div>

    <div id="scare-overlay">
        <img id="scare-img" alt="SCREAM">
    </div>
</div>

<script>
    // --- AUDIO SYSTEM (Procedural Horror Sound) ---
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    
    // เสียง Drone พื้นหลัง + เสียงหัวใจเต้น
    function playAtmosphere() {
        if (audioCtx.state === 'suspended') audioCtx.resume();
        
        // Dark Drone
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.type = 'sawtooth';
        osc.frequency.value = 40;
        
        // LFO for drone fluctuation
        const lfo = audioCtx.createOscillator();
        lfo.frequency.value = 0.2;
        const lfoGain = audioCtx.createGain();
        lfoGain.gain.value = 10;
        lfo.connect(lfoGain);
        lfoGain.connect(osc.frequency);

        gain.gain.value = 0.08;
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.start();
        lfo.start();

        // Heartbeat Loop (Synced with CSS Animation approx 1.2s)
        setInterval(() => {
            playHeartbeat();
        }, 1200);
    }

    function playHeartbeat() {
        const t = audioCtx.currentTime;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        
        osc.frequency.setValueAtTime(60, t);
        osc.frequency.exponentialRampToValueAtTime(30, t + 0.1);
        
        gain.gain.setValueAtTime(0.5, t);
        gain.gain.exponentialRampToValueAtTime(0.001, t + 0.15);
        
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        
        osc.start(t);
        osc.stop(t + 0.2);

        // Second beat (smaller)
        const osc2 = audioCtx.createOscillator();
        const gain2 = audioCtx.createGain();
        osc2.frequency.setValueAtTime(60, t + 0.2);
        osc2.frequency.exponentialRampToValueAtTime(30, t + 0.3);
        gain2.gain.setValueAtTime(0.3, t + 0.2);
        gain2.gain.exponentialRampToValueAtTime(0.001, t + 0.35);
        
        osc2.connect(gain2);
        gain2.connect(audioCtx.destination);
        osc2.start(t + 0.2);
        osc2.stop(t + 0.4);
    }

    function playScream() {
        const t = audioCtx.currentTime;
        
        // Screeching noise
        const bufferSize = audioCtx.sampleRate * 2; // 2 seconds
        const buffer = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
        const data = buffer.getChannelData(0);
        
        for (let i = 0; i < bufferSize; i++) {
            data[i] = Math.random() * 2 - 1;
        }

        const noise = audioCtx.createBufferSource();
        noise.buffer = buffer;
        
        const filter = audioCtx.createBiquadFilter();
        filter.type = 'highpass';
        filter.frequency.value = 1000;
        
        const gain = audioCtx.createGain();
        gain.gain.setValueAtTime(2, t); // Loud!
        gain.gain.exponentialRampToValueAtTime(0.01, t + 1.5);

        noise.connect(filter);
        filter.connect(gain);
        gain.connect(audioCtx.destination);
        noise.start();

        // High pitch discord
        const osc = audioCtx.createOscillator();
        osc.type = 'sawtooth';
        osc.frequency.setValueAtTime(800, t);
        osc.frequency.linearRampToValueAtTime(1500, t + 0.1); // Scream up
        osc.frequency.linearRampToValueAtTime(100, t + 1.5); // Fall down
        
        const oscGain = audioCtx.createGain();
        oscGain.gain.setValueAtTime(0.5, t);
        oscGain.gain.linearRampToValueAtTime(0, t + 1.5);
        
        osc.connect(oscGain);
        oscGain.connect(audioCtx.destination);
        osc.start();
    }

    // --- MATH QUESTIONS ---
    const questions = [
        { q: "มีเลขโดด 0-5 สร้างเลข 3 หลัก ห้ามซ้ำ มากกว่า 300 ได้กี่จำนวน?", a: "60" },
        { q: "ลูก 3 คน คนโตหญิง คนเล็กชาย ความน่าจะเป็นคือ? (ทศนิยม)", a: "0.25" },
        { q: "ทอดลูกเต๋า 2 ลูก ผลรวมหาร 4 ลงตัว ความน่าจะเป็น? (เศษส่วนอย่างต่ำ)", a: "1/4" },
        { q: "หนังสือเลข 3 เล่ม เคมี 2 เล่ม วิชาเดียวกันติดกัน จัดได้กี่วิธี?", a: "24" },
        { q: "ไพ่ 1 ใบ ได้สีแดง หรือ K ความน่าจะเป็น? (เศษส่วน)", a: "7/13" },
        { q: "1-100 หาร 3 หรือ 5 ลงตัว ความน่าจะเป็น? (ทศนิยม)", a: "0.47" },
        { q: "บอลแดง 4 ขาว 3 หยิบ 2 ลูก สีต่างกัน ความน่าจะเป็น? (ทศนิยม 2 ตำแหน่ง)", a: "0.57" },
        { q: "P(A)=0.6, P(B)=0.5, P(AuB)=0.8 จงหา P(AnB)", a: "0.3" },
        { q: "10 จุดบนวงกลม ลากคอร์ดได้กี่เส้น?", a: "45" },
        { q: "ความน่าจะเป็นที่ 3 คนเกิดไม่ตรงกันเลย (ทศนิยม 2 ตำแหน่ง)", a: "0.99" }
    ];

    // --- GAME ENGINE ---
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    
    function resize() {
        canvas.width = window.innerWidth > 600 ? 600 : window.innerWidth - 20;
        canvas.height = window.innerHeight > 600 ? 600 : window.innerHeight * 0.6;
    }
    window.addEventListener('resize', resize);
    resize();

    let gameState = 'MENU';
    let currentLevel = 1;
    let player = { x: 50, y: 50, radius: 6 };
    let goal = { x: 0, y: 0, w: 40, h: 40 };
    let obstacles = [];
    let isDragging = false;
    let glitchOffset = 0;

    // Input
    canvas.addEventListener('mousedown', startDrag);
    canvas.addEventListener('mousemove', drag);
    canvas.addEventListener('mouseup', endDrag);
    canvas.addEventListener('touchstart', (e) => startDrag(e.touches[0]));
    canvas.addEventListener('touchmove', (e) => { e.preventDefault(); drag(e.touches[0]); });
    canvas.addEventListener('touchend', endDrag);

    function startDrag(e) {
        isDragging = true;
        // Start atmospheric sound on first touch in game if not started
        if(audioCtx.state === 'suspended') audioCtx.resume();
    }

    function drag(e) {
        if (!isDragging || gameState !== 'PLAY') return;
        const rect = canvas.getBoundingClientRect();
        let targetX = e.clientX - rect.left;
        let targetY = e.clientY - rect.top;
        
        // Easing with "lag" for horror feel
        player.x += (targetX - player.x) * 0.15;
        player.y += (targetY - player.y) * 0.15;
    }

    function endDrag() { isDragging = false; }

    function startGame() {
        document.getElementById('start-screen').style.display = 'none';
        playAtmosphere();
        initLevel(1);
    }

    function initLevel(level) {
        currentLevel = level;
        gameState = 'PLAY';
        player.x = 30;
        player.y = canvas.height / 2;
        goal.x = canvas.width - 60;
        goal.y = canvas.height / 2 - 20;

        obstacles = [];
        let count = 5 + level * 2;
        for(let i=0; i<count; i++) {
            obstacles.push({
                x: Math.random() * (canvas.width - 120) + 60,
                y: Math.random() * canvas.height,
                w: Math.random() * 30 + 20,
                h: Math.random() * 30 + 20,
                speedY: (Math.random() - 0.5) * (3 + level),
                jitter: 0
            });
        }
        loop();
    }

    function loop() {
        if (gameState !== 'PLAY') return;
        
        // Glitchy BG clear
        ctx.fillStyle = `rgba(0, 0, 0, ${0.7 + Math.random()*0.3})`;
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        // Draw Goal
        ctx.fillStyle = '#00ff00';
        ctx.shadowBlur = 15;
        ctx.shadowColor = '#00ff00';
        ctx.fillRect(goal.x + (Math.random()*2), goal.y, goal.w, goal.h);
        ctx.shadowBlur = 0;

        // Draw Player (Spirit)
        ctx.fillStyle = '#fff';
        ctx.shadowBlur = 20;
        ctx.shadowColor = '#fff';
        ctx.beginPath();
        ctx.arc(player.x, player.y, player.radius + Math.random()*2, 0, Math.PI*2);
        ctx.fill();
        ctx.shadowBlur = 0;

        // Obstacles (Ghosts/Walls)
        ctx.fillStyle = '#800000';
        ctx.shadowColor = '#ff0000';
        ctx.shadowBlur = 5;
        
        for(let obs of obstacles) {
            obs.y += obs.speedY;
            if(obs.y < 0 || obs.y + obs.h > canvas.height) obs.speedY *= -1;

            // Horror Jitter
            let jx = (Math.random() - 0.5) * 4;
            
            ctx.fillRect(obs.x + jx, obs.y, obs.w, obs.h);
            
            // Scary Face
            ctx.fillStyle = '#000';
            ctx.fillRect(obs.x + jx + 5, obs.y + 10, 5, 8); // Eye L
            ctx.fillRect(obs.x + jx + obs.w - 12, obs.y + 10, 5, 8); // Eye R
            ctx.fillRect(obs.x + jx + 10, obs.y + 30, obs.w - 20, 5 + Math.random()*5); // Mouth
            ctx.fillStyle = '#800000';

            // Collision
            if (
                player.x > obs.x && player.x < obs.x + obs.w &&
                player.y > obs.y && player.y < obs.y + obs.h
            ) {
                // Shake & Reset
                canvas.style.transform = `translate(${Math.random()*20 - 10}px, ${Math.random()*20 - 10}px)`;
                player.x = 30; player.y = canvas.height/2;
                playHeartbeat(); // Thump sound on hit
            }
        }
        canvas.style.transform = "none";

        // Goal Check
        if (player.x > goal.x && player.y > goal.y && player.y < goal.y + goal.h) {
            triggerQuiz();
            return;
        }

        requestAnimationFrame(loop);
    }

    function triggerQuiz() {
        gameState = 'QUIZ';
        document.getElementById('quiz-modal').style.display = 'flex';
        document.getElementById('level-indicator').innerText = `Level ${currentLevel}/10`;
        const qIndex = (currentLevel - 1) % questions.length;
        document.getElementById('question-text').innerText = questions[qIndex].q;
        document.getElementById('answer-input').value = '';
        document.getElementById('answer-input').focus();
    }

    function submitAnswer() {
        const input = document.getElementById('answer-input').value.trim();
        const qIndex = (currentLevel - 1) % questions.length;
        
        if (input === questions[qIndex].a) {
            document.getElementById('quiz-modal').style.display = 'none';
            if (currentLevel >= 10) {
                gameState = 'WIN';
                document.getElementById('win-screen').style.display = 'flex';
            } else {
                initLevel(currentLevel + 1);
            }
        } else {
            triggerJumpscare();
        }
    }

    function triggerJumpscare() {
        gameState = 'JUMPSCARE';
        document.getElementById('quiz-modal').style.display = 'none';
        const scare = document.getElementById('scare-overlay');
        scare.style.display = 'flex';
        
        playScream();
        
        if (navigator.vibrate) navigator.vibrate([200, 50, 200, 50, 1000]);

        // Flashing background
        let flash = setInterval(() => {
            scare.style.backgroundColor = Math.random() > 0.5 ? 'white' : 'black';
        }, 50);

        setTimeout(() => {
            clearInterval(flash);
            scare.style.display = 'none';
            document.getElementById('game-over-screen').style.display = 'flex';
        }, 2000);
    }

    function resetGameFull() {
        document.getElementById('game-over-screen').style.display = 'none';
        initLevel(1);
    }
</script>
</body>
</html>
