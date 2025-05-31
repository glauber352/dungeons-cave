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
            if (type ==
