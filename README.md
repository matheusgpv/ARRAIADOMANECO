<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arraiá do Maneco - Reserva do Iguaçu/PR</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&family=Baloo+2:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Adicionar Leaflet CSS para o mapa -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #FFF9E5;
        }
        .title-font {
            font-family: 'Baloo 2', cursive;
        }
        .banner {
            background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), #FF7A00;
            /* Aqui você pode adicionar uma imagem de fundo para o banner */
            /* background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('URL_DA_SUA_IMAGEM'); */
            background-size: cover;
            background-position: center;
        }
        .countdown-item {
            background-color: #FF6B35;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .whatsapp-btn {
            background-color: #25D366;
            transition: all 0.3s ease;
        }
        .whatsapp-btn:hover {
            background-color: #128C7E;
        }
        .ticket-card {
            border: 2px dashed #FF6B35;
        }
        .float-whatsapp {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 100;
        }
        .process-step {
            position: relative;
            padding-left: 40px;
        }
        .process-step:before {
            content: attr(data-step);
            position: absolute;
            left: 0;
            top: 0;
            width: 30px;
            height: 30px;
            background-color: #FF6B35;
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }
        .alert-box {
            border-left: 4px solid #FF6B35;
        }
        .pix-box {
            background-color: #f0f9ff;
            border: 1px solid #bae6fd;
            border-radius: 8px;
        }
        /* Classe para espaços reservados de imagem */
        .image-placeholder {
            background-color: #f3f4f6;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #6b7280;
            font-size: 14px;
            border-radius: 8px;
            overflow: hidden;
            position: relative;
        }
        .image-placeholder img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .caneca-highlight {
            background: linear-gradient(135deg, #ffedd5 0%, #fed7aa 100%);
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        .ribbon {
            position: absolute;
            top: 0;
            right: 0;
            width: 150px;
            height: 150px;
            overflow: hidden;
        }
        .ribbon-content {
            position: absolute;
            display: block;
            width: 225px;
            padding: 15px 0;
            background-color: #FF6B35;
            box-shadow: 0 5px 10px rgba(0,0,0,.1);
            color: #fff;
            font-size: 16px;
            text-align: center;
            transform: rotate(45deg);
            right: -25%;
            top: 30px;
        }
        .cupom-input {
            position: relative;
        }
        .cupom-input button {
            position: absolute;
            right: 0;
            top: 0;
            height: 100%;
            border-top-left-radius: 0;
            border-bottom-left-radius: 0;
        }
        .caneca-only {
            background: linear-gradient(135deg, #fff7ed 0%, #ffedd5 100%);
            border: 2px solid #FB923C;
        }
        .limited-badge {
            background-color: #ef4444;
            color: white;
            font-size: 12px;
            font-weight: bold;
            padding: 4px 8px;
            border-radius: 4px;
            display: inline-block;
            margin-left: 8px;
        }
        .price-breakdown {
            background-color: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 8px;
            padding: 12px;
            margin-top: 12px;
        }
        .compact-countdown {
            padding: 4px 0;
        }
        .compact-countdown .countdown-item {
            padding: 8px;
        }
        .discount-price {
            text-decoration: line-through;
            color: #94a3b8;
            font-size: 0.9em;
        }
        .new-price {
            color: #16a34a;
            font-weight: bold;
        }
        .price-summary {
            background-color: #f0fdf4;
            border: 1px solid #86efac;
            border-radius: 8px;
            padding: 12px;
            margin-top: 12px;
        }
        .calculator-section {
            background-color: #fff8f1;
            border: 2px solid #FFBA7A;
            border-radius: 12px;
            padding: 20px;
            margin: 30px auto;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
        }
        .calculator-result {
            background-color: #f0fdf4;
            border: 1px solid #86efac;
            border-radius: 8px;
            padding: 16px;
            margin-top: 16px;
        }
        .calculator-result.with-discount {
            border-color: #86efac;
            background-color: #f0fdf4;
        }
        .ticket-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 12px;
        }
        .ticket-mini {
            border: 1px solid #FF6B35;
            border-radius: 8px;
            padding: 12px;
            background-color: white;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        .ticket-mini:hover, .ticket-mini.selected {
            border-color: #FF6B35;
            background-color: #fff0e6;
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .ticket-mini.selected {
            border-width: 2px;
        }
        .ticket-mini .price {
            font-weight: bold;
            color: #FF6B35;
        }
        .tab-button {
            padding: 10px 20px;
            background-color: #f8f9fa;
            border: 1px solid #e9ecef;
            border-bottom: none;
            border-radius: 8px 8px 0 0;
            cursor: pointer;
        }
        .tab-button.active {
            background-color: white;
            font-weight: bold;
            color: #FF6B35;
        }
        .tab-content {
            display: none;
            padding: 20px;
            background-color: white;
            border-radius: 0 8px 8px 8px;
            border: 1px solid #e9ecef;
        }
        .tab-content.active {
            display: block;
        }
        .location-badge {
            background-color: #FF6B35;
            color: white;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 14px;
            font-weight: 600;
            display: inline-block;
            margin-top: 8px;
        }
        /* Estilo para o mapa */
        #map {
            height: 300px;
            width: 100%;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            z-index: 1;
        }
        .map-container {
            position: relative;
            margin-top: 20px;
        }
        .map-overlay {
            position: absolute;
            bottom: 10px;
            left: 10px;
            background-color: rgba(255, 255, 255, 0.9);
            padding: 8px 12px;
            border-radius: 4px;
            font-size: 12px;
            z-index: 2;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
        }
        .map-directions {
            display: inline-flex;
            align-items: center;
            background-color: #FF6B35;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: 600;
            margin-top: 10px;
            text-decoration: none;
            transition: all 0.3s ease;
        }
        .map-directions:hover {
            background-color: #e55a24;
            transform: translateY(-2px);
        }
    </style>
</head>
<body>
    <!-- Banner principal com espaço para imagem -->
    <header class="banner w-full h-64 flex items-center justify-center text-center px-4">
        <!-- Comentário: Substitua o estilo inline do banner na tag style acima para adicionar uma imagem de fundo -->
        <div>
            <h1 class="title-font text-4xl md:text-6xl font-bold text-white mb-2">Arraiá do Maneco</h1>
            <p class="text-xl text-white">O MAIOR ARRAIÁ DA REGIÃO 042</p>
            <div class="location-badge mt-3">
                <svg class="w-4 h-4 inline-block mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path>
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path>
                </svg>
                Reserva do Iguaçu/PR
            </div>
            <div class="mt-6">
                <a href="#ingressos" class="bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 px-6 rounded-full text-lg transition duration-300 inline-block">Garantir ingresso</a>
            </div>
        </div>
    </header>

    <!-- Contagem regressiva (versão compacta) -->
    <section class="py-4 px-4 compact-countdown">
        <div class="max-w-4xl mx-auto">
            <h2 class="title-font text-2xl text-center font-bold text-orange-600 mb-3">Contagem Regressiva</h2>
            <div class="grid grid-cols-4 gap-2 max-w-2xl mx-auto">
                <div class="countdown-item p-2 text-center text-white">
                    <span id="days" class="block text-2xl font-bold">00</span>
                    <span class="text-xs">Dias</span>
                </div>
                <div class="countdown-item p-2 text-center text-white">
                    <span id="hours" class="block text-2xl font-bold">00</span>
                    <span class="text-xs">Horas</span>
                </div>
                <div class="countdown-item p-2 text-center text-white">
                    <span id="minutes" class="block text-2xl font-bold">00</span>
                    <span class="text-xs">Minutos</span>
                </div>
                <div class="countdown-item p-2 text-center text-white">
                    <span id="seconds" class="block text-2xl font-bold">00</span>
                    <span class="text-xs">Segundos</span>
                </div>
            </div>
        </div>
    </section>

    <!-- Informações do evento com mapa do local -->
    <section class="py-8 px-4 bg-orange-50">
        <div class="max-w-6xl mx-auto">
            <h2 class="title-font text-3xl text-center font-bold text-orange-600 mb-6">Informações do Evento</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <div class="flex items-start mb-4">
                        <div class="bg-orange-500 p-3 rounded-full mr-4">
                            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
                            </svg>
                        </div>
                        <div>
                            <h3 class="text-xl font-bold mb-2">Data e Horário</h3>
                            <p class="text-gray-700">Sábado, 12 de Julho de 2025</p>
                            <p class="text-gray-700">A partir das 19h</p>
                        </div>
                    </div>
                    <div class="flex items-start">
                        <div class="bg-orange-500 p-3 rounded-full mr-4">
                            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path>
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path>
                            </svg>
                        </div>
                        <div>
                            <h3 class="text-xl font-bold mb-2">Local</h3>
                            <p class="text-gray-700">Parque Hilário Graf Júnior</p>
                            <p class="text-gray-700"><strong>Reserva do Iguaçu/PR</strong></p>
                            <p class="text-gray-700">Região 042</p>
                        </div>
                    </div>
                    
                    <!-- Mapa do local do evento -->
                    <div class="map-container mt-4">
                        <div id="map"></div>
                        <div class="map-overlay">
                            <strong>Parque Hilário Graf Júnior</strong><br>
                            Reserva do Iguaçu/PR
                        </div>
                    </div>
                    <div class="text-center mt-3">
                        <a href="https://www.google.com/maps/dir/?api=1&destination=-25.8383,-52.0172" target="_blank" class="map-directions inline-block">
                            <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7"></path>
                            </svg>
                            Como chegar
                        </a>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <div class="flex items-start mb-4">
                        <div class="bg-orange-500 p-3 rounded-full mr-4">
                            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z"></path>
                            </svg>
                        </div>
                        <div>
                            <h3 class="text-xl font-bold mb-2">Formas de Pagamento</h3>
                            <p class="text-gray-700">Aceitamos PIX</p>
                            <div class="pix-box p-3 mt-2">
                                <p class="font-bold text-blue-800">Chave PIX: arraiadomaneco@gmail.com</p>
                            </div>
                            <p class="text-gray-700 mt-2 italic">Faça o pagamento antes de enviar seus dados</p>
                        </div>
                    </div>
                    
                    <!-- Espaço para QR Code do PIX -->
                    <div class="image-placeholder h-48 mt-4">
                        <!-- Substitua este comentário pela tag img quando tiver a URL da imagem -->
                        <!-- <img src="URL_DO_QR_CODE_PIX" alt="QR Code PIX"> -->
                        <span>QR Code PIX</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Destaque da Caneca -->
    <section class="py-8 px-4">
        <div class="max-w-4xl mx-auto">
            <div class="caneca-highlight p-6 relative overflow-hidden">
                <div class="ribbon">
                    <span class="ribbon-content">EXCLUSIVO</span>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 items-center">
                    <div>
                        <h2 class="title-font text-3xl font-bold text-orange-800 mb-4">Caneca Exclusiva!</h2>
                        <p class="text-orange-900 mb-4">Adquira sua caneca exclusiva do Arraiá do Maneco! Uma lembrança especial que você vai usar o ano todo.</p>
                        <ul class="space-y-2 mb-4">
                            <li class="flex items-center">
                                <svg class="w-5 h-5 mr-2 text-orange-600" fill="currentColor" viewBox="0 0 20 20">
                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path>
                                </svg>
                                <span>Material resistente</span>
                            </li>
                            <li class="flex items-center">
                                <svg class="w-5 h-5 mr-2 text-orange-600" fill="currentColor" viewBox="0 0 20 20">
                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path>
                                </svg>
                                <span>Design exclusivo</span>
                            </li>
                            <li class="flex items-center">
                                <svg class="w-5 h-5 mr-2 text-orange-600" fill="currentColor" viewBox="0 0 20 20">
                                    <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path>
                                </svg>
                                <span>Edição limitada</span>
                            </li>
                        </ul>
                        <div class="flex flex-wrap gap-3">
                            <a href="#calculadora" class="bg-orange-600 hover:bg-orange-700 text-white font-bold py-2 px-6 rounded-lg inline-block transition duration-300" data-ticket="Ingresso + Caneca">Ingresso + caneca</a>
                            <a href="#calculadora" class="bg-orange-500 hover:bg-orange-600 text-white font-bold py-2 px-6 rounded-lg inline-block transition duration-300" data-ticket="Apenas Caneca">Apenas caneca</a>
                        </div>
                    </div>
                    <div class="image-placeholder h-64">
                        <!-- <img src="URL_DA_IMAGEM_CANECA" alt="Caneca Exclusiva do Arraiá do Maneco"> -->
                        <span>Imagem da Caneca Exclusiva</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Ingressos (versão compacta) -->
    <section id="ingressos" class="py-8 px-4">
        <div class="max-w-6xl mx-auto">
            <h2 class="title-font text-3xl text-center font-bold text-orange-600 mb-6">Ingressos</h2>
            
            <div class="ticket-grid max-w-4xl mx-auto">
                <div class="ticket-mini" data-ticket="Ingresso Avulso">
                    <h3 class="font-bold text-orange-600">Ingresso Avulso</h3>
                    <p class="price mt-2">R$ 40,00</p>
                    <button class="mt-2 w-full bg-orange-100 hover:bg-orange-200 text-orange-800 font-bold py-1 px-2 rounded text-sm">Selecionar</button>
                </div>
                
                <div class="ticket-mini" data-ticket="Ingresso + Caneca">
                    <div class="bg-orange-500 text-white text-center py-1 px-2 rounded-full text-xs font-bold mb-2">POPULAR</div>
                    <h3 class="font-bold text-orange-600">Ingresso + Caneca</h3>
                    <p class="price mt-2">R$ 50,00</p>
                    <button class="mt-2 w-full bg-orange-100 hover:bg-orange-200 text-orange-800 font-bold py-1 px-2 rounded text-sm">Selecionar</button>
                </div>
                
                <div class="ticket-mini" data-ticket="Bistrô">
                    <h3 class="font-bold text-orange-600">Bistrô</h3>
                    <p class="price mt-2">R$ 50,00</p>
                    <button class="mt-2 w-full bg-orange-100 hover:bg-orange-200 text-orange-800 font-bold py-1 px-2 rounded text-sm">Selecionar</button>
                </div>
                
                <div class="ticket-mini" data-ticket="Apenas Caneca">
                    <h3 class="font-bold text-orange-600">Apenas Caneca</h3>
                    <p class="price mt-2">R$ 10,00</p>
                    <button class="mt-2 w-full bg-orange-100 hover:bg-orange-200 text-orange-800 font-bold py-1 px-2 rounded text-sm">Selecionar</button>
                </div>
                
                <div class="ticket-mini" data-ticket="Camarote">
                    <div class="bg-red-500 text-white text-center py-1 px-2 rounded-full text-xs font-bold mb-2">LIMITADO</div>
                    <h3 class="font-bold text-orange-600">Camarote</h3>
                    <p class="price mt-2">R$ 800,00</p>
                    <button class="mt-2 w-full bg-orange-100 hover:bg-orange-200 text-orange-800 font-bold py-1 px-2 rounded text-sm">Selecionar</button>
                </div>
            </div>
            
            <div class="mt-6 text-center">
                <a href="#calculadora" class="bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-2 px-6 rounded-full text-lg transition duration-300 inline-block">
                    Calcular valor e garantir ingresso
                </a>
            </div>
        </div>
    </section>

    <!-- Calculadora de Preço com Envio de Comprovante Integrado -->
    <section id="calculadora" class="py-8 px-4">
        <div class="max-w-4xl mx-auto calculator-section">
            <h2 class="title-font text-2xl text-center font-bold text-orange-600 mb-4">Garanta seu ingresso</h2>
            
            <div class="flex mb-4 border-b border-gray-200">
                <button class="tab-button active" data-tab="tab-calcular">1. Calcular valor</button>
                <button class="tab-button" data-tab="tab-comprovante">2. Enviar comprovante</button>
            </div>
            
            <!-- Tab 1: Calculadora -->
            <div id="tab-calcular" class="tab-content active">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div>
                        <div class="mb-4">
                            <label for="calc-ingresso" class="block text-gray-700 font-bold mb-2">Tipo de Ingresso</label>
                            <select id="calc-ingresso" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                                <option value="">Selecione o tipo de ingresso</option>
                                <option value="Ingresso Avulso" data-price="40">Ingresso Avulso - R$ 40,00</option>
                                <option value="Ingresso + Caneca" data-price="50">Ingresso + Caneca - R$ 50,00</option>
                                <option value="Bistrô" data-price="50">Bistrô - R$ 50,00</option>
                                <option value="Apenas Caneca" data-price="10">Apenas Caneca - R$ 10,00</option>
                                <option value="Camarote" data-price="800">Camarote - R$ 800,00</option>
                            </select>
                        </div>
                        
                        <div class="mb-4">
                            <label for="calc-quantidade" class="block text-gray-700 font-bold mb-2">Quantidade</label>
                            <input type="number" id="calc-quantidade" min="1" value="1" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                        </div>
                        
                        <div class="mb-4">
                            <label for="calc-cupom" class="block text-gray-700 font-bold mb-2">Cupom de Desconto</label>
                            <div class="cupom-input">
                                <input type="text" id="calc-cupom" placeholder="Digite seu cupom" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                                <button type="button" id="calc-aplicarCupom" class="bg-orange-500 hover:bg-orange-600 text-white font-bold py-2 px-4 rounded-lg">
                                    Aplicar
                                </button>
                            </div>
                            <p id="calc-cupomMessage" class="text-sm mt-1 hidden"></p>
                        </div>
                    </div>
                    
                    <div>
                        <div id="calc-result" class="calculator-result hidden">
                            <h3 class="font-bold text-lg mb-3">Resumo do Pedido</h3>
                            <div class="flex justify-between mb-2">
                                <span>Ingresso:</span>
                                <span id="calc-ingressoTipo">-</span>
                            </div>
                            <div class="flex justify-between mb-2">
                                <span>Quantidade:</span>
                                <span id="calc-ingressoQtd">-</span>
                            </div>
                            <div class="flex justify-between mb-2">
                                <span>Valor unitário:</span>
                                <span id="calc-valorUnitario">-</span>
                            </div>
                            <div class="flex justify-between mb-2">
                                <span>Valor total:</span>
                                <span id="calc-valorTotal">-</span>
                            </div>
                            <div id="calc-discountRow" class="flex justify-between mb-2 hidden">
                                <span>Desconto (<span id="calc-discountPercent">0</span>%):</span>
                                <span id="calc-discountAmount">- R$ 0,00</span>
                            </div>
                            <div class="flex justify-between font-bold text-lg mt-2 pt-2 border-t border-gray-200">
                                <span>Valor final:</span>
                                <span id="calc-valorFinal" class="text-green-600">-</span>
                            </div>
                            
                            <div class="mt-4 pt-4 border-t border-gray-200">
                                <p class="text-gray-700 mb-2">Faça o pagamento via PIX para:</p>
                                <p class="font-bold text-blue-800 bg-blue-50 p-2 rounded">arraiadomaneco@gmail.com</p>
                            </div>
                            
                            <div class="mt-4">
                                <button id="btn-next-tab" class="whatsapp-btn text-white font-bold py-2 px-4 rounded-lg w-full flex items-center justify-center">
                                    <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                                    </svg>
                                    Já paguei, enviar comprovante
                                </button>
                            </div>
                        </div>
                        
                        <div id="calc-empty" class="h-full flex items-center justify-center">
                            <p class="text-gray-500 text-center">Selecione um ingresso para ver o resumo do pedido</p>
                        </div>
                    </div>
                </div>
                
                <div class="mt-8 bg-yellow-50 p-4 rounded-lg border-l-4 border-yellow-500">
                    <div class="flex">
                        <div class="flex-shrink-0">
                            <svg class="h-5 w-5 text-yellow-500" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2h-1V9a1 1 0 00-1-1z" clip-rule="evenodd"/>
                            </svg>
                        </div>
                        <div class="ml-3">
                            <p class="text-sm text-yellow-700">
                                <strong>IMPORTANTE:</strong> Primeiro calcule o valor final com possíveis descontos, depois faça o pagamento via PIX para a chave <strong>arraiadomaneco@gmail.com</strong>, e por fim preencha o formulário com seus dados e envie o comprovante pelo WhatsApp.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Tab 2: Enviar Comprovante -->
            <div id="tab-comprovante" class="tab-content">
                <div class="bg-orange-50 p-6 rounded-lg">
                    <div class="alert-box bg-yellow-50 p-4 mb-6">
                        <p class="text-orange-800 font-bold">ATENÇÃO: Primeiro faça o pagamento via PIX!</p>
                        <p class="text-orange-700">Só preencha este formulário após realizar o pagamento para a chave: <strong>arraiadomaneco@gmail.com</strong></p>
                    </div>
                    
                    <form id="comprovanteForm" class="space-y-4">
                        <div>
                            <label for="nome" class="block text-gray-700 font-bold mb-2">Nome Completo*</label>
                            <input type="text" id="nome" name="nome" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                        </div>
                        
                        <div>
                            <label for="cpf" class="block text-gray-700 font-bold mb-2">CPF*</label>
                            <input type="text" id="cpf" name="cpf" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500" placeholder="000.000.000-00" maxlength="14" oninput="formatCPF(this)">
                        </div>
                        
                        <div>
                            <label for="ingresso" class="block text-gray-700 font-bold mb-2">Tipo de Ingresso*</label>
                            <select id="ingresso" name="ingresso" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                                <option value="">Selecione o tipo de ingresso</option>
                                <option value="Ingresso Avulso" data-price="40">Ingresso Avulso - R$ 40,00</option>
                                <option value="Ingresso + Caneca" data-price="50">Ingresso + Caneca - R$ 50,00</option>
                                <option value="Bistrô" data-price="50">Bistrô - R$ 50,00</option>
                                <option value="Apenas Caneca" data-price="10">Apenas Caneca - R$ 10,00</option>
                                <option value="Camarote" data-price="800">Camarote - R$ 800,00</option>
                            </select>
                        </div>
                        
                        <div>
                            <label for="quantidade" class="block text-gray-700 font-bold mb-2">Quantidade</label>
                            <input type="number" id="quantidade" name="quantidade" min="1" value="1" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                        </div>
                        
                        <div>
                            <label for="valorPago" class="block text-gray-700 font-bold mb-2">Valor Pago (R$)*</label>
                            <input type="text" id="valorPago" name="valorPago" required class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500" placeholder="0,00">
                        </div>
                        
                        <div>
                            <label for="cupom" class="block text-gray-700 font-bold mb-2">Cupom Utilizado (se houver)</label>
                            <input type="text" id="cupom" name="cupom" placeholder="Digite o cupom utilizado" class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-orange-500">
                        </div>
                        
                        <div class="pt-4">
                            <button type="submit" class="whatsapp-btn text-white font-bold py-3 px-6 rounded-lg w-full flex items-center justify-center">
                                <svg class="w-6 h-6 mr-2" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/>
                                </svg>
                                Enviar Dados e Comprovante via WhatsApp
                            </button>
                        </div>
                    </form>
                </div>
                
                <div class="mt-6">
                    <button id="btn-back-tab" class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-lg flex items-center justify-center">
                        <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path>
                        </svg>
                        Voltar para calculadora
                    </button>
                </div>
            </div>
            
            <!-- Instruções de pagamento -->
            <div class="mt-8">
                <h3 class="text-xl font-bold mb-4 text-orange-600">Como garantir seu ingresso:</h3>
                
                <div class="space-y-4">
                    <div class="process-step" data-step="1">
                        <h4 class="font-bold text-lg">Escolha seu ingresso</h4>
                        <p class="text-gray-700">Decida qual tipo de ingresso você deseja adquirir.</p>
                    </div>
                    
                    <div class="process-step" data-step="2">
                        <h4 class="font-bold text-lg">Calcule o valor final</h4>
                        <p class="text-gray-700">Use nossa calculadora para verificar o valor final com possíveis descontos.</p>
                    </div>
                    
                    <div class="process-step" data-step="3">
                        <h4 class="font-bold text-lg">Faça o pagamento via PIX</h4>
                        <p class="text-gray-700">Envie o valor correspondente ao seu ingresso para a chave PIX: <strong>arraiadomaneco@gmail.com</strong></p>
                    </div>
                    
                    <div class="process-step" data-step="4">
                        <h4 class="font-bold text-lg">Preencha seus dados e envie o comprovante</h4>
                        <p class="text-gray-700">Após o pagamento, preencha o formulário com seu nome completo e CPF. Você será direcionado ao WhatsApp para enviar o comprovante.</p>
                    </div>
                    
                    <div class="process-step" data-step="5">
                        <h4 class="font-bold text-lg">Seu nome na lista</h4>
                        <p class="text-gray-700">Após a confirmação do pagamento, seu nome será adicionado à lista de convidados.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Galeria de Imagens -->
    <section class="py-8 px-4 bg-orange-50">
        <div class="max-w-6xl mx-auto">
            <h2 class="title-font text-3xl text-center font-bold text-orange-600 mb-6">Galeria</h2>
            
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                <!-- Espaços para imagens da galeria -->
                <div class="image-placeholder h-40 md:h-56">
                    <!-- <img src="URL_DA_IMAGEM_1" alt="Arraiá do Maneco"> -->
                    <span>Imagem 1</span>
                </div>
                <div class="image-placeholder h-40 md:h-56">
                    <!-- <img src="URL_DA_IMAGEM_2" alt="Arraiá do Maneco"> -->
                    <span>Imagem 2</span>
                </div>
                <div class="image-placeholder h-40 md:h-56">
                    <!-- <img src="URL_DA_IMAGEM_3" alt="Arraiá do Maneco"> -->
                    <span>Imagem 3</span>
                </div>
                <div class="image-placeholder h-40 md:h-56">
                    <!-- <img src="URL_DA_IMAGEM_4" alt="Arraiá do Maneco"> -->
                    <span>Imagem 4</span>
                </div>
            </div>
        </div>
    </section>

    <!-- Contato -->
    <section class="py-8 px-4 bg-white">
        <div class="max-w-6xl mx-auto">
            <h2 class="title-font text-3xl text-center font-bold text-orange-600 mb-6">Dúvidas?</h2>
            <div class="bg-orange-50 p-6 rounded-lg shadow-md text-center">
                <p class="text-lg mb-4">Se tiver alguma dúvida sobre o evento, entre em contato conosco!</p>
                <p class="font-bold mb-4">(42) 99857-8465</p>
                <button class="whatsapp-btn text-white font-bold py-3 px-6 rounded-lg flex items-center justify-center mx-auto" onclick="openWhatsApp('Olá! Gostaria de mais informações sobre o Arraiá do Maneco.')">
                    <svg class="w-6 h-6 mr-2" fill="currentColor" viewBox="0 0 24 24">
                        <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/>
                    </svg>
                    Falar pelo WhatsApp
                </button>
            </div>
        </div>
    </section>

    <!-- Rodapé -->
    <footer class="bg-orange-900 text-white py-6 px-4 text-center">
        <h2 class="title-font text-2xl font-bold">Arraiá do Maneco</h2>
        <p class="mt-2">O MAIOR ARRAIÁ DA REGIÃO 042!</p>
        <p class="mt-1">Reserva do Iguaçu/PR</p>
        <p class="mt-4">&copy; 2025 Arraiá do Maneco. Todos os direitos reservados.</p>
    </footer>

    <!-- Botão flutuante de WhatsApp -->
    <div class="float-whatsapp">
        <button class="whatsapp-btn w-16 h-16 rounded-full flex items-center justify-center shadow-lg" onclick="openWhatsApp('Olá! Gostaria de mais informações sobre o Arraiá do Maneco.')">
            <svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24">
                <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/>
            </svg>
        </button>
    </div>

    <!-- Adicionar Leaflet JS para o mapa -->
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

    <!-- Script para contagem regressiva, WhatsApp e mapa -->
    <script>
        // Contagem regressiva
        const countDownDate = new Date("Jul 12, 2025 19:00:00").getTime();
        
        function updateCountdown() {
            const now = new Date().getTime();
            const distance = countDownDate - now;
            
            document.getElementById("days").innerText = Math.floor(distance / (1000 * 60 * 60 * 24));
            document.getElementById("hours").innerText = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            document.getElementById("minutes").innerText = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
            document.getElementById("seconds").innerText = Math.floor((distance % (1000 * 60)) / 1000);
        }
        
        updateCountdown();
        setInterval(updateCountdown, 1000);
        
        // Função para formatar CPF
        function formatCPF(input) {
            let value = input.value.replace(/\D/g, '');
            if (value.length > 11) value = value.slice(0, 11);
            
            if (value.length > 9) {
                value = value.replace(/^(\d{3})(\d{3})(\d{3})/, '$1.$2.$3-');
            } else if (value.length > 6) {
                value = value.replace(/^(\d{3})(\d{3})/, '$1.$2.');
            } else if (value.length > 3) {
                value = value.replace(/^(\d{3})/, '$1.');
            }
            
            input.value = value;
        }
        
        // Variáveis para controle de preço e desconto na calculadora
        let calcDiscount = 0;
        let calcPrice = 0;
        let calcQuantity = 1;
        let selectedTicket = '';
        
        // Função para atualizar o display de preço na calculadora
        function updateCalcDisplay() {
            const ingressoSelect = document.getElementById('calc-ingresso');
            const quantidade = parseInt(document.getElementById('calc-quantidade').value) || 1;
            calcQuantity = quantidade;
            
            if (ingressoSelect.selectedIndex > 0) {
                const selectedOption = ingressoSelect.options[ingressoSelect.selectedIndex];
                const basePrice = parseFloat(selectedOption.getAttribute('data-price'));
                calcPrice = basePrice * quantidade;
                selectedTicket = selectedOption.value;
                
                // Mostrar o resumo de preço
                document.getElementById('calc-result').classList.remove('hidden');
                document.getElementById('calc-empty').classList.add('hidden');
                
                document.getElementById('calc-ingressoTipo').textContent = selectedOption.text.split(' - ')[0];
                document.getElementById('calc-ingressoQtd').textContent = quantidade;
                document.getElementById('calc-valorUnitario').textContent = `R$ ${basePrice.toFixed(2).replace('.', ',')}`;
                document.getElementById('calc-valorTotal').textContent = `R$ ${calcPrice.toFixed(2).replace('.', ',')}`;
                
                // Aplicar desconto se houver
                if (calcDiscount > 0) {
                    const discountAmount = (calcPrice * calcDiscount) / 100;
                    const finalPrice = calcPrice - discountAmount;
                    
                    document.getElementById('calc-discountRow').classList.remove('hidden');
                    document.getElementById('calc-discountPercent').textContent = calcDiscount;
                    document.getElementById('calc-discountAmount').textContent = `- R$ ${discountAmount.toFixed(2).replace('.', ',')}`;
                    document.getElementById('calc-valorFinal').textContent = `R$ ${finalPrice.toFixed(2).replace('.', ',')}`;
                    document.getElementById('calc-result').classList.add('with-discount');
                } else {
                    document.getElementById('calc-discountRow').classList.add('hidden');
                    document.getElementById('calc-valorFinal').textContent = `R$ ${calcPrice.toFixed(2).replace('.', ',')}`;
                    document.getElementById('calc-result').classList.remove('with-discount');
                }
            } else {
                document.getElementById('calc-result').classList.add('hidden');
                document.getElementById('calc-empty').classList.remove('hidden');
            }
        }
        
        // Função para verificar cupom de desconto na calculadora
        document.getElementById('calc-aplicarCupom').addEventListener('click', function() {
            const cupom = document.getElementById('calc-cupom').value.trim().toUpperCase();
            const cupomMessage = document.getElementById('calc-cupomMessage');
            
            if (cupom === '') {
                cupomMessage.textContent = 'Por favor, digite um cupom.';
                cupomMessage.className = 'text-sm mt-1 text-red-600';
                cupomMessage.classList.remove('hidden');
                return;
            }
            
            // Lista de cupons válidos (simulação)
            const cuponsValidos = {
                'ARRAIA10': 10,
                'MANECO20': 20,
                'FESTA15': 15,
                'INDIA20': 20
            };
            
            if (cuponsValidos[cupom]) {
                calcDiscount = cuponsValidos[cupom];
                cupomMessage.textContent = `Cupom aplicado! Desconto de ${calcDiscount}% no seu ingresso.`;
                cupomMessage.className = 'text-sm mt-1 text-green-600';
                cupomMessage.classList.remove('hidden');
                
                // Atualizar o display de preço com o desconto
                updateCalcDisplay();
            } else {
                calcDiscount = 0;
                cupomMessage.textContent = 'Cupom inválido ou expirado.';
                cupomMessage.className = 'text-sm mt-1 text-red-600';
                cupomMessage.classList.remove('hidden');
                updateCalcDisplay();
            }
        });
        
        // Eventos para atualizar a calculadora quando os valores mudam
        document.getElementById('calc-ingresso').addEventListener('change', updateCalcDisplay);
        document.getElementById('calc-quantidade').addEventListener('change', updateCalcDisplay);
        document.getElementById('calc-quantidade').addEventListener('input', updateCalcDisplay);
        
        // Função para abrir WhatsApp com dados do formulário
        document.getElementById('comprovanteForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const nome = document.getElementById('nome').value;
            const cpf = document.getElementById('cpf').value;
            const ingresso = document.getElementById('ingresso').value;
            const quantidade = document.getElementById('quantidade').value;
            const valorPago = document.getElementById('valorPago').value;
            const cupom = document.getElementById('cupom').value;
            
            if (!nome || !cpf || !ingresso || !valorPago) {
                alert('Por favor, preencha todos os campos obrigatórios.');
                return;
            }
            
            let message = `Olá! Já realizei o pagamento para o Arraiá do Maneco.\n\n*Dados para lista de convidados:*\nNome: ${nome}\nCPF: ${cpf}\nIngresso: ${ingresso}\nQuantidade: ${quantidade}\nValor pago: R$ ${valorPago}`;
            
            if (cupom) {
                message += `\nCupom utilizado: ${cupom}`;
            }
            
            message += `\n\n*Estou enviando o comprovante de pagamento em seguida.*`;
            
            openWhatsApp(message);
        });
        
        // Função para abrir WhatsApp
        function openWhatsApp(message) {
            const phone = "5542998578465";
            const encodedMessage = encodeURIComponent(message);
            window.open(`https://wa.me/${phone}?text=${encodedMessage}`, '_blank');
        }
        
        // Sistema de abas
        function switchTab(tabId) {
            // Esconder todas as abas
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Desativar todos os botões
            document.querySelectorAll('.tab-button').forEach(button => {
                button.classList.remove('active');
            });
            
            // Mostrar a aba selecionada
            document.getElementById(tabId).classList.add('active');
            
            // Ativar o botão correspondente
            document.querySelector(`[data-tab="${tabId}"]`).classList.add('active');
            
            // Se estiver indo para a aba de comprovante, preencher automaticamente com os dados da calculadora
            if (tabId === 'tab-comprovante' && selectedTicket) {
                document.getElementById('ingresso').value = selectedTicket;
                document.getElementById('quantidade').value = calcQuantity;
                document.getElementById('cupom').value = document.getElementById('calc-cupom').value;
                
                // Calcular o valor final com desconto
                let valorFinal = calcPrice;
                if (calcDiscount > 0) {
                    valorFinal = calcPrice - ((calcPrice * calcDiscount) / 100);
                }
                document.getElementById('valorPago').value = valorFinal.toFixed(2).replace('.', ',');
            }
        }
        
        // Botões para navegar entre abas
        document.getElementById('btn-next-tab').addEventListener('click', function() {
            switchTab('tab-comprovante');
        });
        
        document.getElementById('btn-back-tab').addEventListener('click', function() {
            switchTab('tab-calcular');
        });
        
        document.querySelectorAll('.tab-button').forEach(button => {
            button.addEventListener('click', function() {
                switchTab(this.getAttribute('data-tab'));
            });
        });
        
        // Inicializar o mapa
        document.addEventListener('DOMContentLoaded', function() {
            // Coordenadas para Reserva do Iguaçu/PR (aproximadas)
            const lat = -25.8383;
            const lng = -52.0172;
            
            // Inicializar o mapa
            const map = L.map('map').setView([lat, lng], 15);
            
            // Adicionar camada de mapa do OpenStreetMap
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
            }).addTo(map);
            
            // Adicionar marcador no local do evento
            const marker = L.marker([lat, lng]).addTo(map);
            marker.bindPopup("<strong>Arraiá do Maneco</strong><br>Parque Hilário Graf Júnior<br>Reserva do Iguaçu/PR").openPopup();
            
            // Adicionar um círculo para destacar a área
            L.circle([lat, lng], {
                color: '#FF6B35',
                fillColor: '#FF6B35',
                fillOpacity: 0.2,
                radius: 200
            }).addTo(map);
            
            // Resetar o formulário ao carregar a página
            document.getElementById('comprovanteForm').reset();
            
            // Inicializar a calculadora
            document.getElementById('calc-result').classList.add('hidden');
            document.getElementById('calc-empty').classList.remove('hidden');
            
            // Configurar os mini-cards de ingressos
            document.querySelectorAll('.ticket-mini').forEach(card => {
                card.addEventListener('click', function() {
                    // Remover seleção de todos os cards
                    document.querySelectorAll('.ticket-mini').forEach(c => {
                        c.classList.remove('selected');
                    });
                    
                    // Selecionar este card
                    this.classList.add('selected');
                    
                    // Obter o tipo de ingresso
                    const ticketType = this.getAttribute('data-ticket');
                    
                    // Selecionar no dropdown da calculadora
                    const calcSelect = document.getElementById('calc-ingresso');
                    for (let i = 0; i < calcSelect.options.length; i++) {
                        if (calcSelect.options[i].value === ticketType) {
                            calcSelect.selectedIndex = i;
                            updateCalcDisplay();
                            
                            // Rolar até a calculadora
                            document.getElementById('calculadora').scrollIntoView({
                                behavior: 'smooth'
                            });
                            break;
                        }
                    }
                });
                
                // Adicionar evento ao botão dentro do card
                const button = card.querySelector('button');
                if (button) {
                    button.addEventListener('click', function(e) {
                        e.stopPropagation(); // Evitar que o evento de clique se propague para o card
                        card.click(); // Simular clique no card
                    });
                }
            });
            
            // Adicionar links para pré-selecionar ingressos
            const ingressoLinks = document.querySelectorAll('a[href="#calculadora"]');
            ingressoLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    const ticketType = this.getAttribute('data-ticket');
                    if (ticketType) {
                        // Selecionar no dropdown da calculadora
                        const calcSelect = document.getElementById('calc-ingresso');
                        for (let i = 0; i < calcSelect.options.length; i++) {
                            if (calcSelect.options[i].value === ticketType) {
                                calcSelect.selectedIndex = i;
                                updateCalcDisplay();
                                break;
                            }
                        }
                    }
                });
            });
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'942c748100db010b',t:'MTc0Nzc1MDYyMC4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
