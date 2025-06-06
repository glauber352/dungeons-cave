<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
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

        /* Ajuste para o botão de usar item dentro do inventário */
        .inventory-item .pixel-button {
            padding: 0.25rem 0.5rem;
            font-size: 0.7rem;
            box-shadow: 2px 2px #777;
            border: 2px solid #555;
        }

        .inventory-item .pixel-button:hover {
            transform: translateY(0);
            box-shadow: 1px 1px #777;
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
            <button id="start-game-button" class="pixel-button mt-6">Iniciar Jogo</button>
        </div>

        <!-- MODAL DE SELEÇÃO DE CLASSE (Agora não será exibido, mas o HTML permanece) -->
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
            player: { level: 1, inventory: [], monstersSlain: 0 }, // Inicialização básica
            currentEnemy: null,
            classes: {
                knight: { name: "Cavaleiro", ability: "Escudo Imbatível", baseHp: 120, baseAttack: 15, baseDefense: 10, image: "https://placehold.co/120x120/555/FFF?text=Cavaleiro" },
                rogue: { name: "Ladino", ability: "Ataque Furtivo", baseHp: 80, baseAttack: 25, baseDefense: 5, image: "https://placehold.co/120x120/333/FFF?text=Ladino" },
                mage: { name: "Mago", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3, image: "https://placehold.co/120x120/333/FFF?text=Mago" },
            },
            enemies: {
                giantRat: { name: "Rato Gigante", level: 1, hp: 15, maxHp: 15, attack: 4, defense: 1, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.3 }], image: "https://placehold.co/100x100/666/FFF?text=Rato" },
                caveBat: { name: "Morcego da Caverna", level: 1, hp: 18, maxHp: 18, attack: 5, defense: 1, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.1 }], image: "https://placehold.co/100x100/666/FFF?text=Morcego" },
                smallSpider: { name: "Aranha Pequena", level: 2, hp: 22, maxHp: 22, attack: 6, defense: 2, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.15 }], image: "https://placehold.co/100x100/666/FFF?text=Aranha" },
                goblin: { name: "Goblin Ardiloso", level: 2, hp: 30, maxHp: 30, attack: 8, defense: 2, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.3 }], image: "https://placehold.co/100x100/666/FFF?text=Goblin" },
                orc: { name: "Orc Brutal", level: 3, hp: 50, maxHp: 50, attack: 12, defense: 4, lootTable: [{ item: "Adaga Enferrujada", chance: 0.5 }], image: "https://placehold.co/100x100/666/FFF?text=Orc" },
                caveBoss: { name: "Boss da Caverna", level: 5, hp: 150, maxHp: 150, attack: 20, defense: 8, lootTable: [{ item: "Poção de Cura Grande", chance: 1.0 }, { item: "Espada Flamejante", chance: 0.7 }], image: "https://placehold.co/120x120/800/FFF?text=BOSS" },
                
                // Novos Monstros Adicionados
                zombieOgre: { name: "Ogro Zumbi", level: 2, hp: 85, maxHp: 85, attack: 6, defense: 3, 
                    lootTable: [
                        { item: "Cristal pútrido", chance: 0.2 },
                        { item: "Armadura corroída", chance: 0.1 },
                        // Ouro podre será adicionado diretamente ao ouro do jogador em handleLoot
                        { item: "Fragmento de runa necromântica", chance: 0.05 }
                    ], 
                    image: "https://placehold.co/120x120/4a6d4e/FFF?text=Zumbi" 
                },
                yuanTi: { name: "Yuan-ti Puro-Sangue", level: 3, hp: 40, maxHp: 40, attack: 4, defense: 2, 
                    lootTable: [
                        { item: "Poção de veneno", chance: 0.3 },
                        { item: "Medalhão de serpente", chance: 0.15 },
                        { item: "Escamas de yuan-ti", chance: 0.2 }
                    ], 
                    image: "https://placehold.co/120x120/8b4513/FFF?text=Yuan-ti" 
                },
                youngRedDragon: { name: "Dragão Vermelho Jovem", level: 10, hp: 178, maxHp: 178, attack: 10, defense: 6, 
                    lootTable: [
                        { item: "Escamas vermelhas", chance: 0.8 },
                        { item: "Cristais de fogo", chance: 0.6 },
                        // Tesouro será adicionado diretamente ao ouro do jogador em handleLoot
                        { item: "Osso de dragão", chance: 0.4 }
                    ], 
                    image: "https://placehold.co/120x120/CC0000/FFF?text=Dragao" 
                },
                minorDeathKnight: { name: "Cavaleiro da Morte Menor", level: 5, hp: 95, maxHp: 95, attack: 7, defense: 4, 
                    lootTable: [
                        { item: "Espada das Trevas", chance: 0.25 },
                        { item: "Elmo Amaldiçoado", chance: 0.15 },
                        { item: "Anel de resistência necrótica", chance: 0.1 }
                    ], 
                    image: "https://placehold.co/120x120/333333/FFF?text=Cavaleiro" 
                },
                griffon: { name: "Grifo", level: 2, hp: 59, maxHp: 59, attack: 6, defense: 3, 
                    lootTable: [
                        { item: "Pena mágica de grifo", chance: 0.3 },
                        { item: "Ovo de grifo", chance: 0.05 },
                        { item: "Couro de grifo", chance: 0.2 }
                    ], 
                    image: "https://placehold.co/120x120/87CEEB/FFF?text=Grifo" 
                },
            },
            items: {
                "Poção de Cura Pequena": { type: "potion", effect: "heal", power: 20, consumable: true },
                "Poção de Cura Grande": { type: "potion", effect: "heal", power: 50, consumable: true },
                "Adaga Enferrujada": { type: "weapon", attackBonus: 5 },
                "Espada Flamejante": { type: "weapon", attackBonus: 15 },

                // Novos Itens Adicionados
                "Cristal pútrido": { type: "component", description: "Um cristal escuro que emana energia necromântica." },
                "Armadura corroída": { type: "armor", defenseBonus: 2, description: "Uma armadura gasta, mas que ainda oferece alguma proteção." },
                "Fragmento de runa necromântica": { type: "component", description: "Um pequeno pedaço de uma runa que pulsa com magia da morte." },
                "Poção de veneno": { type: "potion", effect: "damageEnemy", power: 10, consumable: true, description: "Uma poção que pode ser arremessada para causar dano de veneno." },
                "Medalhão de serpente": { type: "trinket", description: "Um medalhão estranho com um símbolo de serpente." },
                "Escamas de yuan-ti": { type: "component", description: "Escamas esverdeadas, usadas em alquimia ou artesanato." },
                "Escamas vermelhas": { type: "material", description: "Escamas duras e brilhantes de um dragão, valiosas para criar itens lendários." },
                "Cristais de fogo": { type: "component", description: "Cristais incandescentes, fonte de energia mágica." },
                "Osso de dragão": { type: "material", description: "Um grande osso, pode ser transformado em armas ou varinhas poderosas." },
                "Pena mágica de grifo": { type: "component", description: "Uma pena iridescente que vibra com magia do vento." },
                "Ovo de grifo": { type: "valuables", description: "Um ovo grande e pesado, extremamente valioso." },
                "Couro de grifo": { type: "material", description: "Couro resistente, ideal para armaduras leves e flexíveis." },
                "Espada das Trevas": { type: "weapon", attackBonus: 8, description: "Uma espada que irradia uma aura escura." }, // Corresponde a uma espada +1
                "Elmo Amaldiçoado": { type: "armor", defenseBonus: 3, description: "Um elmo antigo que parece prender sussurros macabros." },
                "Anel de resistência necrótica": { type: "ring", description: "Um anel simples que oferece proteção contra energia da morte." },
            }
        };

        // --- Funções de UI ---

        /**
         * Exibe um modal com uma mensagem e um botão "OK".
         * @param {string} message - A mensagem a ser exibida no modal.
         * @param {Function} [onOkCallback] - Função opcional a ser executada ao clicar em OK.
         */
        function showModal(message, onOkCallback = null) {
            modalMessage.textContent = message;
            gameModal.style.display = 'block';
            modalOkBtn.onclick = () => {
                gameModal.style.display = 'none';
                if (onOkCallback) onOkCallback();
            };
        }

        /**
         * Adiciona uma mensagem ao registro de eventos do jogo.
         * @param {string} message - A mensagem a ser logada.
         * @param {string} [type='normal'] - O tipo de mensagem para aplicar estilos CSS (e.g., 'combat', 'player_damage', 'heal').
         */
        function logEvent(message, type = 'normal') {
            const p = document.createElement('p');
            p.textContent = message;
            const classMap = {
                important: 'text-yellow-400', combat: 'text-red-400',
                player_damage: 'text-red-400', enemy_damage: 'text-orange-400',
                heal: 'text-green-400', success: 'text-green-400',
                info: 'text-blue-300', error: 'text-red-500' // Adicionado tipo 'error'
            };
            if(classMap[type]) p.classList.add(...classMap[type].split(' '));
            gameLogDiv.appendChild(p);
            gameLogDiv.scrollTop = gameLogDiv.scrollHeight; // Rola para o final
        }

        /** Atualiza todos os elementos da interface do usuário do jogo. */
        function updateAllUI() {
            updatePlayerStats();
            updateEnemyInfo();
            updateCombatUI();
            renderInventory();
            updateActionButtons();
        }

        /** Atualiza as estatísticas do jogador na interface padrão. */
        function updatePlayerStats() {
            const player = gameData.player;
            if(!player.name) return; // Garante que o jogador está inicializado
            const totalAttack = player.attack + (player.equippedWeapon?.attackBonus || 0);
            playerStatsDiv.innerHTML = `
                <span>${player.name} Nv.${player.level}</span>
                <span>HP: ${player.hp}/${player.maxHp}</span>
                <span>Ataque: ${totalAttack}</span>
                <span>Defesa: ${player.defense}</span>
                <span>XP: ${player.xp}/${player.xpToNextLevel}</span>
                <span>Ouro: ${player.gold}</span>
                <span>Monstros Abatidos: ${player.monstersSlain}</span>
                ${player.equippedWeapon ? `<span>Arma: ${player.equippedWeapon.name}</span>` : '<span>Arma: Nenhuma</span>'}
            `;
        }

        /** Atualiza as informações do inimigo na interface padrão. */
        function updateEnemyInfo() {
            const enemy = gameData.currentEnemy;
            if(enemy && !gameData.inCombat) {
                enemyNameSpan.textContent = `${enemy.name}`;
                enemyHpSpan.textContent = `HP: ${enemy.hp}/${enemy.maxHp}`;
            } else {
                enemyNameSpan.textContent = "Nenhum";
                enemyHpSpan.textContent = "HP: --";
            }
        }
        
        /** Atualiza a interface do usuário durante o combate. */
        function updateCombatUI() {
            if (!gameData.inCombat) return;
            const player = gameData.player;
            const enemy = gameData.currentEnemy;

            combatPlayerName.textContent = player.name;
            combatPlayerLevel.textContent = `Nv.${player.level}`;
            updateHealthBar(combatPlayerHpBar, player.hp, player.maxHp);
            combatPlayerHpText.textContent = `${player.hp} / ${player.maxHp}`;

            combatEnemyName.textContent = enemy.name;
            combatEnemyLevel.textContent = `Nv.${enemy.level}`;
            updateHealthBar(combatEnemyHpBar, enemy.hp, enemy.maxHp);
        }

        /**
         * Atualiza a largura e a cor de uma barra de vida.
         * @param {HTMLElement} barElement - O elemento da barra de vida (div).
         * @param {number} current - A vida atual.
         * @param {number} max - A vida máxima.
         */
        function updateHealthBar(barElement, current, max) {
            const percentage = Math.max(0, (current / max) * 100);
            barElement.style.width = `${percentage}%`;
            barElement.classList.remove('green', 'yellow', 'red');
            if (percentage <= 20) {
                barElement.classList.add('red');
            } else if (percentage <= 50) {
                barElement.classList.add('yellow');
            } else {
                barElement.classList.add('green');
            }
        }

        /** Habilita/desabilita botões de ação com base no estado de combate e atualiza a visibilidade das seções do jogo. */
        function updateActionButtons() {
            const inCombat = gameData.inCombat;
            exploreButton.disabled = inCombat;
            attackButton.disabled = !inCombat;
            abilityButton.disabled = !inCombat;
            fleeButton.disabled = !inCombat;
            
            // Gerencia a visibilidade das seções
            if (inCombat) {
                gameScreen.classList.add('in-combat');
                screenTitle.textContent = "Em Combate!";
            } else {
                gameScreen.classList.remove('in-combat');
                screenTitle.textContent = "Aventura em Andamento!";
            }
        }

        /** Renderiza o inventário do jogador. */
        function renderInventory() {
            inventoryDiv.innerHTML = '';
            if (gameData.player.inventory.length === 0) {
                inventoryDiv.innerHTML = '<p class="text-center text-gray-400">Mochila vazia.</p>';
                return;
            }
            gameData.player.inventory.forEach(item => {
                const itemEl = document.createElement('div');
                itemEl.className = 'inventory-item';
                itemEl.innerHTML = `
                    <span>${item.name} (${item.quantity})</span>
                    <button class="pixel-button text-xs px-2 py-1 use-item-btn" data-item="${item.name}">Usar</button>
                `;
                inventoryDiv.appendChild(itemEl);
            });

            document.querySelectorAll('.use-item-btn').forEach(button => {
                button.onclick = (e) => {
                    const itemName = e.target.dataset.item;
                    useItem(itemName);
                };
            });
        }

        // --- Funções de Jogo ---

        /**
         * Inicia o jogo, selecionando uma classe padrão automaticamente.
         * A tela de seleção de classe é pulada.
         */
        function startGame() {
            startScreen.style.display = 'none';
            // Seleciona a classe "knight" (Cavaleiro) automaticamente
            selectClass('knight'); 
        }

        /**
         * Renderiza as opções de seleção de classe no modal.
         * (Esta função não será mais chamada com a seleção automática, mas é mantida por completude).
         */
        function renderClassSelection() {
            classSelectionModal.innerHTML = `
                <h2 class="text-xl mb-4">Escolha sua Classe</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    ${Object.entries(gameData.classes).map(([id, cls]) => `
                        <button class="pixel-button class-select-btn" data-class="${id}">
                            ${cls.name}
                        </button>
                    `).join('')}
                </div>
            `;
            document.querySelectorAll('.class-select-btn').forEach(button => {
                button.onclick = (e) => selectClass(e.target.dataset.class);
            });
        }

        /**
         * Seleciona a classe do jogador e inicializa seus atributos.
         * @param {string} classId - O ID da classe selecionada.
         */
        function selectClass(classId) {
            const selectedClass = gameData.classes[classId];
            gameData.player = {
                name: selectedClass.name,
                class: classId,
                level: 1,
                hp: selectedClass.baseHp,
                maxHp: selectedClass.baseHp,
                attack: selectedClass.baseAttack,
                defense: selectedClass.baseDefense,
                ability: selectedClass.ability,
                xp: 0,
                xpToNextLevel: 100,
                gold: 0,
                inventory: [],
                equippedWeapon: null,
                monstersSlain: 0, 
                image: selectedClass.image
            };
            logEvent(`Você escolheu ser um ${selectedClass.name}!`, 'success');
            classSelectionModal.style.display = 'none'; // Garante que o modal esteja escondido
            gameScreen.style.display = 'flex'; // Exibe a tela do jogo
            playerSprite.src = selectedClass.image; // Define o sprite do jogador
            updateAllUI();
        }
        
        /**
         * Concede experiência ao jogador.
         * @param {number} amount - A quantidade de XP a ser concedida.
         */
        function gainXp(amount) {
            gameData.player.xp += amount;
            logEvent(`Você ganhou ${amount} de XP!`, 'info');
            if (gameData.player.xp >= gameData.player.xpToNextLevel) {
                levelUp();
            }
            updateAllUI();
        }

        /** Aumenta o nível do jogador. */
        function levelUp() {
            const player = gameData.player;
            player.level++;
            player.xp -= player.xpToNextLevel;
            player.xpToNextLevel = Math.floor(player.xpToNextLevel * 1.5); // Aumenta o XP necessário para o próximo nível
            player.maxHp += 15; // Ganho de HP por nível
            player.hp = player.maxHp; // Cura total ao subir de nível
            player.attack += 3; // Ganho de Ataque por nível
            player.defense += 1; // Ganho de Defesa por nível
            logEvent(`Parabéns! Você alcançou o Nível ${player.level}!`, 'important');
            showModal(`Parabéns! Você alcançou o Nível ${player.level}!`, updateAllUI);
        }

        /**
         * Adiciona um item ao inventário do jogador.
         * @param {string} itemName - O nome do item a ser adicionado.
         * @param {number} [quantity=1] - A quantidade do item.
         */
        function addItemToInventory(itemName, quantity = 1) {
            const existingItem = gameData.player.inventory.find(item => item.name === itemName);
            if (existingItem) {
                existingItem.quantity += quantity;
            } else {
                gameData.player.inventory.push({ name: itemName, quantity: quantity });
            }
            logEvent(`Você pegou ${quantity}x ${itemName}.`, 'info');
            renderInventory();
        }

        /**
         * Remove um item do inventário do jogador.
         * @param {string} itemName - O nome do item a ser removido.
         * @param {number} [quantity=1] - A quantidade do item.
         * @returns {boolean} - True se o item foi removido com sucesso, false caso contrário.
         */
        function removeItemFromInventory(itemName, quantity = 1) {
            const itemIndex = gameData.player.inventory.findIndex(item => item.name === itemName);
            if (itemIndex > -1) {
                gameData.player.inventory[itemIndex].quantity -= quantity;
                if (gameData.player.inventory[itemIndex].quantity <= 0) {
                    gameData.player.inventory.splice(itemIndex, 1);
                }
                renderInventory();
                return true;
            }
            return false;
        }

        /**
         * Usa um item do inventário.
         * @param {string} itemName - O nome do item a ser usado.
         */
        function useItem(itemName) {
            const itemInfo = gameData.items[itemName];
            const player = gameData.player;

            if (!itemInfo) {
                logEvent(`Não foi possível usar ${itemName}.`, 'error');
                return;
            }

            if (itemInfo.type === "potion" && itemInfo.effect === "heal") {
                if (player.hp === player.maxHp) {
                    logEvent("Sua vida já está cheia!", 'info');
                    return;
                }
                const healAmount = itemInfo.power;
                player.hp = Math.min(player.maxHp, player.hp + healAmount);
                logEvent(`Você usou ${itemName} e recuperou ${healAmount} de HP.`, 'heal');
                removeItemFromInventory(itemName);
                updateAllUI();
            } else if (itemInfo.type === "potion" && itemInfo.effect === "damageEnemy") { // Novo: Poção de Dano
                if (!gameData.inCombat) {
                    logEvent("Você só pode usar poções de dano em combate.", 'info');
                    return;
                }
                const enemy = gameData.currentEnemy;
                const damageAmount = itemInfo.power;
                enemy.hp -= damageAmount;
                logEvent(`Você usou ${itemName} e causou ${damageAmount} de dano a ${enemy.name}!`, 'player_damage');
                removeItemFromInventory(itemName);
                if (enemy.hp <= 0) {
                    enemy.hp = 0;
                    logEvent(`${enemy.name} foi derrotado!`, 'success');
                    gameData.player.monstersSlain++;
                    gainXp(enemy.level * 20 + (enemy.name.includes("Boss") ? 100 : 0));
                    gameData.player.gold += enemy.level * 5 + Math.floor(Math.random() * 10);
                    logEvent(`Você encontrou ${enemy.level * 5 + Math.floor(Math.random() * 10)} de ouro.`, 'info');
                    handleLoot(enemy);
                    endCombat();
                    checkBossSpawn();
                } else {
                    setTimeout(enemyTurn, 700);
                }
                updateAllUI();
            } else if (itemInfo.type === "weapon") {
                // Se já houver uma arma equipada, guarda-a de volta no inventário
                if (player.equippedWeapon) {
                    // Verifica se a arma que o jogador está tentando equipar é a mesma que já está equipada
                    if (player.equippedWeapon.name === itemName) {
                        logEvent(`Você já está usando ${itemName}.`, 'info');
                        return;
                    }
                    addItemToInventory(player.equippedWeapon.name);
                    logEvent(`Você guardou sua ${player.equippedWeapon.name}.`, 'info');
                }
                // Equipa a nova arma
                player.equippedWeapon = { name: itemName, attackBonus: itemInfo.attackBonus };
                removeItemFromInventory(itemName);
                logEvent(`Você equipou a ${itemName}. Seu ataque aumentou!`, 'success');
                updateAllUI();
            } else {
                logEvent(`Você não pode usar ${itemName} agora.`, 'info');
            }
        }

        // --- Lógica de Combate ---

        /**
         * Inicia um combate com um inimigo.
         * @param {Object} enemy - O objeto do inimigo.
         */
        function enterCombat(enemy) {
            gameData.inCombat = true;
            // Clona o inimigo para não alterar o objeto original na gameData.enemies
            gameData.currentEnemy = { ...enemy }; 
            logEvent(`Você encontrou um(a) ${enemy.name}! Prepare-se para a batalha!`, 'combat');
            enemySprite.src = enemy.image;
            playerSprite.src = gameData.player.image; // Garante que o sprite do jogador esteja visível
            updateAllUI();
        }

        /** Encerra o combate. */
        function endCombat() {
            gameData.inCombat = false;
            gameData.currentEnemy = null;
            updateAllUI();
            logEvent("O combate terminou.", 'info');
        }

        /** Lógica de ataque do jogador. */
        function playerAttack() {
            if (!gameData.inCombat) return;

            const player = gameData.player;
            const enemy = gameData.currentEnemy;
            
            const totalPlayerAttack = player.attack + (player.equippedWeapon?.attackBonus || 0);
            let damage = Math.max(1, totalPlayerAttack - enemy.defense); // Garante no mínimo 1 de dano
            
            enemy.hp -= damage;
            logEvent(`${player.name} atacou ${enemy.name} causando ${damage} de dano!`, 'player_damage');
            
            if (enemy.hp <= 0) {
                enemy.hp = 0; // Garante que a HP não seja negativa na UI
                logEvent(`${enemy.name} foi derrotado!`, 'success');
                gameData.player.monstersSlain++; // Incrementa o contador de monstros abatidos
                // Ganho de XP e ouro baseado no nível do inimigo e se é um boss
                gainXp(enemy.level * 20 + (enemy.name.includes("Boss") || enemy.name.includes("Dragão") || enemy.name.includes("Cavaleiro da Morte") ? 100 : 0)); 
                gameData.player.gold += enemy.level * 5 + Math.floor(Math.random() * 10) + (enemy.name.includes("Ogro Zumbi") ? 25 : 0) + (enemy.name.includes("Dragão Vermelho Jovem") ? 500 : 0); 
                logEvent(`Você encontrou ${enemy.level * 5 + Math.floor(Math.random() * 10) + (enemy.name.includes("Ogro Zumbi") ? 25 : 0) + (enemy.name.includes("Dragão Vermelho Jovem") ? 500 : 0)} de ouro.`, 'info');
                handleLoot(enemy); // Lida com o saque do inimigo
                endCombat();
                checkBossSpawn(); // Verifica se é hora de aparecer um boss
            } else {
                // Se o inimigo ainda estiver vivo, é a vez dele atacar
                setTimeout(enemyTurn, 700); // Pequeno atraso para a mensagem do jogador aparecer
            }
            updateAllUI();
        }

        /** Lógica de ataque do inimigo. */
        function enemyTurn() {
            if (!gameData.inCombat) return;

            const player = gameData.player;
            const enemy = gameData.currentEnemy;

            let damage = Math.max(1, enemy.attack - player.defense); // Garante no mínimo 1 de dano
            player.hp -= damage;
            logEvent(`${enemy.name} atacou ${player.name} causando ${damage} de dano!`, 'enemy_damage');

            if (player.hp <= 0) {
                player.hp = 0; // Garante que a HP não seja negativa na UI
                logEvent("Você foi derrotado! Fim de jogo.", 'important');
                showModal("Você foi derrotado! Tente novamente.", () => location.reload()); // Reinicia o jogo
            }
            updateAllUI();
        }

        /**
         * Usa a habilidade especial do jogador com base em sua classe.
         */
        function useAbility() {
            const player = gameData.player;
            const enemy = gameData.currentEnemy;

            if (!gameData.inCombat) {
                logEvent("Você só pode usar habilidades em combate.", 'info');
                return;
            }

            logEvent(`${player.name} usou ${player.ability}!`, 'info');

            switch (player.class) {
                case 'knight':
                    // Cavaleiro: Aumenta a defesa temporariamente
                    const defenseBoost = 5 + Math.floor(player.level / 2);
                    const originalDefense = player.defense;
                    player.defense += defenseBoost;
                    logEvent(`Sua defesa aumentou em ${defenseBoost} por um turno!`, 'success');
                    updateAllUI();
                    // O efeito dura apenas um turno do inimigo
                    setTimeout(() => {
                        if (gameData.inCombat) { // Assegura que o efeito só se reverte se ainda estiver em combate
                            player.defense = originalDefense;
                            logEvent("O efeito do Escudo Imbatível passou.", 'info');
                            updateAllUI();
                        }
                    }, 800); // Pequeno atraso para o log aparecer antes do inimigo atacar
                    break;
                case 'rogue':
                    // Ladino: Grande dano crítico
                    let stealthDamage = Math.floor((player.attack + (player.equippedWeapon?.attackBonus || 0)) * 1.8); // 180% de dano
                    enemy.hp -= stealthDamage;
                    logEvent(`Seu Ataque Furtivo causou ${stealthDamage} de dano crítico!`, 'player_damage');
                    break;
                case 'mage':
                    // Mago: Dano mágico alto
                    let arcaneDamage = Math.floor((player.attack + (player.equippedWeapon?.attackBonus || 0)) * 1.5); // 150% de dano mágico
                    enemy.hp -= arcaneDamage;
                    logEvent(`Sua Tempestade Arcana atingiu ${enemy.name} causando ${arcaneDamage} de dano mágico!`, 'player_damage');
                    break;
                default:
                    logEvent("Esta classe não tem uma habilidade definida.", 'error');
                    break;
            }
            // Após a habilidade, é a vez do inimigo (se ainda estiver vivo)
            if (gameData.currentEnemy.hp <= 0) {
                 gameData.currentEnemy.hp = 0;
                 logEvent(`${gameData.currentEnemy.name} foi derrotado!`, 'success');
                 gameData.player.monstersSlain++;
                 gainXp(gameData.currentEnemy.level * 20 + (gameData.currentEnemy.name.includes("Boss") || gameData.currentEnemy.name.includes("Dragão") || gameData.currentEnemy.name.includes("Cavaleiro da Morte") ? 100 : 0));
                 gameData.player.gold += gameData.currentEnemy.level * 5 + Math.floor(Math.random() * 10) + (gameData.currentEnemy.name.includes("Ogro Zumbi") ? 25 : 0) + (gameData.currentEnemy.name.includes("Dragão Vermelho Jovem") ? 500 : 0);
                 logEvent(`Você encontrou ${gameData.currentEnemy.level * 5 + Math.floor(Math.random() * 10) + (gameData.currentEnemy.name.includes("Ogro Zumbi") ? 25 : 0) + (gameData.currentEnemy.name.includes("Dragão Vermelho Jovem") ? 500 : 0)} de ouro.`, 'info');
                 handleLoot(gameData.currentEnemy);
                 endCombat();
                 checkBossSpawn();
            } else {
                setTimeout(enemyTurn, 700);
            }
            updateAllUI();
        }

        /** Tenta fugir do combate. */
        function fleeCombat() {
            if (!gameData.inCombat) return;

            // Chance de fuga (e.g., 50%)
            const fleeChance = 0.5;
            if (Math.random() < fleeChance) {
                logEvent("Você conseguiu fugir do combate!", 'success');
                endCombat();
            } else {
                logEvent("Você falhou em fugir!", 'combat');
                setTimeout(enemyTurn, 700); // Inimigo ataca após a falha de fuga
            }
            updateAllUI();
        }

        /**
         * Lida com o saque de itens após derrotar um inimigo.
         * @param {Object} defeatedEnemy - O inimigo derrotado.
         */
        function handleLoot(defeatedEnemy) {
            if (defeatedEnemy.lootTable && defeatedEnemy.lootTable.length > 0) {
                defeatedEnemy.lootTable.forEach(drop => {
                    if (Math.random() < drop.chance) {
                        addItemToInventory(drop.item);
                    }
                });
            } else {
                logEvent(`${defeatedEnemy.name} não deixou nenhum item.`, 'info');
            }
        }

        /** Simula a exploração da caverna, podendo encontrar um inimigo. */
        function explore() {
            logEvent("Você explora as profundezas da caverna...", 'info');
            // Filtra inimigos: exclui bosses e inimigos de alto nível para a exploração normal
            const enemiesList = Object.values(gameData.enemies).filter(e => 
                !e.name.includes("Boss") && 
                !e.name.includes("Dragão") && // Exclui o Dragão Vermelho Jovem
                !e.name.includes("Cavaleiro da Morte") // Exclui o Cavaleiro da Morte Menor
            );
            
            // 70% de chance de encontrar um inimigo, 30% de chance de não encontrar nada
            if (Math.random() < 0.7) {
                const randomEnemy = enemiesList[Math.floor(Math.random() * enemiesList.length)];
                enterCombat(randomEnemy);
            } else {
                logEvent("Você não encontrou nada interessante.", 'info');
            }
        }

        /** Verifica se as condições para o surgimento de um chefe foram atendidas. */
        function checkBossSpawn() {
            const player = gameData.player;
            // Exemplo: Boss da Caverna aparece após 5 monstros comuns abatidos
            if (player.monstersSlain >= 5 && !gameData.enemies.caveBoss.isSpawned) {
                logEvent("Uma presença sombria e poderosa se aproxima!", 'important');
                showModal("Você sente um tremor na caverna... Algo GRANDE está vindo!", () => {
                    enterCombat(gameData.enemies.caveBoss);
                    gameData.enemies.caveBoss.isSpawned = true; // Marca o boss como "spawnado"
                });
            }
        }

        // --- Event Listeners ---
        startGameBtn.addEventListener('click', startGame);
        exploreButton.addEventListener('click', explore);
        attackButton.addEventListener('click', playerAttack);
        abilityButton.addEventListener('click', useAbility);
        fleeButton.addEventListener('click', fleeCombat);

        // Inicializa a UI ao carregar
        updateAllUI();
    </script>
</body>
</html>
