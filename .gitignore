<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>看護師国試！合格ドリル</title>
    <link href="https://fonts.googleapis.com/css2?family=Kiwi+Maru:wght@500&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #ff8fab;
            --secondary-color: #fb6f92;
            --bg-color: #ffe5ec;
            --white: #ffffff;
            --correct: #4cc9f0;
            --wrong: #f72585;
        }

        body {
            font-family: 'Kiwi Maru', serif;
            background-color: var(--bg-color);
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #333;
        }

        #app {
            background: var(--white);
            width: 90%;
            max-width: 400px;
            border-radius: 30px;
            padding: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
        }

        h1 { color: var(--secondary-color); font-size: 1.5rem; margin-bottom: 10px; }

        .progress-bar {
            height: 10px;
            background: #eee;
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        #progress {
            height: 100%;
            background: var(--primary-color);
            width: 0%;
            transition: width 0.3s;
        }

        .question-card { margin-bottom: 20px; }

        .question-text {
            font-size: 1.1rem;
            line-height: 1.6;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .options {
            display: grid;
            gap: 12px;
        }

        .option-btn {
            background: var(--white);
            border: 3px solid var(--primary-color);
            padding: 15px;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 1rem;
            font-family: inherit;
        }

        .option-btn:active { transform: scale(0.95); }
        .option-btn.correct { background: var(--correct); color: white; border-color: var(--correct); }
        .option-btn.wrong { background: var(--wrong); color: white; border-color: var(--wrong); }

        #result-screen { display: none; }
        .score { font-size: 2rem; color: var(--secondary-color); font-weight: bold; }
        
        .next-btn {
            margin-top: 20px;
            background: var(--secondary-color);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 50px;
            font-size: 1rem;
            cursor: pointer;
            display: none;
        }
    </style>
</head>
<body>

<div id="app">
    <div id="game-screen">
        <h1>🏥 看護師国試ドリル</h1>
        <div class="progress-bar"><div id="progress"></div></div>
        
        <div class="question-card">
            <div id="question" class="question-text">読み込み中...</div>
            <div id="options" class="options"></div>
        </div>
        <button id="next-btn" class="next-btn" onclick="nextQuestion()">次の問題へ ✨</button>
    </div>

    <div id="result-screen">
        <h1>おつかれさまでした！🌸</h1>
        <p class="score" id="final-score"></p>
        <p id="result-message"></p>
        <button class="next-btn" style="display:inline-block" onclick="location.reload()">もう一度挑戦する</button>
    </div>
</div>

<script>
    // 問題データ（ここを増やすだけで問題が追加できます）
    const quizData = [
        {
            q: "日本の令和4年（2022年）における合計特殊出生率は？",
            a: ["1.26", "1.30", "1.42", "1.57"],
            correct: 0
        },
        {
            q: "チアノーゼの際、血液中で増加しているのはどれ？",
            a: ["酸化ヘモグロビン", "還元ヘモグロビン", "血小板", "白血球"],
            correct: 1
        },
        {
            q: "成人の静止期における1回換気量の標準値はどれ？",
            a: ["約150mL", "約300mL", "約500mL", "約1,000mL"],
            correct: 2
        },
        {
            q: "法律上、看護師の定義が定められているのは？",
            a: ["保健師助産師看護師法", "医療法", "看護師法", "医師法"],
            correct: 0
        }
    ];

    let currentIdx = 0;
    let score = 0;

    const qElement = document.getElementById('question');
    const optionsElement = document.getElementById('options');
    const nextBtn = document.getElementById('next-btn');
    const progress = document.getElementById('progress');

    function loadQuestion() {
        const data = quizData[currentIdx];
        qElement.innerText = data.q;
        optionsElement.innerHTML = '';
        nextBtn.style.display = 'none';
        
        // 進捗バーの更新
        progress.style.width = `${(currentIdx / quizData.length) * 100}%`;

        data.a.forEach((ans, i) => {
            const btn = document.createElement('button');
            btn.className = 'option-btn';
            btn.innerText = ans;
            btn.onclick = () => checkAnswer(i, btn);
            optionsElement.appendChild(btn);
        });
    }

    function checkAnswer(selected, btn) {
        const correct = quizData[currentIdx].correct;
        const allBtns = document.querySelectorAll('.option-btn');
        
        // 全ボタンを無効化
        allBtns.forEach(b => b.disabled = true);

        if (selected === correct) {
            btn.classList.add('correct');
            btn.innerText += " ⭕";
            score++;
        } else {
            btn.classList.add('wrong');
            btn.innerText += " ❌";
            allBtns[correct].classList.add('correct'); // 正解を表示
        }
        
        nextBtn.style.display = 'inline-block';
    }

    function nextQuestion() {
        currentIdx++;
        if (currentIdx < quizData.length) {
            loadQuestion();
        } else {
            showResult();
        }
    }

    function showResult() {
        document.getElementById('game-screen').style.display = 'none';
        const rs = document.getElementById('result-screen');
        rs.style.display = 'block';
        document.getElementById('final-score').innerText = `${quizData.length}問中 ${score}問正解！`;
        
        let msg = "この調子で国試まで走り抜けよう！🔥";
        if (score === quizData.length) msg = "満点！あなたは天才看護学生！🌸";
        document.getElementById('result-message').innerText = msg;
    }

    // 開始
    loadQuestion();
</script>

</body>
</html>
