<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>과일 한글 낱말 맞추기</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        .game-container { display: flex; justify-content: center; flex-wrap: wrap; gap: 20px; }
        .fruit-box, .word { padding: 20px; border: 2px solid #ccc; border-radius: 5px; cursor: pointer; font-size: 2rem; }
        .fruit-box { 
            width: 200px; 
            height: 200px; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            flex-direction: column; 
            font-size: 6rem; /* 이모지 크기 증가 */
            text-align: center; /* 이름도 가운데 정렬 */
        }
        .word { 
            background-color: #ffeb3b; 
            display: inline-block; 
            margin: 10px; 
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
                fruitBox.ondragover = (event) => event.preventDefault();
                fruitBox.ondrop = (event) => {
                    event.preventDefault();
                    let word = event.dataTransfer.getData("text");
                    let messageBox = document.getElementById("message");
                    if (word === fruit.name) {
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
                wordElement.ondragstart = (event) => {
                    event.dataTransfer.setData("text", word);
                };
                wordContainer.appendChild(wordElement);
            });

            gameArea.appendChild(wordContainer);
        }

        window.onload = loadGame;
    </script>
</body>
</html>

    하트선생님이 언어치료 수업을 위해 만들었어요.
    @heartytalk_slp
    tjdah0420@naver.com
    https://blog.naver.com/mindcarelog
