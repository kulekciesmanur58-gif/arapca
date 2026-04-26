<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arapça Öğreniyorum - Projesi</title>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #27ae60;
            --light: #ecf0f1;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--light);
            margin: 0;
            padding: 20px;
            direction: ltr;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        h1, h2 { color: var(--primary); text-align: center; }
        
        /* Oyun Alanı */
        .game-area {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 10px;
            margin-bottom: 30px;
        }
        .card {
            background: var(--primary);
            color: white;
            padding: 20px;
            text-align: center;
            border-radius: 8px;
            cursor: pointer;
            transition: transform 0.2s;
            font-weight: bold;
        }
        .card:hover { transform: scale(1.05); background: var(--secondary); }

        /* Diyaloglar */
        .dialogue-section {
            background: #f9f9f9;
            padding: 15px;
            border-left: 5px solid var(--secondary);
            margin-bottom: 10px;
        }
        .arabic-text {
            font-size: 1.5rem;
            color: #2980b9;
            display: block;
            margin-bottom: 5px;
            direction: rtl;
        }
        .listen-btn {
            background: #e67e22;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>🌙 Arapça Öğrenme Paneli</h1>
    <p style="text-align:center">Kelimelere tıkla, telaffuzu dinle ve eşini bul!</p>

    <h2>🎮 Kelime Eşleştirme Oyunu</h2>
    <div id="game" class="game-area">
        </div>

    <hr>

    <h2>🗣️ Temel Diyaloglar</h2>
    
    <div class="dialogue-section">
        <span class="arabic-text">السَّلَامُ عَلَيْكُمْ (Esselâmü aleyküm)</span>
        <strong>Anlamı:</strong> Allah'ın selamı üzerine olsun.
        <button class="listen-btn" onclick="speak('السَّلَامُ عَلَيْكُمْ')">🔊 Dinle</button>
    </div>

    <div class="dialogue-section">
        <span class="arabic-text">كَيْفَ حَالُكَ؟ (Keyfe hâlük?)</span>
        <strong>Anlamı:</strong> Nasılsın? (Erkek için)
        <button class="listen-btn" onclick="speak('كَيْفَ حَالُكَ؟')">🔊 Dinle</button>
    </div>

    <div class="dialogue-section">
        <span class="arabic-text">أَنَا بِخَيْرٍ، شُكْراً (Ene bihayrin, şükran)</span>
        <strong>Anlamı:</strong> İyiyim, teşekkürler.
        <button class="listen-btn" onclick="speak('أَنَا بِخَيْرٍ، شُكْراً')">🔊 Dinle</button>
    </div>

    <div class="dialogue-section">
        <span class="arabic-text">مَا اسْمُكَ؟ (Mesmuke?)</span>
        <strong>Anlamı:</strong> Adın ne?
        <button class="listen-btn" onclick="speak('مَا اسْمُكَ؟')">🔊 Dinle</button>
    </div>
</div>

<script>
    // Konuşma Özelliği (Web Speech API)
    function speak(text) {
        const msg = new SpeechSynthesisUtterance();
        msg.text = text;
        msg.lang = 'ar-SA'; // Suudi Arabistan Arapçası
        window.speechSynthesis.speak(msg);
    }

    // Oyun Verileri
    const words = [
        {ar: 'كِتَاب', tr: 'Kitap'},
        {ar: 'قَلَم', tr: 'Kalem'},
        {ar: 'مَدْرَسَة', tr: 'Okul'},
        {ar: 'طَالِب', tr: 'Öğrenci'}
    ];

    const gameArea = document.getElementById('game');
    let selectedCards = [];

    // Kartları Karıştır ve Oluştur
    function initGame() {
        const gameItems = [...words.map(w => w.ar), ...words.map(w => w.tr)];
        gameItems.sort(() => Math.random() - 0.5);

        gameItems.forEach(item => {
            const card = document.createElement('div');
            card.className = 'card';
            card.textContent = item;
            card.onclick = () => {
                speak(item);
                card.style.background = '#27ae60';
                // Burada basit bir eşleşme mantığı eklenebilir
            };
            gameArea.appendChild(card);
        });
    }

    initGame();
</script>

</body>
</html>
