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
            { name: "사과", image: "https://pixabay.com/ko/photos/%EC%82%AC%EA%B3%BC-%EA%B3%BC%EC%9D%BC-%EB%86%8D%EC%9E%A5-%EC%9D%8C%EC%8B%9D-7566512/" },
            { name: "바나나", image: "https://pixabay.com/ko/photos/%EB%B0%94%EB%82%98%EB%82%98-%EA%B3%BC%EC%9D%BC-%EC%9D%B5%EC%9D%80-%EC%8B%A0%EC%84%A0%ED%95%9C-614090/" },
            { name: "키위", image: "https://pixabay.com/ko/photos/%ED%82%A4%EC%9C%84-%EA%B3%BC%EC%9D%BC-%EA%B3%BC%EC%9D%BC-%ED%82%A4%EC%9C%84-%EC%9D%8C%EC%8B%9D-400143/" },
            { name: "망고", image: "https://pixabay.com/ko/photos/%EB%A7%9D%EA%B3%A0-%EC%8B%A0%EC%84%A0%ED%95%9C-%EA%B3%BC%EC%9D%BC-%EB%85%B8%EB%9E%80%EC%83%89-3485606/" },
            { name: "딸기", image: "https://pixabay.com/ko/photos/%EB%94%B8%EA%B8%B0-%EA%B3%BC%EC%9D%BC-%EB%B9%A8%EA%B0%84%EC%83%89-%EC%9D%B5%EC%9D%80-1396330/" },
            { name: "수박", image: "https://pixabay.com/ko/photos/%EC%88%98%EB%B0%95-%EA%B3%BC%EC%9D%BC-%EC%88%98%ED%99%95-%EC%83%9D%EC%82%B0-551235/" },
            { name: "포도", image: "https://pixabay.com/ko/photos/%ED%8F%AC%EB%8F%84-%ED%91%B8%EB%A5%B8-%ED%8F%AC%EB%8F%84-%ED%8F%AC%EB%8F%84%EC%9B%90-%EB%86%8D%EC%97%85-8306833/" },
            { name: "체리", image: "https://pixabay.com/ko/photos/%EB%B2%84%EC%B0%8C-%EA%B3%BC%EC%9D%BC-%EC%9D%8C%EC%8B%9D-%EB%AA%A8%EB%A0%90%EB%A1%9C-%EC%B2%B4%EB%A6%AC-371233/" },
            { name: "블루베리", image: "https://pixabay.com/ko/photos/%EB%B8%94%EB%A3%A8%EB%B2%A0%EB%A6%AC-%EB%B8%94%EB%A3%A8-%EB%B2%A0%EB%A6%AC-%EA%B3%BC%EC%9D%BC-873784/" },
            { name: "파인애플", image: "https://pixabay.com/ko/photos/%ED%8C%8C%EC%9D%B8%EC%95%A0%ED%94%8C-%EC%A1%B0%EA%B0%81-%EA%B3%BC%EC%9D%BC-%EB%B9%84%ED%83%80%EB%AF%BC-636562/" },
            { name: "오렌지", image: "https://pixabay.com/ko/photos/%EC%98%A4%EB%A0%8C%EC%A7%80-%EA%B3%BC%EC%9D%BC-%EC%9D%8C%EC%8B%9D-407429/" }
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
