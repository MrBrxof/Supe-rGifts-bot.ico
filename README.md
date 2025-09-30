<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WebApp</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        body {
            background-color: #2c2f33;
            color: #ffffff;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        .container {
            flex: 1;
            padding: 20px;
        }
        .footer {
            position: fixed;
            bottom: 0;
            width: 100%;
            background-color: #23272a;
            padding: 10px 0;
            display: flex;
            justify-content: space-around;
        }
        .footer button {
            background-color: #7289da;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        .footer button:hover {
            background-color: #677bc4;
        }
        .profile {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }
        .profile img {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            margin-right: 10px;
        }
        .inventory {
            margin-top: 20px;
        }
        .inventory button {
            background-color: #43b581;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }
        .inventory button:hover {
            background-color: #3ca374;
        }
    </style>
</head>
<body>
    <div class="container" id="main-content">
        <!-- Основной контент -->
    </div>
    <div class="footer">
        <button onclick="showSection('pvp')">PVP</button>
        <button onclick="showSection('shop')">Магазин</button>
        <button onclick="showSection('account')">Аккаунт</button>
    </div>

    <script>
        const TelegramWebApp = window.Telegram.WebApp;
        TelegramWebApp.ready();

        function showSection(section) {
            const mainContent = document.getElementById('main-content');
            if (section === 'account') {
                const user = TelegramWebApp.initDataUnsafe.user;
                mainContent.innerHTML = `
                    <div class="profile">
                        <img src="${user.photo_url || 'https://via.placeholder.com/50'}" alt="Profile">
                        <div>
                            <h3>${user.username || user.first_name}</h3>
                            <p>ID: ${user.id}</p>
                        </div>
                    </div>
                    <div class="inventory">
                        <h2>Инвентарь</h2>
                        <p>Пусто</p>
                        <button onclick="replenishInventory()">Пополнить Инвентарь</button>
                    </div>
                `;
            } else if (section === 'pvp') {
                mainContent.innerHTML = '<h2>PVP</h2><p>Скоро будет доступно!</p>';
            } else if (section === 'shop') {
                mainContent.innerHTML = '<h2>Магазин</h2><p>Скоро будет доступно!</p>';
            }
        }

        function replenishInventory() {
            alert('Функция пополнения инвентаря пока не реализована.');
        }

        // Инициализация начального экрана
        showSection('account');
    </script>
</body>
</html>
