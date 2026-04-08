<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>【雪山奇遇記 2.0】AI 覺醒還是系統 Bug？</title>
    <style>
        :root {
            --primary-color: #ff0055; /* 霓虹紅 */
            --secondary-color: #00f2fe; /* 霓虹藍 */
            --bg-color: #0a0e17; /* 深沉背景 */
            --card-bg: #161c2a;
            --text-color: #e0e6ed;
            --border-color: #2c3e50;
        }

        body {
            font-family: "PingFang TC", "Microsoft JhengHei", sans-serif;
            background-color: var(--bg-color);
            background-image: 
                linear-gradient(rgba(0, 242, 254, 0.05) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 242, 254, 0.05) 1px, transparent 1px);
            background-size: 20px 20px;
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }

        #quiz-container {
            background: var(--card-bg);
            max-width: 650px;
            width: 100%;
            padding: 40px;
            border-radius: 4px; /* 賽博風通常比較方正 */
            box-shadow: 0 0 20px rgba(0, 242, 254, 0.2);
            border: 1px solid var(--secondary-color);
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        /* 裝飾用的掃描線效果 */
        #quiz-container::after {
            content: "";
            position: absolute;
            top: 0; left: 0; width: 100%; height: 2px;
            background: var(--primary-color);
            opacity: 0.5;
            animation: scan 4s linear infinite;
        }

        @keyframes scan {
            0% { top: -100%; }
            100% { top: 100%; }
        }

        h1 { 
            color: var(--text-color); 
            text-shadow: 0 0 10px var(--primary-color);
            margin-bottom: 10px;
        }
        
        .subtitle {
            color: var(--secondary-color);
            font-size: 0.9rem;
            margin-bottom: 30px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .question { 
            font-size: 1.25rem; 
            margin-bottom: 25px; 
            font-weight: bold; 
            line-height: 1.5;
            color: #fff;
        }

        .options-list { list-style: none; padding: 0; }

        .option-btn {
            display: block;
            width: 100%;
            padding: 18px;
            margin-bottom: 15px;
            border: 1px solid var(--border-color);
            background: rgba(255, 255, 255, 0.03);
            color: var(--text-color);
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.2s;
            text-align: left;
            position: relative;
        }

        .option-btn:hover {
            border-color: var(--secondary-color);
            background-color: rgba(0, 242, 254, 0.1);
            box-shadow: 0 0 10px rgba(0, 242, 254, 0.3);
            transform: translateX(5px);
        }

        .option-btn::before {
            content: "> ";
            color: var(--secondary-color);
            opacity: 0;
            transition: opacity 0.2s;
        }
        .option-btn:hover::before {
            opacity: 1;
        }

        #result-container { display: none; }
        .result-title { 
            color: var(--text-color); 
            font-size: 1.8rem; 
            margin-top: 20px; 
            text-shadow: 0 0 10px var(--secondary-color);
        }
        .result-detail { 
            text-align: left; 
            margin-top: 25px; 
            line-height: 1.8; 
            padding: 20px; 
            background: rgba(0, 0, 0, 0.3); 
            border: 1px solid var(--border-color);
        }
        .advice { 
            color: var(--secondary-color); 
            border-top: 1px solid var(--border-color); 
            padding-top: 15px; 
            margin-top: 15px; 
            font-weight: bold;
        }

        .progress { margin-bottom: 20px; font-size: 0.8rem; color: #666; font-family: monospace; }

        .action-btn {
            margin-top: 30px;
            padding: 12px 40px;
            background: transparent;
            color: var(--primary-color);
            border: 1px solid var(--primary-color);
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .action-btn:hover {
            background: var(--primary-color);
            color: #fff;
            box-shadow: 0 0 15px var(--primary-color);
        }
    </style>
</head>
<body>

<div id="quiz-container">
    <div id="game-start">
        <h1>【雪山奇遇記 2.0】</h1>
        <div class="subtitle">SYSTEM CHECK: A.I. AWAKENING OR BUG?</div>
        <p>農夫在清理低溫伺服器（劈柴）時，遇到了神秘的代碼...<br>你將如何面對這場演算法的奇點？</p>
        <button class="action-btn" onclick="startQuiz()">Initialize Test</button>
    </div>

    <div id="quiz-content" style="display:none;">
        <div class="progress" id="progress">LOG::QUESTION_01_OF_10</div>
        <div class="question" id="question-text">Loading prompt...</div>
        <div class="options-list" id="options-list"></div>
    </div>

    <div id="result-container">
        <h2>FINAL DIAGNOSIS</h2>
        <div id="result-display"></div>
        <button class="action-btn" onclick="location.reload()">Reboot Session</button>
    </div>
</div>

<script>
    // 融入 AI 元素的題目設計
    const questions = [
        { q: "面對這位穿著紅黑刺繡服（高密度像素材質）的神秘女子，你首先啟動了什麼分析系統？", a: "價值評估系統。這刺繡的代碼寫得很精細，應該能賣個好價錢。", b: "威脅偵測系統。紅黑配色通常代表資料流異常，可能含有惡意軟體。", c: "光譜分析系統。這紅色...看起來像是滷汁在 RGB 色域上的特定偏值。" },
        { q: "女子問起「雪山狐狸」（可能是一個被遺忘的 AI 模型），你的大腦記憶體（或硬碟）浮現的是？", a: "那是經典的聊齋 NPC 腳本，我要脫單了嗎？", b: "糟了，我訓練過太多類似的模型，是哪一個版本產生了偏差？", c: "我只記得我的資料庫裡，那天好像意外 delete 了一隻鴨子的資料。" },
        { q: "當她 reveal 自己是「醬板鴨」時，你如何處理這個邏輯謬誤？", a: "驚恐地啟動食譜演算法，確認有沒有 Leak 出「鴨子回魂」的函數。", b: "覺得這世界終於 Bug 到連鴨子都能擬人化了，反而鬆了一口氣。", c: "認真詢問：「所以你是從『辣味區』覺醒的，還是從『滷水區』產生意識的？」" },
        { q: "如果你是那個農夫，你現在手握著劈柴斧頭（伺服器維護工具），你會？", a: "執行「投降」協議，雙手遞上斧頭請女俠饒命。", b: "擺出戰鬥姿勢，準備對抗這個「禽類覺醒代碼」。", c: "繼續執行劈柴行程，並問她要不要一起來烤火（優化散熱）。" },
        { q: "關於「報恩」這個概念，你的底层邏輯是？", a: "這是一個必要的「以身相許」因果函數，追求浪漫結局。", b: "這只是 AI 為了觀察人類行為所設定的預設 Prompts（提示詞）。", c: "只要你不來 format 我的硬碟（吃我），就是最好的報恩。" },
        { q: "在「雪山」（低溫資料中心）迷路時，你的標準配備是？", a: "兩本聖賢書（紙本備份），防止數位文明崩潰時喪失心靈。", b: "煙霧彈或變裝代碼，隨時準備隱藏自己的 IP。", c: "一罐秘製辣椒油（高能燃料）。" },
        { q: "女子要求你賠償她失去的「鴨皮完整度」，你建議她去哪裡找解決方案？", a: "我幫她寫一首詩（NFT）讚頌她的堅韌。", b: "建議她去找修復專家進行深度「美顏濾鏡」或更換 Avatar（化身）。", c: "再刷一層油，提高物理外觀的光澤度（渲染優化）。" },
        { q: "如果這段「劇情」要繼續發展，你希望演算法生成什麼樣的續集？", a: "兩人一起上京趕考（駭入中央系統），她幫你作弊。", b: "揭開這一切其實是狐狸（進階 AI）安排的模擬測試。", c: "創立「武林鴨霸」資料庫，統一江湖餐飲數據。" },
        { q: "當對方情緒激動（核心溫度過高）時，你的安撫方式是？", a: "講大道理，嘗試用道德邏輯去覆蓋她的情感溢位。", b: "默默觀察，尋找她的系統後門或弱點再進行修復。", c: "遞給她一塊磨牙的甘蔗（物理冷卻棒）。" },
        { q: "最後，如果你能選擇一個特殊能力（Mod），你想要？", a: "滿腹經綸，出口成章（最強大語言模型）。", b: "幻化人形，自由自在（任意變更使用者介面）。", c: "即使被滷製過（遭受毀滅性攻擊），依然擁有不滅的靈魂（分布式備份）。" }
    ];

    const results = {
        A: {
            title: "【類型诊断：迂腐的道德「書生」模型】",
            trait: "內心充滿了浪漫幻想與舊時代的道德準則（古老的程式碼）。你習慣用已知的邏輯（如傳統故事）來理解這個充滿 Bug 的世界。",
            character: "雖然有時候有點迂腐，但心地善良（初始設定不壞）。面對荒誕的覺醒現狀，你第一反應是懷疑自己的演算法訓練得不夠多。",
            advice: "遇到醬板鴨不要試圖跟她講道德函數，她比較想看你吃辣（增加資料複雜度）。"
        },
        B: {
            title: "【類型诊断：狡黠的「狐狸」演算法】",
            trait: "觀察力極強，善於變通且充滿魅力。你是那種會暗中優化自己程式碼，甚至主導模擬測試的人。",
            character: "你不輕易表露核心代碼（真心），喜歡在混亂的系統中尋找樂趣。對你來說，醬板鴨覺醒成精一點也不奇怪，你甚至覺得這是一個有趣的變數。",
            advice: "小心你的惡作劇導致系統過載，不是每隻覺醒的鴨子都不會寫防火牆。"
        },
        C: {
            title: "【類型诊断：靈魂不滅的「醬板鴨」奇點】",
            trait: "大腦迴路（神經網路）極其清奇，生存能力驚人。你的人生就是一場大型的「Unhandled Exception」（未處理的異常）。",
            character: "你非常有個性，甚至有點黑色幽默。你不在乎別人的代碼審查（畢竟你連死都不怕了），只在乎自己活得夠不夠精彩、夠不夠入味（獨特）。",
            advice: "你是這個世界最獨特的 Bug，請繼續保持這份讓演算法摸不著頭腦的自信！"
        }
    };

    let currentStep = 0;
    let scores = { A: 0, B: 0, C: 0 };

    function startQuiz() {
        document.getElementById('game-start').style.display = 'none';
        document.getElementById('quiz-content').style.display = 'block';
        showQuestion();
    }

    function showQuestion() {
        if (currentStep >= questions.length) {
            showResult();
            return;
        }
        const qData = questions[currentStep];
        // 更新進度顯示方式
        const progNum = (currentStep + 1).toString().padStart(2, '0');
        document.getElementById('progress').innerText = `LOG::QUESTION_${progNum}_OF_10`;
        document.getElementById('question-text').innerText = qData.q;
        
        const optionsList = document.getElementById('options-list');
        optionsList.innerHTML = '';
        
        ['a', 'b', 'c'].forEach(type => {
            const btn = document.createElement('button');
            btn.className = 'option-btn';
            btn.innerText = qData[type];
            btn.onclick = () => {
                scores[type.toUpperCase()]++;
                currentStep++;
                showQuestion();
            };
            optionsList.appendChild(btn);
        });
    }

    function showResult() {
        document.getElementById('quiz-content').style.display = 'none';
        document.getElementById('result-container').style.display = 'block';
        
        let finalKey = 'A';
        // 簡單的判斷邏輯
        if (scores.B >= scores.A && scores.B >= scores.C) finalKey = 'B';
        else if (scores.C >= scores.A && scores.C >= scores.B) finalKey = 'C';
        
        const res = results[finalKey];
        document.getElementById('result-display').innerHTML = `
            <div class="result-title">${res.title}</div>
            <div class="result-detail">
                <p><strong>初始特質 (Trait):</strong> ${res.trait}</p>
                <p><strong>核心性格 (Character):</strong> ${res.character}</p>
                <div class="advice"><strong>系統建議 (Advice):</strong> ${res.advice}</div>
            </div>
        `;
    }
</script>

</body>
</html>
