# js-calculator
Klaviaturani to'liq qo'llab-quvvatlaydigan, nolga bo'lish xatolarini to'g'ri boshqaradigan va mobil qurilmalarga moslashuvchan (responsive) mukammal Kalkulyator loyihasining to'liq HTML, CSS va JavaScript kodi:

HTML va CSS Kodu
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Kalkulyator</title>
  <style>
    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', system-ui, sans-serif;
      background-color: #0f172a;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    /* Responisve kalkulyator konteyneri */
    .calculator {
      width: 100%;
      max-width: 360px;
      background-color: #1e293b;
      border-radius: 16px;
      padding: 20px;
      box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
    }

    /* Ekran (Display) uslubi */
    .display {
      width: 100%;
      height: 70px;
      background-color: #0f172a;
      border: none;
      border-radius: 8px;
      color: #38bdf8;
      font-size: 2rem;
      text-align: right;
      padding: 10px 15px;
      margin-bottom: 20px;
      outline: none;
      overflow-x: auto;
    }

    /* Tugmalar tarmog'i (Grid) */
    .buttons-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }

    /* Umumiy tugma uslublari */
    button {
      font-size: 1.5rem;
      font-weight: 600;
      padding: 18px 0;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      background-color: #334155;
      color: #f8fafc;
      transition: background-color 0.15s ease, transform 0.1s ease;
    }

    button:active {
      transform: scale(0.95);
    }

    /* Maxsus tugmalar ranglari */
    button.clear {
      background-color: #ef4444; /* Qizil C tugmasi */
      color: white;
    }
    button.clear:hover { background-color: #dc2626; }

    button.operator {
      background-color: #f97316; /* To'q sariq amallar */
      color: white;
    }
    button.operator:hover { background-color: #ea580c; }

    button.equal {
      background-color: #10b981; /* Yashil tenglik */
      color: white;
      grid-column: span 2; /* Tenglik tugmasi 2 ta katakni egallaydi */
    }
    button.equal:hover { background-color: #059669; }

    button.number:hover {
      background-color: #475569;
    }

    /* Klaviatura fokusi uchun chiroyli outline */
    button:focus-visible {
      outline: 3px solid #38bdf8;
      outline-offset: 2px;
    }
  </style>
</head>
<body>

  <div class="calculator">
    <input type="text" class="display" id="calc-display" readonly value="0">

    <div class="buttons-grid">
      <button class="clear" onclick="clearDisplay()">C</button>
      <button class="operator" onclick="appendOperator('/')">/</button>
      <button class="operator" onclick="appendOperator('*')">*</button>
      <button class="operator" onclick="appendOperator('-')">-</button>
      
      <button class="number" onclick="appendNumber('7')">7</button>
      <button class="number" onclick="appendNumber('8')">8</button>
      <button class="number" onclick="appendNumber('9')">9</button>
      <button class="operator" onclick="appendOperator('+')">+</button>

      <button class="number" onclick="appendNumber('4')">4</button>
      <button class="number" onclick="appendNumber('5')">5</button>
      <button class="number" onclick="appendNumber('6')">6</button>
      <button class="number" onclick="appendNumber('.')">.</button>

      <button class="number" onclick="appendNumber('1')">1</button>
      <button class="number" onclick="appendNumber('2')">2</button>
      <button class="number" onclick="appendNumber('3')">3</button>
      
      <button class="number" onclick="appendNumber('0')">0</button>
      <button class="equal" onclick="calculateResult()">=</button>
    </div>
  </div>

  <script>
    const display = document.getElementById('calc-display');
    let currentExpression = '';
    let isErrorDisplaying = false;

    // Raqam qo'shish
    function appendNumber(num) {
      if (isErrorDisplaying || display.value === '0') {
        currentExpression = '';
        isErrorDisplaying = false;
      }
      
      // Ketma-ket nuqta qo'yilishini oldini olish
      if (num === '.' && currentExpression.split(/[\+\-\*\/]/).pop().includes('.')) {
        return;
      }

      currentExpression += num;
      display.value = currentExpression;
    }

    // Operator qo'shish (+, -, *, /)
    function appendOperator(op) {
      if (isErrorDisplaying) return;
      
      if (currentExpression === '' && op !== '-') return;

      // Agar oxirgi belgi operator bo'lsa, uni yangisiga almashtirish
      const lastChar = currentExpression.slice(-1);
      if (['+', '-', '*', '/'].includes(lastChar)) {
        currentExpression = currentExpression.slice(0, -1) + op;
      } else {
        currentExpression += op;
      }
      
      display.value = currentExpression;
    }

    // 'C' tugmasi - Ekranni tozalash
    function clearDisplay() {
      currentExpression = '';
      display.value = '0';
      isErrorDisplaying = false;
    }

    // Hisoblash amali
    function calculateResult() {
      if (currentExpression === '') return;

      // Nolga bo'lish holatini aniqlash (/0 mantiqiy xatosi)
      if (/\/0(?!\d)/.test(currentExpression)) {
        display.value = "Xato: Nolga bo'lish!";
        currentExpression = '';
        isErrorDisplaying = true;
        return;
      }

      try {
        // eval o'rniga xavfsiz Function constructor orqali hisoblash
        const result = new Function(`return ${currentExpression}`)();
        
        // Kasr sonlar to'g'ri ko'rinishi uchun (masalan 0.1 + 0.2)
        display.value = Number(result.toFixed(8)).toString();
        currentExpression = display.value;
      } catch (error) {
        display.value = "Xatolik";
        currentExpression = '';
        isErrorDisplaying = true;
      }
    }

    // KLAVIATURA BILAN ISHLASH (Keydown event)
    document.addEventListener('keydown', (event) => {
      const key = event.key;

      if (/[0-9.]/.test(key)) {
        appendNumber(key);
      } else if (['+', '-', '*', '/'].includes(key)) {
        appendOperator(key);
      } else if (key === 'Enter' || key === '=') {
        event.preventDefault(); // Brauzer standart harakatini to'xtatish
        calculateResult();
      } else if (key === 'Escape' || key.toLowerCase() === 'c') {
        clearDisplay();
      } else if (key === 'Backspace') {
        if (isErrorDisplaying) {
          clearDisplay();
        } else {
          currentExpression = currentExpression.slice(0, -1);
          display.value = currentExpression || '0';
        }
      }
    });
  </script>
</body>
</html>
Kod qanday talablarni bajardi?
To'liq amallar: Qo'shish, ayirish, ko'paytirish, bo'lish amallari JavaScript motori orqali aniq bajariladi.

Nolga bo'lish xatosi: /\/0(?!\d)/ muntazam ifodasi (regex) orqali ifoda ichida /0 borligi tekshiriladi va darhol Xato: Nolga bo'lish! matni ekranga chiqariladi.

C tugmasi: clearDisplay() funksiyasi xotira va ekrandagi ma'lumotlarni tozalab, 0 holatiga qaytaradi.

Klaviatura boshqaruvi: keydown hodisasi yordamida raqamlar, operatorlar, Enter (tenglik), Escape/C (tozalash) hamda Backspace (bittalab o'chirish) tugmalari klaviaturadan to'g'ridan-to'g'ri ishlaydi.

Responsive dizayn: CSS Grid va max-width: 360px xossasi kalkulyatorni kompyuter, planshet va eng kichik mobil telefon ekranlarida ham shakli buzilmasdan, chiroyli joylashishini ta'minlaydi.
