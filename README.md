<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestión de Pagos</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        html {
            font-size: 16px;
        }

        /* Fondo CIAN 72% sólido (#E9EBFF) */
        body {
            background-color: #E9EBFF !important;
        }

        /* Tipografías personalizadas */
        .font-arial-8 {
            font-family: Arial, sans-serif;
            font-size: 8.5px;
        }
        .font-arial-9 {
            font-family: Arial, sans-serif;
            font-size: 9.5px;
        }
        .font-arial-11 {
            font-family: Arial, sans-serif;
            font-size: 11px;
        }
        .font-arial-12 {
            font-family: Arial, sans-serif;
            font-size: 12.5px;
        }
        .font-arial-13 {
            font-family: Arial, sans-serif;
            font-size: 13.5px;
        }
        .font-arial-18 {
            font-family: Arial, sans-serif;
            font-size: 17px;
        }
        .font-arial-20 {
            font-family: Arial, sans-serif;
            font-size: 19px;
        }
        .text-note {
            font-size: 0.75rem;
            line-height: 1.3;
        }

        /* Colores personalizados Azul (#4153B9) y Verde (#00A859) */
        .text-blue-primary { color: #4153B9; }
        .bg-blue-primary { background-color: #4153B9; }
        .border-blue-primary { border-color: #4153B9; }

        .text-green-76 { color: #00A859; }
        .bg-green-76 { background-color: #00A859; }
        .border-green-76 { border-color: #00A859; }
        .bg-green-soft-76 { background-color: rgba(0, 168, 89, 0.08); }

        .solid-divider {
            background-color: #00A859;
            height: 4px;
            border-radius: 9999px;
        }

        /* Sticker realista de pago */
        .payment-sticker {
            background: linear-gradient(135deg, #ffffff 0%, #f0fdf4 100%);
            border: 2.5px solid #ffffff;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.2), 0 4px 10px -2px rgba(0, 0, 0, 0.1);
            transform: rotate(-3deg);
            transition: transform 0.3s ease;
        }
        .payment-sticker:hover {
            transform: rotate(0deg) scale(1.05);
        }

        /* Animaciones */
        .animate-pop {
            animation: popIn 0.25s ease-out forwards;
        }
        @keyframes popIn {
            0% {
                opacity: 0;
                transform: scale(0.95);
            }
            100% {
                opacity: 1;
                transform: scale(1);
            }
        }

        .animate-step-transition {
            animation: slideInRight 0.25s ease-out forwards;
        }
        @keyframes slideInRight {
            0% {
                opacity: 0;
                transform: translateX(12px);
            }
            100% {
                opacity: 1;
                transform: translateX(0);
            }
        }

        @keyframes marquee {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }
        .animate-marquee {
            display: inline-block;
            white-space: nowrap;
            animation: marquee 16s linear infinite;
        }
        .marquee-container:hover .animate-marquee {
            animation-play-state: paused;
        }
    </style>
</head>
<body class="bg-[#E9EBFF] text-slate-800 font-sans min-h-screen py-4 px-3 flex flex-col justify-center items-center">

    <!-- Header / Indicador Tasa BCV y Título Superior -->
    <header id="mainHeader" class="w-full max-w-md md:max-w-xl mx-auto mb-3 flex flex-col items-center text-center space-y-2">
        <!-- Indicador BCV -->
        <div class="inline-flex items-center gap-2 bg-white backdrop-blur-md border border-[#4153B9]/20 text-[#4153B9] font-arial-12 font-semibold px-4 py-1.5 rounded-full shadow-sm">
            <span class="relative flex h-2.5 w-2.5">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-[#00A859] opacity-75"></span>
                <span class="relative inline-flex rounded-full h-2.5 w-2.5 bg-[#00A859]"></span>
            </span>
            <span>Tasa Oficial BCV:</span>
            <span id="bcvIndicator" class="font-bold text-slate-900">Sincronizando...</span>
            <button onclick="fetchBcvRate()" title="Actualizar tasa BCV en línea" class="ml-1 text-slate-400 hover:text-[#00A859] transition-colors p-0.5">
                <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
                </svg>
            </button>
        </div>

        <!-- Título principal con Sticker Realista de Pago -->
        <div class="text-center pt-1 flex flex-col items-center">
            <div class="flex items-center justify-center gap-3 mb-1">
                <h1 class="text-2xl md:text-3xl font-extrabold text-[#4153B9] tracking-tight drop-shadow-sm">Gestión de Pagos</h1>
                <!-- Sticker Realista de Pago -->
                <div class="payment-sticker px-2.5 py-1 rounded-2xl flex items-center gap-1.5 cursor-pointer" title="Pago 100% Verificado">
                    <svg class="w-5 h-5 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"></path>
                    </svg>
                    <span class="font-arial-8 font-black uppercase text-slate-800 tracking-wider">Pago Seguro</span>
                </div>
            </div>

            <!-- Subtítulo con Badge Solo para afiliados -->
            <p class="font-arial-13 text-slate-700 font-medium mt-1">
                Reporta tu pago en esta sección <span class="bg-[#4153B9]/10 border border-[#4153B9]/20 px-2.5 py-0.5 rounded-full font-bold inline-block mt-1 sm:mt-0 text-[#4153B9] shadow-sm">🔒 Solo para afiliados!</span>
            </p>
        </div>
    </header>

    <!-- Contenedor Principal (Tarjeta Blanca #FFFFFF) -->
    <main class="w-full max-w-md md:max-w-xl bg-white rounded-3xl shadow-lg border border-slate-200 p-4 md:p-6 transition-all duration-300">
        
        <!-- Vista Título Principal y Bienvenida -->
        <div id="mainTitleView" class="mb-4 text-center -mt-1">
            <div class="w-full overflow-hidden bg-green-soft-76 py-1.5 px-2 rounded-lg mb-2 border border-[#00A859]/30 marquee-container">
                <div class="animate-marquee font-arial-12 font-medium text-[#00A859]">
                    ✨ ¡Bienvenido a nuestra página de Gestión de pagos! ✨
                </div>
            </div>

            <p class="font-arial-12 text-slate-500 mt-1">Selecciona tu método preferido para realizar y reportar tu pago</p>
            <div class="solid-divider w-16 mx-auto mt-2.5"></div>
        </div>

        <!-- Indicador de Pasos (Stepper Nav) -->
        <div id="stepperNav" class="hidden mb-4 pb-3 border-b border-slate-100 flex items-center justify-between px-2">
            <div class="flex items-center gap-2">
                <div id="stepDot1" class="w-7 h-7 rounded-full bg-green-soft-76 text-[#00A859] border border-[#00A859] flex items-center justify-center font-arial-12 font-bold transition-all">1</div>
                <span class="font-arial-9 text-slate-400 uppercase tracking-wider hidden sm:inline">Monto</span>
            </div>
            <div class="h-0.5 w-8 bg-slate-200 flex-1 mx-2"></div>
            <div class="flex items-center gap-2">
                <div id="stepDot2" class="w-7 h-7 rounded-full bg-slate-100 text-slate-400 flex items-center justify-center font-arial-12 font-bold transition-all">2</div>
                <span class="font-arial-9 text-slate-400 uppercase tracking-wider hidden sm:inline">Titular</span>
            </div>
            <div class="h-0.5 w-8 bg-slate-200 flex-1 mx-2"></div>
            <div class="flex items-center gap-2">
                <div id="stepDot3" class="w-7 h-7 rounded-full bg-slate-100 text-slate-400 flex items-center justify-center font-arial-12 font-bold transition-all">3</div>
                <span class="font-arial-9 text-slate-400 uppercase tracking-wider hidden sm:inline">Detalles</span>
            </div>
        </div>

        <!-- Vista Selección de Método de Pago -->
        <div id="methodSelectorView" class="space-y-2.5">
            <!-- Opción 1: Pago Móvil -->
            <button onclick="selectPaymentMethod('pago_movil')" class="w-full text-left p-3.5 rounded-2xl border border-slate-200 hover:border-[#00A859] hover:bg-green-soft-76/40 transition-all duration-200 flex items-center justify-between group shadow-sm hover:shadow">
                <div class="flex items-center gap-3">
                    <div class="w-11 h-11 rounded-xl bg-green-soft-76 text-[#00A859] flex items-center justify-center group-hover:scale-105 transition-transform">
                        <svg class="w-6 h-6 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 18h.01M8 21h8a2 2 0 002-2V5a2 2 0 00-2-2H8a2 2 0 00-2 2v14a2 2 0 002 2z"></path>
                        </svg>
                    </div>
                    <div>
                        <h3 class="font-bold text-[#4153B9] font-arial-13 group-hover:text-slate-900">Pago Móvil</h3>
                        <p class="font-arial-11 text-slate-500">Pago rápido e instantáneo en Bolívares (Bs)</p>
                    </div>
                </div>
                <svg class="w-5 h-5 text-[#4153B9] group-hover:text-[#00A859] group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
            </button>

            <!-- Opción 2: Transferencia Bancaria -->
            <button onclick="selectPaymentMethod('transferencia')" class="w-full text-left p-3.5 rounded-2xl border border-slate-200 hover:border-[#00A859] hover:bg-green-soft-76/40 transition-all duration-200 flex items-center justify-between group shadow-sm hover:shadow">
                <div class="flex items-center gap-3">
                    <div class="w-11 h-11 rounded-xl bg-green-soft-76 text-[#00A859] flex items-center justify-center group-hover:scale-105 transition-transform">
                        <svg class="w-6 h-6 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 14v3m4-3v3m4-3v3M3 21h18M3 10h18M3 7l9-4 9 4M4 10h16v11H4V10z"></path>
                        </svg>
                    </div>
                    <div>
                        <h3 class="font-bold text-[#4153B9] font-arial-13 group-hover:text-slate-900">Transferencia Bancaria</h3>
                        <p class="font-arial-11 text-slate-500">Transferencia desde cualquier banco nacional</p>
                    </div>
                </div>
                <svg class="w-5 h-5 text-[#4153B9] group-hover:text-[#00A859] group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
            </button>

            <!-- Opción 3: Pago Cash USD -->
            <button onclick="selectPaymentMethod('cash_usd')" class="w-full text-left p-3.5 rounded-2xl border border-slate-200 hover:border-[#00A859] hover:bg-green-soft-76/40 transition-all duration-200 flex items-center justify-between group shadow-sm hover:shadow">
                <div class="flex items-center gap-3">
                    <div class="w-11 h-11 rounded-xl bg-green-soft-76 text-[#00A859] flex items-center justify-center group-hover:scale-105 transition-transform">
                        <svg class="w-6 h-6 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                    </div>
                    <div>
                        <h3 class="font-bold text-[#4153B9] font-arial-13 group-hover:text-slate-900">Pago Cash USD</h3>
                        <p class="font-arial-11 text-slate-500">Depósito en divisas / Cuenta custodia USD</p>
                    </div>
                </div>
                <svg class="w-5 h-5 text-[#4153B9] group-hover:text-[#00A859] group-hover:translate-x-1 transition-all" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
            </button>
        </div>

        <!-- PASO 1: Monto y Datos Bancarios -->
        <div id="step1View" class="hidden animate-step-transition">
            <div class="flex items-center justify-between mb-3">
                <button onclick="goBackToMain()" class="inline-flex items-center gap-1.5 font-arial-11 font-semibold text-slate-600 hover:text-slate-900 bg-slate-100 hover:bg-slate-200 px-3 py-1.5 rounded-xl transition-all">
                    <svg class="w-4 h-4 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path>
                    </svg>
                    <span>Cambiar Método</span>
                </button>
                <span id="step1Badge" class="bg-green-soft-76 text-[#00A859] border border-[#00A859]/30 font-arial-11 font-bold px-3 py-1 rounded-full">Paso 1 de 3</span>
            </div>

            <div class="mb-3">
                <div class="flex items-center justify-between">
                    <h2 id="methodTitleStep1" class="text-lg font-bold text-[#4153B9] tracking-tight">Detalles de Pago</h2>
                    <button onclick="openPlanModal()" class="text-xs font-bold text-[#00A859] hover:underline bg-green-soft-76 px-2.5 py-1 rounded-lg border border-[#00A859]/30 flex items-center gap-1">
                        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"></path>
                        </svg>
                        <span>Cambiar Plan</span>
                    </button>
                </div>
                <p class="font-arial-11 text-slate-500">Transfiere el monto indicado a la siguiente cuenta:</p>
            </div>

            <!-- Tarjeta Monto Centrado -->
            <div class="bg-green-soft-76 border border-[#00A859]/40 rounded-2xl p-3 mb-3 text-center">
                <span id="selectedPlanBadge" class="inline-block bg-[#00A859] text-white font-arial-9 font-bold px-2.5 py-0.5 rounded-full mb-1">Plan familiar Integral</span>
                <p class="font-arial-11 font-bold uppercase tracking-wider text-[#00A859] mb-0.5">Monto a pagar</p>
                <div id="usdAmountDisplay" class="font-arial-20 font-black text-slate-900 tracking-tight">$ 2.00 USD</div>
                <div id="bsAmountContainer" class="mt-1 pt-1 border-t border-[#00A859]/20 font-arial-12 font-bold text-[#00A859]">
                    Equivalente BCV: <span id="bsAmountText" class="text-slate-900 font-extrabold">Calculando...</span>
                </div>
            </div>

            <!-- Tarjeta Datos Bancarios -->
            <div id="bankDetailsCard" class="bg-slate-50 rounded-2xl p-2.5 border border-slate-200 mb-3 space-y-0.5">
                <!-- Se llena dinámicamente con JS -->
            </div>

            <p class="text-note font-arial-11 text-slate-500 mb-3 bg-slate-100 p-2.5 rounded-xl border border-slate-200 flex items-start gap-2">
                <svg class="w-4 h-4 text-[#00A859] flex-shrink-0 mt-0.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                </svg>
                <span>Por favor realiza el pago por el monto exacto antes de continuar con los datos del titular.</span>
            </p>

            <button onclick="goToStep2()" class="w-full bg-[#4153B9] hover:bg-[#3343A0] text-white font-arial-13 font-bold py-3 rounded-xl shadow transition-all duration-200 flex items-center justify-center gap-2">
                <span>Ya pagué</span>
                <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l7-7m7 7H3"></path>
                </svg>
            </button>
        </div>

        <!-- PASO 2: Datos del Titular -->
        <div id="step2View" class="hidden animate-step-transition">
            <div class="flex items-center justify-between mb-3">
                <button onclick="goToStep1()" class="inline-flex items-center gap-1.5 font-arial-11 font-semibold text-slate-600 hover:text-slate-900 bg-slate-100 hover:bg-slate-200 px-3 py-1.5 rounded-xl transition-all">
                    <svg class="w-4 h-4 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path>
                    </svg>
                    <span>Atrás</span>
                </button>
                <span class="bg-green-soft-76 text-[#00A859] border border-[#00A859]/30 font-arial-11 font-bold px-3 py-1 rounded-full">Paso 2 de 3</span>
            </div>

            <div class="mb-3">
                <h2 class="text-2xl font-bold text-[#4153B9] tracking-tight">Datos del Titular</h2>
                <p class="font-arial-13 text-slate-500">Ingresa la información de quien realiza o representa este pago:</p>
            </div>

            <form id="step2Form" onsubmit="handleStep2Submit(event)" class="space-y-3">
                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Nombre y Apellido *</label>
                    <input type="text" id="holderName" required placeholder="Ej: Juan Pérez" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all">
                </div>

                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Cédula de Identidad *</label>
                    <input type="text" id="holderId" required placeholder="Ej: V-12345678" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all">
                </div>

                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Concepto de Pago *</label>
                    <input type="text" id="paymentConcept" required placeholder="Ej: Mensualidad / Inscripción / Evento" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all">
                </div>

                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">¿Atleta inscrito? *</label>
                    <select id="isAthlete" required onchange="toggleAthleteField()" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 bg-white transition-all">
                        <option value="No">No</option>
                        <option value="Sí">Sí</option>
                    </select>
                </div>

                <div id="athleteNameContainer" class="hidden">
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Nombre de o los Atletas *</label>
                    <input type="text" id="athleteName" placeholder="Ej: Carlos Pérez / Maria Pérez" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all">
                </div>

                <button type="submit" class="w-full mt-2 bg-[#4153B9] hover:bg-[#3343A0] text-white font-arial-11 font-bold py-3 rounded-xl shadow transition-all duration-200 flex items-center justify-center gap-2">
                    <span>Continuar</span>
                    <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l7-7m7 7H3"></path>
                    </svg>
                </button>
            </form>
        </div>

        <!-- PASO 3: Detalles del Pago -->
        <div id="step3View" class="hidden animate-step-transition">
            <div class="flex items-center justify-between mb-3">
                <button onclick="goToStep2FromStep3()" class="inline-flex items-center gap-1.5 font-arial-11 font-semibold text-slate-600 hover:text-slate-900 bg-slate-100 hover:bg-slate-200 px-3 py-1.5 rounded-xl transition-all">
                    <svg class="w-4 h-4 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path>
                    </svg>
                    <span>Atrás</span>
                </button>
                <span class="bg-green-soft-76 text-[#00A859] border border-[#00A859]/30 font-arial-11 font-bold px-3 py-1 rounded-full">Paso 3 de 3</span>
            </div>

            <div class="mb-3">
                <h2 class="text-2xl font-bold text-[#4153B9] tracking-tight">¡Hola! Cuéntanos sobre tu pago:</h2>
                <p class="font-arial-13 text-slate-500">Ingresa la referencia y fecha para validar la transacción:</p>
            </div>

            <form id="step3Form" onsubmit="handleStep3Submit(event)" class="space-y-3">
                <!-- Campos específicos para Pago Móvil -->
                <div id="pagoMovilFields" class="hidden space-y-3 bg-green-soft-76 p-3 rounded-2xl border border-[#00A859]/30">
                    <div>
                        <label class="block font-arial-13 font-bold text-slate-700 mb-1">Teléfono Emisor *</label>
                        <input type="tel" id="issuerPhone" placeholder="Ej: 04141234567" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all bg-white">
                    </div>
                    <div>
                        <label class="block font-arial-13 font-bold text-slate-700 mb-1">Banco Emisor *</label>
                        <input type="text" id="issuerBank" placeholder="Ej: Banesco / Mercantil / BDV" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all bg-white">
                    </div>
                </div>

                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Número de Referencia *</label>
                    <input type="text" id="paymentRef" required pattern="[0-9]{4,10}" maxlength="10" placeholder="Ej: 123456" class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all">
                    <p class="font-arial-9 text-slate-500 mt-1">Ingresa los últimos dígitos del comprobante</p>
                </div>

                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Monto Pagado *</label>
                    <input type="text" id="paidAmount" required readonly class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 bg-slate-100 font-arial-13 font-bold text-slate-700 cursor-not-allowed">
                </div>

                <div>
                    <label class="block font-arial-13 font-bold text-slate-700 mb-1">Fecha de Pago *</label>
                    <input type="date" id="paymentDate" required class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 focus:border-[#00A859] focus:ring-1 focus:ring-[#00A859] outline-none font-arial-13 transition-all">
                </div>

                <button type="submit" class="w-full mt-2 bg-[#4153B9] hover:bg-[#3343A0] text-white font-arial-11 font-bold py-3 rounded-xl shadow transition-all duration-200 flex items-center justify-center gap-2">
                    <span>Confirmar pago</span>
                    <svg class="w-4 h-4 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                    </svg>
                </button>
            </form>
        </div>

    </main>

    <!-- Modal Pestaña Flotante: ¿Qué deseas pagar? -->
    <div id="planModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-5 shadow-2xl border border-slate-100 animate-pop">
            <div class="text-center mb-4">
                <div class="w-12 h-12 bg-green-soft-76 text-[#00A859] rounded-full flex items-center justify-center mx-auto mb-2 border border-[#00A859]/30">
                    <svg class="w-6 h-6 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01"></path>
                    </svg>
                </div>
                <h3 class="text-xl font-bold text-[#4153B9]">¿Qué deseas pagar?</h3>
                <p class="font-arial-11 text-slate-500 mt-0.5">Selecciona el plan que deseas reportar para ajustar la tarifa automáticamente:</p>
            </div>

            <div class="space-y-3 mb-5">
                <!-- Opción 1: Plan Familiar Integral -->
                <button onclick="confirmPlanSelection('Plan familiar Integral', 2.00)" class="w-full text-left p-3.5 rounded-2xl border-2 border-slate-200 hover:border-[#00A859] hover:bg-green-soft-76/30 transition-all flex items-center justify-between group">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-xl bg-blue-100 text-[#4153B9] flex items-center justify-center font-bold font-arial-12">
                            1
                        </div>
                        <div>
                            <h4 class="font-bold text-slate-800 font-arial-13 group-hover:text-[#4153B9]">Plan familiar Integral</h4>
                            <p class="font-arial-11 text-slate-500">Tarifa general mensual</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <span class="font-black text-[#00A859] font-arial-18">$ 2.00</span>
                        <span class="block font-arial-8 text-slate-400">USD</span>
                    </div>
                </button>

                <!-- Opción 2: Plan Salud Deportiva -->
                <button onclick="confirmPlanSelection('Plan salud deportiva', 2.30)" class="w-full text-left p-3.5 rounded-2xl border-2 border-slate-200 hover:border-[#00A859] hover:bg-green-soft-76/30 transition-all flex items-center justify-between group">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-xl bg-emerald-100 text-[#00A859] flex items-center justify-center font-bold font-arial-12">
                            2
                        </div>
                        <div>
                            <h4 class="font-bold text-slate-800 font-arial-13 group-hover:text-[#00A859]">Plan salud deportiva</h4>
                            <p class="font-arial-11 text-slate-500">Atención médica y salud integral</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <span class="font-black text-[#00A859] font-arial-18">$ 2.30</span>
                        <span class="block font-arial-8 text-slate-400">USD</span>
                    </div>
                </button>
            </div>

            <button onclick="closePlanModal()" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-600 font-arial-11 font-bold py-2.5 rounded-xl transition-all">
                Cancelar
            </button>
        </div>
    </div>

    <!-- Modal de Resumen -->
    <div id="summaryModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-5 shadow-2xl border border-slate-100 animate-pop">
            <div class="text-center mb-4">
                <div class="w-12 h-12 bg-green-soft-76 text-[#00A859] rounded-full flex items-center justify-center mx-auto mb-2 border border-[#00A859]/30">
                    <svg class="w-6 h-6 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                    </svg>
                </div>
                <h3 class="text-lg font-bold text-slate-800">Resumen del Reporte</h3>
                <p class="font-arial-11 text-slate-500">Verifica que tus datos sean correctos antes de enviar</p>
            </div>

            <div id="summaryContent" class="bg-slate-50 rounded-2xl p-3.5 border border-slate-200 font-arial-12 space-y-2 mb-4 text-slate-700">
                <!-- Se rellena dinámicamente -->
            </div>

            <div class="grid grid-cols-2 gap-2.5">
                <button onclick="closeSummaryModal()" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-700 font-arial-11 font-bold py-2.5 rounded-xl transition-all">
                    Editar información
                </button>
                <button onclick="sendToWhatsAppDirect()" id="btnSubmitWhatsApp" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-arial-11 font-bold py-2.5 rounded-xl shadow transition-all flex items-center justify-center gap-1.5">
                    <svg class="w-4 h-4 fill-current" viewBox="0 0 24 24">
                        <path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.413 3.488 2.245 2.248 3.481 5.236 3.48 8.414-.003 6.557-5.338 11.892-11.893 11.892-1.99-.001-3.951-.5-5.688-1.448l-6.305 1.654zm6.597-3.807c1.676.995 3.276 1.591 5.392 1.592 5.448 0 9.886-4.434 9.889-9.885.002-5.462-4.415-9.89-9.881-9.892-5.452 0-9.887 4.434-9.889 9.884-.001 2.225.651 3.891 1.746 5.634l-.999 3.648 3.742-.981z"></path>
                    </svg>
                    <span>Continuar</span>
                </button>
            </div>
        </div>
    </div>

    <!-- Modal de Éxito / Notificación de Envío Exitoso sin abrir WhatsApp -->
    <div id="successModal" class="fixed inset-0 bg-slate-900/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 shadow-2xl border border-slate-100 text-center animate-pop">
            <div class="w-16 h-16 bg-green-soft-76 text-[#00A859] rounded-full flex items-center justify-center mx-auto mb-3 border-2 border-[#00A859]/40 shadow-sm">
                <svg class="w-8 h-8 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7"></path>
                </svg>
            </div>
            <h3 class="text-xl font-extrabold text-[#4153B9] mb-2">¡Reporte Enviado con Éxito!</h3>
            <p class="font-arial-13 text-slate-700 font-medium leading-relaxed mb-5 bg-green-soft-76/60 p-3.5 rounded-2xl border border-[#00A859]/30">
                ¡Muchas Gracias por su pago! Espere la confirmación a su número de WhatsApp registrado.
            </p>
            <button onclick="resetPortal()" class="w-full bg-[#4153B9] hover:bg-[#3343A0] text-white font-arial-12 font-bold py-3 rounded-xl shadow transition-all">
                Realizar otro pago
            </button>
        </div>
    </div>

    <!-- JavaScript Application Logic -->
    <script>
        // Configuración Global de Datos
        const TARGET_WHATSAPP_NUMBER = "584168101047";
        const FALLBACK_BCV_RATE = 744.23;

        let currentBcvRate = FALLBACK_BCV_RATE;
        let selectedMethod = "";
        let selectedPlan = "Plan familiar Integral";
        let usdBaseAmount = 2.00;

        let paymentData = {
            planName: "Plan familiar Integral",
            methodName: "",
            holderName: "",
            holderId: "",
            paymentConcept: "",
            isAthlete: "No",
            athleteName: "",
            issuerPhone: "",
            issuerBank: "",
            paymentRef: "",
            paidAmount: "",
            paymentDate: ""
        };

        const bankAccounts = {
            pago_movil: {
                title: "Pago Móvil",
                banco: "0169 - R4",
                id: "J-315941023",
                phone: "0412-6266257",
                copyText: "Banco: 0169 - R4\nRIF: J-315941023\nTeléfono: 0412-6266257"
            },
            transferencia: {
                title: "Transferencia Bancaria",
                titular: "Joel González",
                banco: "0169 - R4",
                id: "J-315941023",
                cuenta: "01690001031001439153",
                copyText: "Titular: Joel González\nBanco: 0169 - R4\nRIF: J-315941023\nCuenta: 01690001031001439153"
            },
            cash_usd: {
                title: "Cash USD",
                tipo: "Cuenta USD",
                cuenta: "01020731220000311922",
                id: "V17452788",
                titular: "Joel Alberto Gonzalez Benavente",
                copyText: "Titular: Joel Alberto Gonzalez Benavente\nCI: V17452788\nTipo: Cuenta USD\nCuenta: 01020731220000311922"
            }
        };

        // Tasa Oficial BCV - Consulta Automática Sincronizada al Abrir
        async function fetchBcvRate() {
            const bcvIndicator = document.getElementById("bcvIndicator");
            if (bcvIndicator) bcvIndicator.innerText = "Actualizando...";

            const apiEndpoints = [
                async () => {
                    const res = await fetch("https://ve.dolarapi.com/v1/dolares/oficial", { cache: "no-cache" });
                    const data = await res.json();
                    return data.promedio || data.monto;
                },
                async () => {
                    const res = await fetch("https://pydolarvenezuela-api.vercel.app/api/v1/dollar?page=bcv", { cache: "no-cache" });
                    const data = await res.json();
                    return data.moneda?.bcv || data.bcv;
                },
                async () => {
                    const res = await fetch("https://bcv-api.rafnixg.dev/v1/exchange-rates/latest/USD", { cache: "no-cache" });
                    const data = await res.json();
                    return data.rate;
                },
                async () => {
                    const res = await fetch("https://api.allorigins.win/raw?url=https://www.bcv.org.ve/", { cache: "no-cache" });
                    const htmlText = await res.text();
                    const match = htmlText.match(/id="dolar"[^>]*>[\s\S]*?<strong>\s*([0-9.,]+)\s*<\/strong>/i);
                    if (match && match[1]) {
                        return match[1].replace(',', '.');
                    }
                    throw new Error("No match on BCV page");
                }
            ];

            for (const endpoint of apiEndpoints) {
                try {
                    const controller = new AbortController();
                    const timeoutId = setTimeout(() => controller.abort(), 3500);
                    const val = await Promise.race([
                        endpoint(),
                        new Promise((_, reject) => setTimeout(() => reject(new Error("Timeout")), 3500))
                    ]);
                    clearTimeout(timeoutId);

                    const rateVal = parseFloat(String(val).replace(/,/g, '.'));
                    if (!isNaN(rateVal) && rateVal > 0) {
                        currentBcvRate = rateVal;
                        updateBcvDisplay(currentBcvRate);
                        localStorage.setItem("bcv_saved_rate", currentBcvRate);
                        return;
                    }
                } catch (e) {
                    console.log("Error al consultar endpoint BCV:", e);
                }
            }

            const saved = localStorage.getItem("bcv_saved_rate");
            if (saved) {
                currentBcvRate = parseFloat(saved);
            } else {
                currentBcvRate = FALLBACK_BCV_RATE;
            }
            updateBcvDisplay(currentBcvRate);
        }

        function updateBcvDisplay(rate) {
            const bcvIndicator = document.getElementById("bcvIndicator");
            if (bcvIndicator) {
                bcvIndicator.innerText = `${rate.toFixed(2)} Bs/USD`;
            }
            recalculateAmounts();
        }

        function recalculateAmounts() {
            const bsAmountText = document.getElementById("bsAmountText");
            const usdAmountDisplay = document.getElementById("usdAmountDisplay");
            const selectedPlanBadge = document.getElementById("selectedPlanBadge");

            if (usdAmountDisplay) {
                usdAmountDisplay.innerText = `$ ${usdBaseAmount.toFixed(2)} USD`;
            }
            if (selectedPlanBadge) {
                selectedPlanBadge.innerText = selectedPlan;
            }

            if (bsAmountText) {
                const bsTotal = (usdBaseAmount * currentBcvRate).toFixed(2);
                bsAmountText.innerText = `${bsTotal} Bs.`;
            }
        }

        // Auto actualización interna cada 15 minutos exactos + al cambiar de ventana
        setInterval(fetchBcvRate, 15 * 60 * 1000);
        document.addEventListener("visibilitychange", () => {
            if (!document.hidden) fetchBcvRate();
        });

        // Selección de Método y Flujo con Pestaña Flotante de Planes
        function selectPaymentMethod(methodKey) {
            selectedMethod = methodKey;
            paymentData.methodName = bankAccounts[methodKey].title;
            openPlanModal();
        }

        function openPlanModal() {
            document.getElementById("planModal").classList.remove("hidden");
        }

        function closePlanModal() {
            document.getElementById("planModal").classList.add("hidden");
        }

        function confirmPlanSelection(planName, usdPrice) {
            selectedPlan = planName;
            usdBaseAmount = usdPrice;
            paymentData.planName = planName;

            closePlanModal();

            // Ocultar cabecera y selección inicial
            document.getElementById("mainHeader").classList.add("hidden");
            document.getElementById("mainTitleView").classList.add("hidden");
            document.getElementById("methodSelectorView").classList.add("hidden");

            // Mostrar Stepper y Paso 1
            document.getElementById("stepperNav").classList.remove("hidden");
            document.getElementById("step1View").classList.remove("hidden");
            updateStepperUI(1);

            document.getElementById("methodTitleStep1").innerText = bankAccounts[selectedMethod].title;

            // Renderizar datos bancarios y recalcular según la tasa del día
            renderBankCard(selectedMethod);
            recalculateAmounts();
        }

        function renderBankCard(methodKey) {
            const container = document.getElementById("bankDetailsCard");
            const data = bankAccounts[methodKey];

            let fields = [];
            if (methodKey === "pago_movil") {
                fields = [
                    { label: "Banco Emisor", val: data.banco },
                    { label: "RIF / Cédula", val: data.id },
                    { label: "Teléfono", val: data.phone }
                ];
            } else if (methodKey === "transferencia") {
                fields = [
                    { label: "Titular", val: data.titular },
                    { label: "Banco", val: data.banco },
                    { label: "RIF", val: data.id },
                    { label: "Nº de Cuenta", val: data.cuenta }
                ];
            } else {
                fields = [
                    { label: "Titular", val: data.titular },
                    { label: "Identificación", val: data.id },
                    { label: "Tipo", val: data.tipo },
                    { label: "Nº de Cuenta", val: data.cuenta }
                ];
            }

            let htmlContent = "";
            fields.forEach((f, idx) => {
                const isLast = idx === fields.length - 1;
                htmlContent += `
                    <div class="flex justify-between items-center py-0.5 ${isLast ? '' : 'border-b border-slate-200'}">
                        <div class="pr-2">
                            <span class="font-arial-9 text-slate-500 uppercase block">${f.label}:</span>
                            <span class="font-arial-13 font-bold text-slate-800 break-all">${f.val}</span>
                        </div>
                        <button onclick="copySingleField('${f.val}', this)" title="Copiar ${f.label}" class="flex-shrink-0 p-1 text-slate-500 hover:text-[#00A859] hover:bg-green-soft-76 rounded-lg transition-all flex items-center gap-1 border border-transparent hover:border-[#00A859]/30">
                            <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path>
                            </svg>
                            <span class="font-arial-8 font-semibold hidden sm:inline">Copiar</span>
                        </button>
                    </div>
                `;
            });

            container.innerHTML = htmlContent + `
                <button onclick="copyToClipboard('${data.copyText}', this)" class="w-full mt-2 bg-green-soft-76 hover:bg-[#00A859]/20 text-[#00A859] border border-[#00A859]/40 font-arial-11 font-bold py-2 rounded-xl transition-all flex items-center justify-center gap-1.5 shadow-sm">
                    <svg class="w-4 h-4 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path>
                    </svg>
                    <span>Copiar Todos los Datos de ${data.title}</span>
                </button>
            `;
        }

        function copySingleField(text, btn) {
            const el = document.createElement('textarea');
            el.value = text;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);

            const orig = btn.innerHTML;
            btn.innerHTML = `<svg class="w-4 h-4 text-[#00A859]" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7"></path></svg><span class="font-arial-8 font-bold text-[#00A859]">¡Listo!</span>`;
            setTimeout(() => { btn.innerHTML = orig; }, 1800);
        }

        function copyToClipboard(text, btn) {
            const el = document.createElement('textarea');
            el.value = text;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);

            const originalHTML = btn.innerHTML;
            btn.innerHTML = `<span class="text-[#00A859] font-bold">¡Todos los datos copiados! ✓</span>`;
            setTimeout(() => {
                btn.innerHTML = originalHTML;
            }, 2000);
        }

        function updateStepperUI(stepNum) {
            [1, 2, 3].forEach(n => {
                const dot = document.getElementById(`stepDot${n}`);
                if (n === stepNum) {
                    dot.className = "w-7 h-7 rounded-full bg-green-soft-76 text-[#00A859] border border-[#00A859] flex items-center justify-center font-arial-12 font-bold transition-all shadow-sm";
                } else if (n < stepNum) {
                    dot.className = "w-7 h-7 rounded-full bg-[#00A859] text-white flex items-center justify-center font-arial-12 font-bold transition-all";
                } else {
                    dot.className = "w-7 h-7 rounded-full bg-slate-100 text-slate-400 flex items-center justify-center font-arial-12 font-bold transition-all";
                }
            });
        }

        function goBackToMain() {
            document.getElementById("step1View").classList.add("hidden");
            document.getElementById("stepperNav").classList.add("hidden");

            document.getElementById("mainHeader").classList.remove("hidden");
            document.getElementById("mainTitleView").classList.remove("hidden");
            document.getElementById("methodSelectorView").classList.remove("hidden");
        }

        function goToStep2() {
            document.getElementById("step1View").classList.add("hidden");
            document.getElementById("step2View").classList.remove("hidden");
            updateStepperUI(2);
        }

        function goToStep1() {
            document.getElementById("step2View").classList.add("hidden");
            document.getElementById("step1View").classList.remove("hidden");
            updateStepperUI(1);
        }

        function toggleAthleteField() {
            const select = document.getElementById("isAthlete");
            const container = document.getElementById("athleteNameContainer");
            const input = document.getElementById("athleteName");

            if (select.value === "Sí") {
                container.classList.remove("hidden");
                input.required = true;
            } else {
                container.classList.add("hidden");
                input.required = false;
                input.value = "";
            }
        }

        function handleStep2Submit(e) {
            e.preventDefault();
            paymentData.holderName = document.getElementById("holderName").value;
            paymentData.holderId = document.getElementById("holderId").value;
            paymentData.paymentConcept = document.getElementById("paymentConcept").value;
            paymentData.isAthlete = document.getElementById("isAthlete").value;
            paymentData.athleteName = document.getElementById("athleteName").value;

            // Preparar Paso 3
            document.getElementById("step2View").classList.add("hidden");
            document.getElementById("step3View").classList.remove("hidden");
            updateStepperUI(3);

            // Si es Pago Móvil mostrar campos condicionales
            const pmFields = document.getElementById("pagoMovilFields");
            const issuerPhone = document.getElementById("issuerPhone");
            const issuerBank = document.getElementById("issuerBank");

            if (selectedMethod === "pago_movil") {
                pmFields.classList.remove("hidden");
                issuerPhone.required = true;
                issuerBank.required = true;
            } else {
                pmFields.classList.add("hidden");
                issuerPhone.required = false;
                issuerBank.required = false;
            }

            // Prellenar monto pagado con cálculo oficial
            const paidAmountInput = document.getElementById("paidAmount");
            if (selectedMethod === "cash_usd") {
                paidAmountInput.value = `$ ${usdBaseAmount.toFixed(2)} USD`;
            } else {
                const bsTotal = (usdBaseAmount * currentBcvRate).toFixed(2);
                paidAmountInput.value = `${bsTotal} Bs. (Eq. $${usdBaseAmount.toFixed(2)} USD)`;
            }

            const today = new Date().toISOString().split('T')[0];
            document.getElementById("paymentDate").value = today;
        }

        function goToStep2FromStep3() {
            document.getElementById("step3View").classList.add("hidden");
            document.getElementById("step2View").classList.remove("hidden");
            updateStepperUI(2);
        }

        function handleStep3Submit(e) {
            e.preventDefault();

            if (selectedMethod === "pago_movil") {
                paymentData.issuerPhone = document.getElementById("issuerPhone").value;
                paymentData.issuerBank = document.getElementById("issuerBank").value;
            }
            paymentData.paymentRef = document.getElementById("paymentRef").value;
            paymentData.paidAmount = document.getElementById("paidAmount").value;
            paymentData.paymentDate = document.getElementById("paymentDate").value;

            // Renderizar Resumen de Datos
            let summaryHTML = `
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Plan:</span>
                    <span class="font-bold text-[#00A859]">${paymentData.planName}</span>
                </div>
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Método:</span>
                    <span class="font-bold text-slate-800">${paymentData.methodName}</span>
                </div>
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Titular:</span>
                    <span>${paymentData.holderName} (${paymentData.holderId})</span>
                </div>
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Concepto:</span>
                    <span>${paymentData.paymentConcept}</span>
                </div>
                ${paymentData.isAthlete === "Sí" ? `
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Atleta(s):</span>
                    <span>${paymentData.athleteName}</span>
                </div>` : ''}
                ${selectedMethod === "pago_movil" ? `
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Teléfono Emisor:</span>
                    <span>${paymentData.issuerPhone}</span>
                </div>
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Banco Emisor:</span>
                    <span>${paymentData.issuerBank}</span>
                </div>` : ''}
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Referencia:</span>
                    <span class="font-bold text-slate-800">${paymentData.paymentRef}</span>
                </div>
                <div class="flex justify-between border-b pb-1 border-slate-200">
                    <span class="font-bold text-slate-500">Monto:</span>
                    <span class="font-bold text-[#00A859]">${paymentData.paidAmount}</span>
                </div>
                <div class="flex justify-between">
                    <span class="font-bold text-slate-500">Fecha:</span>
                    <span>${paymentData.paymentDate}</span>
                </div>
            `;

            document.getElementById("summaryContent").innerHTML = summaryHTML;
            document.getElementById("summaryModal").classList.remove("hidden");
        }

        function closeSummaryModal() {
            document.getElementById("summaryModal").classList.add("hidden");
        }

        function sendToWhatsAppDirect() {
            let message = `*REPORTE DE PAGO*\n`;
            message += `-------------------------\n`;
            message += `*Plan Seleccionado:* ${paymentData.planName}\n`;
            message += `*Método:* ${paymentData.methodName}\n`;
            message += `*Titular:* ${paymentData.holderName}\n`;
            message += `*Cédula:* ${paymentData.holderId}\n`;
            message += `*Concepto:* ${paymentData.paymentConcept}\n`;

            if (paymentData.isAthlete === "Sí") {
                message += `*Atleta(s):* ${paymentData.athleteName}\n`;
            }

            if (selectedMethod === "pago_movil") {
                message += `*Teléfono Emisor:* ${paymentData.issuerPhone}\n`;
                message += `*Banco Emisor:* ${paymentData.issuerBank}\n`;
            }

            message += `*Referencia:* ${paymentData.paymentRef}\n`;
            message += `*Monto Pagado:* ${paymentData.paidAmount}\n`;
            message += `*Fecha:* ${paymentData.paymentDate}\n`;
            message += `-------------------------\n`;
            message += `_Enviado desde el portal de Gestión de Pagos_`;

            const btn = document.getElementById("btnSubmitWhatsApp");
            btn.innerHTML = `<span class="animate-pulse">Enviando...</span>`;
            btn.disabled = true;

            const encodedMsg = encodeURIComponent(message);
            const targetNumber = "584168101047";
            
            // Redirección directa al enlace oficial de WhatsApp con la información y número especificado
            const whatsappUrl = `https://wa.me/${targetNumber}?text=${encodedMsg}`;
            
            window.open(whatsappUrl, '_blank');

            setTimeout(() => {
                btn.innerHTML = `<span>Continuar</span>`;
                btn.disabled = false;
                document.getElementById("summaryModal").classList.add("hidden");
                document.getElementById("successModal").classList.remove("hidden");
            }, 600);
        }

        function finalizeSubmission() {
            const btn = document.getElementById("btnSubmitWhatsApp");
            btn.innerHTML = `<span>Continuar</span>`;
            btn.disabled = false;

            document.getElementById("summaryModal").classList.add("hidden");
            document.getElementById("successModal").classList.remove("hidden");
        }

        function resetPortal() {
            document.getElementById("successModal").classList.add("hidden");
            location.reload();
        }

        window.onload = function() {
            fetchBcvRate();
        };
    </script>
</body>
</html>
