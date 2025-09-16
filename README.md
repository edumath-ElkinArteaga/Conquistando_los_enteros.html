<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reino de los Números - Batalla Épica</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Comic Sans MS', cursive, Arial, sans-serif;
            background: linear-gradient(45deg, #1e3c72, #2a5298, #6dd5ed, #ffffff);
            background-size: 400% 400%;
            animation: gradientShift 8s ease infinite;
            overflow-x: hidden;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .screen { display: none; min-height: 100vh; }
        .screen.active { display: block; }
        
        /* Pantalla de Inicio */
        .inicio-screen {
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            color: white;
        }
        
        .inicio-card {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(20px);
            border-radius: 30px;
            padding: 50px;
            border: 3px solid rgba(255,255,255,0.3);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            animation: float 3s ease-in-out infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }
        
        .titulo-principal {
            font-size: 3.5em;
            background: linear-gradient(45deg, #FFD700, #FFA500, #FF6347);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
        
        .personaje-inicio {
            font-size: 8em;
            animation: bounce 1.5s infinite;
            margin: 20px 0;
        }
        
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-30px); }
            60% { transform: translateY(-15px); }
        }
        
        .input-aventurero {
            padding: 20px;
            font-size: 20px;
            border: 3px solid #FFD700;
            border-radius: 15px;
            text-align: center;
            margin: 20px;
            background: rgba(255,255,255,0.9);
            font-weight: bold;
            width: 300px;
        }
        
        .btn-aventura {
            background: linear-gradient(45deg, #FF6B6B, #4ECDC4, #45B7D1);
            color: white;
            border: none;
            padding: 20px 40px;
            font-size: 22px;
            font-weight: bold;
            border-radius: 25px;
            cursor: pointer;
            margin: 20px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
            transition: all 0.3s;
        }
        
        .btn-aventura:hover {
            transform: scale(1.1);
        }
        
        .btn-aventura:disabled {
            background: #666;
            cursor: not-allowed;
            transform: none;
        }
        
        /* Mapa del Reino */
        .mapa-reino {
            background: linear-gradient(180deg, #87CEEB 0%, #98FB98 50%, #DEB887 100%);
            min-height: 100vh;
            position: relative;
        }
        
        .hud-aventurero {
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(0,0,0,0.8);
            color: white;
            padding: 15px;
            border-radius: 15px;
            font-weight: bold;
            z-index: 1000;
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 10px 0;
        }
        
        .stat-item {
            text-align: center;
            padding: 5px;
            background: rgba(255,255,255,0.1);
            border-radius: 8px;
        }
        
        .mapa-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 50px 20px;
        }
        
        .titulo-reino {
            font-size: 3em;
            color: #2C3E50;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            margin-bottom: 30px;
            text-align: center;
        }
        
        .aventurero {
            font-size: 4em;
            animation: walk 2s infinite;
            margin: 20px 0;
        }
        
        @keyframes walk {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }
        
        .camino {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            margin: 30px 0;
            max-width: 800px;
        }
        
        .nivel {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-weight: bold;
            color: white;
            font-size: 18px;
            transition: all 0.3s;
            position: relative;
            border: 4px solid;
        }
        
        .nivel:hover {
            transform: scale(1.1) rotate(5deg);
        }
        
        .nivel.desbloqueado {
            background: linear-gradient(45deg, #32CD32, #228B22);
            border-color: #FFD700;
            animation: pulse 2s infinite;
        }
        
        .nivel.bloqueado {
            background: linear-gradient(45deg, #696969, #2F4F4F);
            border-color: #A9A9A9;
            cursor: not-allowed;
            opacity: 0.6;
        }
        
        .nivel.completado {
            background: linear-gradient(45deg, #FFD700, #FFA500);
            border-color: #FF6347;
        }
        
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(50, 205, 50, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(50, 205, 50, 0); }
            100% { box-shadow: 0 0 0 0 rgba(50, 205, 50, 0); }
        }
        
        .nivel-emoji { font-size: 2em; margin-bottom: 5px; }
        .nivel-nombre { font-size: 12px; text-align: center; }
        
        .estrella-progreso {
            position: absolute;
            top: -10px;
            right: -10px;
            font-size: 1.5em;
            animation: twinkle 1s infinite;
        }
        
        @keyframes twinkle {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.5; transform: scale(1.2); }
        }
        
        .dialogo-npc {
            background: rgba(0,0,0,0.9);
            color: white;
            padding: 25px;
            border-radius: 20px;
            margin: 20px 0;
            border: 3px solid #FFD700;
        }
        
        .npc-sprite {
            font-size: 3em;
            display: inline-block;
            margin-right: 15px;
            animation: talk 1s infinite;
        }
        
        @keyframes talk {
            0%, 100% { transform: rotateZ(0deg); }
            50% { transform: rotateZ(5deg); }
        }
        
        .texto-dialogo {
            display: inline-block;
            vertical-align: top;
            max-width: 500px;
            font-size: 1.1em;
            line-height: 1.4;
        }
        
        /* Batalla Épica */
        .batalla-screen {
            background: linear-gradient(45deg, #1a1a2e, #16213e, #0f3460);
            color: white;
            min-height: 100vh;
            position: relative;
            overflow: hidden;
        }
        
        .campo-batalla {
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 60vh;
            padding: 40px;
            position: relative;
        }
        
        .mago-contenedor, .monstruo-contenedor {
            display: flex;
            flex-direction: column;
            align-items: center;
            z-index: 10;
        }
        
        .mago-sprite, .monstruo-sprite {
            font-size: 8em;
            margin-bottom: 20px;
            transition: all 0.3s;
        }
        
        .mago-sprite {
            animation: magoIdle 3s infinite;
        }
        
        .monstruo-sprite {
            animation: monstruoIdle 4s infinite;
        }
        
        @keyframes magoIdle {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-10px) scale(1.05); }
        }
        
        @keyframes monstruoIdle {
            0%, 100% { transform: translateX(0) rotateZ(0deg); }
            25% { transform: translateX(-5px) rotateZ(-2deg); }
            75% { transform: translateX(5px) rotateZ(2deg); }
        }
        
        .vida-contenedor {
            width: 200px;
            text-align: center;
        }
        
        .vida-barra {
            width: 100%;
            height: 25px;
            background: #333;
            border-radius: 15px;
            overflow: hidden;
            margin: 10px 0;
            border: 3px solid #fff;
        }
        
        .vida-fill {
            height: 100%;
            transition: width 0.8s ease;
            border-radius: 12px;
        }
        
        .vida-mago {
            background: linear-gradient(90deg, #4FC3F7, #29B6F6, #03A9F4);
        }
        
        .vida-monstruo {
            background: linear-gradient(90deg, #FF5722, #FF7043, #FF8A65);
        }
        
        .panel-batalla {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(0,0,0,0.9);
            padding: 30px;
            border-top: 3px solid #FFD700;
        }
        
        .contador-batalla {
            text-align: center;
            margin-bottom: 20px;
            font-size: 1.2em;
        }
        
        .aciertos { color: #4CAF50; }
        .intentos { color: #FF9800; }
        
        .ejercicio-contenedor {
            background: linear-gradient(45deg, #667eea, #764ba2);
            border-radius: 20px;
            padding: 25px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .ejercicio-texto {
            font-size: 1.8em;
            font-weight: bold;
            margin-bottom: 20px;
        }
        
        .respuesta-contenedor {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .input-respuesta {
            padding: 15px;
            font-size: 1.5em;
            border: 3px solid #FFD700;
            border-radius: 10px;
            text-align: center;
            width: 150px;
            font-weight: bold;
            background: rgba(255,255,255,0.9);
        }
        
        .btn-lanzar-hechizo {
            background: linear-gradient(45deg, #9C27B0, #673AB7);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.2em;
            font-weight: bold;
            border-radius: 15px;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(156,39,176,0.4);
            transition: all 0.3s;
        }
        
        .btn-lanzar-hechizo:hover {
            transform: scale(1.1);
            box-shadow: 0 8px 25px rgba(156,39,176,0.6);
        }
        
        .btn-lanzar-hechizo:disabled {
            background: #666;
            cursor: not-allowed;
            transform: none;
        }
        
        .btn-volver {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(0,0,0,0.7);
            color: white;
            border: none;
            padding: 15px 25px;
            border-radius: 15px;
            cursor: pointer;
            font-weight: bold;
            z-index: 1000;
        }
        
        /* Animaciones de Combate */
        .hechizo-proyectil {
            position: absolute;
            font-size: 3em;
            z-index: 100;
            pointer-events: none;
        }
        
        .ecuacion-resultado {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.9);
            color: white;
            padding: 20px;
            border-radius: 15px;
            font-size: 1.5em;
            font-weight: bold;
            z-index: 200;
            border: 3px solid #FFD700;
            animation: mostrarResultado 2s ease-out;
        }
        
        @keyframes mostrarResultado {
            0% { 
                transform: translate(-50%, -50%) scale(0); 
                opacity: 0; 
            }
            50% { 
                transform: translate(-50%, -50%) scale(1.2); 
                opacity: 1; 
            }
            100% { 
                transform: translate(-50%, -50%) scale(1); 
                opacity: 1; 
            }
        }
        
        /* Animaciones simplificadas para móviles */
        @keyframes magoAtaque {
            0% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-15px) scale(1.1); }
            100% { transform: translateY(0) scale(1); }
        }
        
        @keyframes monstruoDano {
            0% { transform: translateX(0) scale(1); }
            50% { transform: translateX(-10px) scale(0.9); }
            100% { transform: translateX(0) scale(1); }
        }
        
        @keyframes monstruoAtaque {
            0% { transform: translateX(0) scale(1); }
            50% { transform: translateX(-20px) scale(1.2); }
            100% { transform: translateX(0) scale(1); }
        }
        
        @keyframes magoDano {
            0% { transform: translateY(0) scale(1); }
            50% { transform: translateY(10px) scale(0.9); }
            100% { transform: translateY(0) scale(1); }
        }
        
        .mago-atacando { animation: magoAtaque 0.6s ease-out !important; }
        .monstruo-danado { animation: monstruoDano 0.6s ease-out !important; }
        .monstruo-atacando { animation: monstruoAtaque 0.6s ease-out !important; }
        .mago-danado { animation: magoDano 0.6s ease-out !important; }
        
        .explosion-efecto {
            position: absolute;
            font-size: 3em;
            z-index: 150;
            pointer-events: none;
            animation: explosion 0.6s ease-out forwards;
        }
        
        @keyframes explosion {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(1.5); opacity: 0; }
        }
        
        .risa-efecto {
            position: absolute;
            top: 20%;
            left: 70%;
            font-size: 2em;
            z-index: 150;
            animation: risa 1s ease-out forwards;
        }
        
        @keyframes risa {
            0% { transform: scale(0); opacity: 0; }
            50% { transform: scale(1); opacity: 1; }
            100% { transform: scale(0.8); opacity: 0; }
        }
        
        .mensaje-final {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0,0,0,0.95);
            color: white;
            padding: 40px;
            border-radius: 25px;
            text-align: center;
            z-index: 300;
            border: 4px solid #FFD700;
            animation: aparecerFinal 0.5s ease-out;
        }
        
        @keyframes aparecerFinal {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
        }
        
        .oculto { display: none !important; }
        
        /* OPTIMIZACIÓN MÓVIL */
        @media (max-width: 768px) {
            .inicio-card { padding: 30px 20px; margin: 10px; }
            .titulo-principal { font-size: 2.5em; }
            .personaje-inicio { font-size: 5em; }
            .input-aventurero { width: 280px; font-size: 16px; }
            .btn-aventura { width: 280px; min-height: 60px; }
            
            .campo-batalla { 
                flex-direction: column; 
                height: 40vh; 
                padding: 20px 10px; 
            }
            .mago-sprite, .monstruo-sprite { font-size: 4em; }
            .vida-contenedor { width: 150px; }
            
            .panel-batalla { padding: 15px; max-height: 60vh; }
            .ejercicio-texto { font-size: 1.4em; }
            .respuesta-contenedor { flex-direction: column; gap: 15px; }
            .input-respuesta { width: 200px; min-height: 60px; }
            .btn-lanzar-hechizo { width: 200px; min-height: 60px; }
            
            .camino { 
                display: grid; 
                grid-template-columns: repeat(2, 1fr); 
                gap: 15px; 
            }
            .nivel { width: 100px; height: 100px; }
        }
        
        /* Mejoras táctiles */
        button, input, .nivel {
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
        }
        
        input[type="number"] {
            -moz-appearance: textfield;
        }
        
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
    </style>
</head>
<body>
    <!-- Pantalla de Inicio -->
    <div id="inicio" class="screen active inicio-screen">
        <div class="inicio-card">
            <h1 class="titulo-principal">👑 REINO DE LOS NÚMEROS</h1>
            <div class="personaje-inicio">🧙‍♂️</div>
            <h2 style="color: #FFD700; margin-bottom: 30px;">La Gran Aventura Matemática</h2>
            <p style="font-size: 1.2em; margin-bottom: 30px; color: white;">
                ¡Embárcate en una épica aventura para salvar el Reino de los Números!<br>
                Usa la magia de las matemáticas para derrotar a los monstruos del caos.
            </p>
            <input type="text" id="nombreAventurero" class="input-aventurero" 
                   placeholder="Nombre del Mago..." maxlength="15" 
                   oninput="validarNombre()">
            <br>
            <button id="btnComenzarAventura" class="btn-aventura" disabled 
                    onclick="comenzarAventura()">
                ⚔️ Comenzar Aventura
            </button>
        </div>
    </div>

    <!-- Mapa del Reino -->
    <div id="mapa" class="screen mapa-reino">
        <div class="hud-aventurero">
            <div><strong id="nombreJugador">Mago</strong> 🧙‍♂️</div>
            <div class="stats-grid">
                <div class="stat-item"><div>💰</div><div id="monedas">0</div></div>
                <div class="stat-item"><div>⚡</div><div id="energia">100</div></div>
                <div class="stat-item"><div>🏆</div><div id="victorias">0</div></div>
                <div class="stat-item"><div>🔮</div><div id="nivel">1</div></div>
                <div class="stat-item"><div>⭐</div><div id="estrellas">0</div></div>
                <div class="stat-item"><div>🛡️</div><div id="poder">100</div></div>
            </div>
        </div>

        <div class="mapa-container">
            <h1 class="titulo-reino">🏰 Reino de los Números</h1>
            
            <div class="dialogo-npc">
                <span class="npc-sprite">🧙‍♂️</span>
                <div class="texto-dialogo">
                    <strong>Maestro Pitágoras:</strong> ¡Bienvenido joven mago! Los Monstruos del Caos han 
                    invadido nuestro reino. Necesitas 15 hechizos matemáticos correctos de 20 intentos 
                    para derrotar a cada monstruo. ¡La magia de los números es tu arma más poderosa!
                </div>
            </div>

            <div class="aventurero">🧙‍♂️</div>

            <div class="camino">
                <div class="nivel desbloqueado" data-mundo="1" data-nivel="1">
                    <div class="nivel-emoji">🐺</div>
                    <div class="nivel-nombre">Lobo del<br>Caos</div>
                </div>
                
                <div class="nivel bloqueado" data-mundo="1" data-nivel="2">
                    <div class="nivel-emoji">🦉</div>
                    <div class="nivel-nombre">Búho<br>Corrupto</div>
                </div>
                
                <div class="nivel bloqueado" data-mundo="1" data-nivel="3">
                    <div class="nivel-emoji">👻</div>
                    <div class="nivel-nombre">Fantasma<br>Números</div>
                </div>
                
                <div class="nivel bloqueado" data-mundo="1" data-nivel="4">
                    <div class="nivel-emoji">🐉</div>
                    <div class="nivel-nombre">Dragón<br>Matemático</div>
                </div>

                <div class="nivel bloqueado" data-mundo="2" data-nivel="1">
                    <div class="nivel-emoji">🥶</div>
                    <div class="nivel-nombre">Yeti<br>Helado</div>
                </div>
                
                <div class="nivel bloqueado" data-mundo="2" data-nivel="2">
                    <div class="nivel-emoji">🤖</div>
                    <div class="nivel-nombre">Robot<br>Defectuoso</div>
                </div>
                
                <div class="nivel bloqueado" data-mundo="2" data-nivel="3">
                    <div class="nivel-emoji">🚀</div>
                    <div class="nivel-nombre">Comandante<br>Espacial</div>
                </div>
                
                <div class="nivel bloqueado" data-mundo="2" data-nivel="4">
                    <div class="nivel-emoji">👽</div>
                    <div class="nivel-nombre">Emperador<br>Alien</div>
                </div>
            </div>

            <div class="dialogo-npc">
                <span class="npc-sprite">🧚‍♀️</span>
                <div class="texto-dialogo">
                    <strong>Hada de las Ecuaciones:</strong> Recuerda joven mago: cada ejercicio matemático 
                    correcto se convierte en un poderoso hechizo. ¡Apunta bien y que la fuerza de los 
                    números te acompañe!
                </div>
            </div>
        </div>
    </div>

    <!-- Batalla Épica -->
    <div id="batalla" class="screen batalla-screen">
        <button class="btn-volver" onclick="volverMapa()">🏠 Volver al Reino</button>
        
        <div class="campo-batalla">
            <div class="mago-contenedor">
                <div id="magoSprite" class="mago-sprite">🧙‍♂️</div>
                <div class="vida-contenedor">
                    <div><strong id="nombreMago">Mago</strong></div>
                    <div class="vida-barra">
                        <div class="vida-fill vida-mago" id="vidaMago" style="width: 100%;"></div>
                    </div>
                    <div>Poder Mágico: <span id="poderMago">100</span>%</div>
                </div>
            </div>

            <div class="monstruo-contenedor">
                <div id="monstruoSprite" class="monstruo-sprite">🐺</div>
                <div class="vida-contenedor">
                    <div><strong id="nombreMonstruo">Lobo del Caos</strong></div>
                    <div class="vida-barra">
                        <div class="vida-fill vida-monstruo" id="vidaMonstruo" style="width: 100%;"></div>
                    </div>
                    <div>Nivel: <span id="nivelMonstruo">1</span></div>
                </div>
            </div>
        </div>

        <div class="panel-batalla">
            <div class="contador-batalla">
                <span class="aciertos">Hechizos Acertados: <strong id="aciertos">0</strong>/15</span>
                &nbsp;&nbsp;&nbsp;
                <span class="intentos">Intentos: <strong id="intentos">0</strong>/20</span>
            </div>

            <div class="ejercicio-contenedor">
                <div class="ejercicio-texto" id="ejercicioTexto">
                    27 + 15 = ?
                </div>
                
                <div class="respuesta-contenedor">
                    <input type="number" id="respuestaInput" class="input-respuesta" 
                           placeholder="?" onkeypress="if(event.key==='Enter') lanzarHechizo()">
                    <button id="btnLanzarHechizo" class="btn-lanzar-hechizo" onclick="lanzarHechizo()">
                        ⚡ LANZAR HECHIZO
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Variables globales
        let jugador = {
            nombre: '', monedas: 0, energia: 100,
            victorias: 0, nivel: 1, estrellas: 0, poder: 100
        };

        let nivelesDesbloqueados = { 1: [1], 2: [] };
        let nivelesCompletados = { 1: [], 2: [] };
        let batalla = {
            aciertos: 0, intentos: 0, ejercicioActual: 0,
            vidaMago: 100, vidaMonstruo: 100, 
            monstruoActual: null, ejercicios: [], enBatalla: false
        };

        const monstruos = {
            1: {
                1: { nombre: "Lobo del Caos", sprite: "🐺", nivel: 1 },
                2: { nombre: "Búho Corrupto", sprite: "🦉", nivel: 2 },
                3: { nombre: "Fantasma de Números", sprite: "👻", nivel: 3 },
                4: { nombre: "Dragón Matemático", sprite: "🐉", nivel: 4 }
            },
            2: {
                1: { nombre: "Yeti Helado", sprite: "🥶", nivel: 5 },
                2: { nombre: "Robot Defectuoso", sprite: "🤖", nivel: 6 },
                3: { nombre: "Comandante Espacial", sprite: "🚀", nivel: 7 },
                4: { nombre: "Emperador Alien", sprite: "👽", nivel: 8 }
            }
        };

        // FUNCIÓN DE LIMPIEZA CRÍTICA PARA MÓVILES
        function limpiarMemoria() {
            ['.hechizo-proyectil', '.explosion-efecto', '.risa-efecto', '.ecuacion-resultado']
            .forEach(selector => {
                document.querySelectorAll(selector).forEach(el => {
                    if (el.parentNode) el.parentNode.removeChild(el);
                });
            });
            
            if (batalla.ejercicios) batalla.ejercicios = [];
            if (window.gc) window.gc(); // Forzar garbage collection en móviles
        }

        // Generación de ejercicios (manteniendo los números enteros originales)
        function generarEjercicios(mundo, nivel) {
            const nivelGlobal = (mundo - 1) * 4 + nivel;
            
            if (nivelGlobal === 1) {
                return [
                    { texto: "(-5) + 8 = ?", respuesta: 3 },
                    { texto: "12 + (-7) = ?", respuesta: 5 },
                    { texto: "(-15) + (-6) = ?", respuesta: -21 },
                    { texto: "9 - (-4) = ?", respuesta: 13 },
                    { texto: "(-8) - 5 = ?", respuesta: -13 },
                    { texto: "(-3) - (-7) = ?", respuesta: 4 },
                    { texto: "14 + (-9) = ?", respuesta: 5 },
                    { texto: "(-11) + 6 = ?", respuesta: -5 },
                    { texto: "(-12) + (-8) = ?", respuesta: -20 },
                    { texto: "7 - (-5) = ?", respuesta: 12 },
                    { texto: "(-6) - 4 = ?", respuesta: -10 },
                    { texto: "(-2) - (-9) = ?", respuesta: 7 },
                    { texto: "18 + (-12) = ?", respuesta: 6 },
                    { texto: "(-17) + 11 = ?", respuesta: -6 },
                    { texto: "(-4) + (-13) = ?", respuesta: -17 },
                    { texto: "15 - (-8) = ?", respuesta: 23 },
                    { texto: "(-14) - 3 = ?", respuesta: -17 },
                    { texto: "(-1) - (-10) = ?", respuesta: 9 },
                    { texto: "(-19) + 25 = ?", respuesta: 6 },
                    { texto: "8 - (-11) = ?", respuesta: 19 }
                ].sort(() => Math.random() - 0.5);
            }
            
            if (nivelGlobal === 2) {
                return [
                    { texto: "(-25) + 18 = ?", respuesta: -7 },
                    { texto: "32 + (-19) = ?", respuesta: 13 },
                    { texto: "(-14) + (-16) = ?", respuesta: -30 },
                    { texto: "15 - (-12) = ?", respuesta: 27 },
                    { texto: "(-20) - 7 = ?", respuesta: -27 },
                    { texto: "(-6) - (-15) = ?", respuesta: 9 },
                    { texto: "(-28) + 21 = ?", respuesta: -7 },
                    { texto: "35 + (-23) = ?", respuesta: 12 },
                    { texto: "(-17) + (-19) = ?", respuesta: -36 },
                    { texto: "24 - (-16) = ?", respuesta: 40 },
                    { texto: "(-31) - 9 = ?", respuesta: -40 },
                    { texto: "(-8) - (-22) = ?", respuesta: 14 },
                    { texto: "(-33) + 27 = ?", respuesta: -6 },
                    { texto: "41 + (-29) = ?", respuesta: 12 },
                    { texto: "(-22) + (-14) = ?", respuesta: -36 },
                    { texto: "18 - (-25) = ?", respuesta: 43 },
                    { texto: "(-26) - 8 = ?", respuesta: -34 },
                    { texto: "(-5) - (-21) = ?", respuesta: 16 },
                    { texto: "(-37) + 42 = ?", respuesta: 5 },
                    { texto: "29 - (-18) = ?", respuesta: 47 }
                ].sort(() => Math.random() - 0.5);
            }
            
            // Continúa con los demás niveles...
            return [{ texto: "1 + 1 = ?", respuesta: 2 }]; // Fallback
        }

        // Funciones principales simplificadas
        function validarNombre() {
            const input = document.getElementById('nombreAventurero');
            const boton = document.getElementById('btnComenzarAventura');
            if (input && boton) boton.disabled = !input.value.trim();
        }

        function comenzarAventura() {
            jugador.nombre = document.getElementById('nombreAventurero').value.trim();
            if (!jugador.nombre) return;
            
            document.getElementById('nombreJugador').textContent = jugador.nombre;
            document.getElementById('nombreMago').textContent = jugador.nombre;
            actualizarHUD();
            mostrarPantalla('mapa');
        }

        function mostrarPantalla(pantallaId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(pantallaId).classList.add('active');
        }

        function actualizarHUD() {
            ['monedas', 'energia', 'victorias', 'nivel', 'estrellas', 'poder']
            .forEach(stat => {
                const elem = document.getElementById(stat);
                if (elem) elem.textContent = jugador[stat];
            });
        }

        // INICIAR BATALLA SIMPLIFICADO PARA MÓVILES
        function iniciarBatalla(mundo, nivel) {
            limpiarMemoria(); // Crucial para móviles
            
            // Reset total del estado
            batalla = {
                aciertos: 0, intentos: 0, ejercicioActual: 0,
                vidaMago: 100, vidaMonstruo: 100, 
                monstruoActual: monstruos[mundo][nivel],
                ejercicios: generarEjercicios(mundo, nivel),
                enBatalla: true
            };

            // Actualizar UI de forma segura
            const updates = {
                'monstruoSprite': batalla.monstruoActual.sprite,
                'nombreMonstruo': batalla.monstruoActual.nombre,
                'nivelMonstruo': batalla.monstruoActual.nivel,
                'poderMago': '100'
            };
            
            Object.entries(updates).forEach(([id, value]) => {
                const elem = document.getElementById(id);
                if (elem) elem.textContent = value;
            });

            ['vidaMago', 'vidaMonstruo'].forEach(id => {
                const elem = document.getElementById(id);
                if (elem) elem.style.width = '100%';
            });

            cargarSiguienteEjercicio();
            mostrarPantalla('batalla');
        }

        function cargarSiguienteEjercicio() {
            if (batalla.ejercicioActual < batalla.ejercicios.length) {
                const ejercicio = batalla.ejercicios[batalla.ejercicioActual];
                document.getElementById('ejercicioTexto').textContent = ejercicio.texto;
                document.getElementById('respuestaInput').value = '';
                actualizarContadores();
            }
        }

        function actualizarContadores() {
            document.getElementById('aciertos').textContent = batalla.aciertos;
            document.getElementById('intentos').textContent = batalla.intentos;
        }

        function lanzarHechizo() {
            if (!batalla.enBatalla) return;
            
            const respuesta = parseInt(document.getElementById('respuestaInput').value);
            if (isNaN(respuesta)) return;
            
            const ejercicio = batalla.ejercicios[batalla.ejercicioActual];
            const correcto = respuesta === ejercicio.respuesta;
            
            batalla.intentos++;
            document.getElementById('btnLanzarHechizo').disabled = true;
            
            // Animación simplificada para móviles
            setTimeout(() => {
                if (correcto) {
                    batalla.aciertos++;
                    batalla.vidaMonstruo = Math.max(0, batalla.vidaMonstruo - (100/15));
                    document.getElementById('vidaMonstruo').style.width = batalla.vidaMonstruo + '%';
                } else {
                    batalla.vidaMago = Math.max(0, batalla.vidaMago - 16.67);
                    document.getElementById('vidaMago').style.width = batalla.vidaMago + '%';
                    document.getElementById('poderMago').textContent = Math.round(batalla.vidaMago);
                }
                
                // Verificar estado
                if (batalla.aciertos >= 15) {
                    setTimeout(mostrarVictoria, 500);
                } else if (batalla.intentos >= 20 || batalla.vidaMago <= 0) {
                    setTimeout(mostrarDerrota, 500);
                } else {
                    batalla.ejercicioActual++;
                    setTimeout(() => {
                        cargarSiguienteEjercicio();
                        document.getElementById('btnLanzarHechizo').disabled = false;
                    }, 800);
                }
            }, 300);
        }

        function mostrarVictoria() {
            batalla.enBatalla = false;
            limpiarMemoria();
            
            // Actualizar progreso
            jugador.monedas += 200;
            jugador.victorias++;
            jugador.estrellas += 10;
            
            const mundoActual = Math.ceil(batalla.monstruoActual.nivel / 4);
            const nivelActual = ((batalla.monstruoActual.nivel - 1) % 4) + 1;
            
            if (!nivelesCompletados[mundoActual].includes(nivelActual)) {
                nivelesCompletados[mundoActual].push(nivelActual);
            }
            
            if (nivelActual < 4 && !nivelesDesbloqueados[mundoActual].includes(nivelActual + 1)) {
                nivelesDesbloqueados[mundoActual].push(nivelActual + 1);
            } else if (mundoActual === 1 && !nivelesDesbloqueados[2].includes(1)) {
                nivelesDesbloqueados[2].push(1);
            }
            
            mostrarMensaje('¡VICTORIA ÉPICA!', `¡Has derrotado a ${batalla.monstruoActual.nombre}!`, true);
        }

        function mostrarDerrota() {
            batalla.enBatalla = false;
            mostrarMensaje('DERROTA...', `${batalla.monstruoActual.nombre} ha sido más fuerte.`, false);
        }

        function mostrarMensaje(titulo, texto, esVictoria) {
            const mensaje = document.createElement('div');
            mensaje.className = 'mensaje-final';
            mensaje.innerHTML = `
                <h2 style="color: ${esVictoria ? '#FFD700' : '#F44336'};">${titulo}</h2>
                <p>${texto}</p>
                <button onclick="cerrarMensaje()" style="
                    background: ${esVictoria ? '#4CAF50' : '#FF9800'};
                    color: white; border: none; padding: 15px 30px;
                    font-size: 18px; border-radius: 10px; margin-top: 20px;
                ">${esVictoria ? '¡Continuar!' : 'Reintentar'}</button>
            `;
            document.body.appendChild(mensaje);
            
            if (esVictoria) {
                actualizarHUD();
                setTimeout(actualizarMapaNiveles, 1000);
            }
        }

        function cerrarMensaje() {
            const mensaje = document.querySelector('.mensaje-final');
            if (mensaje) mensaje.remove();
            limpiarMemoria();
            volverMapa();
        }

        function actualizarMapaNiveles() {
            limpiarMemoria();
            
            document.querySelectorAll('.nivel').forEach(nivel => {
                const mundo = parseInt(nivel.dataset.mundo);
                const nivelNum = parseInt(nivel.dataset.nivel);
                
                // Recrear event listener para evitar duplicación
                const nuevo = nivel.cloneNode(true);
                nivel.parentNode.replaceChild(nuevo, nivel);
                
                nuevo.addEventListener('click', function() {
                    if (this.classList.contains('desbloqueado')) {
                        iniciarBatalla(parseInt(this.dataset.mundo), parseInt(this.dataset.nivel));
                    }
                });
                
                // Actualizar estado
                if (nivelesDesbloqueados[mundo]?.includes(nivelNum)) {
                    nuevo.classList.remove('bloqueado');
                    nuevo.classList.add('desbloqueado');
                }
                
                if (nivelesCompletados[mundo]?.includes(nivelNum)) {
                    nuevo.classList.remove('desbloqueado');
                    nuevo.classList.add('completado');
                    
                    if (!nuevo.querySelector('.estrella-progreso')) {
                        const estrella = document.createElement('div');
                        estrella.className = 'estrella-progreso';
                        estrella.textContent = '⭐';
                        nuevo.appendChild(estrella);
                    }
                }
            });
        }

        function volverMapa() {
            batalla.enBatalla = false;
            limpiarMemoria();
            mostrarPantalla('mapa');
        }

        // Inicialización
        document.addEventListener('DOMContentLoaded', function() {
            // Configurar niveles iniciales
            document.querySelectorAll('.nivel').forEach(nivel => {
                nivel.addEventListener('click', function() {
                    if (this.classList.contains('desbloqueado')) {
                        iniciarBatalla(parseInt(this.dataset.mundo), parseInt(this.dataset.nivel));
                    }
                });
            });
        });
    </script>
</body>
</html>
