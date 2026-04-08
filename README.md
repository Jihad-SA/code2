<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام كشف المسافات وتجنب التصادم للسيارات ذاتية القيادة</title>
    
    <!-- تحميل مكتبة Tailwind CSS للتصميم -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- تحميل مكتبة Chart.js للرسوم البيانية -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <!-- خطوط جوجل (Cairo) لدعم اللغة العربية بشكل جميل -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #f8fafc;
            color: #0f172a;
        }
        
        /* تنسيق حاوية الرسم البياني لضمان التجاوب (Responsive) */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }

        /* حركات مخصصة (Animations) */
        @keyframes pulse-border {
            0% { border-color: rgba(59, 130, 246, 0.5); }
            50% { border-color: rgba(59, 130, 246, 1); }
            100% { border-color: rgba(59, 130, 246, 0.5); }
        }
        .scanning-effect {
            animation: pulse-border 2s infinite;
        }

        /* تنسيق حاوية المحاكاة (Canvas) */
        .sim-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin: 0 auto;
            height: 400px;
            background-color: #1e293b;
            border-radius: 0.5rem;
            overflow: hidden;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }
        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }

        /* تنسيق التبويب النشط */
        .tab-active {
            border-bottom: 2px solid #2563eb;
            color: #2563eb;
            font-weight: 700;
        }
    </style>
</head>
<body class="antialiased pb-12">

    <!-- الجزء العلوي (Header) -->
    <header class="bg-white shadow-sm border-b border-gray-200 pt-8 pb-6 px-4 mb-8 text-center">
        <div class="max-w-6xl mx-auto">
            <div class="inline-block p-3 rounded-full bg-blue-100 text-blue-600 mb-4 text-2xl font-bold">
                &#128663;
            </div>
            <h1 class="text-3xl md:text-4xl font-extrabold tracking-tight text-gray-900 mb-2">
                نظام كشف المسافات وتجنب التصادم
            </h1>
            <p class="text-lg text-gray-600 max-w-2xl mx-auto">
                مشروع متكامل للسيارات ذاتية القيادة يعتمد على الذكاء الاصطناعي لتمييز الأجسام لحظياً وتقدير المسافات الآمنة لضمان قيادة خالية من الحوادث.
            </p>
        </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 space-y-12">

        <!-- القسم الأول: نظرة عامة -->
        <section class="bg-white rounded-xl shadow-sm border border-gray-100 p-6 md:p-8">
            <h2 class="text-2xl font-bold mb-4 border-r-4 border-blue-500 pr-3">نظرة عامة على النظام</h2>
            <p class="text-gray-700 leading-relaxed mb-6">
                يقدم هذا القسم فهماً مبسطاً لدور النظام. في بيئة القيادة الذاتية، يجب على المركبة ألا تكتفي برؤية الأشياء، بل يجب أن تفهم ماهيتها ومدى بعدها، وأن تتتبع حركتها لاتخاذ قرارات التوقف أو التوجيه في أجزاء من الثانية. يدمج هذا المشروع بين أحدث خوارزميات الرؤية الحاسوبية لتحقيق هذا الهدف.
            </p>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-center">
                <div class="p-4 bg-gray-50 rounded-lg border border-gray-100">
                    <div class="text-3xl mb-2">&#128065;</div>
                    <h3 class="font-bold text-lg mb-1">الإدراك (الرؤية)</h3>
                    <p class="text-sm text-gray-500">التعرف على السيارات، المشاة، والعوائق في الوقت الفعلي.</p>
                </div>
                <div class="p-4 bg-gray-50 rounded-lg border border-gray-100">
                    <div class="text-3xl mb-2">&#128207;</div>
                    <h3 class="font-bold text-lg mb-1">القياس (المسافة)</h3>
                    <p class="text-sm text-gray-500">حساب المسافة الدقيقة بين السيارة والعوائق المحيطة.</p>
                </div>
                <div class="p-4 bg-gray-50 rounded-lg border border-gray-100">
                    <div class="text-3xl mb-2">&#9888;</div>
                    <h3 class="font-bold text-lg mb-1">القرار (التجنب)</h3>
                    <p class="text-sm text-gray-500">إصدار تحذيرات أو أوامر توقف بناءً على مسافة الأمان.</p>
                </div>
            </div>
        </section>

        <!-- القسم الثاني: التقنيات الأساسية (تبويبات) -->
        <section class="bg-white rounded-xl shadow-sm border border-gray-100 overflow-hidden">
            <div class="p-6 md:p-8 border-b border-gray-100">
                <h2 class="text-2xl font-bold mb-2 border-r-4 border-blue-500 pr-3">التقنيات الأساسية المستخدمة</h2>
                <p class="text-gray-600">تعرف على النماذج والخوارزميات التي تشغل عقل النظام. انقر على التبويبات أدناه لاستكشاف كل تقنية.</p>
            </div>
            
            <div class="flex flex-wrap md:flex-nowrap border-b border-gray-200 bg-gray-50">
                <button class="tab-btn w-full md:flex-1 py-4 px-4 text-center font-semibold text-gray-500 hover:bg-gray-100 transition tab-active" data-target="tech-yolo">
                    YOLOv8
                </button>
                <button class="tab-btn w-full md:flex-1 py-4 px-4 text-center font-semibold text-gray-500 hover:bg-gray-100 transition" data-target="tech-stereo">
                    Stereo Vision
                </button>
                <button class="tab-btn w-full md:flex-1 py-4 px-4 text-center font-semibold text-gray-500 hover:bg-gray-100 transition" data-target="tech-deepsort">
                    DeepSORT
                </button>
            </div>

            <div class="p-6 md:p-8 min-h-[250px]">
                <!-- محتوى YOLO -->
                <div id="tech-yolo" class="tab-content block animate-fade-in">
                    <div class="flex flex-col md:flex-row gap-6 items-center">
                        <div class="flex-1">
                            <h3 class="text-xl font-bold text-blue-600 mb-2">تمييز الأجسام لحظياً (Object Detection)</h3>
                            <p class="text-gray-700 leading-relaxed mb-4">
                                يُستخدم نموذج <strong>YOLOv8</strong> (You Only Look Once النسخة 8) كعصب رئيسي للإدراك. يتميز بسرعته الفائقة ودقته العالية في استخراج الميزات (Features) من الصور وتحديد أماكن الأجسام ورسم "مربعات إحاطة" (Bounding Boxes) حولها وتصنيفها (سيارة، إنسان، دراجة) في إطار زمني لا يتعدى أجزاء من الثانية.
                            </p>
                        </div>
                        <div class="w-full md:w-1/3 h-48 bg-gray-900 rounded-lg relative flex items-center justify-center border-4 scanning-effect overflow-hidden">
                            <div class="text-white text-4xl opacity-20">&#128663;</div>
                            <div class="absolute border-2 border-green-500 w-24 h-16 top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex items-start">
                                <span class="bg-green-500 text-white text-xs px-1 font-bold">Car 0.95</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- محتوى Stereo Vision -->
                <div id="tech-stereo" class="tab-content hidden animate-fade-in">
                    <div class="flex flex-col md:flex-row gap-6 items-center">
                        <div class="flex-1">
                            <h3 class="text-xl font-bold text-blue-600 mb-2">تقدير المسافة (Depth Estimation)</h3>
                            <p class="text-gray-700 leading-relaxed mb-4">
                                تعتمد تقنية <strong>Stereo Vision</strong> على محاكاة الرؤية البشرية باستخدام كاميرتين بمسافة معلومة بينهما. من خلال إيجاد الفروق (Disparity) في موقع نفس الجسم بين الصورتين (اليمنى واليسرى)، يقوم النظام بحساب العمق والمسافة الفعلية بين السيارة والعائق بوحدة المتر.
                            </p>
                        </div>
                        <div class="w-full md:w-1/3 flex gap-2 justify-center items-center">
                            <div class="w-20 h-20 bg-gray-200 border-2 border-gray-400 rounded flex items-center justify-center relative">
                                <span class="text-xs absolute top-1 left-1" dir="ltr">L-Cam</span>
                                <div class="w-4 h-4 bg-red-500 rounded-full ml-4"></div>
                            </div>
                            <div class="text-2xl font-bold text-gray-400">&#8594;</div>
                            <div class="w-20 h-20 bg-gray-200 border-2 border-gray-400 rounded flex items-center justify-center relative">
                                <span class="text-xs absolute top-1 right-1" dir="ltr">R-Cam</span>
                                <div class="w-4 h-4 bg-red-500 rounded-full mr-4"></div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- محتوى DeepSORT -->
                <div id="tech-deepsort" class="tab-content hidden animate-fade-in">
                    <div class="flex flex-col md:flex-row gap-6 items-center">
                        <div class="flex-1">
                            <h3 class="text-xl font-bold text-blue-600 mb-2">تتبع الأجسام (Object Tracking)</h3>
                            <p class="text-gray-700 leading-relaxed mb-4">
                                بينما يكتشف YOLO الأجسام في كل إطار (Frame) بشكل منفصل، يأتي دور <strong>DeepSORT</strong> لربط هذه الأجسام عبر الزمن. يعطي النظام معرّفاً فريداً (ID) لكل عائق، ويتوقع مسار حركته. هذا ضروري لعدم فقدان العوائق إذا تم حجبها مؤقتاً ولتحديد ما إذا كان العائق يقترب أم يبتعد.
                            </p>
                        </div>
                        <div class="w-full md:w-1/3 flex flex-col gap-2 items-center" dir="ltr">
                            <div class="flex items-center gap-2">
                                <span class="text-xs text-gray-500">t=1</span>
                                <div class="border border-blue-500 px-2 py-1 text-xs bg-blue-50 rounded">ID: 45</div>
                            </div>
                            <div class="h-4 border-r-2 border-dashed border-gray-300 mr-8"></div>
                            <div class="flex items-center gap-2">
                                <span class="text-xs text-gray-500">t=2</span>
                                <div class="border border-blue-500 px-2 py-1 text-xs bg-blue-50 rounded translate-x-2">ID: 45</div>
                            </div>
                            <div class="h-4 border-r-2 border-dashed border-gray-300 mr-8 translate-x-2"></div>
                            <div class="flex items-center gap-2">
                                <span class="text-xs text-gray-500">t=3</span>
                                <div class="border border-blue-500 px-2 py-1 text-xs bg-blue-50 rounded translate-x-4">ID: 45</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- القسم الثالث: محاكاة النظام التفاعلية -->
        <section class="bg-white rounded-xl shadow-sm border border-gray-100 p-6 md:p-8">
            <h2 class="text-2xl font-bold mb-4 border-r-4 border-blue-500 pr-3">محاكاة النظام التفاعلية</h2>
            <p class="text-gray-700 leading-relaxed mb-6">
                استخدم الشريط المنزلق أدناه لتغيير مسافة العائق الوهمي. لاحظ كيف تتفاعل خوارزميات الرؤية لتحديث البيانات وإصدار التحذيرات المناسبة لتجنب التصادم بناءً على المسافة المحسوبة.
            </p>

            <!-- لوحة التحكم وحالة النظام -->
            <div class="bg-gray-50 p-4 rounded-lg mb-6 flex flex-col md:flex-row items-center justify-between gap-4 border border-gray-200">
                <div class="w-full md:w-1/2">
                    <label for="distanceSlider" class="block text-sm font-medium text-gray-700 mb-2">
                        المسافة بين سيارتك والعائق: <span id="distanceValueDisplay" class="font-bold text-blue-600 text-lg">20</span> متر
                    </label>
                    <input type="range" id="distanceSlider" min="1" max="30" value="20" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer accent-blue-600" dir="ltr">
                </div>
                
                <!-- مخرجات النظام -->
                <div class="w-full md:w-auto min-w-[200px] text-center p-3 rounded-lg border-2 bg-white" id="statusPanel">
                    <div class="text-sm text-gray-500 font-bold mb-1">حالة النظام</div>
                    <div id="statusText" class="text-xl font-extrabold text-green-600">مسار آمن</div>
                    <div id="actionText" class="text-sm text-gray-600 mt-1">متابعة القيادة</div>
                </div>
            </div>

            <!-- حاوية لوحة الرسم (Canvas) -->
            <div class="sim-container relative">
                <!-- تفاصيل محاكاة شاشة العرض (HUD) -->
                <div class="absolute top-4 right-4 bg-black bg-opacity-60 text-white text-xs p-2 rounded font-mono z-10" dir="ltr" style="text-align: left;">
                    <div>Model: YOLOv8_nano</div>
                    <div>FPS: <span id="fpsCounter">30</span></div>
                    <div>Tracker: DeepSORT</div>
                </div>
                <!-- مساحة الرسم الفعلي -->
                <canvas id="simCanvas"></canvas>
            </div>
        </section>

        <!-- القسم الرابع: البيانات والتدريب -->
        <section class="bg-white rounded-xl shadow-sm border border-gray-100 p-6 md:p-8 mb-8">
            <div class="flex flex-col md:flex-row gap-8 items-center">
                <div class="w-full md:w-1/2">
                    <h2 class="text-2xl font-bold mb-4 border-r-4 border-blue-500 pr-3">بيانات التدريب (Dataset)</h2>
                    <p class="text-gray-700 leading-relaxed mb-4">
                        لضمان عمل النظام في مختلف البيئات والظروف، تم تدريب نماذج YOLOv8 باستخدام مجموعة بيانات هجينة لتوفير الشمولية والدقة:
                    </p>
                    <ul class="space-y-3 mb-4 list-none">
                        <li class="flex items-start">
                            <span class="text-blue-500 mr-2 ml-2">&#10004;</span>
                            <div>
                                <strong>COCO Dataset:</strong> مجموعة بيانات معيارية ضخمة وفرت أساساً قوياً لتمييز الأجسام العامة في الشوارع.
                            </div>
                        </li>
                        <li class="flex items-start">
                            <span class="text-blue-500 mr-2 ml-2">&#10004;</span>
                            <div>
                                <strong>Custom Dataset:</strong> بيانات مخصصة تم جمعها لسيناريوهات محلية أو عوائق غير اعتيادية لتحسين دقة النظام.
                            </div>
                        </li>
                    </ul>
                </div>
                <div class="w-full md:w-1/2 flex justify-center">
                    <!-- حاوية Chart.js -->
                    <div class="chart-container">
                        <canvas id="datasetChart"></canvas>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- الأكواد البرمجية (JavaScript) -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            
            /* --- منطق التنقل بين التبويبات --- */
            const tabs = document.querySelectorAll('.tab-btn');
            const contents = document.querySelectorAll('.tab-content');

            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    // إزالة التنسيق النشط من الكل
                    tabs.forEach(t => t.classList.remove('tab-active'));
                    contents.forEach(c => c.classList.add('hidden'));

                    // إضافة التنسيق النشط للعنصر المضغوط
                    tab.classList.add('tab-active');
                    const targetId = tab.getAttribute('data-target');
                    document.getElementById(targetId).classList.remove('hidden');
                });
            });

            /* --- منطق محاكاة النظام التفاعلية (Canvas) --- */
            const canvas = document.getElementById('simCanvas');
            const ctx = canvas.getContext('2d');
            const slider = document.getElementById('distanceSlider');
            const distanceDisplay = document.getElementById('distanceValueDisplay');
            const statusPanel = document.getElementById('statusPanel');
            const statusText = document.getElementById('statusText');
            const actionText = document.getElementById('actionText');
            const fpsCounter = document.getElementById('fpsCounter');

            // تعديل حجم اللوحة تلقائياً لتناسب الشاشات
            function resizeCanvas() {
                canvas.width = canvas.parentElement.clientWidth;
                canvas.height = canvas.parentElement.clientHeight;
                drawSimulation();
            }
            window.addEventListener('resize', resizeCanvas);

            // حالة المحاكاة
            let currentDistance = parseInt(slider.value); // المسافة الافتراضية بالمتر
            let frameCount = 0;
            let lastTime = performance.now();

            // الألوان المعبرة عن الحالات
            const colors = {
                safe: { bg: '#dcfce7', border: '#22c55e', text: '#16a34a', msg: 'مسار آمن', action: 'متابعة القيادة' },
                warning: { bg: '#fef08a', border: '#eab308', text: '#ca8a04', msg: 'تحذير اقتراب', action: 'استعد للفرملة' },
                danger: { bg: '#fee2e2', border: '#ef4444', text: '#dc2626', msg: 'خطر تصادم!', action: 'فرملة طارئة (AEB)' }
            };

            // حساب حالة النظام بناءً على المسافة
            function getSystemState(distance) {
                if (distance > 15) return colors.safe;
                if (distance > 7) return colors.warning;
                return colors.danger;
            }

            // رسم إطار المحاكاة
            function drawSimulation() {
                const width = canvas.width;
                const height = canvas.height;
                const state = getSystemState(currentDistance);

                // 1. رسم الخلفية (السماء والشارع)
                ctx.fillStyle = '#87CEEB'; // السماء
                ctx.fillRect(0, 0, width, height * 0.5);
                
                ctx.fillStyle = '#555'; // الشارع
                ctx.beginPath();
                ctx.moveTo(0, height);
                ctx.lineTo(width * 0.4, height * 0.5);
                ctx.lineTo(width * 0.6, height * 0.5);
                ctx.lineTo(width, height);
                ctx.fill();

                // خطوط الشارع (متقطعة)
                ctx.strokeStyle = '#fff';
                ctx.lineWidth = 4;
                ctx.setLineDash([20, 20]);
                ctx.beginPath();
                ctx.moveTo(width / 2, height);
                ctx.lineTo(width / 2, height * 0.5);
                ctx.stroke();
                ctx.setLineDash([]); // إعادة التعيين

                // 2. رسم العائق (السيارة)
                // حجم السيارة يتناسب عكسياً مع المسافة
                const scale = 1 / (currentDistance * 0.1); 
                const objWidth = 100 * scale;
                const objHeight = 80 * scale;
                const objX = (width / 2) - (objWidth / 2); // التوسيط الأفقي
                const objY = (height * 0.5) + (height * 0.5 - objHeight) * (1 - currentDistance/30) - (objHeight/2); // المنظور

                // رسم هيكل السيارة
                ctx.fillStyle = '#333';
                ctx.fillRect(objX, objY, objWidth, objHeight);
                ctx.fillStyle = '#cc0000'; // المصابيح الخلفية
                ctx.fillRect(objX + objWidth*0.1, objY + objHeight*0.6, objWidth*0.2, objHeight*0.2);
                ctx.fillRect(objX + objWidth*0.7, objY + objHeight*0.6, objWidth*0.2, objHeight*0.2);


                // 3. رسم طبقات YOLO و Stereo Vision (المربعات)
                // محاكاة اهتزاز طفيف لمربع التحديد لجعله يبدو واقعياً
                const jitterX = (Math.random() - 0.5) * 2;
                const jitterY = (Math.random() - 0.5) * 2;
                
                const boxX = objX - 5 + jitterX;
                const boxY = objY - 5 + jitterY;
                const boxW = objWidth + 10;
                const boxH = objHeight + 10;

                // مربع الإحاطة (Bounding Box)
                ctx.strokeStyle = state.border;
                ctx.lineWidth = 3;
                ctx.strokeRect(boxX, boxY, boxW, boxH);

                // خلفية النص المرفق
                ctx.fillStyle = state.border;
                const labelHeight = Math.max(16, 20 * scale); 
                ctx.fillRect(boxX, boxY - labelHeight, boxW, labelHeight);

                // النص (الصنف + المعرف + المسافة)
                ctx.fillStyle = '#fff';
                ctx.font = `bold ${Math.max(10, 12 * scale)}px monospace`;
                ctx.textAlign = 'left';
                ctx.textBaseline = 'middle';
                const labelText = `Car ID:12 | Dist: ${currentDistance}m`;
                ctx.fillText(labelText, boxX + 2, boxY - labelHeight/2);

                // 4. تحديث واجهة المستخدم وحالة النظام
                statusPanel.style.borderColor = state.border;
                statusPanel.style.backgroundColor = state.bg;
                statusText.style.color = state.text;
                statusText.innerText = state.msg;
                actionText.innerText = state.action;

                // حساب الإطارات في الثانية (FPS)
                frameCount++;
                const now = performance.now();
                if (now - lastTime >= 1000) {
                    fpsCounter.innerText = frameCount;
                    frameCount = 0;
                    lastTime = now;
                }

                requestAnimationFrame(drawSimulation);
            }

            // مراقبة حركة الشريط المنزلق
            slider.addEventListener('input', (e) => {
                currentDistance = parseInt(e.target.value);
                distanceDisplay.innerText = currentDistance;
            });

            // بدء المحاكاة
            resizeCanvas(); 


            /* --- منطق Chart.js لرسم بيانات التدريب --- */
            const ctxChart = document.getElementById('datasetChart').getContext('2d');
            new Chart(ctxChart, {
                type: 'doughnut',
                data: {
                    labels: ['COCO Dataset (عامة)', 'Custom Dataset (مخصصة)'],
                    datasets: [{
                        data: [75, 25],
                        backgroundColor: [
                            '#3b82f6', // أزرق
                            '#10b981'  // أخضر
                        ],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '65%',
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                font: {
                                    family: "'Cairo', sans-serif"
                                }
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return ' ' + context.label + ': ' + context.parsed + '%';
                                }
                            },
                            bodyFont: {
                                family: "'Cairo', sans-serif"
                            }
                        }
                    }
                }
            });

        });
    </script>
</body>
</html>
