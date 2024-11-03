<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LaHezQuiz</title>
    <!-- Tambahkan Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        /* Gaya Dasar */
        body {
            font-family: 'Orbitron', sans-serif;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            background-image: url('https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/93b64b88-7e91-4762-bd75-ee9e42975505/daxtpz4-85d6f953-f404-475e-a720-08d18472af16.gif?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcLzkzYjY0Yjg4LTdlOTEtNDc2Mi1iZDc1LWVlOWU0Mjk3NTUwNVwvZGF4dHB6NC04NWQ2Zjk1My1mNDA0LTQ3NWUtYTcyMC0wOGQxODQ3MmFmMTYuZ2lmIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.z7Ail1AfI31gNwXJOlboOQPNsgjZU27V9QBV416NxP8');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            background-color: #2c3e50;
            background-attachment: fixed; /* Mencegah background bergerak saat scroll */
            color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow-x: hidden;
        }
        .quiz-container {
            background: rgba(0, 0, 0, 0.7);
            padding: 30px;
            border-radius: 40px;
            width: 90%;
            max-width: 800px;
            min-height: 500px;
            text-align: center;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3),
                        inset 0 0 0 2px rgba(255, 255, 255, 0.1);
            position: relative;
            transition: transform 0.3s ease;
            overflow-y: auto;
            border: 2px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            margin: 20px auto;
            max-height: 90vh; /* Maksimum tinggi 90% dari viewport height */
            -webkit-overflow-scrolling: touch; /* Smooth scrolling untuk iOS */
        }

        .quiz-container::before {
            content: '';
            position: absolute;
            top: 15px;
            left: 50%;
            transform: translateX(-50%);
            width: 10px;
            height: 10px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 50%;
            box-shadow: inset 0 0 2px rgba(255, 255, 255, 0.5);
        }

        .quiz-container::after {
            content: '';
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 40px;
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 50%;
        }

        .quiz-container h2 {
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            letter-spacing: 1px;
            font-size: 26px;
            margin: 0 0 20px;
            color: #4b8bff;
        }
        .question {
            font-size: 24px;
            margin: 25px 0;
            padding: 15px;
            background: rgba(75, 139, 255, 0.1);
            border-radius: 10px;
            width: 100%;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }
        .answer-btn {
            display: inline-block;
            background: rgba(75, 139, 255, 0.2);
            color: white;
            padding: 15px 20px;
            border: 1px solid rgba(75, 139, 255, 0.3);
            border-radius: 10px;
            margin: 0;
            width: 100%;
            font-size: 18px;
            transition: all 0.3s ease;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
        }
        .answer-btn:hover {
            background: rgba(75, 139, 255, 0.4);
            transform: translateY(-2px);
        }
        .timer {
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            font-size: 18px;
            color: #4b8bff;
            background: rgba(0, 0, 0, 0.5);
            padding: 8px 15px;
            border-radius: 8px;
            border: 1px solid rgba(75, 139, 255, 0.3);
            position: absolute;
            top: 15px;
            right: 15px;
            display: flex;
            align-items: center;
            gap: 4px;
            box-shadow: 0 0 10px rgba(75, 139, 255, 0.2);
            text-shadow: 0 0 8px rgba(75, 139, 255, 0.5);
        }

        .timer::before {
            content: '';
            width: 6px;
            height: 6px;
            background-color: #4b8bff;
            border-radius: 50%;
            margin-right: 4px;
            animation: blink 1s infinite;
        }

        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0; }
        }

        /* Timer warning state */
        .timer.warning {
            color: #ff4757;
            border-color: rgba(255, 71, 87, 0.3);
            text-shadow: 0 0 8px rgba(255, 71, 87, 0.5);
        }

        .timer.warning::before {
            background-color: #ff4757;
        }

        /* Timer critical state */
        .timer.critical {
            color: #ff0000;
            border-color: rgba(255, 0, 0, 0.3);
            text-shadow: 0 0 8px rgba(255, 0, 0, 0.5);
            animation: shake 0.5s infinite;
        }

        .timer.critical::before {
            background-color: #ff0000;
            animation: blink 0.5s infinite;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-2px); }
            75% { transform: translateX(2px); }
        }

        .score {
            font-family: 'Orbitron', sans-serif;
            font-weight: 500;
            letter-spacing: 1px;
            font-size: 24px; /* Ukuran font lebih besar */
            color: #2ecc71;
            text-align: center;
            margin: 20px 0;
        }
        /* Efek Jawaban */
        .correct {
            background-color: #2ecc71 !important;
        }
        .incorrect {
            background-color: #e74c3c !important;
        }
        /* Efek Visual */
        .firework {
            position: fixed;
            pointer-events: none;
            z-index: 999;
        }

        .spark {
            position: absolute;
            width: 4px;
            height: 4px;
            border-radius: 50%;
            animation: spark 1s ease-out forwards;
        }

        @keyframes spark {
            0% {
                transform: scale(1);
                opacity: 1;
            }
            20% {
                opacity: 1;
            }
            100% {
                transform: scale(0) translate(var(--tx), var(--ty));
                opacity: 0;
            }
        }

        .shake {
            animation: shake 0.5s;
        }
        .image {
            width: 100%;
            height: 300px;
            object-fit: cover;
            border-radius: 12px;
            margin: 20px auto;
            display: none;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .question, .answer-btn, .image {
            transition: opacity 0.3s ease;
        }

        .quiz-container {
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .inner-firework {
            position: absolute;
            pointer-events: none;
            z-index: 2;
        }

        .inner-spark {
            position: absolute;
            width: 3px;
            height: 3px;
            border-radius: 50%;
            animation: innerSpark 0.8s ease-out forwards;
        }

        @keyframes innerSpark {
            0% {
                transform: translate(0, 0) scale(1);
                opacity: 1;
            }
            100% {
                transform: translate(var(--tx), var(--ty)) scale(0);
                opacity: 0;
            }
        }

        .welcome-screen {
            text-align: center;
            animation: fadeIn 0.5s ease;
        }

        .welcome-screen h1 {
            font-family: 'Orbitron', sans-serif;
            font-weight: 700;
            letter-spacing: 2px;
            font-size: 48px;
            color: #4b8bff;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .welcome-screen p {
            font-size: 18px;
            margin-bottom: 30px;
            color: #fff;
        }

        .start-btn {
            background: #4b8bff;
            color: white;
            padding: 15px 40px;
            border: none;
            border-radius: 25px;
            font-size: 20px;
            cursor: pointer;
            transition: transform 0.3s ease, background 0.3s;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .start-btn:hover {
            background: #3a73d9;
            transform: scale(1.05);
        }

        #quiz-content {
            display: none;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {transform: translateY(0);}
            40% {transform: translateY(-20px);}
            60% {transform: translateY(-10px);}
        }

        .bounce {
            animation: bounce 2s infinite;
        }

        /* Update question-container style */
        #question-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            margin: 20px auto;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            width: 90%;
            max-width: 600px;
        }

        /* Update answer buttons container */
        .answers-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            width: 100%;
            margin-top: 20px;
        }

        /* Container untuk gambar */
        .image-container {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 80%;
            max-width: 400px;
            z-index: 1000;
            display: none;
        }

        .image {
            width: 100%;
            height: auto;
            max-height: 70vh;
            object-fit: contain;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        /* Style untuk overlay */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            z-index: 999;
            display: none;
        }

        /* Status bar style */
        .status-bar {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 24px;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 12px;
            color: rgba(255, 255, 255, 0.8);
        }

        .status-bar .time {
            font-weight: bold;
        }

        .status-bar .icons {
            display: flex;
            gap: 5px;
        }

        /* Container untuk konten utama */
        .main-content {
            padding: 20px 0;
            position: relative;
            overflow-y: visible;
        }

        /* Styling untuk scrollbar */
        .main-content::-webkit-scrollbar {
            width: 6px;
        }

        .main-content::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
        }

        .main-content::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 3px;
        }

        /* Update restart button style */
        #restart-button {
            background: rgba(75, 139, 255, 0.2);
            color: white;
            padding: 15px 30px;
            border: 1px solid rgba(75, 139, 255, 0.3);
            border-radius: 10px;
            cursor: pointer;
            font-size: 18px;
            transition: all 0.3s ease;
            margin-top: 20px;
        }

        #restart-button:hover {
            background: rgba(75, 139, 255, 0.4);
            transform: scale(1.05);
        }

        /* Style untuk tampilan skor akhir */
        .final-score-container {
            position: relative;
            transform: none;
            left: auto;
            top: auto;
            padding: 20px 0;
        }

        .final-score {
            font-size: 48px;
            color: #4b8bff;
            margin: 20px 0;
            text-shadow: 0 0 10px rgba(75, 139, 255, 0.5);
        }

        /* Style untuk tombol kembali */
        .back-button {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            padding: 12px 25px;
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 10px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s ease;
            margin-top: 15px;
            display: none;
        }

        .back-button:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.05);
        }

        .button-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }

        .username-input {
            padding: 12px 20px;
            margin: 15px 0;
            border: 2px solid rgba(75, 139, 255, 0.3);
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 18px;
            width: 80%;
            max-width: 300px;
            text-align: center;
            font-family: 'Orbitron', sans-serif;
        }

        .username-input::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }

        .leaderboard {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 20px;
            margin: 20px auto;
            width: 90%;
            max-width: 400px;
        }

        .leaderboard h3 {
            color: #4b8bff;
            margin-bottom: 15px;
        }

        .leaderboard-item {
            display: grid;
            grid-template-columns: auto 1fr auto auto;
            gap: 15px;
            padding: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            align-items: center;
        }

        .rank {
            font-weight: bold;
            color: #4b8bff;
            width: 30px;
        }

        .username {
            text-align: left;
        }

        .score, .time {
            color: #2ecc71;
        }

        /* Add styles for better scrolling on iOS */
        .quiz-container::-webkit-scrollbar {
            width: 8px;
        }

        .quiz-container::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 4px;
        }

        .quiz-container::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.3);
            border-radius: 4px;
        }

        /* Add media queries for better mobile responsiveness */
        @media screen and (max-width: 768px) {
            body {
                padding: 10px;
            }

            .quiz-container {
                width: 95%;
                padding: 20px;
                margin: 10px auto;
            }

            .answers-grid {
                grid-template-columns: 1fr; /* Single column for mobile */
                gap: 10px;
            }

            .question {
                font-size: 20px;
            }

            .answer-btn {
                padding: 12px 15px;
                font-size: 16px;
            }

            .leaderboard {
                width: 95%;
            }
        }

        /* Add padding to bottom of content to ensure last items are visible */
        .welcome-screen,
        #quiz-content {
            padding-bottom: 40px;
        }

        /* Ensure status bar stays at top */
        .status-bar {
            position: sticky;
            top: 0;
            z-index: 100;
            background: rgba(0, 0, 0, 0.7);
        }

        /* Update image container positioning */
        .image-container {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1000;
        }

        /* Update overlay positioning */
        .overlay {
            position: fixed;
            z-index: 999;
        }

        /* Digital Clock in Status Bar */
        .status-bar .time {
            font-family: 'Orbitron', sans-serif;
            font-weight: 500;
            font-size: 14px;
            color: #4b8bff;
            text-shadow: 0 0 5px rgba(75, 139, 255, 0.5);
        }

        .digital-time {
            letter-spacing: 2px;
        }

        .digital-seconds {
            font-size: 0.8em;
            opacity: 0.8;
        }

        /* Update status bar style */
        .status-bar {
            background: rgba(0, 0, 0, 0.7);
            padding: 8px 15px;
            border-radius: 20px;
            margin: 10px;
            border: 1px solid rgba(75, 139, 255, 0.2);
        }

        /* Update leaderboard styles */
        .leaderboard {
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            padding: 20px;
            margin: 20px auto;
            width: 90%;
            max-width: 500px;
            border: 1px solid rgba(75, 139, 255, 0.3);
        }

        .leaderboard h3 {
            color: #4b8bff;
            margin-bottom: 15px;
            font-size: 1.2em;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .leaderboard-item {
            display: grid;
            grid-template-columns: 50px 1fr auto auto;
            gap: 15px;
            padding: 12px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            align-items: center;
            transition: background-color 0.3s ease;
        }

        .leaderboard-item:hover {
            background: rgba(75, 139, 255, 0.1);
        }

        .rank {
            font-weight: bold;
            color: #4b8bff;
            text-align: center;
        }

        .username {
            color: #fff;
            text-align: left;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }

        .score {
            color: #2ecc71;
            font-weight: bold;
            padding-right: 15px;
        }

        .time {
            color: rgba(255, 255, 255, 0.6);
            font-size: 0.9em;
        }

        /* Responsive adjustments */
        @media screen and (max-width: 600px) {
            .leaderboard-item {
                grid-template-columns: 40px 1fr auto;
                gap: 10px;
            }
            
            .time {
                display: none; /* Hide timestamp on mobile */
            }
        }
    </style>
</head>
<body>
    <div class="quiz-container" id="quiz-container">
        <!-- Status bar -->
        <div class="status-bar">
            <span class="time" id="clock">12:00</span>
            <div class="icons">
                <span>📶</span>
                <span>🔋</span>
            </div>
        </div>

        <!-- Welcome Screen -->
        <div class="welcome-screen" id="welcome-screen">
            <h1>LaHezQuiz</h1>
            <p>Selamat datang di kuis interaktif tentang teknologi!</p>
            <input type="text" id="username-input" class="username-input" placeholder="Masukkan username Anda" required>
            <p>Jawab pertanyaan dalam waktu 15 detik</p>
            <p>Dapatkan bonus 100 poin jika semua jawaban benar!</p>
            <button class="start-btn bounce" onclick="startQuiz()">Mulai Kuis</button>
        </div>

        <!-- Tambahkan overlay -->
        <div class="overlay" id="overlay"></div>
        
        <!-- Pindahkan image container keluar dari question container -->
        <div class="image-container">
            <img id="answer-image" class="image" src="" alt="Gambar Jawaban">
        </div>

        <div id="quiz-content" class="main-content">
            <h2>LaHezQuiz</h2>
            <div class="timer" id="timer">15</div>
            <div id="question-container">
                <p class="question" id="question"></p>
                <div class="answers-grid">
                    <button class="answer-btn" onclick="selectAnswer(0)"></button>
                    <button class="answer-btn" onclick="selectAnswer(1)"></button>
                    <button class="answer-btn" onclick="selectAnswer(2)"></button>
                    <button class="answer-btn" onclick="selectAnswer(3)"></button>
                </div>
            </div>
            <div class="score" id="score">Skor: 0</div>
            <button id="restart-button" style="display:none;" onclick="restartQuiz()">Mulai Ulang Kuis</button>
        </div>
        <div class="firework" id="firework"></div>
    </div>

    <!-- Audio elements -->
    <audio id="correct-sound" preload="auto">
        <source src="correct-156911.mp3" type="audio/mpeg">
    </audio>
    <audio id="incorrect-sound" preload="auto">
        <source src="wrong-buzzer-6268.mp3" type="audio/mpeg">
    </audio>

    <script>
        const questions = [
            {
                question: "Siapa penemu WWW?",
                answers: ["Steve Jobs", "Tim Berners-Lee", "Bill Gates", "Mark Zuckerberg"],
                correct: 1,
                answerImage: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTOzLvUWhq-sLLPNZfWKgkj1Dend7_S-bi5MQ&s"
            },
            {
                question: "Apa bahasa pemrograman untuk membuat website?",
                answers: ["Python", "Java", "HTML", "C++"],
                correct: 2,
                answerImage: "https://course-net.com/blog/wp-content/uploads/2023/03/sta-je-html.jpeg"
            },
            {
                question: "Apa singkatan dari CSS?",
                answers: ["Computer Style Sheets", "Creative Style System", "Cascading Style Sheets", "Custom Style Sheets"],
                correct: 2,
                answerImage: "https://images.javatpoint.com/fullformpages/images/css.png"
            },
            {
                question: "JavaScript digunakan untuk?",
                answers: ["Menata halaman web", "Membuat fungsi interaktif", "Membuat server", "Mengedit gambar"],
                correct: 1,
                answerImage: "https://idwebhost.com/blog/wp-content/uploads/2018/12/apa-itu-javascript.jpg"
            },
            {
                question: "Apakah itu HTML?",
                answers: ["Bahasa markup untuk membuat halaman web", "Bahasa pemrograman server", "Sistem manajemen database", "Bahasa pemrograman untuk aplikasi desktop"],
                correct: 0,
            },
            {
                question: "SQL digunakan untuk?",
                answers: ["Menata halaman web", "Mengelola database", "Membuat animasi", "Mengelola jaringan"],
                correct: 1,
                answerImage: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ_lTeBk0QdOhzcE4vM82jU6DESzpm27PmiGA&s"
            },
            {
                question: "Apa fungsi dari git?",
                answers: ["Mengelola kode versi", "Menata halaman web", "Membuat server", "Mengelola email"],
                correct: 0,
                answerImage: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSdd25hyNQOMs4Xx1Cv_A_oaT0zagfSWlXMBA&s"
            },
            {
                question: "Siapa yang menciptakan Linux?",
                answers: ["Linus Torvalds", "Dennis Ritchie", "Steve Jobs", "Mark Zuckerberg"],
                correct: 0,
                answerImage: "https://upload.wikimedia.org/wikipedia/commons/thumb/0/01/LinuxCon_Europe_Linus_Torvalds_03_%28cropped%29.jpg/640px-LinuxCon_Europe_Linus_Torvalds_03_%28cropped%29.jpg"
            },
            {
                question: "Apa itu API?",
                answers: ["Antarmuka Pemrograman Aplikasi", "Sistem Manajemen Database", "Bahasa Pemrograman", "Platform Cloud"],
                correct: 0,
            },
            {
                question: "Apa yang dimaksud dengan cloud computing?",
                answers: ["Menyimpan data di server lokal", "Menyimpan data di internet", "Membuat aplikasi desktop", "Sistem operasi untuk smartphone"],
                correct: 1,
                answerImage: "https://indonesiancloud.com/wp-content/uploads/2014/03/Cloud-Computing-01.png"
            },
            {
                question: "Apa yang dimaksud dengan debugging?",
                answers: ["Menemukan dan memperbaiki kesalahan dalam kode", "Menulis kode baru", "Mengelola database", "Membuat server"],
                correct: 0,
            }
        ];

        const config = {
            totalTime: 15,
            pointsPerQuestion: 10,
            bonusPoints: 100,
            transitionDelay: 2000,
            fireworkCount: 5,
            sparkCount: 30
        };

        const gameState = {
            currentQuestionIndex: 0,
            score: 0,
            timer: null,
            remainingTime: config.totalTime,
            isAnswering: false
        };

        function startTimer() {
            gameState.remainingTime = config.totalTime;
            updateTimerDisplay();
            
            gameState.timer = setInterval(() => {
                gameState.remainingTime--;
                updateTimerDisplay();
                
                if (gameState.remainingTime <= 0) {
                    clearInterval(gameState.timer);
                    showFinalScore();
                }
            }, 1000);
        }

        function updateTimerDisplay() {
            const timerEl = document.getElementById('timer');
            
            // Format waktu dengan leading zero
            const formattedTime = gameState.remainingTime.toString().padStart(2, '0');
            timerEl.innerText = formattedTime;
            
            // Update class berdasarkan waktu tersisa
            timerEl.className = 'timer';
            
            if (gameState.remainingTime <= 5) {
                timerEl.classList.add('critical');
            } else if (gameState.remainingTime <= 10) {
                timerEl.classList.add('warning');
            }
            
            // Update status bar clock
            updateStatusBarClock();
        }

        function updateStatusBarClock() {
            const clockEl = document.getElementById('clock');
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            clockEl.innerHTML = `
                <span class="digital-time">${hours}:${minutes}<span class="digital-seconds">:${seconds}</span></span>
            `;
        }

        // Update status bar clock setiap detik
        setInterval(updateStatusBarClock, 1000);

        function loadQuestion() {
            if (gameState.isAnswering) return;
            
            clearInterval(gameState.timer);
            startTimer();

            const currentQuestion = questions[gameState.currentQuestionIndex];
            const questionEl = document.getElementById('question');
            const answerButtons = document.querySelectorAll('.answer-btn');
            
            // Reset gambar
            const imageContainer = document.querySelector('.image-container');
            const overlay = document.getElementById('overlay');
            imageContainer.style.display = 'none';
            overlay.style.display = 'none';

            // Animasi transisi pertanyaan
            questionEl.style.opacity = '0';
            setTimeout(() => {
                questionEl.innerText = currentQuestion.question;
                questionEl.style.opacity = '1';
            }, 300);

            // Setup tombol jawaban
            answerButtons.forEach((btn, index) => {
                btn.innerText = currentQuestion.answers[index];
                btn.className = 'answer-btn';
                btn.disabled = false;
                btn.style.opacity = '0';
                setTimeout(() => {
                    btn.style.opacity = '1';
                }, 300 + (index * 100));
            });
        }

        function selectAnswer(index) {
            if (gameState.isAnswering) return;
            gameState.isAnswering = true;

            const currentQuestion = questions[gameState.currentQuestionIndex];
            const answerButtons = document.querySelectorAll('.answer-btn');
            
            clearInterval(gameState.timer);

            const isCorrect = index === currentQuestion.correct;
            if (isCorrect) {
                gameState.score += config.pointsPerQuestion;
                document.getElementById('correct-sound').play();
                answerButtons[index].classList.add('correct');
                showInnerFirework();
                showFirework();
            } else {
                document.getElementById('incorrect-sound').play();
                answerButtons[index].classList.add('incorrect');
                answerButtons[currentQuestion.correct].classList.add('correct');
            }

            // Tampilkan gambar untuk setiap jawaban
            const imageEl = document.getElementById('answer-image');
            const imageContainer = document.querySelector('.image-container');
            const overlay = document.getElementById('overlay');

            // Jika ada gambar jawaban, tampilkan
            if (currentQuestion.answerImage) {
                imageEl.src = currentQuestion.answerImage;
            } else {
                // Jika tidak ada gambar jawaban, tampilkan gambar default
                imageEl.src = 'https://via.placeholder.com/400x300.png?text=Tidak+ada+gambar+tersedia';
            }

            // Tampilkan container dan overlay
            imageContainer.style.display = 'block';
            overlay.style.display = 'block';

            // Event listener untuk menutup gambar saat mengklik overlay
            overlay.onclick = function() {
                imageContainer.style.display = 'none';
                overlay.style.display = 'none';
            };

            // Sembunyikan gambar setelah beberapa detik
            setTimeout(() => {
                imageContainer.style.display = 'none';
                overlay.style.display = 'none';
            }, 3000);

            answerButtons.forEach(btn => btn.disabled = true);
            
            // Lanjut ke pertanyaan berikutnya
            setTimeout(() => {
                gameState.currentQuestionIndex++;
                gameState.isAnswering = false;
                if (gameState.currentQuestionIndex < questions.length) {
                    loadQuestion();
                } else {
                    if (gameState.score === config.pointsPerQuestion * questions.length) {
                        gameState.score += config.bonusPoints;
                        showFirework();
                    }
                    showFinalScore();
                }
            }, 3500);
        }

        function showFirework() {
            for (let i = 0; i < config.fireworkCount; i++) { // Membuat 5 titik ledakan
                const firework = document.createElement('div');
                firework.className = 'firework';
                firework.style.left = Math.random() * window.innerWidth + 'px';
                firework.style.top = Math.random() * window.innerHeight + 'px';
                document.body.appendChild(firework);
                
                // Membuat percikan untuk setiap ledakan
                for (let j = 0; j < config.sparkCount; j++) { // 30 percikan per ledakan
                    const spark = document.createElement('div');
                    spark.className = 'spark';
                    
                    // Warna acak untuk setiap percikan
                    const colors = ['#ff0', '#f0f', '#0ff', '#f00', '#0f0', '#00f'];
                    spark.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                    
                    // Arah acak untuk setiap percikan
                    const angle = (Math.random() * Math.PI * 2);
                    const velocity = 50 + Math.random() * 100;
                    spark.style.setProperty('--tx', Math.cos(angle) * velocity + 'px');
                    spark.style.setProperty('--ty', Math.sin(angle) * velocity + 'px');
                    
                    firework.appendChild(spark);
                }
                
                // Hapus elemen setelah animasi selesai
                setTimeout(() => {
                    firework.remove();
                }, 1000);
            }
        }

        function showFinalScore() {
            clearInterval(gameState.timer);
            
            const username = document.getElementById('username-input').value || 'Anonymous';
            const leaderboard = saveScore(username, gameState.score);
            
            const mainContent = document.querySelector('.main-content');
            mainContent.innerHTML = `
                <div class="final-score-container">
                    <h2>Quiz Selesai!</h2>
                    <div class="score" id="score">Skor Akhir: 0</div>
                    <div class="leaderboard">
                        <h3>Top 10 Pemain</h3>
                        <div id="leaderboard-list"></div>
                    </div>
                    <div class="button-container">
                        <button id="restart-button" style="display:none;" onclick="restartQuiz()">
                            Mulai Ulang Kuis
                        </button>
                        <button class="back-button" id="back-button" style="display:none;" onclick="goBack()">
                            Kembali
                        </button>
                    </div>
                </div>
            `;
            
            displayLeaderboard();
            animateScore(gameState.score);
        }

        function restartQuiz() {
            gameState.currentQuestionIndex = 0;
            gameState.score = 0;
            
            // Kembalikan tampilan ke kondisi awal
            const mainContent = document.querySelector('.main-content');
            mainContent.innerHTML = `
                <h2>LaHezQuiz</h2>
                <div class="timer" id="timer">15</div>
                <div id="question-container">
                    <p class="question" id="question"></p>
                    <div class="answers-grid">
                        <button class="answer-btn" onclick="selectAnswer(0)"></button>
                        <button class="answer-btn" onclick="selectAnswer(1)"></button>
                        <button class="answer-btn" onclick="selectAnswer(2)"></button>
                        <button class="answer-btn" onclick="selectAnswer(3)"></button>
                    </div>
                </div>
                <div class="score" id="score">Skor: 0</div>
            `;
            
            loadQuestion();
        }

        function showInnerFirework() {
            const container = document.querySelector('.quiz-container');
            const centerX = container.offsetWidth / 2;
            const centerY = container.offsetHeight / 2;
            
            // Membuat 3 titik ledakan
            for (let i = 0; i < 3; i++) {
                const firework = document.createElement('div');
                firework.className = 'inner-firework';
                firework.style.left = centerX + 'px';
                firework.style.top = centerY + 'px';
                container.appendChild(firework);
                
                // Membuat 20 percikan per ledakan
                for (let j = 0; j < 20; j++) {
                    const spark = document.createElement('div');
                    spark.className = 'inner-spark';
                    
                    // Warna-warna cerah untuk percikan
                    const colors = ['#ffff00', '#ff00ff', '#00ffff', '#ff0000', '#00ff00', '#0000ff'];
                    spark.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                    
                    // Arah acak dalam radius yang lebih kecil
                    const angle = (Math.random() * Math.PI * 2);
                    const velocity = 30 + Math.random() * 50; // Jarak lebih pendek
                    spark.style.setProperty('--tx', Math.cos(angle) * velocity + 'px');
                    spark.style.setProperty('--ty', Math.sin(angle) * velocity + 'px');
                    
                    firework.appendChild(spark);
                }
                
                // Hapus elemen setelah animasi selesai
                setTimeout(() => {
                    firework.remove();
                }, 1000);
            }
        }

        function startQuiz() {
            const username = document.getElementById('username-input').value.trim();
            
            if (!username) {
                alert('Silakan masukkan username Anda!');
                return;
            }
            
            // Animasi fade out untuk welcome screen
            const welcomeScreen = document.getElementById('welcome-screen');
            welcomeScreen.style.opacity = '0';
            welcomeScreen.style.transition = 'opacity 0.5s ease';
            
            setTimeout(() => {
                welcomeScreen.style.display = 'none';
                // Tampilkan konten kuis
                const quizContent = document.getElementById('quiz-content');
                quizContent.style.display = 'block';
                quizContent.style.opacity = '0';
                
                // Animasi fade in untuk konten kuis
                setTimeout(() => {
                    quizContent.style.opacity = '1';
                    quizContent.style.transition = 'opacity 0.5s ease';
                    // Mulai kuis
                    loadQuestion();
                }, 50);
            }, 500);
        }

        // Inisialisasi
        loadQuestion();

        // Add this to your existing JavaScript
        function updateClock() {
            const clockElement = document.getElementById('clock');
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            clockElement.textContent = `${hours}:${minutes}`;
        }

        // Update clock every minute
        setInterval(updateClock, 60000);
        updateClock(); // Initial call

        // Tambahkan fungsi animasi skor
        function animateScore(finalScore) {
            const scoreElement = document.getElementById('score');
            let currentScore = 0;
            const duration = 2000; // Durasi animasi dalam milidetik
            const steps = 50; // Jumlah langkah animasi
            const increment = finalScore / steps;
            const stepDuration = duration / steps;

            const animation = setInterval(() => {
                currentScore += increment;
                if (currentScore >= finalScore) {
                    currentScore = finalScore;
                    clearInterval(animation);
                    // Tampilkan kedua tombol setelah animasi selesai
                    setTimeout(() => {
                        document.getElementById('restart-button').style.display = 'inline-block';
                        document.getElementById('back-button').style.display = 'inline-block';
                    }, 500);
                }
                scoreElement.innerText = `Skor Akhir: ${Math.round(currentScore)}`;
            }, stepDuration);
        }

        // Tambahkan fungsi goBack
        function goBack() {
            window.location.href = 'index.html'; // Atau URL halaman utama Anda
        }

        // Tambahkan fungsi untuk menyimpan skor
        function saveScore(username, score) {
            let leaderboard = JSON.parse(localStorage.getItem('quizLeaderboard')) || [];
            
            // Tambahkan skor baru
            leaderboard.push({
                username: username,
                score: score,
                timestamp: new Date().toISOString()
            });
            
            // Urutkan berdasarkan skor tertinggi
            leaderboard.sort((a, b) => b.score - a.score);
            
            // Batasi hanya 10 skor tertinggi
            leaderboard = leaderboard.slice(0, 10);
            
            // Simpan ke localStorage
            localStorage.setItem('quizLeaderboard', JSON.stringify(leaderboard));
            
            return leaderboard;
        }

        // Tambahkan fungsi untuk menampilkan leaderboard
        function displayLeaderboard() {
            const leaderboard = JSON.parse(localStorage.getItem('quizLeaderboard')) || [];
            const leaderboardList = document.getElementById('leaderboard-list');
            
            leaderboardList.innerHTML = leaderboard.map((entry, index) => `
                <div class="leaderboard-item">
                    <span class="rank">#${index + 1}</span>
                    <span class="username">${entry.username}</span>
                    <span class="score">${entry.score}</span>
                    <span class="time">${new Date(entry.timestamp).toLocaleDateString()}</span>
                </div>
            `).join('');
        }
    </script>
</body>
</html>
