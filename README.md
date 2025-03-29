<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>과일 한글 낱말 맞추기</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        .game-container { display: flex; justify-content: center; flex-wrap: wrap; gap: 20px; }
        .fruit-box, .word { padding: 20px; border: 2px solid #ccc; border-radius: 5px; cursor: pointer; font-size: 2rem; }
        .fruit-box { width: 300px; height: 300px; display: flex; align-items: center; justify-content: center; flex-direction: column; }
        .fruit-box img { width: 200px; height: 200px; }
        .word { background-color: #ffeb3b; display: inline-block; margin: 10px; }
        .message { margin-top: 20px; font-size: 2rem; font-weight: bold; }
        .set-title { font-size: 2rem; font-weight: bold; margin-bottom: 20px; }
    </style>
</head>
<body>
    <h1>과일 한글 낱말 맞추기</h1>
    <div id="set-title" class="set-title"></div>
    <div id="game"></div>
    <div id="message" class="message"></div>
    <script>
        const fruits = [
            { name: "사과", image: "https://cdn.pixabay.com/photo/2017/09/26/13/50/apple-2788662_640.jpg" },
            { name: "바나나", image: "https://cdn.pixabay.com/photo/2018/08/16/20/03/bananas-3616193_640.jpg" },
            { name: "키위", image: "https://cdn.pixabay.com/photo/2016/11/18/15/56/kiwi-1839735_640.jpg" },
            { name: "망고", image: "https://cdn.pixabay.com/photo/2016/07/22/09/59/mango-1534061_640.jpg" },
            { name: "딸기", image: "https://cdn.pixabay.com/photo/2018/03/20/23/11/strawberries-3249761_640.jpg" },
            { name: "수박", image: "https://cdn.pixabay.com/photo/2018/08/23/08/55/watermelon-3622979_640.jpg" },
            { name: "포도", image: "https://cdn.pixabay.com/photo/2016/08/23/15/45/grapes-1616662_640.jpg" },
            { name: "체리", image: "https://cdn.pixabay.com/photo/2018/06/28/19/17/cherries-3504853_640.jpg" },
            { name: "블루베리", image: "https://cdn.pixabay.com/photo/2016/03/05/19/02/blueberries-1239279_640.jpg" },
            { name: "파인애플", image: "https://cdn.pixabay.com/photo/2016/03/05/19/02/pineapple-1239425_640.jpg" },
            { name: "오렌지", image: "https://cdn.pixabay.com/photo/2016/11/29/04/17/oranges-1868612_640.jpg" }
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
                fruitBox.innerHTML = `<img src="${fruit.image}" alt="${fruit.name}"><p></p>`;
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
