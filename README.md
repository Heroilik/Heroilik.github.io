<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Уголок Юрьева — Мега-центр развлечений</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Telegram Web App API -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        /* ===== ОСНОВНЫЕ СТИЛИ ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', sans-serif;
            /* кастомный курсор */
            cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="%23ffd700"><circle cx="12" cy="12" r="10" fill="%23ffd700" opacity="0.8"/><circle cx="12" cy="12" r="5" fill="white"/></svg>') 12 12, auto !important;
        }

        /* ===== АНИМИРОВАННЫЙ ЭКРАН ЗАГРУЗКИ ===== */
        .loading-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10000;
            transition: opacity 0.8s ease, visibility 0.8s ease;
        }

        .loading-screen.fade-out {
            opacity: 0;
            visibility: hidden;
        }

        .loading-content {
            text-align: center;
            color: white;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .loading-spinner {
            width: 120px;
            height: 120px;
            margin: 0 auto 30px;
            position: relative;
            animation: rotate 10s linear infinite;
        }

        .spinner-ring {
            position: absolute;
            width: 100%;
            height: 100%;
            border: 4px solid transparent;
            border-top-color: #ffd700;
            border-radius: 50%;
            animation: spin 1.5s cubic-bezier(0.68, -0.55, 0.265, 1.55) infinite;
        }

        .spinner-ring:nth-child(2) {
            width: 80%;
            height: 80%;
            top: 10%;
            left: 10%;
            border-top-color: #ff6b6b;
            animation: spin 2s reverse infinite;
        }

        .spinner-ring:nth-child(3) {
            width: 60%;
            height: 60%;
            top: 20%;
            left: 20%;
            border-top-color: #4CAF50;
            animation: spin 1s infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes rotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .loading-title {
            font-size: 48px;
            font-weight: 900;
            margin-bottom: 15px;
            text-shadow: 0 0 30px rgba(255,215,0,0.5);
            background: linear-gradient(45deg, #ffd700, #ff6b6b, #4CAF50, #ffd700);
            background-size: 300% 300%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradientShift 3s ease infinite;
        }

        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .loading-subtitle {
            font-size: 18px;
            opacity: 0.9;
            margin-bottom: 30px;
        }

        .loading-progress {
            width: 300px;
            height: 6px;
            background: rgba(255,255,255,0.2);
            border-radius: 10px;
            margin: 0 auto;
            overflow: hidden;
        }

        .loading-progress-bar {
            width: 0%;
            height: 100%;
            background: linear-gradient(90deg, #ffd700, #ff6b6b);
            border-radius: 10px;
            animation: progress 2s ease-in-out forwards;
        }

        @keyframes progress {
            0% { width: 0%; }
            20% { width: 20%; }
            40% { width: 40%; }
            60% { width: 60%; }
            80% { width: 80%; }
            100% { width: 100%; }
        }

        .loading-particles {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: rgba(255,255,255,0.5);
            border-radius: 50%;
            animation: particleFloat 3s ease-in-out infinite;
        }

        @keyframes particleFloat {
            0%, 100% { transform: translateY(0) translateX(0); opacity: 0; }
            50% { transform: translateY(-100px) translateX(50px); opacity: 1; }
        }

        body {
            min-height: 100vh;
            background-size: 400% 400%;
            animation: gradientBG 15s ease infinite;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            transition: background 0.5s ease;
            position: relative;
            overflow-x: hidden;
        }

        @keyframes gradientBG {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        /* ===== АНИМИРОВАННЫЕ ТЕМЫ ===== */
        body.theme-sunset { 
            background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
            position: relative;
        }
        
        body.theme-sunset::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 20% 20%, rgba(255,215,0,0.3) 0%, transparent 50%);
            pointer-events: none;
            animation: sunGlow 8s ease-in-out infinite;
        }

        @keyframes sunGlow {
            0%, 100% { opacity: 0.3; transform: scale(1); }
            50% { opacity: 0.6; transform: scale(1.2); }
        }

        body.theme-night { 
            background: linear-gradient(135deg, #141E30 0%, #243B55 100%);
            position: relative;
        }

        body.theme-night::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 80% 20%, rgba(255,255,255,0.8) 0%, transparent 30%);
            pointer-events: none;
            animation: moonGlow 6s ease-in-out infinite;
        }

        body.theme-night::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(2px 2px at 10px 20px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 30px 50px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 70px 80px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 150px 40px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 250px 100px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 350px 150px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 450px 70px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 550px 200px, white, rgba(0,0,0,0));
            background-repeat: repeat;
            pointer-events: none;
            animation: stars 50s linear infinite;
        }

        @keyframes stars {
            from { transform: translateY(0); }
            to { transform: translateY(-1000px); }
        }

        @keyframes moonGlow {
            0%, 100% { opacity: 0.5; transform: scale(1); }
            50% { opacity: 0.8; transform: scale(1.1); }
        }

        body.theme-forest { 
            background: linear-gradient(120deg, #134E5E 0%, #71B280 100%);
            position: relative;
        }

        body.theme-forest::before {
            content: '🌲🌳🌲🌳';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            font-size: 50px;
            white-space: nowrap;
            opacity: 0.2;
            pointer-events: none;
            animation: forestMove 20s linear infinite;
        }

        @keyframes forestMove {
            from { transform: translateX(-100%); }
            to { transform: translateX(100%); }
        }

        body.theme-ocean { 
            background: linear-gradient(135deg, #00B4DB 0%, #0083B0 100%);
            position: relative;
        }

        body.theme-ocean::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: repeating-linear-gradient(transparent 0px, transparent 10px, rgba(255,255,255,0.1) 10px, rgba(255,255,255,0.1) 20px);
            pointer-events: none;
            animation: waves 3s linear infinite;
        }

        @keyframes waves {
            from { transform: translateY(0); }
            to { transform: translateY(20px); }
        }

        body.theme-space { 
            background: linear-gradient(135deg, #0F2027 0%, #203A43 50%, #2C5364 100%);
            position: relative;
        }

        body.theme-space::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.8) 0%, transparent 5%),
                        radial-gradient(circle at 70% 60%, rgba(255,215,0,0.8) 0%, transparent 5%),
                        radial-gradient(circle at 20% 80%, rgba(100,149,237,0.8) 0%, transparent 5%),
                        radial-gradient(circle at 80% 20%, rgba(255,105,180,0.8) 0%, transparent 5%);
            background-size: 200% 200%;
            pointer-events: none;
            animation: spaceTwinkle 4s ease-in-out infinite;
        }

        body.theme-space::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(2px 2px at 10px 20px, white, rgba(0,0,0,0)),
                radial-gradient(3px 3px at 50px 80px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 150px 40px, white, rgba(0,0,0,0)),
                radial-gradient(4px 4px at 250px 100px, white, rgba(0,0,0,0)),
                radial-gradient(2px 2px at 350px 150px, white, rgba(0,0,0,0)),
                radial-gradient(3px 3px at 450px 70px, white, rgba(0,0,0,0));
            background-repeat: repeat;
            pointer-events: none;
            animation: stars 100s linear infinite;
        }

        @keyframes spaceTwinkle {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        /* Бонусы */
        .bonus-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 999;
        }

        .falling-bonus {
            position: absolute;
            width: 60px;
            height: 60px;
            background: linear-gradient(145deg, #ffd700, #ffa500);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 30px;
            box-shadow: 0 0 30px rgba(255, 215, 0, 0.8);
            cursor: pointer;
            pointer-events: auto;
            animation: fall 5s linear forwards;
            z-index: 1000;
            border: 2px solid white;
        }

        @keyframes fall {
            0% { transform: translateY(-100px) rotate(0deg); opacity: 1; }
            100% { transform: translateY(calc(100vh + 100px)) rotate(360deg); opacity: 0; }
        }

        .bonus-active { animation: pulse 1s infinite; }
        @keyframes pulse {
            0% { box-shadow: 0 0 20px rgba(255,215,0,0.5); }
            50% { box-shadow: 0 0 50px rgba(255,215,0,0.8); }
            100% { box-shadow: 0 0 20px rgba(255,215,0,0.5); }
        }

        .bonus-indicator {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(255, 215, 0, 0.2);
            backdrop-filter: blur(10px);
            border: 2px solid #ffd700;
            border-radius: 30px;
            padding: 15px 25px;
            color: white;
            display: none;
            align-items: center;
            gap: 15px;
            z-index: 1001;
            animation: slideInLeft 0.5s ease;
        }
        .bonus-indicator.active { display: flex; }
        .bonus-timer { font-size: 24px; font-weight: bold; color: #ffd700; }

        /* Кнопка поддержки */
        .floating-support {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 2000;
            background: linear-gradient(145deg, #0088cc, #00aaff);
            border: none;
            color: white;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 5px 20px rgba(0,136,204,0.5);
            transition: all 0.3s ease;
            border: 2px solid rgba(255,255,255,0.3);
            animation: pulse-support 2s infinite;
        }
        .floating-support:hover {
            transform: scale(1.1) rotate(10deg);
            box-shadow: 0 8px 30px rgba(0,136,204,0.8);
        }
        @keyframes pulse-support {
            0% { box-shadow: 0 5px 20px rgba(0,136,204,0.5); }
            50% { box-shadow: 0 5px 30px rgba(0,136,204,0.8); }
            100% { box-shadow: 0 5px 20px rgba(0,136,204,0.5); }
        }
        .floating-support .tooltip {
            position: absolute;
            bottom: 80px;
            right: 0;
            background: rgba(0,0,0,0.8);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 14px;
            white-space: nowrap;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s;
            pointer-events: none;
            backdrop-filter: blur(5px);
            border: 1px solid #0088cc;
        }
        .floating-support:hover .tooltip {
            opacity: 1;
            visibility: visible;
            bottom: 90px;
        }

        .main-container {
            display: flex;
            gap: 20px;
            max-width: 1600px;
            width: 100%;
            align-items: flex-start;
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUp 0.8s ease 2s forwards;
        }

        @keyframes fadeInUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* ===== КЛИКЕР СЕКЦИЯ ===== */
        .clicker-section {
            flex: 1;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            padding: 30px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.2);
            color: white;
            text-align: center;
            position: sticky;
            top: 20px;
            transition: transform 0.3s ease;
        }
        .clicker-section:hover { transform: scale(1.02); }

        .clicker-header { margin-bottom: 25px; }
        .clicker-header h2 { font-size: 24px; margin-bottom: 10px; }
        .clicker-header i { font-size: 40px; color: #ffd700; margin-bottom: 15px; }

        .score-container {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 25px;
        }
        .score-label { font-size: 14px; color: rgba(255,255,255,0.7); }
        .score-value {
            font-size: 48px;
            font-weight: bold;
            color: #ffd700;
            text-shadow: 0 0 20px rgba(255,215,0,0.5);
        }

        .click-button {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: linear-gradient(145deg, #ffd700, #ffa500);
            border: none;
            cursor: pointer;
            box-shadow: 0 10px 30px rgba(255,215,0,0.3);
            transition: all 0.1s;
            margin: 20px auto;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        .click-button i { font-size: 60px; color: white; }
        .click-button:active { transform: scale(0.95); }

        .stats-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 25px;
        }
        .stat-item {
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
            padding: 15px;
        }

        .bonus-stats {
            background: rgba(255, 215, 0, 0.1);
            border: 1px solid #ffd700;
            border-radius: 15px;
            padding: 15px;
            margin: 15px 0;
            display: flex;
            justify-content: space-around;
        }

        .theme-buttons {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 5px;
            margin: 15px 0;
        }
        .theme-btn {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: white;
            padding: 8px 5px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 12px;
            transition: all 0.3s;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .theme-btn.active { background: rgba(255, 215, 0, 0.3); border-color: #ffd700; }

        /* КНОПКА ВСЕХ ДОСТИЖЕНИЙ (фиолетовая) */
        .all-achievements-button {
            background: linear-gradient(145deg, #9400D3, #4B0082);
            border: none;
            color: white;
            padding: 15px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 15px 0;
            transition: all 0.3s;
            width: 100%;
        }
        .all-achievements-button:hover {
            transform: scale(1.02);
            box-shadow: 0 5px 20px rgba(148,0,211,0.5);
        }

        /* НОВЫЕ КНОПКИ ПОКУПОК — КРАСИВЫЕ КАРТОЧКИ */
        .upgrade-section {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin: 25px 0;
        }
        .upgrade-card {
            background: linear-gradient(145deg, rgba(0,0,0,0.6), rgba(0,0,0,0.8));
            border: 2px solid rgba(255,215,0,0.3);
            border-radius: 24px;
            padding: 20px;
            display: flex;
            align-items: center;
            gap: 20px;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        .upgrade-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,215,0,0.2), transparent);
            transition: left 0.5s ease;
        }
        .upgrade-card:hover::before { left: 100%; }
        .upgrade-card:hover { border-color: #ffd700; transform: translateY(-4px); box-shadow: 0 10px 30px rgba(255,215,0,0.3); }
        .upgrade-card.disabled { opacity: 0.6; filter: grayscale(0.6); pointer-events: none; border-color: #666; }
        .upgrade-card.cant-afford { opacity: 0.8; border-color: #ff4d4d; }
        
        .upgrade-emoji {
            width: 70px;
            height: 70px;
            background: linear-gradient(145deg, #ffd700, #ff8c00);
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 36px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
        }
        .upgrade-info { flex: 1; text-align: left; }
        .upgrade-title {
            font-size: 20px;
            font-weight: 800;
            color: white;
            margin-bottom: 6px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .upgrade-level-badge {
            background: rgba(255,215,0,0.3);
            border: 1px solid #ffd700;
            border-radius: 30px;
            padding: 3px 12px;
            font-size: 14px;
            color: #ffd700;
        }
        .upgrade-desc {
            color: rgba(255,255,255,0.8);
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .upgrade-price-tag {
            background: rgba(0,0,0,0.7);
            border: 2px solid #ffd700;
            border-radius: 40px;
            padding: 12px 20px;
            font-size: 24px;
            font-weight: 900;
            color: #ffd700;
            display: flex;
            align-items: center;
            gap: 5px;
            box-shadow: 0 5px 15px rgba(255,215,0,0.2);
        }
        .upgrade-price-tag::before { content: '🪙'; font-size: 20px; margin-right: 5px; }
        .hotkey-hint {
            background: rgba(255,215,0,0.2);
            border: 1px solid #ffd700;
            border-radius: 20px;
            padding: 4px 10px;
            font-size: 12px;
            color: #ffd700;
            font-weight: bold;
            margin-left: 10px;
        }

        /* ПАНЕЛЬ СБРОСА — ОТДЕЛЬНАЯ КНОПКА */
        .reset-panel {
            margin-top: 30px;
            padding: 20px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 30px;
            border: 1px dashed rgba(255, 100, 100, 0.5);
        }
        .reset-danger-button {
            background: linear-gradient(145deg, #ff4d4d, #b30000);
            border: none;
            color: white;
            padding: 18px 20px;
            border-radius: 60px;
            cursor: pointer;
            font-size: 18px;
            font-weight: 800;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            transition: all 0.3s;
            width: 100%;
            text-transform: uppercase;
            letter-spacing: 2px;
            box-shadow: 0 10px 20px rgba(255,0,0,0.3);
        }
        .reset-danger-button:hover {
            transform: scale(1.03);
            box-shadow: 0 15px 30px rgba(255,0,0,0.5);
            background: linear-gradient(145deg, #ff3333, #990000);
        }
        .reset-danger-button i { font-size: 24px; }

        /* ===== ПРОФИЛЬ С ФОТО-БАННЕРОМ (РАСШИРЕННЫЙ) ===== */
        .profile-card {
            flex: 2;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 30px;
            overflow: hidden;
            box-shadow: 0 20px 50px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.2);
            transition: transform 0.3s ease;
        }
        .profile-card:hover { transform: scale(1.02); }

        .profile-banner {
            width: 100%;
            height: 180px; /* Увеличено для большего баннера */
            background: linear-gradient(145deg, #4158D0, #C850C0, #FFCC70);
            position: relative;
            cursor: pointer;
            overflow: hidden;
        }
        .profile-banner img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }
        .banner-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transition: opacity 0.3s;
        }
        .profile-banner:hover .banner-overlay { opacity: 1; }
        .banner-overlay i { color: white; font-size: 40px; }

        .profile-header {
            text-align: center;
            margin-top: -50px; /* Скорректировано под новый размер */
            position: relative;
            z-index: 2;
        }
        .profile-avatar {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            border: 4px solid white;
            margin: 0 auto 15px;
            overflow: hidden;
            position: relative;
            cursor: pointer;
            background: #2c3e50;
        }
        .profile-avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .avatar-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transition: opacity 0.3s;
            border-radius: 50%;
        }
        .profile-avatar:hover .avatar-overlay { opacity: 1; }

        .profile-name {
            color: white;
            font-size: 26px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        .profile-bio {
            color: rgba(255,255,255,0.8);
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        .profile-status {
            color: rgba(255,255,255,0.6);
            font-size: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        .profile-status i { color: #4CAF50; }

        .tabs-container {
            display: flex;
            gap: 8px; /* Увеличено для большего количества вкладок */
            margin: 20px 20px 10px;
            border-bottom: 1px solid rgba(255,255,255,0.2);
            padding-bottom: 10px;
            flex-wrap: wrap;
        }
        .tab-button {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: white;
            padding: 12px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
            flex: 1 1 auto;
            justify-content: center;
            transition: all 0.3s;
            min-width: 100px;
        }
        .tab-button:hover { background: rgba(255,255,255,0.2); }
        .tab-button.active { background: linear-gradient(145deg, #ffd700, #ffa500); }

        .tab-content { display: none; padding: 0 20px; }
        .tab-content.active { display: block; }

        /* Плейлист */
        .playlist-section {
            background: rgba(0,0,0,0.25);
            border-radius: 20px;
            padding: 20px;
            margin: 25px 0;
        }
        .track-item {
            display: flex;
            align-items: center;
            padding: 10px;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.2s;
            color: white;
        }
        .track-item:hover { background: rgba(255,255,255,0.15); }
        .now-playing-bar {
            background: rgba(0,0,0,0.4);
            border-radius: 15px;
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            color: white;
            margin: 20px 0;
        }
        .control-btn {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: rgba(255,255,255,0.15);
            border: none;
            color: white;
            cursor: pointer;
        }
        audio { width: 100%; margin-top: 10px; }

        /* Пинг-понг */
        .pong-container {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 20px;
            margin: 20px 0;
        }
        .pong-canvas-container {
            width: 100%;
            aspect-ratio: 16/9;
            background: linear-gradient(145deg, #0a0a1a, #1a1a2a);
            border-radius: 20px;
            overflow: hidden;
            margin-bottom: 20px;
        }
        #pongCanvas { width: 100%; height: 100%; display: block; }
        .pong-controls {
            display: flex;
            gap: 15px;
            justify-content: center;
        }
        .pong-btn {
            background: linear-gradient(145deg, #4a76a8, #6c5b9e);
            border: none;
            color: white;
            padding: 12px 30px;
            border-radius: 30px;
            cursor: pointer;
        }
        .difficulty-btn {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: white;
            padding: 8px 20px;
            border-radius: 20px;
            cursor: pointer;
        }
        .difficulty-btn.active { background: rgba(255,215,0,0.3); border-color: #ffd700; }

        /* Крестики-нолики */
        .tictactoe-container {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 20px;
            margin: 20px 0;
            text-align: center;
        }
        .ttt-board {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            max-width: 300px;
            margin: 20px auto;
        }
        .ttt-cell {
            aspect-ratio: 1;
            background: rgba(255,255,255,0.1);
            border: 2px solid rgba(255,215,0,0.3);
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 48px;
            font-weight: bold;
            color: white;
            cursor: pointer;
            transition: all 0.3s;
        }
        .ttt-cell:hover { background: rgba(255,215,0,0.2); transform: scale(1.05); }
        .ttt-cell.x { color: #ffd700; }
        .ttt-cell.o { color: #ff4d4d; }
        .ttt-status { color: white; font-size: 18px; margin: 15px 0; }
        .ttt-stats {
            display: flex;
            gap: 20px;
            justify-content: center;
            color: white;
        }

        /* PostgreSQL демо-блок */
        .postgres-demo {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 15px;
            margin: 20px 0;
            border: 1px solid #336791;
        }
        .postgres-header {
            display: flex;
            align-items: center;
            gap: 10px;
            color: #336791;
            margin-bottom: 10px;
        }
        .postgres-header i { font-size: 24px; }
        .postgres-query {
            background: rgba(0,0,0,0.5);
            padding: 10px;
            border-radius: 10px;
            font-family: monospace;
            color: #ffd700;
            margin: 10px 0;
        }
        .postgres-result {
            background: rgba(255,255,255,0.1);
            padding: 10px;
            border-radius: 10px;
            color: white;
            font-size: 14px;
        }

        /* REST API демо-блок */
        .restapi-demo {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 15px;
            margin: 20px 0;
            border: 1px solid #4CAF50;
        }
        .restapi-header {
            display: flex;
            align-items: center;
            gap: 10px;
            color: #4CAF50;
            margin-bottom: 10px;
        }
        .restapi-header i { font-size: 24px; }
        .restapi-endpoint {
            background: rgba(0,0,0,0.5);
            padding: 10px;
            border-radius: 10px;
            font-family: monospace;
            color: #4CAF50;
            margin: 10px 0;
            border-left: 4px solid #4CAF50;
        }
        .restapi-method {
            display: inline-block;
            padding: 3px 8px;
            border-radius: 5px;
            font-weight: bold;
            margin-right: 10px;
        }
        .method-get { background: #61affe; color: white; }
        .method-post { background: #49cc90; color: white; }
        .method-put { background: #fca130; color: white; }
        .method-delete { background: #f93e3e; color: white; }
        .restapi-response {
            background: rgba(255,255,255,0.1);
            padding: 10px;
            border-radius: 10px;
            color: white;
            font-size: 14px;
        }

        /* Казино - обновленная система */
        .casino-container {
            background: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 20px;
            margin: 20px 0;
            border: 1px solid #ffd700;
        }
        .casino-balance {
            font-size: 32px;
            font-weight: bold;
            color: #ffd700;
            margin: 15px 0;
        }
        .daily-bonus {
            background: rgba(255,215,0,0.1);
            border: 1px solid #ffd700;
            border-radius: 15px;
            padding: 10px;
            margin: 15px 0;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }
        .daily-bonus button {
            background: linear-gradient(145deg, #ffd700, #ffa500);
            border: none;
            color: white;
            padding: 8px 20px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: bold;
        }
        .daily-bonus button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        .slot-machine {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin: 20px 0;
        }
        .slot {
            width: 80px;
            height: 80px;
            background: rgba(0,0,0,0.5);
            border: 3px solid #ffd700;
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 48px;
            color: white;
        }
        .bet-controls {
            display: flex;
            gap: 10px;
            align-items: center;
            margin: 15px 0;
            justify-content: center;
        }
        .bet-input {
            width: 120px;
            padding: 12px;
            background: rgba(255,255,255,0.1);
            border: 1px solid #ffd700;
            border-radius: 10px;
            color: white;
            text-align: center;
            font-size: 16px;
        }
        .win-probability {
            color: rgba(255,255,255,0.7);
            font-size: 14px;
            margin-bottom: 15px;
        }
        
        /* Магазин казино (расширенный) */
        .casino-shop {
            margin-top: 30px;
            border-top: 2px dashed rgba(255,215,0,0.3);
            padding-top: 20px;
        }
        .shop-title {
            color: #ffd700;
            font-size: 20px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .shop-items {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
        }
        .shop-item {
            background: rgba(0,0,0,0.4);
            border: 1px solid rgba(255,215,0,0.3);
            border-radius: 15px;
            padding: 15px;
            text-align: center;
            transition: all 0.3s;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        .shop-item:hover {
            transform: translateY(-3px);
            border-color: #ffd700;
            box-shadow: 0 10px 20px rgba(255,215,0,0.2);
        }
        .shop-item.disabled {
            opacity: 0.5;
            pointer-events: none;
            filter: grayscale(0.8);
        }
        .shop-item-level {
            position: absolute;
            top: 5px;
            right: 5px;
            background: #ffd700;
            color: black;
            padding: 2px 8px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: bold;
        }
        .shop-item-icon {
            font-size: 40px;
            color: #ffd700;
            margin-bottom: 10px;
        }
        .shop-item-name {
            color: white;
            font-weight: bold;
            margin-bottom: 5px;
        }
        .shop-item-desc {
            color: rgba(255,255,255,0.6);
            font-size: 12px;
            margin-bottom: 10px;
        }
        .shop-item-price {
            color: #ffd700;
            font-weight: bold;
            font-size: 18px;
        }

        .social-links {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            padding: 20px;
        }
        .social-link {
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
            padding: 12px;
            text-align: center;
            color: white;
            text-decoration: none;
        }

        /* Модальные окна */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(10px);
            z-index: 3000;
            justify-content: center;
            align-items: center;
        }
        .modal.active { display: flex; }
        .modal-content {
            background: linear-gradient(145deg, #1a1a2e, #16213e);
            border-radius: 30px;
            padding: 30px;
            max-width: 700px;
            width: 90%;
            max-height: 80vh;
            overflow-y: auto;
            border: 2px solid rgba(255,255,255,0.1);
        }
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            padding-bottom: 15px;
        }
        .modal-header h2 { color: white; }
        .close-button {
            background: rgba(255,255,255,0.1);
            border: none;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 20px;
        }

        /* Сетка достижений */
        .achievements-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 15px;
        }
        .achievement-card {
            background: rgba(255,255,255,0.05);
            border-radius: 15px;
            padding: 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            transition: all 0.3s;
            border: 2px solid rgba(255,255,255,0.1);
            position: relative;
        }
        .achievement-card.unlocked {
            background: rgba(255,215,0,0.15);
            border-color: #ffd700;
        }
        .game-badge {
            position: absolute;
            top: 5px;
            right: 5px;
            padding: 3px 8px;
            border-radius: 20px;
            font-size: 10px;
            font-weight: bold;
            background: rgba(0,0,0,0.5);
            color: white;
        }
        .game-badge.clicker { background: #4a76a8; }
        .game-badge.pong { background: #ff6b6b; }
        .game-badge.tictactoe { background: #32CD32; }
        .game-badge.casino { background: #ffd700; color: black; }
        .achievement-icon {
            width: 50px;
            height: 50px;
            background: linear-gradient(145deg, #4a76a8, #6c5b9e);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
        }
        .achievement-info { flex: 1; }
        .achievement-name {
            color: white;
            font-weight: 700;
            font-size: 14px;
            margin-bottom: 5px;
        }
        .achievement-desc { color: rgba(255,255,255,0.6); font-size: 11px; }
        .achievement-status {
            font-size: 20px;
            color: rgba(255,255,255,0.3);
        }
        .achievement-card.unlocked .achievement-status { color: #ffd700; }

        /* Био модалка */
        .bio-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(10px);
            z-index: 4000;
            justify-content: center;
            align-items: center;
        }
        .bio-modal.active { display: flex; }
        .bio-content {
            background: linear-gradient(145deg, #1a1a2e, #16213e);
            border-radius: 30px;
            padding: 40px;
            max-width: 500px;
            width: 90%;
            border: 2px solid rgba(255,215,0,0.3);
        }
        .bio-content h2 { color: white; margin-bottom: 20px; }
        .bio-content input, .bio-content textarea {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 15px;
            color: white;
        }
        .bio-content button {
            background: linear-gradient(145deg, #ffd700, #ffa500);
            border: none;
            color: white;
            padding: 15px;
            border-radius: 30px;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }

        /* ВСПЛЫВАЮЩИЕ УВЕДОМЛЕНИЯ */
        .notification-container {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 10000;
            display: flex;
            flex-direction: column;
            gap: 10px;
            pointer-events: none;
        }
        .notification {
            background: rgba(0,0,0,0.9);
            backdrop-filter: blur(10px);
            border-left: 6px solid;
            border-radius: 16px;
            padding: 16px 24px;
            color: white;
            font-size: 15px;
            font-weight: 600;
            box-shadow: 0 15px 30px rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            gap: 15px;
            transform: translateX(120%);
            animation: slideInNotification 0.4s forwards, fadeOut 0.4s 2.6s forwards;
            pointer-events: none;
            max-width: 350px;
        }
        .notification.success { border-left-color: #4CAF50; }
        .notification.error { border-left-color: #f44336; }
        .notification.info { border-left-color: #2196F3; }
        .notification.warning { border-left-color: #ff9800; }
        .notification i { font-size: 24px; }
        @keyframes slideInNotification {
            to { transform: translateX(0); }
        }
        @keyframes fadeOut {
            to { opacity: 0; }
        }

        @media (max-width: 900px) {
            .main-container { flex-direction: column; }
            .clicker-section { position: static; }
        }
    </style>
</head>
<body class="theme-sunset">
    <!-- Анимированный экран загрузки -->
    <div class="loading-screen" id="loadingScreen">
        <div class="loading-particles">
            <div class="particle" style="top: 10%; left: 20%; animation-delay: 0s;"></div>
            <div class="particle" style="top: 30%; left: 80%; animation-delay: 0.5s;"></div>
            <div class="particle" style="top: 70%; left: 30%; animation-delay: 1s;"></div>
            <div class="particle" style="top: 50%; left: 60%; animation-delay: 1.5s;"></div>
            <div class="particle" style="top: 90%; left: 40%; animation-delay: 2s;"></div>
        </div>
        <div class="loading-content">
            <div class="loading-spinner">
                <div class="spinner-ring"></div>
                <div class="spinner-ring"></div>
                <div class="spinner-ring"></div>
            </div>
            <h1 class="loading-title">Уголок Юрьева</h1>
            <p class="loading-subtitle">Мега-центр развлечений</p>
            <div class="loading-progress">
                <div class="loading-progress-bar"></div>
            </div>
        </div>
    </div>

    <!-- Бонусы -->
    <div class="bonus-container" id="bonusContainer"></div>
    <div class="bonus-indicator" id="bonusIndicator">
        <div class="bonus-icon"><i class="fas fa-star"></i></div>
        <div class="bonus-text">
            <div class="bonus-title">Супер бонус активен!</div>
            <div class="bonus-multiplier" id="bonusMultiplier">x10</div>
        </div>
        <div class="bonus-timer" id="bonusTimer">5s</div>
    </div>

    <!-- Кнопка поддержки -->
    <button class="floating-support" onclick="openSupport()">
        <i class="fas fa-headset"></i>
        <span class="tooltip">Поддержка</span>
    </button>

    <!-- Контейнер для всплывающих уведомлений -->
    <div class="notification-container" id="notificationContainer"></div>

    <div class="main-container">
        <!-- Кликер секция -->
        <div class="clicker-section">
            <!-- Telegram User Info -->
            <div class="tg-user-info" id="tgUserInfo" style="display: none; background: rgba(0,0,0,0.2); border-radius:30px; padding:15px; margin-bottom:15px; display:flex; gap:15px; align-items:center;">
                <div class="tg-avatar" style="width:50px; height:50px; background:#ffd700; border-radius:50%; display:flex; align-items:center; justify-content:center;"><i class="fab fa-telegram"></i></div>
                <div class="tg-details">
                    <div class="tg-name" id="tgName" style="color:white;">Telegram User</div>
                    <div class="tg-id" id="tgId" style="color:rgba(255,255,255,0.6);">ID: </div>
                </div>
                <div class="tg-share-btn" onclick="shareToTelegram()" style="background:rgba(255,215,0,0.2); border:1px solid #ffd700; color:#ffd700; padding:8px 15px; border-radius:20px; cursor:pointer;"><i class="fab fa-telegram"></i> Поделиться</div>
            </div>

            <div class="clicker-header">
                <i class="fas fa-hand-pointer"></i>
                <h2>Кликер TALLIN</h2>
                <div class="hotkey-info" style="font-size:12px; margin-top:5px; color:#ffd700;">
                    <i class="fas fa-keyboard"></i> Горячие клавиши: 1-4 (покупка), C (казино), M (магазин), P (плейлист)
                </div>
            </div>

            <div class="score-container">
                <div class="score-label">Твои баллы</div>
                <div class="score-value" id="score">0</div>
            </div>

            <button class="click-button" id="clickButton" onclick="handleClick()">
                <i class="fas fa-crown"></i>
                <span>КЛИК!</span>
            </button>

            <div class="stats-container">
                <div class="stat-item">
                    <div class="stat-label">За клик</div>
                    <div class="stat-value" id="perClick">+1</div>
                </div>
                <div class="stat-item">
                    <div class="stat-label">В секунду</div>
                    <div class="stat-value" id="perSecond">0</div>
                </div>
            </div>

            <div class="bonus-stats" id="bonusStats">
                <div class="bonus-stat"><div class="bonus-label">Бонус клика</div><div class="bonus-value" id="bonusClick">+0%</div></div>
                <div class="bonus-stat"><div class="bonus-label">Бонус авто</div><div class="bonus-value" id="bonusAuto">+0%</div></div>
                <div class="bonus-stat"><div class="bonus-label">Крит. шанс</div><div class="bonus-value" id="critChance">0%</div></div>
            </div>

            <!-- Telegram Theme Toggle -->
            <button class="tg-theme-btn" onclick="toggleTelegramTheme()" style="background:rgba(0,0,0,0.3); border:1px solid rgba(255,255,255,0.2); color:white; padding:10px 20px; border-radius:30px; cursor:pointer; margin:10px 0;"><i class="fab fa-telegram"></i> Синхронизировать с темой Telegram</button>

            <div class="theme-buttons">
                <button class="theme-btn active" onclick="changeTheme('sunset')" id="themeSunset"><i class="fas fa-sun"></i><span>Закат</span></button>
                <button class="theme-btn" onclick="changeTheme('night')" id="themeNight"><i class="fas fa-moon"></i><span>Ночь</span></button>
                <button class="theme-btn" onclick="changeTheme('forest')" id="themeForest"><i class="fas fa-tree"></i><span>Лес</span></button>
                <button class="theme-btn" onclick="changeTheme('ocean')" id="themeOcean"><i class="fas fa-water"></i><span>Море</span></button>
                <button class="theme-btn" onclick="changeTheme('space')" id="themeSpace"><i class="fas fa-rocket"></i><span>Космос</span></button>
            </div>

            <!-- ЕДИНАЯ КНОПКА ВСЕХ ДОСТИЖЕНИЙ -->
            <button class="all-achievements-button" onclick="openAllAchievements()">
                <i class="fas fa-trophy"></i>
                <span>Все достижения (<span id="totalAchievementCount">0/52</span>)</span>
            </button>

            <!-- НОВЫЕ КРАСИВЫЕ КАРТОЧКИ ПОКУПОК -->
            <div class="upgrade-info-panel" style="display: flex; justify-content: space-between; background: rgba(255,215,0,0.1); border-radius: 60px; padding: 10px 20px; margin-bottom: 20px;">
                <span><i class="fas fa-info-circle"></i> Нажми на карточку для покупки</span>
                <span><i class="fas fa-keyboard"></i> Горячие клавиши 1-4</span>
            </div>

            <div class="upgrade-section">
                <!-- Сила клика -->
                <div class="upgrade-card" onclick="buyUpgrade('click')" id="upgradeClick">
                    <div class="upgrade-emoji">👆</div>
                    <div class="upgrade-info">
                        <div class="upgrade-title">
                            Сила клика 
                            <span class="upgrade-level-badge" id="clickLevel">ур.0</span>
                            <span class="hotkey-hint">1</span>
                        </div>
                        <div class="upgrade-desc">
                            <i class="fas fa-arrow-up" style="color:#ffd700;"></i> +1 за клик 
                            <span style="background:rgba(255,215,0,0.2); padding:2px 8px; border-radius:30px;">база +1</span>
                        </div>
                    </div>
                    <div class="upgrade-price-tag" id="clickPrice">10</div>
                </div>

                <!-- Авто-кликер -->
                <div class="upgrade-card" onclick="buyUpgrade('auto')" id="upgradeAuto">
                    <div class="upgrade-emoji">🤖</div>
                    <div class="upgrade-info">
                        <div class="upgrade-title">
                            Авто-кликер 
                            <span class="upgrade-level-badge" id="autoLevel">ур.0</span>
                            <span class="hotkey-hint">2</span>
                        </div>
                        <div class="upgrade-desc">
                            <i class="fas fa-clock" style="color:#ffd700;"></i> +1 в секунду 
                            <span style="background:rgba(255,215,0,0.2); padding:2px 8px; border-radius:30px;">пассивно</span>
                        </div>
                    </div>
                    <div class="upgrade-price-tag" id="autoPrice">50</div>
                </div>

                <!-- Супер клик -->
                <div class="upgrade-card" onclick="buyUpgrade('super')" id="upgradeSuper">
                    <div class="upgrade-emoji">⚡</div>
                    <div class="upgrade-info">
                        <div class="upgrade-title">
                            Супер клик 
                            <span class="upgrade-level-badge" id="superLevel">ур.0</span>
                            <span class="hotkey-hint">3</span>
                        </div>
                        <div class="upgrade-desc">
                            <i class="fas fa-star" style="color:#ffd700;"></i> +5 за клик 
                            <span style="background:rgba(255,215,0,0.2); padding:2px 8px; border-radius:30px;">единоразово</span>
                        </div>
                    </div>
                    <div class="upgrade-price-tag" id="superPrice">100</div>
                </div>

                <!-- Золотой клик -->
                <div class="upgrade-card" onclick="buyUpgrade('golden')" id="upgradeGolden">
                    <div class="upgrade-emoji">👑</div>
                    <div class="upgrade-info">
                        <div class="upgrade-title">
                            Золотой клик 
                            <span class="upgrade-level-badge" id="goldenLevel">x2</span>
                            <span class="hotkey-hint">4</span>
                        </div>
                        <div class="upgrade-desc">
                            <i class="fas fa-infinity" style="color:#ffd700;"></i> x2 сила клика 
                            <span style="background:rgba(255,215,0,0.2); padding:2px 8px; border-radius:30px;">уникально</span>
                        </div>
                    </div>
                    <div class="upgrade-price-tag" id="goldenPrice">500</div>
                </div>
            </div>

            <!-- ОТДЕЛЬНАЯ ПАНЕЛЬ СБРОСА -->
            <div class="reset-panel">
                <button class="reset-danger-button" onclick="resetGame()">
                    <i class="fas fa-exclamation-triangle"></i>
                    <span>⚡ СБРОСИТЬ ВЕСЬ ПРОГРЕСС ⚡</span>
                    <i class="fas fa-skull"></i>
                </button>
                <div style="text-align:center; margin-top:10px; font-size:12px; color:rgba(255,100,100,0.7);">
                    <i class="fas fa-ban"></i> Это действие нельзя отменить
                </div>
            </div>
        </div>

        <!-- Профиль секция (РАСШИРЕННАЯ) -->
        <div class="profile-card">
            <!-- Фото-баннер (увеличенный) -->
            <div class="profile-banner" onclick="changeBanner()">
                <img src="https://via.placeholder.com/800x200/2c3e50/ffffff?text=TALLIN" alt="Banner" id="profileBanner">
                <div class="banner-overlay"><i class="fas fa-camera"></i></div>
            </div>

            <div class="profile-header">
                <div class="profile-avatar" onclick="changeAvatar()">
                    <img src="https://via.placeholder.com/120" alt="Avatar" id="profileAvatar">
                    <div class="avatar-overlay"><i class="fas fa-camera"></i></div>
                </div>
                <div class="profile-name">
                    <span id="profileName">TALLIN</span>
                    <i class="fas fa-pencil-alt" onclick="openBioEditor()"></i>
                </div>
                <div class="profile-bio">
                    <span id="profileBio">Лицей • Биология • Лоутаб</span>
                    <i class="fas fa-pencil-alt" onclick="openBioEditor()"></i>
                </div>
                <div class="profile-status">
                    <i class="fas fa-circle"></i> <span id="profileStatus">online</span>
                </div>
            </div>

            <div class="tabs-container">
                <button class="tab-button active" onclick="switchTab('playlist')"><i class="fas fa-music"></i> Плейлист</button>
                <button class="tab-button" onclick="switchTab('pong')"><i class="fas fa-table-tennis"></i> Пинг-понг</button>
                <button class="tab-button" onclick="switchTab('tictactoe')"><i class="fas fa-times"></i> Крестики-нолики</button>
                <button class="tab-button" onclick="switchTab('casino')"><i class="fas fa-dice"></i> Казино</button>
                <button class="tab-button" onclick="switchTab('postgres')"><i class="fas fa-database"></i> PostgreSQL</button>
                <button class="tab-button" onclick="switchTab('restapi')"><i class="fas fa-cloud"></i> REST API</button>
            </div>

            <!-- Плейлист таб -->
            <div class="tab-content active" id="tab-playlist">
                <div class="playlist-section">
                    <div class="tracks-container" id="tracksContainer"></div>
                </div>
                <div class="now-playing-bar">
                    <div class="now-playing-info">
                        <span id="nowTitle">Высокий Градус</span> - <span id="nowArtist">CUPSIZE</span>
                    </div>
                    <div class="control-btns">
                        <button class="control-btn" id="prevBtn"><i class="fas fa-backward"></i></button>
                        <button class="control-btn" id="playPauseBtn"><i class="fas fa-play"></i></button>
                        <button class="control-btn" id="nextBtn"><i class="fas fa-forward"></i></button>
                    </div>
                </div>
                <audio id="audioPlayer" controls></audio>
            </div>

            <!-- Пинг-понг таб -->
            <div class="tab-content" id="tab-pong">
                <div class="pong-container">
                    <div class="pong-canvas-container">
                        <canvas id="pongCanvas"></canvas>
                    </div>
                    <div class="pong-controls">
                        <button class="pong-btn success" onclick="startPongGame()"><i class="fas fa-play"></i> Старт</button>
                        <button class="pong-btn" onclick="pausePongGame()"><i class="fas fa-pause"></i> Пауза</button>
                        <button class="pong-btn danger" onclick="resetPongGame()"><i class="fas fa-redo-alt"></i> Сброс</button>
                    </div>
                    <div class="pong-difficulty" style="display:flex; gap:10px; justify-content:center; margin-top:15px;">
                        <button class="difficulty-btn active" onclick="setPongDifficulty('easy')">Легко</button>
                        <button class="difficulty-btn" onclick="setPongDifficulty('medium')">Средне</button>
                        <button class="difficulty-btn" onclick="setPongDifficulty('hard')">Сложно</button>
                        <button class="difficulty-btn" onclick="setPongDifficulty('impossible')">Невозможно</button>
                    </div>
                </div>
            </div>

            <!-- Крестики-нолики таб -->
            <div class="tab-content" id="tab-tictactoe">
                <div class="tictactoe-container">
                    <h3 style="color:white;">Крестики-нолики с ботом</h3>
                    <div class="ttt-board" id="tttBoard"></div>
                    <div class="ttt-status" id="tttStatus">Ваш ход</div>
                    <div class="ttt-difficulty">
                        <button class="difficulty-btn active" onclick="setTttDifficulty('easy')">Легко</button>
                        <button class="difficulty-btn" onclick="setTttDifficulty('medium')">Средне</button>
                        <button class="difficulty-btn" onclick="setTttDifficulty('hard')">Сложно</button>
                    </div>
                    <div class="ttt-stats">
                        <span>Побед: <span id="tttWins">0</span></span>
                        <span>Ничья: <span id="tttDraws">0</span></span>
                        <span>Поражений: <span id="tttLosses">0</span></span>
                    </div>
                    <button class="pong-btn" onclick="resetTttGame()" style="margin-top:15px;">Новая игра</button>
                </div>
            </div>

            <!-- Казино таб (ОБНОВЛЕННОЕ С ЕЖЕДНЕВНЫМ БОНУСОМ И РАСШИРЕННЫМ МАГАЗИНОМ) -->
            <div class="tab-content" id="tab-casino">
                <div class="casino-container">
                    <h3 style="color:white;">🎰 Однорукий бандит</h3>
                    <div class="casino-balance" id="casinoBalance">100 ₽</div>
                    
                    <!-- ЕЖЕДНЕВНЫЙ БОНУС -->
                    <div class="daily-bonus" id="dailyBonus">
                        <span>🎁 Ежедневный бонус: 50₽</span>
                        <button onclick="claimDailyBonus()" id="dailyBonusBtn">Получить</button>
                    </div>
                    
                    <div class="win-probability" id="winProbability"></div>
                    
                    <div class="slot-machine" id="slotMachine">
                        <div class="slot" id="slot1">🍒</div>
                        <div class="slot" id="slot2">🍒</div>
                        <div class="slot" id="slot3">🍒</div>
                    </div>
                    
                    <div class="bet-controls">
                        <label style="color:white;">Ставка (макс 1000):</label>
                        <input type="number" id="betAmount" class="bet-input" value="10" min="1" max="1000" onchange="updateWinProbability()">
                    </div>
                    
                    <div style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
                        <button class="pong-btn" onclick="spinSlotMachine()" id="spinButton" style="background: linear-gradient(145deg, #ffd700, #ffa500);">
                            <i class="fas fa-play"></i> Крутить
                        </button>
                        <button class="pong-btn" onclick="resetCasino()" style="background: linear-gradient(145deg, #ff6b6b, #ff8e8e);">
                            <i class="fas fa-redo-alt"></i> Сброс баланса
                        </button>
                    </div>
                    
                    <div style="margin-top: 20px; color:white; display: flex; gap: 20px; justify-content: center; flex-wrap: wrap;">
                        <span>🍒 x3 = x5</span>
                        <span>💎 x3 = x10</span>
                        <span>7️⃣ x3 = x20</span>
                    </div>

                    <!-- РАСШИРЕННЫЙ МАГАЗИН КАЗИНО (УВЕЛИЧЕННЫЕ ЦЕНЫ) -->
                    <div class="casino-shop">
                        <div class="shop-title">
                            <i class="fas fa-store"></i> Магазин казино (улучшения)
                        </div>
                        <div class="shop-items" id="casinoShop">
                            <div class="shop-item" onclick="buyShopItem('luck')" id="shopLuck">
                                <span class="shop-item-level" id="luckLevel">Ур.0</span>
                                <div class="shop-item-icon"><i class="fas fa-clover"></i></div>
                                <div class="shop-item-name">Амулет удачи</div>
                                <div class="shop-item-desc">Увеличивает шанс выигрыша на 5% за уровень</div>
                                <div class="shop-item-price" id="luckPrice">1000 ₽</div>
                            </div>
                            <div class="shop-item" onclick="buyShopItem('multiplier')" id="shopMultiplier">
                                <span class="shop-item-level" id="multiplierLevel">Ур.1</span>
                                <div class="shop-item-icon"><i class="fas fa-chart-line"></i></div>
                                <div class="shop-item-name">Множитель выигрыша</div>
                                <div class="shop-item-desc">Увеличивает выигрыш (ур.1 = x1, ур.2 = x2 и т.д.)</div>
                                <div class="shop-item-price" id="multiplierPrice">2000 ₽</div>
                            </div>
                            <div class="shop-item" onclick="buyShopItem('insurance')" id="shopInsurance">
                                <div class="shop-item-icon"><i class="fas fa-shield-alt"></i></div>
                                <div class="shop-item-name">Страховка</div>
                                <div class="shop-item-desc">Возврат 50% при проигрыше (одноразово)</div>
                                <div class="shop-item-price" id="insurancePrice">1500 ₽</div>
                            </div>
                            <div class="shop-item" onclick="buyShopItem('freeSpin')" id="shopFreeSpin">
                                <span class="shop-item-level" id="freeSpinLevel">0</span>
                                <div class="shop-item-icon"><i class="fas fa-gift"></i></div>
                                <div class="shop-item-name">Бесплатные спины</div>
                                <div class="shop-item-desc">Количество бесплатных вращений</div>
                                <div class="shop-item-price" id="freeSpinPrice">500 ₽</div>
                            </div>
                            <div class="shop-item" onclick="buyShopItem('vip')" id="shopVip">
                                <span class="shop-item-level" id="vipLevel">Ур.0</span>
                                <div class="shop-item-icon"><i class="fas fa-crown"></i></div>
                                <div class="shop-item-name">VIP статус</div>
                                <div class="shop-item-desc">+10% к шансу, +0.5 к множителю, +25% страховка</div>
                                <div class="shop-item-price" id="vipPrice">5000 ₽</div>
                            </div>
                            <div class="shop-item" onclick="buyShopItem('doubleWin')" id="shopDoubleWin">
                                <span class="shop-item-level" id="doubleWinLevel">Ур.0</span>
                                <div class="shop-item-icon"><i class="fas fa-star"></i></div>
                                <div class="shop-item-name">Удвоение выигрыша</div>
                                <div class="shop-item-desc">Шанс 10% удвоить любой выигрыш</div>
                                <div class="shop-item-price" id="doubleWinPrice">3000 ₽</div>
                            </div>
                            <div class="shop-item" onclick="buyShopItem('jackpotChance')" id="shopJackpot">
                                <span class="shop-item-level" id="jackpotLevel">Ур.0</span>
                                <div class="shop-item-icon"><i class="fas fa-dragon"></i></div>
                                <div class="shop-item-name">Шанс джекпота</div>
                                <div class="shop-item-desc">Увеличивает шанс выпадения 7️⃣</div>
                                <div class="shop-item-price" id="jackpotPrice">2500 ₽</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- PostgreSQL демо таб -->
            <div class="tab-content" id="tab-postgres">
                <div class="postgres-demo">
                    <div class="postgres-header">
                        <i class="fas fa-database"></i>
                        <h3 style="color:#336791;">PostgreSQL Demo</h3>
                    </div>
                    <p style="color:white; margin-bottom:15px;">Примеры SQL запросов для сохранения игровой статистики</p>
                    
                    <div class="postgres-query">
                        CREATE TABLE users (<br>
                        &nbsp;&nbsp;id SERIAL PRIMARY KEY,<br>
                        &nbsp;&nbsp;username VARCHAR(50) NOT NULL,<br>
                        &nbsp;&nbsp;score BIGINT DEFAULT 0,<br>
                        &nbsp;&nbsp;click_power INT DEFAULT 1,<br>
                        &nbsp;&nbsp;auto_clickers INT DEFAULT 0,<br>
                        &nbsp;&nbsp;casino_balance INT DEFAULT 100,<br>
                        &nbsp;&nbsp;achievements TEXT[],<br>
                        &nbsp;&nbsp;last_save TIMESTAMP DEFAULT NOW()<br>
                        );
                    </div>
                    
                    <div class="postgres-query">
                        INSERT INTO users (username, score, click_power, casino_balance) <br>
                        VALUES ('TALLIN', 1000, 10, 150) <br>
                        RETURNING id;
                    </div>
                    
                    <div class="postgres-query">
                        SELECT username, score, casino_balance <br>
                        FROM users <br>
                        ORDER BY score DESC <br>
                        LIMIT 10;
                    </div>
                    
                    <div class="postgres-result" id="postgresResult">
                        <i class="fas fa-database"></i> Подключение к базе данных...<br>
                        <span id="postgresStatus">✅ PostgreSQL готов к интеграции</span>
                    </div>
                    
                    <button class="pong-btn" onclick="simulatePostgresSave()" style="margin-top:15px; width:100%;">
                        <i class="fas fa-save"></i> Сохранить игру в PostgreSQL (демо)
                    </button>
                </div>
            </div>

            <!-- REST API демо таб -->
            <div class="tab-content" id="tab-restapi">
                <div class="restapi-demo">
                    <div class="restapi-header">
                        <i class="fas fa-cloud"></i>
                        <h3 style="color:#4CAF50;">REST API Demo</h3>
                    </div>
                    <p style="color:white; margin-bottom:15px;">Примеры эндпоинтов для работы с игроками</p>
                    
                    <div class="restapi-endpoint">
                        <span class="restapi-method method-get">GET</span> /api/users
                        <div style="color:#888; font-size:12px; margin-top:5px;">Получить список всех игроков</div>
                    </div>
                    
                    <div class="restapi-endpoint">
                        <span class="restapi-method method-get">GET</span> /api/users/{id}
                        <div style="color:#888; font-size:12px; margin-top:5px;">Получить игрока по ID</div>
                    </div>
                    
                    <div class="restapi-endpoint">
                        <span class="restapi-method method-post">POST</span> /api/users
                        <div style="color:#888; font-size:12px; margin-top:5px;">Создать нового игрока</div>
                        <div style="background:rgba(255,255,255,0.1); padding:5px; border-radius:5px; margin-top:5px; font-size:12px;">
                            { "username": "TALLIN", "score": 1000, "casino_balance": 100 }
                        </div>
                    </div>
                    
                    <div class="restapi-endpoint">
                        <span class="restapi-method method-put">PUT</span> /api/users/{id}
                        <div style="color:#888; font-size:12px; margin-top:5px;">Обновить данные игрока</div>
                    </div>
                    
                    <div class="restapi-endpoint">
                        <span class="restapi-method method-delete">DELETE</span> /api/users/{id}
                        <div style="color:#888; font-size:12px; margin-top:5px;">Удалить игрока</div>
                    </div>
                    
                    <div class="restapi-response" id="restapiResult">
                        <i class="fas fa-cloud"></i> Отправка запроса...<br>
                        <span id="restapiStatus">✅ REST API готов к интеграции</span>
                    </div>
                    
                    <div style="display: flex; gap: 10px; margin-top:15px;">
                        <button class="pong-btn" onclick="simulateRestApiGet()" style="flex:1;">
                            <i class="fas fa-download"></i> GET /users
                        </button>
                        <button class="pong-btn" onclick="simulateRestApiPost()" style="flex:1;">
                            <i class="fas fa-upload"></i> POST /users
                        </button>
                    </div>
                </div>
            </div>

            <!-- Соцсети -->
            <div class="social-links">
                <a href="#" class="social-link"><i class="fab fa-vk"></i> VK</a>
                <a href="#" class="social-link"><i class="fab fa-telegram"></i> Telegram</a>
                <a href="#" class="social-link"><i class="fab fa-steam"></i> Steam</a>
            </div>
        </div>
    </div>

    <!-- Модальное окно достижений -->
    <div class="modal" id="achievementsModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2><i class="fas fa-trophy"></i> Все достижения</h2>
                <button class="close-button" onclick="closeAchievements()"><i class="fas fa-times"></i></button>
            </div>
            <div class="achievements-grid" id="achievementsGrid"></div>
        </div>
    </div>

    <!-- Модальное окно биографии -->
    <div class="bio-modal" id="bioModal">
        <div class="bio-content">
            <h2>Редактировать профиль</h2>
            <input type="text" id="editName" placeholder="Имя" value="TALLIN">
            <textarea id="editBio" placeholder="Биография">Лицей • Биология • Лоутаб</textarea>
            <input type="text" id="editStatus" placeholder="Статус" value="online">
            <button onclick="saveBio()">Сохранить</button>
        </div>
    </div>

    <!-- ЗВУКИ ТОЛЬКО БИПЫ -->
    <audio id="clickSound1" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
    </audio>
    <audio id="clickSound2" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
    </audio>
    <audio id="clickSound3" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
    </audio>
    <audio id="purchaseSound" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
    </audio>
    <audio id="achievementSound" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
    </audio>
    <audio id="casinoWinSound" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
    </audio>

    <script>
        // ==================== КОНСТАНТЫ ====================
        const SAVE_KEY = 'tallin_all_save';
        const BIO_KEY = 'tallin_bio';
        const BANNER_KEY = 'tallin_banner';
        const AVATAR_KEY = 'tallin_avatar';
        const THEME_KEY = 'tallin_theme';
        const DAILY_BONUS_KEY = 'tallin_daily_bonus';

        // Telegram
        let tg = null;
        try { tg = window.Telegram?.WebApp; if(tg) tg.ready(); } catch(e){}

        // Управление экраном загрузки
        window.addEventListener('load', function() {
            setTimeout(function() {
                document.getElementById('loadingScreen').classList.add('fade-out');
            }, 2000);
        });

        // ==================== ДОСТИЖЕНИЯ (52 шт) ====================
        const mainAchievements = [
            { id: 'click1', name: 'Первый клик', desc: 'Сделать 1 клик', icon: 'fa-hand-pointer', game: 'clicker', condition: () => totalClicks >= 1, unlocked: false },
            { id: 'click100', name: 'Трудяга', desc: '100 кликов', icon: 'fa-hand-fist', game: 'clicker', condition: () => totalClicks >= 100, unlocked: false },
            { id: 'click500', name: 'Опытный', desc: '500 кликов', icon: 'fa-hand-peace', game: 'clicker', condition: () => totalClicks >= 500, unlocked: false },
            { id: 'click1000', name: 'Машина', desc: '1000 кликов', icon: 'fa-gem', game: 'clicker', condition: () => totalClicks >= 1000, unlocked: false },
            { id: 'click2500', name: 'Клиборг', desc: '2500 кликов', icon: 'fa-robot', game: 'clicker', condition: () => totalClicks >= 2500, unlocked: false },
            { id: 'click5000', name: 'Легенда', desc: '5000 кликов', icon: 'fa-crown', game: 'clicker', condition: () => totalClicks >= 5000, unlocked: false },
            { id: 'score1k', name: 'Богач', desc: '1000 баллов', icon: 'fa-coins', game: 'clicker', condition: () => score >= 1000, unlocked: false },
            { id: 'score10k', name: 'Миллионер', desc: '10000 баллов', icon: 'fa-sack-dollar', game: 'clicker', condition: () => score >= 10000, unlocked: false },
            { id: 'score100k', name: 'Миллиардер', desc: '100000 баллов', icon: 'fa-kaaba', game: 'clicker', condition: () => score >= 100000, unlocked: false },
            { id: 'crit1', name: 'Критик', desc: 'Первый крит', icon: 'fa-bullseye', game: 'clicker', condition: () => criticalHits >= 1, unlocked: false },
            { id: 'crit100', name: 'Громовержец', desc: '100 критов', icon: 'fa-bolt', game: 'clicker', condition: () => criticalHits >= 100, unlocked: false },
            { id: 'auto5', name: 'Автоматизатор', desc: '5 авто-кликеров', icon: 'fa-robot', game: 'clicker', condition: () => autoClickers >= 5, unlocked: false },
            { id: 'auto10', name: 'Армия', desc: '10 авто-кликеров', icon: 'fa-users-gear', game: 'clicker', condition: () => autoClickers >= 10, unlocked: false },
            { id: 'super', name: 'Суперсила', desc: 'Купить супер клик', icon: 'fa-bolt', game: 'clicker', condition: () => superClickBought, unlocked: false },
            { id: 'golden', name: 'Золотой', desc: 'Купить золотой клик', icon: 'fa-crown', game: 'clicker', condition: () => goldenClickBought, unlocked: false },
        ];

        const pongAchievements = [
            { id: 'pong1', name: 'Первая победа', desc: 'Выиграть 1 игру', icon: 'fa-table-tennis', game: 'pong', condition: () => pongWins >= 1, unlocked: false },
            { id: 'pong10', name: 'Чемпион', desc: '10 побед', icon: 'fa-trophy', game: 'pong', condition: () => pongWins >= 10, unlocked: false },
            { id: 'pong25', name: 'Мастер', desc: '25 побед', icon: 'fa-crown', game: 'pong', condition: () => pongWins >= 25, unlocked: false },
            { id: 'pong50', name: 'Профи', desc: '50 побед', icon: 'fa-star', game: 'pong', condition: () => pongWins >= 50, unlocked: false },
            { id: 'pong100', name: 'Легенда', desc: '100 побед', icon: 'fa-chess-queen', game: 'pong', condition: () => pongWins >= 100, unlocked: false },
            { id: 'pongPerfect', name: 'Сухарь', desc: 'Победа 5:0', icon: 'fa-broom', game: 'pong', condition: () => pongPerfectGames >= 1, unlocked: false },
            { id: 'pongAce', name: 'Эйс', desc: 'Подача навылет', icon: 'fa-bullseye', game: 'pong', condition: () => pongAces >= 1, unlocked: false },
            { id: 'pongComeback', name: 'Камбэк', desc: 'С 0:4 к победе', icon: 'fa-rotate-left', game: 'pong', condition: () => pongComebacks >= 1, unlocked: false },
            { id: 'pongImpossible', name: 'Невозможное', desc: 'Победа на impossible', icon: 'fa-dragon', game: 'pong', condition: () => pongImpossibleWins >= 1, unlocked: false },
        ];

        const tttAchievements = [
            { id: 'ttt1', name: 'Первые крестики', desc: 'Выиграть 1 игру', icon: 'fa-times', game: 'tictactoe', condition: () => tttWins >= 1, unlocked: false },
            { id: 'ttt10', name: 'Тактик', desc: '10 побед', icon: 'fa-trophy', game: 'tictactoe', condition: () => tttWins >= 10, unlocked: false },
            { id: 'ttt25', name: 'Стратег', desc: '25 побед', icon: 'fa-crown', game: 'tictactoe', condition: () => tttWins >= 25, unlocked: false },
            { id: 'tttPerfect', name: 'Идеал', desc: 'Выиграть 3-0', icon: 'fa-star', game: 'tictactoe', condition: () => tttPerfectGames >= 1, unlocked: false },
            { id: 'tttHard', name: 'Победитель бога', desc: 'Победа на сложном', icon: 'fa-dragon', game: 'tictactoe', condition: () => tttHardWins >= 1, unlocked: false },
            { id: 'tttNoLoss', name: 'Непобедимый', desc: '10 игр без поражений', icon: 'fa-shield', game: 'tictactoe', condition: () => tttNoLossStreak >= 10, unlocked: false },
        ];

        const casinoAchievements = [
            { id: 'casino1', name: 'Первая ставка', desc: 'Сделать первую ставку в казино', icon: 'fa-dice', game: 'casino', condition: () => casinoTotalBets >= 1, unlocked: false },
            { id: 'casino10', name: 'Игрок', desc: 'Выиграть 10 раз', icon: 'fa-trophy', game: 'casino', condition: () => casinoWins >= 10, unlocked: false },
            { id: 'casinoJackpot', name: 'Джекпот', desc: 'Сорвать джекпот (3 семерки)', icon: 'fa-crown', game: 'casino', condition: () => casinoJackpots >= 1, unlocked: false },
            { id: 'casinoShop', name: 'Шопоголик', desc: 'Купить предмет в магазине', icon: 'fa-shopping-cart', game: 'casino', condition: () => casinoShopPurchases >= 1, unlocked: false },
            { id: 'casinoHighRoller', name: 'Хайроллер', desc: 'Поставить 500₽ за раз', icon: 'fa-gem', game: 'casino', condition: () => casinoHighRoller >= 1, unlocked: false },
            { id: 'casinoLucky', name: 'Счастливчик', desc: 'Выиграть с минимальным шансом', icon: 'fa-star', game: 'casino', condition: () => casinoLuckyWin >= 1, unlocked: false },
            { id: 'casinoDaily', name: 'Ежедневный', desc: 'Получить 10 ежедневных бонусов', icon: 'fa-calendar', game: 'casino', condition: () => casinoDailyBonuses >= 10, unlocked: false },
            { id: 'casinoVIP', name: 'VIP', desc: 'Купить VIP статус', icon: 'fa-crown', game: 'casino', condition: () => casinoVIP > 0, unlocked: false },
            { id: 'casinoDouble', name: 'Удачливый', desc: 'Активировать удвоение выигрыша', icon: 'fa-star', game: 'casino', condition: () => casinoDoubleWins >= 1, unlocked: false },
        ];

        const allAchievements = [...mainAchievements, ...pongAchievements, ...tttAchievements, ...casinoAchievements];

        // ==================== ПЕРЕМЕННЫЕ КЛИКЕРА ====================
        let score = 0;
        let clickPower = 1;
        let autoClickers = 0;
        let autoInterval = null;
        let totalClicks = 0;
        let criticalHits = 0;
        let clickPrice = 10;
        let autoPrice = 50;
        let superPrice = 100;
        let superClickBought = false;
        let goldenPrice = 500;
        let goldenClickBought = false;
        let clickBonus = 0, autoBonus = 0, critChance = 0;
        let superBonusActive = false, superBonusMultiplier = 2;
        let superBonusTimeLeft = 0, superBonusInterval;

        // ==================== ПЕРЕМЕННЫЕ КАЗИНО (РАСШИРЕННЫЕ) ====================
        let casinoBalance = 100;
        let casinoTotalBets = 0;
        let casinoWins = 0;
        let casinoJackpots = 0;
        let casinoShopPurchases = 0;
        let casinoHighRoller = 0;
        let casinoLuckyWin = 0;
        let casinoDailyBonuses = 0;
        let casinoVIP = 0;
        let casinoDoubleWins = 0;
        
        // Бонусы магазина (расширенные)
        let casinoLuckBonus = 0; // +% к шансу выигрыша за уровень
        let casinoMultiplier = 1; // базовый множитель
        let casinoInsurance = false; // страховка (одноразовая)
        let casinoFreeSpins = 0; // количество бесплатных спинов
        let casinoVIPLevel = 0; // уровень VIP (0-5)
        let casinoDoubleWinChance = 0; // шанс удвоения выигрыша (%)
        let casinoJackpotBonus = 0; // +% к шансу джекпота

        // Кулдаун для казино (5 секунд)
        let casinoCooldown = false;
        let casinoCooldownTimer = null;

        // ==================== ПИНГ-ПОНГ ====================
        let pongWins = 0, pongPerfectGames = 0, pongAces = 0, pongComebacks = 0, pongImpossibleWins = 0;
        let pongGameRunning = false, pongGamePaused = false;
        let pongCanvas, pongCtx, pongWidth, pongHeight;
        let player = { y: 0, height: 80, width: 10, score: 0 };
        let bot = { y: 0, height: 80, width: 10, score: 0 };
        let ball = { x: 0, y: 0, size: 8, velocityX: 5, velocityY: 5 };
        let pongDifficulty = 'easy';
        let botSpeed = 3, ballSpeed = 5;
        let keys = {}, mouseY = 0;
        let currentGamePerfect = true, lastHitWasAce = true, comebackTracker = false;

        // ==================== КРЕСТИКИ-НОЛИКИ ====================
        let tttBoard = ['', '', '', '', '', '', '', '', ''];
        let tttGameActive = true;
        let tttDifficulty = 'easy';
        let tttWins = 0, tttDraws = 0, tttLosses = 0;
        let tttPerfectGames = 0, tttHardWins = 0, tttNoLossStreak = 0;

        // ==================== ПЛЕЙЛИСТ ====================
        const playlist = [
            { title: "Высокий Градус", artist: "CUPSIZE", src: "CUPSIZE - Высокий градус (zaycev.net).mp3" },
            { title: "Цветы", artist: "Темный Принц", src: "tjomnyjj_princ_-_cvety_79225579.mp3" },
            { title: "Маршрутка", artist: "CUPSIZE", src: "CUPSIZE_-_marshrutka_78201129.mp3" }
        ];
        let currentTrack = 0;
        const audio = document.getElementById('audioPlayer');

        // ==================== ЗВУКИ ====================
        const clickSounds = [
            document.getElementById('clickSound1'),
            document.getElementById('clickSound2'),
            document.getElementById('clickSound3')
        ];
        const purchaseSound = document.getElementById('purchaseSound');
        const achievementSound = document.getElementById('achievementSound');
        const casinoWinSound = document.getElementById('casinoWinSound');

        function playSound(soundElement, volume = 0.3) {
            if (!soundElement) return;
            soundElement.pause();
            soundElement.currentTime = 0;
            soundElement.volume = volume;
            const playPromise = soundElement.play();
            if (playPromise !== undefined) {
                playPromise.catch(error => {
                    console.log('Ошибка воспроизведения звука:', error);
                });
            }
        }

        function playRandomClickSound() {
            const randomIndex = Math.floor(Math.random() * clickSounds.length);
            playSound(clickSounds[randomIndex], 0.3);
        }

        function playPurchaseSound() {
            playSound(purchaseSound, 0.4);
        }

        function playAchievementSound() {
            playSound(achievementSound, 0.5);
        }

        function playCasinoWinSound() {
            playSound(casinoWinSound, 0.5);
        }

        // ==================== ВСПЛЫВАЮЩИЕ УВЕДОМЛЕНИЯ ====================
        function showNotification(message, type = 'info') {
            const container = document.getElementById('notificationContainer');
            const notification = document.createElement('div');
            notification.className = `notification ${type}`;
            
            let icon = 'fa-info-circle';
            if (type === 'success') icon = 'fa-check-circle';
            else if (type === 'error') icon = 'fa-exclamation-circle';
            else if (type === 'warning') icon = 'fa-exclamation-triangle';
            
            notification.innerHTML = `<i class="fas ${icon}"></i><span>${message}</span>`;
            container.appendChild(notification);
            
            setTimeout(() => {
                notification.remove();
            }, 3000);
        }

        // ==================== ФУНКЦИИ ЗАГРУЗКИ ====================
        function loadGame() {
            const saved = localStorage.getItem(SAVE_KEY);
            if (saved) {
                try {
                    const data = JSON.parse(saved);
                    score = data.score || 0;
                    clickPower = data.clickPower || 1;
                    autoClickers = data.autoClickers || 0;
                    totalClicks = data.totalClicks || 0;
                    criticalHits = data.criticalHits || 0;
                    clickPrice = data.clickPrice || 10;
                    autoPrice = data.autoPrice || 50;
                    superPrice = data.superPrice || 100;
                    superClickBought = data.superClickBought || false;
                    goldenPrice = data.goldenPrice || 500;
                    goldenClickBought = data.goldenClickBought || false;
                    pongWins = data.pongWins || 0;
                    pongPerfectGames = data.pongPerfectGames || 0;
                    pongAces = data.pongAces || 0;
                    pongComebacks = data.pongComebacks || 0;
                    pongImpossibleWins = data.pongImpossibleWins || 0;
                    tttWins = data.tttWins || 0;
                    tttDraws = data.tttDraws || 0;
                    tttLosses = data.tttLosses || 0;
                    tttPerfectGames = data.tttPerfectGames || 0;
                    tttHardWins = data.tttHardWins || 0;
                    tttNoLossStreak = data.tttNoLossStreak || 0;
                    casinoBalance = data.casinoBalance || 100;
                    casinoTotalBets = data.casinoTotalBets || 0;
                    casinoWins = data.casinoWins || 0;
                    casinoJackpots = data.casinoJackpots || 0;
                    casinoShopPurchases = data.casinoShopPurchases || 0;
                    casinoHighRoller = data.casinoHighRoller || 0;
                    casinoLuckyWin = data.casinoLuckyWin || 0;
                    casinoDailyBonuses = data.casinoDailyBonuses || 0;
                    casinoVIP = data.casinoVIP || 0;
                    casinoDoubleWins = data.casinoDoubleWins || 0;
                    casinoLuckBonus = data.casinoLuckBonus || 0;
                    casinoMultiplier = data.casinoMultiplier || 1;
                    casinoInsurance = data.casinoInsurance || false;
                    casinoFreeSpins = data.casinoFreeSpins || 0;
                    casinoVIPLevel = data.casinoVIPLevel || 0;
                    casinoDoubleWinChance = data.casinoDoubleWinChance || 0;
                    casinoJackpotBonus = data.casinoJackpotBonus || 0;
                    
                    if (data.achievements) {
                        data.achievements.forEach(saved => {
                            const ach = allAchievements.find(a => a.id === saved.id);
                            if (ach) ach.unlocked = saved.unlocked;
                        });
                    }
                } catch(e){}
            }
            updateUI();
            updateCasinoUI();
            updateShopUI();
            checkDailyBonus();
        }

        function saveGame() {
            const data = {
                score, clickPower, autoClickers, totalClicks, criticalHits,
                clickPrice, autoPrice, superPrice, superClickBought, goldenPrice, goldenClickBought,
                pongWins, pongPerfectGames, pongAces, pongComebacks, pongImpossibleWins,
                tttWins, tttDraws, tttLosses, tttPerfectGames, tttHardWins, tttNoLossStreak,
                casinoBalance, casinoTotalBets, casinoWins, casinoJackpots,
                casinoShopPurchases, casinoHighRoller, casinoLuckyWin, casinoDailyBonuses,
                casinoVIP, casinoDoubleWins, casinoLuckBonus, casinoMultiplier,
                casinoInsurance, casinoFreeSpins, casinoVIPLevel, casinoDoubleWinChance, casinoJackpotBonus,
                achievements: allAchievements.map(a => ({ id: a.id, unlocked: a.unlocked }))
            };
            localStorage.setItem(SAVE_KEY, JSON.stringify(data));
        }

        // ==================== ДОСТИЖЕНИЯ ====================
        function checkAchievements() {
            let anyNew = false;
            allAchievements.forEach(ach => {
                if (!ach.unlocked && ach.condition()) {
                    ach.unlocked = true;
                    anyNew = true;
                    playAchievementSound();
                    showNotification(`🏆 ${ach.name}`, 'success');
                }
            });
            if (anyNew) { updateTotalCount(); saveGame(); }
        }

        function updateTotalCount() {
            const unlocked = allAchievements.filter(a => a.unlocked).length;
            document.getElementById('totalAchievementCount').textContent = `${unlocked}/${allAchievements.length}`;
        }

        function openAllAchievements() {
            const grid = document.getElementById('achievementsGrid');
            grid.innerHTML = allAchievements.map(ach => {
                const gameClass = ach.game === 'clicker' ? 'clicker' : ach.game === 'pong' ? 'pong' : ach.game === 'tictactoe' ? 'tictactoe' : 'casino';
                const gameName = ach.game === 'clicker' ? 'Кликер' : ach.game === 'pong' ? 'Пинг-понг' : ach.game === 'tictactoe' ? 'Крестики-нолики' : 'Казино';
                return `<div class="achievement-card ${ach.unlocked ? 'unlocked' : ''}">
                    <div class="game-badge ${gameClass}">${gameName}</div>
                    <div class="achievement-icon"><i class="fas ${ach.icon}"></i></div>
                    <div class="achievement-info">
                        <div class="achievement-name">${ach.name}</div>
                        <div class="achievement-desc">${ach.desc}</div>
                    </div>
                    <div class="achievement-status"><i class="fas ${ach.unlocked ? 'fa-check-circle' : 'fa-lock'}"></i></div>
                </div>`;
            }).join('');
            document.getElementById('achievementsModal').classList.add('active');
        }

        function closeAchievements() { document.getElementById('achievementsModal').classList.remove('active'); }

        // ==================== КЛИКЕР ====================
        function calculateClickPower() {
            let power = clickPower * (1 + clickBonus/100);
            if (superBonusActive) {
                const randomMultiplier = 2 + Math.random();
                power *= randomMultiplier;
                showNotification(`✨ Бонус x${randomMultiplier.toFixed(1)}!`, 'success');
            }
            if (Math.random() * 100 < critChance) { 
                criticalHits++; 
                power *= 2;
                showNotification('🎯 Критический удар!', 'warning');
            }
            return Math.floor(power);
        }

        function handleClick() {
            score += calculateClickPower(); totalClicks++;
            playRandomClickSound();
            updateUI(); checkAchievements(); saveGame();
            document.getElementById('clickButton').style.transform = 'scale(0.95)';
            setTimeout(() => document.getElementById('clickButton').style.transform = 'scale(1)', 100);
        }

        function buyUpgrade(type) {
            let success = false;
            if (type === 'click' && score >= clickPrice) {
                score -= clickPrice; clickPower++; clickPrice = Math.floor(clickPrice * 1.5); success = true;
            } else if (type === 'auto' && score >= autoPrice) {
                score -= autoPrice; autoClickers++; autoPrice = Math.floor(autoPrice * 1.5); success = true;
            } else if (type === 'super' && !superClickBought && score >= superPrice) {
                score -= superPrice; clickPower += 5; superClickBought = true; success = true;
            } else if (type === 'golden' && !goldenClickBought && score >= goldenPrice) {
                score -= goldenPrice; clickPower *= 2; goldenClickBought = true; success = true;
            }
            if (success) {
                playPurchaseSound();
                showNotification('✅ Улучшение куплено!', 'success');
            } else {
                showNotification('❌ Не хватает монет!', 'error');
            }
            updateUI(); checkAchievements(); saveGame();
        }

        function updateUI() {
            document.getElementById('score').textContent = Math.floor(score);
            document.getElementById('perClick').textContent = '+' + calculateClickPower();
            document.getElementById('perSecond').textContent = autoClickers;
            document.getElementById('clickPrice').textContent = clickPrice;
            document.getElementById('autoPrice').textContent = autoPrice;
            document.getElementById('superPrice').textContent = superClickBought ? '✓' : superPrice;
            document.getElementById('goldenPrice').textContent = goldenClickBought ? '✓' : goldenPrice;
            document.getElementById('clickLevel').textContent = `ур.${clickPower-1}`;
            document.getElementById('autoLevel').textContent = `ур.${autoClickers}`;
            document.getElementById('superLevel').textContent = superClickBought ? 'куплено' : 'ур.0';
            document.getElementById('goldenLevel').textContent = goldenClickBought ? 'активно' : 'x2';

            document.getElementById('upgradeClick').classList.toggle('cant-afford', score < clickPrice && !superClickBought);
            document.getElementById('upgradeAuto').classList.toggle('cant-afford', score < autoPrice);
            document.getElementById('upgradeSuper').classList.toggle('cant-afford', score < superPrice && !superClickBought);
            document.getElementById('upgradeGolden').classList.toggle('cant-afford', score < goldenPrice && !goldenClickBought);
            if (superClickBought) document.getElementById('upgradeSuper').classList.add('disabled');
            if (goldenClickBought) document.getElementById('upgradeGolden').classList.add('disabled');

            updateTotalCount();
            if (autoInterval) clearInterval(autoInterval);
            if (autoClickers > 0) {
                autoInterval = setInterval(() => { score += autoClickers; checkAchievements(); saveGame(); updateUI(); }, 1000);
            }
        }

        // ==================== КАЗИНО (ОБНОВЛЕННОЕ С КУЛДАУНОМ) ====================
        function updateCasinoUI() {
            document.getElementById('casinoBalance').textContent = casinoBalance + ' ₽';
            updateWinProbability();
        }

        function updateWinProbability() {
            const bet = parseInt(document.getElementById('betAmount').value) || 10;
            // Шанс выигрыша обратно пропорционален ставке
            let baseChance = Math.max(5, 30 - Math.floor(bet / 10));
            baseChance = Math.min(40, baseChance);
            
            // Добавляем бонусы
            const totalChance = baseChance + casinoLuckBonus + (casinoVIPLevel * 2);
            
            document.getElementById('winProbability').innerHTML = 
                `Шанс выигрыша: <span style="color:#ffd700;">${Math.min(80, totalChance)}%</span> (чем больше ставка, тем меньше шанс)`;
        }

        function startCasinoCooldown() {
            casinoCooldown = true;
            const spinButton = document.getElementById('spinButton');
            spinButton.disabled = true;
            spinButton.style.opacity = '0.5';
            spinButton.innerHTML = '<i class="fas fa-hourglass"></i> 3с';
            
            let secondsLeft = 5;
            casinoCooldownTimer = setInterval(() => {
                secondsLeft--;
                if (secondsLeft > 0) {
                    spinButton.innerHTML = `<i class="fas fa-hourglass"></i> ${secondsLeft}с`;
                } else {
                    clearInterval(casinoCooldownTimer);
                    casinoCooldown = false;
                    spinButton.disabled = false;
                    spinButton.style.opacity = '1';
                    spinButton.innerHTML = '<i class="fas fa-play"></i> Крутить';
                }
            }, 1000);
        }

        function spinSlotMachine() {
            // Проверяем кулдаун
            if (casinoCooldown) {
                showNotification('⏳ Подождите 5 секунд между вращениями!', 'warning');
                return;
            }

            let betAmount = parseInt(document.getElementById('betAmount').value);
            
            // Принудительно ограничиваем ставку до 1000
            if (betAmount > 1000) {
                betAmount = 1000;
                document.getElementById('betAmount').value = 1000;
            }
            
            // Используем бесплатный спин если есть
            if (casinoFreeSpins > 0) {
                casinoFreeSpins--;
                showNotification('🎰 Использован бесплатный спин!', 'info');
            } else {
                if (betAmount > casinoBalance) {
                    showNotification('❌ Недостаточно средств!', 'error');
                    return;
                }
                casinoBalance -= betAmount;
            }
            
            if (betAmount < 1) {
                showNotification('❌ Минимальная ставка 1₽', 'error');
                return;
            }

            if (betAmount >= 500) {
                casinoHighRoller++;
                checkAchievements();
            }

            casinoTotalBets++;

            // Определяем выигрыш
            const baseWinChance = Math.max(5, 30 - Math.floor(betAmount / 10));
            const winChance = Math.min(80, baseWinChance + casinoLuckBonus + (casinoVIPLevel * 2));
            
            const isWin = Math.random() * 100 < winChance;
            
            let slot1, slot2, slot3;
            let multiplier = 0;

            if (isWin) {
                // Генерируем выигрышную комбинацию
                const jackpotRoll = Math.random() * 100;
                const jackpotThreshold = 10 + casinoJackpotBonus;
                
                if (jackpotRoll < jackpotThreshold) { // джекпот
                    slot1 = slot2 = slot3 = '7️⃣';
                    multiplier = 20;
                    casinoJackpots++;
                    showNotification('🎰 ДЖЕКПОТ!', 'success');
                } else if (Math.random() < 0.3) { // 30% - хороший выигрыш
                    slot1 = slot2 = slot3 = '💎';
                    multiplier = 10;
                } else { // 60% - обычный выигрыш
                    slot1 = slot2 = slot3 = '🍒';
                    multiplier = 5;
                }
                
                if (multiplier === 20) {
                    casinoLuckyWin++;
                }
                
                casinoWins++;
            } else {
                // Генерируем проигрышную комбинацию
                const symbols = ['🍒', '🍒', '🍒', '🍒', '💎', '💎', '7️⃣'];
                slot1 = symbols[Math.floor(Math.random() * symbols.length)];
                slot2 = symbols[Math.floor(Math.random() * symbols.length)];
                slot3 = symbols[Math.floor(Math.random() * symbols.length)];
                
                // Убеждаемся что не все три одинаковые
                while (slot1 === slot2 && slot2 === slot3) {
                    slot3 = symbols[Math.floor(Math.random() * symbols.length)];
                }
            }

            document.getElementById('slot1').textContent = slot1;
            document.getElementById('slot2').textContent = slot2;
            document.getElementById('slot3').textContent = slot3;

            if (multiplier > 0) {
                let winAmount = betAmount * multiplier * (casinoMultiplier + casinoVIPLevel * 0.5);
                
                // Проверка на удвоение
                if (Math.random() * 100 < casinoDoubleWinChance) {
                    winAmount *= 2;
                    casinoDoubleWins++;
                    showNotification('✨ Удвоение выигрыша!', 'success');
                }
                
                casinoBalance += Math.floor(winAmount);
                playCasinoWinSound();
                showNotification(`🎉 Вы выиграли ${Math.floor(winAmount)}₽! (x${multiplier * (casinoMultiplier + casinoVIPLevel * 0.5)})`, 'success');
            } else {
                if (casinoInsurance) {
                    const refund = Math.floor(betAmount * (0.5 + casinoVIPLevel * 0.05));
                    casinoBalance += refund;
                    showNotification(`😢 Проигрыш, но страховка вернула ${refund}₽`, 'info');
                } else {
                    showNotification('😢 Попробуйте еще раз', 'info');
                }
            }

            updateCasinoUI();
            checkAchievements();
            saveGame();

            // Запускаем кулдаун после успешного вращения
            startCasinoCooldown();
        }

        function resetCasino() {
            casinoBalance = 100;
            casinoTotalBets = 0;
            casinoWins = 0;
            casinoJackpots = 0;
            casinoLuckBonus = 0;
            casinoMultiplier = 1;
            casinoInsurance = false;
            casinoFreeSpins = 0;
            casinoVIPLevel = 0;
            casinoDoubleWinChance = 0;
            casinoJackpotBonus = 0;
            document.getElementById('slot1').textContent = '🍒';
            document.getElementById('slot2').textContent = '🍒';
            document.getElementById('slot3').textContent = '🍒';
            updateCasinoUI();
            updateShopUI();
            showNotification('🔄 Казино сброшено до начальных значений', 'info');
            saveGame();
        }

        // ==================== ЕЖЕДНЕВНЫЙ БОНУС ====================
        function checkDailyBonus() {
            const lastBonus = localStorage.getItem(DAILY_BONUS_KEY);
            const today = new Date().toDateString();
            
            if (lastBonus === today) {
                document.getElementById('dailyBonusBtn').disabled = true;
                document.getElementById('dailyBonusBtn').textContent = 'Уже получено';
            } else {
                document.getElementById('dailyBonusBtn').disabled = false;
                document.getElementById('dailyBonusBtn').textContent = 'Получить';
            }
        }

        function claimDailyBonus() {
            const lastBonus = localStorage.getItem(DAILY_BONUS_KEY);
            const today = new Date().toDateString();
            
            if (lastBonus !== today) {
                casinoBalance += 50;
                casinoDailyBonuses++;
                localStorage.setItem(DAILY_BONUS_KEY, today);
                showNotification('🎁 Получен ежедневный бонус 50₽!', 'success');
                updateCasinoUI();
                checkAchievements();
                saveGame();
                checkDailyBonus();
            }
        }

        // ==================== РАСШИРЕННЫЙ МАГАЗИН КАЗИНО (УВЕЛИЧЕННЫЕ ЦЕНЫ) ====================
        function updateShopUI() {
            document.getElementById('luckLevel').textContent = `Ур.${casinoLuckBonus/5}`;
            document.getElementById('luckPrice').textContent = (1000 + casinoLuckBonus * 200) + ' ₽';
            
            document.getElementById('multiplierLevel').textContent = `Ур.${casinoMultiplier}`;
            document.getElementById('multiplierPrice').textContent = (2000 * casinoMultiplier) + ' ₽';
            
            document.getElementById('insurancePrice').textContent = casinoInsurance ? 'Куплено' : '1500 ₽';
            document.getElementById('shopInsurance').classList.toggle('disabled', casinoInsurance);
            
            document.getElementById('freeSpinLevel').textContent = casinoFreeSpins;
            document.getElementById('freeSpinPrice').textContent = (500 + casinoFreeSpins * 100) + ' ₽';
            
            document.getElementById('vipLevel').textContent = `Ур.${casinoVIPLevel}`;
            document.getElementById('vipPrice').textContent = (5000 * (casinoVIPLevel + 1)) + ' ₽';
            
            document.getElementById('doubleWinLevel').textContent = `Ур.${casinoDoubleWinChance/10}`;
            document.getElementById('doubleWinPrice').textContent = (3000 + casinoDoubleWinChance * 200) + ' ₽';
            
            document.getElementById('jackpotLevel').textContent = `Ур.${casinoJackpotBonus}`;
            document.getElementById('jackpotPrice').textContent = (2500 + casinoJackpotBonus * 500) + ' ₽';
        }

        function buyShopItem(item) {
            let price = 0;
            let success = false;
            
            switch(item) {
                case 'luck':
                    price = 1000 + casinoLuckBonus * 200;
                    if (casinoBalance >= price && casinoLuckBonus < 50) {
                        casinoBalance -= price;
                        casinoLuckBonus += 5;
                        success = true;
                        showNotification('🍀 Амулет удачи улучшен! +5% к шансу', 'success');
                    }
                    break;
                    
                case 'multiplier':
                    price = 2000 * casinoMultiplier;
                    if (casinoBalance >= price && casinoMultiplier < 10) {
                        casinoBalance -= price;
                        casinoMultiplier++;
                        success = true;
                        showNotification('📈 Множитель увеличен!', 'success');
                    }
                    break;
                    
                case 'insurance':
                    price = 1500;
                    if (!casinoInsurance && casinoBalance >= price) {
                        casinoBalance -= price;
                        casinoInsurance = true;
                        success = true;
                        showNotification('🛡️ Страховка активирована!', 'success');
                    }
                    break;
                    
                case 'freeSpin':
                    price = 500 + casinoFreeSpins * 100;
                    if (casinoBalance >= price) {
                        casinoBalance -= price;
                        casinoFreeSpins++;
                        success = true;
                        showNotification('🎰 Бесплатный спин получен!', 'success');
                    }
                    break;
                    
                case 'vip':
                    price = 5000 * (casinoVIPLevel + 1);
                    if (casinoBalance >= price && casinoVIPLevel < 5) {
                        casinoBalance -= price;
                        casinoVIPLevel++;
                        casinoVIP++;
                        success = true;
                        showNotification('👑 VIP статус повышен! +2% к шансу, +0.5 к множителю', 'success');
                    }
                    break;
                    
                case 'doubleWin':
                    price = 3000 + casinoDoubleWinChance * 200;
                    if (casinoBalance >= price && casinoDoubleWinChance < 50) {
                        casinoBalance -= price;
                        casinoDoubleWinChance += 10;
                        success = true;
                        showNotification('✨ Шанс удвоения увеличен!', 'success');
                    }
                    break;
                    
                case 'jackpot':
                    price = 2500 + casinoJackpotBonus * 500;
                    if (casinoBalance >= price && casinoJackpotBonus < 20) {
                        casinoBalance -= price;
                        casinoJackpotBonus++;
                        success = true;
                        showNotification('🐉 Шанс джекпота увеличен!', 'success');
                    }
                    break;
            }
            
            if (success) {
                casinoShopPurchases++;
                playPurchaseSound();
                updateCasinoUI();
                updateShopUI();
                checkAchievements();
                saveGame();
            } else {
                showNotification('❌ Недостаточно средств или достигнут максимум', 'error');
            }
        }

        // ==================== ПИНГ-ПОНГ ====================
        function initPong() {
            pongCanvas = document.getElementById('pongCanvas');
            if (!pongCanvas) return;
            pongCtx = pongCanvas.getContext('2d');
            resizePong();
            resetPongGame();
            window.addEventListener('resize', resizePong);
            document.addEventListener('keydown', e => keys[e.key] = true);
            document.addEventListener('keyup', e => keys[e.key] = false);
            pongCanvas.addEventListener('mousemove', e => {
                const rect = pongCanvas.getBoundingClientRect();
                mouseY = (e.clientY - rect.top) * (pongCanvas.height / rect.height);
            });
            pongGameLoop();
        }

        function resizePong() {
            if (!pongCanvas) return;
            pongWidth = pongCanvas.parentElement.clientWidth;
            pongHeight = pongCanvas.parentElement.clientHeight;
            pongCanvas.width = pongWidth; pongCanvas.height = pongHeight;
            player.y = pongHeight/2 - player.height/2;
            bot.y = pongHeight/2 - bot.height/2;
            ball.x = pongWidth/2; ball.y = pongHeight/2;
        }

        function resetBall() {
            ball.x = pongWidth/2; ball.y = pongHeight/2;
            ball.velocityX = (Math.random()>0.5?1:-1)*ballSpeed;
            ball.velocityY = (Math.random()*2-1)*ballSpeed;
        }

        function resetPongGame() { player.score=0; bot.score=0; currentGamePerfect=true; lastHitWasAce=true; comebackTracker=false; resetBall(); }

        function setPongDifficulty(level) {
            pongDifficulty = level;
            document.querySelectorAll('#tab-pong .difficulty-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            const sets = { easy:[2,4], medium:[4,6], hard:[6,8], impossible:[8,10] };
            botSpeed = sets[level][0]; ballSpeed = sets[level][1];
            ball.velocityX = (ball.velocityX>0?1:-1)*ballSpeed;
            ball.velocityY = (ball.velocityY>0?1:-1)*ballSpeed;
        }

        function startPongGame() { pongGameRunning = true; pongGamePaused = false; }
        function pausePongGame() { pongGamePaused = !pongGamePaused; }

        function updatePong() {
            if (!pongGameRunning || pongGamePaused) return;
            if (player.score===0 && bot.score===4) comebackTracker=true;
            if (keys['ArrowUp'] && player.y>0) player.y-=7;
            if (keys['ArrowDown'] && player.y<pongHeight-player.height) player.y+=7;
            if (mouseY) player.y = Math.max(0, Math.min(pongHeight-player.height, mouseY-player.height/2));
            if (ball.y > bot.y+bot.height/2) bot.y+=botSpeed; else if (ball.y < bot.y+bot.height/2) bot.y-=botSpeed;
            bot.y = Math.max(0, Math.min(pongHeight-bot.height, bot.y));
            ball.x += ball.velocityX; ball.y += ball.velocityY;
            if (ball.y<=0 || ball.y>=pongHeight) ball.velocityY*=-1;
            if (ball.x <= player.width && ball.y>=player.y && ball.y<=player.y+player.height) {
                ball.velocityX = Math.abs(ball.velocityX); lastHitWasAce = false;
            } else if (ball.x <= player.width) lastHitWasAce = true;
            if (ball.x >= pongWidth - bot.width && ball.y>=bot.y && ball.y<=bot.y+bot.height) ball.velocityX = -Math.abs(ball.velocityX);
            if (ball.x < 0) {
                bot.score++;
                if (lastHitWasAce) pongAces++;
                if (bot.score >=5) {
                    pongWins++; if (currentGamePerfect) pongPerfectGames++; if (comebackTracker) pongComebacks++;
                    player.score=0; bot.score=0;
                }
                resetBall();
            } else if (ball.x > pongWidth) {
                player.score++;
                if (player.score >=5) {
                    pongWins++; if (pongDifficulty==='impossible') pongImpossibleWins++;
                    player.score=0; bot.score=0;
                }
                resetBall();
            }
        }

        function drawPong() {
            pongCtx.fillStyle = 'rgba(0,0,0,0.1)'; pongCtx.fillRect(0,0,pongWidth,pongHeight);
            pongCtx.fillStyle = '#ffd700'; pongCtx.fillRect(10, player.y, player.width, player.height);
            pongCtx.fillStyle = '#ff4d4d'; pongCtx.fillRect(pongWidth-20, bot.y, bot.width, bot.height);
            pongCtx.fillStyle = 'white';
            pongCtx.beginPath(); pongCtx.arc(ball.x, ball.y, ball.size, 0, Math.PI*2); pongCtx.fill();
        }

        function pongGameLoop() { updatePong(); drawPong(); requestAnimationFrame(pongGameLoop); }

        // ==================== КРЕСТИКИ-НОЛИКИ ====================
        function renderTttBoard() {
            const boardEl = document.getElementById('tttBoard');
            boardEl.innerHTML = '';
            tttBoard.forEach((cell, i) => {
                const cellDiv = document.createElement('div');
                cellDiv.className = `ttt-cell ${cell==='X'?'x':cell==='O'?'o':''}`;
                cellDiv.textContent = cell;
                cellDiv.onclick = () => tttMakeMove(i);
                boardEl.appendChild(cellDiv);
            });
            document.getElementById('tttStatus').textContent = tttGameActive ? 'Ваш ход' : 'Игра окончена';
        }

        function tttMakeMove(index) {
            if (!tttGameActive || tttBoard[index]!=='') return;
            tttBoard[index] = 'X';
            if (tttCheckWin('X')) { tttEndGame('win'); return; }
            if (tttBoard.every(c => c!=='')) { tttEndGame('draw'); return; }
            setTimeout(tttBotMove, 300);
        }

        function tttBotMove() {
            if (!tttGameActive) return;
            let move = -1;
            const empty = tttBoard.reduce((acc,cell,i) => cell===''?[...acc,i]:acc, []);
            if (tttDifficulty === 'easy') move = empty[Math.floor(Math.random()*empty.length)];
            else {
                for (let i of empty) { tttBoard[i]='O'; if(tttCheckWin('O')) { tttBoard[i]=''; move=i; break; } tttBoard[i]=''; }
                if (move===-1) for (let i of empty) { tttBoard[i]='X'; if(tttCheckWin('X')) { tttBoard[i]=''; move=i; break; } tttBoard[i]=''; }
                if (move===-1) move = empty[Math.floor(Math.random()*empty.length)];
            }
            if (move!==-1) {
                tttBoard[move] = 'O';
                if (tttCheckWin('O')) { tttEndGame('lose'); return; }
                if (tttBoard.every(c => c!=='')) { tttEndGame('draw'); return; }
            }
            renderTttBoard();
        }

        function tttCheckWin(player) {
            const win = [[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]];
            return win.some(combo => combo.every(i => tttBoard[i]===player));
        }

        function tttEndGame(result) {
            tttGameActive = false;
            if (result === 'win') {
                tttWins++;
                if (tttBoard.filter(c=>c==='O').length===0) tttPerfectGames++;
                if (tttDifficulty === 'hard') tttHardWins++;
                tttNoLossStreak++;
                showNotification('🎉 Вы победили!', 'success');
            } else if (result === 'lose') { 
                tttLosses++; 
                tttNoLossStreak=0;
                showNotification('😵 Бот победил...', 'error');
            } else { 
                tttDraws++; 
                tttNoLossStreak=0;
                showNotification('🤝 Ничья!', 'info');
            }
            document.getElementById('tttWins').textContent = tttWins;
            document.getElementById('tttDraws').textContent = tttDraws;
            document.getElementById('tttLosses').textContent = tttLosses;
            checkAchievements(); saveGame(); renderTttBoard();
        }

        function resetTttGame() {
            tttBoard = ['','','','','','','','','']; tttGameActive = true; renderTttBoard();
            showNotification('🔄 Новая игра', 'info');
        }

        function setTttDifficulty(level) {
            tttDifficulty = level;
            document.querySelectorAll('#tab-tictactoe .difficulty-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            resetTttGame();
        }

        // ==================== POSTGRESQL ДЕМО ====================
        function simulatePostgresSave() {
            showNotification('💾 Данные сохранены в PostgreSQL (демо)', 'success');
            document.getElementById('postgresResult').innerHTML = `
                <i class="fas fa-check-circle" style="color:#4CAF50;"></i> 
                INSERT INTO users (username, score, click_power, auto_clickers, casino_balance, casino_vip, casino_double_chance) 
                VALUES ('TALLIN', ${score}, ${clickPower}, ${autoClickers}, ${casinoBalance}, ${casinoVIPLevel}, ${casinoDoubleWinChance});<br>
                <span style="color:#4CAF50;">✅ Запрос выполнен успешно! (демо)</span>
            `;
        }

        // ==================== REST API ДЕМО ====================
        function simulateRestApiGet() {
            const mockUsers = [
                { id: 1, username: 'TALLIN', score: score, click_power: clickPower, auto_clickers: autoClickers, casino_balance: casinoBalance, casino_vip: casinoVIPLevel, casino_double: casinoDoubleWinChance },
                { id: 2, username: 'Player2', score: 500, click_power: 3, auto_clickers: 1, casino_balance: 200, casino_vip: 0, casino_double: 0 },
                { id: 3, username: 'Player3', score: 250, click_power: 2, auto_clickers: 0, casino_balance: 50, casino_vip: 1, casino_double: 10 }
            ];
            
            document.getElementById('restapiResult').innerHTML = `
                <i class="fas fa-check-circle" style="color:#4CAF50;"></i> 
                GET /api/users → 200 OK<br>
                <span style="color:#4CAF50;">Ответ:</span><br>
                ${JSON.stringify(mockUsers, null, 2)}
            `;
            showNotification('📡 GET запрос выполнен (демо)', 'success');
        }

        function simulateRestApiPost() {
            const newUser = {
                id: 4,
                username: 'TALLIN',
                score: score,
                click_power: clickPower,
                auto_clickers: autoClickers,
                casino_balance: casinoBalance,
                casino_vip: casinoVIPLevel,
                casino_double: casinoDoubleWinChance,
                created_at: new Date().toISOString()
            };
            
            document.getElementById('restapiResult').innerHTML = `
                <i class="fas fa-check-circle" style="color:#4CAF50;"></i> 
                POST /api/users → 201 Created<br>
                <span style="color:#4CAF50;">Новый игрок:</span><br>
                ${JSON.stringify(newUser, null, 2)}
            `;
            showNotification('📡 POST запрос выполнен (демо)', 'success');
        }

        // ==================== БИО И ФОТО ====================
        function changeBanner() {
            const input = document.createElement('input');
            input.type='file'; input.accept='image/*';
            input.onchange = e => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onload = event => { 
                    document.getElementById('profileBanner').src = event.target.result; 
                    localStorage.setItem(BANNER_KEY, event.target.result);
                    showNotification('🖼️ Баннер обновлён', 'success');
                };
                reader.readAsDataURL(file);
            }; input.click();
        }

        function changeAvatar() {
            const input = document.createElement('input');
            input.type='file'; input.accept='image/*';
            input.onchange = e => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onload = event => { 
                    document.getElementById('profileAvatar').src = event.target.result; 
                    localStorage.setItem(AVATAR_KEY, event.target.result);
                    showNotification('🖼️ Аватар обновлён', 'success');
                };
                reader.readAsDataURL(file);
            }; input.click();
        }

        function loadBio() {
            const saved = localStorage.getItem(BIO_KEY);
            if (saved) {
                const bio = JSON.parse(saved);
                document.getElementById('profileName').textContent = bio.name || 'TALLIN';
                document.getElementById('profileBio').textContent = bio.bio || 'Лицей • Биология • Лоутаб';
                document.getElementById('profileStatus').textContent = bio.status || 'online';
            }
        }

        function saveBio() {
            const name = document.getElementById('editName').value;
            const bio = document.getElementById('editBio').value;
            const status = document.getElementById('editStatus').value;
            localStorage.setItem(BIO_KEY, JSON.stringify({ name, bio, status }));
            document.getElementById('profileName').textContent = name;
            document.getElementById('profileBio').textContent = bio;
            document.getElementById('profileStatus').textContent = status;
            closeBioEditor();
            showNotification('✅ Профиль сохранён', 'success');
        }

        function openBioEditor() { document.getElementById('bioModal').classList.add('active'); }
        function closeBioEditor() { document.getElementById('bioModal').classList.remove('active'); }

        // ==================== ПЛЕЙЛИСТ ====================
        function loadPlaylist() {
            const container = document.getElementById('tracksContainer');
            container.innerHTML = playlist.map((t,i) => `<div class="track-item" onclick="playTrack(${i})"><span>${t.title} - ${t.artist}</span></div>`).join('');
        }

        function playTrack(index) {
            currentTrack = index; 
            audio.src = playlist[index].src; 
            audio.play().catch(e => console.log('Не удалось воспроизвести трек:', e));
            document.getElementById('playPauseBtn').innerHTML = '<i class="fas fa-pause"></i>';
            document.getElementById('nowTitle').textContent = playlist[index].title;
            document.getElementById('nowArtist').textContent = playlist[index].artist;
        }

        document.getElementById('prevBtn').onclick = () => playTrack((currentTrack-1+playlist.length)%playlist.length);
        document.getElementById('nextBtn').onclick = () => playTrack((currentTrack+1)%playlist.length);
        document.getElementById('playPauseBtn').onclick = () => {
            if (audio.paused) { 
                audio.play().catch(e => console.log('Не удалось воспроизвести:', e));
                playPauseBtn.innerHTML='<i class="fas fa-pause"></i>'; 
            }
            else { 
                audio.pause(); 
                playPauseBtn.innerHTML='<i class="fas fa-play"></i>'; 
            }
        };

        // ==================== ПОДДЕРЖКА, ТЕМЫ, ТАБЫ, ГОРЯЧИЕ КЛАВИШИ ====================
        function openSupport() { 
            window.open('https://t.me/siteheroilik_bot', '_blank');
            showNotification('📨 Открываю чат с поддержкой', 'info');
        }

        function shareToTelegram() { 
            if(tg) tg.sendData(JSON.stringify({score, casinoBalance, casinoVIPLevel}));
            showNotification('📤 Данные отправлены в Telegram', 'success');
        }

        function changeTheme(theme) { 
            document.body.className = ''; 
            document.body.classList.add(`theme-${theme}`); 
            localStorage.setItem(THEME_KEY, theme);
            document.querySelectorAll('.theme-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(`theme${theme.charAt(0).toUpperCase() + theme.slice(1)}`).classList.add('active');
            showNotification(`🎨 Тема: ${theme}`, 'info');
        }

        function toggleTelegramTheme() { if(tg) changeTheme(tg.colorScheme==='dark'?'night':'sunset'); }

        function switchTab(tab) {
            document.querySelectorAll('.tab-content').forEach(t=>t.classList.remove('active'));
            document.querySelectorAll('.tab-button').forEach(b=>b.classList.remove('active'));
            document.getElementById(`tab-${tab}`).classList.add('active');
            event.target.closest('.tab-button').classList.add('active');
            if (tab==='pong') setTimeout(initPong,100);
            if (tab==='tictactoe') renderTttBoard();
            if (tab==='casino') {
                updateCasinoUI();
                updateShopUI();
                checkDailyBonus();
            }
        }

        // Горячие клавиши
        document.addEventListener('keydown', (e) => {
            const key = e.key.toLowerCase();
            if (key === '1') buyUpgrade('click');
            else if (key === '2') buyUpgrade('auto');
            else if (key === '3') buyUpgrade('super');
            else if (key === '4') buyUpgrade('golden');
            else if (key === 'c') switchTab('casino');
            else if (key === 'm') {
                switchTab('casino');
                document.getElementById('casinoShop').scrollIntoView({ behavior: 'smooth' });
            }
            else if (key === 'p') switchTab('playlist');
            else if (key === 'b') claimDailyBonus();
        });

        // ==================== СБРОС ====================
        function resetGame() {
            if (confirm('⚠️ ТОЧНО СБРОСИТЬ ВЕСЬ ПРОГРЕСС? Это удалит все баллы, улучшения и достижения!')) {
                score = 0; clickPower = 1; autoClickers = 0; totalClicks = 0; criticalHits = 0;
                clickPrice = 10; autoPrice = 50; superPrice = 100; superClickBought = false; goldenPrice = 500; goldenClickBought = false;
                pongWins = 0; pongPerfectGames = 0; pongAces = 0; pongComebacks = 0; pongImpossibleWins = 0;
                tttWins = 0; tttDraws = 0; tttLosses = 0; tttPerfectGames = 0; tttHardWins = 0; tttNoLossStreak = 0;
                casinoBalance = 100; casinoTotalBets = 0; casinoWins = 0; casinoJackpots = 0;
                casinoShopPurchases = 0; casinoHighRoller = 0; casinoLuckyWin = 0; casinoDailyBonuses = 0;
                casinoVIP = 0; casinoDoubleWins = 0; casinoLuckBonus = 0; casinoMultiplier = 1;
                casinoInsurance = false; casinoFreeSpins = 0; casinoVIPLevel = 0; casinoDoubleWinChance = 0;
                casinoJackpotBonus = 0;
                allAchievements.forEach(a => a.unlocked = false);
                if (autoInterval) clearInterval(autoInterval);
                deactivateSuperBonus();
                localStorage.removeItem(SAVE_KEY);
                localStorage.removeItem(DAILY_BONUS_KEY);
                updateUI();
                updateCasinoUI();
                updateShopUI();
                showNotification('💥 Прогресс сброшен!', 'warning');
            }
        }

        function deactivateSuperBonus() { superBonusActive = false; }

        // ==================== ЗАГРУЗКА ====================
        window.onload = () => {
            loadGame(); loadBio();
            const banner = localStorage.getItem(BANNER_KEY); if(banner) document.getElementById('profileBanner').src=banner;
            const avatar = localStorage.getItem(AVATAR_KEY); if(avatar) document.getElementById('profileAvatar').src=avatar;
            const theme = localStorage.getItem(THEME_KEY); if(theme) changeTheme(theme);
            loadPlaylist(); renderTttBoard();
            updateCasinoUI();
            updateShopUI();
            checkDailyBonus();
            setInterval(()=>saveGame(),10000);
            
            clickSounds.forEach(sound => { sound.load(); });
            purchaseSound.load();
            achievementSound.load();
            casinoWinSound.load();

            // Сброс кулдауна при загрузке
            if (casinoCooldownTimer) {
                clearInterval(casinoCooldownTimer);
                casinoCooldown = false;
                const spinButton = document.getElementById('spinButton');
                spinButton.disabled = false;
                spinButton.style.opacity = '1';
                spinButton.innerHTML = '<i class="fas fa-play"></i> Крутить';
            }
        };
    </script>
</body>
</html>

