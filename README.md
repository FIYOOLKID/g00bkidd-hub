<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>PREFEITURA Bc</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, black, purple);
      color: white;
    }
    .login-container, .main-container {
      text-align: center;
      padding: 20px;
    }
    h1 {
      font-family: cursive;
      font-size: 2.2em;
      margin-bottom: 20px;
    }
    input {
      display: block;
      margin: 10px auto;
      padding: 10px;
      border-radius: 8px;
      border: none;
      width: 250px;
    }
    button {
      padding: 12px 20px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 10px;
      font-weight: bold;
      transition: transform 0.2s;
    }
    button:hover { transform: scale(1.05); }
    #multasBtn { background: purple; color: white; }
    #comentariosBtn { background: white; color: purple; }
    .emoji-bg {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 120px;
      background: black;
      color: purple;
      animation: fadeIn 2s forwards;
      z-index: 999;
    }
    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    .hidden { display: none; }
    .registro-lista, .comentarios-lista {
      margin-top: 20px;
      text-align: left;
      max-height: 200px;
      overflow-y: auto;
      background: rgba(255,255,255,0.1);
      padding: 10px;
      border-radius: 10px;
    }
    .registro-item, .comentario-item {
      border-bottom: 1px solid rgba(255,255,255,0.2);
      padding: 5px;
    }
    img {
      width: 100px;
      margin-top: 15px;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <!-- Login -->
  <div class="login-container" id="loginContainer">
    <h1>PREFEITURA Bc</h1>
    <input type="text" id="discordId" placeholder="ID do Discord">
    <input type="password" id="senha" placeholder="Senha">
    <button onclick="login()">Entrar</button>
    <img src="SUA_IMAGEM_AQUI.png" alt="Logo">
  </div>

  <!-- Principal -->
  <div class="main-container hidden" id="mainContainer">
    <h1>Bem-vindo à Prefeitura Bc 🌆</h1>

    <!-- Botão Multas -->
    <button id="multasBtn">Registro de multas</button>
    <div id="multasBox" class="hidden">
      <input type="text" id="nomeUsuario" placeholder="Nome do usuário">
      <input type="text" id="idUsuario" placeholder="ID do usuário">
      <button onclick="registrarMulta()">Registrar Multa</button>
      <div class="registro-lista" id="listaMultas"></div>
    </div>

    <!-- Botão Comentários -->
    <button id="comentariosBtn">Comentários à gestão</button>
    <div id="comentarioBox" class="hidden">
      <input type="text" id="comentarioInput" placeholder="Digite o que devemos melhorar...">
      <button onclick="enviarComentario()">Enviar texto</button>
      <div class="comentarios-lista" id="listaComentarios"></div>
    </div>

    <img src="SUA_IMAGEM_AQUI.png" alt="Logo">
  </div>

  <script>
    const loginContainer = document.getElementById("loginContainer");
    const mainContainer = document.getElementById("mainContainer");

    // Checa login automático
    window.onload = function() {
      if(localStorage.getItem("logado") === "true"){
        showMain();
      }
      carregarMultas();
      carregarComentarios();
    }

    function login(){
      let discordId = document.getElementById("discordId").value;
      let senha = document.getElementById("senha").value;

      if(discordId && senha){
        localStorage.setItem("logado", "true");
        showAnimation();
        setTimeout(showMain, 2000);
      } else {
        alert("Preencha ID e senha!");
      }
    }

    function showAnimation(){
      let emojiBg = document.createElement("div");
      emojiBg.classList.add("emoji-bg");
      emojiBg.innerText = "🌆";
      document.body.appendChild(emojiBg);
      setTimeout(() => { emojiBg.remove(); }, 2000);
    }

    function showMain(){
      loginContainer.classList.add("hidden");
      mainContainer.classList.remove("hidden");
    }

    // --- Registro de Multas ---
    document.getElementById("multasBtn").onclick = function(){
      let box = document.getElementById("multasBox");
      box.classList.toggle("hidden");
    }

    function registrarMulta(){
      let nome = document.getElementById("nomeUsuario").value;
      let id = document.getElementById("idUsuario").value;
      if(!nome || !id){ alert("Preencha todos os campos!"); return; }

      let agora = new Date();
      let data = agora.toLocaleDateString();
      let hora = agora.getHours();
      let min = agora.getMinutes();
      let seg = agora.getSeconds();
      let ms = agora.getMilliseconds();

      let multa = {
        nome: nome,
        id: id,
        data: data,
        hora: hora,
        minuto: min,
        segundo: seg,
        ms: ms
      };

      let multas = JSON.parse(localStorage.getItem("multas") || "[]");
      multas.push(multa);
      localStorage.setItem("multas", JSON.stringify(multas));
      carregarMultas();
    }

    function carregarMultas(){
      let lista = document.getElementById("listaMultas");
      lista.innerHTML = "";
      let multas = JSON.parse(localStorage.getItem("multas") || "[]");
      multas.forEach(m => {
        let div = document.createElement("div");
        div.className = "registro-item";
        div.innerText = `Nome: ${m.nome} | ID: ${m.id} | ${m.data} ${m.hora}:${m.minuto}:${m.segundo}.${m.ms}`;
        lista.appendChild(div);
      });
    }

    // --- Comentários ---
    document.getElementById("comentariosBtn").onclick = function(){
      let box = document.getElementById("comentarioBox");
      box.classList.toggle("hidden");
    }

    function enviarComentario(){
      let comentario = document.getElementById("comentarioInput").value;
      if(!comentario){ alert("Digite algo!"); return; }

      let comentarios = JSON.parse(localStorage.getItem("comentarios") || "[]");
      comentarios.push(comentario);
      localStorage.setItem("comentarios", JSON.stringify(comentarios));
      document.getElementById("comentarioInput").value = "";
      carregarComentarios();
    }

    function carregarComentarios(){
      let lista = document.getElementById("listaComentarios");
      lista.innerHTML = "";
      let comentarios = JSON.parse(localStorage.getItem("comentarios") || "[]");
      comentarios.forEach(c => {
        let div = document.createElement("div");
        div.className = "comentario-item";
        div.innerText = c;
        lista.appendChild(div);
      });
    }
  </script>
</body>
</html>
