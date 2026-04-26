<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arapça Pro - Öğren & Pratik Yap</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root {
            --primary: #1a5f7a;
            --secondary: #159895;
            --accent: #eb455f;
            --light: #f8f9fa;
            --dark: #2c3333;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #eef2f3;
            margin: 0;
            color: var(--dark);
        }
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 2rem;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        .container { max-width: 1000px; margin: 2rem auto; padding: 0 20px; }
        
        .section-card {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
        }

        /* Konuşma Alanı */
        .speech-box {
            text-align: center;
            padding: 30px;
            border: 2px dashed var(--secondary);
            border-radius: 15px;
            background: #f0fdfa;
        }
        .mic-btn {
            background: var(--accent);
            color: white;
            border: none;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            font-size: 24px;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 10px rgba(235, 69, 95, 0.3);
        }
        .mic-btn.recording {
            animation: pulse 1.5s infinite;
            background: #950101;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        /* Oyun Alanı */
        .grid-game {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 15px;
        }
        .flash-card {
            background: white;
            border: 1px solid #ddd;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            transition: 0.3s;
            font-weight: bold;
        }
        .flash-card:hover { border-color: var(--secondary); transform: translateY(-5px); }
        
        /* Diyaloglar */
        .dialogue-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 15px;
            border-bottom: 1px solid #eee;
        }
        .ar-text { font-size: 1.4rem; color: var(--primary); direction: rtl; }
        .feedback { margin-top: 15px; font-weight: bold; }
    </style>
</head>
<body>

<header>
    <h1>Arapça Dil Atölyesi</h1>
    <p>Dinle, Konuş ve Kelimeleri Keşfet</p>
</header>

<div class="container">
    
    <div class="section-card">
        <h2><i class="fas fa-microphone-alt"></i> Konuşma Pratiği</h2>
        <p>Aşağıdaki kelimeyi söylemeyi dene, bakalım doğru telaffuz edebilecek misin?</p>
        <div class="speech-box">
            <h3 id="target-word" style="font-size: 2.5rem; color: var(--primary);">مَرْحَبًا</h3>
            <p>(Merhaba)</p>
            <button id="start-record" class="mic-btn"><i class="fas fa-microphone"></i></button>
            <div id="result-msg" class="feedback">Mikrofona bas ve konuş...</div>
        </div>
    </div>

    <div class="section-card">
        <h2><i class="fas fa-layer-group"></i> Kelime Kartları (Sesli)</h2>
        <div class="grid-game" id="word-grid"></div>
    </div>

    <div class="section-card">
        <h2><i class="fas fa-comments"></i> Günlük Diyaloglar</h2>
        <div id="dialogue-list"></div>
    </div>

</div>

<script>
    // --- SESLENDİRME (Text-to-Speech) ---
    function playSound(text) {
        const synth = window.speechSynthesis;
        const utter = new SpeechSynthesisUtterance(text);
        utter.lang = 'ar-SA';
        utter.rate = 0.8; // Biraz daha yavaş ve anlaşılır
        synth.speak(utter);
    }

    // --- SES TANIMA (Speech-to-Text) ---
    const startBtn = document.getElementById('start-record');
    const resultMsg = document.getElementById('result-msg');
    const targetWord = "مرحبا"; // Karşılaştırılacak hedef kelime

    if ('webkitSpeechRecognition' in window) {
        const recognition = new webkitSpeechRecognition();
        recognition.lang = 'ar-SA';
        recognition.continuous = false;

        startBtn.onclick = () => {
            recognition.start();
            startBtn.classList.add('recording');
            resultMsg.innerText = "Dinleniyor...";
        };

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript;
            startBtn.classList.remove('recording');
            
            if(transcript.includes(targetWord) || transcript == targetWord) {
                resultMsg.innerHTML = `<span style="color:green">✔ Harika! Şunu dedin: "${transcript}"</span>`;
            } else {
                resultMsg.innerHTML = `<span style="color:orange">Tekrar dene. Sen şunu dedin: "${transcript}"</span>`;
            }
        };

        recognition.onerror = () => {
            startBtn.classList.remove('recording');
            resultMsg.innerText = "Hata oluştu, tekrar dene.";
        };
    } else {
        resultMsg.innerText = "Tarayıcın ses tanımayı desteklemiyor.";
    }

    // --- KARTLAR VE DİYALOGLAR ---
    const words = [
        {ar: 'أنا أدْرُسُ', tr: 'Ders çalışıyorum'},
        {ar: 'أَيْنَ؟', tr: 'Nerede?'},
        {ar: 'طعام', tr: 'Yemek'},
        {ar: 'ساعة', tr: 'Saat'},
        {ar: 'مكتبة', tr: 'Kütüphane'},
        {ar: 'صديق', tr: 'Arkadaş'}
    ];

    const dialogues = [
        {ar: "كَيْفَ حَالُكَ؟", tr: "Nasılsın?", phonetic: "Keyfe haluk?"},
        {ar: "أَنَا سَعِيدٌ جِدًّا", tr: "Çok mutluyum", phonetic: "Ene saidun cidden"},
        {ar: "مَاذَا تَفْعَلُ؟", tr: "Ne yapıyorsun?", phonetic: "Maza tefal?"},
        {ar: "إِلَى اللِّقَاءِ", tr: "Görüşmek üzere", phonetic: "İle-llika"}
    ];

    // Sayfa Yüklendiğinde İçeriği Oluştur
    window.onload = () => {
        const grid = document.getElementById('word-grid');
        words.forEach(w => {
            const div = document.createElement('div');
            div.className = 'flash-card';
            div.innerHTML = `<div>${w.ar}</div><div style="font-size:0.8rem; color:grey">${w.tr}</div>`;
            div.onclick = () => playSound(w.ar);
            grid.appendChild(div);
        });

        const dList = document.getElementById('dialogue-list');
        dialogues.forEach(d => {
            const row = document.createElement('div');
            row.className = 'dialogue-row';
            row.innerHTML = `
                <div>
                    <div class="ar-text">${d.ar}</div>
                    <small>${d.tr} (${d.phonetic})</small>
                </div>
                <button class="mic-btn" style="width:40px; height:40px; font-size:14px;" onclick="playSound('${d.ar}')">
                    <i class="fas fa-volume-up"></i>
                </button>
            `;
            dList.appendChild(row);
        });
    };
</script>

</body>
</html>
