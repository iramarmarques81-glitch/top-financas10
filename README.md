<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Top+ Finanças Elite Unificado</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root { --gold: #d4af37; --primary: #27ae60; --danger: #c0392b; --dark: #121212; --blue: #2980b9; }
        body { margin: 0; font-family: 'Segoe UI', sans-serif; background: #f0f2f5; overflow-x: hidden; }

        /* --- CAMADA 1: CAPA (DASHBOARD) --- */
        #capa { padding: 15px; background: var(--dark); min-height: 100vh; color: white; display: block; }
        .header-capa { text-align: center; border-bottom: 2px solid var(--gold); padding-bottom: 10px; margin-bottom: 20px; }
        .card-capa { background: rgba(255,255,255,0.1); padding: 15px; border-radius: 15px; border-left: 5px solid var(--gold); margin-bottom: 15px; }
        .status-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; }
        
        /* --- CAMADA 2: LANÇAMENTOS (BACK) --- */
        #lancamentos { display: none; padding: 15px; background: white; min-height: 100vh; }
        .overlay-back { background: url('https://www.bcb.gov.br/content/cedulasmoedas/cedulaloreal/cedula_200_frente.jpg') no-repeat center center fixed; background-size: cover; padding: 15px; border-radius: 20px; min-height: 90vh; }

        /* TABELAS PROFISSIONAIS */
        .section-title { background: linear-gradient(90deg, var(--dark), var(--gold)); color: white; padding: 10px; border-radius: 8px; margin-top: 20px; font-size: 13px; }
        table { width: 100%; border-collapse: collapse; margin-top: 8px; font-size: 11px; background: white; }
        th { background: var(--dark); color: var(--gold); padding: 8px; }
        td { border: 1px solid #ddd; padding: 5px; text-align: center; }
        input { width: 90%; padding: 5px; border: 1px solid #ccc; border-radius: 4px; font-size: 11px; }

        /* BARRA DE PROGRESSO */
        .progress-container { background: #444; border-radius: 10px; height: 25px; margin-top: 10px; overflow: hidden; border: 1px solid var(--gold); }
        .progress-bar { background: linear-gradient(90deg, #b8860b, #d4af37); height: 100%; width: 0%; transition: 0.8s; text-align: center; line-height: 25px; color: black; font-weight: bold; }

        /* BOTÕES */
        .btn { padding: 15px; border-radius: 10px; border: none; font-weight: bold; cursor: pointer; width: 100%; margin-top: 10px; font-size: 14px; text-transform: uppercase; }
        .btn-edit { background: var(--primary); color: white; box-shadow: 0 4px 10px rgba(39, 174, 96, 0.4); }
        .btn-gold { background: var(--gold); color: black; }
        .btn-danger { background: var(--danger); color: white; margin-top: 40px; }

        .resumo-fixo { position: sticky; bottom: 0; background: var(--dark); color: white; padding: 15px; display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; border-radius: 20px 20px 0 0; border-top: 3px solid var(--gold); text-align: center; margin: 20px -15px -15px -15px; }
        footer { text-align: center; padding: 20px; color: var(--gold); font-weight: bold; font-size: 1.4em; }
    </style>
</head>
<body>

<div id="capa">
    <div class="header-capa">
        <h1 style="margin:0;">💎 TOP+ ELITE PRO</h1>
        <p id="data-atual" style="color: var(--gold); font-size: 14px;"></p>
    </div>

    <div class="status-grid">
        <div id="box-analista" class="card-capa" style="border-left-color: var(--primary); background: rgba(39,174,96,0.1);">
            <small>ANALISTA</small><br><b id="txt-analista">AZUL</b>
        </div>
        <div class="card-capa" style="border-left-color: var(--blue); background: rgba(41,128,185,0.1);">
            <small>RESERVA</small><br><b id="txt-reserva">R$ 0,00</b>
        </div>
    </div>

    <div class="card-capa">
        <h3>🎯 Meta de Investimento Anual</h3>
        <div class="progress-container">
            <div id="barra-progresso" class="progress-bar">0%</div>
        </div>
        <p id="status-meta" style="font-size: 11px; margin-top: 8px; text-align: center;"></p>
    </div>

    <div class="card-capa" style="height: 220px; background: rgba(255,255,255,0.05);">
        <canvas id="graficoDashboard"></canvas>
    </div>

    <button class="btn btn-edit" onclick="abrirModulo('lancamentos')">🖊️ ACESSAR LANÇAMENTOS / EDITAR</button>
    <button class="btn btn-gold" style="margin-top:15px;" onclick="window.print()">📥 GERAR RELATÓRIO PDF</button>

    <div class="footer">DEUS SEJA LOUVADO</div>
</div>

<div id="lancamentos">
    <div class="overlay-back">
        <button class="btn btn-gold" onclick="abrirModulo('capa')">⬅️ VOLTAR PARA O DASHBOARD</button>

        <h2 style="text-align:center; color: var(--dark); text-shadow: 1px 1px white;">⚙️ CENTRAL DE CONTROLO</h2>
        
        <p><b>Definir Meta de Reserva (R$):</b></p>
        <input type="number" id="meta-anual" class="v-save" placeholder="Ex: 5000" oninput="calc()" style="width:95%; padding:10px; border:2px solid var(--gold);">

        <h3 class="section-title">🌅 Entradas Diárias (Manhã e Tarde)</h3>
        <div style="overflow-x:auto">
            <table>
                <thead><tr><th>Data</th><th>Manhã (R$)</th><th>Tarde (R$)</th></tr></thead>
                <tbody id="tab-turnos"></tbody>
            </table>
        </div>

        <h3 class="section-title">⚡ Movimentação Extra (Entrada/Saída)</h3>
        <div style="overflow-x:auto">
            <table>
                <thead><tr><th>Data</th><th>Entrada (R$)</th><th>Desc.</th><th>Saída (R$)</th><th>Desc.</th></tr></thead>
                <tbody id="tab-extra"></tbody>
            </table>
        </div>

        <h3 class="section-title">🎒 Materiais, Equipamentos e Gastos Fixos</h3>
        <div style="overflow-x:auto">
            <table>
                <thead><tr><th>Data</th><th>Descrição</th><th>Valor (R$)</th></tr></thead>
                <tbody id="tab-gastos"></tbody>
            </table>
        </div>

        <h3 class="section-title">🏛️ Cofrinho e Investimentos Mensais</h3>
        <div style="overflow-x:auto">
            <table>
                <thead><tr><th>Mês</th><th>Cofrinho</th><th>Investimento</th></tr></thead>
                <tbody id="tab-invest"></tbody>
            </table>
        </div>

        <button class="btn btn-danger" onclick="limparTudo()">🗑️ ZERAR TODOS OS DADOS</button>
        
        <div class="resumo-fixo">
            <div>ENTRADAS<br><span id="total-e" style="color:var(--primary)">0.00</span></div>
            <div>SAÍDAS<br><span id="total-s" style="color:var(--danger)">0.00</span></div>
            <div>SALDO<br><span id="total-l" style="color:var(--gold)">0.00</span></div>
        </div>
    </div>
</div>

<script>
    let meuGrafico;
    const meses = ["Jan", "Fev", "Mar", "Abr", "Mai", "Jun", "Jul", "Ago", "Set", "Out", "Nov", "Dez"];

    function abrirModulo(id) {
        document.getElementById('capa').style.display = id === 'capa' ? 'block' : 'none';
        document.getElementById('lancamentos').style.display = id === 'lancamentos' ? 'block' : 'none';
    }

    function init() {
        document.getElementById('data-atual').innerText = new Date().toLocaleDateString('pt-BR', { weekday: 'long', day: 'numeric', month: 'long' });
        
        const turnos = document.getElementById('tab-turnos');
        const extra = document.getElementById('tab-extra');
        const gastos = document.getElementById('tab-gastos');
        const invest = document.getElementById('tab-invest');

        for(let i=0; i<7; i++) {
            turnos.innerHTML += `<tr><td><input type="date" class="v-save"></td><td><input type="number" class="v-ent v-save" oninput="calc()"></td><td><input type="number" class="v-ent v-save" oninput="calc()"></td></tr>`;
            extra.innerHTML += `<tr><td><input type="date" class="v-save"></td><td><input type="number" class="v-ent v-save" oninput="calc()"></td><td><input type="text" class="v-save"></td><td><input type="number" class="v-sai v-save" oninput="calc()"></td><td><input type="text" class="v-save"></td></tr>`;
            gastos.innerHTML += `<tr><td><input type="date" class="v-save"></td><td><input type="text" class="v-save" placeholder="Ex: Aluguel"></td><td><input type="number" class="v-sai v-save" oninput="calc()"></td></tr>`;
        }
        meses.forEach(m => {
            invest.innerHTML += `<tr><td>${m}</td><td><input type="number" class="v-inv v-save" oninput="calc()"></td><td><input type="number" class="v-inv v-save" oninput="calc()"></td></tr>`;
        });

        initChart();
        carregar();
        calc();
    }

    function initChart() {
        const ctx = document.getElementById('graficoDashboard').getContext('2d');
        meuGrafico = new Chart(ctx, {
            type: 'line',
            data: { labels: ['S', 'T', 'Q', 'Q', 'S', 'S', 'D'], datasets: [{ label: 'Fluxo', data: [0,0,0,0,0,0,0], borderColor: '#d4af37', backgroundColor: 'rgba(212,171,55,0.2)', fill: true, tension: 0.4 }] },
            options: { maintainAspectRatio: false, plugins: { legend: { display: false } }, scales: { y: { grid: { color: '#333' }, ticks: { color: '#aaa' } }, x: { ticks: { color: '#aaa' } } } }
        });
    }

    function calc() {
        let ent = 0, sai = 0, inv = 0;
        document.querySelectorAll('.v-ent').forEach(i => ent += parseFloat(i.value || 0));
        document.querySelectorAll('.v-sai').forEach(i => sai += parseFloat(i.value || 0));
        document.querySelectorAll('.v-inv').forEach(i => inv += parseFloat(i.value || 0));

        let totalSaida = sai + inv;
        let saldo = ent - totalSaida;

        // Atualizar Capa
        document.getElementById('total-e').innerText = `R$ ${ent.toFixed(2)}`;
        document.getElementById('total-s').innerText = `R$ ${totalSaida.toFixed(2)}`;
        document.getElementById('total-l').innerText = `R$ ${saldo.toFixed(2)}`;
        document.getElementById('txt-reserva').innerText = `R$ ${inv.toFixed(2)}`;

        // Meta
        let metaVal = parseFloat(document.getElementById('meta-anual').value || 0);
        let perc = metaVal > 0 ? (inv / metaVal) * 100 : 0;
        document.getElementById('barra-progresso').style.width = Math.min(perc, 100) + "%";
        document.getElementById('barra-progresso').innerText = Math.round(perc) + "%";
        document.getElementById('status-meta').innerText = `Faltam R$ ${Math.max(0, metaVal - inv).toFixed(2)} para a sua meta!`;

        // Analista
        const analista = document.getElementById('box-analista');
        if(saldo < 0) {
            analista.style.background = "rgba(192,57,43,0.2)";
            document.getElementById('txt-analista').innerText = "VERMELHO";
            document.getElementById('txt-analista').style.color = "#e74c3c";
        } else {
            analista.style.background = "rgba(39,174,96,0.2)";
            document.getElementById('txt-analista').innerText = "AZUL";
            document.getElementById('txt-analista').style.color = "#2ecc71";
        }

        salvar();
    }

    function salvar() {
        const dados = Array.from(document.querySelectorAll('.v-save')).map(i => i.value);
        localStorage.setItem('topFinancasPro_DB', JSON.stringify(dados));
    }

    function carregar() {
        const memoria = JSON.parse(localStorage.getItem('topFinancasPro_DB'));
        if(memoria) {
            const inputs = document.querySelectorAll('.v-save');
            memoria.forEach((val, index) => { if(inputs[index]) inputs[index].value = val; });
        }
    }

    function limparTudo() {
        if(confirm("Deseja apagar todos os dados para começar de novo?")) {
            localStorage.clear();
            location.reload();
        }
    }

    init();
</script>
</body>
</html>
