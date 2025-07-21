<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Enhanced Calculator Pro</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Inter', sans-serif;
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a2e 25%, #16213e 50%, #1a1a2e 75%, #0a0a0a 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .calculator-container {
            background: linear-gradient(145deg, rgba(10, 10, 10, 0.95), rgba(26, 26, 46, 0.9), rgba(22, 33, 62, 0.85));
            backdrop-filter: blur(20px);
            border-radius: 32px;
            padding: 32px;
            box-shadow: 
                25px 25px 80px rgba(0, 0, 0, 0.9),
                -25px -25px 80px rgba(255, 255, 255, 0.02),
                inset 2px 2px 8px rgba(255, 255, 255, 0.05),
                0 0 100px rgba(255, 69, 0, 0.03);
            max-width: 450px;
            width: 100%;
            position: relative; /* Keep this for dropdown menu positioning */
            border: 1px solid rgba(255, 255, 255, 0.08);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            overflow: hidden; /* Ensure content transitions smoothly */
        }

        .calculator-container:hover {
            box-shadow: 
                30px 30px 120px rgba(0, 0, 0, 0.95),
                -30px -30px 120px rgba(255, 255, 255, 0.03),
                inset 2px 2px 8px rgba(255, 255, 255, 0.08),
                0 0 150px rgba(255, 69, 0, 0.05);
            transform: translateY(-3px);
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
        }

        .calculator-title {
            font-size: 24px;
            font-weight: 700;
            background: linear-gradient(135deg, #ffffff 0%, #e0e0e0 50%, #ffffff 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 30px rgba(255, 255, 255, 0.3);
            letter-spacing: -0.5px;
        }

        .menu-btn {
            background: linear-gradient(145deg, #1a1a2e, #0a0a0a);
            border: none;
            border-radius: 50%;
            width: 48px;
            height: 48px;
            color: #ffffff;
            cursor: pointer;
            font-size: 24px;
            box-shadow: 
                8px 8px 20px rgba(0, 0, 0, 0.8),
                -8px -8px 20px rgba(255, 255, 255, 0.03);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            border: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .menu-btn:hover {
            background: linear-gradient(145deg, #2a2a3e, #1a1a2e);
            box-shadow: 
                12px 12px 30px rgba(0, 0, 0, 0.9),
                -12px -12px 30px rgba(255, 255, 255, 0.05);
            transform: translateY(-2px);
        }

        .menu-btn:active {
            transform: scale(0.95) translateY(1px);
            box-shadow: inset 6px 6px 15px rgba(0, 0, 0, 0.8);
        }

        .dropdown-menu {
            position: absolute;
            top: 55px;
            right: 0;
            background: linear-gradient(145deg, rgba(26, 26, 46, 0.98), rgba(10, 10, 10, 0.95));
            backdrop-filter: blur(25px);
            border-radius: 20px;
            padding: 16px;
            box-shadow: 
                0 25px 80px rgba(0, 0, 0, 0.9),
                0 0 50px rgba(255, 69, 0, 0.08);
            z-index: 1000;
            opacity: 0;
            transform: translateY(-20px) scale(0.9);
            pointer-events: none;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            border: 1px solid rgba(255, 255, 255, 0.1);
            min-width: 200px;
        }

        .dropdown-menu.active {
            opacity: 1;
            transform: translateY(0) scale(1);
            pointer-events: all;
        }

        .dropdown-item {
            background: none;
            border: none;
            color: #ffffff;
            padding: 16px 24px;
            cursor: pointer;
            border-radius: 16px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            width: 100%;
            text-align: left;
            font-size: 16px;
            font-weight: 500;
            margin-bottom: 6px;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .dropdown-item:hover {
            background: linear-gradient(135deg, rgba(255, 69, 0, 0.15), rgba(255, 69, 0, 0.08));
            transform: translateX(8px);
            box-shadow: 0 8px 25px rgba(255, 69, 0, 0.2);
        }

        .screen-container {
            background: linear-gradient(145deg, rgba(0, 0, 0, 0.9), rgba(10, 10, 10, 0.8));
            backdrop-filter: blur(10px);
            border-radius: 28px;
            padding: 32px 28px;
            margin-bottom: 32px;
            box-shadow: 
                inset 12px 12px 35px rgba(0, 0, 0, 0.9),
                inset -6px -6px 15px rgba(255, 255, 255, 0.02);
            min-height: 160px;
            position: relative;
            border: 1px solid rgba(255, 255, 255, 0.06);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .history-display {
            color: #888888;
            font-size: 18px;
            text-align: right;
            margin-bottom: 12px;
            min-height: 28px;
            overflow: hidden;
            font-weight: 300;
            opacity: 0.8;
        }

        .main-display {
            color: #ffffff;
            font-size: 48px;
            text-align: right;
            font-weight: 200;
            line-height: 1.1;
            word-break: break-all;
            min-height: 55px;
            text-shadow: 0 0 30px rgba(255, 255, 255, 0.15);
            letter-spacing: -1px;
        }

        .buttons-grid, .converter-section {
            padding-top: 20px; /* Add some top padding to separate from screen */
            transition: all 0.4s ease-in-out;
            transform: scale(1);
            opacity: 1;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
        }

        .buttons-grid.scientific {
            grid-template-columns: repeat(5, 1fr);
            gap: 16px;
        }
        
        /* New styles for showing/hiding sections */
        .hidden-section {
            opacity: 0;
            transform: scale(0.95);
            pointer-events: none;
            display: none; /* Hide when not active */
        }

        /* Override display for converter-section to be block for its internal flex/grid */
        .converter-section:not(.hidden-section) {
            display: flex; /* Or whatever layout you need for its children */
            flex-direction: column;
        }

        .btn {
            background: linear-gradient(145deg, #1a1a2e, #0a0a0a);
            border: none;
            border-radius: 22px;
            height: 80px;
            color: #ffffff;
            font-size: 26px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 
                8px 8px 25px rgba(0, 0, 0, 0.8),
                -8px -8px 25px rgba(255, 255, 255, 0.03);
            user-select: none;
            border: 1px solid rgba(255, 255, 255, 0.08);
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.08), transparent);
            transition: left 0.6s;
        }

        .btn:hover::before {
            left: 100%;
        }

        .btn:active {
            transform: scale(0.95) translateY(2px);
            box-shadow: 
                inset 8px 8px 20px rgba(0, 0, 0, 0.9),
                inset -4px -4px 10px rgba(255, 255, 255, 0.02);
        }

        .btn:hover {
            background: linear-gradient(145deg, #2a2a3e, #1a1a2e);
            box-shadow: 
                12px 12px 35px rgba(0, 0, 0, 0.9),
                -12px -12px 35px rgba(255, 255, 255, 0.05);
            transform: translateY(-3px);
        }

        .btn.operator {
            background: linear-gradient(145deg, #ff6600, #ff4500);
            color: #ffffff;
            border: 1px solid rgba(255, 102, 0, 0.3);
            box-shadow: 
                8px 8px 25px rgba(0, 0, 0, 0.8),
                -8px -8px 25px rgba(255, 102, 0, 0.05),
                0 0 40px rgba(255, 102, 0, 0.15);
        }

        .btn.operator:hover {
            background: linear-gradient(145deg, #ff7700, #ff5500);
            box-shadow: 
                12px 12px 35px rgba(0, 0, 0, 0.9),
                -12px -12px 35px rgba(255, 102, 0, 0.08),
                0 0 50px rgba(255, 102, 0, 0.25);
            transform: translateY(-3px);
        }

        .btn.equals {
            background: linear-gradient(145deg, #00cc44, #00aa33);
            grid-column: span 2;
            border: 1px solid rgba(0, 204, 68, 0.3);
            box-shadow: 
                8px 8px 25px rgba(0, 0, 0, 0.8),
                -8px -8px 25px rgba(0, 204, 68, 0.05),
                0 0 40px rgba(0, 204, 68, 0.15);
        }

        .btn.equals:hover {
            background: linear-gradient(145deg, #00dd55, #00cc44);
            box-shadow: 
                12px 12px 35px rgba(0, 0, 0, 0.9),
                -12px -12px 35px rgba(0, 204, 68, 0.08),
                0 0 50px rgba(0, 204, 68, 0.25);
            transform: translateY(-3px);
        }

        .btn.zero {
            grid-column: span 2;
        }

        .btn.clear {
            background: linear-gradient(145deg, #ee0000, #cc0000);
            border: 1px solid rgba(238, 0, 0, 0.3);
            box-shadow: 
                8px 8px 25px rgba(0, 0, 0, 0.8),
                -8px -8px 25px rgba(238, 0, 0, 0.05),
                0 0 40px rgba(238, 0, 0, 0.15);
        }

        .btn.clear:hover {
            background: linear-gradient(145deg, #ff1100, #ee0000);
            box-shadow: 
                12px 12px 35px rgba(0, 0, 0, 0.9),
                -12px -12px 35px rgba(238, 0, 0, 0.08),
                0 0 50px rgba(238, 0, 0, 0.25);
            transform: translateY(-3px);
        }

        .btn.scientific {
            background: linear-gradient(145deg, #6a1b9a, #4a148c);
            font-size: 20px;
            border: 1px solid rgba(106, 27, 154, 0.3);
            height: 70px;
            box-shadow: 
                8px 8px 25px rgba(0, 0, 0, 0.8),
                -8px -8px 25px rgba(106, 27, 154, 0.05),
                0 0 40px rgba(106, 27, 154, 0.15);
        }

        .btn.scientific:hover {
            background: linear-gradient(145deg, #7a2baa, #5a1a9c);
            box-shadow: 
                12px 12px 35px rgba(0, 0, 0, 0.9),
                -12px -12px 35px rgba(106, 27, 154, 0.08),
                0 0 50px rgba(106, 27, 154, 0.25);
            transform: translateY(-3px);
        }

        .panel { /* This class is now only for history panel */
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(145deg, rgba(10, 10, 10, 0.98), rgba(26, 26, 46, 0.95));
            backdrop-filter: blur(25px);
            border-radius: 32px;
            padding: 32px;
            transform: translateX(100%) scale(0.95);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            z-index: 100;
            display: flex;
            flex-direction: column;
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .panel.active {
            transform: translateX(0) scale(1);
        }

        .panel-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
        }

        .panel-title {
            color: #ffffff;
            font-size: 24px;
            font-weight: 700;
            background: linear-gradient(135deg, #ffffff, #e0e0e0);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .close-btn {
            background: linear-gradient(145deg, #ee0000, #cc0000);
            border: none;
            color: #ffffff;
            font-size: 24px;
            cursor: pointer;
            padding: 12px;
            border-radius: 50%;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            width: 44px;
            height: 44px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .close-btn:hover {
            background: linear-gradient(145deg, #ff1100, #ee0000);
            transform: rotate(90deg) scale(1.1);
        }

        .history-list {
            flex: 1;
            overflow-y: auto;
            color: #ffffff;
            max-height: 400px;
        }

        .history-item {
            padding: 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.08);
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border-radius: 16px;
            margin-bottom: 12px;
        }

        .history-item:hover {
            background: linear-gradient(135deg, rgba(255, 69, 0, 0.08), rgba(255, 69, 0, 0.04));
            transform: translateX(12px);
            box-shadow: 0 8px 25px rgba(255, 69, 0, 0.15);
        }

        .history-expression {
            font-size: 16px;
            color: #aaaaaa;
            margin-bottom: 8px;
            font-weight: 400;
        }

        .history-result {
            font-size: 20px;
            color: #ffffff;
            font-weight: 600;
        }

        .clear-history-btn {
            background: linear-gradient(145deg, #ee0000, #cc0000);
            border: none;
            border-radius: 16px;
            color: #ffffff;
            padding: 16px;
            cursor: pointer;
            margin-top: 20px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-weight: 600;
            font-size: 16px;
        }

        .clear-history-btn:hover {
            background: linear-gradient(145deg, #ff1100, #ee0000);
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(238, 0, 0, 0.3);
        }

        /* Converter Styles */
        .converter-controls {
            margin: 20px 0;
            flex-grow: 1; /* Allow converter content to take available space */
            display: flex;
            flex-direction: column;
            justify-content: center; /* Vertically center controls */
        }

        .converter-select, .converter-input {
            background: linear-gradient(145deg, #1a1a2e, #0a0a0a);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            color: #ffffff;
            padding: 16px;
            width: 100%;
            margin-bottom: 16px;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            font-size: 16px;
            font-weight: 500;
        }

        .converter-select {
            appearance: none;
            background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='white' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6,9 12,15 18,9'%3e%3c/polyline%3e%3c/svg%3e");
            background-repeat: no-repeat;
            background-position: right 12px center;
            background-size: 18px;
            padding-right: 45px;
            cursor: pointer;
        }

        .converter-select:focus, .converter-input:focus {
            border-color: #ff6600;
            box-shadow: 0 0 25px rgba(255, 102, 0, 0.3);
            outline: none;
            transform: translateY(-2px);
        }

        .converter-result {
            background: linear-gradient(145deg, rgba(0, 0, 0, 0.9), rgba(10, 10, 10, 0.8));
            border-radius: 16px;
            padding: 20px;
            color: #ffffff;
            font-size: 24px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.06);
            box-shadow: inset 6px 6px 15px rgba(0, 0, 0, 0.8);
            font-weight: 600;
            text-align: center;
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.15);
        }

        .conversion-arrow {
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 16px 0;
            color: #ff6600;
            font-size: 28px;
            font-weight: bold;
        }

        /* Enhanced Responsive Design */
        
        /* Large Desktop (1440px+) */
        @media (min-width: 1440px) {
            .calculator-container {
                max-width: 500px;
                padding: 36px;
            }
            
            .btn {
                height: 85px;
                font-size: 28px;
            }
            
            .main-display {
                font-size: 52px;
            }
            
            .buttons-grid {
                gap: 24px;
            }

            .buttons-grid.scientific {
                gap: 18px;
            }
        }
        
        /* Desktop (1024px - 1439px) */
        @media (min-width: 1024px) and (max-width: 1439px) {
            .calculator-container {
                max-width: 470px;
                padding: 34px;
            }
            
            .btn {
                height: 82px;
                font-size: 27px;
            }
            
            .main-display {
                font-size: 50px;
            }
        }
        
        /* Tablet Landscape (768px - 1023px) */
        @media (min-width: 768px) and (max-width: 1023px) {
            .calculator-container {
                max-width: 420px;
                padding: 30px;
            }
            
            .btn {
                height: 75px;
                font-size: 24px;
            }
            
            .main-display {
                font-size: 44px;
            }
            
            .buttons-grid {
                gap: 18px;
            }

            .buttons-grid.scientific {
                gap: 14px;
            }
        }
        
        /* Tablet Portrait & Large Mobile (481px - 767px) */
        @media (min-width: 481px) and (max-width: 767px) {
            body {
                padding: 16px;
            }
            
            .calculator-container {
                max-width: 400px;
                padding: 28px;
                border-radius: 28px;
            }
            
            .btn {
                height: 70px;
                font-size: 22px;
                border-radius: 20px;
            }
            
            .main-display {
                font-size: 40px;
            }
            
            .screen-container {
                padding: 28px 24px;
                min-height: 140px;
            }
            
            .btn.scientific {
                font-size: 18px;
                height: 64px;
            }
            
            .buttons-grid {
                gap: 16px;
            }

            .buttons-grid.scientific {
                gap: 12px;
            }
        }
        
        /* Mobile (320px - 480px) */
        @media (max-width: 480px) {
            body {
                padding: 12px;
            }
            
            .calculator-container {
                padding: 24px;
                margin: 8px;
                border-radius: 24px;
                max-width: 100%;
            }

            .btn {
                height: 65px;
                font-size: 20px;
                border-radius: 18px;
            }

            .main-display {
                font-size: 36px;
            }
            
            .history-display {
                font-size: 16px;
            }
            
            .screen-container {
                padding: 24px 20px;
                min-height: 120px;
                border-radius: 20px;
            }
            
            .calculator-title {
                font-size: 20px;
            }
            
            .menu-btn {
                width: 44px;
                height: 44px;
                font-size: 22px;
            }

            .btn.scientific {
                font-size: 16px;
                height: 58px;
            }
            
            .buttons-grid {
                gap: 14px;
            }

            .buttons-grid.scientific {
                gap: 10px;
            }
            
            .dropdown-menu {
                right: -8px;
                padding: 14px;
                min-width: 180px;
            }
            
            .dropdown-item {
                padding: 14px 20px;
                font-size: 15px;
            }
        }

        /* Small Mobile (320px - 375px) */
        @media (max-width: 360px) {
            .calculator-container {
                padding: 20px;
                border-radius: 20px;
            }
            
            .btn {
                height: 60px;
                font-size: 18px;
                border-radius: 16px;
            }

            .main-display {
                font-size: 32px;
            }
            
            .screen-container {
                padding: 22px 18px;
                min-height: 110px;
                border-radius: 18px;
            }
            
            .btn.scientific {
                font-size: 14px;
                height: 54px;
            }

            .buttons-grid {
                gap: 12px;
            }

            .buttons-grid.scientific {
                gap: 8px;
            }
            
            .calculator-title {
                font-size: 18px;
            }
            
            .menu-btn {
                width: 40px;
                height: 40px;
                font-size: 20px;
            }
        }
        
        /* Extra Small Mobile (< 320px) */
        @media (max-width: 319px) {
            .calculator-container {
                padding: 16px;
                border-radius: 18px;
            }
            
            .btn {
                height: 55px;
                font-size: 16px;
                border-radius: 14px;
            }
            
            .main-display {
                font-size: 28px;
            }
            
            .screen-container {
                padding: 20px 16px;
                min-height: 100px;
                border-radius: 16px;
            }
            
            .btn.scientific {
                font-size: 12px;
                height: 50px;
            }
            
            .buttons-grid {
                gap: 10px;
            }

            .buttons-grid.scientific {
                gap: 6px;
            }
        }
        
        /* Landscape orientation for mobile */
        @media (max-height: 500px) and (orientation: landscape) {
            body {
                padding: 8px;
            }
            
            .calculator-container {
                padding: 16px;
                max-width: 90vw;
                max-height: 90vh;
            }
            
            .btn {
                height: 50px;
                font-size: 18px;
            }
            
            .main-display {
                font-size: 28px;
            }
            
            .screen-container {
                padding: 16px;
                min-height: 80px;
            }
            
            .buttons-grid {
                gap: 10px;
            }
            
            .btn.scientific {
                height: 45px;
                font-size: 16px;
            }

            .buttons-grid.scientific {
                gap: 8px;
            }
        }

        /* Animations */
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .calculator-container {
            animation: slideIn 0.6s ease-out;
        }

        .btn {
            animation: fadeIn 0.4s ease-out;
        }

        /* High contrast mode support */
        @media (prefers-contrast: high) {
            .calculator-container {
                border: 2px solid #ffffff;
            }
            
            .btn {
                border: 2px solid #ffffff;
            }
        }

        /* Reduced motion support */
        @media (prefers-reduced-motion: reduce) {
            * {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }
        }

        /* Dark mode support */
        @media (prefers-color-scheme: dark) {
            body {
                background: linear-gradient(135deg, #000000 0%, #1a1a2e 25%, #16213e 50%, #1a1a2e 75%, #000000 100%);
            }
        }
    </style>
</head>
<body>
    <div class="calculator-container" id="calculator">
        <div class="header">
            <div class="calculator-title">Calculator Pro</div>
            <button class="menu-btn" id="menuBtn">⋮</button>
            <div class="dropdown-menu" id="dropdownMenu">
                <button class="dropdown-item" onclick="toggleConverterView()">📐 Unit Converter</button>
                <button class="dropdown-item" onclick="toggleScientific()">🧮 <span id="scientificToggleText">Scientific</span></button>
                <button class="dropdown-item" onclick="toggleHistory()">📋 History</button>
            </div>
        </div>

        <div class="screen-container">
            <div class="history-display" id="historyDisplay"></div>
            <div class="main-display" id="mainDisplay">0</div>
        </div>

        <div id="mainContentArea">
            <div class="buttons-grid" id="buttonsGrid">
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
                <button class="btn operator" onclick="inputNumber('3')">3</button>
                <button class="btn operator" onclick="inputOperator('+')">+</button>

                <button class="btn zero" onclick="inputNumber('0')">0</button>
                <button class="btn" onclick="inputDecimal()">.</button>
                <button class="btn equals" onclick="calculate()">=</button>
            </div>

            <div class="converter-section hidden-section" id="converterSection">
                <div class="panel-header">
                    <div class="panel-title">Unit Converter</div>
                    <button class="close-btn" onclick="toggleConverterView()">×</button>
                </div>
                <div class="converter-controls">
                    <select class="converter-select" id="converterType" onchange="updateConverter()">
                        <option value="length">Length</option>
                        <option value="weight">Weight</option>
                        <option value="temperature">Temperature</option>
                        <option value="volume">Volume</option>
                    </select>
                    <select class="converter-select" id="fromUnit">
                        <option value="meter">Meter (m)</option>
                        <option value="kilometer">Kilometer (km)</option>
                        <option value="centimeter">Centimeter (cm)</option>
                        <option value="millimeter">Millimeter (mm)</option>
                        <option value="inch">Inch (in)</option>
                        <option value="foot">Foot (ft)</s</option>
                        <option value="yard">Yard (yd)</option>
                        <option value="mile">Mile (mi)</option>
                    </select>
                    <input type="number" class="converter-input" id="converterInput" placeholder="Enter value" oninput="convertValue()">
                    <div class="conversion-arrow">↓</div>
                    <select class="converter-select" id="toUnit">
                        <option value="meter">Meter (m)</option>
                        <option value="kilometer">Kilometer (km)</option>
                        <option value="centimeter">Centimeter (cm)</option>
                        <option value="millimeter">Millimeter (mm)</option>
                        <option value="inch">Inch (in)</option>
                        <option value="foot">Foot (ft)</option>
                        <option value="yard">Yard (yd)</option>
                        <option value="mile">Mile (mi)</option>
                    </select>
                    <div class="converter-result" id="converterResult">0</div>
                </div>
            </div>
        </div>

        <div class="panel" id="historyPanel">
            <div class="panel-header">
                <div class="panel-title">History</div>
                <button class="close-btn" onclick="toggleHistory()">×</button>
            </div>
            <div class="history-list" id="historyList">
                <div style="text-align: center; color: #666; margin-top: 50px;">No calculations yet</div>
            </div>
            <button class="clear-history-btn" onclick="clearHistory()">Clear History</button>
        </div>
    </div>

    <script>
        // Enhanced Calculator State Management
        class CalculatorState {
            constructor() {
                this.currentInput = '0';
                this.previousInput = '';
                this.operator = '';
                this.shouldResetDisplay = false;
                this.history = JSON.parse(localStorage.getItem('calculatorHistory')) || [];
                this.isScientificMode = false;
                this.isConverterMode = false; /* New state variable */
            }

            // Enhanced precision arithmetic
            static formatResult(num) {
                if (isNaN(num) || !isFinite(num)) return 'Error';
                if (Math.abs(num) > 1e15) return num.toExponential(6);
                if (Math.abs(num) < 1e-10 && num !== 0) return num.toExponential(6);
                
                // Format with proper precision
                const result = parseFloat(num.toPrecision(12));
                
                // Add comma separators for large numbers
                if (Math.abs(result) >= 1000) {
                    return result.toLocaleString('en-US', {
                        maximumFractionDigits: 8,
                        useGrouping: true
                    });
                }
                
                return result.toString();
            }

            static calculate(prev, current, operator) {
                const a = parseFloat(prev);
                const b = parseFloat(current);
                
                switch (operator) {
                    case '+': return CalculatorState.formatResult(a + b);
                    case '-': return CalculatorState.formatResult(a - b);
                    case '×': return CalculatorState.formatResult(a * b);
                    case '÷': 
                        if (b === 0) throw new Error('Division by zero');
                        return CalculatorState.formatResult(a / b);
                    case '^': return CalculatorState.formatResult(Math.pow(a, b));
                    case 'mod': return CalculatorState.formatResult(a % b); /* Added modulo */
                    default: throw new Error('Unknown operator');
                }
            }
        }

        // Global state instance
        const calc = new CalculatorState();

        // DOM Elements
        const mainDisplay = document.getElementById('mainDisplay');
        const historyDisplay = document.getElementById('historyDisplay');
        const menuBtn = document.getElementById('menuBtn');
        const dropdownMenu = document.getElementById('dropdownMenu');
        const buttonsGrid = document.getElementById('buttonsGrid');
        const calculator = document.getElementById('calculator');
        const scientificToggleText = document.getElementById('scientificToggleText');
        const converterSection = document.getElementById('converterSection'); /* Get converter section */

        // Enhanced Display Functions
        function updateDisplay() {
            mainDisplay.textContent = formatDisplayValue(calc.currentInput || '0');
            if (calc.previousInput && calc.operator) {
                historyDisplay.textContent = `${formatDisplayValue(calc.previousInput)} ${calc.operator}`;
            } else {
                historyDisplay.textContent = '';
            }
        }

        function formatDisplayValue(value) {
            if (!value || value === '0') return '0';
            const num = parseFloat(value);
            if (isNaN(num)) return 'Error';
            
            // Scientific notation for very large/small numbers
            if (Math.abs(num) >= 1e12 || (Math.abs(num) < 1e-6 && num !== 0)) {
                return num.toExponential(6);
            }
            
            // Regular formatting with commas for large numbers
            if (Math.abs(num) >= 1000) {
                return num.toLocaleString('en-US', {
                    maximumFractionDigits: 8,
                    useGrouping: true
                });
            }
            
            return num.toString();
        }

        // Enhanced Input Functions
        function inputNumber(num) {
            if (calc.shouldResetDisplay) {
                calc.currentInput = num;
                calc.shouldResetDisplay = false;
            } else {
                if (calc.currentInput.length < 15) {
                    calc.currentInput = calc.currentInput === '0' ? num : calc.currentInput + num;
                }
            }
            updateDisplay();
        }

        function inputDecimal() {
            if (calc.shouldResetDisplay) {
                calc.currentInput = '0.';
                calc.shouldResetDisplay = false;
            } else if (!calc.currentInput.includes('.')) {
                calc.currentInput = (calc.currentInput || '0') + '.';
            }
            updateDisplay();
        }

        function inputOperator(op) {
            if (calc.currentInput === '' && calc.previousInput === '') return;
            
            if (calc.previousInput !== '' && calc.currentInput !== '' && calc.operator !== '') {
                calculate();
            }
            
            calc.operator = op;
            calc.previousInput = calc.currentInput || calc.previousInput;
            calc.currentInput = '';
            updateDisplay();
        }

        function calculate() {
            if (calc.previousInput === '' || calc.currentInput === '' || calc.operator === '') return;
            
            try {
                const result = CalculatorState.calculate(calc.previousInput, calc.currentInput, calc.operator);
                
                // Add to history
                const calculation = {
                    id: Date.now(),
                    expression: `${calc.previousInput} ${calc.operator} ${calc.currentInput}`,
                    result: result,
                    timestamp: new Date().toLocaleString()
                };
                
                calc.history.unshift(calculation);
                if (calc.history.length > 50) calc.history.pop();
                localStorage.setItem('calculatorHistory', JSON.stringify(calc.history));
                updateHistoryDisplay();
                
                calc.currentInput = result.toString();
                calc.previousInput = '';
                calc.operator = '';
                calc.shouldResetDisplay = true;
                updateDisplay();
            } catch (error) {
                calc.currentInput = 'Error';
                calc.previousInput = '';
                calc.operator = '';
                calc.shouldResetDisplay = true;
                updateDisplay();
            }
        }

        function clearAll() {
            calc.currentInput = '0';
            calc.previousInput = '';
            calc.operator = '';
            calc.shouldResetDisplay = false;
            updateDisplay();
        }

        function clearEntry() {
            calc.currentInput = '0';
            updateDisplay();
        }

        function deleteLast() {
            if (calc.currentInput.length > 1) {
                calc.currentInput = calc.currentInput.slice(0, -1);
            } else {
                calc.currentInput = '0';
            }
            updateDisplay();
        }

        // Enhanced Scientific Functions
        function scientificFunction(func) {
            if (calc.currentInput === '' || calc.currentInput === '0') return;
            
            const value = parseFloat(calc.currentInput);
            let result;
            
            try {
                switch (func) {
                    case 'sin': result = Math.sin(value * Math.PI / 180); break;
                    case 'cos': result = Math.cos(value * Math.PI / 180); break;
                    case 'tan': result = Math.tan(value * Math.PI / 180); break;
                    case 'asin': result = Math.asin(value) * 180 / Math.PI; break;
                    case 'acos': result = Math.acos(value) * 180 / Math.PI; break;
                    case 'atan': result = Math.atan(value) * 180 / Math.PI; break;
                    case 'log': result = Math.log10(value); break;
                    case 'ln': result = Math.log(value); break;
                    case 'sqrt': result = Math.sqrt(value); break;
                    case 'cbrt': result = Math.cbrt(value); break;
                    case 'square': result = value * value; break;
                    case 'cube': result = value * value * value; break;
                    case 'factorial': result = factorial(value); break;
                    case 'inverse': result = 1 / value; break;
                    case 'percent': result = value / 100; break;
                    case 'negate': result = -value; break;
                    default: return;
                }
                
                calc.currentInput = CalculatorState.formatResult(result);
                calc.shouldResetDisplay = true;
                updateDisplay();
            } catch (error) {
                calc.currentInput = 'Error';
                calc.shouldResetDisplay = true;
                updateDisplay();
            }
        }

        function factorial(n) {
            if (n < 0 || n !== Math.floor(n) || n > 170) return NaN;
            if (n === 0 || n === 1) return 1;
            let result = 1;
            for (let i = 2; i <= n; i++) {
                result *= i;
            }
            return result;
        }

        // Scientific Mode Toggle
        const scientificButtonsConfig = [
            // Row 1
            { text: 'sin', func: 'scientificFunction("sin")', class: 'btn scientific' },
            { text: 'cos', func: 'scientificFunction("cos")', class: 'btn scientific' },
            { text: 'tan', func: 'scientificFunction("tan")', class: 'btn scientific' },
            { text: 'asin', func: 'scientificFunction("asin")', class: 'btn scientific' },
            { text: 'acos', func: 'scientificFunction("acos")', class: 'btn scientific' },
            { text: 'atan', func: 'scientificFunction("atan")', class: 'btn scientific' },
            
            // Row 2
            { text: 'log', func: 'scientificFunction("log")', class: 'btn scientific' },
            { text: 'ln', func: 'scientificFunction("ln")', class: 'btn scientific' },
            { text: 'xʸ', func: 'inputOperator("^")', class: 'btn scientific' },
            { text: 'mod', func: 'inputOperator("mod")', class: 'btn scientific' },
            { text: '√', func: 'scientificFunction("sqrt")', class: 'btn scientific' },
            { text: '³√', func: 'scientificFunction("cbrt")', class: 'btn scientific' },

            // Row 3
            { text: 'x²', func: 'scientificFunction("square")', class: 'btn scientific' },
            { text: 'x³', func: 'scientificFunction("cube")', class: 'btn scientific' },
            { text: 'x!', func: 'scientificFunction("factorial")', class: 'btn scientific' },
            { text: '1/x', func: 'scientificFunction("inverse")', class: 'btn scientific' },
            { text: '+/-', func: 'scientificFunction("negate")', class: 'btn scientific' },
            { text: '%', func: 'scientificFunction("percent")', class: 'btn scientific' },

            // Row 4
            { text: 'π', func: 'inputNumber("3.14159265359")', class: 'btn scientific' },
            { text: 'e', func: 'inputNumber("2.71828182846")', class: 'btn scientific' }
        ];

        function toggleScientific() {
            if (calc.isConverterMode) { // If converter is active, switch to calculator first
                toggleConverterView();
            }

            calc.isScientificMode = !calc.isScientificMode;
            buttonsGrid.classList.toggle('scientific', calc.isScientificMode);
            scientificToggleText.textContent = calc.isScientificMode ? 'Standard' : 'Scientific';
            
            if (calc.isScientificMode) {
                addScientificButtons();
            } else {
                removeScientificButtons();
            }
            closeMenu();
        }

        function addScientificButtons() {
            removeScientificButtons(); // Ensure no duplicates
            
            scientificButtonsConfig.forEach(btnConfig => {
                const button = document.createElement('button');
                button.className = btnConfig.class;
                button.textContent = btnConfig.text;
                button.setAttribute('onclick', btnConfig.func);
                buttonsGrid.insertBefore(button, buttonsGrid.children[0]); // Insert at the beginning for dynamic layout
            });
        }

        function removeScientificButtons() {
            const scientificBtns = document.querySelectorAll('.btn.scientific');
            scientificBtns.forEach(btn => btn.remove());
        }

        // Menu Functions
        function toggleMenu() {
            dropdownMenu.classList.toggle('active');
        }

        function closeMenu() {
            dropdownMenu.classList.remove('active');
        }

        // History Functions
        function toggleHistory() {
            const historyPanel = document.getElementById('historyPanel');
            historyPanel.classList.toggle('active');
            if (historyPanel.classList.contains('active')) {
                updateHistoryDisplay();
            }
            closeMenu();
        }

        function updateHistoryDisplay() {
            const historyList = document.getElementById('historyList');
            
            if (calc.history.length === 0) {
                historyList.innerHTML = '<div style="text-align: center; color: #666; margin-top: 50px;">No calculations yet</div>';
                return;
            }
            
            historyList.innerHTML = calc.history.map(item => `
                <div class="history-item" onclick="useHistoryResult('${item.result}')">
                    <div class="history-expression">${item.expression}</div>
                    <div class="history-result">= ${item.result}</div>
                    <div style="color: #666; font-size: 12px; margin-top: 4px;">${item.timestamp}</div>
                </div>
            `).join('');
        }

        function useHistoryResult(result) {
            calc.currentInput = result.replace(/,/g, ''); // Remove commas for calculations
            updateDisplay();
            toggleHistory();
        }

        function clearHistory() {
            calc.history = [];
            localStorage.removeItem('calculatorHistory');
            updateHistoryDisplay();
        }

        // Unit Converter Functions (Modified for in-place view)
        const conversionData = {
            length: {
                meter: 1,
                kilometer: 0.001,
                centimeter: 100,
                millimeter: 1000,
                inch: 39.3701,
                foot: 3.28084,
                yard: 1.09361,
                mile: 0.000621371
            },
            weight: {
                kilogram: 1,
                gram: 1000,
                pound: 2.20462,
                ounce: 35.274,
                ton: 0.001,
                stone: 0.157473
            },
            temperature: {
                celsius: { base: true },
                fahrenheit: { formula: 'f' },
                kelvin: { formula: 'k' },
                rankine: { formula: 'r' }
            },
            volume: {
                liter: 1,
                milliliter: 1000,
                gallon: 0.264172,
                quart: 1.05669,
                pint: 2.11338,
                cup: 4.22675,
                ounce: 33.814
            }
        };

        const unitOptions = {
            length: [
                { value: 'meter', label: 'Meter (m)' },
                { value: 'kilometer', label: 'Kilometer (km)' },
                { value: 'centimeter', label: 'Centimeter (cm)' },
                { value: 'millimeter', label: 'Millimeter (mm)' },
                { value: 'inch', label: 'Inch (in)' },
                { value: 'foot', label: 'Foot (ft)' },
                { value: 'yard', label: 'Yard (yd)' },
                { value: 'mile', label: 'Mile (mi)' }
            ],
            weight: [
                { value: 'kilogram', label: 'Kilogram (kg)' },
                { value: 'gram', label: 'Gram (g)' },
                { value: 'pound', label: 'Pound (lb)' },
                { value: 'ounce', label: 'Ounce (oz)' },
                { value: 'ton', label: 'Ton (t)' },
                { value: 'stone', label: 'Stone (st)' }
            ],
            temperature: [
                { value: 'celsius', label: 'Celsius (°C)' },
                { value: 'fahrenheit', label: 'Fahrenheit (°F)' },
                { value: 'kelvin', label: 'Kelvin (K)' },
                { value: 'rankine', label: 'Rankine (°R)' }
            ],
            volume: [
                { value: 'liter', label: 'Liter (L)' },
                { value: 'milliliter', label: 'Milliliter (mL)' },
                { value: 'gallon', label: 'Gallon (gal)' },
                { value: 'quart', label: 'Quart (qt)' },
                { value: 'pint', label: 'Pint (pt)' },
                { value: 'cup', label: 'Cup' },
                { value: 'ounce', label: 'Fluid Ounce (fl oz)' }
            ]
        };

        function toggleConverterView() {
            calc.isConverterMode = !calc.isConverterMode;
            
            if (calc.isConverterMode) {
                // Hide calculator buttons, show converter
                buttonsGrid.classList.add('hidden-section');
                converterSection.classList.remove('hidden-section');
                updateConverter(); // Ensure converter dropdowns are populated
            } else {
                // Show calculator buttons, hide converter
                buttonsGrid.classList.remove('hidden-section');
                converterSection.classList.add('hidden-section');
                // Ensure scientific mode state is correctly reflected if coming back to calculator
                if (calc.isScientificMode) {
                    addScientificButtons();
                } else {
                    removeScientificButtons();
                }
            }
            closeMenu();
        }

        function updateConverter() {
            const type = document.getElementById('converterType').value;
            const fromUnit = document.getElementById('fromUnit');
            const toUnit = document.getElementById('toUnit');
            
            const units = unitOptions[type];
            
            fromUnit.innerHTML = '';
            toUnit.innerHTML = '';
            
            units.forEach(unit => {
                fromUnit.innerHTML += `<option value="${unit.value}">${unit.label}</option>`;
                toUnit.innerHTML += `<option value="${unit.value}">${unit.label}</option>`;
            });
            
            // Set default values
            if (units.length > 1) {
                toUnit.selectedIndex = 1;
            }
            
            document.getElementById('converterInput').value = '';
            document.getElementById('converterResult').textContent = '0';
            convertValue();
        }

        function convertValue() {
            const type = document.getElementById('converterType').value;
            const value = parseFloat(document.getElementById('converterInput').value) || 0;
            const fromUnit = document.getElementById('fromUnit').value;
            const toUnit = document.getElementById('toUnit').value;
            const resultElement = document.getElementById('converterResult');
            
            let result = 0;
            
            try {
                if (type === 'temperature') {
                    result = convertTemperature(value, fromUnit, toUnit);
                } else {
                    const data = conversionData[type];
                    if (data && data[fromUnit] && data[toUnit]) {
                        result = (value / data[fromUnit]) * data[toUnit];
                    }
                }
                
                resultElement.textContent = CalculatorState.formatResult(result);
            } catch (error) {
                resultElement.textContent = 'Error';
            }
        }

        function convertTemperature(value, from, to) {
            let celsius = value;
            
            // Convert to Celsius first
            switch (from) {
                case 'fahrenheit':
                    celsius = (value - 32) * 5/9;
                    break;
                case 'kelvin':
                    celsius = value - 273.15;
                    break;
                case 'rankine':
                    celsius = (value - 491.67) * 5/9;
                    break;
            }
            
            // Convert from Celsius to target
            switch (to) {
                case 'fahrenheit':
                    return celsius * 9/5 + 32;
                case 'kelvin':
                    return celsius + 273.15;
                case 'rankine':
                    return (celsius + 273.15) * 9/5;
                default:
                    return celsius;
            }
        }

        // Enhanced Event Listeners
        menuBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            toggleMenu();
        });

        document.addEventListener('click', (e) => {
            // Close menu if click outside dropdown AND not on menu button
            if (!dropdownMenu.contains(e.target) && e.target !== menuBtn) {
                closeMenu();
            }
            // Close panels if click outside calculator (which is now the parent of converter and buttons)
            if (!calculator.contains(e.target) && historyPanel.classList.contains('active')) {
                historyPanel.classList.remove('active');
            }
        });

        // Enhanced Keyboard Support
        document.addEventListener('keydown', (event) => {
            // Only allow keyboard input if converter is NOT active
            if (calc.isConverterMode) return;

            const key = event.key;
            
            if (key >= '0' && key <= '9') {
                inputNumber(key);
            } else if (key === '.') {
                inputDecimal();
            } else if (key === '+') {
                inputOperator('+');
            } else if (key === '-') {
                inputOperator('−');
            } else if (key === '*') {
                inputOperator('×');
            } else if (key === '/') {
                event.preventDefault();
                inputOperator('÷');
            } else if (key === 'Enter' || key === '=') {
                event.preventDefault();
                calculate();
            } else if (key === 'Escape') {
                clearAll();
            } else if (key === 'Backspace') {
                deleteLast();
            } else if (key === 'Delete') {
                clearEntry();
            }
        });

        // Initialize Calculator
        function init() {
            updateDisplay();
            updateConverter(); /* Initial update for converter dropdowns */
            
            // Add loading animation
            calculator.style.opacity = '0';
            calculator.style.transform = 'translateY(30px)';
            
            setTimeout(() => {
                calculator.style.transition = 'all 0.6s ease-out';
                calculator.style.opacity = '1';
                calculator.style.transform = 'translateY(0)';
            }, 100);
        }

        // Start the calculator
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
