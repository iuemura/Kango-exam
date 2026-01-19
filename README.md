<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>母性看護学・特訓ドリル</title>
    <link href="https://fonts.googleapis.com/css2?family=Kiwi+Maru:wght@500&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #ffb3c1;
            --secondary-color: #ff4d6d;
            --bg-color: #fff0f3;
            --white: #ffffff;
            --correct: #4cc9f0;
            --wrong: #f72585;
            --accent: #ff85a1;
        }

        body {
            font-family: 'Kiwi Maru', serif;
            background-color: var(--bg-color);
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #444;
        }

        #app {
            background: var(--white);
            width: 92%;
            max-width: 420px;
            border-radius: 25px;
            padding: 25px;
            box-shadow: 0 8px 30px rgba(255, 77, 109, 0.15);
            text-align: center;
            border: 4px solid var(--primary-color);
        }

        h1 { color: var(--secondary-color); font-size: 1.4rem; margin-bottom: 5px; }
        .sub-title { font-size: 0.8rem; color: var(--accent); margin-bottom: 15px; }

        .progress-bar {
            height: 12px;
            background: #ffe5ec;
            border-radius: 10px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        #progress {
            height: 100%;
            background: linear-gradient(to right, var(--primary-color), var(--secondary-color));
            width: 0%;
            transition: width 0.4s ease;
        }

        .question-text {
            font-size: 1.1rem;
            line-height: 1.5;
            margin-bottom: 20px;
            text-align: left;
            background: #fffafa;
            padding: 15px;
            border-radius: 15px;
            border-left: 5px solid var(--secondary-color);
        }

        .options {
            display: grid;
            gap: 12px;
        }

        .option-btn {
            background: var(--white);
            border: 2px solid #ffccd5;
            padding: 14px;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.2s;
            font-size: 1rem;
            font-family: inherit;
            color: #555;
        }

        .option-btn:hover { background-color: #fff5f7; }
        .option-btn.correct { background: var(--correct) !important; color: white; border-color: var(--correct); }
        .option-btn.wrong { background: var(--wrong) !important; color: white; border-color: var(--wrong); }

        #result-screen { display: none; }
        .score-val { font-size: 2.5rem; color: var(--secondary-color); margin: 10px 0; }
        
        .next-btn {
            margin-top: 20px;
            background: var(--secondary-color);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 50px;
            font-size: 1.1rem;
            cursor: pointer;
            display: none;
            box-shadow: 0 4px 15px rgba(255, 77, 109, 0.3);
        }
    </style>
</head>
<body>

<div id="app">
    <div id="game-screen">
        <h1>👶 母性看護学ドリル</h1>
        <div class="sub-title">～ 国試対策・実習応援編 ～</div>
        <div class="progress-bar"><div id="progress"></div></div>
        
        <div id="question" class="question-text">読み込み中...</div>
        <div id="options" class="options"></div>
        
        <button id="next-btn" class="next-btn" onclick="nextQuestion()">次へ進む 🐾</button>
    </div>

    <div id="result-screen">
        <h1>実習もお疲れさま！🌸</h1>
        <p class="score-val" id="final-score"></p>
        <p id="result-message" style="line-height:1.6"></p>
        <button class="next-btn" style="display:inline-block" onclick="location.reload()">もう一度解く</button>
    </div>
</div>

<script>
    const quizData = [
        {
            q: "正常な妊娠経過において、レオポルド触診法で胎位、胎向を確認できる時期として最も適切なのはどれか？",
            a: ["妊娠12週以降", "妊娠16週以降", "妊娠20週以降", "妊娠28週以降"],
            correct: 3
        },
        {
            q: "母子保健法において、妊娠した者が届け出ることによって交付されるのはどれか？",
            a: ["出産育児一時金", "母子健康手帳", "児童手当", "育児休業給付金"],
            correct: 1
        },
        {
            q: "レオポルド触診法で、胎児の背中が母体の左側にある場合、胎向はどれか？",
            a: ["第1胎向", "第2胎向", "縦産式", "横産式"],
            correct: 0
        },
        {
            q: "産褥3日の褥婦。子宮底の高さはどこにあるのが正常か？",
            a: ["臍上1横指", "臍下1～2横指", "臍下3横指（臍と恥骨結合の中間）", "恥骨結合上縁"],
            correct: 2
        },
        {
            q: "母体保護法で規定されているのはどれか？",
            a: ["産前産後の休業", "不妊手術", "出生届の提出", "乳幼児健診"],
            correct: 1
        }
    ];

    let currentIdx = 0;
    let score = 0;

    function loadQuestion() {
        const data = quizData[currentIdx];
        document.getElementById('question').innerText = `Q${currentIdx + 1}: ${data.q}`;
        const opts = document.getElementById('options');
        opts.innerHTML = '';
        document.getElementById('next-btn').style.display = 'none';
        
        document.getElementById('progress').style.width = `${(currentIdx / quizData.length) * 100}%`;

        data.a.forEach((ans, i) => {
            const btn = document.createElement('button');
            btn.className = 'option-btn';
            btn.innerText = ans;
            btn.onclick = () => {
                if(document.getElementById('next-btn').style.display === 'inline-block') return;
                checkAnswer(i, btn);
            };
            opts.appendChild(btn);
        });
    }

    function checkAnswer(selected, btn) {
        const correct = quizData[currentIdx].correct;
        const allBtns = document.querySelectorAll('.option-btn');
        
        if (selected === correct) {
            btn.classList.add('correct');
            score++;
        } else {
            btn.classList.add('wrong');
            allBtns[correct].classList.add('correct');
        }
        document.getElementById('next-btn').style.display = 'inline-block';
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
        document.getElementById('result-screen').style.display = 'block';
        document.getElementById('final-score').innerText = `${score} / ${quizData.length}`;
        
        let msg = "母性の基礎はバッチリ！実習中もこの知識が役立ちます。";
        if (score === quizData.length) msg = "完璧です！✨ この調子で国試まで突っ走りましょう！🌸";
        document.getElementById('result-message').innerText = msg;
    }

    loadQuestion();
</script>

</body>
</html>
