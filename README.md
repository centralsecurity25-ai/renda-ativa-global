# renda-ativa-global

      <!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cloud Mining Pro - Dashboard</title>
    <style>
        /* Reset para ocupar a tela toda */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body { 
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; 
            background-color: #0b0e11; /* Fundo mais escuro, estilo Binance/Trade */
            color: white; 
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .app-card { 
            background-color: #161b22; 
            border: 1px solid #30363d; 
            padding: 30px 20px; 
            border-radius: 16px; 
            width: 100%;
            max-width: 380px; 
            text-align: center;
            box-shadow: 0 20px 40px rgba(0,0,0,0.7); 
            position: relative;
            overflow: hidden;
        }

        /* Área de Setup (Login) */
        #setup-view { display: block; }
        #mining-view { display: none; }

        input {
            width: 100%;
            padding: 15px;
            margin: 20px 0;
            background: #0b0e11;
            border: 1px solid #30363d;
            border-radius: 8px;
            color: white;
            text-align: center;
            font-size: 1.1em;
        }
        input:focus { border-color: #f1c40f; outline: none; }

        .btn {
            background-color: #f1c40f; /* Amarelo, estilo cripto */
            color: #0b0e11;
            padding: 18px;
            border: none;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
            font-size: 1.2em;
            transition: 0.2s;
        }
        .btn:hover { background-color: #d4ac0d; transform: scale(1.01); }

        /* Estilo do Painel Ativo (Realista) */
        .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px; }
        .status-dot { height: 10px; width: 10px; background-color: #3fb950; border-radius: 50%; display: inline-block; margin-right: 5px;}
        .operator-info { color: #8b949e; font-size: 0.9em; }
        .operator-info strong { color: #58a6ff; }

        .balance-label { font-size: 0.9em; color: #8b949e; margin-bottom: 5px; }
        .balance-value { 
            font-size: 3.8em; 
            color: #f0f6fc; 
            font-weight: 800; 
            letter-spacing: -2px;
            display: flex;
            justify-content: center;
            align-items: baseline;
        }
        .currency-symbol { font-size: 0.5em; color: #3fb950; margin-right: 10px; font-weight: 400;}

        /* A NOVA ANIMAÇÃO: GRÁFICO FALSO */
        .chart-container {
            width: 100%;
            height: 60px;
            margin: 25px 0 10px;
            position: relative;
            background: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjYwIj48ZyBmaWxsPSJub25lIiBzdHJva2U9IiMyMzgyMzYiIHN0cm9rZS13aWR0aD0iMSIgc3Ryb2tlLW9wYWNpdHk9IjAuMiI+PGxpbmUgeDE9IjAiIHkxPSIxMCIgeDI9IjEwMCUiIHkyPSIxMCIvPjxsaW5lIHgxPSIwIiB5MT0iMzAiIHgyPSIxMDAlIiB5Mj0iMzAiLz48bGluZSB4MT0iMCIgeTE9IjUwIiB4Mj0iMTAwJSIgeTI9IjUwIi8+PC9nPjwvc3ZnPg==');
        }

        .fake-chart-line {
            position: absolute;
            bottom: 0;
            left: 0;
            height: 100%;
            width: 100%;
            fill: none;
            stroke: #3fb950;
            stroke-width: 2;
            stroke-linecap: round;
            stroke-dasharray: 1000;
            stroke-dashoffset: 1000;
            transition: stroke-dashoffset 0.1s linear; /* Animação da linha */
        }

        .progress-bar-container {
            width: 100%;
            height: 6px;
            background-color: #0b0e11;
            border-radius: 10px;
            overflow: hidden;
            border: 1px solid #30363d;
        }
        .progress-fill {
            width: 0%;
            height: 100%;
            background: linear-gradient(90deg, #238636 0%, #3fb950 100%);
            border-radius: 10px;
            transition: width 0.1s linear;
        }

        .status-text { color: #8b949e; font-size: 0.85em; margin-top: 15px; text-transform: uppercase; letter-spacing: 1px;}
    </style>
</head>
<body>

    <div class="app-card">
        <div id="setup-view">
            <h2>Cloud Mining Pro</h2>
            <p style="color: #8b949e; margin-top: 10px;">Inicie seu nó de processamento</p>
            <input type="text" id="operator-name" placeholder="Nome do Operador">
            <button class="btn" onclick="iniciarNivel()">CONECTAR REDE</button>
        </div>

        <div id="mining-view">
            <div class="header">
                <div><span class="status-dot"></span> <span style="font-size:0.8em; color:#3fb950;">ATIVO</span></div>
                <div class="operator-info">Op: <strong id="name-tag"></strong></div>
            </div>

            <p class="balance-label">SALDO EM MINERAÇÃO</p>
            <div class="balance-value">
                <span class="currency-symbol">R$</span>
                <span id="valor-real">0,00</span>
            </div>
            
            <div class="chart-container">
                <svg width="100%" height="100%" viewBox="0 0 100 60" preserveAspectRatio="none">
                    <path class="fake-chart-line" id="chart-path" d="M0,60 L5,55 L10,58 L15,50 L20,53 L25,40 L30,45 L35,30 L40,35 L45,20 L50,25 L55,10 L60,15 L65,5 L70,8 L75,2 L80,5 L85,1 L90,3 L95,0 L100,2" />
                </svg>
            </div>

            <div class="progress-bar-container">
                <div class="progress-fill" id="bar-fill"></div>
            </div>

            <p class="status-text">Processando blocos na nuvem...</p>
        </div>
    </div>

    <script>
        let saldoAcumulado = 0;
        let progressoGeral = 0;

        function iniciarNivel() {
            const nomeStr = document.getElementById('operator-name').value;
            if(!nomeStr) return alert("Por favor, digite um nome.");

            document.getElementById('setup-view').style.display = 'none';
            document.getElementById('mining-view').style.display = 'block';
            document.getElementById('name-tag').innerText = nomeStr;

            iniciarAnimacaoRealista();
        }

        function iniciarAnimacaoRealista() {
            const path = document.getElementById('chart-path');
            const pathLength = path.getTotalLength();
            
            // Configura o SVG para a animação
            path.style.strokeDasharray = pathLength + ' ' + pathLength;
            path.style.strokeDashoffset = pathLength;

            setInterval(() => {
                if(progressoGeral < 100) {
                    progressoGeral += 0.4; // Sobe a barra
                    saldoAcumulado += 0.12; // Sobe o dinheiro
                    
                    // Atualiza a Barra de Progresso
                    document.getElementById('bar-fill').style.width = progressoGeral + '%';
                    
                    // Atualiza o Saldo (R$ X,XX)
                    document.getElementById('valor-real').innerText = saldoAcumulado.toFixed(2).replace('.', ',');

                    // ATUALIZA O GRÁFICO: Faz a linha "aparecer" conforme o progresso
                    const drawLength = pathLength * (progressoGeral / 100);
                    path.style.strokeDashoffset = pathLength - drawLength;
                }
            }, 100); // Roda a cada 0.1 segundo
        }
    </script>

</body>
</html>
                   
        
        
