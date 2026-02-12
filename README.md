# BIRTHDAY-BABY<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>For Nurin Nabilah</title>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600&family=Playfair+Display:wght@500;700&family=Quicksand:wght@300;500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

    <style>
        /* --- AESTHETIC THEME VARIABLES --- */
        :root {
            --bg-color: #fdfbfb;
            --glass: rgba(255, 255, 255, 0.7);
            --glass-border: 1px solid rgba(255, 255, 255, 0.8);
            --primary: #c08497;  /* Dusty Rose */
            --accent: #b05f6d;    /* Deep Rose */
            --text-main: #5d4037; /* Mocha */
            --paper: #fffcf5;
            --shadow: 0 10px 30px -5px rgba(192, 132, 151, 0.2);
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #fff0f3 0%, #fff5e6 100%);
            font-family: 'Quicksand', sans-serif;
            color: var(--text-main);
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* --- FLOATING HEADER --- */
        header {
            text-align: center;
            padding: 40px 20px 20px;
            animation: fadeIn 1.5s ease;
        }

        h1 {
            font-family: 'Playfair Display', serif;
            font-size: 2.2rem;
            color: var(--accent);
            margin: 0;
            line-height: 1.2;
        }

        .name-highlight {
            font-family: 'Dancing Script', cursive;
            font-size: 2.8rem;
            display: block;
            margin-top: 5px;
            background: linear-gradient(45deg, #d81b60, #ff80ab);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* --- COUNTDOWN PILLS --- */
        .countdown {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 20px auto;
        }

        .time-pill {
            background: #fff;
            padding: 8px 15px;
            border-radius: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
            text-align: center;
            min-width: 60px;
        }

        .time-val { font-weight: 700; color: var(--accent); font-size: 1.1rem; display: block; }
        .time-lbl { font-size: 0.7rem; text-transform: uppercase; color: #999; }

        /* --- INTERACTIVE ENVELOPE (THE STAR) --- */
        .envelope-container {
            height: 320px;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 20px 0;
            perspective: 1000px;
        }

        .envelope {
            width: 300px;
            height: 200px;
            background: #e6cace; /* Soft Pink Envelope */
            position: relative;
            transform-style: preserve-3d;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            border-radius: 5px;
            cursor: pointer;
            transition: transform 0.5s;
        }

        .envelope:hover { transform: translateY(-5px); }

        .flap {
            position: absolute;
            width: 0; height: 0;
            z-index: 5;
        }

        .flap.top {
            top: 0; left: 0;
            border-left: 150px solid transparent;
            border-right: 150px solid transparent;
            border-top: 110px solid #edaeb6; /* Lighter Pink */
            transform-origin: top;
            transition: transform 0.8s ease 0.2s, z-index 0.2s;
            z-index: 10;
        }

        .flap.right { top: 0; right: 0; border-top: 100px solid transparent; border-bottom: 100px solid transparent; border-right: 150px solid #dcb5bc; z-index: 4; }
        .flap.left { top: 0; left: 0; border-top: 100px solid transparent; border-bottom: 100px solid transparent; border-left: 150px solid #dcb5bc; z-index: 4; }
        .flap.bottom { bottom: 0; left: 0; border-left: 150px solid transparent; border-right: 150px solid transparent; border-bottom: 110px solid #e6cace; z-index: 4; border-radius: 0 0 5px 5px; }

        .wax-seal {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            width: 45px; height: 45px;
            background: #b71c1c;
            border-radius: 50%;
            z-index: 11;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            display: flex; justify-content: center; align-items: center;
            color: rgba(255,255,255,0.8);
            font-family: 'Dancing Script';
            font-size: 1.5rem;
            border: 2px solid rgba(255,255,255,0.1);
            transition: opacity 0.4s;
            animation: pulse 2s infinite;
        }

        .letter-preview {
            position: absolute;
            bottom: 0; left: 15px; right: 15px;
            height: 90%;
            background: #fff;
            padding: 15px;
            z-index: 2;
            transition: transform 0.8s ease 0.6s;
            text-align: center;
            display: flex; flex-direction: column; justify-content: center;
            box-shadow: 0 -2px 5px rgba(0,0,0,0.05);
        }

        /* Open State Animations */
        .envelope.open .flap.top { transform: rotateX(180deg); z-index: 1; }
        .envelope.open .wax-seal { opacity: 0; pointer-events: none; }
        .envelope.open .letter-preview { transform: translateY(-120px); z-index: 6; }

        /* Full Screen Letter Modal */
        #letter-modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(255,255,255,0.95);
            backdrop-filter: blur(5px);
            z-index: 100;
            display: flex; justify-content: center; align-items: center;
            opacity: 0; pointer-events: none;
            transition: opacity 0.5s;
        }
        #letter-modal.active { opacity: 1; pointer-events: all; }
        
        .modal-paper {
            width: 85%; max-width: 500px;
            background: #fffcf5;
            padding: 40px 30px;
            border: 1px solid #eee;
            box-shadow: 0 20px 50px rgba(0,0,0,0.15);
            text-align: center;
            position: relative;
            background-image: url('https://www.transparenttextures.com/patterns/cream-paper.png');
        }

        /* --- CARDS (Map & Quiz) --- */
        .card {
            background: var(--glass);
            border: var(--glass-border);
            margin: 20px;
            padding: 25px;
            border-radius: 20px;
            box-shadow: var(--shadow);
        }

        .card h2 { font-family: 'Playfair Display'; color: var(--accent); margin-top: 0; display: flex; align-items: center; gap: 10px; }
        
        /* Map Styles */
        #map { height: 300px; border-radius: 15px; width: 100%; margin-top: 15px; z-index: 1; }

        /* Quiz Styles */
        .quiz-btn {
            display: block; width: 100%;
            padding: 15px; margin-bottom: 10px;
            border: 1px solid #e0e0e0;
            background: #fff;
            border-radius: 12px;
            text-align: left;
            font-family: 'Quicksand';
            font-weight: 500;
            cursor: pointer;
            transition: 0.2s;
            display: flex; justify-content: space-between;
        }
        .quiz-btn.correct { background: #e8f5e9; border-color: #a5d6a7; color: #2e7d32; }
        .quiz-btn.wrong { background: #ffebee; border-color: #ef9a9a; color: #c62828; }

        /* --- CONTROLS --- */
        .music-fab {
            position: fixed; bottom: 20px; right: 20px;
            width: 50px; height: 50px;
            background: var(--accent);
            color: #fff;
            border-radius: 50%;
            display: flex; justify-content: center; align-items: center;
            box-shadow: 0 5px 15px rgba(176, 95, 109, 0.4);
            z-index: 90;
            cursor: pointer;
            border: none;
        }

        /* Animations */
        @keyframes pulse { 0% { transform: translate(-50%, -50%) scale(1); } 50% { transform: translate(-50%, -50%) scale(1.1); } 100% { transform: translate(-50%, -50%) scale(1); } }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

    </style>
</head>
<body>

    <header>
        <div style="font-size: 0.9rem; text-transform: uppercase; letter-spacing: 2px; color: #888;">February 14, 2026</div>
        <h1>Happy Birthday,<br><span class="name-highlight">Nurin Nabilah</span></h1>
        
        <div class="countdown" id="timer">
            <div class="time-pill"><span class="time-val" id="d">0</span><span class="time-lbl">Day</span></div>
            <div class="time-pill"><span class="time-val" id="h">0</span><span class="time-lbl">Hrs</span></div>
            <div class="time-pill"><span class="time-val" id="m">0</span><span class="time-lbl">Min</span></div>
        </div>
    </header>

    <div class="envelope-container" onclick="openEnvelope()">
        <div class="envelope" id="envelope">
            <div class="flap bottom"></div>
            <div class="letter-preview">
                <p style="font-family:'Dancing Script'; font-size:1.5rem; color:var(--accent); margin:0;">My Dearest,</p>
                <p style="font-size:0.8rem; color:#666;">Tap to read...</p>
            </div>
            <div class="flap left"></div>
            <div class="flap right"></div>
            <div class="flap top"></div>
            <div class="wax-seal">N</div>
        </div>
    </div>
    
    <div style="text-align: center; color: #999; font-size: 0.8rem; margin-top: -10px; margin-bottom: 30px;">
        <i class="fas fa-arrow-up"></i> Tap the seal to open
    </div>

    <div id="letter-modal" onclick="closeLetter()">
        <div class="modal-paper" onclick="event.stopPropagation()">
            <h2 style="font-family:'Dancing Script'; font-size:2.5rem; color:var(--accent); margin-bottom:20px;">To Nurin,</h2>
            <p id="letter-content" style="line-height:1.8; color:#444; font-size:1.05rem;">
                "Distance maps the miles, but my heart maps the moments. 
                <br><br>
                Happy Birthday and Happy Valentine's Day. You are the best thing that ever happened to me."
            </p>
            <br>
            <p style="font-family:'Dancing Script'; font-size:1.5rem; text-align:right;">- Yours, Nufail</p>
            
            <hr style="border:0; border-top:1px dashed #ddd; margin:20px 0;">
            
            <textarea id="edit-box" style="width:100%; border:1px solid #eee; padding:10px; display:none;" placeholder="Edit text..."></textarea>
            <div style="display:flex; gap:10px; justify-content:center;">
                <button onclick="toggleEdit()" style="background:none; border:1px solid #ccc; padding:5px 15px; border-radius:20px; font-size:0.8rem; cursor:pointer;">Edit Text</button>
                <button onclick="closeLetter()" style="background:var(--accent); color:white; border:none; padding:5px 20px; border-radius:20px; font-size:0.8rem; cursor:pointer;">Close</button>
            </div>
        </div>
    </div>

    <div class="card">
        <h2><i class="fas fa-map-pin" style="font-size:1.2rem;"></i> Our Places</h2>
        <p style="font-size:0.9rem; color:#666;">Pin a location in KL/Selangor that reminds you of us.</p>
        <div id="map"></div>
    </div>

    <div class="card">
        <h2><i class="fas fa-heart" style="font-size:1.2rem;"></i> The 'Us' Quiz</h2>
        
        <div id="q1">
            <p style="font-weight:600;">1. What is the most beautiful view?</p>
            <button class="quiz-btn" onclick="nextQ(this, false, 'q2')">Puncak Alam Sunset ❌</button>
            <button class="quiz-btn" onclick="nextQ(this, true, 'q2')">Nurin's Smile ✅</button>
        </div>

        <div id="q2" style="display:none;">
            <p style="font-weight:600;">2. Why is today special?</p>
            <button class="quiz-btn" onclick="nextQ(this, false, 'q3')">Valentine's Day ❌</button>
            <button class="quiz-btn" onclick="nextQ(this, true, 'q3')">YOUR Birthday! 🎂</button>
        </div>

        <div id="q3" style="display:none;">
            <p style="font-weight:600;">3. Be my Valentine?</p>
            <button class="quiz-btn" onclick="finishQuiz()">Yes! 🌹</button>
            <button class="quiz-btn" onclick="finishQuiz()">Absolutely! ❤️</button>
        </div>
    </div>

    <br><br><br>

    <button class="music-fab" onclick="toggleAudio()">
        <i class="fas fa-music" id="music-icon"></i>
    </button>
    <audio id="bg-music" loop src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-10.mp3"></audio>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        // --- ENVELOPE LOGIC ---
        function openEnvelope() {
            const env = document.getElementById('envelope');
            if(!env.classList.contains('open')) {
                env.classList.add('open');
                // Wait for animation then show modal
                setTimeout(() => {
                    document.getElementById('letter-modal').classList.add('active');
                }, 1200);
            } else {
                // If already open, just show modal
                document.getElementById('letter-modal').classList.add('active');
            }
        }

        function closeLetter() {
            document.getElementById('letter-modal').classList.remove('active');
        }

        // --- EDIT TEXT LOGIC ---
        let editing = false;
        function toggleEdit() {
            const p = document.getElementById('letter-content');
            const box = document.getElementById('edit-box');
            
            if(!editing) {
                // Switch to edit mode
                box.style.display = 'block';
                box.value = p.innerText;
                p.style.display = 'none';
                editing = true;
            } else {
                // Save
                p.innerText = box.value;
                p.style.display = 'block';
                box.style.display = 'none';
                editing = false;
            }
        }

        // --- QUIZ LOGIC ---
        function nextQ(btn, isCorrect, nextId) {
            if(isCorrect) {
                btn.classList.add('correct');
                setTimeout(() => {
                    btn.parentElement.style.display = 'none';
                    document.getElementById(nextId).style.display = 'block';
                }, 600);
            } else {
                btn.classList.add('wrong');
            }
        }

        function finishQuiz() {
            alert("Happy Birthday Nurin! I love you! ❤️");
        }

        // --- MAP LOGIC ---
        window.onload = function() {
            // Countdown
            setInterval(() => {
                const diff = new Date("Feb 14, 2026 00:00:00").getTime() - new Date().getTime();
                if(diff > 0) {
                    document.getElementById('d').innerText = Math.floor(diff / (1000 * 60 * 60 * 24));
                    document.getElementById('h').innerText = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                    document.getElementById('m').innerText = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                }
            }, 1000);

            // Map
            const map = L.map('map').setView([3.1390, 101.6869], 10);
            L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', { maxZoom: 19 }).addTo(map);
            
            const heartIcon = L.divIcon({
                html: '<i class="fas fa-heart" style="color:#b05f6d; font-size:24px;"></i>',
                className: 'heart-pin', iconSize: [24,24], iconAnchor: [12,24]
            });

            map.on('click', (e) => {
                const txt = prompt("Memory here?");
                if(txt) L.marker(e.latlng, {icon: heartIcon}).addTo(map).bindPopup(txt).openPopup();
            });
        };

        // --- AUDIO ---
        function toggleAudio() {
            const audio = document.getElementById('bg-music');
            const icon = document.getElementById('music-icon');
            if(audio.paused) {
                audio.play();
                icon.classList.remove('fa-music');
                icon.classList.add('fa-pause');
            } else {
                audio.pause();
                icon.classList.add('fa-music');
                icon.classList.remove('fa-pause');
            }
        }
    </script>
</body>
</html>
