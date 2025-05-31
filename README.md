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

        /* Estilos para a mochila */
        .inventory {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            margin-top: 1rem;
        }

        .inventory-item {
            background-color: #1a242f;
            padding: 0.4rem 0.8rem;
            border-radius: 4px;
            border: 1px solid #555;
            display: flex;
            justify-content: space-between;
            align-items: center;
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
                <h3 class="mt-4">Tabela de Classes e Armas</h3>
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
                        <tr>
                            <td>Bárbaro</td>
                            <td>✔️</td>
                            <td>✔️</td>
                            <td>Usa quase todas as armas</td>
                        </tr>
                        <tr>
                            <td>Bardo</td>
                            <td>✔️</td>
                            <td>Poucas</td>
                            <td>Espada curta, florete, rapieira e besta leve</td>
                        </tr>
                        <tr>
                            <td>Clérigo</td>
                            <td>✔️</td>
                            <td>❌</td>
                            <td>Focado em armas simples</td>
                        </tr>
                        <tr>
                            <td>Druida</td>
                            <td>✔️ (parcial)</td>
                            <td>❌</td>
                            <td>Só armas que não sejam de metal: cajado, clava, foice curta, azagaia, lança, dardo, machado de mão, funda</td>
                        </tr>
                        <tr>
                            <td>Guerreiro</td>
                            <td>✔️</td>
                            <td>✔️</td>
                            <td>Usa todas as armas</td>
                        </tr>
                        <tr>
                            <td>Monge</td>
                            <td>✔️ (parcial)</td>
                            <td>❌</td>
                            <td>Armas simples e armas de monge: bastão, espada curta, foice curta, etc.</td>
                        </tr>
                        <tr>
                            <td>Paladino</td>
                            <td>✔️</td>
                            <td>✔️</td>
                            <td>Usa todas as armas</td>
                        </tr>
                        <tr>
                            <td>Patrulheiro</td>
                            <td>✔️</td>
                            <td>✔️</td>
                            <td>Usa todas as armas</td>
                        </tr>
                        <tr>
                            <td>Ladino</td>
                            <td>✔️</td>
                            <td>Poucas</td>
                            <td>Espada curta, florete, rapieira e besta de mão</td>
                        </tr>
                        <tr>
                            <td>Feiticeiro</td>
                            <td>✔️ (muito básico)</td>
                            <td>❌</td>
                            <td>Adaga, bastão, funda, besta leve, dardo</td>
                        </tr>
                        <tr>
                            <td>Bruxo</td>
                            <td>✔️ (muito básico)</td>
                            <td>❌</td>
                            <td>Adaga, bastão, funda, besta leve, dardo</td>
                        </tr>
                        <tr>
                            <td>Mago</td>
                            <td>✔️ (muito básico)</td>
                            <td>❌</td>
                            <td>Adaga, bastão, funda, besta leve, dardo</td>
                        </tr>
                    </tbody>
                </table>
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
                <h3 class="text-lg">Mochila</h3>
                <div id="inventory" class="inventory"></div>
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
                boss: { name: "Gorak, O Devorador", hp: 100, attack: 15 }, // Boss padrão
                ancientDragon: { name: "🐉 Dragão Ancião", hp: Math.floor(Math.random() * (5000 - 1000 + 1)) + 1000, attack: Math.floor(Math.random() * (350 - 100 + 1)) + 100 },
                lichKing: { name: "👑 Rei Lich", hp: Math.floor(Math.random() * (3000 - 800 + 1)) + 800, attack: Math.floor(Math.random() * (250 - 120 + 1)) + 120 },
                infernoLord: { name: "🔥 Senhor do Inferno", hp: Math.floor(Math.random() * (6000 - 1500 + 1)) + 1500, attack: Math.floor(Math.random() * (400 - 150 + 1)) + 150 },
                worldDevourer: { name: "🕷️ Devoradora de Mundos", hp: Math.floor(Math.random() * (8000 - 2000 + 1)) + 2000, attack: Math.floor(Math.random() * (500 - 200 + 1)) + 200 },
                timeAvatar: { name: "⏳ Avatar do Tempo", hp: Math.floor(Math.random() * (4000 - 1200 + 1)) + 1200, attack: Math.floor(Math.random() * (350 - 150 + 1)) + 150 },
                boneColossus: { name: "🦴 Colosso de Ossos", hp: Math.floor(Math.random() * (7000 - 1800 + 1)) + 1800, attack: Math.floor(Math.random() * (400 - 180 + 1)) + 180 },
                elementalTitan: { name: "🌋 Titã Elemental", hp: Math.floor(Math.random() * (6000 - 1500 + 1)) + 1500, attack: Math.floor(Math.random() * (400 - 150 + 1)) + 150 },
                bloodGod: { name: "🩸 Deus Sangrento", hp: Math.floor(Math.random() * (9000 - 2500 + 1)) + 2500, attack: Math.floor(Math.random() * (500 - 200 + 1)) + 200 },
                voidEntity: { name: "🌑 Entidade do Vazio", hp: Infinity, attack: Math.floor(Math.random() * (600 - 250 + 1)) + 250 } // HP infinito
            },
            dungeons: [
                { name: "Caverna Sombria", levels: "1-5", boss: "Gorak, O Devorador", enemy
