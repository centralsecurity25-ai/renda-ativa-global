# renda-ativa-global

<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Processamento</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background-color: #0d1117; 
            color: white; 
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .painel { 
            background-color: #161b22; 
            border: 1px solid #30363d; 
            padding: 30px; 
            border-radius: 12px; 
            width: 90%;
            max-width: 400px; 
            text-align: center;
        }

        #setup-view { display: block; }
        #mining-view { display: none; }

        input {
            width: 100%;
            padding: 12px;
            margin: 20px 0;
            background: #0d1117;
            border: 1px solid #30363d;
            border-radius: 6px;
            color: white;
            text-align: center;
        }

        .btn {
            background-color: #238636;
            color: white;
            padding: 15px 25px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }

        .saldo-display {
            font-size: 3em;
            color: #3fb950;
            margin: 20px 0;
            font-weight: bold;
        }

        .progress-container {
            background: #0d1117;
            border-radius: 10px;
            height: 10px;
            width: 100%;
            margin-top: 10px;
            overflow: hidden;
        }

        .progress-bar {
            background: #238636;
            height: 100%;
            width: 0%;
            transition: width 0.1s linear;
        }

        .status-text { color: #8b949e; font-size: 0.9em; margin-top: 10px; }
    </style>
</head>
<body>

    <div class="painel">
        <div id="setup-view">
            <h2>Configurar Operação</h2>
            <input type="text" id="user-name" placeholder="Digite seu nome de operador">
            <button class="btn" onclick="iniciar()">CONECTAR SERVIDOR</button>
        </div>

        <div id="mining-view">
            <p>Operador: <span id="name-tag" style="color: #58a6ff;"></span></p>
            <div class="saldo-display">R$ <span id="valor">0,00</span></div>
            
            <div class="progress-container">
                <div class="progress-bar" id="bar"></div>
            </div>
            <p class="status-text">Processando dados em nuvem...</p>
        </div>
    </div>

    <script>
        let saldo = 0;
        let porcetagem = 0;

        function iniciar() {
            const nome = document.getElementById('user-name').value;
            if(!nome) return alert("Digite um nome");

            document.getElementById('setup-view').style.display = 'none';
            document.getElementById('mining-view').style.display = 'block';
            document.getElementById('name-tag').innerText = nome;

            setInterval(() => {
                if(porcetagem < 100) {
                    porcetagem += 0.5;
                    saldo += 0.15;
                    
                    document.getElementById('bar').style.width = porcetagem + '%';
                    document.getElementById('valor').innerText = saldo.toFixed(2).replace('.', ',');
                }
            }, 100);
        }
    </script>

</body>
</html>
