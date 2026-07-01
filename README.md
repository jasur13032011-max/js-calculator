# js-calculator
1. HTML Fayl (index.html)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsiv Kalkulyator</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="kalkulyator">
        <div class="displey-konteyner">
            <div id="tarix-ekran" class="tarix"></div>
            <div id="asosiy-ekran" class="ekran">0</div>
        </div>

        <div class="tugmalar">
            <button class="tugma funksiya" onclick="tozalash()">C</button>
            <button class="tugma funksiya" onclick="operatorniTanlash('/')">/</button>
            <button class="tugma funksiya" onclick="operatorniTanlash('*')">&times;</button>
            <button class="tugma funksiya" onclick="ochirish()">&#9003;</button>

            <button class="tugma" onclick="raqamQoshish('7')">7</button>
            <button class="tugma" onclick="raqamQoshish('8')">8</button>
            <button class="tugma" onclick="raqamQoshish('9')">9</button>
            <button class="tugma funksiya" onclick="operatorniTanlash('-')">-</button>

            <button class="tugma" onclick="raqamQoshish('4')">4</button>
            <button class="tugma" onclick="raqamQoshish('5')">5</button>
            <button class="tugma" onclick="raqamQoshish('6')">6</button>
            <button class="tugma funksiya" onclick="operatorniTanlash('+')">+</button>

            <button class="tugma" onclick="raqamQoshish('1')">1</button>
            <button class="tugma" onclick="raqamQoshish('2')">2</button>
            <button class="tugma" onclick="raqamQoshish('3')">3</button>
            <button class="tugma tenglik" onclick="hisoblash()">=</button>

            <button class="tugma nol" onclick="raqamQoshish('0')">0</button>
            <button class="tugma" onclick="nuqtaQoshish()">.</button>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
2. CSS Fayl (style.css)
CSS
/* Umumiy sozlamalar */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', system-ui, sans-serif;
}

body {
    background-color: #0f172a;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    padding: 20px;
}

/* Kalkulyator korpusi va Responsivlik */
.kalkulyator {
    background-color: #1e293b;
    width: 100%;
    max-width: 360px; /* Kompyuterda ixcham ko'rinadi */
    border-radius: 24px;
    padding: 24px;
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
}

/* Displey stillari */
.displey-konteyner {
    background-color: #0f172a;
    padding: 20px;
    border-radius: 16px;
    text-align: right;
    margin-bottom: 20px;
    min-height: 100px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    overflow: hidden;
}

.tarix {
    color: #64748b;
    font-size: 1.1rem;
    margin-bottom: 5px;
    word-wrap: break-word;
}

.ekran {
    color: #ffffff;
    font-size: clamp(1.8rem, 8vw, 2.5rem); /* Ekran torayganda matn o'zi kichrayadi */
    font-weight: 600;
    word-wrap: break-word;
}

/* Grid yordamida tugmalarni joylashtirish */
.tugmalar {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
}

.tugma {
    background-color: #334155;
    color: #ffffff;
    border: none;
    padding: 18px 0;
    font-size: 1.4rem;
    font-weight: 600;
    border-radius: 16px;
    cursor: pointer;
    transition: background-color 0.1s ease, transform 0.05s ease;
}

.tugma:active {
    transform: scale(0.95);
}

/* Tugma turlari bo'yicha ranglar */
.tugma:hover {
    background-color: #475569;
}

.funksiya {
    background-color: #f97316;
}

.funksiya:hover {
    background-color: #ea580c;
}

.tenglik {
    background-color: #2563eb;
    grid-row: span 2; /* 2 ta qator joyni egallaydi */
    height: 100%;
}

.tenglik:hover {
    background-color: #1d4ed8;
}

.nol {
    grid-column: span 2; /* 2 ta ustun joyni egallaydi */
}
3. JavaScript Fayl (script.js)
JavaScript
// Ekran elementlarini bog'lab olamiz
const asosiyEkran = document.getElementById('asosiy-ekran');
const tarixEkran = document.getElementById('tarix-ekran');

let joriyQiymat = '0';
let oldingiQiymat = '';
let operator = null;
let yangiRaqamKutilmoqda = false;

// Ekranni yangilash funksiyasi
function ekranniYangila() {
    asosiyEkran.innerText = joriyQiymat;
    if (operator && oldingiQiymat) {
        tarixEkran.innerText = `${oldingiQiymat} ${operator}`;
    } else {
        tarixEkran.innerText = '';
    }
}

// Raqam tugmalari bosilganda
function raqamQoshish(raqam) {
    if (joriyQiymat === '0' || yangiRaqamKutilmoqda || joriyQiymat === 'Xato') {
        joriyQiymat = raqam;
        yangiRaqamKutilmoqda = false;
    } else {
        // Displey sig'imidan oshib ketmasligi uchun cheklov (max 12 belgi)
        if (joriyQiymat.length < 12) {
            joriyQiymat += raqam;
        }
    }
    ekranniYangila();
}

// Nuqta (vergul) tugmasi bosilganda
function nuqtaQoshish() {
    if (yangiRaqamKutilmoqda || joriyQiymat === 'Xato') {
        joriyQiymat = '0.';
        yangiRaqamKutilmoqda = false;
    } else if (!joriyQiymat.includes('.')) {
        joriyQiymat += '.';
    }
    ekranniYangila();
}

// Operatorlar (+, -, *, /) bosilganda
function operatorniTanlash(tanlanganOperator) {
    if (joriyQiymat === 'Xato') return;
    
    if (operator && !yangiRaqamKutilmoqda) {
        hisoblash();
    }
    
    oldingiQiymat = joriyQiymat;
    operator = tanlanganOperator;
    yangiRaqamKutilmoqda = true;
    ekranniYangila();
}

// Matematik hisob-kitob qismi
function hisoblash() {
    if (!operator || yangiRaqamKutilmoqda) return;

    const bering = parseFloat(oldingiQiymat);
    const olgan = parseFloat(joriyQiymat);
    let natija = 0;

    switch (operator) {
        case '+': natija = bering + olgan; break;
        case '-': natija = bering - olgan; break;
        case '*': natija = bering * olgan; break;
        case '/':
            // Nolga bo'lish holatini tekshirish
            if (olgan === 0) {
                joriyQiymat = 'Xato';
                operator = null;
                oldingiQiymat = '';
                ekranniYangila();
                return;
            }
            natija = bering / olgan;
            break;
        default: return;
    }

    // Natija juda uzun kasr bo'lsa, qisqartirish (max 4 ta kasr raqami)
    joriyQiymat = parseFloat(natija.toFixed(4)).toString();
    operator = null;
    oldingiQiymat = '';
    yangiRaqamKutilmoqda = true;
    ekranniYangila();
}

// 'C' tugmasi - Ekranni tozalash
function tozalash() {
    joriyQiymat = '0';
    oldingiQiymat = '';
    operator = null;
    yangiRaqamKutilmoqda = false;
    ekranniYangila();
}

// Bitta belgini o'chirish (Backspace)
function ochirish() {
    if (joriyQiymat === 'Xato' || joriyQiymat.length === 1) {
        joriyQiymat = '0';
    } else {
        joriyQiymat = joriyQiymat.slice(0, -1);
    }
    ekranniYangila();
}

// KLAVIATURA BILAN ISHLASH TIZIMI
document.addEventListener('keydown', (event) => {
    const kalit = event.key;

    if (kalit >= '0' && kalit <= '9') raqamQoshish(kalit);
    if (kalit === '.' || kalit === ',') nuqtaQoshish();
    if (kalit === '+' || kalit === '-' || kalit === '*' || kalit === '/') operatorniTanlash(kalit);
    if (kalit === 'Enter' || kalit === '=') hisoblash();
    if (kalit === 'Escape' || kalit.toLowerCase() === 'c') tozalash();
    if (kalit === 'Backspace') ochirish();
});
Talablar qanday bajarildi?
Matematik amallar: +, -, *, / amallari JavaScript-dagi switch operatori orqali xatosiz hisoblanadi.

Nolga bo'lish cheklovi: Bo'luvchi 0 bo'lgan holatda ekran displeyida silliqqina "Xato" xabari paydo bo'ladi.

Tozalash ('C'): tozalash() funksiyasi barcha o'zgaruvchilarni dastlabki (nol) holatiga qaytaradi.

Klaviatura integratsiyasi: keydown hodisasi yordamida kompyuter klaviaturasidagi raqamlar, Enter, Backspace va Escape (C vazifasini bajaradi) tugmalari kalkulyator bilan to'liq bog'langan.

Responsiv dizayn: CSS-da max-width: 360px, grid va clamp() funksiyalari qo'llanilgani sababli, kalkulyator kichik mobil telefonlarda ham, katta ekranlarda ham mukammal joylashadi.
