<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>DUNGEONS CAVE</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet" />
    <style>
        /* Estilos personalizados para o jogo */
        body {
            font-family: 'Press Start 2P', cursive;
            background-color: #2c3e50;
            color: #ecf0f1;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
            box-sizing: border-box;
            margin: 0;
        }

        .game-container {
            background-color: #34495e;
            border: 4px solid #1a242f;
            border-radius: 8px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            padding: 1.5rem;
            max-width: 900px;
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            position: relative;
        }

        h1, h2, h3 {
            color: #f1c40f;
            text-align: center;
            margin-bottom: 1rem;
            text-shadow: 2px 2px #000;
        }

        .pixel-button {
            background-color: #e74c3c;
            color: white;
            padding: 0.75rem 1.5rem;
            border: 3px solid #c0392b;
            border-radius: 5px;
            cursor: pointer;
            font-family: 'Press Start 2P', cursive;
            font-size: 0.9rem;
            transition: background-color 0.1s, transform 0.1s;
            box-shadow: 3px 3px #a52a22;
            display: inline-block;
            text-align: center;
            image-rendering: pixelated;
        }

        .pixel-button:hover {
            background-color: #c0392b;
            transform: translateY(1px);
            box-shadow: 2px 2px #a52a22;
        }

        .pixel-button:active {
            transform: translateY(3px);
            box-shadow: 0 0 #a52a22;
        }

        .pixel-button:disabled {
            background-color: #95a5a6;
            border-color: #7f8c8d;
            color: #bdc3c7;
            cursor: not-allowed;
            box-shadow: 3px 3px #525e5e;
        }

        .panel {
            background-color: #2c3e50;
            border: 2px solid #1a242f;
            border-radius: 5px;
            padding: 1rem;
            box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.3);
        }

        .game-log {
            height: 150px;
            overflow-y: auto;
            background-color: #1a242f;
            border: 2px solid #000;
            padding: 0.75rem;
            font-size: 0.8rem;
            line-height: 1.4;
            color: #95a5a6;
            border-radius: 5px;
        }

        .game-log p { margin-bottom: 0.25rem; }
        .game-log p.text-yellow-400 { color: #f1c40f; }
        .game-log p.text-red-400 { color: #e74c3c; }
        .game-log p.text-orange-400 { color: #f39c12; }
        .game-log p.text-green-400 { color: #2ecc71; }
        .game-log p.text-blue-300 { color: #85c1e9; }

        .modal {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #34495e;
            border: 4px solid #1a242f;
            border-radius: 8px;
            padding: 1.5rem 2rem;
            z-index: 100;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.7);
            text-align: center;
            width: 90%;
            max-width: 600px;
        }
        
        #start-screen, #class-selection-modal, #game-screen {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        #start-screen {
            align-items: center;
        }

        #game-screen { display: none; }
        
        /* --- LÓGICA DE VISIBILIDADE DAS TELAS --- */
        #default-view, #combat-scene {
            display: none; /* Ambos começam escondidos */
            flex-direction: column;
            gap: 1rem;
        }
        
        /* Quando não está em combate, mostra a visão padrão */
        #game-screen:not(.in-combat) #default-view {
            display: flex;
        }

        /* Quando está em combate, mostra a cena de combate */
        #game-screen.in-combat #combat-scene {
            display: flex;
        }
        
        /* --- ESTILOS DA VISÃO PADRÃO --- */
        .stats-area, .actions-inventory-area {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        @media (min-width: 768px) {
            .stats-area, .actions-inventory-area { flex-direction: row; }
        }

        .player-panel, .enemy-panel, .inventory-panel { flex: 1; }
        .actions-panel { flex: 2; }

        .player-stats, .enemy-info {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem 1rem;
            justify-content: center;
            font-size: 0.85rem;
        }

        .player-stats span, .enemy-info span {
            background-color: #1a242f;
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
            white-space: nowrap;
        }
        
        .inventory {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            max-height: 150px;
            overflow-y: auto;
            background-color: #1a242f;
            border: 1px solid #555;
            border-radius: 4px;
            padding: 0.5rem;
        }

        .inventory-item {
            background-color: #2c3e50;
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.8rem;
        }

        /* --- ESTILOS DA CENA DE COMBATE --- */
        #combat-scene {
            background-color: #5a7d3c;
            border: 4px solid #1a242f;
            border-radius: 5px;
            padding: 1rem;
            position: relative;
            height: 450px;
        }
        
        .combat-area {
            position: absolute;
            width: 45%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        #enemy-combat-area {
            top: 20px;
            right: 20px;
            align-items: flex-end;
        }
        
        #player-combat-area {
            bottom: 20px;
            left: 20px;
            align-items: flex-start;
        }

        .sprite {
            height: 120px;
            width: auto;
            image-rendering: pixelated;
            margin-bottom: 10px;
        }
        
        #enemy-sprite {
            height: 100px;
        }

        .status-box {
            background-color: #ecf0f1;
            color: #2c3e50;
            border: 3px solid #1a242f;
            border-radius: 8px;
            padding: 0.5rem;
            font-size: 0.8rem;
            box-shadow: 3px 3px #1a242f;
            text-align: left;
            width: 100%;
        }

        .status-box .name-level {
            display: flex;
            justify-content: space-between;
            font-weight: bold;
            margin-bottom: 4px;
        }
        
        .hp-bar-container {
            width: 100%;
            height: 15px;
            background-color: #95a5a6;
            border: 1px solid #1a242f;
            border-radius: 4px;
            overflow: hidden;
            display: flex;
            align-items: center;
            position: relative; /* Para posicionar a barra de HP */
        }
        
        .hp-bar-label {
            font-size: 0.7rem;
            font-weight: bold;
            color: #fff;
            margin-left: 5px;
            text-shadow: 1px 1px #000;
            z-index: 2;
        }

        .hp-bar {
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            background-color: #2ecc71; /* Verde */
            border-radius: 2px;
            transition: width 0.5s ease-out, background-color 0.5s ease;
            z-index: 1;
        }
        
        .hp-bar.yellow { background-color: #f1c40f; }
        .hp-bar.red { background-color: #e74c3c; }

        #combat-player-hp-text {
            text-align: right;
            font-size: 0.8rem;
            margin-top: 4px;
        }
        
        /* --- AÇÕES E LOG (COMPARTILHADO) --- */
        .game-actions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 0.75rem;
        }
        
        #game-screen.in-combat .actions-panel {
            flex-basis: 50%;
        }

        #game-screen.in-combat .inventory-panel {
             display: none; /* Esconde inventário na tela de combate */
        }
    </style>
</head>
<body>
    <div class="game-container">
        <!-- MODAL GENÉRICO -->
        <div id="game-modal" class="modal">
            <div class="modal-content">
                <p id="modal-message" class="text-lg mb-4"></p>
                <button id="modal-ok-button" class="pixel-button">OK</button>
            </div>
        </div>

        <!-- TELA INICIAL -->
        <div id="start-screen">
            <h1>DUNGEONS CAVE</h1>
            <p class="text-center italic mb-4">"Aventure-se nas profundezas... se tiver coragem."</p>
            <button id="start-game-button" class="pixel-button mt-6">Escolher Classe</button>
        </div>

        <!-- MODAL DE SELEÇÃO DE CLASSE -->
        <div id="class-selection-modal" class="modal">
            <!-- Conteúdo é gerado via JS -->
        </div>

        <!-- TELA PRINCIPAL DO JOGO -->
        <div id="game-screen">
            <h2 id="screen-title" class="text-2xl">Aventura em Andamento!</h2>
            
            <!-- CENA DE COMBATE -->
            <div id="combat-scene">
                 <div id="enemy-combat-area" class="combat-area">
                    <div class="status-box">
                        <div class="name-level"><span id="combat-enemy-name"></span><span id="combat-enemy-level"></span></div>
                        <div class="hp-bar-container"><span class="hp-bar-label">HP</span><div class="hp-bar" id="combat-enemy-hp-bar"></div></div>
                    </div>
                    <img id="enemy-sprite" class="sprite" src="" alt="Inimigo">
                </div>

                <div id="player-combat-area" class="combat-area">
                    <img id="player-sprite" class="sprite" src="" alt="Herói">
                    <div class="status-box">
                       <div class="name-level"><span id="combat-player-name"></span><span id="combat-player-level"></span></div>
                        <div class="hp-bar-container"><span class="hp-bar-label">HP</span><div class="hp-bar" id="combat-player-hp-bar"></div></div>
                        <div id="combat-player-hp-text"></div>
                    </div>
                </div>
            </div>

            <!-- VISÃO PADRÃO/EXPLORAÇÃO -->
            <div id="default-view">
                <div class="stats-area">
                    <div class="panel player-panel">
                        <h3>Seu Herói</h3>
                        <div id="player-stats" class="player-stats"></div>
                    </div>
                    <div class="panel enemy-panel">
                        <h3>Inimigo Próximo</h3>
                        <div id="enemy-info" class="enemy-info">
                            <span id="enemy-name">Nenhum</span>
                            <span id="enemy-hp">HP: --</span>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- PAINÉIS DE LOG, AÇÕES E INVENTÁRIO -->
            <div class="panel log-panel">
                <h3>Registro de Eventos</h3>
                <div id="game-log" class="game-log">
                    <p>Bem-vindo à Dungeons Cave!</p>
                </div>
            </div>
            
            <div class="actions-inventory-area">
                <div class="panel actions-panel">
                    <h3>Ações</h3>
                    <div id="game-actions" class="game-actions">
                        <button id="explore-button" class="pixel-button">Explorar</button>
                        <button id="attack-button" class="pixel-button">Atacar</button>
                        <button id="ability-button" class="pixel-button">Habilidade</button> 
                        <button id="flee-button" class="pixel-button">Fugir</button>
                    </div>
                </div>
                <div class="panel inventory-panel">
                    <h3>Mochila</h3>
                    <div id="inventory" class="inventory"></div>
                </div>
            </div>

        </div>
    </div>

    <script>
        // --- Referências aos Elementos do DOM ---
        const getEl = (id) => document.getElementById(id);

        const gameScreen = getEl('game-screen');
        const startScreen = getEl('start-screen');
        const classSelectionModal = getEl('class-selection-modal');
        const gameModal = getEl('game-modal');
        const startGameBtn = getEl('start-game-button');
        const screenTitle = getEl('screen-title');

        // Sprites de Combate
        const playerSprite = getEl('player-sprite');
        const enemySprite = getEl('enemy-sprite');
        
        // UI de Combate
        const combatPlayerName = getEl('combat-player-name');
        const combatPlayerLevel = getEl('combat-player-level');
        const combatPlayerHpBar = getEl('combat-player-hp-bar');
        const combatPlayerHpText = getEl('combat-player-hp-text');
        const combatEnemyName = getEl('combat-enemy-name');
        const combatEnemyLevel = getEl('combat-enemy-level');
        const combatEnemyHpBar = getEl('combat-enemy-hp-bar');
        
        // UI Padrão
        const playerStatsDiv = getEl('player-stats');
        const enemyNameSpan = getEl('enemy-name');
        const enemyHpSpan = getEl('enemy-hp');

        // Elementos Compartilhados
        const gameLogDiv = getEl('game-log');
        const inventoryDiv = getEl('inventory');
        const exploreButton = getEl('explore-button');
        const attackButton = getEl('attack-button');
        const abilityButton = getEl('ability-button');
        const fleeButton = getEl('flee-button');
        const modalMessage = getEl('modal-message');
        const modalOkBtn = getEl('modal-ok-button');

        // --- Objeto de Dados do Jogo ---
        const gameData = {
            inCombat: false,
            player: { level: 1 },
            currentEnemy: null,
            classes: {
                knight: { name: "Cavaleiro", ability: "Escudo Imbatível", baseHp: 120, baseAttack: 15, baseDefense: 10, image: "image_f35a10.png" },
                rogue: { name: "Ladino", ability: "Ataque Furtivo", baseHp: 80, baseAttack: 25, baseDefense: 5, image: "https://placehold.co/120x120/333/FFF?text=Ladino" },
                mage: { name: "Mago", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3, image: "https://placehold.co/120x120/333/FFF?text=Mago" },
            },
            enemies: {
                giantRat: { name: "Rato Gigante", level: 1, hp: 15, maxHp: 15, attack: 4, defense: 1, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.1 }], image: "https://placehold.co/100x100/666/FFF?text=Rato" },
                caveBat: { name: "Morcego da Caverna", level: 1, hp: 18, maxHp: 18, attack: 5, defense: 1, lootTable: [], image: "https://placehold.co/100x100/666/FFF?text=Morcego" },
                smallSpider: { name: "Aranha Pequena", level: 2, hp: 22, maxHp: 22, attack: 6, defense: 2, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.15 }], image: "https://placehold.co/100x100/666/FFF?text=Aranha" },
                goblin: { name: "Goblin Ardiloso", level: 2, hp: 30, maxHp: 30, attack: 8, defense: 2, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.3 }], image: "https://placehold.co/100x100/666/FFF?text=Goblin" },
                orc: { name: "Orc Brutal", level: 3, hp: 50, maxHp: 50, attack: 12, defense: 4, lootTable: [{ item: "Adaga Enferrujada", chance: 0.2 }], image: "https://placehold.co/100x100/666/FFF?text=Orc" },
            },
            items: {
                "Poção de Cura Pequena": { type: "potion", effect: "heal", power: 20, consumable: true },
                "Adaga Enferrujada": { type: "weapon", attackBonus: 2 },
            }
        };

        // --- Funções de UI ---

        function showModal(message, onOkCallback = null) {
            modalMessage.textContent = message;
            gameModal.style.display = 'block';
            modalOkBtn.onclick = () => {
                gameModal.style.display = 'none';
                if (onOkCallback) onOkCallback();
            };
        }

        function logEvent(message, type = 'normal') {
            const p = document.createElement('p');
            p.textContent = message;
            const classMap = {
                important: 'text-yellow-400', combat: 'text-red-400',
                player_damage: 'text-red-400', enemy_damage: 'text-orange-400',
                heal: 'text-green-400', success: 'text-green-400',
                info: 'text-blue-300'
            };
            if(classMap[type]) p.classList.add(...classMap[type].split(' '));
            gameLogDiv.appendChild(p);
            gameLogDiv.scrollTop = gameLogDiv.scrollHeight;
        }

        function updateAllUI() {
            updatePlayerStats();
            updateEnemyInfo();
            updateCombatUI();
            renderInventory();
            updateActionButtons();
        }

        function updatePlayerStats() {
            const player = gameData.player;
            if(!player.name) return;
            const totalAttack = player.attack + (player.equippedWeapon?.attackBonus || 0);
            playerStatsDiv.innerHTML = `
                <span>${player.name} Nv.${player.level}</span>
                <span>HP: ${player.hp}/${player.maxHp}</span>
                <span>Ataque: ${totalAttack}</span>
                <span>Defesa: ${player.defense}</span>
            `;
        }

        function updateEnemyInfo() {
             const enemy = gameData.currentEnemy;
             if(enemy && !gameData.inCombat) {
                 enemyNameSpan.textContent = `${enemy.name}`;
                 enemyHpSpan.textContent = `HP: ${enemy.hp}`;
             } else {
                 enemyNameSpan.textContent = "Nenhum";
                 enemyHpSpan.textContent = "HP: --";
             }
        }
        
        function updateCombatUI() {
            if (!gameData.inCombat) return;
            const player = gameData.player;
            const enemy = gameData.currentEnemy;

            combatPlayerName.textContent = player.name;
            combatPlayerLevel.textContent = `Nv${player.level}`;
            updateHealthBar(combatPlayerHpBar, player.hp, player.maxHp);
            combatPlayerHpText.textContent = `${player.hp} / ${player.maxHp}`;

            combatEnemyName.textContent = enemy.name;
            combatEnemyLevel.textContent = `Nv${enemy.level}`;
            updateHealthBar(combatEnemyHpBar, enemy.hp, enemy.maxHp);
        }

        function updateHealthBar(barElement, current, max) {
            const percentage = Math.max(0, (current / max) * 100);
            barElement.style.width = `${percentage}%`;
            barElement.classList.remove('green', 'yellow', 'red');
            if (percentage <= 25) barElement.classList.add('red');
            else if (percentage <= 50) barElement.classList.add('yellow');
            else barElement.classList.add('green');
        }

        function updateActionButtons() {
            exploreButton.disabled = gameData.inCombat;
            attackButton.disabled = !gameData.inCombat;
            abilityButton.disabled = !gameData.inCombat || gameData.player.specialAbilityUsedThisCombat;
            fleeButton.disabled = !gameData.inCombat;
        }

        function renderInventory() {
            inventoryDiv.innerHTML = '';
            if (!gameData.player.inventory || gameData.player.inventory.length === 0) {
                inventoryDiv.innerHTML = '<p class="text-gray-400 text-sm text-center">Vazia</p>';
                return;
            }
            gameData.player.inventory.forEach(itemName => {
                const item = gameData.items[itemName];
                const itemDiv = document.createElement('div');
                itemDiv.className = 'inventory-item';
                itemDiv.innerHTML = `<span>${itemName}</span> <button class="pixel-button text-xs py-1 px-2 use-item-btn" data-item="${itemName}">Usar</button>`;
                inventoryDiv.appendChild(itemDiv);
            });
            document.querySelectorAll('.use-item-btn').forEach(btn => {
                btn.onclick = (e) => useItem(e.target.dataset.item);
            });
        }
        
        // --- Funções de Lógica do Jogo ---

        function selectClass(classKey) {
            const cls = gameData.classes[classKey];
            const player = gameData.player;
            player.name = cls.name;
            player.maxHp = cls.baseHp;
            player.hp = cls.baseHp;
            player.attack = cls.baseAttack;
            player.defense = cls.baseDefense;
            player.abilityName = cls.ability;
            player.image = cls.image; // Guarda a URL da imagem
            player.inventory = ["Poção de Cura Pequena"];

            playerSprite.src = player.image; // Define a imagem do jogador
            
            classSelectionModal.style.display = 'none';
            startScreen.style.display = 'none';
            gameScreen.style.display = 'flex';
            
            logEvent(`Você é um ${cls.name}!`, 'important');
            updateAllUI();
        }

        function explore() {
            logEvent("Você explora a masmorra...", 'info');
            if (Math.random() > 0.5) {
                logEvent("...mas não encontra nada.", 'info');
                return;
            }
            const enemyKeys = Object.keys(gameData.enemies);
            const randomKey = enemyKeys[Math.floor(Math.random() * enemyKeys.length)];
            startCombat(randomKey);
        }

        function startCombat(enemyKey) {
            const enemyTemplate = gameData.enemies[enemyKey];
            gameData.currentEnemy = { ...enemyTemplate };
            gameData.player.specialAbilityUsedThisCombat = false;
            gameData.inCombat = true;
            
            enemySprite.src = gameData.currentEnemy.image; // Define a imagem do inimigo
            
            gameScreen.classList.add('in-combat');
            screenTitle.textContent = "EM COMBATE!";
            logEvent(`Um ${gameData.currentEnemy.name} aparece!`, 'important');
            
            updateAllUI();
        }

        function endCombat(fled = false) {
            if (!fled) {
                const enemy = gameData.currentEnemy;
                logEvent(`Você derrotou o ${enemy.name}!`, 'success');
                enemy.lootTable.forEach(drop => {
                    if (Math.random() < drop.chance) {
                        gameData.player.inventory.push(drop.item);
                        logEvent(`Você encontrou: ${drop.item}!`, 'info');
                    }
                });
            }

            gameData.inCombat = false;
            gameData.currentEnemy = null;
            enemySprite.src = ""; // Limpa a imagem do inimigo
            gameScreen.classList.remove('in-combat');
            screenTitle.textContent = "Aventura em Andamento!";
            updateAllUI();
        }

        function attack() {
            if (!gameData.inCombat) return;

            const player = gameData.player;
            const enemy = gameData.currentEnemy;
            const playerAttack = player.attack + (player.equippedWeapon?.attackBonus || 0);
            const damage = Math.max(1, Math.floor(playerAttack * (1 + Math.random()*0.1)) - enemy.defense);
            
            enemy.hp -= damage;
            logEvent(`Você ataca e causa ${damage} de dano!`, 'enemy_damage');
            
            if (enemy.hp <= 0) {
                endCombat();
            } else {
                disableActions(true);
                setTimeout(enemyTurn, 1000);
            }
            updateCombatUI();
        }

        function enemyTurn() {
            if (!gameData.inCombat) return;
            const player = gameData.player;
            const enemy = gameData.currentEnemy;
            const damage = Math.max(1, Math.floor(enemy.attack * (1 + Math.random()*0.1)) - player.defense);

            player.hp -= damage;
            logEvent(`O ${enemy.name} ataca e causa ${damage} de dano!`, 'player_damage');
            
            if (player.hp <= 0) {
                player.hp = 0;
                gameOver();
            } else {
                disableActions(false);
            }
            updateAllUI();
        }
        
        function useSpecialAbility() {
             const player = gameData.player;
             logEvent(`${player.name} usa ${player.abilityName}!`, 'success');
             player.defense += 5;
             logEvent("Sua defesa aumentou temporariamente!", 'info');
             player.specialAbilityUsedThisCombat = true;

             disableActions(true);
             setTimeout(enemyTurn, 1000);
             updateAllUI();
        }
        
        function flee() {
            if (Math.random() < 0.5) {
                logEvent("Você conseguiu fugir!", 'success');
                endCombat(true);
            } else {
                logEvent("A fuga falhou!", 'player_damage');
                disableActions(true);
                setTimeout(enemyTurn, 1000);
            }
        }
        
        function useItem(itemName) {
            const item = gameData.items[itemName];
            if (!item?.consumable) return;
            
            if(item.type === 'potion' && item.effect === 'heal') {
                const player = gameData.player;
                player.hp = Math.min(player.maxHp, player.hp + item.power);
                logEvent(`Você usou ${itemName} e recuperou ${item.power} HP.`, 'heal');
            }
            
            const itemIndex = gameData.player.inventory.indexOf(itemName);
            if (itemIndex > -1) gameData.player.inventory.splice(itemIndex, 1);
            
            updateAllUI();
        }

        function disableActions(disabled) {
            attackButton.disabled = disabled;
            abilityButton.disabled = disabled || gameData.player.specialAbilityUsedThisCombat;
            fleeButton.disabled = disabled;
        }

        function gameOver() {
            disableActions(true);
            showModal("Você foi derrotado... A masmorra o consumiu.", () => window.location.reload());
        }

        function initialize() {
            // Preencher modal de seleção de classe
            classSelectionModal.innerHTML = `
                <div class="modal-content">
                    <h2>Escolha sua Classe</h2>
                    <div id="class-options" class="my-4 grid grid-cols-2 gap-4"></div>
                </div>
            `;
            const classOptionsDiv = getEl('class-options');
            for (const key in gameData.classes) {
                const button = document.createElement('button');
                button.className = 'pixel-button';
                button.textContent = gameData.classes[key].name;
                button.onclick = () => selectClass(key);
                classOptionsDiv.appendChild(button);
            }
            
            // Listeners dos botões
            startGameBtn.onclick = () => classSelectionModal.style.display = 'block';
            exploreButton.onclick = explore;
            attackButton.onclick = attack;
            abilityButton.onclick = useSpecialAbility;
            fleeButton.onclick = flee;
            
            updateActionButtons();
            logEvent("Escolha uma classe para começar.", 'important');
        }

        // --- Iniciar Jogo ---
        initialize();

    </script>
</body>
</html>
