# renda-ativa-global


      <!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Enterprise Terminal</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #000; color: #fff; font-family: 'Inter', sans-serif; display: flex; justify-content: center; height: 100vh; }
        
        .screen { width: 100%; max-width: 430px; background: #0d1117; display: flex; flex-direction: column; padding: 30px; position: relative; }
        
        /* LOGIN */
        #login { display: flex; flex-direction: column; justify-content: center; height: 100%; }
        input { background: #161b22; border: 1px solid #30363d; padding: 18px; border-radius: 12px; color: #fff; font-size: 16px; margin: 20px 0; text-align: center; }
        button { background: #238636; color: #fff; border: none; padding: 18px; border-radius: 12px; font-weight: bold; font-size: 16px; cursor: pointer; }

        /* DASHBOARD */
        #dash { display: none; padding-top: 20px; }
        .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 40px; }
        .status { background: rgba(46, 160, 67, 0.1); color: #3fb950; padding: 5px 12px; border-radius: 20px; font-size: 12px; display: flex; align-items: center; }
        .dot { width: 6px; height: 6px; background: #3fb950; border-radius: 50%; margin-right: 8px; animation: pulse 1s infinite; }
        
        .counter-box { text-align: center; margin: 50px 0; }
        .val { font-size: 55px; font-weight: 800; font-family: monospace; }
        .unit { color: #3fb950; font-size: 25px; margin-right: 5px; }

        /* BARRA DE PROGRESSO REALISTA */
        .track { width: 100%; height: 6px; background: #30363d; border-radius: 10px; margin-bottom: 20px; overflow: hidden; }
        #bar { width: 0%; height: 100%; background: #2ea043; transition: width 0.3s ease; }

        .console { background: #010409; border: 1px solid #30363d; border-radius: 8px; padding: 15px; font-family: monospace; font-size: 11px; color: #8b949e; height: 150px; overflow: hidden; }
        @keyframes pulse { 0% { opacity: 0.4; } 50% { opacity: 1; } 100% { opacity: 0.4; } }
    </style>
</head>
<body>

    <div class="screen">
        <div id="login">
            <h1 style="text-align:center">Enterprise Data</h1>
            <input type="text" id="user" placeholder="ID do Operador">
            <button onclick="start()">Conectar Agora</button>
        </div>

        <div id="dash">
            <div class="header">
                <div class="status"><div class="dot"></div> OPERANDO</div>
                <div id="id-show" style="font-size:12px; color:#58a6ff"></div>
            </div>

            <div class="counter-box">
                <p style="color:#8b949e; font-size:12px; margin-bottom:10px">CRÉDITOS PROCESSADOS</p>
                <div class="val"><span class="unit">R$</span><span id="num">0,00</span></div>
            </div>

            <div class="track"><div id="bar"></div></div>
            
            <div class="console" id="log">
                <div style="color:#3fb950">> Conexão segura estabelecida...</div>
                <div style="color:#3fb950">> Sincronizando blocos...</div>
            </div>
        </div>
    </div>

    <script>
        function start() {
            const u = document.getElementById('user').value;
            if(!u) return;
            document.getElementById('login').style.display = 'none';
            document.getElementById('dash').style.display = 'block';
            document.getElementById('id-show').innerText = "ID: " + u;
            
            let p = 0; let s = 0;
            const logs = ["Processando hash...", "Alocando cache...", "Buffer verificado.", "Link estável."];

            const timer = setInterval(() => {
                if(p < 100) {
                    p += 0.5; s += 0.12;
                    document.getElementById('bar').style.width = p + "%";
                    document.getElementById('num').innerText = s.toFixed(2).replace('.', ',');
                    
                    if(Math.random() > 0.9) {
                        const l = document.createElement('div');
                        l.innerText = "> " + logs[Math.floor(Math.random()*logs.length)];
                        const box = document.getElementById('log');
                        box.prepend(l);
                    }
                } else { clearInterval(timer); }
            }, 100);
        }
    </script>
</body>
</html>
                          

        
