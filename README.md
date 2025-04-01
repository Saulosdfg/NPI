# NPI
Projetos pro Victor, vulgo meu padrinho, achar os erros (que as vezes são só inconveniências).

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Listador de Tarefas Moderno</title>
    <style>
        /* Estilos gerais */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            background-color: #f8f9fa;
            color: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
        }

        /* Barra superior */
        .barra-superior {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            position: fixed;
            top: 0;
            left: 0;
            z-index: 1000;
            overflow: hidden;
            box-sizing: border-box;
        }

        .barra-superior h1 {
            margin: 0;
            font-size: 20px;
            margin-right: auto;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 50%;
        }

        .barra-superior .botoes {
            display: flex;
            gap: 5px;
            max-width: 45%;
            justify-content: flex-end;
        }

        .barra-superior button {
            background: none;
            border: none;
            color: white;
            font-size: 14px;
            cursor: pointer;
            padding: 8px 12px;
            white-space: nowrap;
        }

        .barra-superior button:hover {
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
        }

        /* Botão Hamburguer */
        .menu-hamburguer {
            display: none;
            background: none;
            border: none;
            color: white;
            font-size: 24px;
            cursor: pointer;
            padding: 5px;
        }

        /* Menu de botões (Home e Stock) */
        .menu-botoes {
            display: flex;
            gap: 5px;
        }

        /* Esconder os botões Home e Stock em telas pequenas */
        @media (max-width: 768px) {
            .menu-hamburguer {
                display: block;
            }

            .menu-botoes {
                display: none;
                flex-direction: column;
                background-color: #4CAF50;
                padding: 10px;
                border-radius: 5px;
                position: absolute;
                top: 50px;
                right: 10px;
                z-index: 1000;
                width: 120px;
            }

            .menu-botoes.aberto {
                display: flex;
            }
            
            .barra-superior .botoes {
                max-width: 60%;
            }
            
            .barra-superior h1 {
                max-width: 40%;
                font-size: 18px;
            }
        }

        /* Container principal */
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 500px;
            margin-top: 60px;
        }

        /* Input e botão de adicionar */
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .input-group input {
            flex: 1;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        .input-group button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }

        .input-group button:hover {
            background-color: #45a049;
        }

        /* Listas e tarefas */
        .lista, .tarefa {
            background-color: #f8f9fa;
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 5px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: background-color 0.3s ease;
        }

        .lista:hover, .tarefa:hover {
            background-color: #e9ecef;
        }

        .tarefa-texto {
            flex: 1;
            margin: 0 10px;
            word-wrap: break-word;
        }

        .tarefa-concluida {
            text-decoration: line-through;
            color: #888;
        }

        .botoes-tarefa {
            display: flex;
            gap: 10px;
        }

        .botao-riscar, .botao-apagar, .botao-mover {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 16px;
            color: #4CAF50;
        }

        .botao-apagar {
            color: #ff4444;
        }

        .botao-mover {
            color: #007bff;
        }

        /* Responsividade */
        @media (max-width: 768px) {
            .container {
                width: 100%;
                margin-top: 50px;
            }

            .input-group {
                flex-direction: column;
            }

            .input-group button {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <!-- Barra superior -->
    <div class="barra-superior">
        <h1>Listador de Tarefas</h1>
        <div class="botoes">
            <!-- Botão Hamburguer (visível apenas em celulares) -->
            <button class="menu-hamburguer" onclick="toggleMenu()">☰</button>
            <!-- Botões Home e Stock (ocultos em celulares por padrão) -->
            <div id="menu-botoes" class="menu-botoes">
                <button onclick="irParaHome()">Home</button>
                <button onclick="mostrarMensagemStock()">Stock</button>
            </div>
        </div>
    </div>

    <!-- Tela de Listas -->
    <div id="tela-listas" class="container">
        <h2>Minhas Listas</h2>
        <div class="input-group">
            <input type="text" id="nova-lista" placeholder="Nome da nova lista">
            <button onclick="criarLista()">Criar Lista</button>
        </div>
        <ul id="listas"></ul>
    </div>

    <!-- Tela de Tarefas -->
    <div id="tela-tarefas" class="container" style="display: none;">
        <h2 id="titulo-lista"></h2>
        <div class="input-group">
            <input type="text" id="tarefa" placeholder="Escreva sua tarefa..." onkeypress="adicionarComEnter(event)">
            <button onclick="adicionarTarefa()">Adicionar</button>
        </div>
        <ul id="lista-tarefas"></ul>
        <button onclick="irParaHome()" style="width: 100%; margin-top: 10px;">Voltar para Listas</button>
    </div>

    <script>
        // Dados das listas e tarefas
        let listas = [];
        let listaAtual = null;

        // Função para salvar as listas no localStorage
        function salvarListas() {
            localStorage.setItem('listas', JSON.stringify(listas));
        }

        // Função para carregar as listas do localStorage
        function carregarListas() {
            const listasSalvas = localStorage.getItem('listas');
            if (listasSalvas) {
                listas = JSON.parse(listasSalvas);
            }
        }

        // Carregar as listas ao iniciar a página
        carregarListas();

        // Função para alternar a visibilidade do menu de botões
        function toggleMenu() {
            const menuBotoes = document.getElementById('menu-botoes');
            menuBotoes.classList.toggle('aberto');
        }

        // Função para o botão Home
        function irParaHome() {
            document.getElementById('tela-listas').style.display = 'block';
            document.getElementById('tela-tarefas').style.display = 'none';
            atualizarListas();
        }

        // Função para o botão Stock
        function mostrarMensagemStock() {
            alert("Não sei o que stock significa.");
            // Esconde o menu após clicar
            toggleMenu();
        }

        // Função para criar uma nova lista
        function criarLista() {
            const input = document.getElementById('nova-lista');
            const nomeLista = input.value.trim();

            if (nomeLista === '') {
                alert('Por favor, escreva um nome para a lista!');
                return;
            }

            // Adiciona a nova lista
            listas.push({ nome: nomeLista, tarefas: [] });
            input.value = '';
            salvarListas();
            atualizarListas();
        }

        // Função para atualizar a exibição das listas
        function atualizarListas() {
            const ul = document.getElementById('listas');
            ul.innerHTML = '';

            listas.forEach((lista, index) => {
                const li = document.createElement('li');
                li.className = 'lista';
                li.innerHTML = `
                    <span class="tarefa-texto">${lista.nome}</span>
                    <div class="botoes-tarefa">
                        <button class="botao-apagar" onclick="apagarLista(${index}, event)">❌</button>
                    </div>
                `;
                li.onclick = () => abrirLista(index);
                ul.appendChild(li);
            });
        }

        // Função para apagar uma lista
        function apagarLista(index, event) {
            event.stopPropagation(); // Impede que o clique abra a lista
            if (confirm('Tem certeza que deseja apagar esta lista?')) {
                listas.splice(index, 1);
                salvarListas();
                atualizarListas();
            }
        }

        // Função para abrir uma lista de tarefas
        function abrirLista(index) {
            if (index < 0 || index >= listas.length) {
                alert("Índice de lista inválido!");
                return;
            }

            listaAtual = index;
            document.getElementById('tela-listas').style.display = 'none';
            document.getElementById('tela-tarefas').style.display = 'block';
            document.getElementById('titulo-lista').textContent = listas[index].nome;
            atualizarTarefas();
        }

        // Função para adicionar tarefa ao pressionar Enter
        function adicionarComEnter(event) {
            if (event.key === 'Enter') {
                adicionarTarefa();
            }
        }

        // Função para adicionar tarefa
        function adicionarTarefa() {
            if (listaAtual === null || listaAtual < 0 || listaAtual >= listas.length) {
                alert("Nenhuma lista selecionada!");
                return;
            }

            const input = document.getElementById('tarefa');
            const tarefa = input.value.trim();

            if (tarefa === '') {
                alert('Por favor, escreva uma tarefa!');
                return;
            }

            // Adiciona a tarefa à lista atual
            listas[listaAtual].tarefas.push({ texto: tarefa, concluida: false });
            input.value = '';
            salvarListas();
            atualizarTarefas();
        }

        // Função para atualizar a exibição das tarefas
        function atualizarTarefas() {
            if (listaAtual === null || listaAtual < 0 || listaAtual >= listas.length) {
                alert("Nenhuma lista selecionada!");
                return;
            }

            const ul = document.getElementById('lista-tarefas');
            ul.innerHTML = '';

            listas[listaAtual].tarefas.forEach((tarefa, index) => {
                const li = document.createElement('li');
                li.className = 'tarefa';
                li.innerHTML = `
                    <div class="botoes-tarefa">
                        <button class="botao-mover" onclick="moverTarefa(${index}, 'baixo', event)">⬇️</button>
                        <button class="botao-apagar" onclick="apagarTarefa(${index}, event)">❌</button>
                    </div>
                    <span class="tarefa-texto ${tarefa.concluida ? 'tarefa-concluida' : ''}">${tarefa.texto}</span>
                    <div class="botoes-tarefa">
                        <button class="botao-mover" onclick="moverTarefa(${index}, 'cima', event)">⬆️</button>
                        <button class="botao-riscar" onclick="riscarTarefa(${index}, event)">✔️</button>
                    </div>
                `;
                ul.appendChild(li);
            });
        }

        // Função para mover tarefa para cima ou para baixo
        function moverTarefa(index, direcao, event) {
            event.stopPropagation();
            if (listaAtual === null || listaAtual < 0 || listaAtual >= listas.length) {
                alert("Nenhuma lista selecionada!");
                return;
            }

            const tarefas = listas[listaAtual].tarefas;

            if (direcao === 'cima' && index > 0) {
                // Move para cima
                const temp = tarefas[index];
                tarefas[index] = tarefas[index - 1];
                tarefas[index - 1] = temp;
            } else if (direcao === 'baixo' && index < tarefas.length - 1) {
                // Move para baixo
                const temp = tarefas[index];
                tarefas[index] = tarefas[index + 1];
                tarefas[index + 1] = temp;
            }

            salvarListas();
            atualizarTarefas();
        }

        // Função para riscar a tarefa
        function riscarTarefa(index, event) {
            event.stopPropagation();
            if (listaAtual === null || listaAtual < 0 || listaAtual >= listas.length) {
                alert("Nenhuma lista selecionada!");
                return;
            }

            listas[listaAtual].tarefas[index].concluida = !listas[listaAtual].tarefas[index].concluida;
            salvarListas();
            atualizarTarefas();
        }

        // Função para apagar a tarefa
        function apagarTarefa(index, event) {
            event.stopPropagation();
            if (listaAtual === null || listaAtual < 0 || listaAtual >= listas.length) {
                alert("Nenhuma lista selecionada!");
                return;
            }

            if (confirm('Tem certeza que deseja apagar esta tarefa?')) {
                listas[listaAtual].tarefas.splice(index, 1);
                salvarListas();
                atualizarTarefas();
            }
        }

        // Inicializa a tela de listas
        irParaHome();
    </script>
</body>
</html>
