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
            min-height: 100vh; /* Garante que o body ocupe a altura total da viewport */
            padding: 1rem;
            box-sizing: border-box; /* Inclui padding e borda no cálculo da largura/altura */
            margin: 0; /* Reseta a margem padrão do body */
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
            position: relative; /* Essencial para posicionar o modal */
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
            transition: background-color 0.1s, transform 0.1s; /* Transição suave */
            box-shadow: 3px 3px #a52a22; /* Sombra para efeito de profundidade */
            display: inline-block;
            width: auto;
            text-align: center;
            image-rendering: pixelated; /* Garante que fontes pixel art renderizem nitidamente */
        }

        .pixel-button:hover {
            background-color: #c0392b;
            transform: translateY(1px); /* Efeito de "levantar" no hover */
            box-shadow: 2px 2px #a52a22;
        }

        .pixel-button:active {
            transform: translateY(3px); /* Efeito de "pressionar" no clique */
            box-shadow: 0 0 #a52a22;
        }

        .pixel-button.full-width {
            display: block;
            width: 100%;
        }

         .pixel-button:disabled {
            background-color: #95a5a6; /* Cor cinza para botões desabilitados */
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
            height: 150px;
            overflow-y: auto; /* Adiciona barra de rolagem se o conteúdo exceder */
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
        /* Cores para diferentes tipos de mensagens no log */
        .game-log p.text-yellow-400 { color: #f1c40f; } /* Importante */
        .game-log p.text-red-400 { color: #e74c3c; } /* Combate/Dano ao jogador */
        .game-log p.text-orange-400 { color: #f39c12; } /* Dano ao inimigo */
        .game-log p.text-green-400 { color: #2ecc71; } /* Cura/Sucesso */
        .game-log p.text-blue-300 { color: #85c1e9; } /* Informação neutra */


        .modal {
            display: none; /* Escondido por padrão */
            position: absolute; /* Posição absoluta dentro do contêiner do jogo */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%); /* Centraliza o modal */
            background-color: #34495e;
            border: 4px solid #1a242f;
            border-radius: 8px;
            padding: 1.5rem 2rem;
            z-index: 100; /* Garante que o modal fique acima de outros elementos */
            box-shadow: 0 0 30px rgba(0, 0, 0, 0.7);
            text-align: center;
            width: 90%; /* Largura do modal */
            max-width: 600px; /* Largura máxima do modal */
        }
        
        .modal h2 {
            font-size: 1.2rem;
        }

        .modal-content button {
            margin-top: 1rem;
        }

        /* Estilos para tabelas */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
            font-size: 0.8rem;
        }

        th,
        td {
            border: 1px solid #555;
            padding: 0.4rem;
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
        
        #default-view { /* Container para a visão padrão (não-combate) */
            display: flex;
            flex-direction: column;
            gap: 1rem;
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
            font-size: 0.85rem;
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
            padding: 0.6rem 0.5rem;
            font-size: 0.8rem;
        }

        /* Estilos para a mochila */
        .inventory {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            max-height: 150px; /* Altura máxima para a mochila, com scroll se necessário */
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
            color: #ecf0f1;
        }
         .inventory p.empty-inventory-message { /* Classe específica para a mensagem de mochila vazia */
            font-size: 0.8rem;
            text-align: center;
            padding: 0.5rem;
            color: #95a5a6;
        }

        /* Melhorias para o modal de seleção de classe */
        #class-selection-modal .modal-content {
            max-height: 80vh;
            overflow-y: auto;
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

        /* --- NOVOS ESTILOS PARA A CENA DE COMBATE --- */
        #combat-scene {
            display: none; /* Começa escondido */
            flex-direction: column;
            background-color: #5a7d3c; /* Fundo verde, simulando campo de batalha */
            border: 4px solid #1a242f;
            border-radius: 5px;
            padding: 1rem;
            position: relative;
            height: 450px; /* Altura fixa para a cena */
        }
        
        .combat-area {
            position: absolute;
            width: 45%;
        }
        
        #enemy-combat-area {
            top: 20px;
            right: 20px;
            text-align: right;
        }
        
        #player-combat-area {
            bottom: 130px; /* Posição acima da caixa de texto/ações */
            left: 20px;
        }

        .sprite-placeholder {
            height: 100px;
            width: 100px;
            background-color: rgba(0,0,0,0.2);
            border: 2px dashed #1a242f;
            display: inline-flex; /* Usando flex para centralizar o texto */
            justify-content: center;
            align-items: center;
            font-size: 0.7rem;
            color: #ecf0f1;
            margin-bottom: 10px;
        }
        
        #player-combat-area .sprite-placeholder {
            /* Placeholder pode ser maior para o jogador */
            height: 120px;
            width: 120px;
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
            height: 100%;
            background-color: #2ecc71; /* Verde */
            border-radius: 2px;
            transition: width 0.5s ease-out, background-color 0.5s ease;
            position: absolute;
            z-index: 1;
        }
        
        .hp-bar.yellow { background-color: #f1c40f; } /* Amarelo */
        .hp-bar.red { background-color: #e74c3c; } /* Vermelho */

        /* Ajustando a parte de baixo para o combate */
        #combat-bottom-panel {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            padding: 0.5rem;
            box-sizing: border-box;
            display: flex;
            flex-direction: row;
            gap: 1rem;
        }
        #combat-bottom-panel .log-panel,
        #combat-bottom-panel .actions-panel {
            flex: 1;
            margin: 0;
        }
        #combat-bottom-panel .game-log {
             height: 100px;
        }
        #combat-bottom-panel .actions-panel {
            padding-top: 0;
        }
        #combat-bottom-panel .game-actions {
             grid-template-columns: repeat(2, 1fr);
             height: 115px;
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
                <div class="overflow-x-auto">
                    <table>
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
            <h2 id="screen-title" class="text-2xl">Aventura em Andamento!</h2>
            
            <div id="combat-scene">
                 <div id="enemy-combat-area" class="combat-area">
                    <div class="status-box" id="enemy-status-box">
                        <div class="name-level">
                            <span id="combat-enemy-name"></span>
                            <span id="combat-enemy-level"></span>
                        </div>
                        <div class="hp-bar-container">
                            <span class="hp-bar-label">HP</span>
                            <div class="hp-bar" id="combat-enemy-hp-bar"></div>
                        </div>
                    </div>
                    <div class="sprite-placeholder">INIMIGO</div>
                </div>

                <div id="player-combat-area" class="combat-area">
                     <div class="sprite-placeholder">HERÓI</div>
                    <div class="status-box" id="player-status-box">
                       <div class="name-level">
                            <span id="combat-player-name"></span>
                            <span id="combat-player-level"></span>
                        </div>
                        <div class="hp-bar-container">
                             <span class="hp-bar-label">HP</span>
                            <div class="hp-bar" id="combat-player-hp-bar"></div>
                        </div>
                         <div id="combat-player-hp-text" class="text-right text-xs mt-1"></div>
                    </div>
                </div>
                
                <div id="combat-bottom-panel">
                      </div>
            </div>

            <div id="default-view">
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

                <div id="default-log-panel" class="panel log-panel">
                    <h3 class="text-lg">Registro de Eventos</h3>
                    <div id="game-log" class="game-log">
                        <p>Bem-vindo à Dungeons Cave! Escolha uma classe para começar.</p>
                    </div>
                </div>
                
                <div id="default-actions-inventory-area" class="actions-inventory-area">
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
    </div>

    <script>
        // Objeto global para armazenar todos os dados e estados do jogo
        const gameData = {
            classes: {
                knight: { name: "Cavaleiro", description: "Tanque, defesa alta, espada e escudo.", ability: "Escudo Imbatível", baseHp: 120, baseAttack: 15, baseDefense: 10 },
                rogue: { name: "Ladino", description: "Ágil, furtivo, dano crítico alto.", ability: "Ataque Furtivo", baseHp: 80, baseAttack: 25, baseDefense: 5 },
                mage: { name: "Mago", description: "Mestre dos elementos, dano mágico em área.", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3 },
                cleric: { name: "Clérigo", description: "Suporte, cura, buffs e dano sagrado.", ability: "Bênção Divina", baseHp: 90, baseAttack: 20, baseDefense: 7 },
                barbarian: { name: "Bárbaro", description: "Selvagem, dano físico bruto, pouca defesa.", ability: "Fúria Insana", baseHp: 110, baseAttack: 28, baseDefense: 4 },
                hunter: { name: "Patrulheiro", description: "Perito em combate à distância, usa arcos.", ability: "Tiro Certeiro", baseHp: 85, baseAttack: 22, baseDefense: 6 }
            },
            selectedClassKey: null,
            selectedClass: null,
            
            player: {
                name: "",
                level: 1, // Nível do jogador
                hp: 0,
                maxHp: 0,
                attack: 0,
                defense: 0,
                abilityName: "",
                inventory: [],
                equippedWeapon: null,
                specialAbilityUsedThisCombat: false
            },
            
            currentEnemy: null,
            inCombat: false, // Flag para controlar o estado de combate
            
            enemies: {
                // Inimigos normais
                goblin: { name: "Goblin Ardiloso", level: 1, hp: 30, maxHp: 30, attack: 8, defense: 2, gold: 5, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.3 }] },
                orc: { name: "Orc Brutal", level: 2, hp: 50, maxHp: 50, attack: 12, defense: 4, gold: 10, lootTable: [{ item: "Adaga Enferrujada", chance: 0.2 }] },
                slime: { name: "Slime Ácido", level: 1, hp: 20, maxHp: 20, attack: 6, defense: 1, gold: 3, lootTable: [{ item: "Gosma Pegajosa", chance: 0.5 }] },
                spider: { name: "Aranha Gigante", level: 2, hp: 40, maxHp: 40, attack: 10, defense: 3, gold: 7, lootTable: [{ item: "Teia Pegajosa", chance: 0.4 }] },

                // Chefes Lendários
                tarrasque: { name: "Tarrasque", level: 30, hp: 676, maxHp: 676, attack: 50, defense: 18, gold: 500, lootTable: [{ item: "Fragmento de Carapaça do Tarrasque", chance: 1.0 }] },
                ancientRedDragon: { name: "Dragão Vermelho Ancestral", level: 24, hp: 546, maxHp: 546, attack: 45, defense: 15, gold: 300, lootTable: [{ item: "Escama de Dragão Rubra", chance: 1.0 }] },
                aspectOfTiamat: { name: "Aspecto de Tiamat", level: 27, hp: 615, maxHp: 615, attack: 55, defense: 20, gold: 750, lootTable: [{ item: "Olho de Tiamat", chance: 1.0 }] },
                kraken: { name: "Kraken", level: 23, hp: 472, maxHp: 472, attack: 40, defense: 12, gold: 250, lootTable: [{ item: "Tentáculo de Kraken", chance: 1.0 }] },
                acererak: { name: "Acererak, o Lich", level: 21, hp: 285, maxHp: 285, attack: 60, defense: 14, gold: 400, lootTable: [{ item: "Cajado do Lich Supremo", chance: 0.2 }] },
                balor: { name: "Balor", level: 19, hp: 262, maxHp: 262, attack: 40, defense: 13, gold: 200, lootTable: [{ item: "Chicote de Fogo Demoníaco", chance: 0.3 }] }
            },
            
            items: {
                "Poção de Cura Pequena": { type: "potion", effect: "heal", power: 20, consumable: true, description: "Restaura 20 HP." },
                "Adaga Enferrujada": { type: "weapon", attackBonus: 2, description: "Uma adaga velha, mas ainda pontuda." },
                "Espada Curta de Iniciante": { type: "weapon", attackBonus: 3, description: "Uma espada básica para aventureiros." },
                "Cajado do Lich Supremo": { type: "weapon", attackBonus: 25, description: "Um cajado imbuído de magia necrótica suprema." },
                "Chicote de Fogo Demoníaco": { type: "weapon", attackBonus: 20, description: "Um chicote que arde com fogo infernal." },
                "Gosma Pegajosa": { type: "junk", description: "Uma gosma nojenta." },
                "Teia Pegajosa": { type: "junk", description: "Uma teia grudenta." },
                "Fragmento de Carapaça do Tarrasque": { type: "crafting_material", description: "Um pedaço da carapaça indestrutível do Tarrasque." },
                "Escama de Dragão Rubra": { type: "crafting_material", description: "Uma escama vermelha brilhante de um dragão ancestral." },
                "Olho de Tiamat": { type: "magic_artifact", description: "Um olho de Tiamat, a Rainha Dragão." },
                "Tentáculo de Kraken": { type:
