<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Signal</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            font-family: Arial, sans-serif;
            background-color: #0a0a1a;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
        }
        
        /* Стили для главного меню */
        .menu-container {
            width: 100%;
            max-width: 500px;
            background-color: #1a1a2e;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 0 20px rgba(0, 100, 255, 0.3);
            text-align: center;
        }
        
        .menu-title {
            font-size: 28px;
            margin-bottom: 30px;
            color: #4cc9f0;
            text-shadow: 0 0 10px rgba(76, 201, 240, 0.5);
        }
        
        .game-btn {
            background: linear-gradient(to right, #f72585, #7209b7);
            color: white;
            border: none;
            border-radius: 10px;
            padding: 15px;
            width: 100%;
            font-size: 18px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .game-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(114, 9, 183, 0.4);
        }
        
        .game-btn i {
            font-size: 24px;
        }
        
        .back-btn {
            background: #333;
            color: white;
            border: none;
            border-radius: 8px;
            padding: 10px 15px;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 20px;
        }
        
        .back-btn:hover {
            background: #444;
        }
        
        /* Общие стили для игр */
        .game-container {
            width: 100%;
            max-width: 500px;
            background-color: #1a1a2e;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 0 20px rgba(0, 100, 255, 0.3);
            position: relative;
            display: none;
        }
        
        /* Стили для игры с ракетой */
        .rocket-game .game-area {
            width: 100%;
            height: 60vh;
            max-height: 500px;
            min-height: 300px;
            background: linear-gradient(to bottom, #000428, #004e92);
            border-radius: 10px;
            position: relative;
            overflow: hidden;
            margin-bottom: 15px;
            border: 1px solid #1e3a8a;
        }
        
        .rocket {
            position: absolute;
            bottom: 5%;
            left: 5%;
            font-size: 2.5em;
            transition: all 0.05s linear;
            filter: drop-shadow(0 0 5px #ff9e00);
            transform: rotate(45deg);
            z-index: 2;
        }
        
        .multiplier {
            position: absolute;
            right: 5%;
            top: 5%;
            font-size: 1.5em;
            font-weight: bold;
            color: #f72585;
            text-shadow: 0 0 5px rgba(247, 37, 133, 0.7);
            background-color: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 5px;
            z-index: 2;
        }
        
        .controls {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .signal-btn {
            background: linear-gradient(to right, #f72585, #7209b7);
            color: white;
            border: none;
            border-radius: 8px;
            padding: 15px 0;
            width: 100%;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .onewin-btn {
            background: linear-gradient(to right, #FFD700, #FFA500);
            color: black;
            border: none;
            border-radius: 8px;
            padding: 15px 0;
            width: 100%;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
            text-decoration: none;
            display: block;
            margin-top: 5px;
        }
        
        .signal-btn:hover, .onewin-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(114, 9, 183, 0.4);
        }
        
        .signal-btn:disabled {
            background: #6c757d;
            cursor: not-allowed;
        }
        
        .stats {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            font-size: 0.9em;
            color: #adb5bd;
        }
        
        .flames {
            position: absolute;
            font-size: 1.8em;
            color: #ff9e00;
            opacity: 0;
            transition: opacity 0.1s;
            transform: rotate(225deg);
            pointer-events: none;
            z-index: 1;
        }
        
        .stars {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        
        .star {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            animation: twinkle 2s infinite alternate;
        }
        
        @keyframes twinkle {
            0% { opacity: 0.2; }
            100% { opacity: 1; }
        }
        
        .explosion {
            position: absolute;
            font-size: 4em;
            opacity: 0;
            z-index: 10;
            pointer-events: none;
            transition: opacity 0.2s;
        }
        
        /* Стили для игры с бомбами */
        :root {
            --cell-size: clamp(40px, 15vw, 70px);
            --gap-size: clamp(5px, 2vw, 10px);
            --button-padding: clamp(8px, 3vw, 15px) clamp(15px, 5vw, 30px);
            --font-size: clamp(12px, 3vw, 16px);
            --promo-font-size: clamp(14px, 3.5vw, 18px);
            --bomb-font-size: clamp(20px, 6vw, 30px);
            --dot-size: clamp(8px, 3vw, 15px);
        }
        
        .bombs-game .header {
            margin-bottom: clamp(15px, 5vw, 30px);
            text-align: center;
            width: 100%;
        }
        
        .promo-btn {
            padding: var(--button-padding);
            font-size: var(--promo-font-size);
            background: linear-gradient(145deg, #ff5722, #e64a19);
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.3);
            text-decoration: none;
            display: inline-block;
            animation: pulse 2s infinite;
            margin-bottom: clamp(10px, 3vw, 15px);
            width: auto;
            max-width: 100%;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .bombs-game .game-board {
            display: grid;
            grid-template-columns: repeat(5, var(--cell-size));
            grid-template-rows: repeat(5, var(--cell-size));
            gap: var(--gap-size);
            width: 100%;
            justify-content: center;
        }
        
        .cell {
            width: 100%;
            height: 100%;
            border-radius: 12px;
            background: linear-gradient(145deg, #4fc3f7, #29b6f6);
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: var(--bomb-font-size);
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            user-select: none;
        }
        
        .cell:hover, .cell:active {
            transform: scale(1.05);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
        }
        
        .cell.bomb {
            background: linear-gradient(145deg, #f44336, #b71c1c);
            animation: bombAppear 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            box-shadow: 0 0 20px #ff5252;
        }
        
        @keyframes bombAppear {
            0% { transform: scale(0) rotate(180deg); opacity: 0; }
            50% { transform: scale(1.2) rotate(-10deg); }
            70% { transform: scale(0.9) rotate(5deg); }
            100% { transform: scale(1) rotate(0deg); opacity: 1; }
        }
        
        .splash {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }
        
        .dot {
            position: absolute;
            width: var(--dot-size);
            height: var(--dot-size);
            border-radius: 50%;
            background: #0d47a1;
            opacity: 0;
            animation: splash 1s ease-out forwards;
            will-change: transform, opacity;
        }
        
        @keyframes splash {
            0% {
                transform: translate(0, 0) scale(0);
                opacity: 1;
            }
            100% {
                transform: translate(var(--tx), var(--ty)) scale(1);
                opacity: 0;
            }
        }
        
        .bombs-game .controls {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: clamp(10px, 3vw, 20px);
            margin-top: clamp(10px, 3vw, 20px);
            width: 100%;
        }
        
        .count-buttons {
            display: flex;
            gap: clamp(5px, 2vw, 10px);
            flex-wrap: wrap;
            justify-content: center;
            width: 100%;
        }
        
        .bombs-game button {
            padding: var(--button-padding);
            font-size: var(--font-size);
            border: none;
            border-radius: 8px;
            background: linear-gradient(145deg, #2196f3, #0d47a1);
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
            min-width: clamp(40px, 15vw, 60px);
            flex: 1 1 auto;
            user-select: none;
        }
        
        .bombs-game button:hover, .bombs-game button:active {
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
        }
        
        .bombs-game button:active {
            transform: translateY(1px);
        }
        
        .active {
            background: linear-gradient(145deg, #4caf50, #2e7d32);
            transform: scale(1.05);
        }
        
        .bomb-icon {
            animation: bombFloat 2s ease-in-out infinite;
            display: inline-block;
        }
        
        @keyframes bombFloat {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-5px) rotate(5deg); }
        }
        
        @media (orientation: portrait) and (max-height: 700px) {
            :root {
                --cell-size: clamp(35px, 12vh, 60px);
                --font-size: clamp(10px, 2.5vw, 14px);
            }
            
            .bombs-game .game-container {
                gap: clamp(10px, 3vh, 20px);
            }
            
            .rocket-game .game-area {
                height: 50vh;
                min-height: 250px;
            }
        }
        
        @media (max-width: 500px) {
            :root {
                --cell-size: clamp(35px, 14vw, 50px);
                --font-size: clamp(10px, 3vw, 14px);
            }
            
            .count-buttons button {
                min-width: clamp(35px, 12vw, 50px);
                padding: clamp(5px, 2vw, 10px) clamp(8px, 3vw, 15px);
            }
            
            .menu-title {
                font-size: 24px;
                margin-bottom: 20px;
            }
            
            .game-btn {
                padding: 12px;
                font-size: 16px;
            }
            
            .menu-container, .game-container {
                padding: 20px;
            }
        }
        
        @media (max-width: 400px) {
            .rocket-game .game-area {
                height: 45vh;
                min-height: 200px;
            }
        }
    </style>
</head>
<body>
    <!-- Главное меню -->
    <div class="menu-container" id="menuContainer">
        <h1 class="menu-title">Game Signal</h1>
        <button class="game-btn" id="rocketGameBtn">
            <span>🚀</span> Lucky Jet
        </button>
        <button class="game-btn" id="bombsGameBtn">
            <span>💣</span> Mines
        </button>
    </div>
    
    <!-- Игра с ракетой -->
    <div class="game-container rocket-game" id="rocketGame">
        <div class="game-area">
            <div class="stars" id="stars"></div>
            <div class="multiplier" id="multiplier">1.00x</div>
            <div class="rocket" id="rocket">🚀</div>
            <div class="flames" id="flames">🔥</div>
            <div class="explosion" id="explosion">💥</div>
        </div>
        
        <div class="controls">
            <button class="signal-btn" id="signalBtn">SIGNAL</button>
            <a href="https://1wilib.life/v3/lucky-jet-updated?p=f3lx" target="_blank" class="onewin-btn">GO 1WIN 🚀</a>
        </div>
        
        <div class="stats">
            <div>Total: <span id="totalFlights">0</span></div>
            <div>Last: <span id="lastFlight">0.00x</span></div>
        </div>
        
        <button class="back-btn" id="backBtn1">Back to Menu</button>
    </div>
    
    <!-- Игра с бомбами -->
    <div class="game-container bombs-game" id="bombsGame">
        <div class="game-board" id="gameBoard"></div>
        <div class="controls">
            <div class="count-buttons">
                <button id="count1" class="active">1</button>
                <button id="count3">3</button>
                <button id="count5">5</button>
                <button id="count7">7</button>
            </div>
            <button id="goButton">GO TO SIGNAL</button>
            <a href="https://1wilib.life/v3/lucky-jet-updated?p=f3lx" target="_blank" class="onewin-btn">GO 1WIN 🚀</a>
        </div>
        
        <button class="back-btn" id="backBtn2">Back to Menu</button>
    </div>

    <script>
        // Управление переключением между меню и играми
        const menuContainer = document.getElementById('menuContainer');
        const rocketGameBtn = document.getElementById('rocketGameBtn');
        const bombsGameBtn = document.getElementById('bombsGameBtn');
        const rocketGame = document.getElementById('rocketGame');
        const bombsGame = document.getElementById('bombsGame');
        const backBtn1 = document.getElementById('backBtn1');
        const backBtn2 = document.getElementById('backBtn2');
        
        rocketGameBtn.addEventListener('click', () => {
            menuContainer.style.display = 'none';
            rocketGame.style.display = 'block';
        });
        
        bombsGameBtn.addEventListener('click', () => {
            menuContainer.style.display = 'none';
            bombsGame.style.display = 'block';
            initializeBombsGame();
        });
        
        backBtn1.addEventListener('click', () => {
            rocketGame.style.display = 'none';
            menuContainer.style.display = 'block';
        });
        
        backBtn2.addEventListener('click', () => {
            bombsGame.style.display = 'none';
            menuContainer.style.display = 'block';
        });
        
        // Код для игры с ракетой
        const rocket = document.getElementById('rocket');
        const flames = document.getElementById('flames');
        const explosion = document.getElementById('explosion');
        const multiplierDisplay = document.getElementById('multiplier');
        const signalBtn = document.getElementById('signalBtn');
        const totalFlightsDisplay = document.getElementById('totalFlights');
        const lastFlightDisplay = document.getElementById('lastFlight');
        const starsContainer = document.getElementById('stars');
        const gameArea = document.querySelector('.rocket-game .game-area');
        
        let isFlying = false;
        let currentMultiplier = 1.0;
        let totalFlights = 0;
        let flightInterval;
        let crashPoint;
        let maxRight, maxTop;
        
        // Создаем звезды на фоне
        function createStars() {
            starsContainer.innerHTML = '';
            const starCount = Math.floor(gameArea.offsetWidth * gameArea.offsetHeight / 200);
            
            for (let i = 0; i < starCount; i++) {
                const star = document.createElement('div');
                star.classList.add('star');
                star.style.width = `${Math.random() * 3 + 1}px`;
                star.style.height = star.style.width;
                star.style.left = `${Math.random() * 100}%`;
                star.style.top = `${Math.random() * 100}%`;
                star.style.animationDelay = `${Math.random() * 2}s`;
                starsContainer.appendChild(star);
            }
        }
        
        // Генерируем случайную точку краша (3.00x - 10.00x)
        function generateCrashPoint() {
            // 70% chance for 3.00x-5.00x, 30% chance for 5.00x-10.00x
            if (Math.random() < 0.7) {
                crashPoint = parseFloat((3 + Math.random() * 2).toFixed(2));
            } else {
                crashPoint = parseFloat((5 + Math.random() * 5).toFixed(2));
            }
            console.log(`Crash point: ${crashPoint}x`);
        }
        
        // Рассчитываем максимальные координаты
        function calculateMaxCoordinates() {
            const rocketWidth = 40; // примерная ширина ракеты
            const rocketHeight = 40; // примерная высота ракеты
            maxRight = gameArea.offsetWidth - rocketWidth - 20;
            maxTop = gameArea.offsetHeight - rocketHeight - 20;
        }
        
        // Запуск ракеты
        function launchRocket() {
            if (isFlying) return;
            
            isFlying = true;
            currentMultiplier = 1.0;
            totalFlights++;
            totalFlightsDisplay.textContent = totalFlights;
            signalBtn.disabled = true;
            flames.style.opacity = '1';
            explosion.style.opacity = '0';
            
            generateCrashPoint();
            calculateMaxCoordinates();
            
            // Сброс позиции ракеты
            rocket.style.bottom = '5%';
            rocket.style.left = '5%';
            rocket.style.opacity = '1';
            rocket.style.transform = 'rotate(45deg)';
            
            const startTime = Date.now();
            const duration = 3000 + Math.random() * 7000; // 3-10 секунд
            
            flightInterval = setInterval(() => {
                const elapsed = Date.now() - startTime;
                const progress = Math.min(elapsed / duration, 1);
                
                // Обновляем множитель
                currentMultiplier = 1 + (crashPoint - 1) * progress;
                multiplierDisplay.textContent = currentMultiplier.toFixed(2) + 'x';
                
                // Двигаем ракету по диагонали
                const bottom = 5 + 75 * progress;
                const left = 5 + 75 * progress;
                
                rocket.style.bottom = `${bottom}%`;
                rocket.style.left = `${left}%`;
                
                // Позиционируем пламя
                flames.style.bottom = `${bottom - 5}%`;
                flames.style.left = `${left - 5}%`;
                
                // Проверка на завершение полета
                if (progress >= 1) {
                    crashRocket();
                }
            }, 30); // Интервал обновления
        }
        
        // Краш ракеты
        function crashRocket() {
            clearInterval(flightInterval);
            
            // Позиция взрыва
            explosion.style.bottom = rocket.style.bottom;
            explosion.style.left = rocket.style.left;
            
            // Показываем взрыв
            explosion.style.opacity = '1';
            rocket.style.opacity = '0';
            flames.style.opacity = '0';
            
            setTimeout(() => {
                isFlying = false;
                lastFlightDisplay.textContent = currentMultiplier.toFixed(2) + 'x';
                signalBtn.disabled = false;
                explosion.style.opacity = '0';
            }, 1000);
        }
        
        // Инициализация игры с ракетой
        window.addEventListener('resize', createStars);
        createStars();
        signalBtn.addEventListener('click', launchRocket);
        
        // Код для игры с бомбами
        function initializeBombsGame() {
            const boardSize = 5;
            let selectedBombCount = 1;
            let board = [];
            
            // Инициализация игрового поля
            function initializeBoard() {
                const gameBoard = document.getElementById('gameBoard');
                gameBoard.innerHTML = '';
                board = [];
                
                for (let i = 0; i < boardSize; i++) {
                    board[i] = [];
                    for (let j = 0; j < boardSize; j++) {
                        board[i][j] = { hasBomb: false };
                        
                        const cell = document.createElement('div');
                        cell.className = 'cell';
                        cell.dataset.row = i;
                        cell.dataset.col = j;
                        
                        // Обработчик для touch устройств
                        cell.addEventListener('click', handleCellClick);
                        cell.addEventListener('touchend', handleCellClick, { passive: true });
                        
                        gameBoard.appendChild(cell);
                    }
                }
            }
            
            function handleCellClick(e) {
                e.preventDefault();
                const cell = e.currentTarget;
                const row = parseInt(cell.dataset.row);
                const col = parseInt(cell.dataset.col);
                
                if (!board[row][col].hasBomb) {
                    createSplashEffect(cell);
                }
            }
            
            // Создание эффекта брызг
            function createSplashEffect(element) {
                const splash = document.createElement('div');
                splash.className = 'splash';
                
                // Оптимизация: меньше точек на мобильных
                const dotsCount = window.innerWidth < 500 ? 10 : 15;
                
                for (let i = 0; i < dotsCount; i++) {
                    const dot = document.createElement('div');
                    dot.className = 'dot';
                    
                    const angle = Math.random() * Math.PI * 2;
                    const distance = 10 + Math.random() * 30;
                    const tx = Math.cos(angle) * distance;
                    const ty = Math.sin(angle) * distance;
                    
                    dot.style.setProperty('--tx', `${tx}px`);
                    dot.style.setProperty('--ty', `${ty}px`);
                    dot.style.left = '50%';
                    dot.style.top = '50%';
                    dot.style.backgroundColor = `hsl(${200 + Math.random() * 40}, 80%, 40%)`;
                    
                    splash.appendChild(dot);
                }
                
                element.appendChild(splash);
                
                // Автоматическая очистка
                setTimeout(() => {
                    element.removeChild(splash);
                }, 1000);
            }
            
            // Размещение бомб случайным образом
            function placeBombs() {
                // Очищаем предыдущие бомбы
                const cells = document.querySelectorAll('.cell');
                cells.forEach(cell => {
                    cell.classList.remove('bomb');
                    cell.innerHTML = '';
                });
                
                // Сбрасываем состояние доски
                for (let i = 0; i < boardSize; i++) {
                    for (let j = 0; j < boardSize; j++) {
                        board[i][j].hasBomb = false;
                    }
                }
                
                // Размещаем новые бомбы
                let bombsPlaced = 0;
                const maxBombs = selectedBombCount;
                
                while (bombsPlaced < maxBombs) {
                    const row = Math.floor(Math.random() * boardSize);
                    const col = Math.floor(Math.random() * boardSize);
                    
                    if (!board[row][col].hasBomb) {
                        board[row][col].hasBomb = true;
                        const cell = document.querySelector(`.cell[data-row="${row}"][data-col="${col}"]`);
                        cell.classList.add('bomb');
                        cell.innerHTML = '<span class="bomb-icon">💣</span>';
                        bombsPlaced++;
                    }
                }
            }
            
            // Инициализация кнопок выбора количества бомб
            function setupBombCountButtons() {
                const buttons = [
                    document.getElementById('count1'),
                    document.getElementById('count3'),
                    document.getElementById('count5'),
                    document.getElementById('count7')
                ];
                
                buttons.forEach(btn => {
                    btn.addEventListener('click', function() {
                        selectedBombCount = parseInt(this.textContent);
                        updateActiveButton();
                    });
                    
                    // Для touch устройств
                    btn.addEventListener('touchend', function(e) {
                        e.preventDefault();
                        selectedBombCount = parseInt(this.textContent);
                        updateActiveButton();
                    }, { passive: false });
                });
            }
            
            function updateActiveButton() {
                document.querySelectorAll('.count-buttons button').forEach(btn => {
                    btn.classList.remove('active');
                });
                
                document.getElementById(`count${selectedBombCount}`).classList.add('active');
            }
            
            // Инициализация кнопки "GO TO SIGNAL"
            function setupGoButton() {
                const btn = document.getElementById('goButton');
                
                btn.addEventListener('click', placeBombs);
                btn.addEventListener('touchend', function(e) {
                    e.preventDefault();
                    placeBombs();
                }, { passive: false });
            }
            
            // Инициализация игры
            initializeBoard();
            setupBombCountButtons();
            setupGoButton();
        }
    </script>
</body>
</html>
