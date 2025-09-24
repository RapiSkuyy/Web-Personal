<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teka Teki Mahasiswa</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Poppins:wght@600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            touch-action: manipulation;
        }
        .title-font {
            font-family: 'Poppins', sans-serif;
        }
        .card-enter {
            animation: card-enter 0.5s ease-out;
        }
        .feedback-pop {
            animation: feedback-pop 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        @keyframes card-enter {
            from {
                opacity: 0;
                transform: translateY(20px) scale(0.95);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }
        @keyframes feedback-pop {
            from {
                opacity: 0;
                transform: scale(0.8);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }
    </style>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-200 flex items-center justify-center min-h-screen p-4">

    <div id="game-container" class="w-full max-w-md mx-auto">
        <div id="game-card" class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl p-6 sm:p-8 transition-all duration-300 card-enter">
            <!-- Header -->
            <div class="text-center mb-6">
                <h1 class="title-font text-3xl font-bold text-indigo-600 dark:text-indigo-400">Teka Teki Mahasiswa</h1>
                <p id="level-indicator" class="mt-2 text-lg font-semibold text-gray-500 dark:text-gray-400">Level 1 dari 10</p>
            </div>

            <!-- Riddle Question -->
            <div class="bg-gray-100 dark:bg-gray-700 rounded-lg p-4 mb-6 min-h-[100px] flex items-center justify-center">
                <p id="question-text" class="text-center text-lg leading-relaxed"></p>
            </div>

            <!-- Input and Actions -->
            <div id="input-area">
                <label for="answer-input" class="font-semibold mb-2 block text-gray-600 dark:text-gray-300">Jawabanmu:</label>
                <input type="text" id="answer-input" class="w-full px-4 py-3 bg-gray-50 dark:bg-gray-600 border-2 border-gray-300 dark:border-gray-500 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 transition duration-200" placeholder="Ketik jawaban di sini...">
                <button id="submit-button" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-4 rounded-lg mt-4 transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                    Cek Jawaban
                </button>
            </div>
            
            <!-- Feedback Message -->
            <div id="feedback-message" class="text-center mt-4 font-semibold text-xl"></div>

            <!-- Next Level Button -->
            <button id="next-level-button" class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-4 rounded-lg mt-4 transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 hidden">
                Lanjut ke Level Berikutnya
            </button>
        </div>

        <!-- Completion Screen -->
        <div id="completion-screen" class="text-center p-8 bg-white dark:bg-gray-800 rounded-2xl shadow-xl hidden card-enter">
            <h2 class="title-font text-3xl font-bold text-green-500 dark:text-green-400">Selamat!</h2>
            <p class="mt-4 text-xl">Kamu berhasil menyelesaikan semua teka-teki!</p>
            <p class="mt-2 text-gray-500 dark:text-gray-400">Jiwa mahasiswamu terbukti tangguh!</p>
            <button id="restart-button" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-6 rounded-lg mt-8 transition-transform transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                Main Lagi
            </button>
        </div>
    </div>

    <script>
        const riddles = [
            {
                level: 1,
                question: "Aku punya banyak SKS, tapi bukan kartu kredit. Aku selalu dikejar *deadline*. Siapakah aku?",
                answer: "mahasiswa"
            },
            {
                level: 2,
                question: "Aku adalah tempat mencari ilmu, penuh dengan buku dan keheningan. Di mana aku?",
                answer: "perpustakaan"
            },
            {
                level: 3,
                question: "Aku adalah selembar kertas sakti yang dinanti di akhir semester. Isiku bisa membuatmu senang atau sedih. Apakah aku?",
                answer: "khs" // Kartu Hasil Studi
            },
            {
                level: 4,
                question: "Pagi jadi teman, malam jadi lawan. Aku adalah penyelamat saat kantuk menyerang di kelas. Apa aku?",
                answer: "kopi"
            },
            {
                level: 5,
                question: "Aku adalah tugas akhir yang paling ditakuti, butuh berbulan-bulan untuk menyelesaikanku. Siapakah aku?",
                answer: "skripsi"
            },
            {
                level: 6,
                question: "Benda apa yang kalau datang membuat semua orang senang, tapi seringnya datang di tanggal tua?",
                answer: "gaji"
            },
            {
                level: 7,
                question: "Aku adalah orang yang selalu benar di kelas dan memegang kendali nilai. Siapakah aku?",
                answer: "dosen"
            },
            {
                level: 8,
                question: "Aku adalah makanan andalan di akhir bulan, murah meriah dan banyak rasa. Siapakah aku?",
                answer: "mie instan"
            },
            {
                level: 9,
                question: "Kegiatan tak terlihat, namun kehadirannya tercatat. Apakah itu?",
                answer: "titip absen"
            },
            {
                level: 10,
                question: "Aku adalah gelar yang diperjuangkan mati-matian selama bertahun-tahun. Apakah aku?",
                answer: "sarjana"
            }
        ];

        let currentLevel = 0;

        // Element Selectors
        const levelIndicator = document.getElementById('level-indicator');
        const questionText = document.getElementById('question-text');
        const answerInput = document.getElementById('answer-input');
        const submitButton = document.getElementById('submit-button');
        const nextLevelButton = document.getElementById('next-level-button');
        const feedbackMessage = document.getElementById('feedback-message');
        const inputArea = document.getElementById('input-area');
        const gameCard = document.getElementById('game-card');
        const completionScreen = document.getElementById('completion-screen');
        const restartButton = document.getElementById('restart-button');

        function loadRiddle() {
            if (currentLevel < riddles.length) {
                const riddle = riddles[currentLevel];
                levelIndicator.textContent = `Level ${riddle.level} dari ${riddles.length}`;
                questionText.innerHTML = riddle.question;
                resetUI();
            } else {
                showCompletionScreen();
            }
        }

        function resetUI() {
            answerInput.value = '';
            feedbackMessage.textContent = '';
            feedbackMessage.className = 'text-center mt-4 font-semibold text-xl'; // Reset class
            inputArea.style.display = 'block';
            nextLevelButton.classList.add('hidden');
            answerInput.focus();
        }

        function checkAnswer() {
            const userAnswer = answerInput.value.trim().toLowerCase();
            const correctAnswer = riddles[currentLevel].answer.toLowerCase();

            if (userAnswer === correctAnswer) {
                feedbackMessage.textContent = 'Benar! Jawabanmu Tepat!';
                feedbackMessage.className = 'text-center mt-4 font-semibold text-xl text-green-500 dark:text-green-400 feedback-pop';
                inputArea.style.display = 'none';
                if (currentLevel < riddles.length - 1) {
                    nextLevelButton.classList.remove('hidden');
                } else {
                    setTimeout(showCompletionScreen, 1000);
                }
            } else {
                feedbackMessage.textContent = 'Salah! Coba lagi ya.';
                feedbackMessage.className = 'text-center mt-4 font-semibold text-xl text-red-500 dark:text-red-400 feedback-pop';
                answerInput.value = '';
                answerInput.focus();
                // Add a little shake for wrong answer
                gameCard.classList.add('animate-shake');
                setTimeout(() => gameCard.classList.remove('animate-shake'), 500);
            }
        }
        
        // Add a simple shake animation in a style tag for feedback
        const style = document.createElement('style');
        style.innerHTML = `
            @keyframes shake {
              10%, 90% { transform: translate3d(-1px, 0, 0); }
              20%, 80% { transform: translate3d(2px, 0, 0); }
              30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
              40%, 60% { transform: translate3d(4px, 0, 0); }
            }
            .animate-shake {
                animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
            }
        `;
        document.head.appendChild(style);


        function goToNextLevel() {
            currentLevel++;
            loadRiddle();
        }

        function showCompletionScreen() {
            gameCard.classList.add('hidden');
            completionScreen.classList.remove('hidden');
        }

        function restartGame() {
            currentLevel = 0;
            completionScreen.classList.add('hidden');
            gameCard.classList.remove('hidden');
            loadRiddle();
        }

        // Event Listeners
        submitButton.addEventListener('click', checkAnswer);
        answerInput.addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                checkAnswer();
            }
        });
        nextLevelButton.addEventListener('click', goToNextLevel);
        restartButton.addEventListener('click', restartGame);

        // Initial Load
        loadRiddle();
    </script>
</body>
</html>
