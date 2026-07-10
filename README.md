<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Programa de Factura</title>
<style>
    body { font-family: Arial, sans-serif; margin: 20px; background: #f5f5f5; }
   .invoice { background: white; padding: 30px; max-width: 800px; margin: auto; box-shadow: 0 0 10px #ccc; }
   .header { text-align: center; border-bottom: 2px solid #333; padding-bottom: 10px; margin-bottom: 20px; }
   .header h1 { margin: 0; color: #333; }
   .info { display: flex; justify-content: space-between; margin-bottom: 20px; }
   .info div { width: 48%; }
    table { width: 100%; border-collapse: collapse; margin-bottom: 20px; }
    table, th, td { border: 1px solid #ddd; }
    th { background: #333; color: white; padding: 10px; }
    td { padding: 8px; text-align: center; }
    input { width: 90%; padding: 5px; border: 1px solid #ccc; }
   .totals { text-align: right; font-size: 16px; }
   .btn { background: #28a745; color: white; padding: 10px 20px; border: none; cursor: pointer; margin-top: 10px; }
   .btn:hover { background: #218838; }
    @media print {.btn { display: none; } }
</style>
</head>
<body>

<div class="invoice">
    <div class="header">
        <h1>FACTURA</h1>
        <p><b>Minha Empresa Lda</b><br>
        Luanda, Angola | NIF: 000000000<br>
        Tel: +244 900 000 000</p>
    </div>

    <div class="info">
        <div>
            <h3>Dados do Cliente</h3>
            <input type="text" id="cliente" placeholder="Nome do Cliente"><br><br>
            <input type="text" id="endereco" placeholder="Endereço"><br><br>
            <input type="text" id="nif" placeholder="NIF do Cliente">
        </div>
        <div>
            <h3>Dados da Factura</h3>
            Nº: <input type="text" id="numFactura" value="FAT 001/2026"><br><br>
            Data: <input type="date" id="data">
        </div>
    </div>

    <table id="tabelaItens">
        <thead>
            <tr>
                <th>Descrição</th>
                <th>Qtd</th>
                <th>Preço Unit KZ</th>
                <th>Subtotal KZ</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><input type="text" placeholder="Produto 1"></td>
                <td><input type="number" value="1" oninput="calcular()"></td>
                <td><input type="number" value="0" oninput="calcular()"></td>
                <td class="subtotal">0.00</td>
                <td><button onclick="removerLinha(this)">X</button></td>
            </tr>
        </tbody>
    </table>

    <button class="btn" onclick="adicionarLinha()">+ Adicionar Item</button>

    <div class="totals">
        <p>Subtotal: <span id="subtotal">0.00</span> KZ</p>
        <p>IVA 14%: <span id="iva">0.00</span> KZ</p>
        <h2>Total: <span id="total">0.00</span> KZ</h2>
    </div>

    <button class="btn" onclick="window.print()">🖨️ Imprimir / Salvar PDF</button>
</div>

<script>
    // Coloca data de hoje
    document.getElementById('data').valueAsDate = new Date();

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

        let iva = subtotalGeral * 0.14;
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
            <td><button onclick="removerLinha(this)">X</button></td>
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
