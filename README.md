<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }

        .calculator {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            max-width: 350px;
            width: 100%;
        }

        .display {
            width: 100%;
            height: 80px;
            background: rgba(0, 0, 0, 0.2);
            border: none;
            border-radius: 15px;
            color: white;
            font-size: 2.5rem;
            text-align: right;
            padding: 0 20px;
            margin-bottom: 20px;
            font-weight: 300;
            letter-spacing: 1px;
        }

        .display:focus {
            outline: none;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
        }

        button {
            height: 60px;
            border: none;
            border-radius: 15px;
            font-size: 1.2rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
        }

        .btn-number, .btn-decimal {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .btn-number:hover, .btn-decimal:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .btn-operator {
            background: rgba(255, 107, 107, 0.8);
            color: white;
            border: 1px solid rgba(255, 107, 107, 0.3);
        }

        .btn-operator:hover {
            background: rgba(255, 107, 107, 1);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
        }

        .btn-equals {
            background: rgba(72, 187, 120, 0.8);
            color: white;
            border: 1px solid rgba(72, 187, 120, 0.3);
        }

        .btn-equals:hover {
            background: rgba(72, 187, 120, 1);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(72, 187, 120, 0.4);
        }

        .btn-clear {
            background: rgba(237, 137, 54, 0.8);
            color: white;
            border: 1px solid rgba(237, 137, 54, 0.3);
        }

        .btn-clear:hover {
            background: rgba(237, 137, 54, 1);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(237, 137, 54, 0.4);
        }

        .btn-zero {
            grid-column: span 2;
        }

        button:active {
            transform: translateY(1px);
        }

        @media (max-width: 400px) {
            .calculator {
                margin: 20px;
                padding: 20px;
            }
            
            .display {
                font-size: 2rem;
                height: 70px;
            }
            
            button {
                height: 50px;
                font-size: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <input type="text" class="display" id="display" readonly>
        <div class="buttons">
            <button class="btn-clear" onclick="clearDisplay()">C</button>
            <button class="btn-clear" onclick="deleteLast()">⌫</button>
            <button class="btn-operator" onclick="appendToDisplay('/')">/</button>
            <button class="btn-operator" onclick="appendToDisplay('*')">×</button>
            
            <button class="btn-number" onclick="appendToDisplay('7')">7</button>
            <button class="btn-number" onclick="appendToDisplay('8')">8</button>
            <button class="btn-number" onclick="appendToDisplay('9')">9</button>
            <button class="btn-operator" onclick="appendToDisplay('-')">-</button>
            
            <button class="btn-number" onclick="appendToDisplay('4')">4</button>
            <button class="btn-number" onclick="appendToDisplay('5')">5</button>
            <button class="btn-number" onclick="appendToDisplay('6')">6</button>
            <button class="btn-operator" onclick="appendToDisplay('+')">+</button>
            
            <button class="btn-number" onclick="appendToDisplay('1')">1</button>
            <button class="btn-number" onclick="appendToDisplay('2')">2</button>
            <button class="btn-number" onclick="appendToDisplay('3')">3</button>
            <button class="btn-equals" onclick="calculate()" rowspan="2">=</button>
            
            <button class="btn-number btn-zero" onclick="appendToDisplay('0')">0</button>
            <button class="btn-decimal" onclick="appendToDisplay('.')">.</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentInput = '';
        let operator = '';
        let previousInput = '';
        let shouldResetDisplay = false;

        function appendToDisplay(value) {
            if (shouldResetDisplay) {
                display.value = '';
                shouldResetDisplay = false;
            }

            // Handle operators
            if (['+', '-', '*', '/'].includes(value)) {
                if (currentInput === '' && display.value === '') return;
                
                if (currentInput !== '' && previousInput !== '' && operator !== '') {
                    calculate();
                }
                
                previousInput = display.value || currentInput;
                operator = value;
                currentInput = '';
                shouldResetDisplay = true;
                
                // Show the operator in display
                display.value = previousInput + ' ' + (value === '*' ? '×' : value) + ' ';
                return;
            }

            // Handle decimal point
            if (value === '.') {
                if (currentInput.includes('.')) return;
                if (currentInput === '') {
                    currentInput = '0.';
                } else {
                    currentInput += '.';
                }
            } else {
                currentInput += value;
            }

            if (operator && previousInput) {
                display.value = previousInput + ' ' + (operator === '*' ? '×' : operator) + ' ' + currentInput;
            } else {
                display.value = currentInput;
            }
        }

        function calculate() {
            if (previousInput === '' || currentInput === '' || operator === '') return;

            let prev = parseFloat(previousInput);
            let current = parseFloat(currentInput);
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
                    if (current === 0) {
                        display.value = 'Error';
                        resetCalculator();
                        return;
                    }
                    result = prev / current;
                    break;
                default:
                    return;
            }

            // Format result to avoid floating point precision issues
            result = Math.round((result + Number.EPSILON) * 100000000) / 100000000;
            
            display.value = result.toString();
            currentInput = result.toString();
            previousInput = '';
            operator = '';
            shouldResetDisplay = true;
        }

        function clearDisplay() {
            display.value = '';
            currentInput = '';
            previousInput = '';
            operator = '';
            shouldResetDisplay = false;
        }

        function deleteLast() {
            if (shouldResetDisplay) {
                clearDisplay();
                return;
            }

            if (currentInput.length > 0) {
                currentInput = currentInput.slice(0, -1);
                if (operator && previousInput) {
                    display.value = previousInput + ' ' + (operator === '*' ? '×' : operator) + ' ' + currentInput;
                } else {
                    display.value = currentInput;
                }
            } else if (operator) {
                operator = '';
                currentInput = previousInput;
                previousInput = '';
                display.value = currentInput;
            }
        }

        function resetCalculator() {
            setTimeout(() => {
                clearDisplay();
            }, 1500);
        }

        // Keyboard support
        document.addEventListener('keydown', function(event) {
            const key = event.key;
            
            if (key >= '0' && key <= '9') {
                appendToDisplay(key);
            } else if (key === '.') {
                appendToDisplay('.');
            } else if (['+', '-', '*', '/'].includes(key)) {
                appendToDisplay(key);
            } else if (key === 'Enter' || key === '=') {
                event.preventDefault();
                calculate();
            } else if (key === 'Escape' || key === 'c' || key === 'C') {
                clearDisplay();
            } else if (key === 'Backspace') {
                event.preventDefault();
                deleteLast();
            }
        });
    </script>
</body>
</html>
