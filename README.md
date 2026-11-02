# calculator2
calculator2 in html and python
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Interactive Calculator</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                        }
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Custom scrollbar styling for history panel */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(156, 163, 175, 0.4);
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(156, 163, 175, 0.6);
        }
        
        /* Smooth button active feedback */
        .btn-press {
            transition: all 0.05s ease-in-out;
        }
        .btn-press:active {
            transform: scale(0.95);
        }
    </style>
</head>
<body class="bg-slate-100 dark:bg-slate-900 text-slate-800 dark:text-slate-100 min-h-full font-sans transition-colors duration-300 flex flex-col items-center justify-center p-3 sm:p-6">

    <!-- Header / Nav -->
    <header class="w-full max-w-4xl flex justify-between items-center mb-4 sm:mb-6 px-2">
        <div class="flex items-center gap-3">
            <div class="p-2.5 bg-blue-600 text-white rounded-xl shadow-lg shadow-blue-500/30">
                <i class="fa-solid font-bold fa-calculator text-xl"></i>
            </div>
            <div>
                <h1 class="text-xl sm:text-2xl font-bold tracking-tight">PyCalc Web</h1>
                <p class="text-xs text-slate-500 dark:text-slate-400">Python Logic • Modern UI</p>
            </div>
        </div>

        <div class="flex items-center gap-2">
            <!-- Clear History Quick Toggle -->
            <button id="toggleHistoryBtn" class="sm:hidden p-2.5 rounded-xl bg-white dark:bg-slate-800 shadow-sm border border-slate-200 dark:border-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-50 dark:hover:bg-slate-700 transition" title="Toggle History">
                <i class="fa-solid fa-clock-rotate-left"></i>
            </button>
            
            <!-- Dark / Light Mode Toggle -->
            <button id="themeToggle" class="p-2.5 rounded-xl bg-white dark:bg-slate-800 shadow-sm border border-slate-200 dark:border-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-50 dark:hover:bg-slate-700 transition flex items-center justify-center" title="Toggle theme">
                <i id="themeIcon" class="fa-solid fa-moon text-lg"></i>
            </button>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="w-full max-w-4xl grid grid-cols-1 sm:grid-cols-12 gap-5 items-start">

        <!-- Calculator Card -->
        <section class="sm:col-span-7 lg:col-span-8 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700/80 rounded-3xl p-4 sm:p-6 shadow-xl shadow-slate-200/50 dark:shadow-none flex flex-col justify-between transition-colors duration-300">
            
            <!-- Calculator Screen -->
            <div class="bg-slate-50 dark:bg-slate-950/70 border border-slate-200 dark:border-slate-800 rounded-2xl p-4 mb-5 text-right relative overflow-hidden transition-colors">
                
                <!-- Feedback Toast Banner for Errors -->
                <div id="errorBanner" class="absolute inset-x-0 top-0 bg-red-500/10 border-b border-red-500/20 text-red-600 dark:text-red-400 text-xs px-3 py-1.5 font-medium flex items-center justify-between opacity-0 transition-opacity duration-200 pointer-events-none">
                    <span id="errorMessage" class="truncate"><i class="fa-solid fa-circle-exclamation mr-1.5"></i>Error</span>
                    <button id="closeErrorBtn" class="text-red-500 hover:text-red-700 ml-2"><i class="fa-solid fa-xmark"></i></button>
                </div>

                <!-- Expression Line -->
                <div id="expressionDisplay" class="text-sm sm:text-base text-slate-400 dark:text-slate-400 min-h-[24px] overflow-x-auto whitespace-nowrap scrollbar-none font-mono"></div>
                
                <!-- Current Value Line -->
                <div id="mainDisplay" class="text-3xl sm:text-4xl font-semibold tracking-tight text-slate-900 dark:text-white min-h-[48px] overflow-x-auto whitespace-nowrap scrollbar-none font-mono flex items-center justify-end">0</div>
            </div>

            <!-- Keyboard Layout Grid -->
            <div class="grid grid-cols-4 gap-2.5 sm:gap-3">
                <!-- Row 1 -->
                <button data-action="clear" class="btn-press py-3.5 sm:py-4 rounded-xl bg-red-50 dark:bg-red-950/30 text-red-600 dark:text-red-400 font-semibold text-lg hover:bg-red-100 dark:hover:bg-red-900/40 transition">C</button>
                <button data-action="delete" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-100 dark:bg-slate-700/60 text-slate-700 dark:text-slate-200 font-semibold text-lg hover:bg-slate-200 dark:hover:bg-slate-700 transition">⌫</button>
                <button data-action="percent" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-100 dark:bg-slate-700/60 text-slate-700 dark:text-slate-200 font-semibold text-lg hover:bg-slate-200 dark:hover:bg-slate-700 transition">%</button>
                <button data-operator="/" class="btn-press py-3.5 sm:py-4 rounded-xl bg-blue-50 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 font-semibold text-xl hover:bg-blue-100 dark:hover:bg-blue-900/50 transition">÷</button>

                <!-- Row 2 -->
                <button data-number="7" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">7</button>
                <button data-number="8" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">8</button>
                <button data-number="9" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">9</button>
                <button data-operator="*" class="btn-press py-3.5 sm:py-4 rounded-xl bg-blue-50 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 font-semibold text-xl hover:bg-blue-100 dark:hover:bg-blue-900/50 transition">×</button>

                <!-- Row 3 -->
                <button data-number="4" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">4</button>
                <button data-number="5" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">5</button>
                <button data-number="6" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">6</button>
                <button data-operator="-" class="btn-press py-3.5 sm:py-4 rounded-xl bg-blue-50 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 font-semibold text-xl hover:bg-blue-100 dark:hover:bg-blue-900/50 transition">−</button>

                <!-- Row 4 -->
                <button data-number="1" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">1</button>
                <button data-number="2" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">2</button>
                <button data-number="3" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">3</button>
                <button data-operator="+" class="btn-press py-3.5 sm:py-4 rounded-xl bg-blue-50 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 font-semibold text-xl hover:bg-blue-100 dark:hover:bg-blue-900/50 transition">+</button>

                <!-- Row 5 -->
                <button data-action="toggle-sign" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">±</button>
                <button data-number="0" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">0</button>
                <button data-action="decimal" class="btn-press py-3.5 sm:py-4 rounded-xl bg-slate-50 dark:bg-slate-700/30 text-slate-800 dark:text-slate-100 font-medium text-xl hover:bg-slate-100 dark:hover:bg-slate-700/60 transition">.</button>
                <button data-action="equals" class="btn-press py-3.5 sm:py-4 rounded-xl bg-blue-600 text-white font-semibold text-2xl hover:bg-blue-700 shadow-md shadow-blue-500/20 transition">=</button>
            </div>
        </section>

        <!-- Calculation History Sidebar -->
        <section id="historyPanel" class="hidden sm:block sm:col-span-5 lg:col-span-4 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700/80 rounded-3xl p-4 sm:p-5 shadow-xl shadow-slate-200/50 dark:shadow-none h-full flex flex-col transition-colors duration-300">
            <div class="flex items-center justify-between pb-3 border-b border-slate-100 dark:border-slate-700/60">
                <div class="flex items-center gap-2 text-slate-700 dark:text-slate-200 font-semibold">
                    <i class="fa-solid fa-clock-rotate-left text-blue-500"></i>
                    <h2>Calculation History</h2>
                </div>
                <button id="clearHistoryBtn" class="text-xs text-slate-400 hover:text-red-500 transition flex items-center gap-1 px-2 py-1 rounded-md hover:bg-slate-100 dark:hover:bg-slate-700">
                    <i class="fa-solid fa-trash-can"></i>
                    Clear
                </button>
            </div>

            <!-- History Entries List -->
            <div id="historyList" class="flex-1 overflow-y-auto my-3 space-y-2 max-h-[380px] sm:max-h-[420px] pr-1">
                <div id="emptyHistory" class="text-center text-slate-400 dark:text-slate-500 text-sm py-12 flex flex-col items-center gap-2">
                    <i class="fa-solid fa-calculator text-3xl opacity-40"></i>
                    <p>No history yet</p>
                </div>
            </div>

            <div class="pt-2 border-t border-slate-100 dark:border-slate-700/60 text-xs text-slate-400 text-center">
                Click any history item to load its result into the calculator.
            </div>
        </section>
    </main>

    <!-- JavaScript Calculator Engine -->
    <script>
        // Python-equivalent calculation functions
        const PythonCalc = {
            add: (a, b) => a + b,
            subtract: (a, b) => a - b,
            multiply: (a, b) => a * b,
            divide: (a, b) => {
                if (b === 0) throw new Error("Cannot divide by zero.");
                return a / b;
            },
            calculate: function(a, b, operation) {
                switch (operation) {
                    case '+': return this.add(a, b);
                    case '-': return this.subtract(a, b);
                    case '*': return this.multiply(a, b);
                    case '/': return this.divide(a, b);
                    default: throw new Error("Invalid operation.");
                }
            }
        };

        // UI State Variables
        let currentValue = "0";
        let previousValue = null;
        let pendingOperator = null;
        let overwriteNext = false;
        let history = [];

        // DOM Element References
        const mainDisplay = document.getElementById('mainDisplay');
        const expressionDisplay = document.getElementById('expressionDisplay');
        const errorBanner = document.getElementById('errorBanner');
        const errorMessage = document.getElementById('errorMessage');
        const closeErrorBtn = document.getElementById('closeErrorBtn');
        const historyList = document.getElementById('historyList');
        const emptyHistory = document.getElementById('emptyHistory');
        const clearHistoryBtn = document.getElementById('clearHistoryBtn');
        const themeToggle = document.getElementById('themeToggle');
        const themeIcon = document.getElementById('themeIcon');
        const toggleHistoryBtn = document.getElementById('toggleHistoryBtn');
        const historyPanel = document.getElementById('historyPanel');

        // Helper: Format number output cleanly (e.g. drop trailing zeroes on whole numbers)
        function formatNumber(num) {
            if (isNaN(num)) return "Error";
            
            // Handle large or tiny numbers gracefully
            const strNum = num.toString();
            if (strNum.length > 12) {
                return parseFloat(num.toFixed(8)).toString();
            }
            
            // Format whole floats cleanly like Python (15.0 -> 15)
            return Number.isInteger(num) ? num.toString() : num.toString();
        }

        // Update Calculator Display
        function updateDisplay() {
            mainDisplay.textContent = currentValue;
            
            if (previousValue !== null && pendingOperator !== null) {
                const opSymbol = pendingOperator === '*' ? '×' : pendingOperator === '/' ? '÷' : pendingOperator === '-' ? '−' : pendingOperator;
                expressionDisplay.textContent = `${formatNumber(previousValue)} ${opSymbol}`;
            } else {
                expressionDisplay.textContent = '';
            }
        }

        // Show Error Banner with message
        function showError(msg) {
            errorMessage.innerHTML = `<i class="fa-solid fa-circle-exclamation mr-1.5"></i> ${msg}`;
            errorBanner.classList.remove('opacity-0', 'pointer-events-none');
        }

        // Hide Error Banner
        function clearError() {
            errorBanner.classList.add('opacity-0', 'pointer-events-none');
        }

        // Handle Digit Input
        function handleNumber(digit) {
            clearError();
            if (overwriteNext) {
                currentValue = digit;
                overwriteNext = false;
            } else {
                if (currentValue === "0") {
                    currentValue = digit;
                } else if (currentValue.length < 14) {
                    currentValue += digit;
                }
            }
            updateDisplay();
        }

        // Handle Decimal Input
        function handleDecimal() {
            clearError();
            if (overwriteNext) {
                currentValue = "0.";
                overwriteNext = false;
            } else if (!currentValue.includes('.')) {
                currentValue += '.';
            }
            updateDisplay();
        }

        // Handle Operator Input
        function handleOperator(op) {
            clearError();
            const inputNum = parseFloat(currentValue);

            if (previousValue !== null && pendingOperator !== null && !overwriteNext) {
                // Perform pending step calculation
                try {
                    const result = PythonCalc.calculate(previousValue, inputNum, pendingOperator);
                    previousValue = result;
                    currentValue = formatNumber(result);
                } catch (err) {
                    showError(err.message);
                    return;
                }
            } else {
                previousValue = inputNum;
            }

            pendingOperator = op;
            overwriteNext = true;
            updateDisplay();
        }

        // Perform Final Calculation (=)
        function handleEquals() {
            if (previousValue === null || pendingOperator === null) return;

            const inputNum = parseFloat(currentValue);
            try {
                const result = PythonCalc.calculate(previousValue, inputNum, pendingOperator);
                const formattedResult = formatNumber(result);

                // Add entry to calculation history
                const opSymbol = pendingOperator === '*' ? '×' : pendingOperator === '/' ? '÷' : pendingOperator === '-' ? '−' : pendingOperator;
                addHistoryEntry(`${formatNumber(previousValue)} ${opSymbol} ${formatNumber(inputNum)}`, formattedResult);

                currentValue = formattedResult;
                previousValue = null;
                pendingOperator = null;
                overwriteNext = true;
                clearError();
                updateDisplay();

            } catch (err) {
                showError(err.message);
            }
        }

        // Handle Clear Button (C)
        function handleClear() {
            currentValue = "0";
            previousValue = null;
            pendingOperator = null;
            overwriteNext = false;
            clearError();
            updateDisplay();
        }

        // Handle Backspace
        function handleDelete() {
            clearError();
            if (overwriteNext) {
                currentValue = "0";
                overwriteNext = false;
            } else if (currentValue.length > 1) {
                currentValue = currentValue.slice(0, -1);
                if (currentValue === "-" || currentValue === "") currentValue = "0";
            } else {
                currentValue = "0";
            }
            updateDisplay();
        }

        // Toggle Sign (+/-)
        function handleToggleSign() {
            clearError();
            if (currentValue !== "0") {
                currentValue = currentValue.startsWith('-') ? currentValue.slice(1) : '-' + currentValue;
                updateDisplay();
            }
        }

        // Handle Percentage
        function handlePercent() {
            clearError();
            const num = parseFloat(currentValue);
            if (!isNaN(num)) {
                currentValue = formatNumber(num / 100);
                updateDisplay();
            }
        }

        // Add Item to History Panel
        function addHistoryEntry(expression, result) {
            history.unshift({ expression, result });
            renderHistory();
        }

        // Render History UI
        function renderHistory() {
            if (history.length === 0) {
                emptyHistory.style.display = 'flex';
                historyList.querySelectorAll('.history-item').forEach(el => el.remove());
                return;
            }

            emptyHistory.style.display = 'none';
            
            // Clear existing history DOM items
            historyList.querySelectorAll('.history-item').forEach(el => el.remove());

            history.forEach(item => {
                const card = document.createElement('div');
                card.className = "history-item p-3 bg-slate-50 dark:bg-slate-700/40 hover:bg-slate-100 dark:hover:bg-slate-700/80 rounded-xl border border-slate-200/60 dark:border-slate-700/60 cursor-pointer transition text-right group";
                card.innerHTML = `
                    <div class="text-xs text-slate-400 dark:text-slate-400 font-mono">${item.expression} =</div>
                    <div class="text-lg font-semibold text-slate-800 dark:text-slate-100 font-mono group-hover:text-blue-600 dark:group-hover:text-blue-400">${item.result}</div>
                `;
                
                // Clicking history restores the result
                card.addEventListener('click', () => {
                    currentValue = item.result;
                    overwriteNext = true;
                    clearError();
                    updateDisplay();
                });

                historyList.appendChild(card);
            });
        }

        // Event Delegation for Calculator Buttons
        document.querySelector('main').addEventListener('click', (e) => {
            const btn = e.target.closest('button');
            if (!btn) return;

            if (btn.dataset.number !== undefined) {
                handleNumber(btn.dataset.number);
            } else if (btn.dataset.operator !== undefined) {
                handleOperator(btn.dataset.operator);
            } else if (btn.dataset.action !== undefined) {
                switch (btn.dataset.action) {
                    case 'clear': handleClear(); break;
                    case 'delete': handleDelete(); break;
                    case 'decimal': handleDecimal(); break;
                    case 'equals': handleEquals(); break;
                    case 'toggle-sign': handleToggleSign(); break;
                    case 'percent': handlePercent(); break;
                }
            }
        });

        // Keyboard Support
        window.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') {
                handleNumber(e.key);
            } else if (e.key === '.') {
                handleDecimal();
            } else if (e.key === '+' || e.key === '-') {
                handleOperator(e.key);
            } else if (e.key === '*') {
                handleOperator('*');
            } else if (e.key === '/') {
                e.preventDefault();
                handleOperator('/');
            } else if (e.key === 'Enter' || e.key === '=') {
                e.preventDefault();
                handleEquals();
            } else if (e.key === 'Backspace') {
                handleDelete();
            } else if (e.key === 'Escape') {
                handleClear();
            } else if (e.key === '%') {
                handlePercent();
            }
        });

        // Close Error Toast
        closeErrorBtn.addEventListener('click', clearError);

        // Clear History Action
        clearHistoryBtn.addEventListener('click', () => {
            history = [];
            renderHistory();
        });

        // Mobile History Toggle Button
        toggleHistoryBtn.addEventListener('click', () => {
            historyPanel.classList.toggle('hidden');
        });

        // Dark / Light Mode Switching logic
        themeToggle.addEventListener('click', () => {
            document.documentElement.classList.toggle('dark');
            const isDark = document.documentElement.classList.contains('dark');
            themeIcon.className = isDark ? 'fa-solid fa-sun text-yellow-400' : 'fa-solid fa-moon text-slate-600';
        });

        // Initialize state
        updateDisplay();
    </script>
</body>
</html>
