# typingtest
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Typing Test</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.0.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #0099cc;
            --secondary-color: #e6f7ff;
            --text-color: #333;
            --bg-color: #f0f8ff;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            transition: all 0.3s ease;
        }
        
        body.dark-mode {
            --bg-color: #1a1a2e;
            --text-color: #e6e6e6;
            --secondary-color: #16213e;
        }
        
        .logo {
            width: 50px;
            height: 50px;
            background-color: var(--primary-color);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-weight: bold;
            font-size: 24px;
        }
        
        .typing-box {
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 15px;
            background-color: white;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            min-height: 150px;
            position: relative;
        }
        
        .dark-mode .typing-box {
            background-color: #2a2a3a;
            border-color: #444;
        }
        
        .char {
            display: inline-block;
            font-family: monospace;
            font-size: 18px;
        }
        
        .correct {
            color: #28a745;
        }
        
        .incorrect {
            color: #dc3545;
            text-decoration: underline;
        }
        
        .current {
            background-color: rgba(0, 153, 204, 0.2);
            border-left: 2px solid var(--primary-color);
        }
        
        .typing-input {
            position: absolute;
            opacity: 0;
            pointer-events: none;
        }
        
        .start-button {
            background-color: var(--primary-color);
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            border: none;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        
        .start-button:hover {
            background-color: #007799;
        }
        
        .switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 24px;
        }
        
        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 34px;
        }
        
        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .slider {
            background-color: var(--primary-color);
        }
        
        input:checked + .slider:before {
            transform: translateX(26px);
        }
        
        .card {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            transition: transform 0.3s ease;
        }
        
        .dark-mode .card {
            background-color: #2a2a3a;
        }
        
        .card:hover {
            transform: translateY(-5px);
        }
        
        .results-container {
            display: none;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body class="min-h-screen">
    <header class="container mx-auto p-4">
        <div class="flex items-center">
            <div class="logo mr-2">T</div>
            <div class="text-xl font-bold text-primary">TypingTest.com</div>
            <div class="ml-auto flex items-center">
                <label class="switch mr-2">
                    <input type="checkbox" id="darkModeToggle">
                    <span class="slider"></span>
                </label>
                <span class="text-sm">Dark Mode</span>
            </div>
        </div>
    </header>

    <main class="container mx-auto p-4">
        <div class="max-w-3xl mx-auto bg-white rounded-lg shadow-lg p-6 mb-8 dark:bg-gray-800">
            <h1 class="text-3xl font-bold text-center text-primary mb-2">Check your typing skills in a minute</h1>
            <p class="text-center text-gray-600 dark:text-gray-300 mb-6">Type away to join millions of test takers!</p>

            <div class="flex flex-col md:flex-row gap-6 mb-6">
                <div class="w-full md:w-1/2">
                    <label class="block text-gray-700 dark:text-gray-300 mb-2">Select Your Test</label>
                    <select id="testDuration" class="w-full p-2 border rounded bg-white dark:bg-gray-700 dark:text-white">
                        <option value="1">1 Minute Test</option>
                        <option value="2">2 Minute Test</option>
                        <option value="3">3 Minute Test</option>
                        <option value="5">5 Minute Test</option>
                    </select>
                </div>
                <div class="w-full md:w-1/2">
                    <label class="block text-gray-700 dark:text-gray-300 mb-2">Select Difficulty</label>
                    <select id="textDifficulty" class="w-full p-2 border rounded bg-white dark:bg-gray-700 dark:text-white">
                        <option value="easy">Easy Text</option>
                        <option value="medium" selected>Medium Text</option>
                        <option value="hard">Hard Text</option>
                    </select>
                </div>
            </div>

            <div class="typing-box bg-white dark:bg-gray-700 rounded mb-4 relative" id="typingContainer">
                <div id="typingText" class="text-gray-800 dark:text-gray-200"></div>
                <input type="text" class="typing-input" id="typingInput" autocomplete="off">
            </div>

            <div class="flex items-center justify-between">
                <div>
                    <span id="timer" class="text-2xl font-bold">1:00</span>
                </div>
                <button id="startTest" class="start-button">START TEST</button>
            </div>

            <div id="resultsContainer" class="results-container mt-6 p-4 border rounded-lg bg-gray-50 dark:bg-gray-900">
                <h2 class="text-2xl font-bold mb-4">Your Results</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="text-center">
                        <div class="text-3xl font-bold" id="wpmResult">0</div>
                        <div class="text-sm text-gray-600 dark:text-gray-400">Words Per Minute</div>
                    </div>
                    <div class="text-center">
                        <div class="text-3xl font-bold" id="accuracyResult">0%</div>
                        <div class="text-sm text-gray-600 dark:text-gray-400">Accuracy</div>
                    </div>
                    <div class="text-center">
                        <div class="text-3xl font-bold" id="errorResult">0</div>
                        <div class="text-sm text-gray-600 dark:text-gray-400">Errors</div>
                    </div>
                </div>
                <div class="mt-6 text-center">
                    <button id="retryButton" class="start-button">Try Again</button>
                </div>
            </div>
        </div>

        <div class="my-12">
            <h2 class="text-2xl font-bold text-center mb-6">Take The Next Step</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="card">
                    <div class="p-6">
                        <h3 class="text-xl font-bold mb-2">Train Typing Skills</h3>
                        <p class="text-gray-600 dark:text-gray-300 mb-4">What if you could type as fluently as you speak? Typing lessons will get you there!</p>
                        <a href="#" class="text-primary hover:underline">Learn more</a>
                    </div>
                </div>
                <div class="card">
                    <div class="p-6">
                        <h3 class="text-xl font-bold mb-2">Typing Challenge</h3>
                        <p class="text-gray-600 dark:text-gray-300 mb-4">Can you name the progressively longer words? Test your typing skills!</p>
                        <a href="#" class="text-primary hover:underline">Start challenge</a>
                    </div>
                </div>
                <div class="card">
                    <div class="p-6">
                        <h3 class="text-xl font-bold mb-2">Type 1-100</h3>
                        <p class="text-gray-600 dark:text-gray-300 mb-4">Can you type the numbers 1-100 in under 1 minute? Try this speed challenge!</p>
                        <a href="#" class="text-primary hover:underline">Start challenge</a>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <footer class="bg-gray-100 dark:bg-gray-900 py-6">
        <div class="container mx-auto px-4">
            <div class="flex flex-col md:flex-row justify-center items-center">
                <div class="flex items-center mb-4 md:mb-0">
                    <div class="logo mr-2 text-sm">T</div>
                    <div class="text-sm font-bold">TypingTest.com</div>
                </div>
                <div class="md:ml-auto">
                    <ul class="flex flex-wrap justify-center">
                        <li class="mx-2"><a href="#" class="text-sm hover:underline">Terms & Privacy</a></li>
                        <li class="mx-2"><a href="#" class="text-sm hover:underline">About</a></li>
                        <li class="mx-2"><a href="#" class="text-sm hover:underline">Blog</a></li>
                        <li class="mx-2"><a href="#" class="text-sm hover:underline">Contact</a></li>
                    </ul>
                </div>
            </div>
            <div class="text-center text-sm mt-4 text-gray-600 dark:text-gray-400">
                &copy; 2023 Typing Test - All Rights Reserved
            </div>
        </div>
    </footer>

    <script>
        // Sample text options
        const texts = {
            easy: [
                "The quick brown fox jumps over the lazy dog. This pangram contains every letter of the alphabet. It is often used to test typing skills.",
                "She sells seashells by the seashore. The shells she sells are seashells, I'm sure. So if she sells seashells on the seashore, I'm sure she sells seashore shells."
            ],
            medium: [
                "We know sharks are dangerous but do you know that there is a more dangerous animal we can find on a beach? It's the mosquito. Mosquitoes kill more humans than sharks do each year.",
                "Learning to type is an essential skill in today's digital world. The faster you can type, the more productive you can be at work or school. Practice daily to improve your speed."
            ],
            hard: [
                "The exploration of quantum physics reveals fascinating paradoxes that challenge our understanding of reality. Schrödinger's cat exemplifies quantum superposition where particles exist in multiple states simultaneously.",
                "Pneumonoultramicroscopicsilicovolcanoconiosis, a factitious word created to represent a lung disease, is among the longest words in English dictionaries, although it's rarely used in medical practice."
            ]
        };

        // DOM elements
        const typingContainer = document.getElementById('typingContainer');
        const typingText = document.getElementById('typingText');
        const typingInput = document.getElementById('typingInput');
        const timer = document.getElementById('timer');
        const startButton = document.getElementById('startTest');
        const resultsContainer = document.getElementById('resultsContainer');
        const wpmResult = document.getElementById('wpmResult');
        const accuracyResult = document.getElementById('accuracyResult');
        const errorResult = document.getElementById('errorResult');
        const retryButton = document.getElementById('retryButton');
        const testDuration = document.getElementById('testDuration');
        const textDifficulty = document.getElementById('textDifficulty');
        const darkModeToggle = document.getElementById('darkModeToggle');

        // Variables
        let timeLeft = 60;
        let timer_interval = null;
        let charIndex = 0;
        let mistakes = 0;
        let isTyping = false;
        let currentText = '';

        // Initialize text
        function loadText() {
            const difficulty = textDifficulty.value;
            const randomIndex = Math.floor(Math.random() * texts[difficulty].length);
            currentText = texts[difficulty][randomIndex];
            
            typingText.innerHTML = '';
            currentText.split('').forEach(char => {
                const span = document.createElement('span');
                span.className = 'char';
                span.textContent = char;
                typingText.appendChild(span);
            });
            
            typingText.querySelectorAll('.char')[0].classList.add('current');
            document.addEventListener('keydown', () => typingInput.focus());
        }

        // Update timer
        function updateTimer() {
            if (timeLeft > 0) {
                timeLeft--;
                const minutes = Math.floor(timeLeft / 60);
                const seconds = timeLeft % 60;
                timer.textContent = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
            } else {
                clearInterval(timer_interval);
                finishTyping();
            }
        }

        // Start typing test
        function startTyping() {
            resetTest();
            typingInput.value = '';
            loadText();
            
            timeLeft = parseInt(testDuration.value) * 60;
            updateTimer();
            
            timer_interval = setInterval(updateTimer, 1000);
            isTyping = true;
            typingInput.focus();
            
            startButton.style.display = 'none';
            testDuration.disabled = true;
            textDifficulty.disabled = true;
        }

        // Process typing
        function processTyping() {
            const inputChars = typingInput.value.split('');
            
            charIndex = inputChars.length - 1;
            const chars = typingText.querySelectorAll('.char');
            
            // Reset all classes
            chars.forEach(char => {
                char.classList.remove('correct', 'incorrect', 'current');
            });
            
            // Check each character
            let correctCount = 0;
            inputChars.forEach((char, index) => {
                if (index < chars.length) {
                    if (char === chars[index].textContent) {
                        chars[index].classList.add('correct');
                        correctCount++;
                    } else {
                        chars[index].classList.add('incorrect');
                    }
                }
            });
            
            mistakes = inputChars.length - correctCount;
            
            // Set current character
            if (charIndex < chars.length - 1) {
                chars[inputChars.length].classList.add('current');
            }
            
            // If typing is complete
            if (charIndex === currentText.length - 1) {
                clearInterval(timer_interval);
                finishTyping();
            }
        }

        // Calculate typing metrics
        function calculateMetrics() {
            const timeSpent = parseInt(testDuration.value) * 60 - timeLeft;
            const timeSpentMinutes = timeSpent / 60;
            
            // Calculate WPM: (characters typed / 5) / time in minutes
            const characterTyped = typingInput.value.length;
            const wpm = Math.round((characterTyped / 5) / timeSpentMinutes);
            
            // Calculate accuracy
            const correctChars = characterTyped - mistakes;
            const accuracy = Math.round((correctChars / characterTyped) * 100);
            
            return {
                wpm: isNaN(wpm) ? 0 : wpm,
                accuracy: isNaN(accuracy) ? 0 : accuracy,
                errors: mistakes
            };
        }

        // Finish typing test
        function finishTyping() {
            isTyping = false;
            const metrics = calculateMetrics();
            
            wpmResult.textContent = metrics.wpm;
            accuracyResult.textContent = `${metrics.accuracy}%`;
            errorResult.textContent = metrics.errors;
            
            resultsContainer.style.display = 'block';
            startButton.style.display = 'none';
        }

        // Reset test
        function resetTest() {
            clearInterval(timer_interval);
            timeLeft = parseInt(testDuration.value) * 60;
            timer.textContent = `${testDuration.value}:00`;
            charIndex = 0;
            mistakes = 0;
            isTyping = false;
            typingInput.value = '';
            typingText.innerHTML = '';
            resultsContainer.style.display = 'none';
            startButton.style.display = 'block';
            testDuration.disabled = false;
            textDifficulty.disabled = false;
        }

        // Event listeners
        startButton.addEventListener('click', startTyping);
        retryButton.addEventListener('click', resetTest);
        typingInput.addEventListener('input', processTyping);
        
        // Dark mode toggle
        darkModeToggle.addEventListener('change', () => {
            document.body.classList.toggle('dark-mode');
        });

        // Check for saved dark mode preference
        if (localStorage.getItem('darkMode') === 'true') {
            darkModeToggle.checked = true;
            document.body.classList.add('dark-mode');
        }

        // Save dark mode preference
        darkModeToggle.addEventListener('change', () => {
            localStorage.setItem('darkMode', darkModeToggle.checked);
        });

        // Initialize
        window.onload = () => {
            loadText();
            timer.textContent = `${testDuration.value}:00`;
            typingInput.focus();
        };
    </script>
</body>
</html>
