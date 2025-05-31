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
            /* margin-bottom: 1rem; */ /* Removido, o gap do flex container pai cuidará disso */
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
            /* margin-top: 1rem; */ /* Removido, o gap do flex container pai cuidará disso */
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
            flex-wrap: wrap;
            gap: 0.5rem 1rem; /* Espaçamento entre itens e linhas */
            justify-content: center;
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
            /* margin-top: 1rem; */ /* Removido, o gap do flex container pai cuidará disso */
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
            /* margin-top: 1rem; */ /* Removido */
            max-height: 150px; /* Altura máxima para a mochila, com scroll se necessário */
            overflow-y: auto;
        }

        .inventory-item {
            background-color: #1a242f;
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.8rem;
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
                        <button id="ability-button" class="pixel-button" disabled>Habilidade</button> <button id="flee-button" class="pixel-button" disabled>Fugir</button>
                    </div>
                </div>
                <div class="panel inventory-panel">
                    <h3 class="text-lg">Mochila</h3>
                    <div id="inventory" class="inventory">
                        <p class="text-sm text-gray-400 text-center p-2">Sua mochila está vazia.</p>
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
                rogue: { name: "Ladino", description: "Ágil, furtivo, dano crítico alto.", ability: "Desaparecer", baseHp: 80, baseAttack: 25, baseDefense: 5 },
                mage: { name: "Mago", description: "Mestre dos elementos, dano mágico em área.", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3 },
                cleric: { name: "Clérigo", description: "Suporte, cura, buffs e dano sagrado.", ability: "Bênção Divina", baseHp: 90, baseAttack: 20, baseDefense: 7 },
                barbarian: { name: "Bárbaro", description: "Selvagem, dano físico bruto, pouca defesa.", ability: "Fúria Insana", baseHp: 110, baseAttack: 28, baseDefense: 4 },
                hunter: { name: "Patrulheiro", description: "Perito em combate à distância, usa arcos.", ability: "Tiro Certeiro", baseHp: 85, baseAttack: 22, baseDefense: 6 }
            },
            selectedClassKey: null,
            selectedClass: null,
            // Adicionar estado do jogador
            player: {
                hp: 0,
                maxHp: 0,
                attack: 0,
                defense: 0,
                inventory: [],
                specialAbilityUsed: false
            },
            currentEnemy: null
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
            if (type === 'combat') p.classList.add('text-red-400');
            if (type === 'success') p.classList.add('text-green-400');
            gameLogDiv.appendChild(p);
            gameLogDiv.scrollTop = gameLogDiv.scrollHeight; // Auto-scroll para a última mensagem
        }

        // Mostra o modal genérico
        function showModal(message) {
            modalMessage.textContent = message;
            gameModal.style.display = 'block';
        }

        // Fecha o modal genérico
        modalOkBtn.addEventListener('click', () => {
            gameModal.style.display = 'none';
        });
        
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
                    btn.className = 'pixel-button'; // Usa a classe de botão existente
                    btn.textContent = classInfo.name;
                    btn.title = classInfo.description; // Tooltip com a descrição
                    btn.dataset.classKey = key; // Armazena a chave da classe no botão
                    btn.onclick = () => selectClass(key, btn);
                    classOptionsDiv.appendChild(btn);
                }
            }
        });

        // Função para selecionar uma classe
        function selectClass(classKey, button) {
            gameData.selectedClassKey = classKey;
            
            // Remove destaque de todos os botões de classe
            Array.from(classOptionsDiv.children).forEach(btn => {
                btn.style.borderColor = '#c0392b'; // Cor da borda padrão
                btn.style.backgroundColor = '#e74c3c'; // Cor de fundo padrão
            });
            // Destaque o botão selecionado
            button.style.borderColor = '#f1c40f'; // Cor da borda de destaque
            button.style.backgroundColor = '#f39c12'; // Cor de fundo de destaque

            confirmClassBtn.disabled = false; // Habilita o botão de confirmar
        }

        // Confirmar seleção da classe e iniciar jogo
        confirmClassBtn.addEventListener('click', () => {
            if (gameData.selectedClassKey) {
                gameData.selectedClass = gameData.classes[gameData.selectedClassKey];
                
                // Inicializar stats do jogador
                gameData.player.hp = gameData.selectedClass.baseHp;
                gameData.player.maxHp = gameData.selectedClass.baseHp;
                gameData.player.attack = gameData.selectedClass.baseAttack;
                gameData.player.defense = gameData.selectedClass.baseDefense;
                gameData.player.inventory = []; // Começa com mochila vazia
                gameData.player.specialAbilityUsed = false;

                classSelectionModal.style.display = 'none';
                startScreen.style.display = 'none';
                gameScreen.style.display = 'flex'; // Mostra a tela do jogo

                updatePlayerStatsUI();
                updateEnemyUI(); // Garante que a UI do inimigo esteja limpa
                updateInventoryUI();
                updateActionButtons();

                logEvent(`Você escolheu a classe: ${gameData.selectedClass.name}.`, 'important');
                logEvent("Clique em 'Explorar' para encontrar seu primeiro desafio!");
                showModal(`Classe escolhida: ${gameData.selectedClass.name}! Boa sorte, aventureiro!`);
            }
        });

        // Atualiza as estatísticas do jogador na UI
        function updatePlayerStatsUI() {
            if (gameData.selectedClass) {
                playerStatsDiv.innerHTML = `
                    <span>Classe: ${gameData.selectedClass.name}</span>
                    <span>HP: ${gameData.player.hp} / ${gameData.player.maxHp}</span>
                    <span>Ataque: ${gameData.player.attack}</span>
                    <span>Defesa: ${gameData.player.defense}</span>
                    <span>Habilidade: ${gameData.selectedClass.ability} ${gameData.player.specialAbilityUsed ? '(Usada)' : '(Disponível)'}</span>
                `;
            }
        }

        // Atualiza a UI do inimigo
        function updateEnemyUI() {
            if (gameData.currentEnemy) {
                enemyNameSpan.textContent = gameData.currentEnemy.name;
                enemyHpSpan.textContent = `HP: ${gameData.currentEnemy.hp}`;
            } else {
                enemyNameSpan.textContent = 'Nenhum Inimigo';
                enemyHpSpan.textContent = 'HP: --';
            }
        }
        
        // Atualiza a UI da mochila
        function updateInventoryUI() {
            inventoryDiv.innerHTML = ''; // Limpa a mochila
            if (gameData.player.inventory.length === 0) {
                const p = document.createElement('p');
                p.className = 'text-sm text-gray-400 text-center p-2';
                p.textContent = 'Sua mochila está vazia.';
                inventoryDiv.appendChild(p);
            } else {
                gameData.player.inventory.forEach(item => {
                    const itemDiv = document.createElement('div');
                    itemDiv.className = 'inventory-item';
                    itemDiv.textContent = `${item.name} (${item.type})`;
                    // Poderia adicionar botão de usar item aqui
                    inventoryDiv.appendChild(itemDiv);
                });
            }
        }

        // Atualiza o estado dos botões de ação
        function updateActionButtons() {
            const inCombat = !!gameData.currentEnemy;
            exploreButton.disabled = inCombat;
            attackButton.disabled = !inCombat;
            abilityButton.disabled = !inCombat || gameData.player.specialAbilityUsed;
            fleeButton.disabled = !inCombat;
        }

        // Lógica de Exploração (placeholder)
        exploreButton.addEventListener('click', () => {
            logEvent("Você explora a masmorra...");
            // Simular encontro com inimigo
            const enemies = [
                { name: "Goblin Fraco", hp: 30, attack: 8, defense: 2, gold: 10 },
                { name: "Morcego Gigante", hp: 25, attack: 10, defense: 1, gold: 5 },
                { name: "Esqueleto", hp: 40, attack: 12, defense: 3, gold: 15 }
            ];
            gameData.currentEnemy = {...enemies[Math.floor(Math.random() * enemies.length)]};
            logEvent(`Um ${gameData.currentEnemy.name} aparece!`, 'combat');
            updateEnemyUI();
            updateActionButtons();
        });

        // Lógica de Ataque (placeholder)
        attackButton.addEventListener('click', () => {
            if (!gameData.currentEnemy) return;

            // Jogador ataca
            let playerDamage = Math.max(1, gameData.player.attack - gameData.currentEnemy.defense + Math.floor(Math.random() * 6 - 3)); // Adiciona um pouco de variação
            gameData.currentEnemy.hp = Math.max(0, gameData.currentEnemy.hp - playerDamage);
            logEvent(`Você ataca o ${gameData.currentEnemy.name} causando ${playerDamage} de dano. HP do inimigo: ${gameData.currentEnemy.hp}`, 'combat');

            if (gameData.currentEnemy.hp <= 0) {
                logEvent(`Você derrotou o ${gameData.currentEnemy.name}!`, 'success');
                // Adicionar recompensas (ex: ouro, itens)
                const goldFound = gameData.currentEnemy.gold || 0;
                if (goldFound > 0) {
                     logEvent(`Você encontrou ${goldFound} moedas de ouro!`);
                     // Adicionar lógica para guardar ouro
                }
                gameData.currentEnemy = null;
                updatePlayerStatsUI(); // Para caso a habilidade tenha sido usada
                updateEnemyUI();
                updateActionButtons();
                return;
            }

            // Inimigo ataca
            let enemyDamage = Math.max(1, gameData.currentEnemy.attack - gameData.player.defense + Math.floor(Math.random() * 4 - 2));
            gameData.player.hp = Math.max(0, gameData.player.hp - enemyDamage);
            logEvent(`${gameData.currentEnemy.name} ataca você causando ${enemyDamage} de dano. Seu HP: ${gameData.player.hp}`, 'combat');
            
            updatePlayerStatsUI();
            updateEnemyUI();

            if (gameData.player.hp <= 0) {
                logEvent("Você foi derrotado...", 'important');
                showModal("Fim de Jogo! Você foi derrotado. Tente novamente!");
                // Resetar jogo ou ir para tela de game over
                startScreen.style.display = 'flex';
                gameScreen.style.display = 'none';
                gameLogDiv.innerHTML = '<p>Bem-vindo à Dungeons Cave! Escolha uma classe para começar.</p>'; // Limpa o log
            }
        });
        
        // Lógica da Habilidade Especial (placeholder)
        abilityButton.addEventListener('click', () => {
            if (!gameData.currentEnemy || gameData.player.specialAbilityUsed) return;

            logEvent(`Você usa sua habilidade especial: ${gameData.selectedClass.ability}!`, 'important');
            gameData.player.specialAbilityUsed = true;

            // Efeitos da habilidade (exemplos)
            switch(gameData.selectedClassKey) {
                case 'knight': // Escudo Imbatível - Aumenta defesa temporariamente ou bloqueia próximo ataque
                    gameData.player.defense += 10;
                    logEvent("Sua defesa aumentou drasticamente por um momento!");
                    setTimeout(() => { gameData.player.defense -= 10; updatePlayerStatsUI(); logEvent("O efeito do Escudo Imbatível passou."); }, 10000); // Dura 10s
                    break;
                case 'rogue': // Desaparecer - Chance de evitar o próximo ataque ou garantir crítico
                     let enemyDamageReduction = gameData.currentEnemy.attack; // Simula evitar todo o dano
                     logEvent(`Você desaparece nas sombras, o ${gameData.currentEnemy.name} erra o ataque!`);
                     // No próximo turno do inimigo, ele erraria. Implementação mais complexa.
                     // Por simplicidade, vamos dar um ataque bônus.
                     let bonusDamage = gameData.player.attack;
                     gameData.currentEnemy.hp = Math.max(0, gameData.currentEnemy.hp - bonusDamage);
                     logEvent(`Você surge e ataca de surpresa, causando ${bonusDamage} de dano extra! HP do inimigo: ${gameData.currentEnemy.hp}`);
                    break;
                case 'mage': // Tempestade Arcana - Dano em área (se houvessem múltiplos inimigos) ou dano massivo
                    let magicDamage = gameData.player.attack * 2;
                    gameData.currentEnemy.hp = Math.max(0, gameData.currentEnemy.hp - magicDamage);
                    logEvent(`Uma tempestade arcana atinge ${gameData.currentEnemy.name} causando ${magicDamage} de dano mágico! HP do inimigo: ${gameData.currentEnemy.hp}`);
                    break;
                case 'cleric': // Bênção Divina - Cura ou buff
                    let healAmount = Math.floor(gameData.player.maxHp * 0.3);
                    gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + healAmount);
                    logEvent(`Você se cura em ${healAmount} HP!`);
                    break;
                case 'barbarian': // Fúria Insana - Aumenta ataque massivamente, talvez reduz defesa
                    gameData.player.attack += 15;
                    gameData.player.defense = Math.max(0, gameData.player.defense - 2);
                    logEvent("Você entra em FÚRIA! Seu ataque aumenta, mas sua guarda baixa!");
                    setTimeout(() => { gameData.player.attack -= 15; gameData.player.defense +=2; updatePlayerStatsUI(); logEvent("Sua fúria diminuiu."); }, 15000);
                    break;
                case 'hunter': // Tiro Certeiro - Dano alto garantido ou chance de crítico
                    let preciseShotDamage = gameData.player.attack + 10; // Dano extra
                    gameData.currentEnemy.hp = Math.max(0, gameData.currentEnemy.hp - preciseShotDamage);
                    logEvent(`Seu Tiro Certeiro atinge ${gameData.currentEnemy.name} causando ${preciseShotDamage} de dano! HP do inimigo: ${gameData.currentEnemy.hp}`);
                    break;
            }

            if (gameData.currentEnemy && gameData.currentEnemy.hp <= 0) {
                 logEvent(`Você derrotou o ${gameData.currentEnemy.name} com sua habilidade!`, 'success');
                 gameData.currentEnemy = null;
            }

            updatePlayerStatsUI();
            updateEnemyUI();
            updateActionButtons(); // Desabilita o botão de habilidade
        });

        // Lógica de Fugir (placeholder)
        fleeButton.addEventListener('click', () => {
            if (!gameData.currentEnemy) return;

            logEvent("Você tenta fugir...");
            if (Math.random() < 0.6) { // 60% de chance de fugir
                logEvent("Você conseguiu fugir!", 'success');
                gameData.currentEnemy = null;
                updateEnemyUI();
                updateActionButtons();
            } else {
                logEvent("A fuga falhou!", 'important');
                // Inimigo ataca após falha na fuga
                let enemyDamage = Math.max(1, gameData.currentEnemy.attack - gameData.player.defense);
                gameData.player.hp = Math.max(0, gameData.player.hp - enemyDamage);
                logEvent(`${gameData.currentEnemy.name} ataca você durante a tentativa de fuga, causando ${enemyDamage} de dano. Seu HP: ${gameData.player.hp}`, 'combat');
                updatePlayerStatsUI();
                if (gameData.player.hp <= 0) {
                    logEvent("Você foi derrotado...", 'important');
                    showModal("Fim de Jogo! Você foi derrotado. Tente novamente!");
                    startScreen.style.display = 'flex';
                    gameScreen.style.display = 'none';
                    gameLogDiv.innerHTML = '<p>Bem-vindo à Dungeons Cave! Escolha uma classe para começar.</p>';
                }
            }
        });

        // Inicialização
        logEvent("Carregando Dungeons Cave...", "system");
        // Garante que a tela de jogo esteja escondida inicialmente e botões de ação atualizados
        gameScreen.style.display = 'none';
        startScreen.style.display = 'flex';
        updateActionButtons();

    </script>
</body>
</html>
// Adicionar poções de cura ao gameData
gameData.potions = [
    { name: "Poção de Cura Menor", healAmount: 20 },
    { name: "Poção de Cura", healAmount: 50 },
    { name: "Poção de Cura Maior", healAmount: 100 }
];
// Adicionar armas e armaduras ao gameData
gameData.items = {
    weapons: [
        { name: "Espada Curta", attack: 5 },
        { name: "Espada Longa", attack: 10 },
        { name: "Machado", attack: 8 }
    ],
    armors: [
        { name: "Armadura de Couro", defense: 3 },
        { name: "Armadura de Ferro", defense: 5 },
        { name: "Armadura de Placas", defense: 8 }
    ]
};
// Lógica de Exploração (atualizada)
exploreButton.addEventListener('click', () => {
    logEvent("Você explora a masmorra...");
    // Simular encontro com inimigo ou itens
    const encounters = [
        { type: 'enemy', enemy: { name: "Goblin Fraco", hp: 30, attack: 8, defense: 2, gold: 10 } },
        { type: 'item', item: gameData.potions[Math.floor(Math.random() * gameData.potions.length)] },
        { type: 'item', item: gameData.items.weapons[Math.floor(Math.random() * gameData.items.weapons.length)] }
    ];
    const encounter = encounters[Math.floor(Math.random() * encounters.length)];

    if (encounter.type === 'enemy') {
        gameData.currentEnemy = { ...encounter.enemy };
        logEvent(`Um ${gameData.currentEnemy.name} aparece!`, 'combat');
    } else {
        const item = encounter.item;
        gameData.player.inventory.push(item);
        logEvent(`Você encontrou uma ${item.name}!`, 'success');
    }

    updateEnemyUI();
    updateInventoryUI();
    updateActionButtons();
});
<button id="use-potion-button" class="pixel-button" disabled>Usar Poção</button>
// Lógica para usar poções
document.getElementById('use-potion-button').addEventListener('click', () => {
    if (gameData.player.inventory.length === 0) {
        logEvent("Você não tem poções para usar!", 'important');
        return;
    }

    const potion = gameData.player.inventory.find(item => item.healAmount);
    if (potion) {
        gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + potion.healAmount);
        logEvent(`Você usou uma ${potion.name} e curou ${potion.healAmount} HP!`, 'success');
        gameData.player.inventory = gameData.player.inventory.filter(item => item !== potion); // Remove a poção usada
        updatePlayerStatsUI();
        updateInventoryUI();
    } else {
        logEvent("Você não tem poções para usar!", 'important');
    }
});
