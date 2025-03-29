<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>과일 한글 낱말 맞추기</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        .game-container { display: flex; justify-content: center; flex-wrap: wrap; gap: 15px; }
        .fruit-box, .word { 
            padding: 10px; 
            border: 2px solid #ccc; 
            border-radius: 5px; 
            cursor: pointer; 
            font-size: 2rem; 
            user-select: none; /* 텍스트 선택 방지 */
        }
        .fruit-box { 
            width: min(180px, 30vw); 
            height: min(180px, 30vw); 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            flex-direction: column; 
            font-size: 4rem; /* 이모지 크기 조정 */
            text-align: center; 
            overflow: hidden; 
            white-space: nowrap; 
            touch-action: none; /* 터치 이벤트 반응 개선 */
            background-color: #f9f9f9;
        }
        .fruit-box p {
            font-size: 1.5rem;
            margin: 5px 0 0 0;
        }
        .word { 
            background-color: #ffeb3b; 
            display: inline-block; 
            margin: 10px; 
            padding: 15px;
            border-radius: 10px;
            text-align: center;
        }
        .message { 
            margin-top: 20px; 
            font-size: 2rem; 
            font-weight: bold; 
        }
        .set-title { 
            font-size: 2rem; 
            font-weight: bold; 
            margin-bottom: 20px; 
        }
    </style>
</head>
<body>
    <h1>과일 한글 낱말 맞추기</h1>
    <div id="set-title" class="set-title"></div>
    <div id="game"></div>
    <div id="message" class="message"></div>
    <script>
        const fruits = [
            { name: "사과", emoji: "🍎" },
            { name: "바나나", emoji: "🍌" },
            { name: "키위", emoji: "🥝" },
            { name: "망고", emoji: "🥭" },
            { name: "딸기", emoji: "🍓" },
            { name: "수박", emoji: "🍉" },
            { name: "포도", emoji: "🍇" },
            { name: "체리", emoji: "🍒" },
            { name: "블루베리", emoji: "🫐" },
            { name: "파인애플", emoji: "🍍" },
            { name: "오렌지", emoji: "🍊" }
        ];

        let currentSet = 0;
        const totalSets = 10;
        let correctCount = 0;
        let draggedWord = null;

        function shuffleArray(array) {
            return array.sort(() => Math.random() - 0.5);
        }

        function loadGame() {
            if (currentSet >= totalSets) {
                alert("게임 완료!");
                return;
            }

            document.getElementById("message").textContent = "";
            document.getElementById("set-title").textContent = `${currentSet + 1}세트`;
            let gameArea = document.getElementById("game");
            gameArea.innerHTML = "";
            correctCount = 0;

            let selectedFruits = shuffleArray([...fruits]).slice(0, 2);
            let words = shuffleArray(selectedFruits.map(f => f.name));

            let fruitContainer = document.createElement("div");
            fruitContainer.className = "game-container";

            selectedFruits.forEach(fruit => {
                let fruitBox = document.createElement("div");
                fruitBox.className = "fruit-box";
                fruitBox.dataset.name = fruit.name;
                fruitBox.innerHTML = `${fruit.emoji}<p></p>`;

                // Drag and drop 지원 (PC)
                fruitBox.ondragover = (event) => event.preventDefault();
                fruitBox.ondrop = (event) => {
                    event.preventDefault();
                    checkAnswer(event.dataTransfer.getData("text"), fruitBox);
                };

                // 터치 이벤트 지원 (모바일)
                fruitBox.ontouchstart = (event) => event.preventDefault();
                fruitBox.ontouchend = (event) => {
                    if (draggedWord) checkAnswer(draggedWord, fruitBox);
                };

                fruitContainer.appendChild(fruitBox);
            });

            gameArea.appendChild(fruitContainer);

            let wordContainer = document.createElement("div");
            wordContainer.className = "game-container";

            words.forEach(word => {
                let wordElement = document.createElement("div");
                wordElement.className = "word";
                wordElement.draggable = true;
                wordElement.textContent = word;

                // Drag 지원 (PC)
                wordElement.ondragstart = (event) => {
                    event.dataTransfer.setData("text", word);
                };

                // 터치 지원 (모바일)
                wordElement.ontouchstart = (event) => {
                    draggedWord = word;
                    event.target.style.backgroundColor = "#ccc";
                };
                wordElement.ontouchend = (event) => {
                    event.target.style.backgroundColor = "#ffeb3b";
                };

                wordContainer.appendChild(wordElement);
            });

            gameArea.appendChild(wordContainer);
        }

        function checkAnswer(word, fruitBox) {
            let messageBox = document.getElementById("message");
            if (word === fruitBox.dataset.name) {
                fruitBox.style.border = "4px solid green";
                fruitBox.querySelector("p").textContent = word;
                correctCount++;
                if (correctCount === 2) {
                    messageBox.textContent = "성공입니다!";
                    messageBox.style.color = "green";
                    setTimeout(() => {
                        currentSet++;
                        loadGame();
                    }, 1000);
                }
            } else {
                messageBox.textContent = "다시 시도해보세요";
                messageBox.style.color = "red";
            }
        }

        window.onload = loadGame;
    </script>
</body>
</html>
    하트선생님이 언어치료 수업을 위해 만들었어요.
    @heartytalk_slp
    tjdah0420@naver.com
    https://blog.naver.com/mindcarelog
