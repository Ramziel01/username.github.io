<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>КЛИКЕР | ПРЕМИАЛЬНАЯ ГРАФИКА</title>
    
    <!-- Telegram WebApp SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --tg-theme-bg-color: var(--tg-theme-bg-color, #0a0a1f);
            --tg-theme-text-color: var(--tg-theme-text-color, #ffffff);
            --tg-theme-hint-color: var(--tg-theme-hint-color, #8a8aa0);
            --tg-theme-link-color: var(--tg-theme-link-color, #00ffff);
            --tg-theme-button-color: var(--tg-theme-button-color, #00ffff);
            --tg-theme-button-text-color: var(--tg-theme-button-text-color, #000000);
            
            --neon-blue: #00ffff;
            --neon-pink: #ff00ff;
            --neon-purple: #9d00ff;
            --neon-green: #00ff9d;
            --neon-yellow: #ffff00;
            --dark-bg: #0a0a1f;
            --darker-bg: #050510;
            --glass-bg: rgba(10, 10, 31, 0.7);
            --glass-border: rgba(0, 255, 255, 0.3);
        }

        body {
            min-height: 100vh;
            background: var(--dark-bg);
            background: radial-gradient(circle at 50% 0%, #1a1a3a, #0a0a1f);
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 10px;
            color: var(--tg-theme-text-color);
            position: relative;
            overflow-x: hidden;
        }

        /* Фоновые частицы */
        .background-particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        .particle {
            position: absolute;
            width: 2px;
            height: 2px;
            background: rgba(0, 255, 255, 0.3);
            border-radius: 50%;
            box-shadow: 0 0 10px #00ffff;
            animation: floatParticle 20s linear infinite;
        }

        @keyframes floatParticle {
            0% { transform: translateY(100vh) translateX(0); opacity: 0; }
            10% { opacity: 1; }
            90% { opacity: 1; }
            100% { transform: translateY(-100vh) translateX(100px); opacity: 0; }
        }

        /* КИНЕМАТОГРАФИЧЕСКАЯ ЗАСТАВКА */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #000;
            z-index: 20000;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: opacity 1s ease;
        }

        .splash-screen.fade-out {
            opacity: 0;
            pointer-events: none;
        }

        .splash-content {
            text-align: center;
            animation: splashZoom 3s ease-in-out;
        }

        @keyframes splashZoom {
            0% { transform: scale(0.3); opacity: 0; filter: blur(10px); }
            20% { transform: scale(1.1); opacity: 1; filter: blur(0); }
            40% { transform: scale(1); }
            80% { transform: scale(1); opacity: 1; }
            100% { transform: scale(1.5); opacity: 0; filter: blur(5px); }
        }

        .splash-logo {
            font-size: 8rem;
            animation: glowPulse 2s infinite;
            margin-bottom: 20px;
            filter: drop-shadow(0 0 30px #00ffff);
        }

        @keyframes glowPulse {
            0% { text-shadow: 0 0 30px #00ffff, 0 0 60px #ff00ff; transform: scale(1); }
            50% { text-shadow: 0 0 50px #00ffff, 0 0 100px #ff00ff; transform: scale(1.05); }
            100% { text-shadow: 0 0 30px #00ffff, 0 0 60px #ff00ff; transform: scale(1); }
        }

        .splash-title {
            font-size: 4rem;
            font-weight: 900;
            background: linear-gradient(45deg, #00ffff, #ff00ff, #00ffff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 20px;
            letter-spacing: 4px;
            text-transform: uppercase;
            animation: titleGlow 2s infinite;
        }

        @keyframes titleGlow {
            0% { filter: drop-shadow(0 0 20px #00ffff); }
            50% { filter: drop-shadow(0 0 40px #ff00ff); }
            100% { filter: drop-shadow(0 0 20px #00ffff); }
        }

        .splash-subtitle {
            color: rgba(255,255,255,0.8);
            font-size: 1.2rem;
            letter-spacing: 2px;
            animation: fadeInOut 2s infinite;
        }

        @keyframes fadeInOut {
            0% { opacity: 0.5; }
            50% { opacity: 1; }
            100% { opacity: 0.5; }
        }

        /* МОДАЛЬНОЕ ОКНО ВХОДА */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            z-index: 10000;
            justify-content: center;
            align-items: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: var(--glass-bg);
            border: 2px solid var(--neon-blue);
            border-radius: 40px;
            padding: 40px 30px;
            max-width: 320px;
            width: 90%;
            text-align: center;
            box-shadow: 0 0 50px var(--neon-blue), inset 0 0 20px rgba(0,255,255,0.3);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            animation: modalAppear 0.5s;
        }

        @keyframes modalAppear {
            from { transform: scale(0.8) translateY(20px); opacity: 0; }
            to { transform: scale(1) translateY(0); opacity: 1; }
        }

        .modal h2 {
            color: var(--neon-blue);
            font-size: 2.5rem;
            margin-bottom: 30px;
            text-shadow: 0 0 20px var(--neon-blue);
            letter-spacing: 2px;
        }

        .tg-login-btn {
            background: linear-gradient(45deg, #0088cc, #00aaff);
            border: none;
            border-radius: 60px;
            padding: 18px;
            color: white;
            font-weight: bold;
            font-size: 1.3rem;
            border: 2px solid white;
            cursor: pointer;
            width: 100%;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            box-shadow: 0 10px 20px rgba(0,136,204,0.3);
            transition: all 0.2s;
        }

        .tg-login-btn:active {
            transform: scale(0.95);
            box-shadow: 0 5px 10px rgba(0,136,204,0.5);
        }

        .nickname-input {
            width: 100%;
            padding: 18px;
            border-radius: 60px;
            background: rgba(255,255,255,0.1);
            border: 2px solid var(--neon-blue);
            color: white;
            font-size: 1.2rem;
            margin-bottom: 20px;
            outline: none;
            text-align: center;
            box-shadow: 0 0 20px rgba(0,255,255,0.3);
        }

        .nickname-input:focus {
            border-color: var(--neon-pink);
            box-shadow: 0 0 30px var(--neon-pink);
        }

        .guest-btn {
            background: rgba(255,255,255,0.1);
            border: 2px solid var(--neon-blue);
            border-radius: 60px;
            padding: 18px;
            color: white;
            font-weight: bold;
            font-size: 1.3rem;
            cursor: pointer;
            width: 100%;
            backdrop-filter: blur(5px);
            transition: all 0.2s;
        }

        .guest-btn:active {
            background: rgba(0,255,255,0.2);
            transform: scale(0.95);
        }

        /* ИГРОВОЙ КОНТЕЙНЕР */
        .game-container {
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 2px solid var(--glass-border);
            border-radius: 50px;
            padding: 20px 16px;
            max-width: 420px;
            width: 100%;
            box-shadow: 0 20px 40px rgba(0,0,0,0.4), 0 0 50px rgba(0,255,255,0.3);
            display: none;
            position: relative;
            z-index: 1;
        }

        .game-container.visible {
            display: block;
            animation: gameAppear 0.5s;
        }

        @keyframes gameAppear {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ПРОФИЛЬ С УЛУЧШЕННОЙ ГРАФИКОЙ */
        .profile-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 40px;
            padding: 10px 20px;
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .player-info {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .player-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--neon-blue), var(--neon-pink));
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            border: 3px solid white;
            box-shadow: 0 0 20px var(--neon-blue);
            animation: avatarGlow 3s infinite;
        }

        @keyframes avatarGlow {
            0% { box-shadow: 0 0 20px var(--neon-blue); }
            50% { box-shadow: 0 0 40px var(--neon-pink); }
            100% { box-shadow: 0 0 20px var(--neon-blue); }
        }

        .tg-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            border: 3px solid #0088cc;
            display: none;
            box-shadow: 0 0 20px #0088cc;
        }

        .player-name {
            color: white;
            font-weight: bold;
            font-size: 1.3rem;
            text-shadow: 0 0 10px rgba(255,255,255,0.5);
        }

        .level-badge {
            background: linear-gradient(135deg, gold, #ffaa00);
            color: black;
            border-radius: 30px;
            padding: 4px 12px;
            font-size: 0.9rem;
            font-weight: bold;
            box-shadow: 0 0 15px gold;
        }

        .tg-premium-badge {
            background: linear-gradient(135deg, #8774e1, #c85b9c);
            color: white;
            border-radius: 20px;
            padding: 3px 10px;
            font-size: 0.8rem;
            margin-left: 5px;
            box-shadow: 0 0 10px #8774e1;
        }

        .balance {
            display: flex;
            align-items: center;
            gap: 8px;
            background: rgba(0,0,0,0.5);
            padding: 8px 15px;
            border-radius: 30px;
            border: 1px solid var(--neon-blue);
        }

        .crystal {
            color: var(--neon-blue);
            font-size: 1.5rem;
            filter: drop-shadow(0 0 10px var(--neon-blue));
        }

        /* ТАБЫ С 3D ЭФФЕКТОМ */
        .tabs {
            display: flex;
            gap: 5px;
            margin-bottom: 20px;
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            padding: 6px;
            border-radius: 40px;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .tab {
            flex: 1;
            padding: 10px 5px;
            border-radius: 30px;
            color: rgba(255,255,255,0.5);
            font-weight: bold;
            text-align: center;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }

        .tab::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s;
        }

        .tab:hover::before {
            left: 100%;
        }

        .tab.active {
            background: linear-gradient(135deg, var(--neon-blue), var(--neon-pink));
            color: white;
            box-shadow: 0 0 20px var(--neon-blue);
            transform: translateY(-2px);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: contentFade 0.3s;
        }

        @keyframes contentFade {
            from { opacity: 0; transform: translateX(10px); }
            to { opacity: 1; transform: translateX(0); }
        }

        /* СЧЕТ С НЕОНОМ */
        .score-container {
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 50px;
            padding: 25px 20px;
            margin-bottom: 20px;
            text-align: center;
            border: 1px solid rgba(0,255,255,0.3);
            box-shadow: inset 0 0 30px rgba(0,0,0,0.5), 0 10px 20px rgba(0,0,0,0.3);
        }

        .score-label {
            color: var(--neon-blue);
            font-size: 1rem;
            letter-spacing: 3px;
            text-transform: uppercase;
            margin-bottom: 10px;
            text-shadow: 0 0 10px var(--neon-blue);
        }

        #score {
            font-size: 4.5rem;
            font-weight: 900;
            color: white;
            text-shadow: 0 0 20px var(--neon-blue), 0 0 40px var(--neon-pink);
            line-height: 1;
            margin-bottom: 10px;
            transition: transform 0.1s;
        }

        .tap-power {
            color: var(--neon-green);
            font-size: 1.2rem;
            text-shadow: 0 0 10px var(--neon-green);
        }

        /* КНОПКА КЛИКЕРА С 3D ЭФФЕКТОМ */
        .clicker-button {
            width: 240px;
            height: 240px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #ff44ff, #a020f0);
            border: 6px solid white;
            margin: 15px auto;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 4rem;
            cursor: pointer;
            box-shadow: 0 20px 30px rgba(0,0,0,0.3), 0 0 60px #ff44ff, inset 0 -10px 20px rgba(0,0,0,0.5);
            transition: all 0.1s;
            position: relative;
            overflow: hidden;
        }

        .clicker-button::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.3) 0%, transparent 70%);
            animation: rotate 10s linear infinite;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .clicker-button:active {
            transform: scale(0.92);
            box-shadow: 0 10px 15px rgba(0,0,0,0.3), 0 0 80px #ff44ff;
        }

        /* ДРОНЫ С ГРАДИЕНТАМИ */
        .drones-container {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin: 25px 0;
        }

        .drone-card {
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 15px 10px;
            text-align: center;
            border: 1px solid rgba(0,255,255,0.3);
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .drone-card:hover {
            transform: translateY(-5px);
            border-color: var(--neon-blue);
            box-shadow: 0 10px 25px rgba(0,255,255,0.2);
        }

        .drone-name {
            color: white;
            font-weight: bold;
            font-size: 1.1rem;
            margin-bottom: 10px;
        }

        .drone-count {
            font-size: 2.5rem;
            font-weight: bold;
            color: var(--neon-blue);
            text-shadow: 0 0 15px var(--neon-blue);
            line-height: 1;
        }

        .drone-price {
            color: var(--neon-green);
            font-size: 1.1rem;
            margin: 8px 0;
            text-shadow: 0 0 10px var(--neon-green);
        }

        .buy-drone {
            background: linear-gradient(135deg, var(--neon-blue), var(--neon-pink));
            border: none;
            border-radius: 40px;
            padding: 12px;
            width: 100%;
            color: white;
            font-weight: bold;
            font-size: 1rem;
            border: 2px solid white;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 5px 15px rgba(0,255,255,0.3);
        }

        .buy-drone:active {
            transform: scale(0.95);
            box-shadow: 0 2px 10px rgba(0,255,255,0.5);
        }

        .buy-drone:disabled {
            opacity: 0.3;
            cursor: not-allowed;
            transform: none;
        }

        /* МАГАЗИН С КАРТОЧКАМИ */
        .shop-section {
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 35px;
            padding: 20px 15px;
            margin: 15px 0;
            border: 1px solid rgba(255,215,0,0.3);
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
        }

        .shop-title {
            color: gold;
            font-size: 1.4rem;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
            text-shadow: 0 0 15px gold;
        }

        .shop-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
        }

        .shop-item {
            background: linear-gradient(135deg, rgba(0,0,0,0.6), rgba(0,0,0,0.8));
            border-radius: 25px;
            padding: 15px 10px;
            text-align: center;
            border: 1px solid gold;
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(5px);
            transition: all 0.3s;
        }

        .shop-item:hover {
            transform: scale(1.02);
            border-color: var(--neon-blue);
            box-shadow: 0 0 30px gold;
        }

        .shop-item.premium {
            border-color: #8774e1;
            background: linear-gradient(135deg, rgba(135,116,225,0.2), rgba(200,91,156,0.2));
        }

        .shop-item-icon {
            font-size: 3rem;
            margin-bottom: 8px;
            filter: drop-shadow(0 0 10px currentColor);
        }

        .shop-item-name {
            color: white;
            font-weight: bold;
            font-size: 1.1rem;
        }

        .shop-item-desc {
            color: rgba(255,255,255,0.6);
            font-size: 0.8rem;
            margin: 5px 0;
        }

        .shop-item-price {
            color: var(--neon-green);
            font-weight: bold;
            font-size: 1.2rem;
            margin: 8px 0;
            text-shadow: 0 0 10px var(--neon-green);
        }

        .shop-item-buy {
            background: linear-gradient(135deg, gold, #ffaa00);
            border: none;
            border-radius: 30px;
            padding: 10px;
            width: 100%;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            border: 2px solid white;
        }

        .shop-item-buy.premium {
            background: linear-gradient(135deg, #8774e1, #c85b9c);
            color: white;
        }

        .premium-tag {
            position: absolute;
            top: 8px;
            right: 8px;
            background: linear-gradient(135deg, #8774e1, #c85b9c);
            color: white;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.7rem;
            font-weight: bold;
            box-shadow: 0 0 15px #8774e1;
        }

        /* ТУРНИРЫ */
        .tournament-card {
            background: linear-gradient(135deg, #1a0033, #330000);
            border-radius: 30px;
            padding: 20px;
            margin: 12px 0;
            border: 2px solid gold;
            box-shadow: 0 0 30px rgba(255,215,0,0.3);
        }

        .tournament-title {
            color: gold;
            font-size: 1.3rem;
            font-weight: bold;
            text-shadow: 0 0 15px gold;
        }

        .tournament-prize {
            color: var(--neon-blue);
            font-size: 1.1rem;
            margin: 8px 0;
        }

        .tournament-progress {
            height: 12px;
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
            margin: 12px 0;
            overflow: hidden;
        }

        .tournament-fill {
            height: 100%;
            background: linear-gradient(90deg, gold, #ffaa00);
            border-radius: 10px;
            transition: width 0.3s;
            box-shadow: 0 0 20px gold;
        }

        .join-tournament {
            background: linear-gradient(135deg, gold, #ffaa00);
            border: none;
            border-radius: 40px;
            padding: 12px;
            width: 100%;
            font-weight: bold;
            font-size: 1.1rem;
            border: 2px solid white;
            cursor: pointer;
            transition: all 0.2s;
        }

        .join-tournament:active {
            transform: scale(0.95);
        }

        /* КЛАН */
        .clan-section {
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 35px;
            padding: 20px;
            margin: 15px 0;
            border: 1px solid gold;
        }

        .clan-header {
            display: flex;
            justify-content: space-between;
            color: gold;
            font-size: 1.3rem;
            margin-bottom: 15px;
        }

        .clan-member {
            display: flex;
            justify-content: space-between;
            padding: 12px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            color: white;
            transition: all 0.2s;
        }

        .clan-member:hover {
            background: rgba(255,255,255,0.05);
        }

        .clan-chat {
            background: rgba(0,0,0,0.6);
            border-radius: 25px;
            padding: 15px;
            height: 160px;
            overflow-y: auto;
            margin: 15px 0;
            border: 1px solid rgba(0,255,255,0.3);
        }

        .clan-message {
            padding: 8px;
            color: var(--neon-blue);
            font-size: 0.95rem;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        /* БИТВЫ */
        .boss-battle {
            background: linear-gradient(135deg, #2a0000, #4a0000);
            border-radius: 35px;
            padding: 25px;
            text-align: center;
            margin: 15px 0;
            border: 2px solid #ff0000;
            box-shadow: 0 0 40px rgba(255,0,0,0.3);
        }

        .boss-health {
            height: 25px;
            background: rgba(0,0,0,0.5);
            border-radius: 15px;
            margin: 15px 0;
            overflow: hidden;
            border: 2px solid #ff0000;
        }

        .boss-health-fill {
            height: 100%;
            background: linear-gradient(90deg, #ff0000, #ff6600);
            border-radius: 15px;
            transition: width 0.3s;
            box-shadow: 0 0 30px #ff0000;
        }

        .attack-boss {
            background: linear-gradient(135deg, #ff0000, #ff4400);
            border: none;
            border-radius: 40px;
            padding: 18px;
            color: white;
            font-weight: bold;
            font-size: 1.3rem;
            border: 2px solid white;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 0 30px #ff0000;
        }

        /* АЧИВКИ */
        .achievement-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin: 15px 0;
        }

        .achievement-card {
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 25px;
            padding: 15px 10px;
            text-align: center;
            border: 1px solid gold;
            transition: all 0.3s;
        }

        .achievement-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px gold;
        }

        .achievement-icon {
            font-size: 2.5rem;
            filter: drop-shadow(0 0 10px gold);
        }

        .achievement-name {
            color: white;
            font-size: 1rem;
            margin: 8px 0;
        }

        .achievement-progress {
            height: 8px;
            background: rgba(255,255,255,0.1);
            border-radius: 5px;
            margin: 8px 0;
            overflow: hidden;
        }

        .achievement-fill {
            height: 100%;
            background: linear-gradient(90deg, gold, #ffaa00);
            border-radius: 5px;
            transition: width 0.3s;
            box-shadow: 0 0 10px gold;
        }

        /* TELEGRAM */
        .tg-story {
            background: linear-gradient(135deg, #0088cc, #00aaff);
            border-radius: 35px;
            padding: 20px;
            margin: 15px 0;
            color: white;
            box-shadow: 0 0 30px #0088cc;
        }

        .tg-poll {
            background: rgba(0,0,0,0.4);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 20px;
            margin: 15px 0;
        }

        .poll-option {
            display: flex;
            justify-content: space-between;
            padding: 12px;
            background: rgba(255,255,255,0.1);
            border-radius: 20px;
            margin: 8px 0;
            cursor: pointer;
            transition: all 0.2s;
        }

        .poll-option:hover {
            background: rgba(0,255,255,0.2);
            transform: scale(1.02);
        }

        .poll-option.selected {
            background: var(--neon-blue);
            color: black;
        }

        .poll-bar {
            height: 8px;
            background: rgba(255,255,255,0.1);
            border-radius: 5px;
            margin: 5px 0;
            overflow: hidden;
        }

        .poll-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--neon-blue), var(--neon-pink));
            border-radius: 5px;
            transition: width 0.3s;
        }

        /* ЗВУКИ */
        .sound-toggle {
            position: fixed;
            bottom: 25px;
            right: 25px;
            width: 55px;
            height: 55px;
            border-radius: 50%;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 2px solid var(--neon-blue);
            color: var(--neon-blue);
            font-size: 1.8rem;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 1000;
            box-shadow: 0 0 30px var(--neon-blue);
            transition: all 0.2s;
        }

        .sound-toggle:active {
            transform: scale(0.9);
        }

        /* ПЛАВАЮЩИЕ ЦИФРЫ */
        .floating-number {
            position: fixed;
            font-size: 2.5rem;
            font-weight: bold;
            pointer-events: none;
            z-index: 1000;
            animation: floatUp 1s ease-out forwards;
            text-shadow: 0 0 20px currentColor;
        }

        @keyframes floatUp {
            0% { opacity: 1; transform: translateY(0) scale(1); }
            100% { opacity: 0; transform: translateY(-120px) scale(1.5); }
        }

        .combo-popup {
            position: fixed;
            top: 20%;
            left: 50%;
            transform: translateX(-50%);
            background: linear-gradient(135deg, #ff00ff, #ffaa00);
            padding: 15px 40px;
            border-radius: 60px;
            color: white;
            font-weight: bold;
            font-size: 2rem;
            border: 4px solid white;
            box-shadow: 0 0 80px gold;
            z-index: 2000;
            animation: comboPop 0.5s ease-out;
            text-shadow: 0 0 20px rgba(255,255,255,0.5);
        }

        @keyframes comboPop {
            0% { transform: translateX(-50%) scale(0); opacity: 0; }
            50% { transform: translateX(-50%) scale(1.3); }
            100% { transform: translateX(-50%) scale(1); opacity: 1; }
        }

        /* БАЛАНС */
        .balance-container {
            display: flex;
            align-items: center;
            gap: 10px;
            background: rgba(0,0,0,0.5);
            padding: 8px 18px;
            border-radius: 40px;
            border: 1px solid var(--neon-blue);
        }

        /* АНИМАЦИИ */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .pulse {
            animation: pulse 2s infinite;
        }

        /* СКРОЛЛ */
        ::-webkit-scrollbar {
            width: 5px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.3);
        }

        ::-webkit-scrollbar-thumb {
            background: var(--neon-blue);
            border-radius: 5px;
        }
    </style>
</head>
<body>

<!-- Фоновые частицы -->
<div class="background-particles" id="particles"></div>

<!-- КИНЕМАТОГРАФИЧЕСКАЯ ЗАСТАВКА -->
<div class="splash-screen" id="splashScreen">
    <div class="splash-content">
        <div class="splash-logo">🚀</div>
        <div class="splash-title">КЛИКЕР</div>
        <div class="splash-subtitle">ПРЕМИАЛЬНАЯ ВЕРСИЯ</div>
    </div>
</div>

<!-- МОДАЛЬНОЕ ОКНО ВХОДА -->
<div class="modal" id="loginModal">
    <div class="modal-content">
        <h2>⚡ ВХОД ⚡</h2>
        
        <button class="tg-login-btn" id="telegramLoginBtn">
            <span>📱</span>
            <span>Войти через Telegram</span>
        </button>
        
        <input type="text" class="nickname-input" id="nicknameInput" placeholder="Твой никнейм" maxlength="15">
        
        <button class="guest-btn" id="guestLoginBtn">
            👤 Войти как гость
        </button>
    </div>
</div>

<!-- КНОПКА ЗВУКА -->
<div class="sound-toggle" id="soundToggle">🔊</div>

<!-- ИГРОВОЙ КОНТЕЙНЕР -->
<div class="game-container" id="gameContainer">
    <!-- Профиль -->
    <div class="profile-bar">
        <div class="player-info">
            <div class="player-avatar" id="playerAvatar">👤</div>
            <img class="tg-avatar" id="tgAvatar">
            <div>
                <div class="player-name" id="playerName">Загрузка...</div>
                <div>
                    <span class="level-badge" id="playerLevel">Ур. 1</span>
                    <span class="tg-premium-badge" id="premiumBadge" style="display: none;">⭐ PREMIUM</span>
                </div>
            </div>
        </div>
        <div class="balance-container">
            <span class="crystal">💎</span>
            <span id="prestigeDisplay">0</span>
        </div>
    </div>

    <!-- ТАБЫ -->
    <div class="tabs">
        <div class="tab active" data-tab="game">🎮 Игра</div>
        <div class="tab" data-tab="shop">🏪 Магазин</div>
        <div class="tab" data-tab="achievements">🏆 Ачивки</div>
        <div class="tab" data-tab="tournaments">⚔️ Турниры</div>
        <div class="tab" data-tab="clan">👥 Клан</div>
        <div class="tab" data-tab="telegram">📱 TG</div>
        <div class="tab" data-tab="battle">💥 Битвы</div>
    </div>

    <!-- Вкладка ИГРА -->
    <div class="tab-content active" id="tab-game">
        <div class="score-container">
            <div class="score-label">НЕО-ЭНЕРГИЯ</div>
            <div id="score">0</div>
            <div class="tap-power">⚡ Сила: <span id="tapPower">1</span></div>
        </div>

        <div class="clicker-button" id="clickButton">🚀</div>

        <div class="drones-container">
            <div class="drone-card">
                <div class="drone-name">📡 Обычный</div>
                <div class="drone-count" id="commonCount">0</div>
                <div class="drone-price" id="commonPrice">10</div>
                <button class="buy-drone" id="buyCommon">Купить</button>
            </div>
            <div class="drone-card">
                <div class="drone-name">⚡ Редкий</div>
                <div class="drone-count" id="rareCount">0</div>
                <div class="drone-price" id="rarePrice">50</div>
                <button class="buy-drone" id="buyRare">Купить</button>
            </div>
            <div class="drone-card">
                <div class="drone-name">💫 Эпический</div>
                <div class="drone-count" id="epicCount">0</div>
                <div class="drone-price" id="epicPrice">200</div>
                <button class="buy-drone" id="buyEpic">Купить</button>
            </div>
            <div class="drone-card">
                <div class="drone-name">👑 Легендарный</div>
                <div class="drone-count" id="legendaryCount">0</div>
                <div class="drone-price" id="legendaryPrice">1000</div>
                <button class="buy-drone" id="buyLegendary">Купить</button>
            </div>
        </div>

        <button class="buy-drone" id="prestigeBtn" style="background: linear-gradient(135deg, gold, #ffaa00); color: black; margin-top: 10px;">
            🌟 ПРЕСТИЖ (нужно 10,000)
        </button>
    </div>

    <!-- Вкладка МАГАЗИН -->
    <div class="tab-content" id="tab-shop">
        <!-- Бустеры -->
        <div class="shop-section">
            <div class="shop-title">
                <span>⚡ БУСТЕРЫ</span>
                <span style="font-size: 0.9rem; color: var(--neon-blue);">x2 сила</span>
            </div>
            <div class="shop-grid">
                <div class="shop-item">
                    <div class="shop-item-icon">🚀</div>
                    <div class="shop-item-name">x2 на 5 мин</div>
                    <div class="shop-item-desc">Удвой доход</div>
                    <div class="shop-item-price">500</div>
                    <button class="shop-item-buy" onclick="buyShopItem('boost1')">Купить</button>
                </div>
                <div class="shop-item">
                    <div class="shop-item-icon">💪</div>
                    <div class="shop-item-name">x3 на 3 мин</div>
                    <div class="shop-item-desc">Мега буст</div>
                    <div class="shop-item-price">1000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('boost2')">Купить</button>
                </div>
                <div class="shop-item premium">
                    <div class="premium-tag">PREMIUM</div>
                    <div class="shop-item-icon">⚡</div>
                    <div class="shop-item-name">x5 на 1 мин</div>
                    <div class="shop-item-desc">Ультра буст</div>
                    <div class="shop-item-price">5000</div>
                    <button class="shop-item-buy premium" onclick="buyShopItem('boost3')">Купить</button>
                </div>
                <div class="shop-item">
                    <div class="shop-item-icon">💎</div>
                    <div class="shop-item-name">Авто-кликер</div>
                    <div class="shop-item-desc">+10/сек</div>
                    <div class="shop-item-price">2000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('autoclicker')">Купить</button>
                </div>
            </div>
        </div>

        <!-- Скины -->
        <div class="shop-section">
            <div class="shop-title">
                <span>🎨 СКИНЫ</span>
                <span style="font-size: 0.9rem; color: var(--neon-blue);">меняют цвет</span>
            </div>
            <div class="shop-grid">
                <div class="shop-item">
                    <div class="shop-item-icon">👑</div>
                    <div class="shop-item-name">Золотой</div>
                    <div class="shop-item-desc">Престижный</div>
                    <div class="shop-item-price">5000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('skin_gold')">Купить</button>
                </div>
                <div class="shop-item">
                    <div class="shop-item-icon">💎</div>
                    <div class="shop-item-name">Неоновый</div>
                    <div class="shop-item-desc">Синее свечение</div>
                    <div class="shop-item-price">3000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('skin_neon')">Купить</button>
                </div>
                <div class="shop-item">
                    <div class="shop-item-icon">🔥</div>
                    <div class="shop-item-name">Плазменный</div>
                    <div class="shop-item-desc">Красный</div>
                    <div class="shop-item-price">4000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('skin_plasma')">Купить</button>
                </div>
                <div class="shop-item premium">
                    <div class="premium-tag">PREMIUM</div>
                    <div class="shop-item-icon">🌈</div>
                    <div class="shop-item-name">Радужный</div>
                    <div class="shop-item-desc">Переливается</div>
                    <div class="shop-item-price">10000</div>
                    <button class="shop-item-buy premium" onclick="buyShopItem('skin_rainbow')">Купить</button>
                </div>
            </div>
        </div>

        <!-- Специальные предметы -->
        <div class="shop-section">
            <div class="shop-title">
                <span>🎁 СПЕЦИАЛЬНОЕ</span>
                <span style="font-size: 0.9rem; color: var(--neon-blue);">редкие вещи</span>
            </div>
            <div class="shop-grid">
                <div class="shop-item">
                    <div class="shop-item-icon">💊</div>
                    <div class="shop-item-name">Мгновенный престиж</div>
                    <div class="shop-item-desc">+1 уровень</div>
                    <div class="shop-item-price">20000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('instant_prestige')">Купить</button>
                </div>
                <div class="shop-item">
                    <div class="shop-item-icon">🎰</div>
                    <div class="shop-item-name">Лотерейный билет</div>
                    <div class="shop-item-desc">Шанс на джекпот</div>
                    <div class="shop-item-price">1000</div>
                    <button class="shop-item-buy" onclick="buyShopItem('lottery')">Купить</button>
                </div>
                <div class="shop-item premium">
                    <div class="premium-tag">PREMIUM</div>
                    <div class="shop-item-icon">👥</div>
                    <div class="shop-item-name">Слот в клане</div>
                    <div class="shop-item-desc">+1 место</div>
                    <div class="shop-item-price">15000</div>
                    <button class="shop-item-buy premium" onclick="buyShopItem('clan_slot')">Купить</button>
                </div>
                <div class="shop-item">
                    <div class="shop-item-icon">📱</div>
                    <div class="shop-item-name">TG Story</div>
                    <div class="shop-item-desc">Поделиться</div>
                    <div class="shop-item-price">500</div>
                    <button class="shop-item-buy" onclick="shareToTelegram()">Купить</button>
                </div>
            </div>
        </div>

        <!-- Ежедневный бонус -->
        <div class="shop-section">
            <div class="shop-title">
                <span>📅 ЕЖЕДНЕВНОЕ</span>
                <span style="font-size: 0.9rem; color: var(--neon-blue);">обновляется</span>
            </div>
            <div style="display: flex; gap: 10px;">
                <div class="shop-item" style="flex: 1;">
                    <div class="shop-item-icon">🎁</div>
                    <div class="shop-item-name">День 1</div>
                    <div class="shop-item-price">500</div>
                    <button class="shop-item-buy" onclick="claimDaily(1)">Забрать</button>
                </div>
                <div class="shop-item" style="flex: 1;">
                    <div class="shop-item-icon">🎁</div>
                    <div class="shop-item-name">День 2</div>
                    <div class="shop-item-price">1000</div>
                    <button class="shop-item-buy" onclick="claimDaily(2)">Забрать</button>
                </div>
                <div class="shop-item" style="flex: 1;">
                    <div class="shop-item-icon">🎁</div>
                    <div class="shop-item-name">День 3</div>
                    <div class="shop-item-price">2000</div>
                    <button class="shop-item-buy" onclick="claimDaily(3)">Забрать</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Вкладка АЧИВКИ -->
    <div class="tab-content" id="tab-achievements">
        <div class="achievement-grid">
            <div class="achievement-card">
                <div class="achievement-icon">👆</div>
                <div class="achievement-name">1000 тапов</div>
                <div class="achievement-progress">
                    <div class="achievement-fill" id="achieve1" style="width: 0%"></div>
                </div>
                <div id="achieve1Text">0/1000</div>
            </div>
            <div class="achievement-card">
                <div class="achievement-icon">📡</div>
                <div class="achievement-name">10 дронов</div>
                <div class="achievement-progress">
                    <div class="achievement-fill" id="achieve2" style="width: 0%"></div>
                </div>
                <div id="achieve2Text">0/10</div>
            </div>
            <div class="achievement-card">
                <div class="achievement-icon">🌟</div>
                <div class="achievement-name">Престиж 5</div>
                <div class="achievement-progress">
                    <div class="achievement-fill" id="achieve3" style="width: 0%"></div>
                </div>
                <div id="achieve3Text">0/5</div>
            </div>
            <div class="achievement-card">
                <div class="achievement-icon">👥</div>
                <div class="achievement-name">5 друзей</div>
                <div class="achievement-progress">
                    <div class="achievement-fill" id="achieve4" style="width: 0%"></div>
                </div>
                <div id="achieve4Text">0/5</div>
            </div>
        </div>
    </div>

    <!-- Вкладка ТУРНИРЫ -->
    <div class="tab-content" id="tab-tournaments">
        <div class="tournament-card">
            <div class="tournament-title">🏆 ТУРНИР ЧАТОВ</div>
            <div class="tournament-prize">Приз: 5000 энергии</div>
            <div class="tournament-progress">
                <div class="tournament-fill" id="tournament1" style="width: 75%;"></div>
            </div>
            <button class="join-tournament" onclick="joinTournament('chat')">Участвовать (500💎)</button>
        </div>

        <div class="tournament-card">
            <div class="tournament-title">⚔️ КЛАНОВАЯ БИТВА</div>
            <div class="tournament-prize">Приз: Легендарный дрон</div>
            <div class="tournament-progress">
                <div class="tournament-fill" id="tournament2" style="width: 30%;"></div>
            </div>
            <button class="join-tournament" onclick="joinTournament('clan')">Участвовать (1000💎)</button>
        </div>

        <div class="tournament-card">
            <div class="tournament-title">👥 ТУРНИР ДРУЗЕЙ</div>
            <div class="tournament-prize">Приз: Эксклюзивный скин</div>
            <button class="join-tournament" onclick="createFriendTournament()">Создать (2000💎)</button>
        </div>
    </div>

    <!-- Вкладка КЛАН -->
    <div class="tab-content" id="tab-clan">
        <div class="clan-section">
            <div class="clan-header">
                <span id="clanName">⚡ КИБЕРПАНКИ</span>
                <span>Уровень 5</span>
            </div>
            
            <div style="margin: 10px 0;">
                <div class="clan-member">
                    <span>👑 Командир</span>
                    <span>150,000</span>
                </div>
                <div class="clan-member">
                    <span>⚔️ Воин 1</span>
                    <span>75,000</span>
                </div>
                <div class="clan-member">
                    <span>⚔️ Воин 2</span>
                    <span>50,000</span>
                </div>
            </div>

            <div class="clan-chat" id="clanChat">
                <div class="clan-message">👑 Командир: В атаку!</div>
                <div class="clan-message">⚔️ Воин 1: Готов!</div>
            </div>

            <div style="display: flex; gap: 5px; margin-top: 10px;">
                <input type="text" id="clanMessageInput" placeholder="Чат..." style="flex: 1; padding: 12px; border-radius: 25px; border: none; background: rgba(255,255,255,0.1); color: white; outline: none;">
                <button id="sendClanMessage" style="padding: 12px 20px; border-radius: 25px; background: linear-gradient(135deg, #0ff, #f0f); border: none; cursor: pointer; color: white; font-weight: bold;">📤</button>
            </div>
        </div>
    </div>

    <!-- Вкладка TELEGRAM -->
    <div class="tab-content" id="tab-telegram">
        <div class="tg-story" id="tgStory">
            <div style="font-size: 1.3rem; margin-bottom: 10px;">📱 TG STORY</div>
            <div id="storyResult">Ваш результат: 0 энергии</div>
            <div style="font-size: 0.9rem; margin-top: 8px;" id="storyViews">👁 0 просмотров</div>
            <button class="join-tournament" onclick="shareStory()" style="margin-top: 15px; background: white; color: black;">
                📤 Опубликовать (500💎)
            </button>
        </div>

        <div class="tg-poll" id="tgPoll">
            <div style="color: white; margin-bottom: 15px; font-size: 1.1rem;">📊 Кто победит в турнире?</div>
            <div class="poll-option" onclick="vote('clan1')">
                <span>Клан КИБЕРПАНКИ</span>
                <span id="vote1">45%</span>
            </div>
            <div class="poll-bar">
                <div class="poll-bar-fill" id="voteBar1" style="width: 45%;"></div>
            </div>
            <div class="poll-option" onclick="vote('clan2')">
                <span>Клан НОВИЧКИ</span>
                <span id="vote2">30%</span>
            </div>
            <div class="poll-bar">
                <div class="poll-bar-fill" id="voteBar2" style="width: 30%;"></div>
            </div>
            <div class="poll-option" onclick="vote('clan3')">
                <span>Клан LEGENDS</span>
                <span id="vote3">25%</span>
            </div>
            <div class="poll-bar">
                <div class="poll-bar-fill" id="voteBar3" style="width: 25%;"></div>
            </div>
        </div>

        <button class="join-tournament" id="inviteFriendsBtn" style="margin-top: 10px; background: linear-gradient(135deg, gold, #ffaa00); color: black;">
            👥 Пригласить друзей (100💎)
        </button>
    </div>

    <!-- Вкладка БИТВЫ -->
    <div class="tab-content" id="tab-battle">
        <div class="boss-battle">
            <div style="font-size: 1.8rem; color: white; text-shadow: 0 0 20px red;">👾 КОСМИЧЕСКИЙ БОСС</div>
            <div style="color: white; margin: 8px 0;" id="bossHealthText">Здоровье: 10000</div>
            <div class="boss-health">
                <div class="boss-health-fill" id="bossHealth" style="width: 100%;"></div>
            </div>
            <div style="color: white; margin: 12px 0;" id="bossPlayers">Участвуют: 5 игроков</div>
            <button class="attack-boss" id="attackBossBtn">⚔️ АТАКОВАТЬ (100💎)</button>
        </div>

        <div class="clan-section" style="margin-top: 15px;">
            <div style="color: #0ff; margin-bottom: 15px; font-size: 1.2rem;">🎥 Видеозвонок с друзьями</div>
            <div style="display: flex; gap: 15px; margin-top: 15px; flex-wrap: wrap; justify-content: center;" id="videoCallParticipants">
                <div style="width: 70px; text-align: center;">
                    <div style="width: 70px; height: 70px; background: linear-gradient(135deg, #0ff, #f0f); border-radius: 20px; border: 3px solid white; box-shadow: 0 0 20px #0ff;"></div>
                    <div style="color: white; font-size: 0.8rem; margin-top: 5px;">Ты</div>
                </div>
                <div style="width: 70px; text-align: center;">
                    <div style="width: 70px; height: 70px; background: #333; border-radius: 20px; border: 2px solid #0ff;"></div>
                    <div style="color: white; font-size: 0.8rem; margin-top: 5px;">Друг 1</div>
                </div>
                <div style="width: 70px; text-align: center;">
                    <div style="width: 70px; height: 70px; background: #333; border-radius: 20px; border: 2px solid #0ff;"></div>
                    <div style="color: white; font-size: 0.8rem; margin-top: 5px;">Друг 2</div>
                </div>
            </div>
            <button class="join-tournament" id="startVideoCall" style="margin-top: 15px;">📞 Начать звонок (200💎)</button>
        </div>
    </div>
</div>

<script>
    // ========== СОЗДАНИЕ ФОНОВЫХ ЧАСТИЦ ==========
    function createParticles() {
        const particlesContainer = document.getElementById('particles');
        for (let i = 0; i < 50; i++) {
            const particle = document.createElement('div');
            particle.className = 'particle';
            particle.style.left = Math.random() * 100 + '%';
            particle.style.animationDelay = Math.random() * 20 + 's';
            particle.style.animationDuration = (Math.random() * 10 + 15) + 's';
            particle.style.background = i % 2 === 0 ? '#00ffff' : '#ff00ff';
            particle.style.boxShadow = `0 0 ${Math.random() * 20 + 10}px ${particle.style.background}`;
            particlesContainer.appendChild(particle);
        }
    }

    // ========== ИНИЦИАЛИЗАЦИЯ TELEGRAM ==========
    let tg = window.Telegram?.WebApp;
    
    if (tg) {
        tg.ready();
        tg.expand();
        
        document.documentElement.style.setProperty('--tg-theme-bg-color', tg.themeParams.bg_color || '#0a0a1f');
        document.documentElement.style.setProperty('--tg-theme-text-color', tg.themeParams.text_color || '#ffffff');
        document.documentElement.style.setProperty('--tg-theme-hint-color', tg.themeParams.hint_color || '#8a8aa0');
        document.documentElement.style.setProperty('--tg-theme-link-color', tg.themeParams.link_color || '#00ffff');
        document.documentElement.style.setProperty('--tg-theme-button-color', tg.themeParams.button_color || '#00ffff');
        document.documentElement.style.setProperty('--tg-theme-button-text-color', tg.themeParams.button_text_color || '#000000');
    }

    // ========== ЗВУКИ ==========
    class SoundManager {
        constructor() {
            this.enabled = true;
            this.context = null;
            this.init();
        }

        init() {
            try {
                window.AudioContext = window.AudioContext || window.webkitAudioContext;
                this.context = new AudioContext();
            } catch (e) {}
        }

        playTap() {
            if (!this.enabled || !this.context) return;
            try {
                const osc = this.context.createOscillator();
                const gain = this.context.createGain();
                osc.type = 'sine';
                osc.frequency.value = 400;
                gain.gain.value = 0.1;
                gain.gain.exponentialRampToValueAtTime(0.01, this.context.currentTime + 0.1);
                osc.connect(gain);
                gain.connect(this.context.destination);
                osc.start();
                osc.stop(this.context.currentTime + 0.1);
            } catch (e) {}
        }

        playBuy() {
            if (!this.enabled || !this.context) return;
            try {
                const osc = this.context.createOscillator();
                const gain = this.context.createGain();
                osc.type = 'triangle';
                osc.frequency.value = 600;
                gain.gain.value = 0.1;
                gain.gain.exponentialRampToValueAtTime(0.01, this.context.currentTime + 0.1);
                osc.connect(gain);
                gain.connect(this.context.destination);
                osc.start();
                osc.stop(this.context.currentTime + 0.1);
            } catch (e) {}
        }

        playCombo() {
            if (!this.enabled || !this.context) return;
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    try {
                        const osc = this.context.createOscillator();
                        const gain = this.context.createGain();
                        osc.type = 'square';
                        osc.frequency.value = 600 + i * 200;
                        gain.gain.value = 0.1;
                        gain.gain.exponentialRampToValueAtTime(0.01, this.context.currentTime + 0.1);
                        osc.connect(gain);
                        gain.connect(this.context.destination);
                        osc.start();
                        osc.stop(this.context.currentTime + 0.1);
                    } catch (e) {}
                }, i * 70);
            }
        }

        playAchievement() {
            if (!this.enabled || !this.context) return;
            try {
                const osc = this.context.createOscillator();
                const gain = this.context.createGain();
                osc.type = 'sine';
                osc.frequency.value = 800;
                osc.frequency.exponentialRampToValueAtTime(1200, this.context.currentTime + 0.2);
                gain.gain.value = 0.2;
                gain.gain.exponentialRampToValueAtTime(0.01, this.context.currentTime + 0.3);
                osc.connect(gain);
                gain.connect(this.context.destination);
                osc.start();
                osc.stop(this.context.currentTime + 0.3);
            } catch (e) {}
        }

        toggle() {
            this.enabled = !this.enabled;
            document.getElementById('soundToggle').textContent = this.enabled ? '🔊' : '🔇';
        }
    }

    // ========== ЗАСТАВКА ==========
    setTimeout(() => {
        document.getElementById('splashScreen').classList.add('fade-out');
        setTimeout(() => {
            document.getElementById('splashScreen').style.display = 'none';
        }, 1000);
    }, 2500);

    // ========== ИНИЦИАЛИЗАЦИЯ ==========
    const sound = new SoundManager();
    
    let currentPlayer = {
        nickname: '',
        level: 1,
        exp: 0,
        premium: false,
        taps: 0,
        friends: [],
        telegramId: null,
        telegramName: '',
        telegramPhoto: '',
        crystals: 1000,
        inventory: {
            skins: [],
            boosters: []
        }
    };

    let energy = 1000;
    let prestigeLevel = 0;
    let tapCombo = 0;
    let activeBoost = null;
    let boostMultiplier = 1;
    
    let drones = {
        common: { count: 0, price: 10 },
        rare: { count: 0, price: 50 },
        epic: { count: 0, price: 200 },
        legendary: { count: 0, price: 1000 }
    };

    let achievements = {
        taps: 0,
        drones: 0,
        prestige: 0,
        friends: 0
    };

    let bossHealth = 10000;
    let bossPlayers = 5;

    let pollVotes = {
        clan1: 45,
        clan2: 30,
        clan3: 25
    };

    let dailyClaims = {
        day1: false,
        day2: false,
        day3: false
    };

    // DOM элементы
    const loginModal = document.getElementById('loginModal');
    const gameContainer = document.getElementById('gameContainer');
    const nicknameInput = document.getElementById('nicknameInput');
    const telegramLoginBtn = document.getElementById('telegramLoginBtn');
    const guestLoginBtn = document.getElementById('guestLoginBtn');
    const soundToggle = document.getElementById('soundToggle');
    
    const playerNameEl = document.getElementById('playerName');
    const playerAvatarEl = document.getElementById('playerAvatar');
    const tgAvatar = document.getElementById('tgAvatar');
    const playerLevel = document.getElementById('playerLevel');
    const premiumBadge = document.getElementById('premiumBadge');
    
    const scoreEl = document.getElementById('score');
    const tapPowerEl = document.getElementById('tapPower');
    const clickBtn = document.getElementById('clickButton');
    
    const prestigeDisplay = document.getElementById('prestigeDisplay');
    const prestigeBtn = document.getElementById('prestigeBtn');

    // Дроны
    const commonCount = document.getElementById('commonCount');
    const commonPrice = document.getElementById('commonPrice');
    const buyCommon = document.getElementById('buyCommon');
    const rareCount = document.getElementById('rareCount');
    const rarePrice = document.getElementById('rarePrice');
    const buyRare = document.getElementById('buyRare');
    const epicCount = document.getElementById('epicCount');
    const epicPrice = document.getElementById('epicPrice');
    const buyEpic = document.getElementById('buyEpic');
    const legendaryCount = document.getElementById('legendaryCount');
    const legendaryPrice = document.getElementById('legendaryPrice');
    const buyLegendary = document.getElementById('buyLegendary');

    // ТАБЫ
    const tabs = document.querySelectorAll('.tab');
    const tabContents = document.querySelectorAll('.tab-content');

    tabs.forEach(tab => {
        tab.addEventListener('click', () => {
            tabs.forEach(t => t.classList.remove('active'));
            tab.classList.add('active');
            
            const tabId = tab.dataset.tab;
            tabContents.forEach(content => content.classList.remove('active'));
            document.getElementById(`tab-${tabId}`).classList.add('active');
        });
    });

    // ========== ПРОВЕРКА TELEGRAM АВТОРИЗАЦИИ ==========
    if (tg && tg.initDataUnsafe && tg.initDataUnsafe.user) {
        const user = tg.initDataUnsafe.user;
        loginWithTelegram(user);
    } else {
        loginModal.classList.add('active');
    }

    function loginWithTelegram(user) {
        currentPlayer.telegramId = user.id;
        currentPlayer.telegramName = user.first_name + (user.last_name ? ' ' + user.last_name : '');
        currentPlayer.nickname = user.username || currentPlayer.telegramName;
        currentPlayer.premium = user.is_premium || false;
        
        if (user.photo_url) {
            tgAvatar.src = user.photo_url;
            tgAvatar.style.display = 'block';
            playerAvatarEl.style.display = 'none';
        } else {
            playerAvatarEl.textContent = currentPlayer.nickname.charAt(0).toUpperCase();
        }
        
        playerNameEl.textContent = currentPlayer.nickname;
        
        if (currentPlayer.premium) {
            premiumBadge.style.display = 'inline';
            currentPlayer.crystals += 500;
        }
        
        if (tg) {
            tg.sendData(JSON.stringify({
                action: 'login',
                user: currentPlayer.nickname
            }));
        }
        
        loginModal.classList.remove('active');
        gameContainer.classList.add('visible');
        
        loadGame();
        updateUI();
    }

    // ========== ВХОД ==========
    telegramLoginBtn.addEventListener('click', () => {
        if (tg) {
            tg.sendData(JSON.stringify({ action: 'request_auth' }));
        } else {
            loginWithTelegram({
                id: 12345,
                first_name: 'Telegram',
                last_name: 'User',
                username: 'tg_user',
                is_premium: true
            });
        }
    });

    guestLoginBtn.addEventListener('click', () => {
        const nickname = nicknameInput.value.trim();
        if (!nickname) {
            tg?.showPopup({
                title: 'Ошибка',
                message: 'Введи никнейм!',
                buttons: [{type: 'ok'}]
            });
            return;
        }
        
        currentPlayer.nickname = nickname;
        playerNameEl.textContent = nickname;
        playerAvatarEl.textContent = nickname.charAt(0).toUpperCase();
        
        loginModal.classList.remove('active');
        gameContainer.classList.add('visible');
        
        loadGame();
    });

    // ========== ЗАГРУЗКА/СОХРАНЕНИЕ ==========
    function loadGame() {
        try {
            const saved = localStorage.getItem(`clicker3000_${currentPlayer.nickname}`);
            if (saved) {
                const data = JSON.parse(saved);
                energy = data.energy || 1000;
                prestigeLevel = data.prestigeLevel || 0;
                drones = data.drones || drones;
                currentPlayer.level = data.level || 1;
                currentPlayer.exp = data.exp || 0;
                currentPlayer.taps = data.taps || 0;
                currentPlayer.friends = data.friends || [];
                currentPlayer.crystals = data.crystals || 1000;
                currentPlayer.inventory = data.inventory || { skins: [], boosters: [] };
            }
        } catch (e) {}
        updateUI();
    }

    function saveGame() {
        if (currentPlayer.nickname) {
            localStorage.setItem(`clicker3000_${currentPlayer.nickname}`, JSON.stringify({
                energy, prestigeLevel, drones,
                level: currentPlayer.level,
                exp: currentPlayer.exp,
                taps: currentPlayer.taps,
                friends: currentPlayer.friends,
                crystals: currentPlayer.crystals,
                inventory: currentPlayer.inventory
            }));
        }
    }

    // ========== ФУНКЦИИ ИГРЫ ==========
    function updateUI() {
        scoreEl.textContent = Math.floor(energy);
        prestigeDisplay.textContent = currentPlayer.crystals;
        
        let power = (1 + prestigeLevel) * boostMultiplier;
        if (currentPlayer.premium) power *= 2;
        tapPowerEl.textContent = power;
        
        commonCount.textContent = drones.common.count;
        commonPrice.textContent = drones.common.price;
        rareCount.textContent = drones.rare.count;
        rarePrice.textContent = drones.rare.price;
        epicCount.textContent = drones.epic.count;
        epicPrice.textContent = drones.epic.price;
        legendaryCount.textContent = drones.legendary.count;
        legendaryPrice.textContent = drones.legendary.price;

        buyCommon.disabled = energy < drones.common.price;
        buyRare.disabled = energy < drones.rare.price;
        buyEpic.disabled = energy < drones.epic.price;
        buyLegendary.disabled = energy < drones.legendary.price;

        updateAchievements();
        checkLevelUp();
        
        document.getElementById('storyResult').textContent = `Ваш результат: ${Math.floor(energy)} энергии`;
    }

    function addEnergy(amount) {
        energy += amount * boostMultiplier;
        currentPlayer.exp += amount;
        updateUI();
        
        scoreEl.style.transform = 'scale(1.1)';
        setTimeout(() => scoreEl.style.transform = 'scale(1)', 100);
    }

    function spendCrystals(amount) {
        if (currentPlayer.crystals >= amount) {
            currentPlayer.crystals -= amount;
            updateUI();
            return true;
        }
        return false;
    }

    function createFloatingNumber(x, y, value) {
        const div = document.createElement('div');
        div.className = 'floating-number';
        div.textContent = `+${Math.floor(value)}`;
        div.style.left = (x - 20) + 'px';
        div.style.top = (y - 30) + 'px';
        div.style.color = '#00ffff';
        div.style.textShadow = '0 0 20px #00ffff';
        document.body.appendChild(div);
        setTimeout(() => div.remove(), 1000);
    }

    function showCombo() {
        const div = document.createElement('div');
        div.className = 'combo-popup';
        div.textContent = '⚡ КОМБО x5! ⚡';
        document.body.appendChild(div);
        sound.playCombo();
        setTimeout(() => div.remove(), 500);
    }

    function checkLevelUp() {
        const expNeeded = currentPlayer.level * 1000;
        if (currentPlayer.exp >= expNeeded) {
            currentPlayer.level++;
            currentPlayer.exp -= expNeeded;
            playerLevel.textContent = `Ур. ${currentPlayer.level}`;
            sound.playAchievement();
            
            tg?.showPopup({
                title: '🎉 Уровень повышен!',
                message: `Ты достиг ${currentPlayer.level} уровня! +100💎`,
                buttons: [{type: 'ok'}]
            });
            
            currentPlayer.crystals += 100;
        }
        playerLevel.textContent = `Ур. ${currentPlayer.level}`;
    }

    function updateAchievements() {
        achievements.taps = currentPlayer.taps;
        achievements.drones = drones.common.count + drones.rare.count + drones.epic.count + drones.legendary.count;
        achievements.prestige = prestigeLevel;
        achievements.friends = currentPlayer.friends.length;

        document.getElementById('achieve1').style.width = Math.min(100, (achievements.taps / 1000) * 100) + '%';
        document.getElementById('achieve1Text').textContent = `${Math.min(achievements.taps, 1000)}/1000`;
        
        document.getElementById('achieve2').style.width = Math.min(100, (achievements.drones / 10) * 100) + '%';
        document.getElementById('achieve2Text').textContent = `${Math.min(achievements.drones, 10)}/10`;
        
        document.getElementById('achieve3').style.width = Math.min(100, (achievements.prestige / 5) * 100) + '%';
        document.getElementById('achieve3Text').textContent = `${Math.min(achievements.prestige, 5)}/5`;
        
        document.getElementById('achieve4').style.width = Math.min(100, (achievements.friends / 5) * 100) + '%';
        document.getElementById('achieve4Text').textContent = `${Math.min(achievements.friends, 5)}/5`;

        if (achievements.taps >= 1000 && !window.achieve1done) {
            window.achieve1done = true;
            addEnergy(5000);
            currentPlayer.crystals += 200;
            tg?.showPopup({
                title: '🏆 Достижение!',
                message: '1000 тапов! +5000 энергии и 200💎',
                buttons: [{type: 'ok'}]
            });
        }
    }

    // ========== КЛИК ==========
    function handleTap(e) {
        e.preventDefault();

        let x, y;
        if (e.touches) {
            x = e.touches[0].clientX;
            y = e.touches[0].clientY;
        } else {
            x = e.clientX;
            y = e.clientY;
        }

        let power = 1 + prestigeLevel;
        
        if (currentPlayer.premium) power *= 2;
        power *= boostMultiplier;
        
        currentPlayer.taps++;
        tapCombo++;
        
        if (tapCombo % 5 === 0) {
            power *= 3;
            showCombo();
        }

        addEnergy(power);
        createFloatingNumber(x, y, power);
        sound.playTap();

        if (tg) {
            tg.HapticFeedback?.impactOccurred('light');
        }
    }

    // ========== ПОКУПКА ДРОНОВ ==========
    function buyDrone(type) {
        if (energy >= drones[type].price) {
            energy -= drones[type].price;
            drones[type].count++;
            drones[type].price = Math.floor(drones[type].price * 1.5);
            updateUI();
            sound.playBuy();
            
            if (tg) {
                tg.HapticFeedback?.notificationOccurred('success');
            }
        }
    }

    buyCommon.addEventListener('click', () => buyDrone('common'));
    buyRare.addEventListener('click', () => buyDrone('rare'));
    buyEpic.addEventListener('click', () => buyDrone('epic'));
    buyLegendary.addEventListener('click', () => buyDrone('legendary'));

    // ========== ПРЕСТИЖ ==========
    prestigeBtn.addEventListener('click', () => {
        if (energy >= 10000) {
            prestigeLevel++;
            energy = 0;
            drones = {
                common: { count: 0, price: 10 },
                rare: { count: 0, price: 50 },
                epic: { count: 0, price: 200 },
                legendary: { count: 0, price: 1000 }
            };
            currentPlayer.crystals += 500;
            updateUI();
            sound.playAchievement();
            
            tg?.showPopup({
                title: '🌟 ПРЕСТИЖ!',
                message: `Ты достиг ${prestigeLevel} уровня престижа! +500💎`,
                buttons: [{type: 'ok'}]
            });
        }
    });

    // ========== МАГАЗИН ==========
    window.buyShopItem = function(item) {
        switch(item) {
            case 'boost1':
                if (spendCrystals(500)) {
                    boostMultiplier = 2;
                    setTimeout(() => { boostMultiplier = 1; }, 300000);
                    tg?.showPopup({ title: 'Буст активирован!', message: 'x2 на 5 минут', buttons: [{type: 'ok'}] });
                }
                break;
            case 'boost2':
                if (spendCrystals(1000)) {
                    boostMultiplier = 3;
                    setTimeout(() => { boostMultiplier = 1; }, 180000);
                    tg?.showPopup({ title: 'Буст активирован!', message: 'x3 на 3 минуты', buttons: [{type: 'ok'}] });
                }
                break;
            case 'boost3':
                if (spendCrystals(5000) && currentPlayer.premium) {
                    boostMultiplier = 5;
                    setTimeout(() => { boostMultiplier = 1; }, 60000);
                    tg?.showPopup({ title: 'Ультра буст!', message: 'x5 на 1 минуту', buttons: [{type: 'ok'}] });
                }
                break;
            case 'autoclicker':
                if (spendCrystals(2000)) {
                    setInterval(() => {
                        addEnergy(10);
                    }, 1000);
                    tg?.showPopup({ title: 'Авто-кликер!', message: '+10 энергии в секунду', buttons: [{type: 'ok'}] });
                }
                break;
            case 'skin_gold':
                if (spendCrystals(5000)) {
                    clickBtn.style.background = 'radial-gradient(circle at 30% 30%, gold, #ffaa00)';
                    currentPlayer.inventory.skins.push('gold');
                    tg?.showPopup({ title: 'Скин получен!', message: 'Золотой шар', buttons: [{type: 'ok'}] });
                }
                break;
            case 'skin_neon':
                if (spendCrystals(3000)) {
                    clickBtn.style.background = 'radial-gradient(circle at 30% 30%, #0ff, #00f)';
                    currentPlayer.inventory.skins.push('neon');
                    tg?.showPopup({ title: 'Скин получен!', message: 'Неоновый шар', buttons: [{type: 'ok'}] });
                }
                break;
            case 'skin_plasma':
                if (spendCrystals(4000)) {
                    clickBtn.style.background = 'radial-gradient(circle at 30% 30%, #f0f, #f00)';
                    currentPlayer.inventory.skins.push('plasma');
                    tg?.showPopup({ title: 'Скин получен!', message: 'Плазменный шар', buttons: [{type: 'ok'}] });
                }
                break;
            case 'skin_rainbow':
                if (spendCrystals(10000) && currentPlayer.premium) {
                    setInterval(() => {
                        const colors = ['#ff44ff', '#0ff', '#f0f', '#ff0', '#0f0'];
                        const color = colors[Math.floor(Math.random() * colors.length)];
                        clickBtn.style.background = `radial-gradient(circle at 30% 30%, ${color}, #000)`;
                    }, 500);
                    currentPlayer.inventory.skins.push('rainbow');
                    tg?.showPopup({ title: 'Скин получен!', message: 'Радужный шар', buttons: [{type: 'ok'}] });
                }
                break;
            case 'instant_prestige':
                if (spendCrystals(20000)) {
                    prestigeLevel++;
                    tg?.showPopup({ title: 'Мгновенный престиж!', message: '+1 уровень престижа', buttons: [{type: 'ok'}] });
                }
                break;
            case 'lottery':
                if (spendCrystals(1000)) {
                    const win = Math.random() > 0.5;
                    if (win) {
                        const prize = Math.floor(Math.random() * 5000) + 1000;
                        addEnergy(prize);
                        tg?.showPopup({ title: '🎰 ДЖЕКПОТ!', message: `Ты выиграл ${prize} энергии!`, buttons: [{type: 'ok'}] });
                    } else {
                        tg?.showPopup({ title: '🎰 Повезёт в следующий раз', message: 'Попробуй ещё!', buttons: [{type: 'ok'}] });
                    }
                }
                break;
        }
        updateUI();
    };

    window.claimDaily = function(day) {
        if (!dailyClaims['day' + day]) {
            dailyClaims['day' + day] = true;
            const rewards = {1: 500, 2: 1000, 3: 2000};
            addEnergy(rewards[day]);
            tg?.showPopup({ title: 'Ежедневный бонус!', message: `+${rewards[day]} энергии`, buttons: [{type: 'ok'}] });
        }
    };

    // ========== ТУРНИРЫ ==========
    window.joinTournament = function(type) {
        if (type === 'chat' && spendCrystals(500)) {
            addEnergy(5000);
            tg?.showPopup({ title: 'Турнир!', message: 'Ты участвуешь! +5000 энергии', buttons: [{type: 'ok'}] });
        } else if (type === 'clan' && spendCrystals(1000)) {
            drones.legendary.count++;
            tg?.showPopup({ title: 'Клановая битва!', message: 'Ты получил легендарного дрона!', buttons: [{type: 'ok'}] });
        }
    };

    window.createFriendTournament = function() {
        if (spendCrystals(2000)) {
            if (tg) {
                tg.switchInlineQuery('tournament', ['users']);
            }
        }
    };

    // ========== КЛАН ==========
    document.getElementById('sendClanMessage').addEventListener('click', () => {
        const input = document.getElementById('clanMessageInput');
        const msg = input.value.trim();
        if (msg) {
            const chat = document.getElementById('clanChat');
            chat.innerHTML += `<div class="clan-message">👤 ${currentPlayer.nickname}: ${msg}</div>`;
            input.value = '';
            chat.scrollTop = chat.scrollHeight;
            
            if (tg) {
                tg.HapticFeedback?.impactOccurred('light');
            }
        }
    });

    // ========== БИТВА ==========
    document.getElementById('attackBossBtn').addEventListener('click', () => {
        if (spendCrystals(100)) {
            const power = 1000 + (prestigeLevel * 500);
            bossHealth -= power;
            
            if (bossHealth < 0) bossHealth = 0;
            
            const percent = (bossHealth / 10000) * 100;
            document.getElementById('bossHealth').style.width = percent + '%';
            document.getElementById('bossHealthText').textContent = `Здоровье: ${Math.max(0, bossHealth)}`;
            
            bossPlayers += Math.floor(Math.random() * 3);
            document.getElementById('bossPlayers').textContent = `Участвуют: ${bossPlayers} игроков`;
            
            if (bossHealth === 0) {
                const reward = 5000 + (prestigeLevel * 1000);
                addEnergy(reward);
                currentPlayer.crystals += 1000;
                
                tg?.showPopup({
                    title: '🎉 БОСС ПОБЕЖДЕН!',
                    message: `Ты получил ${reward} энергии и 1000💎!`,
                    buttons: [{type: 'ok'}]
                });
                
                bossHealth = 10000;
                bossPlayers = 5;
                document.getElementById('bossHealth').style.width = '100%';
                document.getElementById('bossHealthText').textContent = 'Здоровье: 10000';
                document.getElementById('bossPlayers').textContent = 'Участвуют: 5 игроков';
            }
            
            sound.playBuy();
            
            if (tg) {
                tg.HapticFeedback?.impactOccurred('heavy');
            }
        }
    });

    // ========== ВИДЕОЗВОНОК ==========
    document.getElementById('startVideoCall').addEventListener('click', () => {
        if (spendCrystals(200)) {
            if (currentPlayer.friends.length > 0) {
                tg?.showPopup({
                    title: '📞 Видеозвонок',
                    message: 'Звонок друзьям...',
                    buttons: [{type: 'ok'}]
                });
                
                const participants = document.getElementById('videoCallParticipants');
                participants.innerHTML += `
                    <div style="width: 70px; text-align: center;">
                        <div style="width: 70px; height: 70px; background: linear-gradient(135deg, #0ff, #f0f); border-radius: 20px; animation: pulse 1s infinite;"></div>
                        <div style="color: white; font-size: 0.8rem; margin-top: 5px;">Соединение...</div>
                    </div>
                `;
            } else {
                tg?.showPopup({
                    title: '👥 Нет друзей',
                    message: 'Сначала добавь друзей!',
                    buttons: [{type: 'ok'}]
                });
            }
        }
    });

    // ========== TELEGRAM ФУНКЦИИ ==========
    window.shareStory = function() {
        if (spendCrystals(500)) {
            if (tg) {
                tg.shareToStory({
                    text: `🚀 У меня ${Math.floor(energy)} энергии в КЛИКЕР!`,
                    widget_link: {
                        url: 'https://t.me/Clicker3000Bot',
                        name: 'Играть'
                    }
                });
                
                const views = parseInt(document.getElementById('storyViews').textContent.match(/\d+/)[0]) + 1;
                document.getElementById('storyViews').textContent = `👁 ${views} просмотров`;
                addEnergy(1000);
            }
        }
    };

    document.getElementById('inviteFriendsBtn').addEventListener('click', () => {
        if (spendCrystals(100)) {
            if (tg) {
                tg.switchInlineQuery('play', ['users']);
            } else {
                const link = `https://t.me/share/url?url=https://t.me/Clicker3000Bot&text=${encodeURIComponent('🚀 Заходи в КЛИКЕР!')}`;
                window.open(link, '_blank');
            }
        }
    });

    window.vote = function(clan) {
        if (clan === 'clan1') pollVotes.clan1 += 5;
        if (clan === 'clan2') pollVotes.clan2 += 5;
        if (clan === 'clan3') pollVotes.clan3 += 5;
        
        const total = pollVotes.clan1 + pollVotes.clan2 + pollVotes.clan3;
        
        document.getElementById('vote1').textContent = Math.round(pollVotes.clan1 / total * 100) + '%';
        document.getElementById('voteBar1').style.width = (pollVotes.clan1 / total * 100) + '%';
        
        document.getElementById('vote2').textContent = Math.round(pollVotes.clan2 / total * 100) + '%';
        document.getElementById('voteBar2').style.width = (pollVotes.clan2 / total * 100) + '%';
        
        document.getElementById('vote3').textContent = Math.round(pollVotes.clan3 / total * 100) + '%';
        document.getElementById('voteBar3').style.width = (pollVotes.clan3 / total * 100) + '%';
        
        addEnergy(100);
        tg?.HapticFeedback?.impactOccurred('light');
    };

    // ========== ЗВУК ==========
    soundToggle.addEventListener('click', () => sound.toggle());

    // ========== СОБЫТИЯ КЛИКА ==========
    clickBtn.addEventListener('touchstart', handleTap, { passive: false });
    clickBtn.addEventListener('mousedown', handleTap);

    // ========== АВТО-КЛИКЕР ==========
    setInterval(() => {
        let totalPower = 
            drones.common.count * 1 +
            drones.rare.count * 2 +
            drones.epic.count * 5 +
            drones.legendary.count * 15;

        if (totalPower > 0) {
            addEnergy(totalPower);
        }
    }, 1000);

    // ========== АВТОСОХРАНЕНИЕ ==========
    setInterval(() => {
        saveGame();
    }, 30000);

    // ========== TELEGRAM BACK BUTTON ==========
    if (tg) {
        tg.BackButton.onClick(() => {
            tg.close();
        });
    }

    // ========== ИНИЦИАЛИЗАЦИЯ ==========
    createParticles();
    updateUI();
</script>
</body>
</html>
