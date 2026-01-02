# fist-try
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }

        .calculator {
            background: #fff;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            width: 320px;
        }

        .display {
            background: #222;
            color: #fff;
            padding: 30px 20px;
            text-align: right;
            font-size: 2.5em;
            min-height: 100px;
            word-wrap: break-word;
            word-break: break-all;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background: #ddd;
        }

        button {
            border: none;
            background: #f9f9f9;
            padding: 25px;
            font-size: 1.2em;
            cursor: pointer;
            transition: background 0.2s;
        }

        button:hover {
            background: #e0e0e0;
        }

        button:active {
            background: #d0d0d0;
        }

        .operator {
            background: #ff9500;
            color: #fff;
        }

        .operator:hover {
            background: #e08600;
        }

        .operator:active {
            background: #c77700;
        }

        .equals {
            background: #4caf50;
            color: #fff;
            grid-column: span 2;
        }

        .equals:hover {
            background: #45a049;
        }

        .equals:active {
            background: #3d8b40;
        }

        .clear {
            background: #f44336;
            color: #fff;
            grid-column: span 2;
        }

        .clear:hover {
            background: #da190b;
        }

        .clear:active {
            background: #c1170a;
        }

        .zero {
            grid-column: span 2;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="operator" onclick="appendOperator('/')">/</button>
            <button class="operator" onclick="appendOperator('*')">×</button>
            
            <button onclick="appendNumber('7')">7</button>
            <button onclick="appendNumber('8')">8</button>
            <button onclick="appendNumber('9')">9</button>
            <button class="operator" onclick="appendOperator('-')">-</button>
            
            <button onclick="appendNumber('4')">4</button>
            <button onclick="appendNumber('5')">5</button>
            <button onclick="appendNumber('6')">6</button>
            <button class="operator" onclick="appendOperator('+')">+</button>
            
            <button onclick="appendNumber('1')">1</button>
            <button onclick="appendNumber('2')">2</button>
            <button onclick="appendNumber('3')">3</button>
            <button class="operator" onclick="appendDecimal()">.</button>
            
            <button class="zero" onclick="appendNumber('0')">0</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>
    </div>

    <script>
        let currentInput = '0';
        let operator = null;
        let previousInput = null;
        let shouldResetDisplay = false;

        function updateDisplay() {
            document.getElementById('display').textContent = currentInput;
        }

        function clearDisplay() {
            currentInput = '0';
            operator = null;
            previousInput = null;
            shouldResetDisplay = false;
            updateDisplay();
        }

        function appendNumber(num) {
            if (shouldResetDisplay) {
                currentInput = num;
                shouldResetDisplay = false;
            } else {
                currentInput = currentInput === '0' ? num : currentInput + num;
            }
            updateDisplay();
        }

        function appendDecimal() {
            if (shouldResetDisplay) {
                currentInput = '0.';
                shouldResetDisplay = false;
            } else if (!currentInput.includes('.')) {
                currentInput += '.';
            }
            updateDisplay();
        }

        function appendOperator(op) {
            if (operator !== null && !shouldResetDisplay) {
                calculate();
            }
            previousInput = currentInput;
            operator = op;
            shouldResetDisplay = true;
        }

        function calculate() {
            if (operator === null || previousInput === null) return;

            const prev = parseFloat(previousInput);
            const current = parseFloat(currentInput);
            let result;

            switch (operator) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    result = current === 0 ? 'Error' : prev / current;
                    break;
                default:
                    return;
            }

            currentInput = result.toString();
            operator = null;
            previousInput = null;
            shouldResetDisplay = true;
            updateDisplay();
        }

        // Keyboard support
        document.addEventListener('keydown', function(e) {
            if (e.key >= '0' && e.key <= '9') {
                appendNumber(e.key);
            } else if (e.key === '.') {
                appendDecimal();
            } else if (e.key === '+' || e.key === '-' || e.key === '*' || e.key === '/') {
                appendOperator(e.key);
            } else if (e.key === 'Enter' || e.key === '=') {
                calculate();
            } else if (e.key === 'Escape' || e.key === 'c' || e.key === 'C') {
                clearDisplay();
            }
        });
    </script>
</body>
</html>
