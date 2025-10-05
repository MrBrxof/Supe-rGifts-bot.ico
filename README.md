<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Telegram WebApp</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root {
            --bg-primary: #2c2f33;
            --bg-secondary: #23272a;
            --accent-color: #7289da;
            --accent-hover: #677bc4;
            --success-color: #43b581;
            --success-hover: #3ca374;
            --text-color: #ffffff;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            background-color: var(--bg-primary);
            color: var(--text-color);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            line-height: 1.5;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            padding-bottom: 70px; /* Для футера */
        }
        
        .container {
            flex: 1;
            padding: 16px;
            max-width: 100%;
            overflow-x: hidden;
        }
        
        .footer {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            background-color: var(--bg-secondary);
            padding: 12px 0;
            display: flex;
            justify-content: space-around;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            z-index: 100;
        }
        
        .footer button {
            background-color: transparent;
            color: var(--text-color);
            border: none;
            padding: 8px 16px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.2s;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
        }
        
        .footer button.active {
            background-color: var(--accent-color);
        }
        
        .footer button:hover {
            background-color: var(--accent-hover);
        }
        
        .footer button i {
            font-size: 18px;
        }
        
        .profile {
            display: flex;
            align-items: center;
            margin-bottom: 24px;
            padding: 16px;
            background-color: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
        }
        
        .profile img {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            margin-right: 16px;
            object-fit: cover;
        }
        
        .profile-info h3 {
            font-size: 18px;
            margin-bottom: 4px;
        }
        
        .profile-info p {
            color: rgba(255, 255, 255, 0.7);
            font-size: 14px;
        }
        
        .inventory {
            margin-top: 24px;
        }
        
        .inventory h2 {
            margin-bottom: 16px;
            font-size: 20px;
        }
        
        .inventory-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }
        
        .inventory-item {
            background-color: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            aspect-ratio: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 8px;
        }
        
        .inventory-item.empty {
            border: 1px dashed rgba(255, 255, 255, 0.3);
        }
        
        .inventory-item i {
            font-size: 24px;
            margin-bottom: 4px;
        }
        
        .inventory-item span {
            font-size: 12px;
            text-align: center;
        }
        
        .btn {
            background-color: var(--success-color);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 500;
            width: 100%;
            transition: background-color 0.2s;
        }
        
        .btn:hover {
            background-color: var(--success-hover);
        }
        
        .section {
            display: none;
        }
        
        .section.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .section-header {
            margin-bottom: 20px;
            font-size: 24px;
        }
        
        .placeholder-content {
            text-align: center;
            padding: 40px 20px;
            color: rgba(255, 255, 255, 0.7);
        }
        
        .placeholder-content i {
            font-size: 48px;
            margin-bottom: 16px;
            opacity: 0.5;
        }
        
        /* Адаптивность для очень маленьких экранов */
        @media (max-width: 360px) {
            .profile {
                flex-direction: column;
                text-align: center;
            }
            
            .profile img {
                margin-right: 0;
                margin-bottom: 12px;
            }
            
            .inventory-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
    <!-- Иконки Font Awesome для улучшенного UI -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <div class="container">
        <!-- Секция Аккаунт -->
        <div id="account-section" class="section active">
            <div class="profile">
                <img id="user-photo" src="" alt="Profile">
                <div class="profile-info">
                    <h3 id="user-name">Загрузка...</h3>
                    <p id="user-id">ID: ...</p>
                </div>
            </div>
            <div class="inventory">
                <h2>Инвентарь</h2>
                <div class="inventory-grid">
                    <div class="inventory-item empty">
                        <i class="fas fa-box-open"></i>
                        <span>Пусто</span>
                    </div>
                    <div class="inventory-item empty">
                        <i class="fas fa-box-open"></i>
                        <span>Пусто</span>
                    </div>
                    <div class="inventory-item empty">
                        <i class="fas fa-box-open"></i>
                        <span>Пусто</span>
                    </div>
                    <div class="inventory-item empty">
                        <i class="fas fa-box-open"></i>
                        <span>Пусто</span>
                    </div>
                    <div class="inventory-item empty">
                        <i class="fas fa-box-open"></i>
                        <span>Пусто</span>
                    </div>
                    <div class="inventory-item empty">
                        <i class="fas fa-box-open"></i>
                        <span>Пусто</span>
                    </div>
                </div>
                <button class="btn" onclick="replenishInventory()">
                    <i class="fas fa-plus"></i> Пополнить Инвентарь
                </button>
            </div>
        </div>
        
        <!-- Секция PVP -->
        <div id="pvp-section" class="section">
            <h2 class="section-header">PVP Арена</h2>
            <div class="placeholder-content">
                <i class="fas fa-trophy"></i>
                <h3>Скоро будет доступно!</h3>
                <p>Готовьтесь к эпическим битвам с другими игроками</p>
            </div>
        </div>
        
        <!-- Секция Магазин -->
        <div id="shop-section" class="section">
            <h2 class="section-header">Магазин</h2>
            <div class="placeholder-content">
                <i class="fas fa-shopping-cart"></i>
                <h3>Скоро будет доступно!</h3>
                <p>Покупайте уникальные предметы и улучшения</p>
            </div>
        </div>
    </div>
    
    <div class="footer">
        <button id="pvp-btn" onclick="showSection('pvp')">
            <i class="fas fa-fist-raised"></i>
            <span>PVP</span>
        </button>
        <button id="shop-btn" onclick="showSection('shop')">
            <i class="fas fa-shopping-cart"></i>
            <span>Магазин</span>
        </button>
        <button id="account-btn" class="active" onclick="showSection('account')">
            <i class="fas fa-user"></i>
            <span>Аккаунт</span>
        </button>
    </div>

    <script>
        // Инициализация Telegram Web App
        const TelegramWebApp = window.Telegram.WebApp;
        
        // Расширяем приложение на весь экран
        TelegramWebApp.expand();
        
        // Устанавливаем цвет фона для Telegram
        TelegramWebApp.setBackgroundColor('#2c2f33');
        
        // Получаем данные пользователя
        const user = TelegramWebApp.initDataUnsafe.user;
        
        // Функция для отображения секции
        function showSection(section) {
            // Скрываем все секции
            document.querySelectorAll('.section').forEach(el => {
                el.classList.remove('active');
            });
            
            // Убираем активный класс со всех кнопок
            document.querySelectorAll('.footer button').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Показываем выбранную секцию и активируем кнопку
            document.getElementById(`${section}-section`).classList.add('active');
            document.getElementById(`${section}-btn`).classList.add('active');
        }
        
        // Функция пополнения инвентаря
        function replenishInventory() {
            // В реальном приложении здесь будет логика покупки
            TelegramWebApp.showPopup({
                title: 'Пополнение инвентаря',
                message: 'Функция пополнения инвентаря пока не реализована. В будущем здесь будет магазин предметов.',
                buttons: [{ type: 'ok' }]
            });
        }
        
        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            // Заполняем данные пользователя
            if (user) {
                document.getElementById('user-photo').src = user.photo_url || 'https://via.placeholder.com/60';
                document.getElementById('user-name').textContent = user.username || 
                    (user.first_name + (user.last_name ? ' ' + user.last_name : ''));
                document.getElementById('user-id').textContent = `ID: ${user.id}`;
            } else {
                // Если данные пользователя недоступны
                document.getElementById('user-name').textContent = 'Гость';
                document.getElementById('user-id').textContent = 'Войдите в Telegram';
            }
            
            // Инициализация начального экрана
            showSection('account');
        });
    </script>
</body>
</html>
