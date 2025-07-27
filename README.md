<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Enhanced dark theme and animations */
        body {
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
            min-height: 100vh;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            margin: 0;
            padding: 0;
        }
        
        .transition-all { transition: all 0.3s ease; }
        .transform { transform: translateZ(0); }
        .scale-95 { transform: scale(0.95); }
        .scale-100 { transform: scale(1); }
        .max-h-0 { max-height: 0; }
        .max-h-64 { max-height: 16rem; }
        .max-h-60 { max-height: 15rem; }
        .max-h-80 { max-height: 20rem; }
        .opacity-0 { opacity: 0; }
        .opacity-100 { opacity: 1; }
        .pointer-events-none { pointer-events: none; }
        
        /* Enhanced button animations */
        .calc-btn {
            position: relative;
            overflow: hidden;
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            transform: translateZ(0);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            border: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        
        .calc-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
        }
        
        .calc-btn:active {
            transform: translateY(0) scale(0.95);
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.4);
        }
        
        /* Enhanced ripple effect */
        .calc-btn::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
            pointer-events: none;
        }
        
        .calc-btn.ripple::before {
            width: 300px;
            height: 300px;
        }
        
        /* Enhanced glow effects */
        .glow {
            box-shadow: 0 0 25px rgba(59, 130, 246, 0.6), 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .glow-green {
            box-shadow: 0 0 25px rgba(34, 197, 94, 0.6), 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .glow-orange {
            box-shadow: 0 0 25px rgba(249, 115, 22, 0.6), 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .glow-purple {
            box-shadow: 0 0 25px rgba(147, 51, 234, 0.6), 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        .glow-blue {
            box-shadow: 0 0 25px rgba(59, 130, 246, 0.8), 0 4px 15px rgba(0, 0, 0, 0.2);
        }
        
        /* Mode content transitions */
        .mode-content {
            transition: opacity 0.4s cubic-bezier(0.4, 0, 0.2, 1), 
                        transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .mode-content.hidden {
            opacity: 0;
            transform: scale(0.9) translateY(20px);
            position: absolute;
            pointer-events: none;
            width: 100%;
        }
        
        .mode-content.active {
            opacity: 1;
            transform: scale(1) translateY(0);
            position: relative;
            pointer-events: auto;
        }
        
        /* Enhanced particle animation */
        .particle {
            position: absolute;
            width: 6px;
            height: 6px;
            background: #3b82f6;
            border-radius: 50%;
            pointer-events: none;
            animation: float 2.5s ease-out forwards;
            z-index: 1000;
        }
        
        @keyframes float {
            0% {
                opacity: 1;
                transform: translateY(0) scale(1) rotate(0deg);
            }
            100% {
                opacity: 0;
                transform: translateY(-120px) scale(0) rotate(360deg);
            }
        }
        
        /* Shake animation for errors */
        .shake {
            animation: shake 0.6s ease-in-out;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-8px); }
            20%, 40%, 60%, 80% { transform: translateX(8px); }
        }
        
        /* Enhanced pulse animation */
        .pulse {
            animation: pulse 0.4s ease-in-out;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        /* Currency input enhancements - NO ARROW KEY INCREMENTING */
        input[type="text"]::-webkit-outer-spin-button,
        input[type="text"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
            display: none;
        }
        
        input[type="text"],
        input[type="number"] {
            -moz-appearance: textfield;
        }
        
        /* Simplified currency input styling */
        .currency-input {
            background: #374151;
            border: 1px solid #6b7280;
            transition: all 0.2s ease;
        }
        
        .currency-input:focus {
            border-color: #3b82f6;
            outline: none;
        }
        
        /* Simplified select styling */
        .currency-select {
            background: #374151;
            border: 1px solid #6b7280;
            transition: all 0.2s ease;
        }
        
        .currency-select:focus {
            border-color: #3b82f6;
            outline: none;
        }
        
        /* Simplified result display */
        .currency-result {
            background: #065f46;
            border: 1px solid #10b981;
        }
        
        /* Scientific calculator enhancements */
        .scientific-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 0.5rem;
        }
        
        .scientific-btn {
            min-height: 3rem;
            font-size: 0.75rem;
            font-weight: 600;
        }
        
        .scientific-btn-large {
            min-height: 3.5rem;
            font-size: 1rem;
        }
        
        /* Custom ripple effect */
        .ripple-effect {
            position: absolute;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.4);
            transform: scale(0);
            animation: ripple-expand 0.6s linear;
            pointer-events: none;
        }
        
        @keyframes ripple-expand {
            to {
                transform: scale(4);
                opacity: 0;
            }
        }
        
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 10px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: linear-gradient(180deg, #3b82f6, #1d4ed8);
            border-radius: 5px;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: linear-gradient(180deg, #2563eb, #1e40af);
        }
        
        /* Mobile enhancements */
        @media (max-width: 640px) {
            .calc-btn {
                min-height: 3.5rem;
                font-size: 1rem;
            }
            
            .scientific-btn {
                min-height: 2.5rem;
                font-size: 0.7rem;
            }
            
            .scientific-btn-large {
                min-height: 3rem;
                font-size: 0.9rem;
            }
            
            .currency-input {
                font-size: 18px; /* Prevents zoom on iOS */
                padding: 1rem;
            }
            
            .currency-select {
                font-size: 16px;
                padding: 1rem;
            }
        }
        
        /* Enhanced menu transitions */
        .menu-item {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .menu-item:hover {
            transform: translateX(5px);
            background: linear-gradient(90deg, rgba(59, 130, 246, 0.1), rgba(59, 130, 246, 0.05));
        }
        
        .menu-item.active {
            background: linear-gradient(90deg, #3b82f6, #2563eb);
            transform: scale(1.02);
        }
        
        /* History animation */
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }
        
        /* Enhanced responsive grid for scientific mode */
        @media (max-width: 480px) {
            .scientific-grid {
                gap: 0.25rem;
            }
            
            .scientific-btn {
                min-height: 2.2rem;
                font-size: 0.65rem;
                padding: 0.25rem;
            }
        }

        /* Country flag visibility fix */
        .currency-option {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .currency-flag {
            opacity: 0;
            transition: opacity 0.2s ease;
        }
        
        .currency-select:hover .currency-flag,
        .currency-select:focus .currency-flag,
        .currency-option:hover .currency-flag {
            opacity: 1;
        }
    </style>
</head>
<body>
    <div class="min-h-screen flex items-center justify-center p-2 sm:p-4 relative">
        <div class="w-full max-w-sm sm:max-w-md lg:max-w-lg bg-gray-900 rounded-2xl shadow-2xl overflow-hidden border border-gray-700 backdrop-blur-lg bg-opacity-95">
            <!-- Header -->
            <div class="bg-gray-800 p-3 sm:p-4 flex items-center justify-between border-b border-gray-700">
                <div class="flex items-center gap-3">
                    <!-- Calculator Icon -->
                    <svg class="w-5 h-5 sm:w-6 sm:h-6 text-blue-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <rect x="4" y="3" width="16" height="18" rx="2" ry="2"></rect>
                        <line x1="8" y1="7" x2="16" y2="7"></line>
                        <line x1="8" y1="11" x2="12" y2="11"></line>
                        <line x1="8" y1="15" x2="12" y2="15"></line>
                        <line x1="16" y1="11" x2="16" y2="15"></line>
                    </svg>
                    <h1 class="text-lg sm:text-xl font-semibold text-white">Calculator</h1>
                </div>
                <button
                    id="menuToggle"
                    class="p-2 rounded-lg bg-gray-700 hover:bg-gray-600 transition-colors duration-200"
                >
                    <!-- Menu Icon -->
                    <svg id="menuIcon" class="w-4 h-4 sm:w-5 sm:h-5 text-gray-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <line x1="3" y1="6" x2="21" y2="6"></line>
                        <line x1="3" y1="12" x2="21" y2="12"></line>
                        <line x1="3" y1="18" x2="21" y2="18"></line>
                    </svg>
                    <!-- Close Icon -->
                    <svg id="closeIcon" class="w-4 h-4 sm:w-5 sm:h-5 text-gray-300 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <line x1="18" y1="6" x2="6" y2="18"></line>
                        <line x1="6" y1="6" x2="18" y2="18"></line>
                    </svg>
                </button>
            </div>

            <!-- Menu Dropdown -->
            <div id="menuDropdown" class="bg-gray-800 border-b border-gray-700 transition-all duration-300 overflow-hidden max-h-0 opacity-0">
                <div class="p-3 sm:p-4 space-y-1 sm:space-y-2">
                    <button data-mode="standard" class="mode-btn menu-item flex items-center gap-2 sm:gap-3 w-full px-3 sm:px-4 py-2 sm:py-3 rounded-lg transition-all duration-200 text-sm sm:text-base bg-blue-600 text-white active">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <rect x="4" y="3" width="16" height="18" rx="2" ry="2"></rect>
                            <line x1="8" y1="7" x2="16" y2="7"></line>
                            <line x1="8" y1="11" x2="12" y2="11"></line>
                            <line x1="8" y1="15" x2="12" y2="15"></line>
                            <line x1="16" y1="11" x2="16" y2="15"></line>
                        </svg>
                        <span>Standard</span>
                    </button>
                    <button data-mode="scientific" class="mode-btn menu-item flex items-center gap-2 sm:gap-3 w-full px-3 sm:px-4 py-2 sm:py-3 rounded-lg transition-all duration-200 text-sm sm:text-base text-gray-300 hover:bg-gray-700">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path d="M9 7h6l1 10-4-4-4 4 1-10z"></path>
                            <circle cx="12" cy="5" r="2"></circle>
                        </svg>
                        <span>Scientific</span>
                    </button>
                    <button data-mode="currency" class="mode-btn menu-item flex items-center gap-2 sm:gap-3 w-full px-3 sm:px-4 py-2 sm:py-3 rounded-lg transition-all duration-200 text-sm sm:text-base text-gray-300 hover:bg-gray-700">
                        <!-- Enhanced Dollar Sign Icon -->
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <line x1="12" y1="1" x2="12" y2="23"></line>
                            <path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path>
                        </svg>
                        <span>Currency Converter</span>
                    </button>
                    <button data-mode="history" class="mode-btn menu-item flex items-center gap-2 sm:gap-3 w-full px-3 sm:px-4 py-2 sm:py-3 rounded-lg transition-all duration-200 text-sm sm:text-base text-gray-300 hover:bg-gray-700">
                        <!-- History Icon -->
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"></path>
                            <path d="M3 3v5h5"></path>
                            <path d="M12 7v5l4 2"></path>
                        </svg>
                        <span>History</span>
                    </button>
                </div>
            </div>

            <!-- Display -->
            <div class="bg-gradient-to-r from-gray-900 to-gray-800 p-4 sm:p-6 border-b border-gray-700">
                <div class="text-right">
                    <div id="previousDisplay" class="text-gray-400 text-xs sm:text-sm min-h-[1rem] sm:min-h-[1.25rem] mb-1 sm:mb-2 overflow-hidden">
                    </div>
                    <div id="currentDisplay" class="text-white text-2xl sm:text-3xl font-light break-all">
                        0
                    </div>
                </div>
            </div>

            <!-- Content Area -->
            <div class="p-3 sm:p-4 lg:p-6 bg-gray-900 relative">
                <!-- Standard Calculator -->
                <div id="standardMode" class="mode-content active">
                    <div class="grid grid-cols-4 gap-2 sm:gap-3">
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-red-600 hover:bg-red-500 text-white" data-action="clear-all">AC</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-red-600 hover:bg-red-500 text-white" data-action="clear-entry">CE</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="delete">⌫</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-orange-600 hover:bg-orange-500 text-white" data-action="operator" data-value="÷">÷</button>

                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="7">7</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="8">8</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="9">9</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-orange-600 hover:bg-orange-500 text-white" data-action="operator" data-value="×">×</button>

                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="4">4</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="5">5</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="6">6</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-orange-600 hover:bg-orange-500 text-white" data-action="operator" data-value="−">−</button>

                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="1">1</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="2">2</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="number" data-value="3">3</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-orange-600 hover:bg-orange-500 text-white" data-action="operator" data-value="+">+</button>

                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200 col-span-2" data-action="number" data-value="0">0</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-gray-700 hover:bg-gray-600 text-gray-200" data-action="decimal">.</button>
                        <button class="calc-btn h-12 sm:h-14 lg:h-16 rounded-lg font-semibold text-sm sm:text-base lg:text-lg transition-all duration-200 transform hover:shadow-md touch-manipulation bg-green-600 hover:bg-green-500 text-white" data-action="calculate">=</button>
                    </div>
                </div>

                <!-- FIXED Scientific Calculator -->
                <div id="scientificMode" class="mode-content hidden">
                    <div class="scientific-grid">
                        <!-- Row 1: Advanced Functions -->
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="sin">sin</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="cos">cos</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="tan">tan</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="log">log</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="ln">ln</button>

                        <!-- Row 2: More Functions -->
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="sqrt">√</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="square">x²</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="factorial">x!</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="operator" data-value="^">xʸ</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="inverse">1/x</button>

                        <!-- Row 3: Constants and Brackets -->
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="scientific" data-value="percent">%</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="3.14159">π</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="2.71828">e</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="(">(</button>
                        <button class="calc-btn scientific-btn bg-blue-600 hover:bg-blue-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value=")">)</button>

                        <!-- Row 4: Numbers and Operators -->
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="7">7</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="8">8</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="9">9</button>
                        <button class="calc-btn scientific-btn-large bg-orange-600 hover:bg-orange-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="operator" data-value="×">×</button>
                        <button class="calc-btn scientific-btn-large bg-red-600 hover:bg-red-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="clear-all">AC</button>

                        <!-- Row 5: Numbers and Operators -->
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="4">4</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="5">5</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="6">6</button>
                        <button class="calc-btn scientific-btn-large bg-orange-600 hover:bg-orange-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="operator" data-value="−">−</button>
                        <button class="calc-btn scientific-btn-large bg-red-600 hover:bg-red-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="clear-entry">CE</button>

                        <!-- Row 6: Numbers and Operators -->
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="1">1</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="2">2</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="number" data-value="3">3</button>
                        <button class="calc-btn scientific-btn-large bg-orange-600 hover:bg-orange-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="operator" data-value="+">+</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="delete">⌫</button>

                        <!-- Row 7: Zero, Decimal, Equals -->
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation col-span-2" data-action="number" data-value="0">0</button>
                        <button class="calc-btn scientific-btn-large bg-gray-700 hover:bg-gray-600 text-gray-200 rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation" data-action="decimal">.</button>
                        <button class="calc-btn scientific-btn-large bg-green-600 hover:bg-green-500 text-white rounded-lg font-semibold transition-all duration-200 transform hover:shadow-md touch-manipulation col-span-2" data-action="calculate">=</button>
                    </div>
                </div>

                <!-- Simplified Currency Converter -->
                <div id="currencyMode" class="mode-content hidden">
                    <div class="space-y-4">
                        <div class="text-center">
                            <h3 class="text-xl font-bold text-white mb-2">Currency Converter</h3>
                        </div>
                        
                        <div class="space-y-4">
                            <!-- Amount Input -->
                            <div>
                                <label class="block text-sm font-medium text-gray-300 mb-2">Amount</label>
                                <input
                                    type="text"
                                    inputmode="decimal"
                                    id="currencyAmount"
                                    value="1"
                                    class="currency-input w-full px-3 py-3 rounded-lg text-white"
                                    placeholder="Enter amount..."
                                />
                            </div>

                            <!-- Currency Selection -->
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-300 mb-2">From</label>
                                    <select id="currencyFrom" class="currency-select w-full px-3 py-3 rounded-lg text-white">
                                        <option value="USD">USD - US Dollar</option>
                                        <option value="EUR">EUR - Euro</option>
                                        <option value="GBP">GBP - British Pound</option>
                                        <option value="JPY">JPY - Japanese Yen</option>
                                        <option value="INR">INR - Indian Rupee</option>
                                        <option value="CAD">CAD - Canadian Dollar</option>
                                        <option value="AUD">AUD - Australian Dollar</option>
                                        <option value="CNY">CNY - Chinese Yuan</option>
                                        <option value="CHF">CHF - Swiss Franc</option>
                                        <option value="SEK">SEK - Swedish Krona</option>
                                    </select>
                                </div>

                                <div>
                                    <label class="block text-sm font-medium text-gray-300 mb-2">To</label>
                                    <select id="currencyTo" class="currency-select w-full px-3 py-3 rounded-lg text-white">
                                        <option value="USD">USD - US Dollar</option>
                                        <option value="EUR" selected>EUR - Euro</option>
                                        <option value="GBP">GBP - British Pound</option>
                                        <option value="JPY">JPY - Japanese Yen</option>
                                        <option value="INR">INR - Indian Rupee</option>
                                        <option value="CAD">CAD - Canadian Dollar</option>
                                        <option value="AUD">AUD - Australian Dollar</option>
                                        <option value="CNY">CNY - Chinese Yuan</option>
                                        <option value="CHF">CHF - Swiss Franc</option>
                                        <option value="SEK">SEK - Swedish Krona</option>
                                    </select>
                                </div>
                            </div>

                            <!-- Result Display -->
                            <div class="currency-result mt-4 p-4 rounded-lg">
                                <div class="text-center">
                                    <div class="text-sm text-green-200 mb-1">Converted Amount</div>
                                    <div id="currencyResult" class="text-2xl font-bold text-green-100">0.92 EUR</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- History -->
                <div id="historyMode" class="mode-content hidden">
                    <div class="space-y-4">
                        <div class="flex items-center justify-between">
                            <h3 class="text-lg sm:text-xl font-semibold text-white">History</h3>
                            <button
                                id="clearHistoryBtn"
                                class="px-3 py-1 text-sm bg-red-600 hover:bg-red-500 text-white rounded-lg transition-colors duration-200 hidden"
                            >
                                Clear All
                            </button>
                        </div>

                        <div id="historyContainer" class="max-h-60 sm:max-h-80 overflow-y-auto space-y-2">
                            <div class="text-center text-gray-400 py-8">
                                No calculations yet
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Calculator state
        let currentInput = '0';
        let previousInput = '';
        let operator = '';
        let history = [];
        let activeMode = 'standard';
        let isMenuOpen = false;

        // Touch/gesture variables
        let touchStartX = 0;
        let touchStartY = 0;
        let touchEndX = 0;
        let touchEndY = 0;
        let isLongPress = false;
        let longPressTimer;

        // Currency rates
        const currencyRates = {
            USD: 1,
            EUR: 0.92,
            GBP: 0.79,
            JPY: 158.85,
            INR: 83.5,
            CAD: 1.37,
            AUD: 1.50,
            CNY: 7.24,
            CHF: 0.88,
            SEK: 10.45
        };

        // DOM elements
        const currentDisplay = document.getElementById('currentDisplay');
        const previousDisplay = document.getElementById('previousDisplay');
        const menuToggle = document.getElementById('menuToggle');
        const menuDropdown = document.getElementById('menuDropdown');
        const menuIcon = document.getElementById('menuIcon');
        const closeIcon = document.getElementById('closeIcon');

        // Enhanced animations
        function createRippleEffect(element, event) {
            const ripple = document.createElement('span');
            const rect = element.getBoundingClientRect();
            const size = Math.max(rect.width, rect.height);
            const x = event.clientX - rect.left - size / 2;
            const y = event.clientY - rect.top - size / 2;
            
            ripple.style.width = ripple.style.height = size + 'px';
            ripple.style.left = x + 'px';
            ripple.style.top = y + 'px';
            ripple.classList.add('ripple-effect');
            
            element.appendChild(ripple);
            
            setTimeout(() => {
                ripple.remove();
            }, 600);
        }

        function createParticles(element) {
            const rect = element.getBoundingClientRect();
            const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#06b6d4'];
            
            for (let i = 0; i < 8; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.background = colors[Math.floor(Math.random() * colors.length)];
                particle.style.left = (rect.left + rect.width / 2) + 'px';
                particle.style.top = (rect.top + rect.height / 2) + 'px';
                particle.style.transform = `translate(${Math.random() * 80 - 40}px, ${Math.random() * 80 - 40}px)`;
                particle.style.animationDelay = `${Math.random() * 0.5}s`;
                
                document.body.appendChild(particle);
                
                setTimeout(() => {
                    particle.remove();
                }, 2500);
            }
        }

        function animateButton(button, type = 'default') {
            button.classList.add('ripple');
            
            // Enhanced glow effects
            if (type === 'calculate') {
                button.classList.add('glow-green');
            } else if (type === 'operator') {
                button.classList.add('glow-orange');
            } else if (type === 'currency') {
                button.classList.add('glow-purple');
            } else if (type === 'scientific') {
                button.classList.add('glow-blue');
            } else {
                button.classList.add('glow');
            }
            
            setTimeout(() => {
                button.classList.remove('ripple', 'glow', 'glow-green', 'glow-orange', 'glow-purple', 'glow-blue');
            }, 600);
        }

        function pulseDisplay() {
            currentDisplay.classList.add('pulse');
            setTimeout(() => {
                currentDisplay.classList.remove('pulse');
            }, 400);
        }

        function shakeDisplay() {
            currentDisplay.classList.add('shake');
            setTimeout(() => {
                currentDisplay.classList.remove('shake');
            }, 600);
        }

        // Update display with animation
        function updateDisplay() {
            currentDisplay.textContent = currentInput;
            previousDisplay.textContent = previousInput && operator ? `${previousInput} ${operator}` : '';
            pulseDisplay();
        }

        // Input number with animation
        function inputNumber(num) {
            currentInput = currentInput === '0' ? num : currentInput + num;
            updateDisplay();
        }

        // Input decimal
        function inputDecimal() {
            if (!currentInput.includes('.')) {
                currentInput += '.';
                updateDisplay();
            }
        }

        // Input operator with animation
        function inputOperator(op) {
            if (currentInput === '') return;
            if (previousInput !== '') calculate();
            operator = op;
            previousInput = currentInput;
            currentInput = '';
            updateDisplay();
        }

        // Calculate with enhanced feedback
        function calculate() {
            const a = parseFloat(previousInput);
            const b = parseFloat(currentInput);
            let result;

            switch (operator) {
                case '+': result = a + b; break;
                case '−': result = a - b; break;
                case '×': result = a * b; break;
                case '÷': 
                    if (b === 0) {
                        result = 'Error';
                        shakeDisplay();
                    } else {
                        result = a / b;
                    }
                    break;
                case '^': result = Math.pow(a, b); break;
                default: return;
            }

            if (result !== 'Error') {
                const historyItem = {
                    expression: `${previousInput} ${operator} ${currentInput}`,
                    result: result.toString(),
                    timestamp: new Date()
                };
                history.unshift(historyItem);
                if (history.length > 50) history.pop();
                updateHistoryDisplay();
                
                // Create celebration particles for successful calculation
                createParticles(currentDisplay);
            }

            currentInput = result.toString();
            previousInput = '';
            operator = '';
            updateDisplay();
        }

        // Clear all
        function clearAll() {
            currentInput = '0';
            previousInput = '';
            operator = '';
            updateDisplay();
        }

        // Clear entry
        function clearEntry() {
            currentInput = '0';
            updateDisplay();
        }

        // Delete last
        function deleteLast() {
            currentInput = currentInput.length > 1 ? currentInput.slice(0, -1) : '0';
            updateDisplay();
        }

        // Scientific functions with enhanced feedback
        function scientificFunction(func) {
            const value = parseFloat(currentInput);
            let result;

            switch (func) {
                case 'sin': result = Math.sin(value * Math.PI / 180); break;
                case 'cos': result = Math.cos(value * Math.PI / 180); break;
                case 'tan': result = Math.tan(value * Math.PI / 180); break;
                case 'log': result = Math.log10(value); break;
                case 'ln': result = Math.log(value); break;
                case 'sqrt': result = Math.sqrt(value); break;
                case 'square': result = value * value; break;
                case 'factorial': result = factorial(value); break;
                case 'inverse': result = 1 / value; break;
                case 'percent': result = value / 100; break;
                default: result = value;
            }

            if (isFinite(result)) {
                currentInput = result.toString();
                createParticles(currentDisplay);
            } else {
                currentInput = 'Error';
                shakeDisplay();
            }
            updateDisplay();
        }

        // Factorial function
        function factorial(n) {
            if (n < 0 || !Number.isInteger(n) || n > 170) return NaN;
            let res = 1;
            for (let i = 2; i <= n; i++) res *= i;
            return res;
        }

        // Simplified Currency Converter
        function convertCurrency() {
            const amount = parseFloat(document.getElementById('currencyAmount').value) || 0;
            const from = document.getElementById('currencyFrom').value;
            const to = document.getElementById('currencyTo').value;
            
            const result = (amount / currencyRates[from]) * currencyRates[to];
            
            document.getElementById('currencyResult').textContent = `${result.toFixed(2)} ${to}`;
        }

        // Simplified input handling
        function setupCurrencyInput() {
            const input = document.getElementById('currencyAmount');
            
            // Disable arrow key incrementing
            input.addEventListener('keydown', function(e) {
                if (e.key === 'ArrowUp' || e.key === 'ArrowDown') {
                    e.preventDefault();
                    return false;
                }
                
                const allowedKeys = [
                    'Backspace', 'Delete', 'Tab', 'Escape', 'Enter',
                    'ArrowLeft', 'ArrowRight', 'Home', 'End'
                ];
                
                const isNumber = /^[0-9]$/.test(e.key);
                const isDecimal = e.key === '.' && !input.value.includes('.');
                
                if (!allowedKeys.includes(e.key) && !isNumber && !isDecimal) {
                    e.preventDefault();
                    return false;
                }
            });

            input.addEventListener('input', function(e) {
                let value = e.target.value;
                value = value.replace(/[^0-9.]/g, '');
                
                const parts = value.split('.');
                if (parts.length > 2) {
                    value = parts[0] + '.' + parts.slice(1).join('');
                }
                
                if (parts[1] && parts[1].length > 4) {
                    value = parts[0] + '.' + parts[1].substring(0, 4);
                }
                
                if (value.length > 12) {
                    value = value.substring(0, 12);
                }
                
                e.target.value = value;
                convertCurrency();
            });
        }

        // Update history display with animations
        function updateHistoryDisplay() {
            const container = document.getElementById('historyContainer');
            const clearBtn = document.getElementById('clearHistoryBtn');
            
            if (history.length === 0) {
                container.innerHTML = '<div class="text-center text-gray-400 py-8">No calculations yet</div>';
                clearBtn.classList.add('hidden');
            } else {
                clearBtn.classList.remove('hidden');
                container.innerHTML = history.map((item, index) => `
                    <div class="bg-gray-800 rounded-lg p-3 hover:bg-gray-700 transition-all duration-200 cursor-pointer border border-gray-600 transform hover:scale-105"
                         onclick="setCurrentInput('${item.result}')" style="animation: slideIn 0.3s ease forwards; animation-delay: ${index * 0.1}s; opacity: 0;">
                        <div class="text-gray-200 font-medium text-sm sm:text-base">
                            ${item.expression} = ${item.result}
                        </div>
                        <div class="text-xs text-gray-400 mt-1">
                            ${item.timestamp.toLocaleString()}
                        </div>
                    </div>
                `).join('');
            }
        }

        // Set current input with animation
        function setCurrentInput(value) {
            currentInput = value;
            updateDisplay();
            createParticles(currentDisplay);
        }

        // Toggle menu with animation
        function toggleMenu() {
            isMenuOpen = !isMenuOpen;
            if (isMenuOpen) {
                menuDropdown.classList.remove('max-h-0', 'opacity-0');
                menuDropdown.classList.add('max-h-64', 'opacity-100');
                menuIcon.classList.add('hidden');
                closeIcon.classList.remove('hidden');
            } else {
                menuDropdown.classList.add('max-h-0', 'opacity-0');
                menuDropdown.classList.remove('max-h-64', 'opacity-100');
                menuIcon.classList.remove('hidden');
                closeIcon.classList.add('hidden');
            }
        }

        // Switch mode with enhanced transitions
        function switchMode(mode) {
            activeMode = mode;
            
            // Hide all mode contents with stagger effect
            const modeContents = document.querySelectorAll('.mode-content');
            modeContents.forEach((content, index) => {
                setTimeout(() => {
                    content.classList.remove('active');
                    content.classList.add('hidden');
                }, index * 50);
            });
            
            // Show selected mode content with delay
            setTimeout(() => {
                document.getElementById(mode + 'Mode').classList.remove('hidden');
                document.getElementById(mode + 'Mode').classList.add('active');
            }, modeContents.length * 50 + 100);
            
            // Update mode buttons
            document.querySelectorAll('.mode-btn').forEach(btn => {
                btn.classList.remove('bg-blue-600', 'text-white', 'active');
                btn.classList.add('text-gray-300', 'hover:bg-gray-700');
            });
            
            const activeBtn = document.querySelector(`[data-mode="${mode}"]`);
            activeBtn.classList.remove('text-gray-300', 'hover:bg-gray-700');
            activeBtn.classList.add('bg-blue-600', 'text-white', 'active');
            
            // Animate the button
            activeBtn.style.transform = 'scale(1.05)';
            setTimeout(() => {
                activeBtn.style.transform = 'scale(1)';
            }, 200);
            
            // Close menu
            if (isMenuOpen) toggleMenu();
            
            // Auto-convert currency if in currency mode
            if (mode === 'currency') {
                setTimeout(() => {
                    convertCurrency();
                    setupCurrencyInput();
                }, 300);
            }
        }

        // Enhanced touch handling
        function handleTouchStart(e) {
            touchStartX = e.touches[0].clientX;
            touchStartY = e.touches[0].clientY;
            
            // Long press detection
            if (e.target.classList.contains('calc-btn')) {
                longPressTimer = setTimeout(() => {
                    isLongPress = true;
                    if (navigator.vibrate) {
                        navigator.vibrate(50);
                    }
                    createParticles(e.target);
                }, 500);
            }
        }

        function handleTouchMove(e) {
            clearTimeout(longPressTimer);
            isLongPress = false;
        }

        function handleTouchEnd(e) {
            clearTimeout(longPressTimer);
            touchEndX = e.changedTouches[0].clientX;
            touchEndY = e.changedTouches[0].clientY;
            
            if (!isLongPress) {
                handleSwipe();
            }
            
            isLongPress = false;
        }

        function handleSwipe() {
            const deltaX = touchEndX - touchStartX;
            const deltaY = touchEndY - touchStartY;
            const minSwipeDistance = 50;
            
            if (Math.abs(deltaX) > Math.abs(deltaY) && Math.abs(deltaX) > minSwipeDistance) {
                // Horizontal swipe
                if (deltaX > 0) {
                    // Swipe right - previous mode
                    switchToPreviousMode();
                } else {
                    // Swipe left - next mode
                    switchToNextMode();
                }
            }
        }

        function switchToNextMode() {
            const modes = ['standard', 'scientific', 'currency', 'history'];
            const currentIndex = modes.indexOf(activeMode);
            const nextIndex = (currentIndex + 1) % modes.length;
            switchMode(modes[nextIndex]);
        }

        function switchToPreviousMode() {
            const modes = ['standard', 'scientific', 'currency', 'history'];
            const currentIndex = modes.indexOf(activeMode);
            const prevIndex = (currentIndex - 1 + modes.length) % modes.length;
            switchMode(modes[prevIndex]);
        }

        // Event listeners
        menuToggle.addEventListener('click', toggleMenu);

        // Mode button listeners
        document.querySelectorAll('.mode-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                animateButton(btn);
                switchMode(btn.dataset.mode);
            });
        });

        // Enhanced calculator button listeners
        document.addEventListener('click', (e) => {
            if (e.target.classList.contains('calc-btn')) {
                const action = e.target.dataset.action;
                const value = e.target.dataset.value;
                
                // Create ripple effect
                createRippleEffect(e.target, e);
                
                // Animate button based on type
                let buttonType = 'default';
                if (action === 'calculate') buttonType = 'calculate';
                else if (action === 'operator') buttonType = 'operator';
                else if (action === 'scientific') buttonType = 'scientific';
                
                animateButton(e.target, buttonType);
                
                // Vibrate on touch devices
                if (navigator.vibrate) {
                    navigator.vibrate(30);
                }
                
                switch (action) {
                    case 'number':
                        inputNumber(value);
                        break;
                    case 'decimal':
                        inputDecimal();
                        break;
                    case 'operator':
                        inputOperator(value);
                        break;
                    case 'calculate':
                        calculate();
                        break;
                    case 'clear-all':
                        clearAll();
                        break;
                    case 'clear-entry':
                        clearEntry();
                        break;
                    case 'delete':
                        deleteLast();
                        break;
                    case 'scientific':
                        scientificFunction(value);
                        break;
                }
            }
        });

        // Touch event listeners for enhanced gestures
        document.addEventListener('touchstart', handleTouchStart, { passive: true });
        document.addEventListener('touchmove', handleTouchMove, { passive: true });
        document.addEventListener('touchend', handleTouchEnd, { passive: true });

        // Currency converter listeners
        document.getElementById('currencyFrom').addEventListener('change', convertCurrency);
        document.getElementById('currencyTo').addEventListener('change', convertCurrency);

        // Clear history button with animation
        document.getElementById('clearHistoryBtn').addEventListener('click', (e) => {
            animateButton(e.target);
            
            // Animate history items out
            const historyItems = document.querySelectorAll('#historyContainer > div');
            historyItems.forEach((item, index) => {
                setTimeout(() => {
                    item.style.transform = 'scale(0)';
                    item.style.opacity = '0';
                }, index * 50);
            });
            
            setTimeout(() => {
                history = [];
                updateHistoryDisplay();
            }, historyItems.length * 50 + 200);
        });

        // Enhanced keyboard support with no arrow key conflicts
        document.addEventListener('keydown', (e) => {
            // Skip if focused on currency input
            if (document.activeElement.id === 'currencyAmount') {
                return;
            }
            
            const key = e.key;
            
            // Find and animate the corresponding button
            let buttonSelector = '';
            
            if (!isNaN(Number(key))) {
                buttonSelector = `[data-action="number"][data-value="${key}"]`;
                inputNumber(key);
            } else if (key === '.' || key === ',') {
                buttonSelector = '[data-action="decimal"]';
                inputDecimal();
            } else if (['+', '-', '*', '/', '^'].includes(key)) {
                const ops = { '*': '×', '/': '÷', '+': '+', '-': '−', '^': '^' };
                buttonSelector = `[data-action="operator"][data-value="${ops[key]}"]`;
                inputOperator(ops[key]);
            } else if (key === 'Enter' || key === '=') {
                buttonSelector = '[data-action="calculate"]';
                calculate();
            } else if (key === 'Backspace') {
                buttonSelector = '[data-action="delete"]';
                deleteLast();
            } else if (key.toLowerCase() === 'c') {
                buttonSelector = '[data-action="clear-all"]';
                clearAll();
            } else if (key === 'Escape') {
                buttonSelector = '[data-action="clear-entry"]';
                clearEntry();
            }
            
            // Animate the corresponding button
            if (buttonSelector) {
                e.preventDefault();
                const button = document.querySelector(buttonSelector);
                if (button) {
                    animateButton(button);
                    createRippleEffect(button, { 
                        clientX: button.offsetLeft + button.offsetWidth / 2, 
                        clientY: button.offsetTop + button.offsetHeight / 2 
                    });
                }
            }
        });

        // Initialize
        updateDisplay();
        convertCurrency();
        setupCurrencyInput();
    </script>
</body>
</html>
