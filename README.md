# propuestaMimmo
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotización Interactiva - Proyecto Mimmo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: Se ha diseñado una SPA tipo dashboard para facilitar la comprensión de la cotización. La estructura se divide en: 1) Un resumen general con métricas clave (costo, plazo) y un gráfico de desglose. 2) Un interruptor interactivo para el servicio opcional que actualiza dinámicamente todos los costos y el plan de pago, permitiendo al cliente visualizar el impacto de su decisión de forma inmediata. 3) Secciones claras para el alcance del proyecto, el plan de pagos y los detalles finales. Esta estructura fue elegida para transformar una cotización estática en una herramienta de decisión interactiva, mejorando la claridad y la confianza del cliente. -->
    <!-- Visualization & Content Choices: 
        - Report Info: Inversión del Proyecto. -> Goal: Comparar componentes del costo. -> Viz/Method: Gráfico de dona (Chart.js/Canvas). -> Interaction: El gráfico muestra el desglose del costo base; los totales numéricos se actualizan con un interruptor. -> Justification: Un gráfico de dona es ideal para mostrar proporciones de un total de forma clara y visual.
        - Report Info: Servicio Opcional y Condiciones de Pago. -> Goal: Mostrar el impacto de una opción binaria. -> Viz/Method: Interruptor (HTML/CSS/JS) y campos de texto dinámicos. -> Interaction: Al hacer clic, se recalculan y actualizan en tiempo real el valor total y las cuotas. -> Justification: Es la forma más directa e intuitiva de visualizar el efecto de una opción adicional sobre el presupuesto.
        - Report Info: Alcance del proyecto. -> Goal: Organizar los entregables. -> Viz/Method: Lista estructurada con íconos (HTML/Tailwind). -> Interaction: Ninguna, se prioriza la legibilidad. -> Justification: Una lista visualmente atractiva es más fácil de procesar que un bloque de texto.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8f7f4; 
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 320px;
            margin-left: auto;
            margin-right: auto;
            height: 320px;
        }
        .switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }
        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 34px;
        }
        .slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        input:checked + .slider {
            background-color: #f59e0b;
        }
        input:checked + .slider:before {
            transform: translateX(26px);
        }
    </style>
</head>
<body class="text-gray-800">

    <div class="container mx-auto p-4 sm:p-6 md:p-8 max-w-6xl">
        
        <header class="text-center mb-10">
            <h1 class="text-3xl md:text-4xl font-bold text-amber-700">Propuesta Interactiva: Proyecto Web Mimmo</h1>
            <p class="text-lg text-gray-600 mt-2">Una visualización de la cotización para una fácil comprensión.</p>
        </header>

        <main class="space-y-12">

            <!-- Resumen General y Costos -->
            <section id="summary" class="bg-white p-6 rounded-2xl shadow-lg">
                <h2 class="text-2xl font-bold mb-6 text-center">Resumen de la Inversión</h2>
                
                <div class="grid md:grid-cols-5 gap-8 items-center">
                    
                    <div class="md:col-span-3 space-y-6">
                        <div class="flex items-center justify-between bg-amber-50 p-4 rounded-xl">
                            <span class="font-semibold text-lg">Servicio Opcional (Dominio + Hosting)</span>
                            <label class="switch">
                                <input type="checkbox" id="optionalServiceToggle">
                                <span class="slider"></span>
                            </label>
                        </div>

                        <div class="grid sm:grid-cols-2 gap-4 text-center">
                            <div class="bg-gray-50 p-4 rounded-xl">
                                <h3 class="text-gray-500 font-semibold">Valor Total del Proyecto</h3>
                                <p id="totalValue" class="text-3xl font-bold text-amber-600 mt-1">$130.000</p>
                            </div>
                            <div class="bg-gray-50 p-4 rounded-xl">
                                <h3 class="text-gray-500 font-semibold">Plazo de Entrega Estimado</h3>
                                <p class="text-3xl font-bold text-amber-600 mt-1">3-5</p>
                                <span class="text-sm">días hábiles</span>
                            </div>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-xl text-center">
                             <p class="text-gray-600"><strong class="text-gray-800">Contexto:</strong> El objetivo es finalizar la web de Mimmo para reflejar su esencia de confianza y profesionalismo, completando el desarrollo existente en Next.js para crear una vitrina digital cálida y funcional.</p>
                        </div>
                    </div>
                    
                    <div class="md:col-span-2">
                        <h3 class="text-xl font-bold text-center mb-4">Desglose del Costo Base</h3>
                        <div class="chart-container">
                            <canvas id="costChart"></canvas>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Alcance del Proyecto -->
            <section id="scope" class="bg-white p-6 rounded-2xl shadow-lg">
                <h2 class="text-2xl font-bold mb-6 text-center">Alcance del Proyecto</h2>
                <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Esta sección detalla todas las páginas y componentes que se completarán, conectarán y desplegarán como parte de este proyecto para asegurar una experiencia de usuario coherente y fluida.</p>
                <div class="grid sm:grid-cols-2 md:grid-cols-3 gap-6">
                    <div class="bg-gray-50 p-4 rounded-xl border-l-4 border-amber-500">
                        <h3 class="font-bold text-lg">Página Principal</h3>
                        <ul class="mt-2 text-gray-700 list-inside space-y-1">
                            <li><span class="text-amber-600 mr-2">✔</span>Header y Navegación</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Sección Hero</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Muestra de Servicios</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Sección Social y Footer</li>
                        </ul>
                    </div>
                    <div class="bg-gray-50 p-4 rounded-xl border-l-4 border-amber-500">
                        <h3 class="font-bold text-lg">Páginas de Contenido</h3>
                        <ul class="mt-2 text-gray-700 list-inside space-y-1">
                            <li><span class="text-amber-600 mr-2">✔</span>Página "Acerca de"</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Página de "Servicios"</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Sección Misión y Visión</li>
                        </ul>
                    </div>
                    <div class="bg-gray-50 p-4 rounded-xl border-l-4 border-amber-500">
                        <h3 class="font-bold text-lg">Funcionalidades Clave</h3>
                        <ul class="mt-2 text-gray-700 list-inside space-y-1">
                            <li><span class="text-amber-600 mr-2">✔</span>Formulario de contacto funcional</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Botón de WhatsApp</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Ajustes de componentes</li>
                            <li><span class="text-amber-600 mr-2">✔</span>Transiciones fluidas</li>
                        </ul>
                    </div>
                </div>
            </section>

            <!-- Plan de Pagos -->
            <section id="payment" class="bg-white p-6 rounded-2xl shadow-lg">
                <h2 class="text-2xl font-bold mb-6 text-center">Plan de Pagos Sugerido</h2>
                <p class="text-center text-gray-600 mb-8 max-w-3xl mx-auto">Para facilitar el proceso, el valor total del proyecto se puede dividir en 3 cuotas. Los montos se ajustarán automáticamente si decides incluir el servicio opcional de dominio y hosting.</p>
                <div class="grid sm:grid-cols-3 gap-6 text-center">
                    <div class="bg-amber-50 p-6 rounded-xl transform transition hover:scale-105">
                        <div class="text-2xl font-bold text-amber-700 bg-white rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4 border-2 border-amber-200">40%</div>
                        <h3 class="font-semibold text-lg">Primera Cuota</h3>
                        <p id="payment1" class="text-2xl font-bold mt-1">$52.000</p>
                        <p class="text-sm text-gray-600 mt-1">Para iniciar el proyecto</p>
                    </div>
                    <div class="bg-amber-50 p-6 rounded-xl transform transition hover:scale-105">
                        <div class="text-2xl font-bold text-amber-700 bg-white rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4 border-2 border-amber-200">30%</div>
                        <h3 class="font-semibold text-lg">Segunda Cuota</h3>
                        <p id="payment2" class="text-2xl font-bold mt-1">$39.000</p>
                        <p class="text-sm text-gray-600 mt-1">Contra entrega para revisión</p>
                    </div>
                    <div class="bg-amber-50 p-6 rounded-xl transform transition hover:scale-105">
                        <div class="text-2xl font-bold text-amber-700 bg-white rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4 border-2 border-amber-200">30%</div>
                        <h3 class="font-semibold text-lg">Tercera Cuota</h3>
                        <p id="payment3" class="text-2xl font-bold mt-1">$39.000</p>
                        <p class="text-sm text-gray-600 mt-1">Al finalizar y publicar</p>
                    </div>
                </div>
                 <p id="paymentNote" class="text-center text-sm text-gray-500 mt-6 hidden">Nota: Las cuotas de la propuesta original ($60k, $45k, $45k) ya consideraban el servicio opcional incluido.</p>
            </section>
            
             <!-- Detalles Finales -->
            <section id="details" class="bg-white p-6 rounded-2xl shadow-lg">
                <h2 class="text-2xl font-bold mb-6 text-center">Detalles Adicionales y Futuro</h2>
                <div class="grid md:grid-cols-2 gap-8">
                    <div>
                        <h3 class="font-bold text-lg mb-2">Observaciones Finales</h3>
                        <ul class="space-y-2 text-gray-700">
                           <li class="flex items-start"><span class="text-amber-600 font-bold mr-3 mt-1">›</span><span>El <em class="font-semibold">copywriting</em> (textos) final será entregado por el equipo de Mimmo.</span></li>
                           <li class="flex items-start"><span class="text-amber-600 font-bold mr-3 mt-1">›</span><span>El diseño busca reflejar un tono emocional, cálido y profesional.</span></li>
                           <li class="flex items-start"><span class="text-amber-600 font-bold mr-3 mt-1">›</span><span>El valor incluye una (1) ronda de revisiones y ajustes finales posterior a la entrega.</span></li>
                           <li class="flex items-start"><span class="text-amber-600 font-bold mr-3 mt-1">›</span><span>Cambios fuera del alcance original serán cotizados por separado.</span></li>
                        </ul>
                    </div>
                     <div>
                        <h3 class="font-bold text-lg mb-2">Escalabilidad Futura</h3>
                        <p class="text-gray-700 mb-3">La arquitectura en Next.js es ideal para futuras expansiones, permitiendo añadir funcionalidades complejas de manera eficiente:</p>
                        <div class="flex flex-wrap gap-2">
                            <span class="bg-gray-100 text-gray-800 px-3 py-1 rounded-full text-sm">Blog informativo</span>
                            <span class="bg-gray-100 text-gray-800 px-3 py-1 rounded-full text-sm">Sistema de reservas</span>
                            <span class="bg-gray-100 text-gray-800 px-3 py-1 rounded-full text-sm">Área de clientes</span>
                            <span class="bg-gray-100 text-gray-800 px-3 py-1 rounded-full text-sm">E-commerce</span>
                        </div>
                    </div>
                </div>
            </section>
        </main>
        
        <footer class="text-center mt-12 pt-8 border-t border-gray-200">
            <p class="font-bold">Kevin Steven Perlaza Ordoñez</p>
            <p class="text-gray-600">Cotización generada el 15 de Junio de 2025 para Mimmo.</p>
        </footer>

    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const optionalServiceToggle = document.getElementById('optionalServiceToggle');
            const totalValueEl = document.getElementById('totalValue');
            const payment1El = document.getElementById('payment1');
            const payment2El = document.getElementById('payment2');
            const payment3El = document.getElementById('payment3');
            const paymentNoteEl = document.getElementById('paymentNote');

            const costs = {
                base: 130000,
                optional: 20000,
                totalWithOptional: 150000
            };
            
            const formatCurrency = (value) => {
                return new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP', minimumFractionDigits: 0 }).format(value);
            };

            const updateValues = () => {
                const includeOptional = optionalServiceToggle.checked;
                
                if (includeOptional) {
                    totalValueEl.textContent = formatCurrency(costs.totalWithOptional);
                    payment1El.textContent = formatCurrency(costs.totalWithOptional * 0.4);
                    payment2El.textContent = formatCurrency(costs.totalWithOptional * 0.3);
                    payment3El.textContent = formatCurrency(costs.totalWithOptional * 0.3);
                    paymentNoteEl.classList.remove('hidden');
                } else {
                    totalValueEl.textContent = formatCurrency(costs.base);
                    payment1El.textContent = formatCurrency(costs.base * 0.4);
                    payment2El.textContent = formatCurrency(costs.base * 0.3);
                    payment3El.textContent = formatCurrency(costs.base * 0.3);
                    paymentNoteEl.classList.add('hidden');
                }
            };

            optionalServiceToggle.addEventListener('change', updateValues);
            
            updateValues();

            const ctx = document.getElementById('costChart').getContext('2d');
            const costChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: [
                        'Desarrollo y Diseño',
                        'Revisión y Ajustes',
                        'Funcionalidades Backend',
                        'Despliegue y Pruebas',
                    ],
                    datasets: [{
                        label: 'Desglose de Costos',
                        data: [50000, 30000, 30000, 20000],
                        backgroundColor: [
                            '#f59e0b',
                            '#fbbf24',
                            '#fcd34d',
                            '#fef3c7'
                        ],
                        borderColor: '#f8f7f4',
                        borderWidth: 4,
                        hoverOffset: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '60%',
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                padding: 15,
                                font: {
                                    family: 'Inter',
                                    size: 12
                                }
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed !== null) {
                                        label += formatCurrency(context.parsed);
                                    }
                                    return label;
                                }
                            }
                        }
                    }
                }
            });
        });
    </script>

</body>
</html>
