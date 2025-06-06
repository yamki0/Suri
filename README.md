
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tarjetas Educativas - Literatura y Lingüística</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #8a4baf 0%, #4b6cb7 100%);
            min-height: 100vh;
        }
        
        .card {
            perspective: 1000px;
            height: 400px;
            transition: transform 0.6s;
        }
        
        .card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            transition: transform 0.8s;
            transform-style: preserve-3d;
        }
        
        .card.flipped .card-inner {
            transform: rotateY(180deg);
        }
        
        .card-front, .card-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            border-radius: 1rem;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 2rem;
            overflow-y: auto;
        }
        
        .card-front {
            background: linear-gradient(135deg, #ffffff 0%, #f5f7fa 100%);
            color: #333;
        }
        
        .card-back {
            background: linear-gradient(135deg, #4b6cb7 0%, #182848 100%);
            color: white;
            transform: rotateY(180deg);
        }
        
        .option {
            transition: all 0.3s ease;
            cursor: pointer;
            border: 2px solid #e2e8f0;
        }
        
        .option:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .option.correct {
            background-color: #10b981;
            color: white;
            border-color: #10b981;
        }
        
        .option.incorrect {
            background-color: #ef4444;
            color: white;
            border-color: #ef4444;
        }
        
        .progress-bar {
            height: 10px;
            background-color: #e2e8f0;
            border-radius: 5px;
            overflow: hidden;
        }
        
        .progress {
            height: 100%;
            background: linear-gradient(90deg, #4b6cb7 0%, #8a4baf 100%);
            transition: width 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .animate-fadeIn {
            animation: fadeIn 0.5s ease forwards;
        }
        
        .animate-pulse {
            animation: pulse 1.5s infinite;
        }
        
        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #f00;
            border-radius: 50%;
            pointer-events: none;
        }

        /* Categorías */
        .category-badge {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
            margin-bottom: 0.75rem;
        }

        .category-literatura {
            background-color: #f59e0b;
            color: #fff;
        }

        .category-linguistica {
            background-color: #3b82f6;
            color: #fff;
        }

        .category-autores {
            background-color: #10b981;
            color: #fff;
        }

        .category-vocabulario {
            background-color: #8b5cf6;
            color: #fff;
        }

        .category-comprension {
            background-color: #ec4899;
            color: #fff;
        }

        /* Animaciones para las opciones */
        @keyframes correctAnswer {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .option.correct.animated {
            animation: correctAnswer 0.5s ease;
        }

        /* Estilo para el contador */
        .timer {
            font-size: 1.25rem;
            font-weight: 600;
            color: white;
            background: rgba(255, 255, 255, 0.2);
            padding: 0.5rem 1rem;
            border-radius: 9999px;
        }
        
        /* Estilo para textos largos */
        .long-text {
            font-size: 0.9rem;
            line-height: 1.5;
            max-height: 200px;
            overflow-y: auto;
            padding-right: 10px;
            margin-bottom: 1rem;
        }
        
        .long-text::-webkit-scrollbar {
            width: 6px;
        }
        
        .long-text::-webkit-scrollbar-track {
            background: rgba(0,0,0,0.05);
            border-radius: 10px;
        }
        
        .long-text::-webkit-scrollbar-thumb {
            background: rgba(0,0,0,0.2);
            border-radius: 10px;
        }
        
        /* Botones de filtro */
        .filter-btn {
            padding: 0.5rem 1rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
            transition: all 0.3s ease;
            margin-right: 0.5rem;
            margin-bottom: 0.5rem;
        }
        
        .filter-btn.active {
            transform: scale(1.05);
            box-shadow: 0 3px 10px rgba(0,0,0,0.2);
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="max-w-4xl mx-auto">
        <!-- Header -->
        <div class="text-center mb-8 animate-fadeIn">
            <h1 class="text-3xl md:text-4xl font-bold text-white mb-2">Tarjetas Educativas</h1>
            <h2 class="text-xl md:text-2xl text-white opacity-90">Literatura y Lingüística</h2>
        </div>
        
        <!-- Filters -->
        <div class="mb-4 flex flex-wrap justify-center" id="filters-container">
            <button class="filter-btn bg-white text-gray-800 active" data-category="all">Todas</button>
            <button class="filter-btn bg-white bg-opacity-20 text-white" data-category="literatura">Literatura</button>
            <button class="filter-btn bg-white bg-opacity-20 text-white" data-category="linguistica">Lingüística</button>
            <button class="filter-btn bg-white bg-opacity-20 text-white" data-category="autores">Autores</button>
            <button class="filter-btn bg-white bg-opacity-20 text-white" data-category="vocabulario">Vocabulario</button>
            <button class="filter-btn bg-white bg-opacity-20 text-white" data-category="comprension">Comprensión</button>
        </div>
        
        <!-- Progress Tracker -->
        <div class="mb-6 bg-white bg-opacity-20 p-4 rounded-lg">
            <div class="flex flex-wrap justify-between text-white mb-2">
                <span>Progreso: <span id="current-card">1</span>/<span id="total-cards">52</span></span>
                <span>Puntuación: <span id="score">0</span> puntos</span>
                <span class="timer hidden" id="timer">30</span>
            </div>
            <div class="progress-bar">
                <div class="progress" id="progress-bar" style="width: 0%"></div>
            </div>
        </div>
        
        <!-- Card Container -->
        <div class="card" id="card">
            <div class="card-inner">
                <!-- Card Front -->
                <div class="card-front">
                    <div class="flex justify-between items-center mb-4">
                        <div class="text-sm text-gray-500">Pregunta <span id="question-number">1</span></div>
                        <div id="category-badge" class="category-badge category-literatura">Literatura</div>
                    </div>
                    <div id="question-container">
                        <h3 class="text-xl md:text-2xl font-semibold mb-6" id="question">Escoja la proposición correcta</h3>
                        <div id="question-text" class="long-text hidden"></div>
                    </div>
                    
                    <div class="space-y-3" id="options-container">
                        <!-- Options will be inserted here -->
                    </div>
                </div>
                
                <!-- Card Back -->
                <div class="card-back">
                    <div class="text-center">
                        <div id="result-icon" class="text-5xl mb-4">✅</div>
                        <h3 class="text-xl md:text-2xl font-semibold mb-2" id="result-title">¡Correcto!</h3>
                        <p class="mb-6" id="explanation">La respuesta correcta es...</p>
                        <button id="next-btn" class="bg-white text-indigo-700 px-6 py-2 rounded-full font-medium hover:bg-opacity-90 transition-all">
                            Siguiente pregunta
                        </button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Controls -->
        <div class="mt-8 flex justify-between">
            <button id="prev-btn" class="bg-white bg-opacity-20 text-white px-4 py-2 rounded-lg hover:bg-opacity-30 transition-all">
                ← Anterior
            </button>
            <button id="restart-btn" class="bg-white text-indigo-700 px-6 py-2 rounded-lg font-medium hover:bg-opacity-90 transition-all hidden">
                Reiniciar juego
            </button>
            <button id="flip-btn" class="bg-white bg-opacity-20 text-white px-4 py-2 rounded-lg hover:bg-opacity-30 transition-all">
                Ver respuesta
            </button>
        </div>
        
        <!-- Results Modal (hidden by default) -->
        <div id="results-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
            <div class="bg-white rounded-xl p-8 max-w-md w-full mx-4">
                <h2 class="text-2xl font-bold mb-4 text-center">¡Has completado el juego!</h2>
                <div class="text-center mb-6">
                    <div class="text-5xl mb-4" id="final-emoji">🎉</div>
                    <p class="text-xl mb-2">Tu puntuación final:</p>
                    <p class="text-3xl font-bold text-indigo-700" id="final-score">0/520</p>
                </div>
                <div class="mb-6">
                    <h3 class="font-semibold mb-2">Resumen:</h3>
                    <div class="flex justify-between mb-1">
                        <span>Respuestas correctas:</span>
                        <span id="correct-count">0</span>
                    </div>
                    <div class="flex justify-between">
                        <span>Respuestas incorrectas:</span>
                        <span id="incorrect-count">0</span>
                    </div>
                </div>
                <button id="restart-modal-btn" class="w-full bg-indigo-600 text-white py-3 rounded-lg font-medium hover:bg-indigo-700 transition-all">
                    Jugar de nuevo
                </button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Quiz data
            const quizData = [
                {
                    question: "Escoja la proposición correcta",
                    options: [
                        "Un texto es una expresión lingüística, oral o escrita con una intensión comunicativa y unidad de sentido",
                        "Descontextualizar es contar la realidad de un hecho verdadero, sin alterarlo en el mismo contexto que sucedió",
                        "Parafrasear es enunciar con las mismas palabras, del autor de un texto, lo que el lector pretende explicar"
                    ],
                    correctAnswer: 0,
                    explanation: "Un texto es una expresión lingüística, oral o escrita con una intensión comunicativa y unidad de sentido.",
                    category: "linguistica"
                },
                {
                    question: "¿A qué tipo pertenece la novela de Jorge Icaza Coronel?",
                    options: [
                        "Novela Romántica",
                        "Novela de la tierra",
                        "Novela Indigenista"
                    ],
                    correctAnswer: 2,
                    explanation: "La novela de Jorge Icaza Coronel pertenece al tipo de Novela Indigenista.",
                    category: "literatura"
                },
                {
                    question: "¿A qué obra se considera la primera novela escrita en Latinoamérica?",
                    options: [
                        "Periquillo Sarmiento José",
                        "María",
                        "Amalia"
                    ],
                    correctAnswer: 0,
                    explanation: "Periquillo Sarmiento José es considerada la primera novela escrita en Latinoamérica.",
                    category: "literatura"
                },
                {
                    question: "¿A qué escritor pertenece el poema La sombra inquieta?",
                    options: [
                        "José Santos Chocano",
                        "Gabriela Mistral",
                        "Amado Nervo"
                    ],
                    correctAnswer: 1,
                    explanation: "El poema La sombra inquieta pertenece a Gabriela Mistral.",
                    category: "autores"
                },
                {
                    question: "Señale la obra que inauguró el realismo ecuatoriano",
                    options: [
                        "Los de abajo",
                        "Los que se van",
                        "Juyungo, Jorac Icaza"
                    ],
                    correctAnswer: 1,
                    explanation: "Los que se van es la obra que inauguró el realismo ecuatoriano.",
                    category: "literatura"
                },
                {
                    question: "Escoja el autor al que pertenece el párrafo de la obra \"El Cholo que odió la plata\"",
                    options: [
                        "Demetrio Aguilera Malta",
                        "Joaquín Gallegos Lara",
                        "Enrique Gil Gilbert"
                    ],
                    correctAnswer: 0,
                    explanation: "El párrafo de la obra \"El Cholo que odió la plata\" pertenece a Demetrio Aguilera Malta.",
                    category: "autores"
                },
                {
                    question: "Subraye la proposición correcta",
                    options: [
                        "El mapa semántico es un procedimiento que tiene como finalidad sintetizar y, al mismo tiempo, relacionar y jerarquizar de manera significativa los conceptos contenidos en un tema.",
                        "La técnica del subrayado favorece la comprensión e incrementa el sentido crítico de la lectura porque se diferencia lo esencial de lo secundario."
                    ],
                    correctAnswer: 0,
                    explanation: "El mapa semántico es un procedimiento que tiene como finalidad sintetizar y, al mismo tiempo, relacionar y jerarquizar de manera significativa los conceptos contenidos en un tema.",
                    category: "linguistica"
                },
                {
                    question: "Escoja el literal que tenga la proposición correcta sobre el subrayado y el editorial",
                    options: [
                        "Se deben subrayar, las palabras claves, los nombres propios, fechas y detalles que no son relevantes.",
                        "Subrayar las partículas que marcan la sintaxis (pero, aunque, etc., con el objeto de que lo subrayado tenga sentido por sí mismo).",
                        "Un editorial, nunca se debe Escribir directamente y sin rodeos"
                    ],
                    correctAnswer: 1,
                    explanation: "La proposición correcta es: Subrayar las partículas que marcan la sintaxis (pero, aunque, etc., con el objeto de que lo subrayado tenga sentido por sí mismo).",
                    category: "linguistica"
                },
                {
                    question: "Escoge la palabra correcta y completa la frase: Parafrasear es ___ con palabras lo que se expresa en un ___",
                    options: [
                        "denunciar - texto",
                        "texto - enunciar",
                        "enunciar - texto"
                    ],
                    correctAnswer: 2,
                    explanation: "Parafrasear es enunciar con palabras lo que se expresa en un texto.",
                    category: "linguistica"
                },
                {
                    question: "Complete: ___ se inauguró con la publicación del libro de cuentos ___ cuyos autores fueron Demetrio Aguilera Malta, Joaquín Gallegos Lara y Enrique Gil Gilbert.",
                    options: [
                        "Generación del Treinta / Revolución mexicana",
                        "Generación del treinta / Los que se van",
                        "Grupo de Guayaquil - Nuestro Pan"
                    ],
                    correctAnswer: 1,
                    explanation: "Generación del treinta / Los que se van es la respuesta correcta.",
                    category: "literatura"
                },
                {
                    question: "¿A qué tipo de falacia pertenece el siguiente ejemplo: \"Los ecologistas dicen que consumimos demasiado energía; pero no hagas caso porque los ecologistas siempre exageran\"?",
                    options: [
                        "ad ignorantiam (por la ignorancia)",
                        "ad hominem (dirigido contra el hombre)",
                        "ad baculum (apela al bastón)"
                    ],
                    correctAnswer: 1,
                    explanation: "Este ejemplo pertenece a la falacia ad hominem (dirigido contra el hombre).",
                    category: "linguistica"
                },
                {
                    question: "¿A qué palabra corresponde el siguiente concepto: Se aplica cuando a partir de un conjunto de datos particulares se llega a una información o conclusión general?",
                    options: [
                        "Inducción",
                        "Hipótesis",
                        "Deducción"
                    ],
                    correctAnswer: 0,
                    explanation: "El concepto corresponde a la palabra Inducción.",
                    category: "linguistica"
                },
                {
                    question: "Es un proceso cognitivo mediante el cual se extrae información explícita en los textos a discursos.",
                    options: [
                        "Referencia",
                        "Inferencia",
                        "Explícito"
                    ],
                    correctAnswer: 0,
                    explanation: "La Referencia es un proceso cognitivo mediante el cual se extrae información explícita en los textos a discursos.",
                    category: "linguistica"
                },
                {
                    question: "Se refiere al uso exagerado de palabras nuevas, y que en algunos casos ingresan al idioma y corresponden a las nuevas tecnologías, modas, alimentos o formas de ver al mundo",
                    options: [
                        "Neologismos",
                        "Vicios pragmáticos",
                        "Arcaísmos"
                    ],
                    correctAnswer: 0,
                    explanation: "Los Neologismos se refieren al uso exagerado de palabras nuevas, y que en algunos casos ingresan al idioma y corresponden a las nuevas tecnologías, modas, alimentos o formas de ver al mundo.",
                    category: "linguistica"
                },
                {
                    question: "¿A qué clase de vicio pertenece el siguiente ejemplo \"hicieron fríos intenso este mes\"?",
                    options: [
                        "Cosismo",
                        "Solecismos",
                        "Pleonasmo"
                    ],
                    correctAnswer: 1,
                    explanation: "El ejemplo \"hicieron fríos intenso este mes\" pertenece a la clase de vicio llamado Solecismos.",
                    category: "linguistica"
                },
                {
                    question: "La entrevista consta de las siguientes partes",
                    options: [
                        "Presentación/Inicio/cierre",
                        "Cuerpo/Inicio/cierre",
                        "Presentación/cuerpo/cierre"
                    ],
                    correctAnswer: 2,
                    explanation: "La entrevista consta de las siguientes partes: Presentación/cuerpo/cierre.",
                    category: "linguistica"
                },
                {
                    question: "Las siguientes palabras pertenecen: \"haiga, lluvió, juagar, cantenos\"",
                    options: [
                        "Barbarismos",
                        "Jergas",
                        "Malas palabras"
                    ],
                    correctAnswer: 0,
                    explanation: "Las palabras \"haiga, lluvió, juagar, cantenos\" pertenecen a Barbarismos.",
                    category: "linguistica"
                },
                {
                    question: "Escoge el nombre del autor del siguiente párrafo de la obra \"Las ruinas circulares\": \"Nadie lo vio desembarcar en la unánime noche, nadie vio la canoa de bambú sumiéndose en el fango sagrado, pero a los pocos días nadie ignoraba que el hombre taciturno venía del Sur y que su patria era una de las infinitas aldeas que están aguas arriba, en el flanco violento de la montaña, donde el idioma zend no está contaminado de griego y donde es infrecuente la lepra.\"",
                    options: [
                        "Julio Cortázar",
                        "Jorge Luis Borges",
                        "Gabriel García Márquez"
                    ],
                    correctAnswer: 1,
                    explanation: "El párrafo de la obra \"Las ruinas circulares\" pertenece a Jorge Luis Borges.",
                    category: "autores"
                },
                {
                    question: "Son novelistas de la narrativa ecuatoriana a partir del 1960",
                    options: [
                        "Pablo Palacios/ Juan Rulfo/Joaquín Gallegos Lara",
                        "Alicia Yánez/ Jorge Enrique Adum/Iván Egüez",
                        "José de la Cuadra/ Alicia Yánez/ Pablo Palacios"
                    ],
                    correctAnswer: 1,
                    explanation: "Los novelistas de la narrativa ecuatoriana a partir del 1960 son: Alicia Yánez/ Jorge Enrique Adum/Iván Egüez.",
                    category: "autores"
                },
                {
                    question: "Escoja la oración que tenga catacresis",
                    options: [
                        "En la tarde iré a ver una película.",
                        "En la tarde iré haber una película."
                    ],
                    correctAnswer: 1,
                    explanation: "La oración \"En la tarde iré haber una película\" tiene catacresis, que es el uso incorrecto de una palabra por otra que suena igual.",
                    category: "linguistica"
                },
                {
                    question: "A qué tipo de barbarismos pertenece la siguiente oración \"Ese hombre es un avaro, ojalá que no haiga nadies quien lo ayude\"",
                    options: [
                        "Ortográfico / Lingüístico",
                        "Lingüístico/ fonético",
                        "Ortográfico/fonético"
                    ],
                    correctAnswer: 1,
                    explanation: "La oración \"Ese hombre es un avaro, ojalá que no haiga nadies quien lo ayude\" pertenece al tipo de barbarismos Lingüístico/fonético.",
                    category: "linguistica"
                },
                {
                    question: "Lenguas aborígenes que han influido en el español de América.",
                    options: [
                        "Griego y latín.",
                        "Quechua, guaraní, náhuatl.",
                        "Francés, portugués, italiano, etc."
                    ],
                    correctAnswer: 1,
                    explanation: "Las lenguas aborígenes que han influido en el español de América son: Quechua, guaraní, náhuatl.",
                    category: "linguistica"
                },
                {
                    question: "Autor de las obras: El coronel no tiene quien le escriba, Los cuentos de la mama grande, Crónica de una muerte anunciada, Cien años de soledad, La Linares, El triple salto, Pájara la memoria, El poder del gran señor",
                    options: [
                        "Mario Benedetti/Coca Ponce",
                        "Gabriel García Márquez/Iván Egüez",
                        "Iván Egüez / Coca Ponce"
                    ],
                    correctAnswer: 1,
                    explanation: "El autor de las obras mencionadas es Gabriel García Márquez/Iván Egüez.",
                    category: "autores"
                },
                {
                    question: "Destacado ensayista ecuatoriano del siglo XIX, una de sus obras representativas.",
                    options: [
                        "Augusto Cueva/Entre la ira y la esperanza",
                        "Benjamín Carrión/Libertad y civilización",
                        "Juan Montalvo/Las catilinarias"
                    ],
                    correctAnswer: 2,
                    explanation: "El destacado ensayista ecuatoriano del siglo XIX es Juan Montalvo, con su obra representativa Las catilinarias.",
                    category: "autores"
                },
                {
                    question: "Autor de la frase célebre: \"La vida es una obra de teatro que no permite ensayos. Por eso, canta, ríe, baila, llora y vive intensamente cada momento de tu vida antes que el telón baje y la obra termine sin aplausos\"",
                    options: [
                        "Charlie Chaplin",
                        "Teresa de Calcuta",
                        "Nelson Mandela"
                    ],
                    correctAnswer: 0,
                    explanation: "El autor de esta frase célebre es Charlie Chaplin.",
                    category: "autores"
                },
                {
                    question: "Escoja el significado que mejor se ajuste al siguiente refrán: \"¿A dónde va Vicente? Donde va la gente\"",
                    options: [
                        "Hace referencia a que la gente hace lo que los demás le piden.",
                        "Hay que seguir a Vicente porque él conoce el camino.",
                        "Hay que aprender de los errores de los otros.",
                        "Es mejor hacer tu propio camino que dejarse influenciar de los demás."
                    ],
                    correctAnswer: 0,
                    explanation: "El refrán \"¿A dónde va Vicente? Donde va la gente\" hace referencia a que la gente hace lo que los demás le piden, o sigue a la mayoría sin cuestionarse.",
                    category: "linguistica"
                },
                {
                    question: "Marca la alternativa correcta que corresponda a los sinónimos de la palabra subrayada: El sueño de todo estudiante es ingresar a la Universidad",
                    options: [
                        "modorra",
                        "ilusionar",
                        "quimera",
                        "anhelo",
                        "visión"
                    ],
                    correctAnswer: 3,
                    explanation: "El sinónimo correcto para 'sueño' en este contexto es 'anhelo', que significa deseo intenso o aspiración.",
                    category: "linguistica"
                },
                {
                    question: "Elige la palabra que no pertenece a la categoría.",
                    options: [
                        "Munición",
                        "Acusación",
                        "Maldición",
                        "Licitación"
                    ],
                    correctAnswer: 0,
                    explanation: "La palabra 'Munición' no pertenece a la categoría, ya que las demás palabras terminan en '-ción' y son sustantivos abstractos relacionados con acciones, mientras que 'munición' es un sustantivo concreto.",
                    category: "vocabulario"
                },
                {
                    question: "¿Cuál de las siguientes palabras está escrita sin faltas de ortografía?",
                    options: [
                        "Abración",
                        "Precición",
                        "Asunción",
                        "Colución"
                    ],
                    correctAnswer: 2,
                    explanation: "La palabra 'Asunción' está escrita correctamente. Las demás tienen errores: debería ser 'abrasión', 'precisión' y 'colusión'.",
                    category: "vocabulario"
                },
                {
                    question: "Elige la palabra que no comparte el mismo significado que las demás.",
                    options: [
                        "Prudente",
                        "Valeroso",
                        "Valiente",
                        "Atrevido"
                    ],
                    correctAnswer: 0,
                    explanation: "La palabra 'Prudente' no comparte el mismo significado que las demás. Mientras que 'valeroso', 'valiente' y 'atrevido' se refieren a personas que muestran valor o audacia, 'prudente' significa cauteloso o que actúa con moderación.",
                    category: "vocabulario"
                },
                {
                    question: "Elige la palabra que no comparte el mismo significado que las demás.",
                    options: [
                        "Aniquilar",
                        "Exterminar",
                        "Remover",
                        "Masacrar"
                    ],
                    correctAnswer: 2,
                    explanation: "La palabra 'Remover' no comparte el mismo significado que las demás. Mientras que 'aniquilar', 'exterminar' y 'masacrar' implican destrucción o eliminación total, 'remover' significa quitar algo de un lugar o mover de un sitio a otro.",
                    category: "vocabulario"
                },
                {
                    question: "Sustituya la palabra en mayúscula para dar un sentido opuesto a la oración: Esa mascota era la más ASEADA de todas",
                    options: [
                        "Incómoda",
                        "Despreciada",
                        "Inmunda",
                        "Fea"
                    ],
                    correctAnswer: 2,
                    explanation: "La palabra 'Inmunda' da un sentido opuesto a 'ASEADA', ya que significa sucia o impura, mientras que aseada significa limpia.",
                    category: "vocabulario"
                },
                {
                    question: "Elige la palabra que da sentido a la oración: Los nativos _____ la tierra y cosechando frutos.",
                    options: [
                        "Talaban",
                        "Cultivan",
                        "Mandaban",
                        "Cortaban"
                    ],
                    correctAnswer: 1,
                    explanation: "La palabra 'Cultivan' da sentido a la oración, ya que se relaciona con la acción de trabajar la tierra para obtener frutos.",
                    category: "vocabulario"
                },
                {
                    question: "Sustituye la palabra en mayúscula para dar sentido opuesto a la oración: La DISCORDIA general era por la injusticia.",
                    options: [
                        "Inteligencia",
                        "Admiración",
                        "Alternancia",
                        "Avenencia"
                    ],
                    correctAnswer: 3,
                    explanation: "La palabra 'Avenencia' da un sentido opuesto a 'DISCORDIA', ya que significa acuerdo, conformidad o armonía, mientras que discordia implica desacuerdo o conflicto.",
                    category: "vocabulario"
                },
                {
                    question: "Sustituye la palabra en mayúscula para dar un sentido opuesto a la oración: El color del agua es AMBIGUO",
                    options: [
                        "Dependiente",
                        "Preciso",
                        "Turbio",
                        "Transparente"
                    ],
                    correctAnswer: 1,
                    explanation: "La palabra 'Preciso' da un sentido opuesto a 'AMBIGUO', ya que ambiguo significa que puede entenderse de varios modos, mientras que preciso es claro y exacto.",
                    category: "vocabulario"
                },
                {
                    question: "Sustituye la palabra en mayúscula para dar un sentido opuesto a la oración: Y ellos lo ABANDONARON en el páramo.",
                    options: [
                        "Abrigaron",
                        "Protegieron",
                        "Ampararon",
                        "Celebraron"
                    ],
                    correctAnswer: 2,
                    explanation: "La palabra 'Ampararon' da un sentido opuesto a 'ABANDONARON', ya que abandonar significa dejar o desamparar, mientras que amparar significa proteger o resguardar.",
                    category: "vocabulario"
                },
                {
                    question: "Elige el par de palabras que difieren de la relación entre: Animar - Dinamizar",
                    options: [
                        "Lijar - Pulir",
                        "Lustrar - Sobar",
                        "Arrugar - Plegar",
                        "Limar - Picar"
                    ],
                    correctAnswer: 1,
                    explanation: "El par 'Lustrar - Sobar' difiere de la relación entre 'Animar - Dinamizar'. Mientras que animar y dinamizar tienen significados similares (dar vida o energía), lustrar (dar brillo) y sobar (amasar o masajear) no tienen una relación semántica tan directa.",
                    category: "vocabulario"
                },
                {
                    question: "Identifique la idea central explícita del texto: \"En Estados Unidos un grupo de científicos ha logrado diseñar en el laboratorio y transplantar a un ratón un tejido muscular que puede repararse a sí mismo, algo que en el futuro próximo podría ayudar a los humanos a recuperarse de lesiones. El tejido muscular artificial puede regenerarse dentro de un animal tal y como lo haría el tejido natural, según dieron a conocer los investigadores de la Universidad de Duke en Durham, Carolina del Norte.\"",
                    options: [
                        "Un grupo de científicos creó un tejido artificial capaz de regenerarse, este descubrimiento será de gran utilidad en medicina.",
                        "El tejido muscular artificial puede regenerarse dentro de un organismo vivo tal y como lo haría el tejido natural.",
                        "Los investigadores del estudio científico pertenecen a la Universidad de Duke en Durham, Carolina del Norte.",
                        "El estudio científico con animales de laboratorio resulta de utilidad para formular hipótesis aplicables a seres humanos."
                    ],
                    correctAnswer: 1,
                    explanation: "La idea central explícita del texto es: \"El tejido muscular artificial puede regenerarse dentro de un organismo vivo tal y como lo haría el tejido natural\", ya que es la información principal que se desarrolla en el texto.",
                    category: "comprension"
                },
                {
                    question: "En el siguiente texto, identifique el argumento",
                    longText: "Pasota es el que \"pasa de todo\", el que decide no preocuparse por ningún problema y vivir al margen de lo que ocurre fuera de sí mismo. En una postura deliberada y permanente de automarginación. Al pasota (al menos aparentemente) no le importa nada, todo le da igual. No se apunta a nada ni se compromete con nadie. Evita cualquier compromiso y responsabilidad. No está dispuesto a vivir con esfuerzo. El pasotismo es una evasión de la realidad en la que se vive. Es la pretensión de vivir cómodamente sin problemas, en un mundo separado y fabricado a la medida de los propios deseos y apetencias [...] En la Psicología del pasota típico suelen apreciarse los mismos síntomas que en las personas depresivas: ansiedad, tristeza, miedo de actuar, etc. Es una depresión mimética. El pasota sufre, inicialmente por incapacidad para afrontar los problemas de su vida, le faltan recursos y entrenamiento para resolverlos. Pero sufre también, más adelante, en el intento imposible de vivir sin problemas, de vivir en un mundo artificial, no existente. Quiere vivir sin vivir. Esta es quizá su mayor contradicción y su auténtica tragedia.",
                    options: [
                        "El pasotismo es una evasión de la realidad en la que se vive.",
                        "El pasotismo es cada vez más frecuente entre la población.",
                        "El pasota pretende crear un mundo separado y a medida de sus deseos y apetencias.",
                        "En la Psicología el pasotismo está relacionado con la depresión y la automarginación."
                    ],
                    correctAnswer: 0,
                    explanation: "El argumento principal del texto es que \"El pasotismo es una evasión de la realidad en la que se vive\", ya que es la tesis central que el autor desarrolla y sustenta a lo largo del texto.",
                    category: "comprension"
                },
                {
                    question: "Identifique la idea central explícita en el texto.",
                    longText: "El individuo educado es aquel que reconoce la legitimidad de toda ley que le impone un comportamiento admisible y aceptable por todos, es decir un comportamiento racional y razonable. Pero es también el individuo que captaría la legitimidad de toda ley que le impusiera no respetar a otro como a sí mismo, que le obligase, por ejemplo, a considerar tal o tal otra categoría de seres humanos como a simples cosas. (Canivez, P. citado por Fernando Savater)",
                    options: [
                        "La persona educada debe admitir la autoridad de toda ley y comportarse conforme a ella.",
                        "Toda ley nos impone un comportamiento racional y razonable y debemos aceptarla.",
                        "Debemos respetar y cumplir toda ley que demande de nosotros un comportamiento considerado admisible y aceptable.",
                        "Debemos respetar toda ley, pero debemos ser capaces de captar la ilegitimidad de las leyes que impidan respetar a todos."
                    ],
                    correctAnswer: 3,
                    explanation: "La idea central explícita del texto es que \"Debemos respetar toda ley, pero debemos ser capaces de captar la ilegitimidad de las leyes que impidan respetar a todos\", ya que el texto menciona que el individuo educado reconoce la legitimidad de las leyes que imponen comportamientos aceptables, pero también captaría la ilegitimidad de leyes que no respeten a otros como a sí mismo.",
                    category: "comprension"
                },
                {
                    question: "Elija la frase que representa la idea principal del texto.",
                    longText: "No se pueden juzgar las intenciones más profundas de las personas al realizar cualquier tipo de acción. Para hacerlo con integridad y un mínimo margen de error habría que estar en su lugar, o como dice el proverbio de alguna etnia norteamericana 'llevar sus mocasines durante un mes'. (Maldonado L. \"Nadie es inocente\")",
                    options: [
                        "Hay que llevar los mocasines de otro durante un mes antes de criticarle.",
                        "No se puede dejar pasar por alto las malas actitudes ajenas al juzgarlas.",
                        "No se puede juzgar las intenciones profundas de las personas al actuar."
                    ],
                    correctAnswer: 2,
                    explanation: "La idea principal del texto es que \"No se puede juzgar las intenciones profundas de las personas al actuar\", ya que es la afirmación con la que comienza el texto y que se desarrolla a lo largo del mismo.",
                    category: "comprension"
                },
                {
                    question: "Elija la inferencia correcta en base a la información del párrafo.",
                    longText: "Un hombre de unos 25 años entra en un cafenet y pide una computadora. Lleva una carpeta con estados de cuenta de dos tarjetas de crédito, navega unos minutos. Se levanta, paga y se dispone a salir del local; cuando se acerca a la puerta, suena su teléfono celular, él responde y comienza a explicar a alguien que en un par de semanas podrá comprarse un vehículo.",
                    options: [
                        "El hombre ha visto en internet los resultados de lotería y sabe que la ha ganado.",
                        "El hombre va a ser embargado por los acreedores de las tarjetas de crédito.",
                        "El hombre estaba verificando precios de automóviles en internet.",
                        "El hombre envió un correo a la persona que lo llamó."
                    ],
                    correctAnswer: 2,
                    explanation: "La inferencia correcta es que \"El hombre estaba verificando precios de automóviles en internet\", ya que llevaba estados de cuenta de tarjetas de crédito (posiblemente para verificar su capacidad de pago), navegó en internet y luego mencionó que podría comprarse un vehículo en un par de semanas.",
                    category: "comprension"
                },
                {
                    question: "Identifique la idea principal del siguiente texto.",
                    longText: "El alemán Roland Belz había trabajado tres años en perfeccionar el concepto de un carruaje con motor y tecnología moderna que no perdiera nada del estilo y la elegancia de sus predecesores, hasta que presentó su proyecto en 2005. No le fue mal. La empresa Aaglander ha sobrevivido hasta hoy día y se encuentra en pleno auge, lo que demuestra que su idea está cuajando y no es tan digamos alocada como se podía pensar.",
                    options: [
                        "La idea de crear un carruaje clásico pero con motor y tecnología moderna resulta novedosa y factible.",
                        "La idea de crear un carruaje clásico con motor y tecnología moderna es absurda en la actualidad.",
                        "El alemán Roland Belz trabajó tres años profesionalizando su idea de un coche clásico con tecnología moderna.",
                        "La empresa Aaglander, creadora de un coche que une lo clásico y lo moderno, se encuentra en pleno auge."
                    ],
                    correctAnswer: 0,
                    explanation: "La idea principal del texto es que \"La idea de crear un carruaje clásico pero con motor y tecnología moderna resulta novedosa y factible\", ya que el texto describe cómo esta idea, que podría parecer alocada, ha tenido éxito y la empresa está en auge.",
                    category: "comprension"
                },
                {
                    question: "Selecciona el significado de las palabras que están en negrita en la oración: El juglar enamorado conservaba un copioso tesoro en su humilde morada fingiendo ser pobre para ganarse el corazón de su amada.",
                    options: [
                        "adivino - exultante",
                        "brujo - eufórico",
                        "hechicero - alborozado",
                        "poeta - abundante"
                    ],
                    correctAnswer: 3,
                    explanation: "Las palabras en negrita son 'juglar' y 'copioso'. 'Juglar' significa poeta o trovador medieval que recitaba o cantaba versos, y 'copioso' significa abundante o numeroso.",
                    category: "vocabulario"
                },
                {
                    question: "Selecciona el significado de las palabras que están en negrita en la oración: Su jovial carácter amenizó el coloquio.",
                    options: [
                        "alegre - diálogo",
                        "avispado - soliloquio",
                        "bullicioso - foro",
                        "vivaz - debate"
                    ],
                    correctAnswer: 0,
                    explanation: "Las palabras en negrita son 'jovial' y 'coloquio'. 'Jovial' significa alegre o de buen humor, y 'coloquio' significa diálogo o conversación.",
                    category: "vocabulario"
                },
                {
                    question: "Selecciona el significado de las palabras que están en negrita en la oración: Su lóbrego semblante combinaba con la brumosa tarde.",
                    options: [
                        "melancólico - nublada",
                        "mustio - cargada",
                        "pesaroso - encapotada",
                        "taciturno - anubarrada"
                    ],
                    correctAnswer: 0,
                    explanation: "Las palabras en negrita son 'lóbrego' y 'brumosa'. 'Lóbrego' significa oscuro, tenebroso o melancólico, y 'brumosa' significa cubierta de bruma o niebla, es decir, nublada.",
                    category: "vocabulario"
                },
                {
                    question: "Con base en el texto titulado \"Precios del petróleo registraron una ligera alza en Nueva York y en Inglaterra\", identifique una evidencia.",
                    longText: "Las cotizaciones del petróleo subieron este miércoles en Nueva York antes de una decisión de política monetaria del Banco Central Europeo (BCE) y la publicación de las reservas de crudo en Estados Unidos. Los precios del barril de \"light sweet crude\" (WTI) para entregar en marzo subieron USD 1.31 y cerraron a USD 47.78 en el New York Mercantile Exchange (Nymex). A su vez en Londres el barril de Brent del mar del Norte para igual entrega cerró en USD 49.3 en el Intercontinental Exchange (ICE), con una alza de USD 1.04 con relación al cierre del martes. [...] La mayoría de los inversores anticipa un vasto programa de compra de activos por parte del BCE para estimular la actividad económica de la zona euro [...] Este miércoles los precios subieron por \"varios anuncios de despidos en la industria, que \"parecen anunciar un descenso de la producción\" de crudo, destacó Phil Flynn, de Price Futures Group. En un mes, las tres principales firmas de servicios petroleros, Schlumberger, Halliburton y Baker Hughes, anunciaron 17.000 despidos [...]",
                    options: [
                        "Las cotizaciones del petróleo subieron este miércoles en Nueva York.",
                        "Los precios de \"light sweet crude\" cerraron a USD 47.78 en New York.",
                        "Este miércoles los precios subieron por \"varios anuncios de despidos\" en la industria.",
                        "Los despidos \"parecen anunciar un descenso de la producción\" de crudo."
                    ],
                    correctAnswer: 1,
                    explanation: "La evidencia más concreta y específica del texto es que \"Los precios de 'light sweet crude' cerraron a USD 47.78 en New York\", ya que es un dato preciso y verificable que respalda la afirmación del título sobre el alza de precios.",
                    category: "comprension"
                },
                {
                    question: "Infiera la idea del refrán: Si quieres que otro se ría, cuenta tus penas María.",
                    options: [
                        "La discreción puede evitarnos burlas innecesarias.",
                        "Cada quien recibe lo que merece y alcanza lo que puede.",
                        "Es preferible reír de nuestras penas que lamentarnos solos.",
                        "Es fácil curarse de las penas, si quieres ver otro día."
                    ],
                    correctAnswer: 0,
                    explanation: "La idea del refrán \"Si quieres que otro se ría, cuenta tus penas María\" es que \"La discreción puede evitarnos burlas innecesarias\", ya que advierte sobre no compartir nuestras penas o problemas con otros porque podrían burlarse de nosotros en lugar de ayudarnos.",
                    category: "comprension"
                },
                {
                    question: "Con base en el texto, identifique la idea secundaria.",
                    longText: "Los seres vivos más abundantes en el ecosistema son los animales y las plantas, además de éstos, existen otros seres como los hongos y las algas que no son animales ni plantas. Los animales constituyen la fauna, y las plantas, la flora de un ecosistema. Las características de todo ecosistema son la temperatura, las precipitaciones, el suelo, el agua y la luz; todos estos, elementos que influyen en los seres vivos.",
                    options: [
                        "Todo ecosistema tiene dos componentes.",
                        "Todo ecosistema tiene componentes como los seres vivos y las características del lugar.",
                        "Las características del lugar son la temperatura, las precipitaciones, el suelo, el agua y la luz.",
                        "El ecosistema está compuesto de seres vivos."
                    ],
                    correctAnswer: 2,
                    explanation: "La idea secundaria del texto es que \"Las características del lugar son la temperatura, las precipitaciones, el suelo, el agua y la luz\", ya que la idea principal se centra en los seres vivos del ecosistema, mientras que esta información complementa describiendo los factores ambientales.",
                    category: "comprension"
                },
                {
                    question: "Se puede inferir de lo expuesto en el texto que:",
                    longText: "Los primeros pasos para los actuales videojuegos se producen en los años 40, cuando los técnicos americanos desarrollaron el primer simulador de vuelo, destinado al entrenamiento de pilotos. En 1962 apareció la tercera generación de computadoras, con reducción de su tamaño y costo de manera drástica; y a partir de ahí el proceso ha sido continuo. En 1969 nació el microprocesador, que en un reducido espacio producía mayor potencial de información que grandes computadoras de los años 50. Es lo que constituye el corazón de nuestras computadoras, videojuegos y calculadoras. En 1970 aparece el disco flexible y en 1972 se desarrolla el primer juego, llamado PONG, que consistía en una rudimentaria partida de tenis o ping-pong. En 1977, la firma Atari lanzó al mercado el primer sistema de videojuegos en cartucho, que alcanzó un gran éxito en Estados Unidos y provocó, al mismo tiempo, una primera preocupación sobre los posibles efectos de los videojuegos en la conducta de los niños. Luego de una voraz evolución, en la que el constante aumento de la potencia de los microprocesadores y de la memoria permitió nuevas mejoras, en 1986 la casa Nintendo lanzó su primer sistema de videojuegos que permitió la presentación de unos juegos impensables nueve años atrás. La calidad del movimiento, el color y el sonido, así como la imaginación de los creadores de juegos fueron tales que, unidos al considerable abaratamiento relativo de dichos videojuegos, a comienzos de los 90, en nuestro país se extendieron de manera masiva los juegos creados por las dos principales compañías, Sega y Nintendo; y en poco tiempo se constituyeron en uno de los juguetes preferidos de los niños. La extensión masiva de los videojuegos en los años 90 ha provocado una segunda oleada de investigaciones, en la medicina, la sociología, la psicología y la educación, además de la preocupación y las valoraciones que dichos juegos han recibido por parte de padres, educadores y principalmente los medios de comunicación, para quienes generalmente los videojuegos son vistos como algo negativo y perjudicial. Las más prestigiosas universidades, revistas y publicaciones son sensibles a la preocupación por una de las tendencias preferidas a la hora de elegir los juegos, no solo de los niños y adolescentes, sino también de jóvenes y adultos.",
                    options: [
                        "La empresa Sega tuvo una duración prolongada en videojuegos",
                        "Fue en países asiáticos que se revolucionó los videojuegos.",
                        "En cuanto a comunicación, los videojuegos resultan nocivos.",
                        "La empresa Atari fue la pionera en la creación de videojuegos.",
                        "La medicina, la psicología y la sociología investigan los videojuegos."
                    ],
                    correctAnswer: 4,
                    explanation: "Del texto se puede inferir que \"La medicina, la psicología y la sociología investigan los videojuegos\", ya que menciona explícitamente que la extensión masiva de los videojuegos provocó una segunda oleada de investigaciones en estas disciplinas.",
                    category: "comprension"
                },
                {
                    question: "Obra clásica de la literatura griega es:",
                    options: [
                        "La Eneida.",
                        "Las tragedias de Sófocles.",
                        "Los relatos de Ben Hur.",
                        "Robin Hood"
                    ],
                    correctAnswer: 1,
                    explanation: "Las tragedias de Sófocles son obras clásicas de la literatura griega. Sófocles fue uno de los tres grandes dramaturgos trágicos de la antigua Grecia, junto con Esquilo y Eurípides.",
                    category: "literatura"
                },
                {
                    question: "El realismo mágico es un movimiento literario que surgió en:",
                    options: [
                        "América Latina.",
                        "Europa del Este.",
                        "Grecia.",
                        "La India."
                    ],
                    correctAnswer: 0,
                    explanation: "El realismo mágico es un movimiento literario que surgió en América Latina. Este estilo narrativo se caracteriza por la inclusión de elementos fantásticos o mágicos en un contexto realista, y tuvo su auge con autores como Gabriel García Márquez, Isabel Allende y Jorge Luis Borges.",
                    category: "literatura"
                },
                {
  question: "La Illíada es:",
  options: ["un mito.", "un poema de amor.", "una epopeya.", "una leyenda."],
  correctAnswer: 2,
  explanation: "La Ilíada es una epopeya atribuida a Homero.",
  category: "literatura"
},
{
  question: "El fundador del modernismo literario fue:",
  options: ["Ernest Hemingway.", "Gabriel García Márquez.", "Homero.", "Rubén Darío."],
  correctAnswer: 3,
  explanation: "Rubén Darío es considerado el fundador del modernismo literario.",
  category: "literatura"
},
{
  question: "La literatura realista surgió como un rechazo a:",
  options: ["el clasicismo.", "el romanticismo.", "los bardos.", "los cantares de Gesta."],
  correctAnswer: 1,
  explanation: "El realismo surgió como reacción al romanticismo.",
  category: "literatura"
},
{
  question: "José Joaquín de Olmedo escribió:",
  options: ["Canto a Junín.", "¡Quejas!", "La bandera del Ecuador."],
  correctAnswer: 0,
  explanation: "José Joaquín de Olmedo escribió el 'Canto a Junín'.",
  category: "autores"
},
{
  question: "Dolores Veintimilla es una poeta romántica porque:",
  options: ["Estaba muy enamorada de su esposo.", "Su poesía exalta el sentimiento sobre la razón.", "Se suicidó."],
  correctAnswer: 1,
  explanation: "Su poesía representa las características del romanticismo: exaltación del sentimiento sobre la razón.",
  category: "autores"
},
{
  question: "Un hecho es:",
  options: ["un suceso contado objetivamente.", "la deducción extraída de un suceso.", "la posibilidad de la realidad."],
  correctAnswer: 0,
  explanation: "Un hecho es un suceso contado de forma objetiva, sin interpretación.",
  category: "linguistica"
},
{
  question: "La obra de Miguel Riofrío tiene características:",
  options: ["románticas.", "clásicas", "modernista"],
  correctAnswer: 0,
  explanation: "Miguel Riofrío pertenece al romanticismo.",
  category: "literatura"
},
{
  question: "¿Qué figura literaria hay en estos versos de César Dávila Andrade: 'Más valiera no ser a este vivir de llanto/ a este amasar con lágrimas el pan de nuestro canto?'",
  options: ["Metáfora.", "Anáfora.", "Símil."],
  correctAnswer: 0,
  explanation: "La metáfora es evidente en la expresión poética y simbólica del dolor.",
  category: "literatura"
},
{
  question: "¿Cuál de los siguientes poetas no pertenece a la generación decapitada?",
  options: ["Jorge Carrera Andrade.", "Medardo Ángel Silva.", "Ernesto Noboa y Caamaño."],
  correctAnswer: 0,
  explanation: "Jorge Carrera Andrade no pertenece a la generación decapitada.",
  category: "autores"
},
{
  question: "Una característica de la poesía producida por los poetas decapitados es:",
  options: ["Sentimientos de evasión.", "Libre verso.", "Ausencia de rima.", "Temas de denuncia social."],
  correctAnswer: 0,
  explanation: "La poesía de esta generación reflejaba sentimientos de evasión y melancolía.",
  category: "literatura"
},
{
  question: "Un caligrama es:",
  options: ["Lo contrario de un haiku.", "Poemas de Carrera Andrade inspirados en los haikus.", "Rimas construidas a partir de los modelos simbolistas."],
  correctAnswer: 2,
  explanation: "Un caligrama es un poema con disposición gráfica especial, inspirado en el simbolismo.",
  category: "literatura"
},
{
  question: "A la frase que señala la postura de una persona en una argumentación la llamamos:",
  options: ["Tesis.", "Introducción.", "Argumento."],
  correctAnswer: 0,
  explanation: "La tesis expresa la postura central en una argumentación.",
  category: "linguistica"
},
{
  question: "Una biblioteca digital tiene:",
  options: ["Únicamente libros.", "Páginas webs.", "Libros e incluso archivos de audio."],
  correctAnswer: 2,
  explanation: "Las bibliotecas digitales pueden incluir múltiples formatos, no solo libros.",
  category: "comprension"
},
{
  question: "En la entrevista laboral, el lenguaje del entrevistado debe ser:",
  options: ["Coloquial.", "Amigable.", "Formal."],
  correctAnswer: 2,
  explanation: "En contextos laborales se espera un lenguaje formal.",
  category: "linguistica"
},
{
  question: "Lee la siguiente frase: «El conductor se quedó dormido y provocó el accidente». Aquí hay:",
  options: ["Un hecho.", "Una inferencia.", "Una opinión."],
  correctAnswer: 1,
  explanation: "Es una inferencia, pues se interpreta que se quedó dormido por lo ocurrido.",
  category: "comprension"
},
{
  question: "«Según Borges, la Tierra Prometida en realidad sería un lugar lleno de libros». Esta frase es:",
  options: ["Un hecho.", "Una cita textual.", "Una paráfrasis."],
  correctAnswer: 2,
  explanation: "Paráfrasis porque no está citada literalmente.",
  category: "comprension"
},
{
  question: "Un texto literario puede estar escrito:",
  options: ["En prosa y en verso.", "Solo en prosa.", "Solo en verso."],
  correctAnswer: 0,
  explanation: "Puede adoptar ambas formas: prosa o verso.",
  category: "literatura"
},
{
  question: "Las palabras tienen:",
  options: ["Un significado contextual.", "Un significado literal.", "Un significado literal y uno contextual."],
  correctAnswer: 2,
  explanation: "El significado depende tanto del diccionario como del contexto.",
  category: "linguistica"
},
{
  question: "La poesía romántica se caracteriza:",
  options: ["Porque habla de la realidad social.", "Porque exalta los sentimientos frente a la razón.", "Porque solo habla de amor."],
  correctAnswer: 1,
  explanation: "El romanticismo destaca por la exaltación del sentimiento sobre la razón.",
  category: "literatura"
},
{
  question: "Montalvo se distingue en el subgénero:",
  options: ["Poético.", "Ensayístico.", "Épico."],
  correctAnswer: 1,
  explanation: "Juan Montalvo se destacó por sus ensayos.",
  category: "autores"
},
{
  question: "Miguel Riofrío escribió:",
  options: ["A la Costa.", "La Emancipada.", "Las catilinarias."],
  correctAnswer: 1,
  explanation: "La obra más representativa de Miguel Riofrío es 'La Emancipada'.",
  category: "autores"
},
{
  question: "Una inferencia es:",
  options: ["Una afirmación que se debe deducir a partir de otra.", "Una reverencia ante una actitud del interlocutor.", "Un tipo de estrofa romántica."],
  correctAnswer: 0,
  explanation: "Una inferencia es una conclusión lógica derivada de otra información.",
  category: "comprension"
},
{
  question: "Boletín y elegía de las mitas es un poema que:",
  options: [
    "Denuncia la explotación que sufrieron los indígenas durante la Conquista y la Colonia.",
    "Escribió Dolores Veintimilla antes de suicidarse.",
    "Habla de evasión y sentimientos de melancolía."
  ],
  correctAnswer: 0,
  explanation: "Es un poema de Jorge Carrera Andrade de crítica social e histórica.",
  category: "autores"
},
{
  question: "Escoge la respuesta correcta: Jorge Icaza cultivó, dentro del realismo, el:",
  options: ["indigenismo.", "modernismo.", "surrealismo."],
  correctAnswer: 0,
  explanation: "Jorge Icaza es un exponente del realismo indigenista en Ecuador.",
  category: "autores"
},
{
  question: "La literatura realista del treinta buscaba sobre todo:",
  options: [
    "brindar una puerta de escape de la realidad al lector.",
    "denunciar las injusticias sociales.",
    "exaltar los sentimientos y el yo personal."
  ],
  correctAnswer: 1,
  explanation: "El realismo de los años 30 tenía un fuerte componente de denuncia social.",
  category: "literatura"
},
{
  question: "La diglosia es:",
  options: [
    "Una situación de bilingüismo en la que las escuelas ofrecen educación en los dos idiomas.",
    "Una situación de bilingüismo en la que las personas manejan indistintamente las dos lenguas.",
    "Una situación de desfase entre lo que habla una persona y lo que dice la nación.",
    "Una situación de bilingüismo en la que una lengua es más prestigiosa que otra."
  ],
  correctAnswer: 3,
  explanation: "La diglosia ocurre cuando una lengua es considerada socialmente superior a otra.",
  category: "linguistica"
},
{
  question: "La literatura de finales del siglo XX en Ecuador:",
  options: ["cuenta con temáticas y técnicas variadas.", "respeta la métrica.", "trata solo temas realistas."],
  correctAnswer: 0,
  explanation: "Es diversa tanto en contenido como en estilo.",
  category: "literatura"
},
{
  question: "Poso Wells, novela de Gabriela Alemán, es un alegato en contra de:",
  options: ["el populismo.", "la corrupción", "la migración."],
  correctAnswer: 1,
  explanation: "La obra es una crítica a la corrupción política y social.",
  category: "literatura"
},
{
  question: "Una de las causas de la diglosia entre el castellano y las lenguas ancestrales de nuestro país, es:",
  options: [
    "El tamaño del diccionario de la Real Academia Española",
    "La república sin el reconocimiento de las nacionalidades",
    "La falta de recursos económicos"
  ],
  correctAnswer: 1,
  explanation: "La exclusión histórica de las lenguas indígenas se debe, entre otras causas, a la falta de reconocimiento oficial.",
  category: "linguistica"
},
{
  question: "Subraye la respuesta correcta. Julio Pazos nació en:",
  options: ["Ambato.", "Baños.", "Quito."],
  correctAnswer: 0,
  explanation: "Julio Pazos Barrera nació en Ambato, Ecuador.",
  category: "autores"
},
{
  question: "La poesía romántica se caracteriza:",
  options: [
    "Porque habla de la realidad social.",
    "Porque exalta los sentimientos frente a la razón.",
    "Porque solo habla de amor"
  ],
  correctAnswer: 1,
  explanation: "El romanticismo da prioridad a los sentimientos y la subjetividad.",
  category: "literatura"
},
{
  question: "Escriba verdadero o falso: Los disfemismos son palabras con carga peyorativa que usamos como insultos.",
  options: ["Verdadero", "Falso"],
  correctAnswer: 0,
  explanation: "Verdadero. Los disfemismos son expresiones despectivas o insultantes.",
  category: "linguistica"
},
{
  question: "Jorge Carrera Andrade escribió:",
  options: ["Boletín y elegía de las mitas", "Biografía para uso de los pájaros", "Emoción vesperal"],
  correctAnswer: 1,
  explanation: "\"Biografía para uso de los pájaros\" es una de sus obras más conocidas.",
  category: "autores"
},
{
  question: "Escoja las palabras que tengan relación con la premisa: PREOCUPACIÓN: INSOMNIO ::",
  options: [
    "debilidad: anemia",
    "soledad: rencor",
    "licor: embriaguez",
    "ira: ofuscación",
    "golpe: amnesia"
  ],
  correctAnswer: 2,
  explanation: "El licor provoca embriaguez, como la preocupación provoca insomnio.",
  category: "vocabulario"
},
{
  question: "Señale el sinónimo de la palabra subrayada: \"No deben arrugarse frente a los problemas\"",
  options: [
    "Amilanarse",
    "Arriesgarse",
    "Arrobarse",
    "Arrojarse",
    "Arroparse"
  ],
  correctAnswer: 0,
  explanation: "Amilanarse significa desanimarse o dejarse vencer.",
  category: "vocabulario"
},
{
  question: "Determine la serie compuesta por tres sinónimos.",
  options: [
    "someter, desertar, difamar",
    "inquirir, impostar, escrutar",
    "separar, transigir, ceder",
    "incordiar, odiar, intimar",
    "luchar, bregar, lidiar"
  ],
  correctAnswer: 4,
  explanation: "Luchar, bregar y lidiar comparten el sentido de esfuerzo o enfrentamiento.",
  category: "vocabulario"
},
{
  question: "Elija el término que debe excluirse por alejarse del campo semántico.",
  options: [
    "Ceñudo",
    "Huraño",
    "Intratable",
    "Hosco",
    "Enajenado"
  ],
  correctAnswer: 4,
  explanation: "'Enajenado' se refiere más a estado mental que a actitud social.",
  category: "vocabulario"
},
{
  question: "Escoja el término que continúa la serie: Homogéneo, heterogéneo, uniforme,...",
  options: [
    "Ponderado",
    "Gigantesco",
    "Combinado",
    "Semejante",
    "Armonioso"
  ],
  correctAnswer: 3,
  explanation: "'Semejante' es coherente con la serie relacionada a la similitud.",
  category: "vocabulario"
},
{
  question: "Escoja el término que continúa la serie: Honra, pundonor, integridad, ...",
  options: ["prudencia", "celebridad", "riqueza", "decoro", "lustre"],
  correctAnswer: 3,
  explanation: "Decoro está relacionado con dignidad y honor, coherente con la serie.",
  category: "vocabulario"
},
{
  question: "Escoja el término que continúa la serie: Desfavorable, adverso, incómodo, ...",
  options: ["llano", "hostil", "vano", "terso", "escaso"],
  correctAnswer: 1,
  explanation: "'Hostil' comparte el mismo sentido negativo y conflictivo.",
  category: "vocabulario"
},
{
  question: "¿Qué valor ético y cívico resalta este fragmento de El Principito? «Solo con el corazón se puede ver bien; lo esencial es invisible a los ojos.»",
  options: ["El orgullo", "La empatía", "La obediencia", "La justicia"],
  correctAnswer: 1,
  explanation: "La frase destaca la empatía y la sensibilidad humana.",
  category: "comprension"
},
{
  question: "¿Qué enseñanza ética y cívica puede extraerse de este fragmento de Borges (La casa de Asterión)?",
  options: [
    "Que la violencia es necesaria para el orden",
    "Que la exclusión puede generar aislamiento y sufrimiento",
    "Que el poder otorga sabiduría",
    "Que la libertad es peligrosa"
  ],
  correctAnswer: 1,
  explanation: "El aislamiento del personaje refleja los efectos de la exclusión.",
  category: "comprension"
},
{
  question: "¿Cuál es el valor ético presente en estos versos de José Martí? \"Con los pobres de la tierra quiero yo mi suerte echar...\"",
  options: ["Ambición", "Individualismo", "Solidaridad", "Indiferencia"],
  correctAnswer: 2,
  explanation: "El verso exalta la solidaridad y el compromiso con los oprimidos.",
  category: "comprension"
},
{
  question: "¿Qué crítica representa esta frase de Rebelión en la granja? \"Todos los animales son iguales, pero algunos son más iguales que otros.\"",
  options: [
    "La igualdad total en todas las sociedades",
    "La manipulación del lenguaje y el poder",
    "El progreso de la granja",
    "La necesidad de líderes autoritarios"
  ],
  correctAnswer: 1,
  explanation: "La frase critica la hipocresía y corrupción del poder.",
  category: "comprension"
},
{
  question: "¿Qué valor predomina si Mateo decide decir la verdad para no poner en peligro a un inocente?",
  options: ["Fidelidad", "Justicia", "Miedo", "Orgullo"],
  correctAnswer: 1,
  explanation: "Decidir decir la verdad refleja sentido de justicia.",
  category: "comprension"
},
{
  question: "Seleccione las funciones del lenguaje presentes en el texto sobre el término 'cool':",
  options: [
    "1, 2, 3",
    "1, 3, 4",
    "1, 3, 5",
    "2, 4, 5"
  ],
  correctAnswer: 2,
  explanation: "Predominan las funciones apelativa, informativa y metalingüística.",
  category: "linguistica"
},
{
  question: "Identifique el nivel del lenguaje en el texto sobre los bosques amazónicos:",
  options: ["Coloquial", "Formal", "Vulgar", "Científico"],
  correctAnswer: 3,
  explanation: "El texto tiene estructura y léxico propios del discurso científico.",
  category: "comprension"
},
{
  question: "¿Cuál es la propuesta implícita en el texto del zorro y el tigre?",
  options: [
    "Todos los animales temían al zorro",
    "Nadie temía al zorro, pero sí al tigre",
    "El tigre era observador y perspicaz",
    "Los animales eran asustadizos de por sí y corrían"
  ],
  correctAnswer: 1,
  explanation: "El zorro era temido porque iba acompañado del tigre, no por sí mismo.",
  category: "comprension"
}
            ];
            
            // Game state
            let currentCardIndex = 0;
            let score = 0;
            let answeredQuestions = new Array(quizData.length).fill(false);
            let correctAnswers = new Array(quizData.length).fill(false);
            let filteredQuizData = [...quizData];
            let currentFilter = 'all';
            
            // DOM elements
            const card = document.getElementById('card');
            const questionElement = document.getElementById('question');
            const questionTextElement = document.getElementById('question-text');
            const optionsContainer = document.getElementById('options-container');
            const questionNumberElement = document.getElementById('question-number');
            const currentCardElement = document.getElementById('current-card');
            const totalCardsElement = document.getElementById('total-cards');
            const scoreElement = document.getElementById('score');
            const progressBar = document.getElementById('progress-bar');
            const resultTitle = document.getElementById('result-title');
            const resultIcon = document.getElementById('result-icon');
            const explanation = document.getElementById('explanation');
            const nextBtn = document.getElementById('next-btn');
            const prevBtn = document.getElementById('prev-btn');
            const flipBtn = document.getElementById('flip-btn');
            const restartBtn = document.getElementById('restart-btn');
            const resultsModal = document.getElementById('results-modal');
            const finalScore = document.getElementById('final-score');
            const correctCount = document.getElementById('correct-count');
            const incorrectCount = document.getElementById('incorrect-count');
            const restartModalBtn = document.getElementById('restart-modal-btn');
            const finalEmoji = document.getElementById('final-emoji');
            const categoryBadge = document.getElementById('category-badge');
            const timerElement = document.getElementById('timer');
            const filtersContainer = document.getElementById('filters-container');
            
            // Initialize the game
            function initGame() {
                totalCardsElement.textContent = filteredQuizData.length;
                updateCard();
                updateProgressBar();
                setupFilterButtons();
            }
            
            // Setup filter buttons
            function setupFilterButtons() {
                const filterButtons = filtersContainer.querySelectorAll('.filter-btn');
                
                filterButtons.forEach(button => {
                    button.addEventListener('click', () => {
                        // Update active state
                        filterButtons.forEach(btn => {
                            btn.classList.remove('active');
                            btn.classList.add('bg-opacity-20');
                            btn.classList.remove('bg-opacity-100');
                        });
                        
                        button.classList.add('active');
                        button.classList.remove('bg-opacity-20');
                        button.classList.add('bg-opacity-100');
                        
                        // Apply filter
                        const category = button.dataset.category;
                        currentFilter = category;
                        
                        if (category === 'all') {
                            filteredQuizData = [...quizData];
                        } else {
                            filteredQuizData = quizData.filter(item => item.category === category);
                        }
                        
                        // Reset game state for new filter
                        currentCardIndex = 0;
                        totalCardsElement.textContent = filteredQuizData.length;
                        updateCard();
                        updateProgressBar();
                    });
                });
            }
            
            // Update the current card
            function updateCard() {
                if (filteredQuizData.length === 0) {
                    // Handle case when no questions match the filter
                    questionElement.textContent = "No hay preguntas disponibles para esta categoría.";
                    optionsContainer.innerHTML = '';
                    return;
                }
                
                const currentQuestion = filteredQuizData[currentCardIndex];
                
                // Reset card flip
                card.classList.remove('flipped');
                
                // Update question number
                questionNumberElement.textContent = currentCardIndex + 1;
                currentCardElement.textContent = currentCardIndex + 1;
                
                // Update question text
                questionElement.textContent = currentQuestion.question;
                
                // Handle long text if present
                if (currentQuestion.longText) {
                    questionTextElement.textContent = currentQuestion.longText;
                    questionTextElement.classList.remove('hidden');
                } else {
                    questionTextElement.classList.add('hidden');
                }
                
                // Update category badge
                categoryBadge.className = `category-badge category-${currentQuestion.category}`;
                categoryBadge.textContent = currentQuestion.category === 'literatura' ? 'Literatura' : 
                                           currentQuestion.category === 'linguistica' ? 'Lingüística' : 
                                           currentQuestion.category === 'autores' ? 'Autores' :
                                           currentQuestion.category === 'vocabulario' ? 'Vocabulario' : 'Comprensión';
                
                // Clear options container
                optionsContainer.innerHTML = '';
                
                // Add options
                currentQuestion.options.forEach((option, index) => {
                    const optionElement = document.createElement('div');
                    const optionLetter = String.fromCharCode(97 + index); // a, b, c, ...
                    
                    optionElement.className = 'option p-3 rounded-lg flex items-start hover:bg-gray-100 animate-fadeIn';
                    optionElement.style.animationDelay = `${index * 0.1}s`;
                    
                    const originalIndex = quizData.indexOf(currentQuestion);
                    
                    if (answeredQuestions[originalIndex]) {
                        if (index === currentQuestion.correctAnswer) {
                            optionElement.classList.add('correct');
                        } else if (index === selectedOption && index !== currentQuestion.correctAnswer) {
                            optionElement.classList.add('incorrect');
                        }
                    }
                    
                    optionElement.innerHTML = `
                        <span class="font-semibold mr-2">${optionLetter}.</span>
                        <span>${option}</span>
                    `;
                    
                    optionElement.addEventListener('click', () => selectOption(index, originalIndex));
                    optionsContainer.appendChild(optionElement);
                });
                
                // Update button states
                prevBtn.disabled = currentCardIndex === 0;
                prevBtn.classList.toggle('opacity-50', currentCardIndex === 0);
                
                if (currentCardIndex === filteredQuizData.length - 1) {
                    nextBtn.textContent = 'Ver resultados';
                } else {
                    nextBtn.textContent = 'Siguiente pregunta';
                }
                
                // Show/hide restart button
                restartBtn.classList.toggle('hidden', !answeredQuestions.some(answered => answered));
                
                // Update explanation
                explanation.textContent = currentQuestion.explanation;
            }
            
            // Handle option selection
            let selectedOption = null;
            
            function selectOption(index, originalIndex) {
                if (answeredQuestions[originalIndex]) return;
                
                selectedOption = index;
                const currentQuestion = filteredQuizData[currentCardIndex];
                const isCorrect = index === currentQuestion.correctAnswer;
                
                // Mark question as answered
                answeredQuestions[originalIndex] = true;
                correctAnswers[originalIndex] = isCorrect;
                
                // Update score
                if (isCorrect) {
                    score += 10;
                    scoreElement.textContent = score;
                    resultTitle.textContent = '¡Correcto!';
                    resultIcon.textContent = '✅';
                } else {
                    resultTitle.textContent = '¡Incorrecto!';
                    resultIcon.textContent = '❌';
                }
                
                // Flip card
                card.classList.add('flipped');
                
                // Update progress
                updateProgressBar();
                
                // Add visual feedback to options
                const options = optionsContainer.querySelectorAll('.option');
                options[index].classList.add(isCorrect ? 'correct' : 'incorrect');
                
                if (!isCorrect) {
                    options[currentQuestion.correctAnswer].classList.add('correct');
                }
                
                // Add animation to correct answer
                if (isCorrect) {
                    options[index].classList.add('animated');
                    createConfetti();
                }
                
                // Check if all questions are answered
                if (answeredQuestions.every(answered => answered)) {
                    restartBtn.classList.remove('hidden');
                }
            }
            
            // Create confetti effect
            function createConfetti() {
                const colors = ['#ff0000', '#00ff00', '#0000ff', '#ffff00', '#ff00ff', '#00ffff'];
                
                for (let i = 0; i < 50; i++) {
                    const confetti = document.createElement('div');
                    confetti.className = 'confetti';
                    confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                    confetti.style.left = Math.random() * 100 + '%';
                    confetti.style.top = '-10px';
                    confetti.style.transform = `rotate(${Math.random() * 360}deg)`;
                    document.body.appendChild(confetti);
                    
                    // Animate confetti
                    const animation = confetti.animate(
                        [
                            { transform: `translate(0, 0) rotate(0deg)`, opacity: 1 },
                            { transform: `translate(${Math.random() * 100 - 50}px, ${window.innerHeight}px) rotate(${Math.random() * 720}deg)`, opacity: 0 }
                        ],
                        {
                            duration: 1500 + Math.random() * 1000,
                            easing: 'cubic-bezier(0.1, 0.8, 0.2, 1)'
                        }
                    );
                    
                    animation.onfinish = () => {
                        confetti.remove();
                    };
                }
            }
            
            // Update progress bar
            function updateProgressBar() {
                const answeredCount = answeredQuestions.filter(Boolean).length;
                const progress = (answeredCount / quizData.length) * 100;
                progressBar.style.width = `${progress}%`;
            }
            
            // Show results modal
            function showResults() {
                const totalCorrect = correctAnswers.filter(Boolean).length;
                const totalIncorrect = answeredQuestions.filter(Boolean).length - totalCorrect;
                
                finalScore.textContent = `${score} / ${quizData.length * 10}`;
                correctCount.textContent = totalCorrect;
                incorrectCount.textContent = totalIncorrect;
                
                // Set emoji based on score
                const percentage = score / (quizData.length * 10);
                if (percentage >= 0.9) {
                    finalEmoji.textContent = '🏆';
                } else if (percentage >= 0.7) {
                    finalEmoji.textContent = '🎉';
                } else if (percentage >= 0.5) {
                    finalEmoji.textContent = '👍';
                } else {
                    finalEmoji.textContent = '📚';
                }
                
                resultsModal.classList.remove('hidden');
            }
            
            // Event listeners
            nextBtn.addEventListener('click', () => {
                if (currentCardIndex < filteredQuizData.length - 1) {
                    currentCardIndex++;
                    updateCard();
                } else {
                    showResults();
                }
            });
            
            prevBtn.addEventListener('click', () => {
                if (currentCardIndex > 0) {
                    currentCardIndex--;
                    updateCard();
                }
            });
            
            flipBtn.addEventListener('click', () => {
                card.classList.toggle('flipped');
            });
            
            function restartGame() {
                currentCardIndex = 0;
                score = 0;
                answeredQuestions = new Array(quizData.length).fill(false);
                correctAnswers = new Array(quizData.length).fill(false);
                scoreElement.textContent = '0';
                resultsModal.classList.add('hidden');
                
                // Reset filters
                const allFilterBtn = document.querySelector('.filter-btn[data-category="all"]');
                if (allFilterBtn) {
                    allFilterBtn.click();
                } else {
                    currentFilter = 'all';
                    filteredQuizData = [...quizData];
                }
                
                updateCard();
                updateProgressBar();
            }
            
            restartBtn.addEventListener('click', restartGame);
            restartModalBtn.addEventListener('click', restartGame);
            
            // Initialize the game
            initGame();
        });
    </script>
</body>
</html>
