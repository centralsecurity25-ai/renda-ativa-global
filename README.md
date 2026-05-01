# renda-ativa-global

         <!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Enterprise Terminal</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #000; color: #fff; font-family: sans-serif; display: flex; justify-content: center; min-height: 100vh; overflow: hidden; }
        .app { width: 100%; max-width: 430px; background: #0d1117; padding: 40px 25px; display: flex; flex-direction: column; }
        
        #login-screen { display: flex; flex-direction: column; justify-content: center; height: 100%; }
        #dash-screen { display: none; }

        input { background: #161b22; border: 1px solid #30363d; padding: 18px; border-radius: 10px; color: #fff; font-size: 16px; margin: 25px 0; text-align: center; }
        .btn { background: #238636; color: #fff; border: none; padding: 18px; border-radius: 10px; font-weight: bold; font-size: 16px; cursor: pointer; }
        
        .counter { font-size: 50px; font-weight: 800; text-align: center; margin: 40px 0; font-family: monospace; color: #fff; }
        .prefix { color: #3fb950; font-size: 25px; margin-right: 5px; }

        .track { width: 100%; height: 10px; background: #30363d; border-radius: 20px; overflow: hidden; margin-top: 20px; }
        #bar { width: 0%; height: 100%; background: #2ea043; transition: width 0.3s linear; }
        
        .console { background: #010409; border: 1px solid #30363d; border-radius: 8px; padding: 15px; font-family: monospace; font-size: 12px; color: #8b949e; height: 130px; margin-top: 30px; line-height: 1.6; }
    </style>
</head>
<body>

    <div class="app">
        <div id="login-screen">
            <h1 style="text-align:center">Enterprise Data</h1>
            <p style="text-align:center; color:#8b949e; font-size:14px; margin-top:10px">Autentique sua estação de trabalho</p>
            <input type="text" id="user-id" placeholder="ID DO OPERADOR">
            <button class="btn" onclick="conectar()">CONECTAR AGORA</button>
        </div>

        <div id="dash-screen">
            <div style="display:flex; justify-content:space-between; font-size:12px; color:#3fb950;">
                <span>● SISTEMA ATIVO</span>
                <span id="display-id" style="color:#58a6ff"></span>
            </div>

            <div class="counter">
                <span class="prefix">R$</span><span id="valor">0,00</span>
            </div>

            <div class="track">
                <div id="bar"></div>
            </div>

            <div class="console" id="logs">
                <div style="color:#3fb950">> Conexão segura estabelecida...</div>
                <div>> Alocando memória secundária...</div>
            </div>
        </div>
    </div>

    <script>
        function conectar() {
            const user = document.getElementById('user-id').value;
            if(!user) return alert("Insira um ID");

            // Troca as telas
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('dash-screen').style.display = 'block';
            document.getElementById('display-id').innerText = "ID: " + user;

            let p = 0;
            let s = 0;
            const logBox = document.getElementById('logs');
            const frases = ["> Processando hash...", "> Sincronizando rede...", "> Buffer OK.", "> Pacote recebido."];

            // Inicia a animação
            const loop = setInterval(() => {
                if(p < 100) {
                    p += 0.5;
                    s += 0.12;
                    
                    document.getElementById('bar').style.width = p + "%";
                    document.getElementById('valor').innerText = s.toFixed(2).replace('.', ',');

                    if(Math.random() > 0.96) {
                        const l = document.createElement('div');
                        l.innerText = frases[Math.floor(Math.random() * frases.length)];
                        logBox.prepend(l);
                    }
                } else {
                    clearInterval(loop);
                }
            }, 100);
        }
    </script>

</body>
</html>       
