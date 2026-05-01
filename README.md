# renda-ativa-global

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Enterprise Terminal</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { background: #000; color: #fff; font-family: sans-serif; display: flex; justify-content: center; min-height: 100vh; }
        .screen { width: 100%; max-width: 430px; background: #0d1117; display: flex; flex-direction: column; padding: 30px; }
        #login { display: flex; flex-direction: column; justify-content: center; height: 100vh; }
        #dash { display: none; padding-top: 20px; }
        input { background: #161b22; border: 1px solid #30363d; padding: 18px; border-radius: 12px; color: #fff; font-size: 16px; margin: 20px 0; text-align: center; }
        button { background: #238636; color: #fff; border: none; padding: 18px; border-radius: 12px; font-weight: bold; font-size: 16px; cursor: pointer; }
        .val { font-size: 50px; font-weight: 800; text-align: center; margin: 40px 0; font-family: monospace; }
        
        /* BARRA - O SEGREDO ESTÁ AQUI */
        .track { width: 100%; height: 10px; background: #30363d; border-radius: 20px; overflow: hidden; margin-bottom: 20px; }
        #bar { width: 0%; height: 100%; background: #2ea043; transition: width 0.4s ease-out; }
        
        .console { background: #010409; border: 1px solid #30363d; border-radius: 8px; padding: 15px; font-family: monospace; font-size: 11px; color: #8b949e; height: 120px; overflow: hidden; }
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
            <div style="display:flex; justify-content:space-between; font-size:12px; color:#3fb950; margin-bottom:20px;">
                <span>● ONLINE</span>
                <span id="id-show" style="color:#58a6ff"></span>
            </div>
            <div class="val"><span style="color:#3fb950; font-size:25px">R$ </span><span id="num">0,00</span></div>
            <div class="track"><div id="bar"></div></div>
            <div class="console" id="log">> Aguardando pacotes...</div>
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
            // O loop que faz a barra e o saldo subirem
            const timer = setInterval(() => {
                if(p < 100) {
                    p += 1; // Sobe a barra de 1 em 1%
                    s += 0.25; // Sobe o dinheiro
                    document.getElementById('bar').style.width = p + "%";
                    document.getElementById('num').innerText = s.toFixed(2).replace('.', ',');
                } else { clearInterval(timer); }
            }, 150); // Velocidade da animação
        }
    </script>
</body>
</html>

              
