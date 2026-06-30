<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>بازی کلمات درهم‌ریخته</title>
    <style>
        body { 
            font-family: Tahoma, sans-serif; 
            background-color: #ffe4e1; 
            text-align: center; 
            padding: 20px;
            overflow: hidden; /* برای جلوگیری از اسکرول */
            height: 100vh;
        }
        
        /* انیمیشن عروسک‌ها */
        .puppet {
            position: absolute;
            font-size: 50px;
            animation: float 3s infinite ease-in-out;
        }
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .container { 
            background: rgba(255, 255, 255, 0.9); 
            padding: 20px; 
            border-radius: 30px; 
            border: 5px solid #ffb6c1; 
            display: inline-block; 
            width: 80%; 
            max-width: 500px; 
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
            position: relative;
            z-index: 10;
        }
        h1 { color: #ff69b4; margin-bottom: 5px; }
        .subtitle { color: #ff1493; margin-top: 0; font-size: 16px; font-weight: bold;}
        button { background: #87ceeb; border: none; padding: 10px 20px; border-radius: 20px; color: white; cursor: pointer; font-size: 16px; margin-top: 10px; transition: 0.3s; }
        button:hover { background: #5f9ea0; transform: scale(1.05); }
        .hidden { display: none; }
        .word { font-size: 40px; font-weight: bold; color: #ff4500; margin: 25px 0; letter-spacing: 15px; }
    </style>
</head>
<body>

<!-- عروسک‌های متحرک در پس‌زمینه -->
<div class="puppet" style="top: 10%; left: 10%;">🧸</div>
<div class="puppet" style="top: 15%; right: 10%; animation-delay: 1s;">🎈</div>
<div class="puppet" style="bottom: 10%; left: 15%; animation-delay: 0.5s;">🌟</div>
<div class="puppet" style="bottom: 15%; right: 15%; animation-delay: 1.5s;">🦄</div>

<div class="container" id="gameBox">
    <h1>بازی آموزشی</h1>
    <p class="subtitle">کلمات در هم ریخته</p>
    <p>آموزگار: <b>شایسته صفی</b></p>
    
    <div id="startScreen">
        <input type="text" id="studentName" placeholder="نام دانش‌آموز را وارد کنید" style="padding: 10px; border-radius: 10px; border: 1px solid #ddd;">
        <br><button onclick="startGame()">شروع بازی</button>
    </div>

    <div id="quizScreen" class="hidden">
        <p>سلام <span id="displayName"></span> عزیز! این کلمه چیست؟</p>
        <div class="word" id="scrambledWord"></div>
        <button id="btn1" onclick="checkAnswer(0)"></button>
        <button id="btn2" onclick="checkAnswer(1)"></button>
    </div>

    <div id="resultScreen" class="hidden">
        <h2>پایان بازی!</h2>
        <p id="finalScore"></p>
        <button onclick="location.reload()">دوباره بازی کن</button>
    </div>
</div>

<script>
    const words = ["میکروب", "مناسب", "گروه", "موجودات", "خطرناک", "دانشمندان", "کثیف", "قورباغه", "سنجاقک", "کشاورز", "شاهنامه", "عظیم"];
    let currentWord = "", score = 0, index = 0;

    function scramble(word) {
        return word.split('').sort(() => Math.random() - 0.5).join(' ');
    }

    function startGame() {
        const name = document.getElementById('studentName').value;
        if (!name) return alert("لطفا نام خود را وارد کنید");
        document.getElementById('displayName').innerText = name;
        document.getElementById('startScreen').classList.add('hidden');
        document.getElementById('quizScreen').classList.remove('hidden');
        showQuestion();
    }

    function showQuestion() {
        if (index >= words.length) return endGame();
        currentWord = words[index];
        let wrongWord = words[(index + 1) % words.length];
        
        document.getElementById('scrambledWord').innerText = scramble(currentWord);
        
        let options = [currentWord, wrongWord].sort(() => Math.random() - 0.5);
        document.getElementById('btn1').innerText = options[0];
        document.getElementById('btn2').innerText = options[1];
    }

    function checkAnswer(btnIdx) {
        let choice = document.getElementById('btn' + (btnIdx + 1)).innerText;
        if (choice === currentWord) score++;
        index++;
        showQuestion();
    }

    function endGame() {
        document.getElementById('quizScreen').classList.add('hidden');
        document.getElementById('resultScreen').classList.remove('hidden');
        document.getElementById('finalScore').innerText = `امتیاز شما: ${score} از ${words.length}. آفرین به شما دانش‌آموز کوشا!`;
    }
</script>
</body>
</html>
