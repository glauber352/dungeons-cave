<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DUNGEONS CAVE</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
    <style>
        /* Estilos personalizados para o jogo */
        body {
            font-family: 'Press Start 2P', cursive; /* Aplica a fonte pixelada */
            background-color: #2c3e50; /* Cor de fundo escura */
            color: #ecf0f1; /* Cor do texto principal */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
            box-sizing: border-box;
        }

        .game-container {
            background-color: #34495e; /* Cor de fundo do contêiner principal */
            border: 4px solid #1a242f; /* Borda grossa para visual de jogo */
            border-radius: 8px; /* Cantos levemente arredondados */
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5); /* Sombra para profundidade */
            padding: 1.5rem;
            max-width: 900px;
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            position: relative; /* Para posicionar o modal */
        }

        h1, h2, h3 {
            color: #f1c40f; /* Cor de destaque para títulos */
            text-align: center;
            margin-bottom: 1rem;
            text-shadow: 2px 2px #000; /* Sombra de texto para efeito pixelado */
        }

        .pixel-button {
            background-color: #e74c3c; /* Cor de botão primária */
            color: white;
            padding: 0.75rem 1.5rem;
            border: 3px solid #c0392b; /* Borda para efeito 3D */
            border-radius: 5px;
            cursor: pointer;
            font-family: 'Press Start 2P', cursive;
            font-size: 0.9rem;
            transition: background-color 0.1s, transform 0.1s;
            box-shadow: 3px 3px #a52a22; /* Sombra para efeito de profundidade */
            display: block; /* Garante que o botão ocupe a largura total do pai flex */
            width: fit-content; /* Ajusta a largura ao conteúdo */
            margin: 0 auto; /* Centraliza o botão */
        }

        .pixel-button:hover {
            background-color: #c0392b;
            transform: translateY(1px); /* Efeito de clique */
            box-shadow: 2px 2px #a52a22;
        }

        .pixel-button:active {
            transform: translateY(3px);
            box-shadow: 0 0 #a52a22;
        }

        .panel {
            background-color: #2c3e50;
            border: 2px solid #1a242f;
            border-radius: 5px;
            padding: 1rem;
            margin-bottom: 1rem;
            box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.3); /* Sombra interna para efeito de profundidade */
        }

        .game-log {
            height: 150px;
            overflow-y: auto;
            background-color: #1a242f;
            border: 2px solid #000;
            padding: 0.75rem;
            font-size: 0.8rem;
            line-height: 1.4;
            color: #95a5a6; /* Cor do texto do log */
            border-radius: 5px;
            margin-top: 1rem;
            box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.5);
        }

        .game-log p {
            margin-bottom: 0.25rem;
        }

        .modal {
            display: none; /* Escondido por padrão */
            position: absolute; /* Posição absoluta dentro do contêiner do jogo */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #34495e;
            border: 4px solid #1a242f;
            border-radius: 8px;
            padding: 2rem;
            z-index: 100;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.7);
            text-align: center;
            max-width: 80%;
        }

        .modal-content button {
            margin-top: 1rem;
        }

        /* Estilos para tabelas */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
        }

        th, td {
            border: 1px solid #555;
            padding: 0.5rem;
            text-align: left;
            font-size: 0.85rem;
        }

        th {
            background-color: #4a627a;
            color: #f1c40f;
        }

        td {
            background-color: #3e5166;
        }

        /* Estilos para a tela inicial */
        #start-screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 1.5rem;
        }

        /* Estilos para a tela de jogo */
        #game-screen {
            display: none; /* Escondido por padrão */
            flex-direction: column;
            gap: 1rem;
        }

        .player-stats, .enemy-info {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem 1.5rem; /* Espaçamento entre itens e linhas */
            justify-content: center;
            font-size: 0.9rem;
        }

        .player-stats span, .enemy-info span {
            background-color: #1a242f;
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
        }

        .game-actions {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            justify-content: center;
            margin-top: 1rem;
        }

        .game-actions button {
            flex-grow: 1; /* Permite que os botões cresçam para preencher o espaço */
            max-width: 200px; /* Limita o tamanho máximo dos botões */
        }
    </style>
</head>
<body class="selection:bg-yellow-400 selection:text-black">
    <div class="game-container">
        <div id="game-modal" class="modal">
            <div class="modal-content">
                <p id="modal-message" class="text-lg mb-4"></p>
                <button id="modal-ok-button" class="pixel-button">OK</button>
            </div>
        </div>

        <div id="start-screen">
            <h1>DUNGEONS CAVE</h1>
            <p class="text-center italic mb-4">"Aventure-se nas profundezas... se tiver coragem."</p>

            <button id="start-game-button" class="pixel-button mt-6">Escolher Classe</button>
        </div>

        <div id="class-selection-modal" class="modal">
            <div class="modal-content">
                <h2>Escolha sua Classe</h2>
                <div id="class-options" class="flex flex-col gap-2">
                    <!-- As opções de classe serão inseridas aqui -->
                </div>
                <button id="modal-class-ok-button" class="pixel-button">Confirmar Classe</button>
            </div>
        </div>

        <div id="game-screen">
            <h2 class="text-2xl">Aventura em Andamento!</h2>

            <div class="panel">
                <h3 class="text-lg">Seu Herói</h3>
                <div id="player-stats" class="player-stats"></div>
            </div>

            <div class="panel">
                <h3 class="text-lg">Inimigo Atual</h3>
                <div id="enemy-info" class="enemy-info">
                    <span id="enemy-name">Nenhum Inimigo</span>
                    <span id="enemy-hp">HP: --</span>
                </div>
            </div>

            <div class="panel">
                <h3 class="text-lg">Ações</h3>
                <div id="game-actions" class="game-actions">
                    <button id="explore-button" class="pixel-button">Explorar</button>
                    <button id="attack-button" class="pixel-button" disabled>Atacar</button>
                    <button id="ability-button" class="pixel-button" disabled>Habilidade Especial</button>
                    <button id="flee-button" class="pixel-button" disabled>Fugir</button>
                </div>
            </div>

            <div class="panel">
                <h3 class="text-lg">Registro de Eventos</h3>
                <div id="game-log" class="game-log">
                    <p>Bem-vindo à Dungeons Cave! Escolha uma classe para começar.</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Dados do jogo
        const gameData = {
            classes: {
                knight: { name: "Cavaleiro", description: "Tanque, defesa alta, espada e escudo.", ability: "Escudo Imbatível", baseHp: 120, baseAttack: 15, baseDefense: 10 },
                rogue: { name: "Ladino", description: "Ágil, furtivo, dano crítico alto.", ability: "Desaparecer", baseHp: 80, baseAttack: 25, baseDefense: 5 },
                mage: { name: "Mago", description: "Mestre dos elementos, dano mágico em área.", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3 },
                cleric: { name: "Clérigo", description: "Suporte, cura, buffs e dano sagrado.", ability: "Bênção Divina", baseHp: 90, baseAttack: 20, baseDefense: 7 },
                barbarian: { name: "Bárbaro", description: "Selvagem, dano físico bruto, pouca defesa.", ability: "Fúria Insana", baseHp: 110, baseAttack: 28, baseDefense: 4 },
                hunter: { name: "Caçador", description: "Longo alcance, armadilhas e animais de suporte.", ability: "Falcão Espreitador", baseHp: 85, baseAttack: 22, baseDefense: 6 }
            },
            enemies: {
                goblin: { name: "Goblin", hp: 30, attack: 5 },
                giantBat: { name: "Morcego Gigante", hp: 25, attack: 6 },
                venomousSpider: { name: "Aranha Venenosa", hp: 35, attack: 7 },
                boss: { name: "Gorak, O Devorador", hp: 100, attack: 15 } // Boss
            },
            dungeons: [
                { name: "Caverna Sombria", levels: "1-5", boss: "Gorak, O Devorador", enemyTypes: ["goblin", "giantBat", "venomousSpider"] },
            ],
        };

        // Estado do jogo
        const gameState = {
            player: null,
            currentDungeon: null,
            currentEnemy: null,
            gameStarted: false,
            gameLog: [],
            items: [], // Array para itens coletados
        };

        // Referências aos elementos HTML
        const startScreen = document.getElementById('start-screen');
        const gameScreen = document.getElementById('game-screen');
        const startGameButton = document.getElementById('start-game-button');
        const classSelectionModal = document.getElementById('class-selection-modal');
        const classOptionsDiv = document.getElementById('class-options');
        const playerStatsDiv = document.getElementById('player-stats');
        const enemyInfoDiv = document.getElementById('enemy-info');
        const enemyNameSpan = document.getElementById('enemy-name');
        const enemyHpSpan = document.getElementById('enemy-hp');
        const gameLogDiv = document.getElementById('game-log');
        const exploreButton = document.getElementById('explore-button');
        const attackButton = document.getElementById('attack-button');
        const abilityButton = document.getElementById('ability-button');
        const fleeButton = document.getElementById('flee-button');

        // Referências do modal
        const gameModal = document.getElementById('game-modal');
        const modalMessage = document.getElementById('modal-message');
        const modalOkButton = document.getElementById('modal-ok-button');

        // Função para exibir o modal
        function showGameModal(message) {
            modalMessage.textContent = message;
            gameModal.style.display = 'block';
        }

        // Event listener para o botão OK do modal
        modalOkButton.addEventListener('click', () => {
            gameModal.style.display = 'none';
        });

        // Iniciar o jogo
        startGameButton.addEventListener('click', () => {
            showClassSelection();
        });

        function showClassSelection() {
            classOptionsDiv.innerHTML = ''; // Limpa as opções anteriores
            for (const key in gameData.classes) {
                const classButton = document.createElement('button');
                classButton.textContent = gameData.classes[key].name;
                classButton.className = 'pixel-button';
                classButton.onclick = () => selectClass(key);
                classOptionsDiv.appendChild(classButton);
            }
            classSelectionModal.style.display = 'block';
        }

        function selectClass(classKey) {
            gameState.player = classKey;
            gameState.gameStarted = true;
            gameState.currentDungeon = gameData.dungeons[0]; // Começa na primeira dungeon
            classSelectionModal.style.display = 'none';
            startGame();
        }

        function startGame() {
            startScreen.style.display = 'none';
            gameScreen.style.display = 'flex';
            updatePlayerStats();
            logEvent(`Você começou sua aventura como ${gameData.classes[gameState.player].name}!`);
        }

        function updatePlayerStats() {
            const playerClass = gameData.classes[gameState.player];
            playerStatsDiv.innerHTML = `
                <span>${playerClass.name}</span>
                <span>HP: ${playerClass.baseHp}</span>
                <span>Itens: ${gameState.items.length}</span>
            `;
        }

        exploreButton.addEventListener('click', () => {
            if (!gameState.gameStarted) return;
            const enemyType = getRandomEnemy();
            startBattle(enemyType);
        });

        function getRandomEnemy() {
            const enemies = gameState.currentDungeon.enemyTypes;
            const randomEnemyKey = enemies[Math.floor(Math.random() * enemies.length)];
            return gameData.enemies[randomEnemyKey];
        }

        function startBattle(enemy) {
            gameState.currentEnemy = enemy;
            enemyNameSpan.textContent = enemy.name;
            enemyHpSpan.textContent = `HP: ${enemy.hp}`;
            attackButton.disabled = false;
            abilityButton.disabled = false;
            fleeButton.disabled = false;
            logEvent(`Um ${enemy.name} apareceu!`);
        }

        attackButton.addEventListener('click', () => {
            if (!gameState.currentEnemy) return;
            const playerClass = gameData.classes[gameState.player];
            const damage = Math.max(0, playerClass.baseAttack - gameState.currentEnemy.attack);
            gameState.currentEnemy.hp -= damage;
            logEvent(`Você atacou ${gameState.currentEnemy.name} e causou ${damage} de dano!`);
            if (gameState.currentEnemy.hp <= 0) {
                logEvent(`${gameState.currentEnemy.name} foi derrotado!`);
                enemyNameSpan.textContent = "Nenhum Inimigo";
                enemyHpSpan.textContent = "HP: --";
                attackButton.disabled = true;
                abilityButton.disabled = true;
                fleeButton.disabled = true;
                collectItem(); // Coleta um item ao derrotar um inimigo
            } else {
                enemyAttack();
            }
        });

        function enemyAttack() {
            const playerClass = gameData.classes[gameState.player];
            const damage = Math.max(0, gameState.currentEnemy.attack - playerClass.baseDefense);
            logEvent(`${gameState.currentEnemy.name} atacou você e causou ${damage} de dano!`);
        }

        function logEvent(message) {
            gameState.gameLog.push(message);
            gameLogDiv.innerHTML += `<p>${message}</p>`;
            gameLogDiv.scrollTop = gameLogDiv.scrollHeight; // Rolagem automática
        }

        function collectItem() {
            const items = ["Poção de Vida", "Espada Antiga", "Escudo de Madeira"];
            const randomItem = items[Math.floor(Math.random() * items.length)];
            gameState.items.push(randomItem);
            logEvent(`Você coletou um item: ${randomItem}`);
            updatePlayerStats();
        }
    </script>
</body>
</html>
