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
        .input-group {
            margin-bottom: 20px;
            text-align: right;
        }
        .input-group label {
            display: block;
            font-size: 16px;
            font-weight: bold;
            color: #333;
            margin-bottom: 8px;
        }
        .input-group input {
            width: 100%;
            padding: 12px;
            font-size: 16px;
            border: 1px solid #d1d5db;
            border-radius: 8px;
            box-sizing: border-box;
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
        .btn-action {
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
        .btn-action:hover {
            background-color: #059669;
        }
        .result {
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            color: #1e3a8a;
            line-height: 1.8;
        }
        .results-board {
            margin-top: 30px;
            border-top: 2px dashed #d1d5db;
            padding-top: 20px;
        }
        .results-board h3 {
            color: #1e3a8a;
            font-size: 18px;
            margin-bottom: 15px;
            text-align: center;
        }
        .results-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .results-list li {
            display: flex;
            justify-content: space-between;
            padding: 10px 15px;
            background-color: #f9fafb;
            border: 1px solid #e5e7eb;
            border-radius: 6px;
            margin-bottom: 8px;
            font-size: 15px;
        }
    </style>
</head>
<body>

<div class="quiz-container">
    <div class="header">
        <h1>اختبار تحصيلي - فيزياء الثالث ثانوي</h1>
        <h2>إعداد أ. خديجة عسيري</h2>
    </div>

    <!-- شاشة إدخال اسم الطالبة -->
    <div id="start-screen">
        <div class="input-group">
            <label for="student-name">الرجاء إدخال اسم الطالبة للبدء:</label>
            <input type="text" id="student-name" placeholder="اكتبي اسمك الثلاثي هنا...">
        </div>
        <button class="btn-action" onclick="startQuiz()">بدء الاختبار</button>
    </div>

    <!-- شاشة الأسئلة -->
    <div id="quiz-body" style="display:none;">
        <div class="progress-bar" id="progress-text">سؤال 1 من 23</div>
        <div class="question" id="question-text">جاري تحميل السؤال...</div>
        <ul class="options" id="options-container"></ul>
        <button class="btn-action" id="next-btn" onclick="nextQuestion()" style="display:none;">السؤال التالي</button>
    </div>

    <!-- شاشة النتيجة -->
    <div id="result-container" class="result" style="display:none;"></div>

    <!-- لوحة نتائج الطالبات -->
    <div class="results-board" id="results-board" style="display:none;">
        <h3>سجل نتائج الطالبات</h3>
        <ul class="results-list" id="students-list"></ul>
        <button class="btn-action" onclick="resetQuiz()" style="background-color: #3b82f6; margin-top: 15px;">طالبة جديدة / إعادة الاختبار</button>
    </div>
</div>

<script>
    const quizData = [
        { question: "1. أي الحركات الآتية تمثل حركة توافقية بسيطة؟", options: ["حركة البندول البسيط", "حركة القمر حول الأرض", "حركة سيارة في مضمار سباق", "سقوط الكرة"], correct: 0 },
        { question: "2. طبقًا لقانون هوك، تتناسب القوة المؤثرة في نابض تناسبًا طرديًا مع مقدار…", options: ["سمك النابض", "استطالته", "طوله الأصلي", "كتلته"], correct: 1 },
        { question: "3. بمَ يمثل ميل منحنى القوة في نابض مقابل الإزاحة؟", options: ["طاقة الوضع المرونية", "ثابت النابض", "الشغل المبذول", "كثافة مادة النابض"], correct: 1 },
        { question: "4. يعتمد الزمن الدوري للبندول البسيط على…", options: ["طول خيط البندول", "كتلة ثقل البندول", "سعة الاهتزازة فقط", "حجم ثقل البندول"], correct: 0 },
        { question: "5. عندما تتحرك موجتان بالسرعة نفسها، فإن معدل نقل الطاقة يتناسب طرديًا مع…", options: ["سعة الموجة", "مربع سعة الموجة", "طول الموجة", "زمنها الدوري"], correct: 1 },
        { question: "6. أي الآتي مثال على موجة ميكانيكية؟", options: ["الضوء", "الصوت", "موجات الراديو", "الأشعة السينية"], correct: 1 },
        { question: "7. في الموجة المستعرضة تهتز جسيمات الوسط…", options: ["موازية لاتجاه انتشار الموجة", "عموديًا على اتجاه انتشار الموجة", "في اتجاه دائري دائمًا", "من المصدر إلى المستقبل"], correct: 1 },
        { question: "8. كيف يُصنَّف الضوء؟", options: ["موجة ميكانيكية طولية", "موجة ميكانيكية مستعرضة", "موجة كهرومغناطيسية", "موجة صوتية"], correct: 2 },
        { question: "9. المسافة من خط الاتزان إلى قمة الموجة تمثل…", options: ["سعة الموجة", "طول الموجة", "التردد", "الزمن الدوري"], correct: 0 },
        { question: "10. أقصر مسافة بين قمتين متتاليتين أو قاعين متتاليين تسمى…", options: ["السعة", "التردد", "الطول الموجي", "الزمن الدوري"], correct: 2 },
        { question: "11. الزمن اللازم لإكمال دورة كاملة من الحركة يسمى…", options: ["التردد", "الزمن الدوري", "السعة", "الطول الموجي"], correct: 1 },
        { question: "12. الاهتزازات الكاملة التي تحدث في الثانية الواحدة يمثل…", options: ["الزمن الدوري", "الطور", "الطول الموجي", "التردد"], correct: 3 },
        { question: "13. تنشأ الموجة الموقوفة من تراكب موجتين…", options: ["تتحركان في اتجاهين متعاكسين", "تتحركان في الاتجاه نفسه", "متعامدتين دائمًا", "مختلفتين في الوسط"], correct: 0 },
        { question: "14. في شبكة موجة موقوفة مكونة من حلقتين، كم عدد البطون والعقد؟", options: ["بطن واحد وعقدتان", "بطنان وثلاث عقد", "ثلاثة بطون وعقدتان", "ثلاثة بطون وثلاث عقد"], correct: 1 },
        { question: "15. المسافة بين خمس عقد متتالية في موجة موقوفة تساوي…", options: ["طولًا موجيًا واحدًا", "نصف طول موجي", "طولين موجيين", "أربعة أطوال موجية"], correct: 2 },
        { question: "16. أي أنواع الموجات الآتية تتكون فيها عقد وبطون؟", options: ["موجات الحبل", "موجات الماء فقط", "موجات الصوت في الهواء فقط", "موجات الضوء"], correct: 0 },
        { question: "17. أي العبارات الآتية تصف انتقال الصوت في الهواء وصفًا صحيحًا؟", options: ["ينتج الصوت بسبب تغير درجة الحرارة وينتقل بالضوء", "ينتج الصوت بسبب الاهتزازات وينتقل بتغير ضغط الهواء", "ينتج الصوت بسبب ضغط الهواء ولا يحتاج إلى اهتزاز", "ينتقل الصوت في الهواء من دون اضطراب"], correct: 1 },
        { question: "18. ينتقل الصوت بسرعة أكبر في…", options: ["الفراغ", "الغازات", "السوائل", "المعادن"], correct: 3 },
        { question: "19. مثال على حدوث انعكاس للموجة الصوتية…", options: ["قوس المطر", "الصدى", "الفضاء", "العدسات"], correct: 1 },
        { question: "20. تعتمد حدة الصوت على…", options: ["تردد الصوت", "سعة الصوت", "سرعة الصوت", "زمن انتقاله"], correct: 0 },
        { question: "21. شخص كبير في السن سمع صوتًا حادًا جدًا لأن تردد الصوت…", options: ["أكبر من 8000 Hz", "يساوي 120 dB", "سرعته أكبر من 8000 m/s", "يقع بين 20 Hz و8000 Hz"], correct: 3 },
        { question: "22. وحدة الديسيبل تقيس…", options: ["مستوى الصوت", "شدة الصوت", "تردد الصوت", "الطول الموجي للصوت"], correct: 0 },
        { question: "23. عندما يقترب مصدر صوت من مراقب ساكن، فإن تردد الصوت الذي يسمعه المراقب…", options: ["يقل", "يزداد", "لا يتغير", "يصبح صفرًا"], correct: 1 }
    ];

    let currentQuestion = 0;
    let score = 0;
    let studentName = "";
    let studentsResults = [];

    function startQuiz() {
        const nameInput = document.getElementById("student-name").value.trim();
        if (nameInput === "") {
            alert("يرجى كتابة اسمك قبل البدء بالاختبار!");
            return;
        }
        studentName = nameInput;
        document.getElementById("start-screen").style.display = "none";
        document.getElementById("quiz-body").style.display = "block";
        loadQuestion();
    }

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
            🎉 مبروك إتمام الاختبار يا <strong>${studentName}</strong>!<br>
            درجتك هي: <strong>${score} من ${quizData.length}</strong>
        `;

        studentsResults.push({ name: studentName, score: score });
        updateResultsBoard();
    }

    function updateResultsBoard() {
        const board = document.getElementById("results-board");
        const list = document.getElementById("students-list");
        board.style.display = "block";
        list.innerHTML = "";

        studentsResults.forEach(student => {
            const li = document.createElement("li");
            li.innerHTML = `<span>👤 ${student.name}</span> <span><strong>${student.score}</strong> / ${quizData.length}</span>`;
            list.appendChild(li);
        });
    }

    function resetQuiz() {
        currentQuestion = 0;
        score = 0;
        studentName = "";
        document.getElementById("student-name").value = "";
        document.getElementById("result-container").style.display = "none";
        document.getElementById("results-board").style.display = "none";
        document.getElementById("start-screen").style.display = "block";
    }
</script>

</body>
</html>
