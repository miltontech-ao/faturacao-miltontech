<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SYDE SOFTWARE - Sistema de Faturação</title>
<style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
    body { 
        background: #0d1117; 
        color: #e6edf3; 
        padding: 20px;
    }
   .header { text-align: center; margin-bottom: 30px; }
   .header h1 { 
        font-size: 32px; 
        font-weight: 800; 
        letter-spacing: 1px;
    }
   .header.syde { color: #1e90ff; }
   .header.software { color: #00ff7f; }
   .header p { color: #8b949e; margin-top: 5px; font-size: 14px; }

   .card {
        background: #161b22;
        border-radius: 12px;
        padding: 20px;
        margin-bottom: 20px;
        border: 1px solid #30363d;
        max-width: 900px;
        margin: 20px auto;
    }
   .card h2 { font-size: 18px; margin-bottom: 5px; }
   .card.subtitle { color: #8b949e; font-size: 13px; margin-bottom: 15px; }

   .form-group { margin-bottom: 15px; }
   .form-group label { 
        display: block; 
        font-size: 14px; 
        margin-bottom: 6px; 
        color: #c9d1d9;
    }
   .form-group input {
        width: 100%;
        padding: 12px;
        background: #0d1117;
        border: 1px solid #30363d;
        border-radius: 8px;
        color: #e6edf3;
        font-size: 14px;
        outline: none;
    }
   .form-group input:focus {
        border-color: #1e90ff;
    }

    table { width: 100%; border-collapse: collapse; margin: 20px 0; }
    table, th, td { border: 1px solid #30363d; }
    th { background: #1e90ff; color: white; padding: 10px; font-size: 14px; }
    td { padding: 8px; text-align: center; background: #0d1117; }
    td input { width: 95%; background: transparent; border: none; color: #e6edf3; text-align: center; }

   .totals { text-align: right; font-size: 16px; margin-top: 20px; }
   .totals h2 { color: #00ff7f; font-size: 22px; }
    
   .btn {
        background: #238636;
        color: white;
        padding: 12px 20px;
        border: none;
        border-radius: 8px;
        font-weight: bold;
        cursor: pointer;
        width: 100%;
        font-size: 15px;
        margin-top: 10px;
    }
   .btn:hover { background: #2ea043; }
   .btn-danger { background: #da3633; }
   .btn-danger:hover { background: #f85149; }

    @media print {.btn,.btn-danger { display: none; } body { background: white; color: black; }.card { border: 1px solid #ccc; } }
</style>
</head>
<body>

    <div class="header">
        <h1><span class="syde">SYDE</span> <span class="software">SOFTWARE</span></h1>
        <p>Sistema de Faturação | Tel: 942 076 215</p>
    </div>

    <div class="card">
        <h2>Dados da Empresa</h2>
        <p class="subtitle">Informações da tua empresa</p>
        <div class="form-group">
            <label>Nome da Empresa</label>
            <input type="text" placeholder="Ex: SYDE SOFTWARE LDA" id="empresa">
        </div>
        <div class="form-group">
            <label>NIF</label>
            <input type="text" placeholder="5000123456" id="nifEmpresa">
        </div>
        <div class="form-group">
            <label>Telefone</label>
            <input type="text" placeholder="942 076 215" id="telEmpresa">
        </div>
        <div class="form-group">
            <label>Endereço</label>
            <input type="text" placeholder="Luanda, Angola" id="endEmpresa">
        </div>
    </div>

    <div class="card">
        <h2>Dados do Cliente</h2>
        <p class="subtitle">Informações do cliente</p>
        <div class="form-group">
            <label>Nome do Cliente</label>
            <input type="text" placeholder="Nome do Cliente" id="cliente">
        </div>
        <div class="form-group">
            <label>NIF do Cliente</label>
            <input type="text" placeholder="999" id="nifCliente">
        </div>
    </div>

    <div class="card">
        <h2>Itens da Factura</h2>
        <table id="tabelaItens">
            <thead>
                <tr>
                    <th>Descrição</th>
                    <th>Qtd</th>
                    <th>Preço Unit KZ</th>
                    <th>Subtotal KZ</th>
                    <th>Ação</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><input type="text" placeholder="Produto 1"></td>
                    <td><input type="number" value="1" oninput="calcular()"></td>
                    <td><input type="number" value="0" oninput="calcular()"></td>
                    <td class="subtotal">0.00</td>
                    <td><button class="btn-danger" onclick="removerLinha(this)">X</button></td>
                </tr>
            </tbody>
        </table>
        <button class="btn" onclick="adicionarLinha()">+ Adicionar Item</button>

        <div class="totals">
            <p>Subtotal: <span id="subtotal">0.00</span> KZ</p>
            <p>IVA 0%: <span id="iva">0.00</span> KZ</p>
            <h2>Total: <span id="total">0.00</span> KZ</h2>
        </div>

        <button class="btn" onclick="window.print()">🖨️ Gerar Factura / Salvar PDF</button>
    </div>

<script>
    function calcular() {
        let tabela = document.getElementById('tabelaItens').getElementsByTagName('tbody')[0];
        let linhas = tabela.rows;
        let subtotalGeral = 0;

        for(let i=0; i<linhas.length; i++) {
            let qtd = parseFloat(linhas[i].cells[1].children[0].value) || 0;
            let preco = parseFloat(linhas[i].cells[2].children[0].value) || 0;
            let sub = qtd * preco;
            linhas[i].cells[3].innerText = sub.toFixed(2);
            subtotalGeral += sub;
        }

        let iva = subtotalGeral * 0; // IVA 0%
        let total = subtotalGeral + iva;

        document.getElementById('subtotal').innerText = subtotalGeral.toFixed(2);
        document.getElementById('iva').innerText = iva.toFixed(2);
        document.getElementById('total').innerText = total.toFixed(2);
    }

    function adicionarLinha() {
        let tabela = document.getElementById('tabelaItens').getElementsByTagName('tbody')[0];
        let novaLinha = tabela.insertRow();
        novaLinha.innerHTML = `
            <td><input type="text" placeholder="Produto"></td>
            <td><input type="number" value="1" oninput="calcular()"></td>
            <td><input type="number" value="0" oninput="calcular()"></td>
            <td class="subtotal">0.00</td>
            <td><button class="btn-danger" onclick="removerLinha(this)">X</button></td>
        `;
    }

    function removerLinha(btn) {
        let linha = btn.parentNode.parentNode;
        linha.parentNode.removeChild(linha);
        calcular();
    }
</script>

</body>
</html>
