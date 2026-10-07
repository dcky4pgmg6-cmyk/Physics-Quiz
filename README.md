<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اختبار تحصيلي فيزياء 3 - أ. خديجة عسيري</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }
        .quiz-container {
            background-color: #ffffff;
            width: 100%;
            max-width: 650px;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .header {
            text-align: center;
            border-bottom: 2px solid #3b82f6;
            padding-bottom: 15px;
            margin-bottom: 20px;
        }
        .header h1 {
            color: #1e3a8a;
            font-size: 22px;
            margin: 0 0 8px 0;
        }
        .header h2 {
            color: #3b82f6;
            font-size: 16px;
            margin: 0;
        }
        .progress-bar {
            text-align: left;
            font-size: 14px;
            color: #6b7280;
            margin-bottom: 10px;
        }
        .question {
            font-size: 18px;
            font-weight: bold;
            color: #333;
            margin-bottom: 15px;
            line-height: 1.5;
        }
        .options {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .options li {
            margin-bottom: 10px;
        }
        .options button {
            width: 100%;
            padding: 12px;
            background-color: #f0f4f8;
            border: 1px solid #d1d5db;
            border-radius: 8px;
            font-size: 16px;
            text-align: right;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        .options button:hover {
            background-color: #e2e8f0;
        }
        .btn-next {
            display: block;
            width: 100%;
            padding: 12px;
            background-color: #10b981;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 20px;
        }
        .btn-next:hover {
            background-color: #059669;
        }
        .result {
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            color: #1e3a8a;
        }
    </style>
</head>
<body>

<div class="quiz-container">
    <div class="header">
        <h1>اختبار تحصيلي - فيزياء الثالث ثانوي</h1>
        <h2>إعداد أ. خديجة عسيري</h2>
    </div>

    <div id="quiz-body">
        <div class="progress-bar" id="progress-text">سؤال 1 من 23</div>
        <div class="question" id="question-text">جاري تحميل السؤال...</div>
        <ul class="options" id="options-container"></ul>
        <button class="btn-next" id="next-btn" onclick="nextQuestion()" style="display:none;">السؤال التالي</button>
    </div>

    <div id="result-container" class="result" style="display:none;"></div>
</div>

<script>
    const quizData = [
        { question: "س1: القوة الكهربائية المتبادلة بين شحنتين تتناسب عكسياً مع:", options: ["مجموع الشحنتين", "حاصل ضرب الشحنتين", "مربع المسافة بينهما", "المسافة بينهما"], correct: 2 },
        { question: "س2: جهاز يستخدم لقياس فرق الجهد الكهربائي:", options: ["الأميتر", "الفولتميتر", "الأومميتر", "الجلڤانوميتر"], correct: 1 },
        { question: "س3: معدل تدفق الطاقة الضوئية من المصدر الضوئي يسمى:", options: ["الاستضاءة", "التدفق الضوئي", "شدة الإضاءة", "التردد"], correct: 1 },
        { question: "س4: السعة الكهربائية للمكثف تُقاس بوحدة:", options: ["الكولوم", "الفاراد", "الأوم", "الفولت"], correct: 1 },
        { question: "س5: النسب الفاصلة بين طاقة المجالات في نظرية الأحزمة تسمى:", options: ["فجوات الطاقة", "حزمة التوصيل", "حزمة التكافؤ", "الموصلية"], correct: 0 },
        { question: "س6: جهاز يحول الطاقة الميكانيكية إلى طاقة كهربائية:", options: ["المحرك الكهربائي", "المولد الكهربائي", "المحول الكهربائي", "الكشاف الكهربائي"], correct: 1 },
        { question: "س7: انحناء الضوء حول الحواجز يسمى:", options: ["الإنكسار", "الإنعكاس", "الحيود", "التداخل"], correct: 2 },
        { question: "س8: المقاومة الكهربائية لسلك تزداد بزيادة:", options: ["مساحة مقطعه", "طوله", "قطره", "درجة برودته"], correct: 1 },
        { question: "س9: مكتشف الحث الكهرومغناطيسي هو العالم:", options: ["كولوم", "أوم", "فاراداي", "تسلا"], correct: 2 },
        { question: "س10: الصورة المتكونة بواسطة المراه المستوية تكون دائماً:", options: ["حقيقية ومكبرة", "خيالية ومعكوسة جانبيًا", "حقيقية ومقلوبة", "خيالية ومكبرة"], correct: 1 },
        { question: "س11: إشعاع أجهزة الميكروويف يعتبر من أنواع الموجات:", options: ["الكهرومغناطيسية", "الميكانيكية", "الصوتية", "الزلازلية"], correct: 0 },
        { question: "س12: القوة المؤثرة في جسيم شحنته (q) يتحرك بسرعة (v) في مجال مغناطيسي (B) تكون أكبر ما يمكن عندما تكون الزاوية بينهما:", options: ["0 درجة", "45 درجة", "90 درجة", "180 درجة"], correct: 2 },
        { question: "س13: يُقاس المجال المغناطيسي بوحدة:", options: ["الواط", "النيوتن", "التسلا", "الأنبير"], correct: 2 },
        { question: "س14: الموجات التي تحتاج لوسط مادي لانتقالها هي الموجات:", options: ["الضوئية", "الكهرومغناطيسية", "الميكانيكية", "الراديوية"], correct: 2 },
        { question: "س15: ناتج قسمة فرق الجهد الكهربائي على شدة التيار يمثل:", options: ["القدرة", "المقاومة الكهربائية", "الشحنة", "الطاقة"], correct: 1 },
        { question: "س16: عند توصيل المقاومات على التوالي فإن الثابت في جميع المقاومات هو:", options: ["شدة التيار", "فرق الجهد", "المقاومة المكافئة", "القدرة المستهلكة"], correct: 0 },
        { question: "س17: أداة تُستخدم لدراسة النظائر وفصل الأيونات بناءً على كتلتها:", options: ["مقياس الفولتميتر", "مطياف الكتلة", "الكشاف الكهربائي", "الميكروسكوب"], correct: 1 },
        { question: "س18: ظاهرة انبعاث إلكترونات عند سقوط ضوء بثرادد مناسب على سطح فلز تسمى:", options: ["التأثير الكهروضوئي", "الحث الذاتي", "الانكسار المزدوج", "حيود الأغشية"], correct: 0 },
        { question: "س19: الفوتون عبارة عن حزمة مركزة من:", options: ["المادة", "الطاقة الكهرومغناطيسية", "الإلكترونات", "البروتونات"], correct: 1 },
        { question: "س20: الشبه موصل من النوع الموجب (p-type) يحتوي على زيادة في:", options: ["الإلكترونات الحرة", "الفجوات", "النيوترونات", "البروتونات"], correct: 1 },
        { question: "س21: رمز الدايود (الوصلة الثنائية) يُستخدم بشكل أساسي في الدوائر لـ:", options: ["تخزين الشحنات", "تقويم التيار المتردد", "زيادة المقاومة", "توليد المجال"], correct: 1 },
        { question: "س22: النموذج الذري الذي افترض أن الإلكترونات تدور في مستويات طاقة محددة هو نموذج:", options: ["رذرفورد", "بور", "طومسون", "دالتون"], correct: 1 },
        { question: "س23: نواة الذرة تحتوي على:", options: ["إلكترونات وفجوات", "بروتونات ونيوترونات", "إلكترونات وبروتونات", "فوتونات ونيوترونات"], correct: 1 }
    ];

    let currentQuestion = 0;
    let score = 0;

    function loadQuestion() {
        const q = quizData[currentQuestion];
        document.getElementById("progress-text").innerText = `سؤال ${currentQuestion + 1} من ${quizData.length}`;
        document.getElementById("question-text").innerText = q.question;
        const optionsContainer = document.getElementById("options-container");
        optionsContainer.innerHTML = "";
        document.getElementById("next-btn").style.display = "none";

        q.options.forEach((opt, index) => {
            const li = document.createElement("li");
            const btn = document.createElement("button");
            btn.innerText = opt;
            btn.onclick = () => selectOption(index, btn);
            li.appendChild(btn);
            optionsContainer.appendChild(li);
        });
    }

    function selectOption(selectedIndex, selectedBtn) {
        const q = quizData[currentQuestion];
        const buttons = document.querySelectorAll(".options button");
        
        buttons.forEach((btn, index) => {
            btn.disabled = true;
            if (index === q.correct) {
                btn.style.backgroundColor = "#d1fae5";
                btn.style.borderColor = "#10b981";
            } else if (index === selectedIndex) {
                btn.style.backgroundColor = "#fee2e2";
                btn.style.borderColor = "#ef4444";
            }
        });

        if (selectedIndex === q.correct) {
            score++;
        }

        document.getElementById("next-btn").style.display = "block";
    }

    function nextQuestion() {
        currentQuestion++;
        if (currentQuestion < quizData.length) {
            loadQuestion();
        } else {
            showResult();
        }
    }

    function showResult() {
        document.getElementById("quiz-body").style.display = "none";
        const resultContainer = document.getElementById("result-container");
        resultContainer.style.display = "block";
        resultContainer.innerHTML = `
            🎉 اكتمل الاختبار!<br><br>
            درجتك هي: ${score} من ${quizData.length}<br><br>
            <small style="font-weight:normal; font-size:14px; color:#6b7280;">إعداد المعلمة: خديجة عسيري</small>
        `;
    }

    loadQuestion();
</script>

</body>
</html>
