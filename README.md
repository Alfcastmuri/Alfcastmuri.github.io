# Alfcastmuri.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cyanotype Digital Negative</title>
    <!-- Tailwind CSS para estilos rápidos y responsivos -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Phosphor Icons para iconografía limpia -->
    <script src="https://unpkg.com/@phosphor-icons/web"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: {
                        prussian: {
                            50: '#f0f5fa',
                            100: '#e1ebf4',
                            200: '#c3d7ea',
                            300: '#94bce3',
                            400: '#5f9edb',
                            500: '#3a83cc',
                            600: '#2867aa',
                            700: '#21538a',
                            800: '#003153', /* Azul de Prusia Real */
                            900: '#1a3a5c',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Estética Blueprint (Plano) */
        body {
            background-color: #003153; /* Fondo base Prusia */
            background-image: 
                linear-gradient(rgba(255, 255, 255, 0.1) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255, 255, 255, 0.1) 1px, transparent 1px);
            background-size: 20px 20px;
            color: #ffffff;
        }

        /* Paneles con efecto cristal (Glassmorphism) */
        .glass-panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            color: #1e293b; /* slate-800 */
        }

        /* Slider Personalizado */
        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: transparent;
        }
        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 20px;
            width: 20px;
            border-radius: 50%;
            background: #003153;
            cursor: pointer;
            margin-top: -8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
            border: 2px solid white;
        }
        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 4px;
            cursor: pointer;
            background: #cbd5e1;
            border-radius: 2px;
        }
        input[type=range]:focus { outline: none; }

        /* Lienzos */
        .canvas-wrapper canvas {
            max-width: 100%;
            max-height: 45vh;
            object-fit: contain;
            border-radius: 0.5rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }
        .checkerboard-bg {
            background-image: 
                linear-gradient(45deg, #e2e8f0 25%, transparent 25%), 
                linear-gradient(-45deg, #e2e8f0 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #e2e8f0 75%), 
                linear-gradient(-45deg, transparent 75%, #e2e8f0 75%);
            background-size: 20px 20px;
            background-position: 0 0, 0 10px, 10px -10px, -10px 0px;
        }
    </style>
</head>
<body class="antialiased min-h-screen flex flex-col font-sans">

    <!-- Header -->
    <header class="bg-prussian-900/90 backdrop-blur border-b border-white/20 py-4 sticky top-0 z-40">
        <div class="container mx-auto px-4 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <i class="ph ph-sun-dim text-3xl text-white/90"></i>
                <div>
                    <h1 class="text-xl font-bold tracking-tight text-white">Digital Negative <span class="font-light">Pro</span></h1>
                    <p class="text-white/60 text-xs font-mono">Cyanotype Exposure Generator</p>
                </div>
            </div>
            <!-- Botón Configuración Oculta -->
            <button id="openSettingsBtn" class="p-2 rounded-full hover:bg-white/10 transition-colors text-white/80 hover:text-white" title="Configuración de Exposición">
                <i class="ph ph-gear text-2xl"></i>
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8 flex-grow flex flex-col lg:flex-row gap-8">
        
        <!-- Sidebar Controles -->
        <aside class="w-full lg:w-1/3 glass-panel p-6 rounded-2xl shadow-2xl flex flex-col gap-6 h-fit shrink-0 border border-white/20">
            
            <!-- Carga de Imagen -->
            <div>
                <h2 class="text-sm font-bold text-slate-800 uppercase tracking-wider mb-3">1. Seleccionar Archivo</h2>
                <label for="imageUpload" class="flex flex-col items-center justify-center w-full h-32 border-2 border-dashed border-prussian-300 rounded-xl cursor-pointer bg-prussian-50 hover:bg-prussian-100 transition-colors group">
                    <div class="flex flex-col items-center justify-center pt-5 pb-6">
                        <i class="ph ph-upload-simple text-4xl text-prussian-500 group-hover:-translate-y-1 transition-transform duration-300 mb-2"></i>
                        <p class="text-sm text-slate-600 font-medium">Click o arrastrar imagen</p>
                    </div>
                    <input id="imageUpload" type="file" class="hidden" accept="image/*" />
                </label>
            </div>

            <hr class="border-slate-200">

            <!-- Resumen de Exposición (Main UI) -->
            <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 shadow-inner">
                <h2 class="text-sm font-bold text-slate-800 uppercase tracking-wider mb-2 flex items-center justify-between">
                    <span>2. Tiempo de Exposición</span>
                    <i class="ph ph-timer text-prussian-600"></i>
                </h2>
                <div class="flex items-end justify-between">
                    <div>
                        <p class="text-[11px] text-slate-500 font-medium leading-tight mb-1" id="mainDistanceText">Calibrado a 20 cm de distancia</p>
                        <p class="text-[10px] text-slate-400">LED UV 50W (395-405nm)</p>
                    </div>
                    <div class="text-2xl font-mono font-bold text-prussian-800 tracking-tighter" id="mainTimeDisplay">
                        08:00
                    </div>
                </div>
            </div>

            <!-- Botón de Descarga -->
            <div class="mt-auto pt-4">
                <button id="downloadBtn" class="w-full bg-prussian-800 hover:bg-prussian-900 text-white font-bold py-3 px-4 rounded-xl flex items-center justify-center gap-2 transition-all shadow-lg disabled:opacity-50 disabled:cursor-not-allowed transform active:scale-95" disabled>
                    <i class="ph ph-printer text-xl"></i>
                    Descargar Acetato
                </button>
                <p class="text-[10px] text-center text-slate-400 mt-2">Se aplicará Auto-Contraste, Inversión, Espejo y Boost DMAX (1.35) automáticamente al descargar.</p>
            </div>
            
        </aside>

        <!-- Área de Previsualización -->
        <section class="w-full lg:w-2/3 flex flex-col gap-6">
            
            <!-- Estado Vacío -->
            <div id="emptyState" class="w-full h-full min-h-[60vh] flex flex-col items-center justify-center glass-panel rounded-2xl border border-white/20 shadow-xl">
                <i class="ph ph-images text-6xl text-slate-300 mb-4"></i>
                <h3 class="text-xl font-semibold text-slate-700">Esperando Fotografía</h3>
                <p class="text-slate-500 text-center max-w-md mt-2 text-sm">El sistema procesará la imagen aplicando los algoritmos fotográficos necesarios para la técnica de cianotipia.</p>
            </div>

            <!-- Previsualizaciones (Ocultas al inicio) -->
            <div id="previewGrid" class="hidden grid grid-cols-1 md:grid-cols-2 gap-6 w-full">
                
                <!-- Negativo -->
                <div class="glass-panel p-4 rounded-2xl shadow-xl flex flex-col border border-white/20">
                    <div class="flex items-center justify-between mb-3">
                        <h2 class="text-sm font-bold text-slate-800 flex items-center gap-2">
                            <i class="ph ph-film-strip text-prussian-600 text-lg"></i> Negativo (Acetato)
                        </h2>
                        <div class="flex gap-1">
                            <span class="bg-slate-200 text-slate-600 text-[9px] uppercase px-2 py-0.5 rounded font-bold">Invertido</span>
                            <span class="bg-slate-200 text-slate-600 text-[9px] uppercase px-2 py-0.5 rounded font-bold">Espejo</span>
                        </div>
                    </div>
                    <div class="canvas-wrapper flex-grow flex items-center justify-center bg-slate-200 rounded-xl overflow-hidden checkerboard-bg p-2 relative">
                        <canvas id="negativeCanvas" class="bg-white"></canvas>
                    </div>
                </div>

                <!-- Simulación -->
                <div class="bg-prussian-50 p-4 rounded-2xl shadow-xl flex flex-col border border-prussian-200">
                    <div class="flex items-center justify-between mb-3">
                        <h2 class="text-sm font-bold text-prussian-900 flex items-center gap-2">
                            <i class="ph ph-drop text-prussian-600 text-lg"></i> Simulación Final
                        </h2>
                        <span class="bg-prussian-200 text-prussian-800 text-[9px] uppercase px-2 py-0.5 rounded font-bold">Positivo</span>
                    </div>
                    <div class="canvas-wrapper flex-grow flex items-center justify-center bg-white rounded-xl overflow-hidden shadow-inner p-2">
                        <canvas id="simulationCanvas"></canvas>
                    </div>
                </div>

            </div>
        </section>
    </main>

    <!-- Modal de Configuración (Popup) -->
    <div id="settingsModal" class="fixed inset-0 z-50 flex items-center justify-center bg-prussian-900/80 backdrop-blur-sm hidden transition-opacity duration-300 opacity-0">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md p-6 transform scale-95 transition-transform duration-300" id="modalContent">
            
            <div class="flex items-center justify-between mb-6">
                <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                    <i class="ph ph-faders text-prussian-600 text-2xl"></i> Configuración de Luz UV
                </h2>
                <button id="closeSettingsBtn" class="text-slate-400 hover:text-red-500 transition-colors">
                    <i class="ph ph-x text-2xl"></i>
                </button>
            </div>

            <div class="bg-prussian-50 p-4 rounded-xl border border-prussian-100 mb-6">
                <p class="text-xs text-slate-600 mb-2">
                    <strong class="text-prussian-800">Hardware calibrado:</strong> Reflector LED UV 50W (395-405nm).<br>
                    El cálculo utiliza la <em>Ley del Inverso del Cuadrado</em> (Tiempo Base: 8 min a 20 cm).
                </p>
            </div>

            <!-- Slider de Distancia -->
            <div class="mb-8">
                <div class="flex justify-between items-end mb-2">
                    <label for="distanceSlider" class="text-sm font-semibold text-slate-700">Distancia de la Lámpara al Papel</label>
                    <span class="text-lg font-bold text-prussian-600 font-mono"><span id="distanceVal">20</span> cm</span>
                </div>
                <input type="range" id="distanceSlider" min="10" max="50" step="1" value="20" class="w-full">
                <div class="flex justify-between text-[10px] text-slate-400 mt-1 font-mono">
                    <span>10 cm</span>
                    <span>30 cm</span>
                    <span>50 cm</span>
                </div>
            </div>

            <!-- Display de Tiempo Recalculado -->
            <div class="bg-slate-900 text-white rounded-xl p-4 text-center shadow-inner relative overflow-hidden">
                <div class="absolute inset-0 bg-prussian-800/50 pattern-dots opacity-20"></div>
                <div class="relative z-10">
                    <p class="text-xs uppercase tracking-widest text-prussian-200 mb-1">Tiempo de Exposición Calculado</p>
                    <p class="text-4xl font-mono font-bold text-white tracking-tighter" id="modalTimeDisplay">08:00</p>
                </div>
            </div>

            <button id="applySettingsBtn" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-800 font-semibold py-2 px-4 rounded-xl mt-6 transition-colors">
                Aplicar y Cerrar
            </button>

        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- Elements ---
            const imageUpload = document.getElementById('imageUpload');
            const emptyState = document.getElementById('emptyState');
            const previewGrid = document.getElementById('previewGrid');
            const negativeCanvas = document.getElementById('negativeCanvas');
            const simulationCanvas = document.getElementById('simulationCanvas');
            const downloadBtn = document.getElementById('downloadBtn');
            
            // Modal Elements
            const settingsModal = document.getElementById('settingsModal');
            const modalContent = document.getElementById('modalContent');
            const openSettingsBtn = document.getElementById('openSettingsBtn');
            const closeSettingsBtn = document.getElementById('closeSettingsBtn');
            const applySettingsBtn = document.getElementById('applySettingsBtn');
            const distanceSlider = document.getElementById('distanceSlider');
            const distanceVal = document.getElementById('distanceVal');
            const modalTimeDisplay = document.getElementById('modalTimeDisplay');
            
            // Main UI Info Elements
            const mainTimeDisplay = document.getElementById('mainTimeDisplay');
            const mainDistanceText = document.getElementById('mainDistanceText');

            // --- State ---
            let originalImage = null;
            const BASE_TIME_MINUTES = 8;
            const BASE_DISTANCE_CM = 20;
            const DMAX_GAMMA = 1.35; // Aumenta densidad de negros en el negativo

            // --- Modal & Math Logic ---

            // Función para calcular la Ley del Inverso del Cuadrado
            // Formula: T2 = T1 * (D2 / D1)^2
            function calculateExposure(distance) {
                const ratio = distance / BASE_DISTANCE_CM;
                const newTimeMinutes = BASE_TIME_MINUTES * Math.pow(ratio, 2);
                
                // Formatear a MM:SS
                const mins = Math.floor(newTimeMinutes);
                const secs = Math.round((newTimeMinutes - mins) * 60);
                
                const timeString = `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
                return timeString;
            }

            // Actualizar textos al mover slider
            distanceSlider.addEventListener('input', (e) => {
                const dist = e.target.value;
                distanceVal.textContent = dist;
                modalTimeDisplay.textContent = calculateExposure(dist);
            });

            // Funciones de abrir/cerrar modal con animaciones Tailwind
            function openModal() {
                settingsModal.classList.remove('hidden');
                // Pequeño delay para permitir que la clase remove se aplique antes de la transición
                setTimeout(() => {
                    settingsModal.classList.remove('opacity-0');
                    modalContent.classList.remove('scale-95');
                    modalContent.classList.add('scale-100');
                }, 10);
            }

            function closeModal() {
                settingsModal.classList.add('opacity-0');
                modalContent.classList.remove('scale-100');
                modalContent.classList.add('scale-95');
                setTimeout(() => {
                    settingsModal.classList.add('hidden');
                }, 300); // match duration-300
                
                // Sincronizar UI principal al cerrar
                const currentDist = distanceSlider.value;
                mainDistanceText.textContent = `Calibrado a ${currentDist} cm de distancia`;
                mainTimeDisplay.textContent = calculateExposure(currentDist);
            }

            openSettingsBtn.addEventListener('click', openModal);
            closeSettingsBtn.addEventListener('click', closeModal);
            applySettingsBtn.addEventListener('click', closeModal);

            // --- Image Processing Logic ---

            imageUpload.addEventListener('change', (e) => {
                const file = e.target.files[0];
                if (!file) return;

                const reader = new FileReader();
                reader.onload = (event) => {
                    const img = new Image();
                    img.onload = () => {
                        originalImage = img;
                        
                        // Switch UI views
                        emptyState.classList.add('hidden');
                        previewGrid.classList.remove('hidden');
                        downloadBtn.disabled = false;
                        
                        processImage();
                    };
                    img.src = event.target.result;
                };
                reader.readAsDataURL(file);
            });

            function processImage() {
                if (!originalImage) return;

                const w = originalImage.width;
                const h = originalImage.height;

                // Setup display canvases dimensions
                negativeCanvas.width = w; 
                negativeCanvas.height = h;
                simulationCanvas.width = w; 
                simulationCanvas.height = h;

                // Offscreen canvas for raw data
                const offCanvas = document.createElement('canvas');
                offCanvas.width = w; 
                offCanvas.height = h;
                const offCtx = offCanvas.getContext('2d');
                offCtx.drawImage(originalImage, 0, 0, w, h);
                
                const imgData = offCtx.getImageData(0, 0, w, h);
                const data = imgData.data;

                const grayscale = new Uint8ClampedArray(w * h);
                let minG = 255;
                let maxG = 0;

                // Paso 1: Escala de grises + Encontrar Min/Max para Auto-Contraste (Histogram Stretch)
                for (let i = 0; i < data.length; i += 4) {
                    const luma = (data[i] * 0.299) + (data[i + 1] * 0.587) + (data[i + 2] * 0.114);
                    grayscale[i / 4] = luma;
                    if (luma < minG) minG = luma;
                    if (luma > maxG) maxG = luma;
                }

                const range = (maxG - minG) === 0 ? 1 : (maxG - minG);

                const negData = new ImageData(w, h);
                const simData = new ImageData(w, h);

                // Colores para la simulación
                const prussianBlue = { r: 0, g: 49, b: 83 }; // Azul denso (áreas oscuras expuestas a UV)
                const paperWhite = { r: 250, g: 252, b: 255 }; // Blanco del papel

                // Paso 2: Procesamiento Píxel por Píxel
                for (let i = 0; i < grayscale.length; i++) {
                    let val = grayscale[i];

                    // A. Auto-Optimización (Histogram Stretch)
                    // Estira los valores para que el más oscuro sea 0 y el más claro 255.
                    val = ((val - minG) / range) * 255;
                    val = Math.max(0, Math.min(255, val));

                    const pixelIdx = i * 4;

                    // --- Construir el Negativo Digital ---
                    // 1. Invertir la imagen (0 -> 255, 255 -> 0)
                    let negVal = 255 - val;
                    
                    // 2. Boost DMAX (Exponencial)
                    // Oscurece los tonos oscuros del negativo para depositar más tinta negra y bloquear el LED UV 50W.
                    // Al elevar un decimal (0.0 a 1.0) a una potencia > 1, el número se hace más pequeño (más cercano a 0 = Negro).
                    negVal = Math.pow(negVal / 255, DMAX_GAMMA) * 255;
                    
                    negData.data[pixelIdx] = negVal;     // R
                    negData.data[pixelIdx + 1] = negVal; // G
                    negData.data[pixelIdx + 2] = negVal; // B
                    negData.data[pixelIdx + 3] = 255;    // Alpha

                    // --- Construir la Simulación ---
                    // En la cianotipia, lo oscuro en la foto original queda claro en el negativo, deja pasar la luz y se tiñe de azul.
                    // Lo claro en la original queda negro en el negativo, bloquea la luz, y se lava dejando el papel blanco.
                    // Interpolamos linealmente entre Azul de Prusia (val=0) y Blanco (val=255).
                    const mix = val / 255;
                    simData.data[pixelIdx] = prussianBlue.r + mix * (paperWhite.r - prussianBlue.r);
                    simData.data[pixelIdx + 1] = prussianBlue.g + mix * (paperWhite.g - prussianBlue.g);
                    simData.data[pixelIdx + 2] = prussianBlue.b + mix * (paperWhite.b - prussianBlue.b);
                    simData.data[pixelIdx + 3] = 255;
                }

                // Paso 3: Dibujar en los Canvases (Aplicando Espejo al Negativo)
                
                // Simulación (Positivo) - Dibujado directo
                const simCtx = simulationCanvas.getContext('2d');
                simCtx.putImageData(simData, 0, 0);

                // Negativo (Invertido + Espejo Horizontal para contacto emulsión-tinta)
                const tempCanvas = document.createElement('canvas');
                tempCanvas.width = w; tempCanvas.height = h;
                tempCanvas.getContext('2d').putImageData(negData, 0, 0);
                
                const negCtx = negativeCanvas.getContext('2d');
                negCtx.clearRect(0, 0, w, h);
                negCtx.save();
                negCtx.translate(w, 0); // Mover origen a la derecha
                negCtx.scale(-1, 1);    // Voltear horizontalmente
                negCtx.drawImage(tempCanvas, 0, 0);
                negCtx.restore();
            }

            // --- Download Handling ---
            downloadBtn.addEventListener('click', () => {
                if (!originalImage) return;
                
                // Se descarga el canvas del negativo, que ya tiene el efecto espejo y DMAX aplicado
                const dataURL = negativeCanvas.toDataURL('image/png', 1.0);
                const link = document.createElement('a');
                link.download = 'Cianotipia_Negativo_Pro.png';
                link.href = dataURL;
                link.click();
            });

        });
    </script>
</body>
</html>
