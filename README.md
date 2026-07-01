# js-calculator
let display = document.getElementById("display");
let currentInput = "";
let operator = "";
let previousInput = "";

function append(value) {
    if (value === "." && currentInput.includes(".")) return;
    currentInput += value;
    updateDisplay();
}

function chooseOperator(op) {
    if (currentInput === "" && previousInput === "") return;

    if (previousInput !== "") {
        calculate();
    }

    operator = op;
    previousInput = currentInput || previousInput;
    currentInput = "";
    updateDisplay();
}

function updateDisplay() {
    if (currentInput && previousInput && operator) {
        display.value = previousInput + " " + operator + " " + currentInput;
    } else if (operator && previousInput) {
        display.value = previousInput + " " + operator;
    } else {
        display.value = currentInput || previousInput || "0";
    }
}

function clearDisplay() {
    currentInput = "";
    previousInput = "";
    operator = "";
    updateDisplay();
}

function deleteLast() {
    currentInput = currentInput.toString().slice(0, -1);
    updateDisplay();
}

function calculate() {
    let prev = parseFloat(previousInput);
    let current = parseFloat(currentInput);

    if (isNaN(prev) || isNaN(current)) return;

    let result;
    switch (operator) {
        case "+": result = prev + current; break;
        case "-": result = prev - current; break;
        case "*": result = prev * current; break;
        case "/":
            if (current === 0) {
                alert("0 ga bo‘lib bo‘lmaydi!");
                clearDisplay();
                return;
            }
            result = prev / current;
            break;
        default: return;
    }

    display.value = previousInput + " " + operator + " " + currentInput + " = " + result;

    currentInput = result.toString();
    operator = "";
    previousInput = "";
}

document.addEventListener("keydown", function (e) {
    if (!isNaN(e.key)) {
        append(e.key);
    } else if (e.key === ".") {
        append(".");
    } else if (["+", "-", "*", "/"].includes(e.key)) {
        chooseOperator(e.key);
    } else if (e.key === "Enter") {
        e.preventDefault();
        calculate();
    } else if (e.key === "Backspace") {
        deleteLast();
    } else if (e.key.toLowerCase() === "c") {
        clearDisplay();
    }
});
let historyDiv = document.getElementById("history");

function addToHistory(entry) {
    const div = document.createElement("div");
    div.textContent = entry;
    historyDiv.appendChild(div);
    historyDiv.scrollTop = historyDiv.scrollHeight; // scroll pastga
}

function calculate() {
    let prev = parseFloat(previousInput);
    let current = parseFloat(currentInput);

    if (isNaN(prev) || isNaN(current)) return;

    let result;
    switch (operator) {
        case "+": result = prev + current; break;
        case "-": result = prev - current; break;
        case "*": result = prev * current; break;
        case "/":
            if (current === 0) {
                alert("0 ga bo‘lib bo‘lmaydi!");
                clearDisplay();
                return;
            }
            result = prev / current;
            break;
        default: return;
    }

    let calculation = `${previousInput} ${operator} ${currentInput} = ${result}`;
    display.value = calculation;
    addToHistory(calculation);

    currentInput = result.toString();
    operator = "";
    previousInput = "";
}

updateDisplay();
