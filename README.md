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

            <div class="panel">
                <h2 class="text-xl">🗺️ História</h2>
                <p class="text-sm">Há milênios, uma série de cavernas interconectadas conhecidas como Dungeons Cave surgiu após um cataclismo mágico. Dentro delas, residem criaturas ancestrais, artefatos proibidos e poderes capazes de alterar o mundo. A cada geração, heróis descem às profundezas em busca de glória, fortuna... ou redenção.</p>
            </div>

            <div class="panel">
                <h2 class="text-xl">🏹 Classes Jogáveis</h2>
                <table class="text-sm">
                    <thead>
                        <tr>
                            <th>Classe</th>
                            <th>Descrição</th>
                            <th>Habilidade Especial</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>Cavaleiro</td><td>Tanque, defesa alta, espada e escudo.</td><td>Escudo Imbatível — bloqueia 100% do dano por 5s.</td></tr>
                        <tr><td>Ladino</td><td>Ágil, furtivo, dano crítico alto.</td><td>Desaparecer — fica invisível por 7s.</td></tr>
                        <tr><td>Mago</td><td>Mestre dos elementos, dano mágico em área.</td><td>Tempestade Arcana — chuva de raios.</td></tr>
                        <tr><td>Clérigo</td><td>Suporte, cura, buffs e dano sagrado.</td><td>Bênção Divina — cura e remove efeitos negativos.</td></tr>
                        <tr><td>Bárbaro</td><td>Selvagem, dano físico bruto, pouca defesa.</td><td>Fúria Insana — dobra o dano por 10s, mas perde defesa.</td></tr>
                        <tr><td>Caçador</td><td>Longo alcance, armadilhas e animais de suporte.</td><td>Falcão Espreitador — invoca um falcão que ataca e revela inimigos ocultos.</td></tr>
                    </tbody>
                </table>
            </div>

            <div class="panel">
                <h2 class="text-xl">⚔️ Armas</h2>
                <table class="text-sm">
                    <thead>
                        <tr>
                            <th>Arma</th>
                            <th>Classes</th>
                            <th>Efeito Especial</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>Espada Longa</td><td>Cavaleiro</td><td>+10% chance de atordoar.</td></tr>
                        <tr><td>Adaga Sombria</td><td>Ladino</td><td>+20% de dano em ataques críticos.</td></tr>
                        <tr><td>Cajado Elemental</td><td>Mago</td><td>Conjura 2 feitiços ao mesmo tempo.</td></tr>
                        <tr><td>Martelo Sagrado</td><td>Clérigo</td><td>Cura aliados próximos a cada golpe.</td></tr>
                        <tr><td>Machado Duplo</td><td>Bárbaro</td><td>Chance de causar sangramento.</td></tr>
                        <tr><td>Arco de Ébano</td><td>Caçador</td><td>Tiros perfuram múltiplos inimigos.</td></tr>
                    </tbody>
                </table>
            </div>

            <div class="panel">
                <h2 class="text-xl">👥 Personagens Importantes</h2>
                <table class="text-sm">
                    <thead>
                        <tr>
                            <th>Nome</th>
                            <th>Função</th>
                            <th>Descrição</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>Eldric</td><td>Mestre da Guilda</td><td>Guia os aventureiros; ex-herói lendário.</td></tr>
                        <tr><td>Maelis</td><td>Mercadora</td><td>Vende poções, itens e armas raras.</td></tr>
                        <tr><td>Sif</td><td>Ferreiro</td><td>Forja armas e melhora equipamentos.</td></tr>
                        <tr><td>Lyria</td><td>Curandeira</td><td>Cura os aventureiros na cidade.</td></tr>
                        <tr><td>Darkan</td><td>Antagonista Final</td><td>Um ex-mago que virou um lich nas profundezas.</td></tr>
                    </tbody>
                </table>
            </div>

            <div class="panel">
                <h2 class="text-xl">🐉 Chefões (BOSS)</h2>
                <table class="text-sm">
                    <thead>
                        <tr>
                            <th>Dungeon</th>
                            <th>Boss</th>
                            <th>Mecânicas</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td>Caverna Sombria</td><td>Gorak, O Devorador</td><td>Devora jogadores, cria clones de sombras.</td></tr>
                        <tr><td>Túnel das Serpentes</td><td>Ssyra, A Rainha Víbora</td><td>Veneno em área, invoca serpentes menores.</td></tr>
                        <tr><td>Poço de Lava</td><td>Vulkanor, O Imolado</td><td>Pisos de lava, rajadas de fogo e erupções.</td></tr>
                        <tr><td>Abismo Gélido</td><td>Frostfang, O Congelador</td><td>Congela jogadores, cria espinhos de gelo.</td></tr>
                        <tr><td>Sala do Lich</td><td>Darkan, O Lich Supremo</td><td>Necromancia, invoca mortos, tempestade arcana.</td></tr>
                    </tbody>
                </table>
            </div>

            <div class="panel">
                <h2 class="text-xl">🕸️ Dungeons</h2>
                <ul class="list-disc list-inside text-sm">
                    <li>Caverna Sombria — iniciantes, nível 1-5.</li>
                    <li>Túnel das Serpentes — intermediário, nível 5-10.</li>
                    <li>Poço de Lava — nível 10-15, perigo constante.</li>
                    <li>Abismo Gélido — nível 15-20, ambiente hostil e traiçoeiro.</li>
                    <li>Sala do Lich (Darkan’s Sanctum) — nível 20+, dungeon final, cheia de puzzles, armadilhas e o boss final.</li>
                </ul>
            </div>

            <div class="panel">
                <h2 class="text-xl">🏆 Sistemas Adicionais</h2>
                <ul class="list-disc list-inside text-sm">
                    <li>🎯 Crafting de armas e armaduras.</li>
                    <li>💎 Loot aleatório com raridade (comum, raro, épico, lendário).</li>
                    <li>🛡️ Sistema de guildas e grupos.</li>
                    <li>🌟 Missões secundárias e eventos aleatórios nas dungeons.</li>
                    <li>🔥 Modo Hardcore (morte permanente).</li>
                </ul>
            </div>

            <button id="start-game-button" class="pixel-button mt-6">Iniciar Aventura</button>
        </div>

        <div id="game-screen">
            <h2 class="text-2xl">Aventura em Andamento!</h2>

            <div class="panel">
                <h3 class="text-lg">Seu Herói</h3>
                <div id="player-stats" class="player-stats">
                    </div>
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
            weapons: {
                longsword: { name: "Espada Longa", classes: ["knight"], effect: "+10% chance de atordoar.", attackBonus: 5 },
                darkdagger: { name: "Adaga Sombria", classes: ["rogue"], effect: "+20% de dano em ataques críticos.", attackBonus: 3 },
                elementalstaff: { name: "Cajado Elemental", classes: ["mage"], effect: "Conjura 2 feitiços ao mesmo tempo.", attackBonus: 7 },
                sacredhammer: { name: "Martelo Sagrado", classes: ["cleric"], effect: "Cura aliados próximos a cada golpe.", attackBonus: 4 },
                doubleaxe: { name: "Machado Duplo", classes: ["barbarian"], effect: "Chance de causar sangramento.", attackBonus: 6 },
                ebonybow: { name: "Arco de Ébano", classes: ["hunter"], effect: "Tiros perfuram múltiplos inimigos.", attackBonus: 5 }
            },
            dungeons: [
                { name: "Caverna Sombria", levels: "1-5", boss: "Gorak, O Devorador", enemyTypes: ["Goblin", "Morcego Gigante", "Aranha Venenosa"] },
                { name: "Túnel das Serpentes", levels: "5-10", boss: "Ssyra, A Rainha Víbora", enemyTypes: ["Serpente", "Naga", "Basilisco"] },
                { name: "Poço de Lava", levels: "10-15", boss: "Vulkanor, O Imolado", enemyTypes: ["Elemental de Fogo", "Salamandra", "Demônio Menor"] },
                { name: "Abismo Gélido", levels: "15-20", boss: "Frostfang, O Congelador", enemyTypes: ["Geleia Congelante", "Lobo do Gelo", "Gigante de Gelo"] },
                { name: "Sala do Lich (Darkan’s Sanctum)", levels: "20+", boss: "Darkan, O Lich Supremo", enemyTypes: ["Esqueleto", "Zumbi", "Fantasma", "Gárgula"] }
            ],
            bosses: {
                "Gorak, O Devorador": { hp: 200, attack: 25, defense: 10, mechanics: "Devora jogadores, cria clones de sombras." },
                "Ssyra, A Rainha Víbora": { hp: 300, attack: 30, defense: 12, mechanics: "Veneno em área, invoca serpentes menores." },
                "Vulkanor, O Imolado": { hp: 400, attack: 35, defense: 15, mechanics: "Pisos de lava, rajadas de fogo e erupções." },
                "Frostfang, O Congelador": { hp: 500, attack: 40, defense: 18, mechanics: "Congela jogadores, cria espinhos de gelo." },
                "Darkan, O Lich Supremo": { hp: 800, attack: 50, defense: 25, mechanics: "Necromancia, invoca mortos, tempestade arcana." }
            }
        };

        // Estado do jogo
        const gameState = {
            player: null,
            currentDungeon: null,
            currentEnemy: null,
            gameStarted: false,
            // Variáveis para controlar o modal
            modalResolve: null,
            modalReject: null
        };

        // Referências aos elementos HTML
        const startScreen = document.getElementById('start-screen');
        const gameScreen = document.getElementById('game-screen');
        const startGameButton = document.getElementById('start-game-button');
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

        /**
         * Exibe uma mensagem em um modal personalizado (substitui alert/confirm).
         * @param {string} message - A mensagem a ser exibida.
         * @returns {Promise<void>} Uma promessa que resolve quando o usuário clica em OK.
         */
        function showGameModal(message) {
            return new Promise((resolve, reject) => {
                modalMessage.textContent = message;
                gameModal.style.display = 'block';
                gameState.modalResolve = resolve;
                gameState.modalReject = reject;
            });
        }

        // Event listener para o botão OK do modal
        modalOkButton.addEventListener('click', () => 
        // Função para explorar e encontrar itens ou inimigos
function explore() {
    if (!gameState.gameStarted) return;

    const randomEvent = Math.random();
    if (randomEvent < 0.5) {
        // Encontrar um inimigo
        const enemyType = getRandomEnemy();
        startBattle(enemyType);
    } else {
        // Encontrar um item
        const item = getRandomItem();
        showGameModal(`Você encontrou uma ${item.name}! Efeito: ${item.effect}`);
    }
}

// Função para obter um inimigo aleatório
function getRandomEnemy() {
    const enemies = gameData.dungeons.find(d => d.name === gameState.currentDungeon).enemyTypes;
    return enemies[Math.floor(Math.random() * enemies.length)];
}

// Função para obter um item aleatório
function getRandomItem() {
    const items = Object.values(gameData.items);
    return items[Math.floor(Math.random() * items.length)];
}
