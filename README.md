<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Futuristic Ocean BMI</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600&family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        body {
            font-family: 'Kanit', sans-serif;
            background-color: #e0f2fe; /* light blue base */
            overflow-x: hidden;
            overflow-y: auto;
        }

        /* Ocean Background Gradient */
        .bg-ocean {
            background: linear-gradient(180deg, #e0f2fe 0%, #bae6fd 40%, #7dd3fc 70%, #0ea5e9 100%);
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -3;
        }

        /* Light Rays */
        .light-rays {
            position: fixed;
            top: -20%;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 50% 0%, rgba(255, 255, 255, 0.4) 0%, transparent 60%);
            z-index: -2;
            animation: pulseLight 8s ease-in-out infinite alternate;
        }

        @keyframes pulseLight {
            0% { opacity: 0.5; transform: scale(1); }
            100% { opacity: 0.8; transform: scale(1.1); }
        }

        /* Bubble Particles */
        .bubble {
            position: absolute;
            background: rgba(255, 255, 255, 0.4);
            border-radius: 50%;
            box-shadow: inset 0 0 5px rgba(255, 255, 255, 0.6);
            z-index: -1;
            animation: rise 10s infinite ease-in;
        }

        @keyframes rise {
            0% { bottom: -20px; transform: translateX(0); opacity: 0; }
            20% { opacity: 0.6; }
            100% { bottom: 100vh; transform: translateX(50px); opacity: 0; }
        }

        /* Fish Animation */
        .fish-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -2;
            pointer-events: none;
        }

        .fish {
            position: absolute;
            color: rgba(255, 255, 255, 0.5);
            font-size: 2rem;
            filter: drop-shadow(0 4px 6px rgba(0, 150, 255, 0.3));
        }

        .swim-left {
            animation: swimLeft 25s linear infinite;
        }
        .swim-right {
            animation: swimRight 30s linear infinite;
        }
        .swim-fast {
             animation-duration: 15s !important;
        }

        @keyframes swimLeft {
            from { left: 110%; transform: scaleX(1); }
            to { left: -10%; transform: scaleX(1); }
        }
        @keyframes swimRight {
            from { left: -10%; transform: scaleX(-1); }
            to { left: 110%; transform: scaleX(-1); }
        }

        /* Glassmorphism Card (Light Theme) */
        .glass-card {
            background: rgba(255, 255, 255, 0.4);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.8);
            box-shadow: 
                0 20px 40px rgba(14, 165, 233, 0.15),
                inset 0 0 20px rgba(255, 255, 255, 0.5);
        }

        /* Modern Gradient Button */
        .ocean-btn {
            background: linear-gradient(135deg, #0ea5e9, #22d3ee);
            background-size: 200% auto;
            transition: all 0.4s;
            position: relative;
            overflow: hidden;
        }

        .ocean-btn::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            transition: 0.5s;
        }

        .ocean-btn:hover::after {
            left: 100%;
        }

        .ocean-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(14, 165, 233, 0.4);
        }

        /* Custom Range Slider */
        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: transparent;
        }

        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 24px;
            width: 24px;
            border-radius: 50%;
            background: #ffffff;
            border: 4px solid #0ea5e9;
            cursor: pointer;
            margin-top: -8px;
            box-shadow: 0 0 10px rgba(14, 165, 233, 0.3);
            transition: all 0.2s;
        }

        input[type=range]::-webkit-slider-thumb:hover {
            transform: scale(1.1);
            box-shadow: 0 0 15px rgba(14, 165, 233, 0.5);
        }

        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 8px;
            cursor: pointer;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 4px;
            border: 1px solid rgba(255, 255, 255, 0.8);
        }

        input[type=range]:focus {
            outline: none;
        }

        /* Result Animation */
        .result-panel {
            max-height: 0;
            opacity: 0;
            overflow: hidden;
            transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .result-panel.active {
            max-height: 600px;
            opacity: 1;
            margin-top: 2rem;
        }

        /* Gauge Bar */
        .gauge-container {
            height: 14px;
            background: rgba(14, 165, 233, 0.1);
            border-radius: 999px;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.5);
        }

        .gauge-fill {
            height: 100%;
            width: 0%;
            transition: width 1s ease-out;
            border-radius: 999px;
            /* Shimmer effect on bar */
            background-image: linear-gradient(45deg,rgba(255,255,255,.15) 25%,transparent 25%,transparent 50%,rgba(255,255,255,.15) 50%,rgba(255,255,255,.15) 75%,transparent 75%,transparent);
            background-size: 1rem 1rem;
        }

        .orbitron {
            font-family: 'Orbitron', sans-serif;
        }
        
        .floating-icon {
            animation: float 4s ease-in-out infinite;
        }
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #e0f2fe; }
        ::-webkit-scrollbar-thumb { background: #7dd3fc; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #38bdf8; }

    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4 text-slate-700">

    <!-- Background Elements -->
    <div class="bg-ocean"></div>
    <div class="light-rays"></div>
    
    <!-- Bubbles -->
    <div id="bubbles-container"></div>

    <!-- Fish (Decorations) -->
    <div class="fish-container">
        <!-- Top Left -->
        <i class="fa-solid fa-fish fish swim-right text-cyan-200" style="top: 15%; animation-duration: 25s; opacity: 0.6;"></i>
        <!-- Middle Right -->
        <i class="fa-solid fa-fish-fins fish swim-left text-blue-200" style="top: 40%; animation-duration: 35s; font-size: 1.5rem; opacity: 0.4;"></i>
        <!-- Bottom Left -->
        <i class="fa-solid fa-shrimp fish swim-right text-pink-200" style="top: 75%; animation-duration: 40s; font-size: 1rem; opacity: 0.3;"></i>
        <!-- Deep Fish -->
        <i class="fa-solid fa-fish fish swim-left text-white" style="top: 85%; animation-duration: 45s; font-size: 3rem; opacity: 0.1;"></i>
        <!-- Extra Fish 1 -->
        <i class="fa-solid fa-fish fish swim-right text-cyan-400" style="top: 10%; animation-duration: 20s; font-size: 0.8rem; opacity: 0.5;"></i>
        <!-- Extra Fish 2 -->
        <i class="fa-solid fa-fish-fins fish swim-left text-teal-200" style="top: 60%; animation-duration: 28s; font-size: 1.2rem; opacity: 0.3;"></i>
        <!-- Fast Fish -->
        <i class="fa-solid fa-fish fish swim-right swim-fast text-sky-300" style="top: 30%; font-size: 1rem; opacity: 0.4;"></i>
    </div>

    <!-- Main Container -->
    <main class="w-full max-w-md glass-card rounded-3xl p-8 relative z-10 my-10">
        
        <!-- Header -->
        <header class="text-center mb-8">
            <div class="w-24 h-24 mx-auto mb-4 bg-gradient-to-tr from-cyan-100 to-white rounded-full flex items-center justify-center shadow-xl shadow-cyan-500/20 floating-icon border-4 border-white/50">
                <i class="fa-solid fa-scale-balanced text-4xl text-cyan-500"></i>
            </div>
            <h1 class="text-3xl font-bold tracking-wider orbitron text-slate-800 drop-shadow-sm">
                BMI <span class="text-cyan-600">OCEAN</span>
            </h1>
            <p class="text-slate-500 text-sm mt-2 font-light">คำนวณดัชนีมวลกายสไตล์อนาคต</p>
        </header>

        <!-- Input Form with Sliders -->
        <div class="space-y-8">
            <!-- Height Slider -->
            <div class="bg-white/30 p-4 rounded-2xl border border-white/40 shadow-sm">
                <div class="flex justify-between items-center mb-4">
                    <label class="text-cyan-800 text-xs uppercase font-bold tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-ruler-vertical text-cyan-600"></i> ส่วนสูง
                    </label>
                    <div class="flex items-baseline gap-1">
                        <span id="heightVal" class="text-2xl font-bold orbitron text-cyan-700">170</span>
                        <span class="text-xs text-slate-500">cm</span>
                    </div>
                </div>
                <input type="range" id="height" min="100" max="250" value="170" 
                    class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
                    oninput="updateVal('height', 'heightVal')">
            </div>

            <!-- Weight Slider -->
            <div class="bg-white/30 p-4 rounded-2xl border border-white/40 shadow-sm">
                <div class="flex justify-between items-center mb-4">
                    <label class="text-blue-800 text-xs uppercase font-bold tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-weight-scale text-blue-600"></i> น้ำหนัก
                    </label>
                    <div class="flex items-baseline gap-1">
                        <span id="weightVal" class="text-2xl font-bold orbitron text-blue-700">65</span>
                        <span class="text-xs text-slate-500">kg</span>
                    </div>
                </div>
                <input type="range" id="weight" min="20" max="200" value="65" 
                    class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer"
                    oninput="updateVal('weight', 'weightVal')">
            </div>

            <!-- Button -->
            <button onclick="calculateBMI()" 
                class="ocean-btn w-full py-4 rounded-2xl font-bold text-white text-lg tracking-wide shadow-lg shadow-cyan-500/30 mt-6 flex items-center justify-center gap-2">
                <span>คำนวณผลลัพธ์</span>
                <i class="fa-solid fa-water text-sm"></i>
            </button>
        </div>

        <!-- Result Section -->
        <div id="resultPanel" class="result-panel bg-white/40 rounded-2xl p-6 border border-white/60 shadow-inner">
            <div class="text-center relative">
                <!-- Watermark Icon -->
                <i class="fa-solid fa-heart-pulse absolute top-0 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-9xl text-cyan-900/5 -z-10"></i>

                <p class="text-slate-500 text-xs mb-1 uppercase tracking-widest font-semibold">ค่า BMI ของคุณ</p>
                <h2 id="bmiValue" class="text-6xl font-bold orbitron text-slate-800 mb-2 drop-shadow-sm">
                    0.0
                </h2>
                <div class="inline-block px-4 py-1 rounded-full bg-white/50 backdrop-blur-sm shadow-sm mb-4">
                    <p id="bmiStatus" class="text-lg font-bold">
                        -
                    </p>
                </div>
            </div>

            <!-- Visual Gauge -->
            <div class="relative pt-2 mb-6">
                <div class="flex justify-between text-[10px] text-slate-500 mb-2 font-mono font-bold uppercase">
                    <span>Thin</span>
                    <span>Normal</span>
                    <span>Over</span>
                    <span>Obese</span>
                </div>
                <div class="gauge-container shadow-inner">
                    <div id="gaugeBar" class="gauge-fill bg-cyan-500"></div>
                </div>
            </div>

            <!-- Recommendation -->
            <div class="bg-white/60 rounded-xl p-4 border border-white/50 shadow-sm">
                <div class="flex items-start gap-4">
                    <div id="adviceIcon" class="w-10 h-10 rounded-full bg-cyan-100 flex items-center justify-center flex-shrink-0 text-cyan-600 shadow-sm">
                        <i class="fa-solid fa-info text-sm"></i>
                    </div>
                    <div>
                        <h4 class="text-sm font-bold text-slate-800">คำแนะนำสุขภาพ</h4>
                        <p id="adviceText" class="text-xs text-slate-600 mt-1 leading-relaxed">
                            กรุณากรอกข้อมูลเพื่อรับคำแนะนำ
                        </p>
                    </div>
                </div>
            </div>
        </div>

    </main>

    <footer class="fixed bottom-4 text-center w-full text-xs text-cyan-800/40 z-0">
        &copy; 2024 Blue Ocean Health.
    </footer>

    <script>
        // Create Bubbles dynamically
        function createBubbles() {
            const container = document.getElementById('bubbles-container');
            const bubbleCount = 15;

            for (let i = 0; i < bubbleCount; i++) {
                const bubble = document.createElement('div');
                bubble.classList.add('bubble');
                
                // Randomize position and size
                const size = Math.random() * 20 + 10 + 'px';
                bubble.style.width = size;
                bubble.style.height = size;
                bubble.style.left = Math.random() * 100 + '%';
                
                // Randomize animation delay and duration
                bubble.style.animationDuration = Math.random() * 5 + 5 + 's';
                bubble.style.animationDelay = Math.random() * 5 + 's';

                container.appendChild(bubble);
            }
        }
        
        // Initial Bubbles
        createBubbles();

        function updateVal(inputId, displayId) {
            const val = document.getElementById(inputId).value;
            document.getElementById(displayId).innerText = val;
        }

        function calculateBMI() {
            const heightInput = document.getElementById('height').value;
            const weightInput = document.getElementById('weight').value;
            const resultPanel = document.getElementById('resultPanel');
            const bmiValueDisplay = document.getElementById('bmiValue');
            const bmiStatusDisplay = document.getElementById('bmiStatus');
            const gaugeBar = document.getElementById('gaugeBar');
            const adviceText = document.getElementById('adviceText');
            const adviceIcon = document.getElementById('adviceIcon');

            // Calculation
            const heightInMeters = heightInput / 100;
            const bmi = (weightInput / (heightInMeters * heightInMeters)).toFixed(1);

            // Determine Status & Styles
            let status = '';
            let colorHex = ''; // For gauge background
            let textColorClass = ''; // For status text
            let widthPercentage = '0%';
            let advice = '';
            let iconClass = '';
            let iconBgClass = '';

            if (bmi < 18.5) {
                status = 'น้ำหนักน้อย / ผอม';
                colorHex = '#38bdf8'; // Sky Blue
                textColorClass = 'text-sky-500';
                widthPercentage = '20%';
                advice = 'น้ำหนักน้อยกว่าเกณฑ์ ควรทานอาหารให้ครบ 5 หมู่ เพิ่มปริมาณโปรตีนและแป้งอย่างเหมาะสม';
                iconClass = 'fa-feather';
                iconBgClass = 'bg-sky-100 text-sky-500';
            } else if (bmi >= 18.5 && bmi <= 22.9) {
                status = 'สมส่วน (ปกติ)';
                colorHex = '#22c55e'; // Green
                textColorClass = 'text-green-500';
                widthPercentage = '45%';
                advice = 'ยอดเยี่ยม! สุขภาพร่างกายสมบูรณ์แข็งแรง ควรรักษาระดับนี้ไว้ด้วยการออกกำลังกายสม่ำเสมอ';
                iconClass = 'fa-thumbs-up';
                iconBgClass = 'bg-green-100 text-green-500';
            } else if (bmi >= 23.0 && bmi <= 24.9) {
                status = 'น้ำหนักเกิน (ท้วม)';
                colorHex = '#fbbf24'; // Amber
                textColorClass = 'text-amber-500';
                widthPercentage = '70%';
                advice = 'เริ่มมีน้ำหนักเกินมาตรฐานเล็กน้อย ควรลดของหวาน ของมัน และเริ่มออกกำลังกายเบาๆ';
                iconClass = 'fa-person-walking';
                iconBgClass = 'bg-amber-100 text-amber-500';
            } else if (bmi >= 25.0 && bmi <= 29.9) {
                status = 'โรคอ้วนระดับ 1';
                colorHex = '#f97316'; // Orange
                textColorClass = 'text-orange-500';
                widthPercentage = '85%';
                advice = 'มีความเสี่ยงต่อโรค ควรควบคุมอาหารอย่างจริงจัง ลดแป้งและน้ำตาล และออกกำลังกายสัปดาห์ละ 150 นาที';
                iconClass = 'fa-triangle-exclamation';
                iconBgClass = 'bg-orange-100 text-orange-500';
            } else {
                status = 'โรคอ้วนระดับ 2 (อันตราย)';
                colorHex = '#ef4444'; // Red
                textColorClass = 'text-red-500';
                widthPercentage = '100%';
                advice = 'อันตรายต่อสุขภาพมาก เสี่ยงต่อเบาหวานและความดันสูง ควรปรึกษาแพทย์เพื่อวางแผนลดน้ำหนักทันที';
                iconClass = 'fa-user-doctor';
                iconBgClass = 'bg-red-100 text-red-500';
            }

            // Update UI
            bmiValueDisplay.innerText = bmi;
            
            bmiStatusDisplay.innerText = status;
            bmiStatusDisplay.className = `text-lg font-bold ${textColorClass}`;
            
            // Animate Gauge
            gaugeBar.style.width = widthPercentage;
            gaugeBar.style.backgroundColor = colorHex;
            gaugeBar.style.boxShadow = `0 0 10px ${colorHex}80`; // Add glow

            // Update Advice
            adviceText.innerText = advice;
            
            // Update Icon
            adviceIcon.className = `w-10 h-10 rounded-full flex items-center justify-center flex-shrink-0 shadow-sm transition-colors duration-300 ${iconBgClass}`;
            adviceIcon.innerHTML = `<i class="fa-solid ${iconClass} text-sm"></i>`;

            // Reveal Panel
            resultPanel.classList.add('active');

            // Smooth Scroll (Mobile friendly)
            setTimeout(() => {
                const yOffset = -20; 
                const y = resultPanel.getBoundingClientRect().top + window.pageYOffset + yOffset;
                window.scrollTo({top: y, behavior: 'smooth'});
            }, 100);
        }
    </script>
</body>
</html>
