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
            font-family: 'Press Start 2P', cursive; /* Aplica a fonte pixelada */
            background-color: #2c3e50; /* Cor de fundo escura */
            color: #ecf0f1; /* Cor do texto principal */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
            box-sizing: border-box;
            margin: 0; /* Reset default margin */
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

        h1,
        h2,
        h3 {
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
            display: inline-block; /* Alterado para inline-block para melhor controle de largura */
            width: auto; /* Ajusta a largura ao conteúdo */
            text-align: center;
            image-rendering: pixelated; /* Ensures pixel art fonts render crisply */
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

        .pixel-button.full-width {
            display: block;
            width: 100%;
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
            box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.3); /* Sombra interna para efeito de profundidade */
        }

        .game-log {
            height: 150px; /* Altura ajustável conforme necessidade */
            overflow-y: auto;
            background-color: #1a242f;
            border: 2px solid #000;
            padding: 0.75rem;
            font-size: 0.8rem;
            line-height: 1.4;
            color: #95a5a6; /* Cor do texto do log */
            border-radius: 5px;
            box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.5);
        }

        .game-log p {
            margin-bottom: 0.25rem;
        }
        .game-log p.text-yellow-400 { color: #f1c40f; }
        .game-log p.text-red-400 { color: #e74c3c; }
        .game-log p.text-green-400 { color: #2ecc71; }


        .modal {
            display: none; /* Escondido por padrão */
            position: absolute; /* Posição absoluta dentro do contêiner do jogo */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #34495e;
            border: 4px solid #1a242f;
            border-radius: 8px;
            padding: 1.5rem 2rem; /* Ajustado padding */
            z-index: 100;
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.7);
            text-align: center;
            width: 90%; /* Largura do modal */
            max-width: 600px; /* Largura máxima do modal */
        }
        
        .modal h2 {
            font-size: 1.2rem; /* Ajustado tamanho da fonte do título do modal */
        }

        .modal-content button {
            margin-top: 1rem;
        }

        /* Estilos para tabelas */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
            font-size: 0.8rem; /* Reduzido para caber melhor no modal */
        }

        th,
        td {
            border: 1px solid #555;
            padding: 0.4rem; /* Reduzido padding */
            text-align: left;
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
            gap: 1rem; /* Espaçamento entre as seções principais da tela do jogo */
        }

        .stats-area { /* Novo container para stats do jogador e inimigo */
            display: flex;
            flex-direction: column; /* Empilha em telas pequenas */
            gap: 1rem;
        }

        @media (min-width: 768px) { /* md breakpoint de Tailwind */
            .stats-area {
                flex-direction: row; /* Lado a lado em telas médias e maiores */
            }
        }

        .player-panel, .enemy-panel {
            flex: 1; /* Permite que os painéis dividam o espaço */
        }

        .player-stats,
        .enemy-info {
            display: flex;
            flex-wrap: wrap; /* Permite que os itens quebrem linha se não couberem */
            gap: 0.5rem 1rem; /* Espaçamento entre itens e linhas */
            justify-content: center; /* Centraliza os itens quando quebram linha */
            font-size: 0.85rem; /* Levemente ajustado */
        }

        .player-stats span,
        .enemy-info span {
            background-color: #1a242f;
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
            white-space: nowrap; /* Evita quebra de linha dentro do span */
        }
        
        .log-panel { /* Container para o game log */
            /* O game-log em si já tem estilos */
        }

        .actions-inventory-area { /* Novo container para ações e mochila */
            display: flex;
            flex-direction: column; /* Empilha em telas pequenas */
            gap: 1rem;
        }

        @media (min-width: 768px) { /* md breakpoint de Tailwind */
            .actions-inventory-area {
                flex-direction: row; /* Lado a lado em telas médias e maiores */
            }
        }

        .actions-panel {
            flex: 2; /* Painel de ações ocupa mais espaço */
        }
        .inventory-panel {
            flex: 1; /* Painel da mochila ocupa menos espaço */
        }

        .game-actions {
            display: grid; /* Usando grid para melhor controle dos botões */
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); /* Colunas responsivas */
            gap: 0.75rem; /* Espaçamento entre botões */
        }

        .game-actions .pixel-button {
            width: 100%; /* Botões ocupam toda a largura da célula do grid */
            padding: 0.6rem 0.5rem; /* Ajuste no padding para caber melhor */
            font-size: 0.8rem; /* Ajuste no tamanho da fonte */
        }

        /* Estilos para a mochila */
        .inventory {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            max-height: 150px; /* Altura máxima para a mochila, com scroll se necessário */
            overflow-y: auto;
             background-color: #1a242f; /* Added background to inventory for consistency */
            border: 1px solid #555; /* Added border */
            border-radius: 4px; /* Added border radius */
            padding: 0.5rem; /* Added padding */
        }

        .inventory-item {
            background-color: #2c3e50; /* Changed for better contrast */
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.8rem;
            color: #ecf0f1; /* Ensure text is visible */
        }
         .inventory p.empty-inventory-message { /* Specific class for the empty message */
            font-size: 0.8rem;
            text-align: center;
            padding: 0.5rem;
            color: #95a5a6;
        }


        /* Melhorias para o modal de seleção de classe */
        #class-selection-modal .modal-content {
            max-height: 80vh; /* Altura máxima para o conteúdo do modal */
            overflow-y: auto; /* Adiciona scroll se o conteúdo exceder */
        }
        #class-selection-modal #class-options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 0.75rem;
            margin-bottom: 1rem;
        }
        #class-selection-modal #class-options .pixel-button {
            width: 100%;
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
                <div id="class-options" class="mb-4">
                    </div>
                <button id="modal-class-ok-button" class="pixel-button full-width" disabled>Confirmar Classe</button>
                <h3 class="mt-6 mb-2 text-base">Tabela de Classes e Armas (Referência D&D)</h3>
                <div class="overflow-x-auto"> <table>
                        <thead>
                            <tr>
                                <th>Classe</th>
                                <th>Armas Simples</th>
                                <th>Armas Marciais</th>
                                <th>Observações</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr><td>Bárbaro</td><td>✔️</td><td>✔️</td><td>Usa quase todas as armas</td></tr>
                            <tr><td>Bardo</td><td>✔️</td><td>Poucas</td><td>Espada curta, florete, rapieira e besta leve</td></tr>
                            <tr><td>Clérigo</td><td>✔️</td><td>❌</td><td>Focado em armas simples</td></tr>
                            <tr><td>Druida</td><td>✔️ (parcial)</td><td>❌</td><td>Só armas que não sejam de metal</td></tr>
                            <tr><td>Guerreiro</td><td>✔️</td><td>✔️</td><td>Usa todas as armas</td></tr>
                            <tr><td>Monge</td><td>✔️ (parcial)</td><td>❌</td><td>Armas simples e armas de monge</td></tr>
                            <tr><td>Paladino</td><td>✔️</td><td>✔️</td><td>Usa todas as armas</td></tr>
                            <tr><td>Patrulheiro</td><td>✔️</td><td>✔️</td><td>Usa todas as armas</td></tr>
                            <tr><td>Ladino</td><td>✔️</td><td>Poucas</td><td>Espada curta, florete, rapieira e besta de mão</td></tr>
                            <tr><td>Feiticeiro</td><td>✔️ (básico)</td><td>❌</td><td>Adaga, bastão, funda, besta leve, dardo</td></tr>
                            <tr><td>Bruxo</td><td>✔️ (básico)</td><td>❌</td><td>Adaga, bastão, funda, besta leve, dardo</td></tr>
                            <tr><td>Mago</td><td>✔️ (básico)</td><td>❌</td><td>Adaga, bastão, funda, besta leve, dardo</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <div id="game-screen">
            <h2 class="text-2xl">Aventura em Andamento!</h2>

            <div class="stats-area">
                <div class="panel player-panel">
                    <h3 class="text-lg">Seu Herói</h3>
                    <div id="player-stats" class="player-stats">
                        </div>
                </div>
                <div class="panel enemy-panel">
                    <h3 class="text-lg">Inimigo Atual</h3>
                    <div id="enemy-info" class="enemy-info">
                        <span id="enemy-name">Nenhum Inimigo</span>
                        <span id="enemy-hp">HP: --</span>
                    </div>
                </div>
            </div>

            <div class="panel log-panel">
                <h3 class="text-lg">Registro de Eventos</h3>
                <div id="game-log" class="game-log">
                    <p>Bem-vindo à Dungeons Cave! Escolha uma classe para começar.</p>
                </div>
            </div>
            
            <div class="actions-inventory-area">
                <div class="panel actions-panel">
                    <h3 class="text-lg">Ações</h3>
                    <div id="game-actions" class="game-actions">
                        <button id="explore-button" class="pixel-button">Explorar</button>
                        <button id="attack-button" class="pixel-button" disabled>Atacar</button>
                        <button id="ability-button" class="pixel-button" disabled>Habilidade</button> 
                        <button id="flee-button" class="pixel-button" disabled>Fugir</button>
                    </div>
                </div>
                <div class="panel inventory-panel">
                    <h3 class="text-lg">Mochila</h3>
                    <div id="inventory" class="inventory">
                        <p class="empty-inventory-message">Sua mochila está vazia.</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Dados do jogo
        const gameData = {
            classes: {
                knight: { name: "Cavaleiro", description: "Tanque, defesa alta, espada e escudo.", ability: "Escudo Imbatível", baseHp: 120, baseAttack: 15, baseDefense: 10 },
                rogue: { name: "Ladino", description: "Ágil, furtivo, dano crítico alto.", ability: "Ataque Furtivo", baseHp: 80, baseAttack: 25, baseDefense: 5 }, // Changed ability name for clarity
                mage: { name: "Mago", description: "Mestre dos elementos, dano mágico em área.", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3 },
                cleric: { name: "Clérigo", description: "Suporte, cura, buffs e dano sagrado.", ability: "Bênção Divina", baseHp: 90, baseAttack: 20, baseDefense: 7 },
                barbarian: { name: "Bárbaro", description: "Selvagem, dano físico bruto, pouca defesa.", ability: "Fúria Insana", baseHp: 110, baseAttack: 28, baseDefense: 4 },
                hunter: { name: "Patrulheiro", description: "Perito em combate à distância, usa arcos.", ability: "Tiro Certeiro", baseHp: 85, baseAttack: 22, baseDefense: 6 }
            },
            selectedClassKey: null,
            selectedClass: null, // Will store the full class object
            player: {
                name: "", // Class name of the player
                hp: 0,
                maxHp: 0,
                attack: 0,
                defense: 0,
                abilityName: "",
                inventory: [],
                specialAbilityUsedThisCombat: false 
            },
            currentEnemy: null,
            // Basic enemy templates
            enemies: {
                goblin: { name: "Goblin Ardiloso", hp: 30, maxHp: 30, attack: 8, defense: 2, gold: 5, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.3 }] },
                orc: { name: "Orc Brutal", hp: 50, maxHp: 50, attack: 12, defense: 4, gold: 10, lootTable: [{ item: "Adaga Enferrujada", chance: 0.2 }] },
                slime: { name: "Slime Ácido", hp: 20, maxHp: 20, attack: 6, defense: 1, gold: 3, lootTable: [{ item: "Gosma Pegajosa", chance: 0.5 }] }
            },
            items: {
                "Poção de Cura Pequena": { type: "potion", effect: "heal", power: 20, description: "Restaura 20 HP." },
                "Adaga Enferrujada": { type: "weapon", attackBonus: 2, description: "Uma adaga velha, mas ainda pontuda." },
                "Gosma Pegajosa": { type: "junk", description: "Uma gosma nojenta. Talvez sirva para algo?" }
            }
        };

        // Elementos do DOM
        const startGameBtn = document.getElementById('start-game-button');
        const classSelectionModal = document.getElementById('class-selection-modal');
        const classOptionsDiv = document.getElementById('class-options');
        const confirmClassBtn = document.getElementById('modal-class-ok-button');
        const gameScreen = document.getElementById('game-screen');
        const startScreen = document.getElementById('start-screen');
        
        const playerStatsDiv = document.getElementById('player-stats');
        const enemyNameSpan = document.getElementById('enemy-name');
        const enemyHpSpan = document.getElementById('enemy-hp');
        
        const exploreButton = document.getElementById('explore-button');
        const attackButton = document.getElementById('attack-button');
        const abilityButton = document.getElementById('ability-button');
        const fleeButton = document.getElementById('flee-button');
        
        const inventoryDiv = document.getElementById('inventory');
        const gameLogDiv = document.getElementById('game-log');

        const gameModal = document.getElementById('game-modal');
        const modalMessage = document.getElementById('modal-message');
        const modalOkBtn = document.getElementById('modal-ok-button');

        // --- Funções do Jogo ---

        // Adiciona uma mensagem ao log do jogo
        function logEvent(message, type = 'normal') {
            const p = document.createElement('p');
            p.textContent = message;
            if (type === 'important') p.classList.add('text-yellow-400');
            if (type === 'combat') p.classList.add('text-red-400'); // Player/enemy actions
            if (type === 'player_damage') p.classList.add('text-red-400', 'font-bold'); // Damage to player
            if (type === 'enemy_damage') p.classList.add('text-orange-400'); // Damage to enemy (using orange for distinction)
            if (type === 'heal') p.classList.add('text-green-400'); // Healing
            if (type === 'success') p.classList.add('text-green-400'); // General success
            if (type === 'info') p.classList.add('text-blue-300'); // Neutral info
            if (type === 'loot') p.classList.add('text-purple-400'); // Loot messages

            // For Tailwind JIT, ensure these classes are recognized if not used elsewhere
            // text-yellow-400 text-red-400 text-green-400 text-orange-400 text-blue-300 text-purple-400 font-bold
            
            gameLogDiv.appendChild(p);
            gameLogDiv.scrollTop = gameLogDiv.scrollHeight; // Auto-scroll para a última mensagem
        }

        // Mostra o modal genérico
        function showModal(message, onOkCallback) {
            modalMessage.textContent = message;
            gameModal.style.display = 'block';
            
            // Remove previous listener to avoid multiple triggers if modal is reused
            const newOkBtn = modalOkBtn.cloneNode(true);
            modalOkBtn.parentNode.replaceChild(newOkBtn, modalOkBtn);
            
            // Assign new listener
            newOkBtn.addEventListener('click', () => {
                gameModal.style.display = 'none';
                if (typeof onOkCallback === 'function') {
                    onOkCallback();
                }
            });
        }
        
        // Abrir modal de seleção de classe
        startGameBtn.addEventListener('click', () => {
            classSelectionModal.style.display = 'block';
            confirmClassBtn.disabled = true;
            gameData.selectedClassKey = null;
            classOptionsDiv.innerHTML = ''; // Limpa opções anteriores

            // Criar botões para cada classe
            for (const key in gameData.classes) {
                if (gameData.classes.hasOwnProperty(key)) {
                    const classInfo = gameData.classes[key];
                    const btn = document.createElement('button');
                    btn.className = 'pixel-button'; 
                    btn.textContent = classInfo.name;
                    btn.title = classInfo.description; 
                    btn.dataset.classKey = key; 
                    btn.onclick = () => selectClass(key, btn);
                    classOptionsDiv.appendChild(btn);
                }
            }
        });

        // Função para selecionar uma classe
        function selectClass(classKey, button) {
            gameData.selectedClassKey = classKey;
            
            Array.from(classOptionsDiv.children).forEach(btn => {
                btn.style.borderColor = '#c0392b'; 
                btn.style.backgroundColor = '#e74c3c'; 
                btn.classList.remove('border-yellow-400', 'bg-yellow-600'); // Using Tailwind for consistency
            });
            
            button.style.borderColor = '#f1c40f'; 
            button.style.backgroundColor = '#f39c12';
            // Alternatively, use Tailwind classes for selection:
            // button.classList.add('border-yellow-400', 'bg-yellow-600');

            confirmClassBtn.disabled = false; // Habilita o botão de confirmação
        }

        // Função para atualizar a UI de stats do jogador
        function updatePlayerStatsUI() {
            if (!gameData.selectedClass) return; // Sai se nenhuma classe foi selecionada ainda

            playerStatsDiv.innerHTML = ''; // Limpa stats anteriores

            const stats = [
                `Classe: ${gameData.player.name}`,
                `HP: ${gameData.player.hp} / ${gameData.player.maxHp}`,
                `Ataque: ${gameData.player.attack}`,
                `Defesa: ${gameData.player.defense}`,
                `Habilidade: ${gameData.player.abilityName}`
            ];

            stats.forEach(statText => {
                const span = document.createElement('span');
                span.textContent = statText;
                playerStatsDiv.appendChild(span);
            });
        }
        
        // Função para atualizar a UI da mochila
        function updateInventoryUI() {
            inventoryDiv.innerHTML = ''; // Limpa a mochila
            if (gameData.player.inventory.length === 0) {
                const p = document.createElement('p');
                p.className = 'empty-inventory-message'; // Use specific class
                p.textContent = 'Sua mochila está vazia.';
                inventoryDiv.appendChild(p);
            } else {
                gameData.player.inventory.forEach((item, index) => {
                    const itemDiv = document.createElement('div');
                    itemDiv.className = 'inventory-item';
                    itemDiv.textContent = `${item.name} (${item.quantity > 1 ? 'x' + item.quantity : '1'})`;
                    
                    // Add use button for potions or other usable items
                    const itemDetails = gameData.items[item.name];
                    if (itemDetails && itemDetails.type === 'potion') {
                        const useButton = document.createElement('button');
                        useButton.textContent = 'Usar';
                        useButton.className = 'pixel-button text-xs py-1 px-2 ml-2'; // Smaller button
                        useButton.style.fontSize = '0.6rem'; // Even smaller font for button
                        useButton.onclick = () => useItem(index);
                        itemDiv.appendChild(useButton);
                    }
                    inventoryDiv.appendChild(itemDiv);
                });
            }
        }


        // Confirmar seleção de classe e iniciar o jogo
        confirmClassBtn.addEventListener('click', () => {
            if (!gameData.selectedClassKey) {
                showModal("Por favor, selecione uma classe.");
                return;
            }

            gameData.selectedClass = gameData.classes[gameData.selectedClassKey];
            const classInfo = gameData.selectedClass;

            // Inicializa o jogador
            gameData.player.name = classInfo.name;
            gameData.player.hp = classInfo.baseHp;
            gameData.player.maxHp = classInfo.baseHp;
            gameData.player.attack = classInfo.baseAttack;
            gameData.player.defense = classInfo.baseDefense;
            gameData.player.abilityName = classInfo.ability;
            gameData.player.inventory = []; // Começa com mochila vazia
            gameData.player.specialAbilityUsedThisCombat = false;

            updatePlayerStatsUI();
            updateInventoryUI();
            updateEnemyUI(); // Ensure enemy UI is reset

            // Transição de telas
            classSelectionModal.style.display = 'none';
            startScreen.style.display = 'none';
            gameScreen.style.display = 'flex'; // Usa flex como definido no CSS

            logEvent(`Você escolheu a classe: ${classInfo.name}. Que sua jornada seja épica!`, 'important');

            // Configura estado inicial dos botões de ação
            exploreButton.disabled = false;
            attackButton.disabled = true;
            abilityButton.disabled = true;
            fleeButton.disabled = true;
        });
        
        // --- Lógica de Combate e Exploração (Básica) ---

        function updateEnemyUI() {
            if (gameData.currentEnemy) {
                enemyNameSpan.textContent = gameData.currentEnemy.name;
                enemyHpSpan.textContent = `HP: ${gameData.currentEnemy.hp}`;
                attackButton.disabled = false;
                abilityButton.disabled = gameData.player.specialAbilityUsedThisCombat;
                fleeButton.disabled = false;
                exploreButton.disabled = true;
            } else {
                enemyNameSpan.textContent = "Nenhum Inimigo";
                enemyHpSpan.textContent = "HP: --";
                attackButton.disabled = true;
                abilityButton.disabled = true;
                fleeButton.disabled = true;
                exploreButton.disabled = false;
            }
        }

        function spawnRandomEnemy() {
            if (gameData.currentEnemy) return; // Don't spawn if one is already active

            const enemyKeys = Object.keys(gameData.enemies);
            const randomKey = enemyKeys[Math.floor(Math.random() * enemyKeys.length)];
            const enemyTemplate = gameData.enemies[randomKey];
            
            // Create a copy of the enemy template for the current encounter
            gameData.currentEnemy = { ...enemyTemplate }; 
            // Reset HP to maxHP for the new spawn
            gameData.currentEnemy.hp = enemyTemplate.maxHp;


            logEvent(`Um ${gameData.currentEnemy.name} selvagem aparece!`, 'important');
            gameData.player.specialAbilityUsedThisCombat = false; // Reset ability use for new combat
            updateEnemyUI();
        }

        exploreButton.addEventListener('click', () => {
            logEvent("Você explora os corredores sombrios da caverna...", 'info');
            // Chance to find an enemy, or an item, or nothing
            const roll = Math.random();
            if (roll < 0.6) { // 60% chance to find an enemy
                spawnRandomEnemy();
            } else if (roll < 0.8) { // 20% chance to find a random item (simple for now)
                const itemsKeys = Object.keys(gameData.items);
                const randomItemKey = itemsKeys[Math.floor(Math.random() * itemsKeys.length)];
                if (gameData.items[randomItemKey].type !== 'junk') { // Avoid giving junk directly for now
                     addItemToInventory(randomItemKey, 1);
                     logEvent(`Você encontrou ${randomItemKey}!`, 'loot');
                } else {
                    logEvent("Você vasculha, mas não encontra nada de valor.", 'info');
                }
            } else { // 20% chance to find nothing
                logEvent("A área parece vazia por enquanto.", 'info');
            }
        });

        attackButton.addEventListener('click', handlePlayerAttack);
        abilityButton.addEventListener('click', handlePlayerAbility);
        fleeButton.addEventListener('click', handleFleeAttempt);

        function handlePlayerAttack() {
            if (!gameData.currentEnemy || gameData.player.hp <= 0) return;

            // Player attacks enemy
            let playerDamage = Math.max(1, gameData.player.attack - gameData.currentEnemy.defense);
            // Add some randomness to damage
            playerDamage = Math.floor(playerDamage * (Math.random() * 0.4 + 0.8)); // 80% to 120% of base damage
            
            gameData.currentEnemy.hp -= playerDamage;
            logEvent(`Você ataca ${gameData.currentEnemy.name} causando ${playerDamage} de dano.`, 'enemy_damage');

            if (gameData.currentEnemy.hp <= 0) {
                handleVictory();
                return;
            }
            updateEnemyUI();
            // Enemy attacks player (after a short delay for readability)
            setTimeout(handleEnemyAttack, 700);
        }
        
        function handlePlayerAbility() {
            if (!gameData.currentEnemy || gameData.player.hp <= 0 || gameData.player.specialAbilityUsedThisCombat) return;

            logEvent(`Você usa ${gameData.player.abilityName}!`, 'important');
            gameData.player.specialAbilityUsedThisCombat = true;
            abilityButton.disabled = true; // Disable after use in this combat

            // Basic ability effects (can be expanded greatly)
            let abilitySuccessful = true;
            switch (gameData.selectedClassKey) {
                case 'knight': // Escudo Imbatível: aumenta defesa temporariamente (efeito passivo no cálculo de dano inimigo por 1 turno)
                    gameData.player.defense += 5;
                    logEvent("Sua defesa aumenta temporariamente!", 'success');
                    setTimeout(() => { gameData.player.defense -= 5; logEvent("O efeito do Escudo Imbatível passou.", "info"); updatePlayerStatsUI(); }, 10000); // Lasts for 10s (approx 1-2 turns)
                    break;
                case 'rogue': // Ataque Furtivo: Causa dano extra
                    let rogueDamage = Math.max(1, Math.floor(gameData.player.attack * 1.5) - gameData.currentEnemy.defense);
                    rogueDamage = Math.floor(rogueDamage * (Math.random() * 0.4 + 0.8));
                    gameData.currentEnemy.hp -= rogueDamage;
                    logEvent(`Seu ataque furtivo causa ${rogueDamage} de dano extra!`, 'enemy_damage');
                    break;
                case 'mage': // Tempestade Arcana: Dano em área (aqui, dano alto no inimigo atual)
                    let mageDamage = Math.max(1, Math.floor(gameData.player.attack * 1.2 + 10) - gameData.currentEnemy.defense); // Higher base, less reliant on weapon
                    mageDamage = Math.floor(mageDamage * (Math.random() * 0.4 + 0.8));
                    gameData.currentEnemy.hp -= mageDamage;
                    logEvent(`Uma tempestade arcana atinge ${gameData.currentEnemy.name} causando ${mageDamage} de dano!`, 'enemy_damage');
                    break;
                case 'cleric': // Bênção Divina: Cura o jogador
                    const healAmount = Math.floor(gameData.player.maxHp * 0.3); // Cura 30% do Max HP
                    gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + healAmount);
                    logEvent(`Você se cura em ${healAmount} HP!`, 'heal');
                    updatePlayerStatsUI();
                    break;
                case 'barbarian': // Fúria Insana: Aumenta ataque, diminui defesa
                     gameData.player.attack += 7;
                     gameData.player.defense = Math.max(0, gameData.player.defense -3);
                     logEvent("Você entra em fúria! Ataque aumentado, defesa reduzida.", 'important');
                     updatePlayerStatsUI();
                     setTimeout(() => {
                        gameData.player.attack -= 7;
                        gameData.player.defense +=3; // Restore original defense reduction
                        logEvent("Sua fúria se acalma.", "info");
                        updatePlayerStatsUI();
                     }, 10000);
                    break;
                case 'hunter': // Tiro Certeiro: Chance alta de acerto crítico (dano dobrado)
                    let hunterDamage = gameData.player.attack;
                    if (Math.random() < 0.5) { // 50% chance of critical
                        hunterDamage *= 2;
                        logEvent("Tiro Crítico!", 'success');
                    }
                    hunterDamage = Math.max(1, hunterDamage - gameData.currentEnemy.defense);
                    hunterDamage = Math.floor(hunterDamage * (Math.random() * 0.4 + 0.8));
                    gameData.currentEnemy.hp -= hunterDamage;
                    logEvent(`Seu tiro certeiro causa ${hunterDamage} de dano!`, 'enemy_damage');
                    break;
                default:
                    logEvent("Sua habilidade não teve efeito aparente...", "info");
                    abilitySuccessful = false; // If no specific ability, don't proceed to enemy attack directly
                    break;
            }
            
            if (gameData.currentEnemy.hp <= 0) {
                handleVictory();
                return;
            }
            updateEnemyUI();

            if (abilitySuccessful && gameData.currentEnemy && gameData.currentEnemy.hp > 0) {
                 // Enemy attacks player (after a short delay for readability)
                setTimeout(handleEnemyAttack, 700);
            }
        }

        function handleEnemyAttack() {
            if (!gameData.currentEnemy || gameData.currentEnemy.hp <= 0 || gameData.player.hp <= 0) return;

            let enemyDamage = Math.max(1, gameData.currentEnemy.attack - gameData.player.defense);
            enemyDamage = Math.floor(enemyDamage * (Math.random() * 0.4 + 0.8)); // 80% to 120%
            
            gameData.player.hp -= enemyDamage;
            logEvent(`${gameData.currentEnemy.name} ataca você causando ${enemyDamage} de dano.`, 'player_damage');
            updatePlayerStatsUI();

            if (gameData.player.hp <= 0) {
                handleDefeat();
            }
        }
        
        function handleFleeAttempt() {
            if (!gameData.currentEnemy || gameData.player.hp <= 0) return;

            logEvent("Você tenta fugir...", 'info');
            if (Math.random() < 0.5) { // 50% chance de sucesso
                logEvent("Você conseguiu escapar!", 'success');
                gameData.currentEnemy = null;
                updateEnemyUI();
            } else {
                logEvent("Sua tentativa de fuga falhou!", 'player_damage');
                setTimeout(handleEnemyAttack, 700); // Inimigo ataca se a fuga falhar
            }
        }

        function handleVictory() {
            logEvent(`Você derrotou ${gameData.currentEnemy.name}!`, 'success');
            const goldFound = gameData.currentEnemy.gold || 0;
            if (goldFound > 0) {
                // For now, just log gold. Later, add to player inventory/stats.
                logEvent(`Você encontrou ${goldFound} moedas de ouro!`, 'loot');
            }

            // Check for loot
            if (gameData.currentEnemy.lootTable) {
                gameData.currentEnemy.lootTable.forEach(loot => {
                    if (Math.random() < loot.chance) {
                        addItemToInventory(loot.item, 1);
                        logEvent(`O inimigo derrubou: ${loot.item}!`, 'loot');
                    }
                });
            }

            gameData.currentEnemy = null;
            updateEnemyUI();
            // Potentially show a victory modal or offer next actions
            showModal("Você venceu o combate!", () => {
                exploreButton.disabled = false; // Re-enable exploration
            });
        }

        function handleDefeat() {
            logEvent("Você foi derrotado... A escuridão te consome.", 'important');
            gameData.player.hp = 0; // Ensure HP is 0
            updatePlayerStatsUI();
            // Disable all actions
            exploreButton.disabled = true;
            attackButton.disabled = true;
            abilityButton.disabled = true;
            fleeButton.disabled = true;
            
            showModal("FIM DE JOGO! Você foi derrotado. Tente novamente.", () => {
                // Reset game or redirect to start screen
                // For simplicity, just refresh the page to restart
                // Or, more gracefully:
                startScreen.style.display = 'flex';
                gameScreen.style.display = 'none';
                classSelectionModal.style.display = 'none';
                gameData.currentEnemy = null;
                gameData.player.inventory = [];
                logEvent("--- Novo Jogo ---", "important");
                // Clear log or keep it, depends on preference
                // gameLogDiv.innerHTML = '<p>Bem-vindo à Dungeons Cave! Escolha uma classe para começar.</p>';
            });
        }

        function addItemToInventory(itemName, quantity) {
            const itemDetails = gameData.items[itemName];
            if (!itemDetails) {
                console.error(`Item desconhecido: ${itemName}`);
                return;
            }

            const existingItem = gameData.player.inventory.find(i => i.name === itemName);
            if (existingItem) {
                existingItem.quantity += quantity;
            } else {
                gameData.player.inventory.push({ name: itemName, quantity: quantity });
            }
            updateInventoryUI();
        }
        
        function useItem(itemIndexInInventory) {
            const itemEntry = gameData.player.inventory[itemIndexInInventory];
            if (!itemEntry) return;

            const itemDetails = gameData.items[itemEntry.name];
            if (!itemDetails) return;

            if (itemDetails.type === 'potion' && itemDetails.effect === 'heal') {
                if (gameData.player.hp >= gameData.player.maxHp) {
                    logEvent("Sua vida já está cheia!", "info");
                    return;
                }
                gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + itemDetails.power);
                logEvent(`Você usou ${itemEntry.name} e recuperou ${itemDetails.power} HP.`, 'heal');
                updatePlayerStatsUI();
                
                itemEntry.quantity--;
                if (itemEntry.quantity <= 0) {
                    gameData.player.inventory.splice(itemIndexInInventory, 1);
                }
                updateInventoryUI();

                // If in combat, enemy might take a turn
                if (gameData.currentEnemy && gameData.currentEnemy.hp > 0) {
                    logEvent("Usar um item te deixa vulnerável!", "info");
                    setTimeout(handleEnemyAttack, 700);
                }

            } else {
                logEvent(`Você não pode usar ${itemEntry.name} agora.`, "info");
            }
        }


        // --- Inicialização ---
        // Garante que a mochila e stats do inimigo estejam no estado inicial correto na UI.
        updateInventoryUI();
        updateEnemyUI();

    </script>
</body>
</html>
