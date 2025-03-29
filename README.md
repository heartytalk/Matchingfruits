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
            { name: "사과", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTMz/MDAxNzQzMjM1MjI5ODI1.xCICDEBco6fazERSuLqwpUIzbZpS0ewX65LgPRXRGFog.pmAwAo7OPKjqZggK4f4ocltQeBrGc7mJ4hnhjzI7WPUg.JPEG/apples-7566512_1280.jpg?type=w966" },
            { name: "바나나", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMjA3/MDAxNzQzMjM1MjI5ODEx.8P36wfj5GkuyjxdcPGaAS_YqZ2RASNglK1L8HRjYFGMg.wquxEjPSiFTyhiEpiYvHcxCTcDFd4WjI9tVvfRs9kWEg.JPEG/banana-1025109_1280.jpg?type=w966" },
            { name: "키위", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTEx/MDAxNzQzMjM1MjI5ODMz.ZGGv555b1G6S5LEdffJysVFAYfvI5twQOIT2zGLuAMgg.-tEEVCYrzyl9NrRbX6GGxrmiPPAP4HoEc3OCyGmjsPMg.JPEG/breakfast-1239438_1280.jpg?type=w966" },
            { name: "망고", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTQ0/MDAxNzQzMjM1MjI5ODE4.LPl_fbgKT5w-mQoGmtZJmOcDFkSdlW31hO1Q7-fb0vcg.O0SHbECfVfZZCuaOPgxexWwSy-KLEdXcSxhf1tkyX-Yg.JPEG/mango-5171747_1280.jpg?type=w966" },
            { name: "딸기", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTI3/MDAxNzQzMjM1MjI5ODMy.HMlwZQM6s3oOt7F6T26oDSSfZZY8hgA36Wa5IoRECRAg.6A97TYn3dUfkgyTMNaqQ6xkJ85oYhy6Q64u0Rve-vIwg.JPEG/strawberries-3359755_1280.jpg?type=w966" },
            { name: "수박", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMjc0/MDAxNzQzMjM1MjMwMjk1.st34moUeutbSVex5gqAKQK6nutJk0rGdq0Pbo0yrUDAg.Jphzvf4dVgp9ul5r47lA7j0mOWgMxYkFqsNY-XMIoVAg.JPEG/watermelon-453214_1280.jpg?type=w966" },
            { name: "포도", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTcz/MDAxNzQzMjM1MjMwMzg0.QfVBjUhyVSYscnhXTD6ivhTgkCuLOSEiDwhjJgNbPTcg.upr-fgSGaEJDB4sF_cWH8wrtxaVajDlEFg_6uRo-5G0g.JPEG/grapes-8306833_1280.jpg?type=w966" },
            { name: "체리", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMjMg/MDAxNzQzMjM1MjMwNDU2.gu-hLtb9DTbEM4Ty9wdmU6YSnRQ_r9sjmhdn6YWu3Lkg.kl01lDwHT0jK9XwAzy8B-bW8yqcvr2siv-A2zZOmO8kg.JPEG/cherry-1914118_1280.jpg?type=w966" },
            { name: "블루베리", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTc2/MDAxNzQzMjM1MjMwNDYx.aLszw-i7X16yFstfA63VNGOjeGn-VIaXpFAVdUQifgMg.LrTWc7yH1nTpF-sad4We3wmKWuRJhYwrhfBjc38TTrAg.JPEG/bilberries-2559051_1280.jpg?type=w966" },
            { name: "파인애플", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfMTAz/MDAxNzQzMjM1MjMwNDc1.7AXtwzCCdXZ3VRzfmYDp1F-G7GGhRP2u3jmQmroXn4og.rg4gsH4_SoDRTn_D-DrRaBGrmiVpy4o95_6eTbe0VWwg.JPEG/pineapple-5108775_1280.jpg?type=w966" },
            { name: "오렌지", image: "https://postfiles.pstatic.net/MjAyNTAzMjlfNDAg/MDAxNzQzMjM1MjI5ODE1.WyyG0P9qQTMMeBZgxg5XCTrSGTqK1_2frfTJmFxZ1okg.Xl0pQ2KLMCTcwg3yDqD_SOZgrhvufRo6gv8ichYWjIEg.JPEG/oranges-407429_1280.jpg?type=w966" }
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
