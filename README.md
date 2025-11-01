<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shelf Life 30% Check</title>
    <!-- Adiciona o Manifesto do PWA -->
    <link rel="manifest" href="manifest.json">
    <!-- Define a cor da barra de status no Android -->
    <meta name="theme-color" content="#1e3a8a">
    
    <!-- O CSS é totalmente embutido para garantir o funcionamento 100% OFFLINE. -->
    <style>
        /* Estilos base e simulação de classes Tailwind para garantir o offline: */
        :root {
            --color-primary: #1e3a8a; /* Azul Escuro para botões e cabeçalho */
            --color-success: #10b981; /* Verde - Safe */
            --color-danger: #ef4444; /* Vermelho - Recusar */
            --color-light: #f7f9fb; /* Fundo */
            --color-dark: #1f2937; /* Texto escuro */
        }
        body {
            font-family: 'Inter', Arial, sans-serif;
            background-color: var(--color-light);
            min-height: 100vh;
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            box-sizing: border-box; 
        }
        .app-container {
            max-width: 30rem;
            width: 100%;
            margin: 0 auto;
        }
        h1, h2, h3 { color: var(--color-dark); }
        .text-center { text-align: center; }
        .mb-6 { margin-bottom: 1.5rem; }
        .text-3xl { font-size: 1.875rem; line-height: 2.25rem; }
        .text-xl { font-size: 1.25rem; line-height: 1.75rem; }
        .font-bold { font-weight: 700; }
        .font-semibold { font-weight: 600; }
        .text-sm { font-size: 0.875rem; line-height: 1.25rem; }
        .text-xs { font-size: 0.75rem; line-height: 1rem; }
        
        /* Card Styles */
        .card {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -4px rgba(0, 0, 0, 0.1);
            border-radius: 1rem;
            background-color: white;
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }
        
        /* Formulário */
        .space-y-4 > * + * { margin-top: 1rem; }
        .input-group { position: relative; }
        .input-group label {
            display: block;
            margin-bottom: 0.25rem;
            font-size: 0.75rem;
            font-weight: 600;
            color: #4b5563;
        }
        input[type="date"] {
            width: 100%;
            padding: 0.75rem 1rem;
            border: 1px solid #d1d5db;
            border-radius: 0.5rem;
            color: var(--color-dark);
            -webkit-appearance: none;
        }
        
        /* Botão Principal */
        button {
            width: 100%;
            background-color: var(--color-primary);
            color: white;
            font-weight: 700;
            padding: 1rem;
            border-radius: 0.5rem;
            transition: background-color 0.2s ease-in-out;
            cursor: pointer;
            border: none;
            margin-top: 1.5rem;
        }
        button:active { background-color: #1e3a8a; }

        /* Resultado */
        .result-container {
            border-radius: 1rem;
            overflow: hidden;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        .result-box-safe { background-color: var(--color-success); }
        .result-box-danger { background-color: var(--color-danger); }
        .text-white { color: white; }
        .font-extrabold { font-weight: 800; }
        .text-4xl { font-size: 2.25rem; line-height: 2.5rem; }
        .text-5xl { font-size: 3rem; line-height: 1; }
        .p-6 { padding: 1.5rem; }

        /* Detalhes do Cálculo */
        .detail-grid {
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            gap: 0.5rem;
            margin-top: 1rem;
        }
        .detail-item {
            background-color: var(--color-light);
            padding: 0.75rem;
            border-radius: 0.5rem;
            border: 1px solid #e5e7eb;
        }
        .detail-label { color: #6b7280; font-size: 0.625rem; font-weight: 500; text-transform: uppercase; }
        .detail-value { font-weight: 700; color: var(--color-dark); font-size: 1rem; }
        
        /* Alerta */
        .alert {
            padding: 1rem;
            border-radius: 0.5rem;
            font-weight: 600;
        }
        .alert-warning { background-color: #fef3c7; color: #b45309; }
        
        /* Ícone SVG (para manter offline) */
        .icon-box {
            display: inline-block;
            width: 1.5rem;
            height: 1.5rem;
            vertical-align: middle;
            margin-right: 0.5rem;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center">

    <div id="app" class="app-container">
        <header class="text-center mb-6">
            <h1 class="text-3xl font-bold flex items-center justify-center">
                <span class="icon-box">
                    <!-- Ícone de Check/Regra - Totalmente Offline -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="color: var(--color-primary);"><path d="M7.5 7.5L4 12l3.5 4.5"/><path d="M16.5 7.5L20 12l-3.5 4.5"/><path d="M15 2h-6"/><path d="M12 2v20"/></svg>
                </span>
                CHECK SHELF LIFE
            </h1>
            <p class="text-sm text-gray-500 mt-1">Regra: Máximo de 30% da vida útil consumida.</p>
        </header>

        <!-- Formulário de Cálculo -->
        <div id="calc-form-container" class="card">
            <h2 class="text-xl font-semibold mb-4 text-gray-700">Insira as Datas</h2>
            <form id="date-form" class="space-y-4">
                
                <div class="input-group">
                    <label for="manufacture-date">1. Data de Fabricação</label>
                    <input type="date" id="manufacture-date" required>
                </div>

                <div class="input-group">
                    <label for="expiry-date">2. Data de Validade (Vencimento)</label>
                    <input type="date" id="expiry-date" required>
                </div>
                
                 <div class="input-group">
                    <label for="receipt-date">3. Data de Recebimento (Hoje)</label>
                    <input type="date" id="receipt-date" required>
                </div>

                <button type="submit">
                    VERIFICAR RECEBIMENTO
                </button>
            </form>
        </div>

        <!-- Resultado do Cálculo -->
        <div id="result-container" class="hidden result-container mb-6">
            <!-- O resultado será injetado aqui -->
        </div>

        <!-- Área de mensagens (Alertas) -->
        <div id="message-area" class="text-center alert alert-warning hidden">
            <!-- Mensagens de erro/aviso aparecem aqui -->
        </div>
        
    </div>

    <!-- Script de Lógica do Cálculo (Totalmente Offline) -->
    <script>
        // Regra de Negócio: Não pode exceder 30% da vida útil consumida
        const THRESHOLD_PERCENTAGE_PASSED = 30;
        const THRESHOLD_PERCENTAGE_REMAINING = 100 - THRESHOLD_PERCENTAGE_PASSED; 

        // Elementos do DOM
        const dateForm = document.getElementById('date-form');
        const mfgDateInput = document.getElementById('manufacture-date');
        const expDateInput = document.getElementById('expiry-date');
        const receiptDateInput = document.getElementById('receipt-date');
        const resultContainer = document.getElementById('result-container');
        const messageArea = document.getElementById('message-area');

        // Define a data de recebimento padrão como hoje ao carregar
        window.onload = () => {
            const today = new Date().toISOString().substring(0, 10);
            receiptDateInput.value = today;
            
            // --- REGISTRA SERVICE WORKER PARA GARANTIR OFFLINE ---
            if ('serviceWorker' in navigator) {
                navigator.serviceWorker.register('service-worker.js')
                    .then(registration => console.log('SW registered: ', registration))
                    .catch(registrationError => console.log('SW registration failed: ', registrationError));
            }
        };

        /**
         * Converte uma string de data (YYYY-MM-DD) para um objeto Date.
         * Garante que o fuso horário não cause problemas (UTC 00:00:00)
         */
        function getDateObject(dateString) {
            const parts = dateString.split('-');
            // new Date(ano, mes-1, dia) - Cria a data em UTC
            return new Date(Date.UTC(parts[0], parts[1] - 1, parts[2]));
        }

        /**
         * Calcula os dias entre duas datas (apenas a diferença de data).
         */
        function dateDiffInDays(date1, date2) {
            // Calcula a diferença em milissegundos
            const diffTime = Math.abs(date2.getTime() - date1.getTime());
            // Converte milissegundos para dias e arredonda para o dia mais próximo
            return Math.round(diffTime / (1000 * 60 * 60 * 24));
        }

        /**
         * Exibe uma mensagem de erro ou aviso na área dedicada.
         */
        function showMessage(message) {
            messageArea.textContent = message;
            messageArea.classList.remove('hidden');
            resultContainer.classList.add('hidden');
        }

        /**
         * Função principal de cálculo e renderização.
         */
        function calculateShelfLife(event) {
            event.preventDefault();
            
            messageArea.classList.add('hidden');
            resultContainer.classList.add('hidden');

            const mfgDate = getDateObject(mfgDateInput.value);
            const expDate = getDateObject(expDateInput.value);
            const receiptDate = getDateObject(receiptDateInput.value);
            
            // --- 1. Validações de Datas ---
            if (mfgDate.getTime() >= expDate.getTime()) {
                showMessage("Erro: A Data de Fabricação não pode ser igual ou posterior à Data de Validade.");
                return;
            }
            if (receiptDate.getTime() >= expDate.getTime()) {
                showMessage("Atenção: A Data de Recebimento está igual ou após a Data de Validade. Produto VENCIDO.");
                renderResult(0, false);
                return;
            }

            // --- 2. Cálculos Baseados em Dias ---
            const totalLifeDays = dateDiffInDays(mfgDate, expDate); 
            const consumedLifeDays = dateDiffInDays(mfgDate, receiptDate);
            const remainingLifeDays = dateDiffInDays(receiptDate, expDate); 

            // --- 3. Cálculo da Porcentagem ---
            const percentageConsumed = (consumedLifeDays / totalLifeDays) * 100;
            const percentageRemaining = 100 - percentageConsumed;
            
            // Arredonda para baixo (conservador)
            const roundedRemaining = Math.max(0, Math.floor(percentageRemaining)); 

            // --- 4. Aplicação da Regra de Negócio (30% Consumido / 70% Restante) ---
            const isReceivable = roundedRemaining >= THRESHOLD_PERCENTAGE_REMAINING;

            // --- 5. Renderiza o Resultado ---
            renderResult(roundedRemaining, isReceivable, totalLifeDays, consumedLifeDays, remainingLifeDays);
        }

        /**
         * Renderiza o resultado do cálculo.
         */
        function renderResult(percentage, isReceivable, totalLifeDays, consumedLifeDays, remainingLifeDays) {
            
            const alertClass = isReceivable ? 'result-box-safe' : 'result-box-danger';
            const icon = isReceivable 
                ? '<svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 10 1 1-5.93-9.14"/><path d="M22 4 12 14.01 9 11.01"/></svg>'
                : '<svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><path d="m15 9-6 6"/><path d="m9 9 6 6"/></svg>';
            
            const statusText = isReceivable ? 'PODE RECEBER' : 'RECUSAR RECEBIMENTO';
            const advisoryText = isReceivable 
                ? `A vida útil restante de ${percentage}% atende ao requisito mínimo de ${THRESHOLD_PERCENTAGE_REMAINING}%.`
                : `A vida útil restante de ${percentage}% está abaixo do mínimo de ${THRESHOLD_PERCENTAGE_REMAINING}% (30% da vida útil total foi excedida).`;

            const totalConsumedPercent = (100 - percentage).toFixed(1);

            const resultHtml = `
                <!-- STATUS DE RECEBIMENTO -->
                <div class="p-6 text-white text-center ${alertClass}">
                    <div class="text-4xl mb-2">${icon}</div>
                    <h3 class="text-2xl font-extrabold">${statusText}</h3>
                    <p class="text-sm font-medium mt-1">${advisoryText}</p>
                </div>
                
                <!-- DETALHES DO CÁLCULO -->
                <div class="p-6 bg-white space-y-4">
                    <!-- Porcentagem de Destaque -->
                    <div class="text-center p-4 rounded-lg" style="border: 2px dashed ${isReceivable ? '#a7f3d0' : '#fca5a5'};">
                        <p class="text-sm text-gray-500 font-semibold">VIDA ÚTIL RESTANTE (Shelf Life)</p>
                        <p class="text-5xl font-extrabold mt-1" style="color: ${isReceivable ? '#059669' : '#dc2626'};">${percentage}%</p>
                    </div>

                    <!-- Detalhes em Grid -->
                    <div class="detail-grid">
                        <div class="detail-item">
                            <p class="detail-label">Vida Útil Total</p>
                            <p class="detail-value">${totalLifeDays} dias</p>
                        </div>
                        <div class="detail-item">
                            <p class="detail-label">Consumido</p>
                            <p class="detail-value">${consumedLifeDays} dias (${totalConsumedPercent}%)</p>
                        </div>
                        <div class="detail-item">
                            <p class="detail-label">Dias Restantes</p>
                            <p class="detail-value">${remainingLifeDays} dias</p>
                        </div>
                        <div class="detail-item">
                            <p class="detail-label">Limite Consumido</p>
                            <p class="detail-value">${THRESHOLD_PERCENTAGE_PASSED}% (30%)</p>
                        </div>
                    </div>
                </div>
            `;
            
            resultContainer.innerHTML = resultHtml;
            resultContainer.classList.remove('hidden');
        }

        dateForm.addEventListener('submit', calculateShelfLife);
    </script>
</body>
</html>
