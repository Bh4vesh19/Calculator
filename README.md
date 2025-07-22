<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Animated Responsive Calculator</title>
  <style>
    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: "Segoe UI", Roboto, sans-serif;
      background: linear-gradient(to right, #141e30, #243b55);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    .calculator-container {
      max-width: 420px;
      width: 100%;
      background: rgba(20, 20, 30, 0.95);
      border-radius: 24px;
      padding: 20px;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.05);
      position: relative;
      transition: all 0.3s ease-in-out;
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
    }

    .calculator-title {
      font-size: 1.5rem;
      font-weight: bold;
      color: white;
    }

    .menu-btn {
      background: #1a1a2e;
      border: none;
      color: white;
      font-size: 1.5rem;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      cursor: pointer;
    }

    .dropdown-menu {
      position: absolute;
      top: 60px;
      right: 20px;
      background: #2e2e3a;
      border-radius: 12px;
      padding: 10px;
      display: none;
      flex-direction: column;
      gap: 6px;
    }

    .dropdown-menu.active {
      display: flex;
    }

    .dropdown-item {
      background: none;
      border: none;
      color: white;
      padding: 8px 12px;
      text-align: left;
      border-radius: 8px;
      cursor: pointer;
    }

    .dropdown-item:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    .screen-container {
      background: #0f0f1a;
      color: white;
      padding: 16px;
      border-radius: 16px;
      margin-bottom: 16px;
    }

    .history-display {
      font-size: 0.9rem;
      color: #999;
      min-height: 1.2em;
      text-align: right;
    }

    .main-display {
      font-size: 2.4rem;
      text-align: right;
      min-height: 2em;
      overflow-x: auto;
    }

    .buttons-grid {
      display: none;
      grid-template-columns: repeat(4, 1fr);
      gap: 16px;
      opacity: 0;
      transform: scale(0.98);
      transition: all 0.3s ease-in-out;
    }

    .buttons-grid.active-view {
      display: grid;
      opacity: 1;
      transform: scale(1);
    }

    #scientificButtonsGrid {
      grid-template-columns: repeat(5, 1fr);
      gap: 12px;
    }

    .btn {
      background: #2a2a3d;
      border: none;
      color: white;
      font-size: 1.25rem;
      border-radius: 12px;
      height: 75px;
      cursor: pointer;
      transition: background 0.3s ease;
    }

    .btn:hover {
      background: #3a3a5c;
    }

    .btn.zero {
      grid-column: span 2;
    }

    .btn.operator {
      background: #ff4500;
    }

    .btn.equals {
      background: #00cc44;
    }

    .btn.clear {
      background: #cc0000;
    }

    .btn.scientific-func {
      background: #4a148c;
      font-size: 0.875rem;
      height: 55px;
    }

    /* Responsive scaling */
    @media (max-width: 768px) {
      .btn {
        height: 60px;
        font-size: 1.1rem;
      }
      .btn.scientific-func {
        height: 45px;
        font-size: 0.75rem;
      }
      .buttons-grid {
        gap: 10px;
      }
      #scientificButtonsGrid {
        gap: 6px;
      }
    }

    @media (max-width: 400px) {
      .btn {
        height: 48px;
        font-size: 1rem;
      }
      .btn.scientific-func {
        height: 38px;
        font-size: 0.65rem;
      }
      .buttons-grid {
        gap: 6px;
      }
      #scientificButtonsGrid {
        gap: 2px;
      }
    }

    /* Animation visibility transitions */
    #currencyConverterPanel,
    #historyPanel {
      opacity: 0;
      transform: scale(0.98);
      visibility: hidden;
      height: 0;
      overflow: hidden;
      transition: all 0.3s ease-in-out;
    }

    #currencyConverterPanel.show-panel,
    #historyPanel.show-panel {
      opacity: 1;
      transform: scale(1);
      visibility: visible;
      height: auto;
    }
  </style>
</head>
<body>
  <div class="calculator-container">
    <div class="header">
      <div class="calculator-title">Calculator Pro</div>
      <button class="menu-btn" id="menuBtn">☰</button>
      <div class="dropdown-menu" id="dropdownMenu">
        <button class="dropdown-item" onclick="toggleStandard()">Standard</button>
        <button class="dropdown-item" onclick="toggleScientific()">Scientific</button>
        <button class="dropdown-item" onclick="toggleCurrency()">Currency</button>
        <button class="dropdown-item" onclick="toggleHistory()">History</button>
      </div>
    </div>

    <div class="screen-container">
      <div class="history-display" id="historyDisplay"></div>
      <div class="main-display" id="mainDisplay">0</div>
    </div>

    <div class="main-content-area">
      <div class="buttons-grid active-view" id="standardButtonsGrid">
        <button class="btn clear" onclick="clearAll()">AC</button>
        <button class="btn" onclick="clearEntry()">CE</button>
        <button class="btn" onclick="deleteLast()">⌫</button>
        <button class="btn operator" onclick="inputOperator('÷')">÷</button>

        <button class="btn" onclick="inputNumber('7')">7</button>
        <button class="btn" onclick="inputNumber('8')">8</button>
        <button class="btn" onclick="inputNumber('9')">9</button>
        <button class="btn operator" onclick="inputOperator('×')">×</button>

        <button class="btn" onclick="inputNumber('4')">4</button>
        <button class="btn" onclick="inputNumber('5')">5</button>
        <button class="btn" onclick="inputNumber('6')">6</button>
        <button class="btn operator" onclick="inputOperator('-')">−</button>

        <button class="btn" onclick="inputNumber('1')">1</button>
        <button class="btn" onclick="inputNumber('2')">2</button>
        <button class="btn" onclick="inputNumber('3')">3</button>
        <button class="btn operator" onclick="inputOperator('+')">+</button>

        <button class="btn zero" onclick="inputNumber('0')">0</button>
        <button class="btn" onclick="inputDecimal()">.</button>
        <button class="btn equals" onclick="calculate()" style="grid-column: span 2;">=</button>
      </div>

      <div class="buttons-grid" id="scientificButtonsGrid">
        <button class="btn scientific-func" onclick="scientificFunction('sin')">sin</button>
        <button class="btn scientific-func" onclick="scientificFunction('cos')">cos</button>
        <button class="btn scientific-func" onclick="scientificFunction('tan')">tan</button>
        <button class="btn scientific-func" onclick="scientificFunction('log')">log</button>
        <button class="btn scientific-func" onclick="scientificFunction('ln')">ln</button>

        <button class="btn scientific-func" onclick="scientificFunction('sqrt')">√</button>
        <button class="btn scientific-func" onclick="scientificFunction('square')">x²</button>
        <button class="btn scientific-func" onclick="scientificFunction('factorial')">x!</button>
        <button class="btn scientific-func" onclick="inputOperator('^')">xʸ</button>
        <button class="btn scientific-func" onclick="scientificFunction('inverse')">1/x</button>

        <button class="btn scientific-func" onclick="scientificFunction('percent')">%</button>
        <button class="btn scientific-func" onclick="inputNumber('3.14159')">π</button>
        <button class="btn scientific-func" onclick="inputNumber('2.71828')">e</button>
        <button class="btn" onclick="inputOperator('(')">(</button>
        <button class="btn" onclick="inputOperator(')')">)</button>

        <button class="btn" onclick="inputNumber('7')">7</button>
        <button class="btn" onclick="inputNumber('8')">8</button>
        <button class="btn" onclick="inputNumber('9')">9</button>
        <button class="btn operator" onclick="inputOperator('×')">×</button>
        <button class="btn clear" onclick="clearAll()">AC</button>

        <button class="btn" onclick="inputNumber('4')">4</button>
        <button class="btn" onclick="inputNumber('5')">5</button>
        <button class="btn" onclick="inputNumber('6')">6</button>
        <button class="btn operator" onclick="inputOperator('-')">−</button>
        <button class="btn" onclick="clearEntry()">CE</button>

        <button class="btn" onclick="inputNumber('1')">1</button>
        <button class="btn" onclick="inputNumber('2')">2</button>
        <button class="btn" onclick="inputNumber('3')">3</button>
        <button class="btn operator" onclick="inputOperator('+')">+</button>
        <button class="btn" onclick="deleteLast()">⌫</button>

        <button class="btn zero" onclick="inputNumber('0')">0</button>
        <button class="btn" onclick="inputDecimal()">.</button>
        <button class="btn equals" onclick="calculate()" style="grid-column: span 2;">=</button>
      </div>

      <div id="currencyConverterPanel" class="" style="margin-top: 20px; color: white;">
        <h3>Currency Converter</h3>
        <label for="fromCurrency">From:</label>
        <select id="fromCurrency"></select>

        <label for="toCurrency">To:</label>
        <select id="toCurrency"></select>

        <label for="currencyInput">Amount:</label>
        <input type="number" id="currencyInput" value="1" />

        <button onclick="convertCurrency()">Convert</button>
        <div id="currencyResult" style="margin-top: 10px; font-size: 18px;"></div>
      </div>

      <div id="historyPanel" class="" style="margin-top: 20px; color: white;">
        <h3>Calculation History</h3>
        <ul id="historyList"></ul>
      </div>
    </div>
  </div>
<script>
  let currentInput = "0";
  let previousInput = "";
  let operator = "";
  let history = [];

  const mainDisplay = document.getElementById("mainDisplay");
  const historyDisplay = document.getElementById("historyDisplay");
  const historyList = document.getElementById("historyList");
  const standardGrid = document.getElementById("standardButtonsGrid");
  const scientificGrid = document.getElementById("scientificButtonsGrid");
  const currencyPanel = document.getElementById("currencyConverterPanel");
  const historyPanel = document.getElementById("historyPanel");

  function updateDisplay() {
    mainDisplay.textContent = currentInput;
    historyDisplay.textContent = previousInput && operator ? `${previousInput} ${operator}` : "";
  }

  function inputNumber(num) {
    currentInput = currentInput === "0" ? num : currentInput + num;
    updateDisplay();
  }

  function inputDecimal() {
    if (!currentInput.includes(".")) {
      currentInput += ".";
      updateDisplay();
    }
  }

  function inputOperator(op) {
    if (currentInput === "") return;
    if (previousInput !== "") calculate();
    operator = op;
    previousInput = currentInput;
    currentInput = "";
    updateDisplay();
  }

  function calculate() {
    const a = parseFloat(previousInput);
    const b = parseFloat(currentInput);
    let result;

    switch (operator) {
      case "+": result = a + b; break;
      case "-": result = a - b; break;
      case "×": result = a * b; break;
      case "÷": result = b === 0 ? "Error" : a / b; break;
      case "^": result = Math.pow(a, b); break;
      default: return;
    }

    if (result !== "Error") {
      history.push(`${previousInput} ${operator} ${currentInput} = ${result}`);
      renderHistory();
    }

    currentInput = result.toString();
    previousInput = "";
    operator = "";
    updateDisplay();
  }

  function clearAll() {
    currentInput = "0";
    previousInput = "";
    operator = "";
    updateDisplay();
  }

  function clearEntry() {
    currentInput = "0";
    updateDisplay();
  }

  function deleteLast() {
    currentInput = currentInput.length > 1 ? currentInput.slice(0, -1) : "0";
    updateDisplay();
  }

  function scientificFunction(func) {
    const value = parseFloat(currentInput);
    let result;

    switch (func) {
      case "sin": result = Math.sin(value * Math.PI / 180); break;
      case "cos": result = Math.cos(value * Math.PI / 180); break;
      case "tan": result = Math.tan(value * Math.PI / 180); break;
      case "log": result = Math.log10(value); break;
      case "ln": result = Math.log(value); break;
      case "sqrt": result = Math.sqrt(value); break;
      case "square": result = value * value; break;
      case "factorial": result = factorial(value); break;
      case "inverse": result = 1 / value; break;
      case "percent": result = value / 100; break;
      default: result = value;
    }

    currentInput = isFinite(result) ? result.toString() : "Error";
    updateDisplay();
  }

  function factorial(n) {
    if (n < 0 || !Number.isInteger(n) || n > 170) return NaN;
    let res = 1;
    for (let i = 2; i <= n; i++) res *= i;
    return res;
  }

  function renderHistory() {
    historyList.innerHTML = "";
    history.slice().reverse().forEach(item => {
      const li = document.createElement("li");
      li.textContent = item;
      historyList.appendChild(li);
    });
  }

  function toggleStandard() {
    standardGrid.classList.add("active-view");
    scientificGrid.classList.remove("active-view");
    currencyPanel.classList.remove("show-panel");
    historyPanel.classList.remove("show-panel");
  }

  function toggleScientific() {
    standardGrid.classList.remove("active-view");
    scientificGrid.classList.add("active-view");
    currencyPanel.classList.remove("show-panel");
    historyPanel.classList.remove("show-panel");
  }

  function toggleCurrency() {
    standardGrid.classList.remove("active-view");
    scientificGrid.classList.remove("active-view");
    historyPanel.classList.remove("show-panel");
    currencyPanel.classList.add("show-panel");
    populateCurrencyOptions();
  }

  function toggleHistory() {
    standardGrid.classList.remove("active-view");
    scientificGrid.classList.remove("active-view");
    currencyPanel.classList.remove("show-panel");
    historyPanel.classList.toggle("show-panel");
  }

  const currencyRates = {
    USD: 1, EUR: 0.92, GBP: 0.79, JPY: 158.85, INR: 83.5, CAD: 1.37, AUD: 1.50, CNY: 7.24
  };

  function populateCurrencyOptions() {
    const from = document.getElementById("fromCurrency");
    const to = document.getElementById("toCurrency");
    const currencies = Object.keys(currencyRates);
    from.innerHTML = "";
    to.innerHTML = "";

    currencies.forEach(code => {
      const option1 = document.createElement("option");
      const option2 = document.createElement("option");
      option1.value = option2.value = code;
      option1.textContent = option2.textContent = code;
      from.appendChild(option1);
      to.appendChild(option2);
    });

    from.value = "USD";
    to.value = "EUR";
  }

  function convertCurrency() {
    const amount = parseFloat(document.getElementById("currencyInput").value) || 0;
    const from = document.getElementById("fromCurrency").value;
    const to = document.getElementById("toCurrency").value;
    const result = (amount / currencyRates[from]) * currencyRates[to];
    document.getElementById("currencyResult").textContent = `${result.toFixed(2)} ${to}`;
  }

  document.getElementById("menuBtn").addEventListener("click", (e) => {
    e.stopPropagation();
    document.getElementById("dropdownMenu").classList.toggle("active");
  });

  document.addEventListener("click", (e) => {
    const menu = document.getElementById("dropdownMenu");
    if (!menu.contains(e.target) && !document.getElementById("menuBtn").contains(e.target)) {
      menu.classList.remove("active");
    }
  });

  // 🎹 Keyboard support
  document.addEventListener("keydown", (e) => {
    const key = e.key;
    if (!isNaN(key)) inputNumber(key);
    else if (key === "." || key === ",") inputDecimal();
    else if (["+", "-", "*", "/", "^"].includes(key)) {
      const ops = { "*": "×", "/": "÷", "+": "+", "-": "-", "^": "^" };
      inputOperator(ops[key]);
    } else if (key === "Enter" || key === "=") calculate();
    else if (key === "Backspace") deleteLast();
    else if (key.toLowerCase() === "c") clearAll();
    else if (key === "Escape") clearEntry();
  });

  window.onload = () => {
    toggleStandard();
    updateDisplay();
  };
</script>
</body>
</html>
