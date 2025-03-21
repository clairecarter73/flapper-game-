<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Flapper Life Game</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background-color: #f7e3c3; }
        h1 { color: #8b0000; }
        .container { width: 50%; margin: auto; padding: 20px; background: white; border-radius: 10px; box-shadow: 0 0 10px gray; }
        .question { font-size: 18px; margin-bottom: 15px; }
        .btn { padding: 10px; margin: 5px; background-color: #8b0000; color: white; border: none; cursor: pointer; border-radius: 5px; }
        .btn:hover { background-color: #a52a2a; }
        #result { font-size: 20px; font-weight: bold; margin-top: 20px; }
    </style>
</head>
<body>

    <div class="container">
        <h1>The Flapper Life Game</h1>
        <p>Make choices as a young woman in the 1920s and see where you end up!</p>
        <button class="btn" onclick="startGame()">Start Game</button>
    </div>

    <div class="container" id="game" style="display:none;">
        <h2>Question <span id="question-number">1</span></h2>
        <p class="question" id="question-text">Loading...</p>
        <button class="btn" onclick="selectOption(0)">Option A</button>
        <button class="btn" onclick="selectOption(1)">Option B</button>
        <button class="btn" onclick="selectOption(2)">Option C</button>
    </div>

    <div class="container" id="result-page" style="display:none;">
        <h2>Your Results</h2>
        <p id="result">Calculating...</p>
        <button class="btn" onclick="restartGame()">Play Again</button>
    </div>

    <script>
        let questions = [
            { text: "You’re invited to a party. What do you wear?", options: [
                { text: "A flapper dress", independence: 5, reputation: -2 },
                { text: "A traditional long dress", independence: -2, reputation: 5 },
                { text: "A mix of both", independence: 3, reputation: 3 }
            ]},
            { text: "You're at a jazz club. Do you...", options: [
                { text: "Smoke & drink", independence: 5, reputation: -3 },
                { text: "Decline politely", independence: -2, reputation: 5 },
                { text: "Just dance!", independence: 3, reputation: 3 }
            ]},
            { text: "Career choice! Do you...", options: [
                { text: "Become a secretary", independence: -3, reputation: 5 },
                { text: "Train to be a pilot", independence: 5, reputation: -3 },
                { text: "Join a women's rights movement", independence: 3, reputation: 3 }
            ]},
            { text: "A newspaper criticizes flappers. Do you...", options: [
                { text: "Write a defense letter", independence: 5, reputation: -3 },
                { text: "Ignore it", independence: -2, reputation: 5 },
                { text: "Organize a debate", independence: 3, reputation: 3 }
            ]}
        ];

        let currentQuestion = 0;
        let independenceScore = 10;
        let reputationScore = 10;

        function startGame() {
            document.querySelector(".container").style.display = "none";
            document.getElementById("game").style.display = "block";
            loadQuestion();
        }

        function loadQuestion() {
            if (currentQuestion < questions.length) {
                document.getElementById("question-number").textContent = currentQuestion + 1;
                document.getElementById("question-text").textContent = questions[currentQuestion].text;
                let buttons = document.querySelectorAll("#game .btn");
                questions[currentQuestion].options.forEach((option, index) => {
                    buttons[index].textContent = option.text;
                });
            } else {
                showResults();
            }
        }

        function selectOption(index) {
            let selectedOption = questions[currentQuestion].options[index];
            independenceScore += selectedOption.independence;
            reputationScore += selectedOption.reputation;
            currentQuestion++;
            loadQuestion();
        }

        function showResults() {
            document.getElementById("game").style.display = "none";
            document.getElementById("result-page").style.display = "block";

            let resultText = "";
            if (independenceScore > reputationScore) {
                resultText = "You are an Ultimate Flapper! Bold, rebellious, and independent.";
            } else if (reputationScore > independenceScore) {
                resultText = "You are a Traditionalist Icon! You respect tradition and maintain a strong social image.";
            } else {
                resultText = "You are a Bridge Between Generations! Balancing change with tradition, you inspire others.";
            }
            
            document.getElementById("result").textContent = resultText;
        }

        function restartGame() {
            currentQuestion = 0;
            independenceScore = 10;
            reputationScore = 10;
            document.getElementById("result-page").style.display = "none";
            document.querySelector(".container").style.display = "block";
        }
    </script>

</body>
</html>
