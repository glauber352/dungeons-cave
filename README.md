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
        // Objeto global para armazenar todos os dados e estados do jogo
        const gameData = {
            // Definições das classes de personagem disponíveis
            classes: {
                knight: { name: "Cavaleiro", description: "Tanque, defesa alta, espada e escudo.", ability: "Escudo Imbatível", baseHp: 120, baseAttack: 15, baseDefense: 10 },
                rogue: { name: "Ladino", description: "Ágil, furtivo, dano crítico alto.", ability: "Ataque Furtivo", baseHp: 80, baseAttack: 25, baseDefense: 5 },
                mage: { name: "Mago", description: "Mestre dos elementos, dano mágico em área.", ability: "Tempestade Arcana", baseHp: 70, baseAttack: 30, baseDefense: 3 },
                cleric: { name: "Clérigo", description: "Suporte, cura, buffs e dano sagrado.", ability: "Bênção Divina", baseHp: 90, baseAttack: 20, baseDefense: 7 },
                barbarian: { name: "Bárbaro", description: "Selvagem, dano físico bruto, pouca defesa.", ability: "Fúria Insana", baseHp: 110, baseAttack: 28, baseDefense: 4 },
                hunter: { name: "Patrulheiro", description: "Perito em combate à distância, usa arcos.", ability: "Tiro Certeiro", baseHp: 85, baseAttack: 22, baseDefense: 6 }
            },
            selectedClassKey: null, // Chave da classe selecionada (ex: 'knight')
            selectedClass: null,    // Objeto completo da classe selecionada
            
            // Dados do jogador atual
            player: {
                name: "",           // Nome da classe do jogador
                hp: 0,              // Pontos de vida atuais
                maxHp: 0,           // Pontos de vida máximos
                attack: 0,          // Valor de ataque base
                defense: 0,         // Valor de defesa
                abilityName: "",    // Nome da habilidade especial
                inventory: [],      // Itens na mochila do jogador
                equippedWeapon: null, // Arma atualmente equipada
                specialAbilityUsedThisCombat: false // Flag para controlar uso de habilidade por combate
            },
            
            currentEnemy: null,     // O inimigo atualmente enfrentado
            
            // Modelos de inimigos básicos e os NOVOS CHEFES
            enemies: {
                goblin: { name: "Goblin Ardiloso", hp: 30, maxHp: 30, attack: 8, defense: 2, gold: 5, lootTable: [{ item: "Poção de Cura Pequena", chance: 0.3 }] },
                orc: { name: "Orc Brutal", hp: 50, maxHp: 50, attack: 12, defense: 4, gold: 10, lootTable: [{ item: "Adaga Enferrujada", chance: 0.2 }] },
                slime: { name: "Slime Ácido", hp: 20, maxHp: 20, attack: 6, defense: 1, gold: 3, lootTable: [{ item: "Gosma Pegajosa", chance: 0.5 }] },
                spider: { name: "Aranha Gigante", hp: 40, maxHp: 40, attack: 10, defense: 3, gold: 7, lootTable: [{ item: "Teia Pegajosa", chance: 0.4 }] },

                // --- NOVOS CHEFES LENDÁRIOS (CRs adaptados para o jogo) ---
                tarrasque: {
                    name: "Tarrasque",
                    hp: 676, maxHp: 676,
                    attack: 50, // Dano muito alto para representar a força e múltiplos ataques
                    defense: 18, // CA 25
                    gold: 500,
                    lootTable: [
                        { item: "Fragmento de Carapaça do Tarrasque", chance: 1.0 },
                        { item: "Coração Pulsante do Tarrasque", chance: 0.5 },
                        { item: "Pergaminho de Bola de Fogo", chance: 0.1 } // Chance de drop de spell
                    ]
                },
                ancientRedDragon: {
                    name: "Dragão Vermelho Ancestral",
                    hp: 546, maxHp: 546,
                    attack: 45, // Alto, representando mordida e sopro
                    defense: 15, // CA 22
                    gold: 300,
                    lootTable: [
                        { item: "Escama de Dragão Rubra", chance: 1.0 },
                        { item: "Gema de Fogo Dracônica", chance: 0.7 },
                        { item: "Pergaminho de Cura Leve", chance: 0.2 } // Chance de drop de spell
                    ]
                },
                aspectOfTiamat: {
                    name: "Aspecto de Tiamat",
                    hp: 615, maxHp: 615,
                    attack: 55, // O mais alto, representando as 5 cabeças e baforadas
                    defense: 20, // CA 27
                    gold: 750,
                    lootTable: [
                        { item: "Olho de Tiamat", chance: 1.0 },
                        { item: "Essência Dracônica Suprema", chance: 1.0 },
                        { item: "Pergaminho de Escudo Arcana", chance: 0.3 } // Chance de drop de spell
                    ]
                },
                kraken: {
                    name: "Kraken",
                    hp: 472, maxHp: 472,
                    attack: 40, // Tentáculos e relâmpago
                    defense: 12, // CA 18
                    gold: 250,
                    lootTable: [
                        { item: "Tentáculo de Kraken", chance: 1.0 },
                        { item: "Orbe de Tempestade", chance: 0.6 }
                    ]
                },
                acererak: {
                    name: "Acererak, o Lich Supremo",
                    hp: 285, maxHp: 285, // HP mais baixo, mas ataque devastador
                    attack: 60, // Dedo da Morte é muito poderoso
                    defense: 14, // CA 21
                    gold: 400,
                    lootTable: [
                        { item: "Cajado do Lich Supremo", chance: 0.2 },
                        { item: "Filactério de Acererak (Quebrado)", chance: 1.0 }
                    ]
                },
                balor: {
                    name: "Balor",
                    hp: 262, maxHp: 262,
                    attack: 40, // Espada flamejante e látego
                    defense: 13, // CA 19
                    gold: 200,
                    lootTable: [
                        { item: "Chicote de Fogo Demoníaco", chance: 0.3 },
                        { item: "Fogo Infernal Engarrafado", chance: 0.8 }
                    ]
                }
            },
            
            // Definições de itens (incluindo os novos itens de loot dos chefes e tipos)
            items: {
                // Poções
                "Poção de Cura Pequena": { type: "potion", effect: "heal", power: 20, consumable: true, description: "Restaura 20 HP." },
                
                // Armas
                "Adaga Enferrujada": { type: "weapon", attackBonus: 2, description: "Uma adaga velha, mas ainda pontuda." },
                "Espada Curta de Iniciante": { type: "weapon", attackBonus: 3, description: "Uma espada básica para aventureiros." },
                "Cajado do Lich Supremo": { type: "weapon", attackBonus: 25, description: "Um cajado imbuído de magia necrótica suprema. Muito poderoso." },
                "Chicote de Fogo Demoníaco": { type: "weapon", attackBonus: 20, description: "Um chicote que arde com fogo infernal. Queima ao toque." },

                // Pergaminhos de Feitiço (consumíveis)
                "Pergaminho de Bola de Fogo": { type: "spell_scroll", spellName: "Bola de Fogo", spellEffect: "fireball", spellPower: 40, consumable: true, description: "Libera uma explosão de chamas." },
                "Pergaminho de Cura Leve": { type: "spell_scroll", spellName: "Cura Leve", spellEffect: "minor_heal", spellPower: 30, consumable: true, description: "Cura ferimentos leves." },
                "Pergaminho de Escudo Arcana": { type: "spell_scroll", spellName: "Escudo Arcana", spellEffect: "arcane_shield", spellPower: 10, consumable: true, description: "Cria um escudo mágico temporário que aumenta a defesa." },

                // Materiais de Criação / Artefatos Mágicos (junk/flavor por enquanto)
                "Gosma Pegajosa": { type: "junk", description: "Uma gosma nojenta. Talvez sirva para algo?" },
                "Teia Pegajosa": { type: "junk", description: "Uma teia grudenta. Pode ser útil para armadilhas." },
                "Fragmento de Carapaça do Tarrasque": { type: "crafting_material", description: "Um pedaço da carapaça indestrutível do Tarrasque." },
                "Coração Pulsante do Tarrasque": { type: "magic_artifact", description: "O coração do Tarrasque, pulsando com energia primordial. Extremamente raro." },
                "Escama de Dragão Rubra": { type: "crafting_material", description: "Uma escama vermelha brilhante de um dragão ancestral." },
                "Gema de Fogo Dracônica": { type: "magic_artifact", description: "Uma gema que irradia calor intenso e poder dracônico." },
                "Olho de Tiamat": { type: "magic_artifact", description: "Um olho de Tiamat, a Rainha Dragão. Emite uma aura de poder divino e maligno." },
                "Essência Dracônica Suprema": { type: "crafting_material", description: "A essência pura de um ser dracônico supremo. Ingrediente lendário." },
                "Tentáculo de Kraken": { type: "crafting_material", description: "Um pedaço de tentáculo do Kraken. Ainda se contorce levemente." },
                "Orbe de Tempestade": { type: "magic_artifact", description: "Um orbe que murmura com o poder das tempestades. Pode invocar raios." },
                "Filactério de Acererak (Quebrado)": { type: "magic_artifact", description: "O filactério quebrado de Acererak. Sua magia está quase esgotada." },
                "Fogo Infernal Engarrafado": { type: "consumable", effect: "damage_aoe", power: 30, consumable: true, description: "Uma garrafa que contém fogo puro do inferno. Explode ao ser jogada." },
                "Essência Demoníaca": { type: "crafting_material", description: "A essência de um demônio poderoso. É palpável e assustadora." }
            }
        };

        // --- Referências aos Elementos do DOM ---
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

        // --- Funções Auxiliares ---

        /**
         * Exibe um modal de mensagem genérico no jogo.
         * @param {string} message A mensagem a ser exibida.
         * @param {function} onOkCallback Função a ser executada quando o botão OK é clicado.
         */
        function showModal(message, onOkCallback = null) {
            modalMessage.textContent = message;
            gameModal.style.display = 'block'; // Exibe o modal
            modalOkBtn.onclick = () => {
                hideModal();
                if (onOkCallback) {
                    onOkCallback(); // Executa o callback se fornecido
                }
            };
        }

        /** Oculta o modal de mensagem genérico. */
        function hideModal() {
            gameModal.style.display = 'none';
        }

        /**
         * Adiciona uma mensagem ao log de eventos do jogo.
         * @param {string} message O texto da mensagem.
         * @param {string} type O tipo da mensagem para estilização (e.g., 'normal', 'important', 'combat', 'heal').
         */
        function logEvent(message, type = 'normal') {
            const p = document.createElement('p');
            p.textContent = message;
            // Adiciona classes CSS baseadas no tipo para coloração e destaque
            if (type === 'important') p.classList.add('text-yellow-400');
            else if (type === 'combat') p.classList.add('text-red-400'); // Ações de jogador/inimigo
            else if (type === 'player_damage') p.classList.add('text-red-400', 'font-bold'); // Dano recebido pelo jogador
            else if (type === 'enemy_damage') p.classList.add('text-orange-400'); // Dano causado ao inimigo
            else if (type === 'heal') p.classList.add('text-green-400'); // Cura
            else if (type === 'success') p.classList.add('text-green-400'); // Sucesso geral (ex: fugir)
            else if (type === 'info') p.classList.add('text-blue-300'); // Informação neutra
            else if (type === 'error') p.classList.add('text-red-500', 'font-bold'); // Erros ou falhas críticas
            
            gameLogDiv.appendChild(p);
            // Mantém o scroll no final para ver as mensagens mais recentes
            gameLogDiv.scrollTop = gameLogDiv.scrollHeight;
        }

        /** Atualiza a exibição das estatísticas do jogador na interface. */
        function updatePlayerStats() {
            const player = gameData.player;
            playerStatsDiv.innerHTML = `
                <span>Classe: ${player.name}</span>
                <span>HP: ${player.hp}/${player.maxHp}</span>
                <span>Ataque: ${player.attack + (player.equippedWeapon ? player.equippedWeapon.attackBonus : 0)}</span>
                <span>Defesa: ${player.defense}</span>
                <span>Habilidade: ${player.abilityName}</span>
                <span>Arma: ${player.equippedWeapon ? player.equippedWeapon.name : 'Nenhuma'}</span>
            `;
        }

        /** Atualiza a exibição das informações do inimigo na interface. */
        function updateEnemyInfo() {
            const enemy = gameData.currentEnemy;
            if (enemy) {
                enemyNameSpan.textContent = `Inimigo: ${enemy.name}`;
                enemyHpSpan.textContent = `HP: ${enemy.hp}/${enemy.maxHp}`;
            } else {
                enemyNameSpan.textContent = "Nenhum Inimigo";
                enemyHpSpan.textContent = "HP: --";
            }
        }

        /**
         * Adiciona um item ao inventário do jogador.
         * @param {string} itemName O nome do item a ser adicionado.
         */
        function addItemToInventory(itemName) {
            gameData.player.inventory.push(itemName);
            logEvent(`Você encontrou: ${itemName}!`, 'info');
            renderInventory(); // Atualiza a exibição do inventário
        }

        /** Renderiza (atualiza) a lista de itens no inventário na interface. */
        function renderInventory() {
            inventoryDiv.innerHTML = ''; // Limpa o inventário atual
            if (gameData.player.inventory.length === 0) {
                const p = document.createElement('p');
                p.classList.add('empty-inventory-message');
                p.textContent = "Sua mochila está vazia.";
                inventoryDiv.appendChild(p);
            } else {
                gameData.player.inventory.forEach(item => {
                    const itemDiv = document.createElement('div');
                    itemDiv.classList.add('inventory-item');
                    itemDiv.innerHTML = `
                        <span>${item}</span>
                        <button class="pixel-button use-item-button" data-item="${item}">Usar</button>
                    `;
                    inventoryDiv.appendChild(itemDiv);
                });
                // Adiciona listeners aos botões "Usar" dos itens
                document.querySelectorAll('.use-item-button').forEach(button => {
                    button.addEventListener('click', (event) => {
                        const itemName = event.target.dataset.item;
                        useItem(itemName);
                    });
                });
            }
        }

        /**
         * Usa um item do inventário.
         * @param {string} itemName O nome do item a ser usado.
         */
        function useItem(itemName) {
            const itemIndex = gameData.player.inventory.indexOf(itemName);
            if (itemIndex === -1) {
                logEvent(`Você não tem ${itemName} no inventário.`, 'error');
                return;
            }

            const itemDetails = gameData.items[itemName];

            if (!itemDetails) {
                logEvent(`Detalhes do item ${itemName} não encontrados.`, 'error');
                return;
            }

            let itemConsumed = false; // Flag para verificar se o item deve ser removido

            switch (itemDetails.type) {
                case "potion":
                    if (itemDetails.effect === "heal") {
                        const healAmount = itemDetails.power;
                        gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + healAmount);
                        logEvent(`Você usou ${itemName} e restaurou ${healAmount} HP!`, 'heal');
                        itemConsumed = true;
                    }
                    break;
                case "weapon":
                    // Verifica se já há uma arma equipada
                    if (gameData.player.equippedWeapon) {
                        const oldWeaponName = gameData.player.equippedWeapon.name;
                        // Remove o bônus da arma antiga do ataque base do jogador
                        gameData.player.attack -= gameData.player.equippedWeapon.attackBonus;
                        // Devolve a arma antiga para o inventário
                        gameData.player.inventory.push(oldWeaponName);
                        logEvent(`Você desequipou ${oldWeaponName}.`, 'info');
                    }
                    // Equipa a nova arma
                    gameData.player.equippedWeapon = { name: itemName, attackBonus: itemDetails.attackBonus };
                    // Adiciona o bônus da nova arma ao ataque base do jogador
                    gameData.player.attack += itemDetails.attackBonus;
                    logEvent(`Você equipou ${itemName}! Seu ataque base aumentou em ${itemDetails.attackBonus}.`, 'success');
                    // O item é removido do inventário porque agora está equipado
                    itemConsumed = true;
                    break;
                case "spell_scroll":
                    if (itemDetails.spellEffect) {
                        logEvent(`Você leu o ${itemName} e conjurou ${itemDetails.spellName || itemDetails.spellEffect}!`, 'important');
                        switch (itemDetails.spellEffect) {
                            case 'fireball':
                                if (gameData.currentEnemy) {
                                    const spellDamage = itemDetails.spellPower;
                                    gameData.currentEnemy.hp -= spellDamage;
                                    logEvent(`Uma bola de fogo atinge o ${gameData.currentEnemy.name} causando ${spellDamage} de dano de fogo!`, 'enemy_damage');
                                } else {
                                    logEvent("Não há inimigo para lançar feitiço.", 'info');
                                    itemConsumed = false; // Não consome se não há alvo
                                }
                                break;
                            case 'minor_heal':
                                const healAmountSpell = itemDetails.spellPower;
                                gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + healAmountSpell);
                                logEvent(`Você se curou em ${healAmountSpell} HP com magia!`, 'heal');
                                break;
                            case 'arcane_shield':
                                // Aumenta a defesa temporariamente
                                const defenseBuff = itemDetails.spellPower;
                                gameData.player.defense += defenseBuff;
                                logEvent(`Um escudo arcano te envolve, aumentando sua defesa em ${defenseBuff} por um turno!`, 'success');
                                // Remove o buff após 2 segundos (simulando 1 turno de combate)
                                if (gameData.currentEnemy) {
                                    setTimeout(() => {
                                        gameData.player.defense -= defenseBuff;
                                        logEvent("O escudo arcano se dissipou.", 'info');
                                        updatePlayerStats();
                                    }, 2000);
                                }
                                break;
                            case 'damage_aoe': // Para o Fogo Infernal Engarrafado
                                if (gameData.currentEnemy) {
                                    const aoeDamage = itemDetails.power;
                                    gameData.currentEnemy.hp -= aoeDamage;
                                    logEvent(`O ${itemName} explodiu, causando ${aoeDamage} de dano em área ao ${gameData.currentEnemy.name}!`, 'enemy_damage');
                                } else {
                                    logEvent("Não há alvo para a explosão.", 'info');
                                    itemConsumed = false;
                                }
                                break;
                            default:
                                logEvent(`Efeito de feitiço desconhecido: ${itemDetails.spellEffect}.`, 'error');
                                itemConsumed = false; // Não consome se o efeito é desconhecido
                        }
                        if (itemConsumed !== false) { // Se o feitiço foi lançado com sucesso ou tinha alvo
                            itemConsumed = true;
                        }
                    }
                    break;
                case "junk":
                case "crafting_material":
                case "magic_artifact":
                    logEvent(`Você não pode usar ${itemName}. É apenas um item de ${itemDetails.type}.`, 'info');
                    break;
                case "consumable": // Para itens como "Fogo Infernal Engarrafado"
                    if (itemDetails.effect === "damage_aoe") {
                        // Reusa a lógica de dano em área de feitiço
                        if (gameData.currentEnemy) {
                            const aoeDamage = itemDetails.power;
                            gameData.currentEnemy.hp -= aoeDamage;
                            logEvent(`O ${itemName} explodiu, causando ${aoeDamage} de dano em área ao ${gameData.currentEnemy.name}!`, 'enemy_damage');
                            itemConsumed = true;
                        } else {
                            logEvent("Não há alvo para a explosão.", 'info');
                            itemConsumed = false;
                        }
                    }
                    break;
                default:
                    logEvent(`Tipo de item desconhecido: ${itemDetails.type}.`, 'error');
                    break;
            }

            // Remove o item do inventário se for consumível OU se for uma arma que foi equipada
            if ((itemConsumed && itemDetails.consumable) || (itemConsumed && itemDetails.type === "weapon")) {
                gameData.player.inventory.splice(itemIndex, 1);
            }

            updatePlayerStats();
            updateEnemyInfo(); // Atualiza HP do inimigo após feitiço
            renderInventory();

            // Se em combate e o item foi usado/consumido, o inimigo age
            if (gameData.currentEnemy && itemConsumed) {
                setTimeout(enemyTurn, 1000);
            }
        }

        /** Habilita ou desabilita os botões de ação de combate. */
        function toggleCombatButtons(enable) {
            attackButton.disabled = !enable;
            abilityButton.disabled = !enable || gameData.player.specialAbilityUsedThisCombat; // Desabilita se já usou
            fleeButton.disabled = !enable;
            exploreButton.disabled = enable; // Desabilita explorar durante o combate
        }

        /** Inicia um novo combate, gerando um inimigo aleatório. */
        function startCombat() {
            const enemyKeys = Object.keys(gameData.enemies);
            // Escolhe um inimigo aleatório do pool
            const randomEnemyKey = enemyKeys[Math.floor(Math.random() * enemyKeys.length)];
            // Cria uma cópia profunda do inimigo para que as mudanças de HP não afetem o template original
            gameData.currentEnemy = JSON.parse(JSON.stringify(gameData.enemies[randomEnemyKey]));
            gameData.player.specialAbilityUsedThisCombat = false; // Reseta a flag da habilidade
            
            logEvent(`Um ${gameData.currentEnemy.name} apareceu! Prepare-se para a batalha!`, 'important');
            updateEnemyInfo();
            toggleCombatButtons(true); // Habilita botões de combate
        }

        /** Finaliza o combate, limpando o inimigo atual e reabilitando o botão de explorar. */
        function endCombat() {
            gameData.currentEnemy = null;
            updateEnemyInfo();
            logEvent("O combate terminou.", 'info');
            toggleCombatButtons(false); // Desabilita botões de combate
            exploreButton.disabled = false; // Reabilita o botão de explorar
        }

        /** Lógica de ataque do jogador ao inimigo. */
        function attackEnemy() {
            if (!gameData.currentEnemy) {
                logEvent("Não há inimigo para atacar.", 'error');
                return;
            }

            // Calcula o ataque total do jogador (base + bônus da arma equipada)
            const playerTotalAttack = gameData.player.attack + (gameData.player.equippedWeapon ? gameData.player.equippedWeapon.attackBonus : 0);
            const enemyDefense = gameData.currentEnemy.defense;
            let damage = Math.max(0, playerTotalAttack - enemyDefense); // Dano mínimo de 0
            
            gameData.currentEnemy.hp -= damage;
            logEvent(`Você atacou o ${gameData.currentEnemy.name} causando ${damage} de dano!`, 'enemy_damage');
            updateEnemyInfo();

            if (gameData.currentEnemy.hp <= 0) {
                logEvent(`Você derrotou o ${gameData.currentEnemy.name}!`, 'success');
                // Adiciona lógica de loot e ouro
                if (gameData.currentEnemy.lootTable) {
                    gameData.currentEnemy.lootTable.forEach(loot => {
                        if (Math.random() < loot.chance) {
                            addItemToInventory(loot.item);
                        }
                    });
                }
                endCombat();
            } else {
                setTimeout(enemyTurn, 1000); // Inimigo ataca após um pequeno delay
            }
        }

        /** Lógica de ataque do inimigo ao jogador. */
        function enemyTurn() {
            if (!gameData.currentEnemy) return; // Não há inimigo para atacar

            const enemyAttack = gameData.currentEnemy.attack;
            const playerDefense = gameData.player.defense;
            let damage = Math.max(0, enemyAttack - playerDefense);

            gameData.player.hp -= damage;
            logEvent(`${gameData.currentEnemy.name} atacou você causando ${damage} de dano!`, 'player_damage');
            updatePlayerStats();

            if (gameData.player.hp <= 0) {
                logEvent("Você foi derrotado! Fim de jogo.", 'important');
                showModal("Você foi derrotado! Fim de jogo.", () => {
                    // Reinicia o jogo ou volta para a tela inicial
                    location.reload(); // Recarrega a página para reiniciar
                });
                toggleCombatButtons(false); // Desabilita todos os botões
                exploreButton.disabled = true;
            }
        }

        /** Lógica para usar a habilidade especial do jogador. */
        function handleAbility() {
            if (gameData.player.specialAbilityUsedThisCombat) {
                logEvent("Você já usou sua habilidade especial neste combate.", 'info');
                return;
            }

            gameData.player.specialAbilityUsedThisCombat = true;
            abilityButton.disabled = true; // Desabilita a habilidade após o uso

            logEvent(`Você usou ${gameData.player.abilityName}!`, 'important');

            // Lógica específica para cada habilidade
            switch (gameData.selectedClassKey) {
                case 'knight':
                    // Escudo Imbatível: Reduz o dano recebido na próxima rodada ou aumenta defesa temporariamente
                    logEvent("Seu escudo brilha, aumentando sua defesa temporariamente!", 'success');
                    // Para simplificar, vamos curar um pouco e causar dano
                    const healAmountKnight = 10;
                    gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + healAmountKnight);
                    logEvent(`Você recuperou ${healAmountKnight} HP!`, 'heal');
                    gameData.currentEnemy.hp -= 5; // Causa um pouco de dano
                    logEvent(`O ${gameData.currentEnemy.name} sofreu 5 de dano pelo impacto!`, 'enemy_damage');
                    break;
                case 'rogue':
                    // Ataque Furtivo: Causa dano extra
                    const rogueDamage = gameData.player.attack * 1.5; // 50% de dano extra
                    gameData.currentEnemy.hp -= Math.max(0, rogueDamage - gameData.currentEnemy.defense);
                    logEvent(`Você desferiu um Ataque Furtivo poderoso!`, 'enemy_damage');
                    break;
                case 'mage':
                    // Tempestade Arcana: Dano mágico alto
                    const mageDamage = gameData.player.attack * 2; // Dano massivo
                    gameData.currentEnemy.hp -= Math.max(0, mageDamage - gameData.currentEnemy.defense);
                    logEvent(`Uma Tempestade Arcana devastadora atingiu o ${gameData.currentEnemy.name}!`, 'enemy_damage');
                    break;
                case 'cleric':
                    // Bênção Divina: Cura o jogador
                    const healAmountCleric = 30;
                    gameData.player.hp = Math.min(gameData.player.maxHp, gameData.player.hp + healAmountCleric);
                    logEvent(`Uma Bênção Divina te curou em ${healAmountCleric} HP!`, 'heal');
                    break;
                case 'barbarian':
                    // Fúria Insana: Aumenta ataque por uma rodada, mas pode reduzir defesa
                    logEvent("Você entrou em Fúria Insana! Seu ataque está mais forte!", 'combat');
                    const barbarianDamage = gameData.player.attack * 1.7; // Dano alto
                    gameData.currentEnemy.hp -= Math.max(0, barbarianDamage - gameData.currentEnemy.defense);
                    logEvent(`Você atacou com fúria causando dano massivo!`, 'enemy_damage');
                    // Para simplificar, o efeito é instantâneo aqui, não dura uma rodada
                    break;
                case 'hunter':
                    // Tiro Certeiro: Dano preciso e alto
                    const hunterDamage = gameData.player.attack * 1.6; // Dano alto
                    gameData.currentEnemy.hp -= Math.max(0, hunterDamage - gameData.currentEnemy.defense);
                    logEvent(`Você disparou um Tiro Certeiro no ${gameData.currentEnemy.name}!`, 'enemy_damage');
                    break;
                default:
                    logEvent("Habilidade desconhecida.", 'error');
                    break;
            }
            
            updatePlayerStats();
            updateEnemyInfo();

            if (gameData.currentEnemy.hp <= 0) {
                logEvent(`Você derrotou o ${gameData.currentEnemy.name} com sua habilidade!`, 'success');
                if (gameData.currentEnemy.lootTable) {
                    gameData.currentEnemy.lootTable.forEach(loot => {
                        if (Math.random() < loot.chance) {
                            addItemToInventory(loot.item);
                        }
                    });
                }
                endCombat();
            } else {
                setTimeout(enemyTurn, 1000); // Inimigo ataca após a habilidade
            }
        }

        /** Lógica para tentar fugir do combate. */
        function fleeAttempt() {
            // Chance de sucesso da fuga (ex: 60%)
            const fleeChance = 0.6; 
            if (Math.random() < fleeChance) {
                logEvent("Você conseguiu fugir do combate!", 'success');
                endCombat();
            } else {
                logEvent("Você falhou em fugir! O inimigo ataca!", 'combat');
                setTimeout(enemyTurn, 1000); // Inimigo ataca se a fuga falhar
            }
        }

        // --- Inicialização do Jogo ---

        /** Preenche o modal de seleção de classe com as opções disponíveis. */
        function populateClassSelection() {
            classOptionsDiv.innerHTML = ''; // Limpa opções existentes
            for (const key in gameData.classes) {
                const classInfo = gameData.classes[key];
                const button = document.createElement('button');
                button.classList.add('pixel-button', 'full-width', 'mb-2');
                button.textContent = classInfo.name;
                button.dataset.classKey = key; // Armazena a chave da classe no dataset do botão
                button.title = classInfo.description; // Dica de ferramenta com a descrição
                
                button.addEventListener('click', () => {
                    // Remove a seleção de todos os botões
                    document.querySelectorAll('#class-options .pixel-button').forEach(btn => {
                        btn.classList.remove('bg-blue-600', 'border-blue-800'); // Remove destaque
                    });
                    // Adiciona destaque ao botão clicado
                    button.classList.add('bg-blue-600', 'border-blue-800');
                    gameData.selectedClassKey = key;
                    gameData.selectedClass = classInfo;
                    confirmClassBtn.disabled = false; // Habilita o botão de confirmar
                });
                classOptionsDiv.appendChild(button);
            }
        }

        /** Inicia o jogo após a seleção da classe. */
        function initializeGame() {
            if (!gameData.selectedClass) {
                showModal("Por favor, selecione uma classe antes de começar a aventura!");
                return;
            }

            // Esconde a tela inicial e o modal de seleção de classe
            startScreen.style.display = 'none';
            classSelectionModal.style.display = 'none';
            // Exibe a tela principal do jogo
            gameScreen.style.display = 'flex'; 

            // Inicializa as estatísticas do jogador com base na classe selecionada
            gameData.player.name = gameData.selectedClass.name;
            gameData.player.maxHp = gameData.selectedClass.baseHp;
            gameData.player.hp = gameData.selectedClass.baseHp; // HP inicial = HP máximo
            gameData.player.attack = gameData.selectedClass.baseAttack;
            gameData.player.defense = gameData.selectedClass.baseDefense;
            gameData.player.abilityName = gameData.selectedClass.ability;
            gameData.player.inventory = []; // Garante que o inventário comece vazio
            gameData.player.equippedWeapon = null; // Garante que comece sem arma equipada

            // Dá uma espada básica ao jogador no início
            addItemToInventory("Espada Curta de Iniciante");
            // Equipa a espada básica automaticamente
            useItem("Espada Curta de Iniciante");

            updatePlayerStats(); // Atualiza a interface com as stats do jogador
            renderInventory(); // Renderiza o inventário
            logEvent(`Bem-vindo, ${gameData.player.name}! Sua aventura começa agora.`, 'important');

            // Habilita apenas o botão de explorar no início
            exploreButton.disabled = false;
            attackButton.disabled = true;
            abilityButton.disabled = true;
            fleeButton.disabled = true;
        }

        // --- Listeners de Eventos ---

        // Ao clicar em "Escolher Classe" na tela inicial
        startGameBtn.addEventListener('click', () => {
            populateClassSelection(); // Preenche as opções de classe
            classSelectionModal.style.display = 'block'; // Exibe o modal de seleção de classe
        });

        // Ao clicar em "Confirmar Classe" no modal de seleção
        confirmClassBtn.addEventListener('click', initializeGame);

        // Botões de Ação do Jogo
        exploreButton.addEventListener('click', startCombat);
        attackButton.addEventListener('click', attackEnemy);
        abilityButton.addEventListener('click', handleAbility);
        fleeButton.addEventListener('click', fleeAttempt);

        // Inicializa o jogo quando a página é carregada
        document.addEventListener('DOMContentLoaded', () => {
            // Garante que a tela inicial esteja visível e a tela de jogo escondida no carregamento
            startScreen.style.display = 'flex';
            gameScreen.style.display = 'none';
            classSelectionModal.style.display = 'none';
            gameModal.style.display = 'none';
        });

    </script>
</body>
</html>
