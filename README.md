# renda-ativa-global


<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Enterprise Cloud - Data Terminal</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; outline: none; }
        
        body { 
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; 
            background-color: #000000; 
            color: #ffffff; 
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        .main-app {
            width: 100%;
            max-width: 430px;
            height: 100%;
            background-color: #0d1117;
            display: flex;
            flex-direction: column;
            padding: 25px;
            position: relative;
        }

        /* VISTA 1: AUTENTICAÇÃO */
        #auth-view { display: flex; flex-direction: column; justify-content: center; height: 100%; }
        h1 { font-size: 24px; font-weight: 700; margin-bottom: 8px; text-align: center; }
        .subtitle { color: #8b949e; text-align: center; margin-bottom: 40px; font-size: 14px; }
        
        .input-box {
            background: #161b22;
            border: 1px solid #30363d;
            padding: 16px;
            border-radius: 12px;
            color: white;
            font-size: 16px;
            margin-bottom: 20px;
            text-align: center;
        }

        .primary-btn {
            background-color: #238636;
            color: white;
            padding: 16px;
            border: none;
            border-radius: 12px;
            font-weight: 600;
            font-size: 16px;
            cursor: pointer;
            transition: 0.2s;
        }

        /* VISTA 2: LOADER */
        #loading-view { display: none; flex-direction: column; justify-content: center; align-items: center; height: 100%; }
        .spinner { width: 40px; height: 40px; border: 3px solid #30363d; border-top: 3px solid #2ea043; border-radius: 50%; animation: spin 1s linear infinite; margin-bottom: 20px; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* VISTA 3: DASHBOARD REALISTA */
        #dashboard-view { display: none; height: 100%; }
        .top-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 40px; }
        .status-pill { background: rgba(46, 160, 67, 0.15); color: #3fb950; padding: 4px 12px; border-radius: 20px; font-size: 12px; font-weight: 600; display: flex; align-items: center; }
        .dot { height: 6px; width: 6px; background: #3fb950; border-radius: 50%; margin-right: 6px; }

        .balance-container { text-align: center; margin: 40px 0; }
        .balance-label { font-size: 13px; color: #8b949e; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 10px; }
        .balance-amount { font-size: 52px; font-weight: 800; font-family: 'Courier New', monospace; color: #ffffff; }
        .balance-amount span { color: #3fb950; font-size: 28px; vertical-align: top; margin-right: 5px; }

        .terminal-box {
            background: #010409;
            border: 1px solid #30363d;
            border-radius: 12px;
            padding: 15px;
            flex-grow: 1;
            font-family: 'Courier New', monospace;
            font-size: 11px;
            color: #8b949e;
            overflow: hidden;
            margin-bottom: 20px;
        }

        .log-line { margin-bottom: 5px; line-height: 1.4; }
        .log-green { color: #3fb950; }

        .progress-track { width: 100%; height: 4px; background: #30363d; border-radius: 2px; margin-bottom: 10px; }
        .progress-thumb { width: 0%; height: 100%; background: #2ea043; border-radius: 2px; transition: 0.1s linear; }
    </style>
</head>
<body>

    <div class="main-app">
        <div id="auth-view">
            <h1>Enterprise Data</h1>
            <p class="subtitle">Conecte sua conta para iniciar o processamento.</p>
            <input type="text" id="user-input" class="input-box" placeholder="Identificação do Operador">
            <button class="primary-btn" onclick="auth()">Conectar Agora</button>
        </div>

        <div id="loading-view">
            <div class="spinner"></div>
            <p style="color: #8b949e;">Sincronizando com o nó principal...</p>
        </div>

        <div id="dashboard-view">
            <div class="top-bar">
                <div class="status-pill"><span class="dot"></span> EM OPERAÇÃO</div>
                <div style="font-size: 13px; color: #8b949e;">ID: <span id="user-id" style="color: #58a6ff;"></span></div>
            </div>

            <div class="balance-container">
                <p class="balance-label">Créditos de Processamento</p>
                <div class="balance-amount"><span>R$</span><span id="counter">0,00</span></div>
            </div>

            <div class="terminal-box" id="terminal">
                <div class="log-line log-green">> Iniciando handshaking... OK</div>
                <div class="log-line log-green">> Conexão estabelecida com servidor BR-SP-04</div>
                <div class="log-line">> Aguardando pacotes de dados...</div>
            </div>

            <div class="progress-track">
                <div class="progress-thumb" id="progress"></div>
            </div>
            <p style="text-align: center; font-size: 11px; color: #484f58;">Eficiência de Mineração: 98.4%</p>
        </div>
    </div>

    <script>
        let balance = 0;
        let progress = 0;
        const terminalLogs = [
            "Validando blocos de transação...",
            "Processando metadados publicitários...",
            "Sincronizando buffer local...",
            "Alocando memória de vídeo em nuvem...",
            "Confirmando recebimento de pacotes...",
            "Executando script de otimização..."
        ];

        function auth() {
            const user = document.getElementById('user-input').value;
            if(!user) return;

            document.getElementById('auth-view').style.display = 'none';
            document.getElementById('loading-view').style.display = 'flex';

            setTimeout(() => {
                document.getElementById('loading-view').style.display = 'none';
                document.getElementById('dashboard-view').style.display = 'block';
                document.getElementById('user-id').innerText = user;
                runOperation();
            }, 2500);
        }

        function runOperation() {
            const counter = document.getElementById('counter');
            const progressThumb = document.getElementById('progress');
            const terminal = document.getElementById('terminal');

            setInterval(() => {
                if(progress < 100) {
                    progress += 0.2;
                    balance += 0.08;
                    
                    progressThumb.style.width = progress + '%';
                    counter.innerText = balance.toFixed(2).replace('.', ',');

                    // Adiciona logs aleatórios no terminal para parecer real
                    if(Math
              
            
