## CODE 1 :

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QuizMaster - Test Your Knowledge</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 800px;
            padding: 40px;
            transition: all 0.3s ease;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .logo {
            background: linear-gradient(45deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-size: 2.5rem;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .subtitle {
            color: #666;
            font-size: 1.1rem;
        }

        .quiz-section {
            display: none;
        }

        .quiz-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .question-counter {
            text-align: center;
            margin-bottom: 20px;
            color: #666;
            font-size: 0.9rem;
        }

        .progress-bar {
            height: 6px;
            background: #e0e0e0;
            border-radius: 3px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(45deg, #667eea, #764ba2);
            width: 0%;
            transition: width 0.3s ease;
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border-left: 5px solid #667eea;
        }

        .question h3 {
            color: #333;
            font-size: 1.3rem;
            line-height: 1.5;
            margin-bottom: 20px;
        }

        .options {
            display: grid;
            gap: 12px;
        }

        .option {
            background: white;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            padding: 15px 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .option:hover {
            border-color: #667eea;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.2);
        }

        .option.selected {
            border-color: #667eea;
            background: linear-gradient(45deg, rgba(102, 126, 234, 0.1), rgba(118, 75, 162, 0.1));
        }

        .option.correct {
            border-color: #4CAF50;
            background: linear-gradient(45deg, rgba(76, 175, 80, 0.1), rgba(76, 175, 80, 0.2));
            animation: correctPulse 0.6s ease;
        }

        .option.incorrect {
            border-color: #f44336;
            background: linear-gradient(45deg, rgba(244, 67, 54, 0.1), rgba(244, 67, 54, 0.2));
            animation: incorrectShake 0.6s ease;
        }

        @keyframes correctPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.02); }
        }

        @keyframes incorrectShake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }

        .feedback {
            margin: 20px 0;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            font-weight: bold;
            display: none;
        }

        .feedback.correct {
            background: rgba(76, 175, 80, 0.1);
            color: #4CAF50;
            border: 2px solid #4CAF50;
        }

        .feedback.incorrect {
            background: rgba(244, 67, 54, 0.1);
            color: #f44336;
            border: 2px solid #f44336;
        }

        .buttons {
            text-align: center;
            margin-top: 30px;
        }

        .btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 0 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .start-screen {
            text-align: center;
        }

        .start-screen h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 1.8rem;
        }

        .start-screen p {
            color: #666;
            margin-bottom: 30px;
            font-size: 1.1rem;
            line-height: 1.6;
        }

        .results-screen {
            text-align: center;
        }

        .score-circle {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            margin: 30px auto;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            font-size: 2rem;
            font-weight: bold;
            color: white;
            position: relative;
            overflow: hidden;
        }

        .score-circle.excellent {
            background: linear-gradient(45deg, #4CAF50, #8BC34A);
        }

        .score-circle.good {
            background: linear-gradient(45deg, #FF9800, #FFC107);
        }

        .score-circle.average {
            background: linear-gradient(45deg, #2196F3, #03A9F4);
        }

        .score-circle.poor {
            background: linear-gradient(45deg, #f44336, #FF5722);
        }

        .score-text {
            font-size: 0.8rem;
            margin-top: 5px;
        }

        .results-message {
            color: #333;
            font-size: 1.3rem;
            margin: 20px 0;
        }

        .results-breakdown {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 25px;
            margin: 20px 0;
        }

        .breakdown-item {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
            font-size: 1.1rem;
        }

        .breakdown-item strong {
            color: #333;
        }

        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            .logo {
                font-size: 2rem;
            }
            
            .option {
                padding: 12px 15px;
            }
            
            .score-circle {
                width: 150px;
                height: 150px;
                font-size: 1.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="logo">🧠 QuizMaster</div>
            <div class="subtitle">Test Your Knowledge</div>
        </div>

        <!-- Start Screen -->
        <div id="start-screen" class="quiz-section active start-screen">
            <h2>Welcome to QuizMaster!</h2>
            <p>Test your knowledge with our interactive quiz. You'll get immediate feedback on each question and see your final score at the end.</p>
            <div class="buttons">
                <button class="btn" onclick="startQuiz()">Start Quiz</button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="quiz-section">
            <div class="question-counter">
                <span id="current-question">1</span> of <span id="total-questions">10</span>
            </div>
            
            <div class="progress-bar">
                <div class="progress-fill" id="progress-fill"></div>
            </div>

            <div class="question">
                <h3 id="question-text"></h3>
                <div class="options" id="options-container">
                </div>
            </div>

            <div class="feedback" id="feedback"></div>

            <div class="buttons">
                <button class="btn" id="next-btn" onclick="nextQuestion()" disabled>Next Question</button>
            </div>
        </div>

        <!-- Results Screen -->
        <div id="results-screen" class="quiz-section results-screen">
            <h2>Quiz Complete!</h2>
            
            <div class="score-circle" id="score-circle">
                <div id="score-percentage">0%</div>
                <div class="score-text">Score</div>
            </div>

            <div class="results-message" id="results-message"></div>

            <div class="results-breakdown">
                <div class="breakdown-item">
                    <span>Correct Answers:</span>
                    <strong id="correct-count">0</strong>
                </div>
                <div class="breakdown-item">
                    <span>Incorrect Answers:</span>
                    <strong id="incorrect-count">0</strong>
                </div>
                <div class="breakdown-item">
                    <span>Total Questions:</span>
                    <strong id="total-count">0</strong>
                </div>
            </div>

            <div class="buttons">
                <button class="btn" onclick="restartQuiz()">Take Quiz Again</button>
            </div>
        </div>
    </div>

    <script>
        // Quiz data
        const quizData = [
            {
                question: "What is the capital of India?",
                options: ["Mumbai", "New Delhi", "Kolkata", "Chennai"],
                correct: 1
            },
            {
                question: "Which planet is known as the Red Planet?",
                options: ["Venus", "Mars", "Jupiter", "Saturn"],
                correct: 1
            },
            {
                question: "What is the largest mammal in the world?",
                options: ["Elephant", "Blue Whale", "Giraffe", "Hippopotamus"],
                correct: 1
            },
            {
                question: "Who painted the Mona Lisa?",
                options: ["Vincent van Gogh", "Pablo Picasso", "Leonardo da Vinci", "Michelangelo"],
                correct: 2
            },
            {
                question: "What is the chemical symbol for gold?",
                options: ["Go", "Gd", "Au", "Ag"],
                correct: 2
            },
            {
                question: "Which is the smallest country in the world?",
                options: ["Monaco", "Vatican City", "San Marino", "Liechtenstein"],
                correct: 1
            },
            {
                question: "What is the fastest land animal?",
                options: ["Lion", "Cheetah", "Leopard", "Tiger"],
                correct: 1
            },
            {
                question: "Which ocean is the largest?",
                options: ["Atlantic Ocean", "Indian Ocean", "Arctic Ocean", "Pacific Ocean"],
                correct: 3
            },
            {
                question: "What is the hardest natural substance on Earth?",
                options: ["Gold", "Iron", "Diamond", "Platinum"],
                correct: 2
            },
            {
                question: "How many continents are there?",
                options: ["5", "6", "7", "8"],
                correct: 2
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let selectedOption = -1;
        let quizCompleted = false;

        function startQuiz() {
            document.getElementById('start-screen').classList.remove('active');
            document.getElementById('quiz-screen').classList.add('active');
            document.getElementById('total-questions').textContent = quizData.length;
            loadQuestion();
        }

        function loadQuestion() {
            if (currentQuestion >= quizData.length) {
                showResults();
                return;
            }

            const question = quizData[currentQuestion];
            document.getElementById('current-question').textContent = currentQuestion + 1;
            document.getElementById('question-text').textContent = question.question;
            
            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.onclick = () => selectOption(index);
                optionsContainer.appendChild(optionElement);
            });

            // Update progress bar
            const progress = ((currentQuestion) / quizData.length) * 100;
            document.getElementById('progress-fill').style.width = progress + '%';

            // Reset UI
            selectedOption = -1;
            document.getElementById('next-btn').disabled = true;
            document.getElementById('feedback').style.display = 'none';
            document.getElementById('feedback').className = 'feedback';
        }

        function selectOption(index) {
            if (selectedOption !== -1) return; // Prevent multiple selections

            selectedOption = index;
            const options = document.querySelectorAll('.option');
            const question = quizData[currentQuestion];
            
            // Show correct/incorrect feedback
            options.forEach((option, i) => {
                if (i === question.correct) {
                    option.classList.add('correct');
                } else if (i === selectedOption && i !== question.correct) {
                    option.classList.add('incorrect');
                }
                option.style.pointerEvents = 'none';
            });

            // Update score and show feedback
            const feedback = document.getElementById('feedback');
            if (selectedOption === question.correct) {
                score++;
                feedback.textContent = '🎉 Correct! Well done!';
                feedback.className = 'feedback correct';
            } else {
                feedback.textContent = `❌ Incorrect. The correct answer is: ${question.options[question.correct]}`;
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            document.getElementById('next-btn').disabled = false;
        }

        function nextQuestion() {
            currentQuestion++;
            setTimeout(loadQuestion, 300); // Small delay for smooth transition
        }

        function showResults() {
            document.getElementById('quiz-screen').classList.remove('active');
            document.getElementById('results-screen').classList.add('active');
            
            const percentage = Math.round((score / quizData.length) * 100);
            const scoreCircle = document.getElementById('score-circle');
            
            document.getElementById('score-percentage').textContent = percentage + '%';
            document.getElementById('correct-count').textContent = score;
            document.getElementById('incorrect-count').textContent = quizData.length - score;
            document.getElementById('total-count').textContent = quizData.length;
            
            // Set score circle color and message based on performance
            let message = '';
            if (percentage >= 80) {
                scoreCircle.className = 'score-circle excellent';
                message = '🏆 Excellent! You\'re a quiz master!';
            } else if (percentage >= 60) {
                scoreCircle.className = 'score-circle good';
                message = '👏 Good job! You did well!';
            } else if (percentage >= 40) {
                scoreCircle.className = 'score-circle average';
                message = '👍 Not bad! Keep practicing!';
            } else {
                scoreCircle.className = 'score-circle poor';
                message = '💪 Don\'t give up! Try again!';
            }
            
            document.getElementById('results-message').textContent = message;
        }

        function restartQuiz() {
            currentQuestion = 0;
            score = 0;
            selectedOption = -1;
            document.getElementById('results-screen').classList.remove('active');
            document.getElementById('start-screen').classList.add('active');
        }
    </script>
</body>
</html>

