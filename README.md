# porsche-sales-dashboard
Dashboard interativo de inteligência de vendas da Porsche - Projeto DIO.
<img width="1487" height="755" alt="PORSCHE PERFORMANCE 2" src="https://github.com/user-attachments/assets/d380091b-a06e-4ac9-87b7-378ffd434eb4" />
<img width="1548" height="694" alt="PORSCHE PERFORMANCE 1" src="https://github.com/user-attachments/assets/3424a996-b1e2-4892-baa5-638da70d4a0d" />
[index.html.html](https://github.com/user-attachments/files/34929440/index.html.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Porsche | Sales Intelligence</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
:root{
  --bg:#0b0c0e;
  --panel:#141619;
  --panel2:#1b1e22;
  --line:#2a2e33;
  --muted:#9299a2;
  --text:#f4f5f6;
  --red:#d5001c;
  --red2:#ff334b;
  --gold:#c9a66b;
}
*{box-sizing:border-box}
body{
  margin:0;
  background:var(--bg);
  color:var(--text);
  font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",sans-serif;
  font-size:14px;
}
.wrap{max-width:1500px;margin:auto;padding:28px 34px 44px}
header{
  display:flex;
  align-items:center;
  justify-content:space-between;
  border-bottom:1px solid var(--line);
  padding:4px 0 24px;
  margin-bottom:26px;
}
.brand{display:flex;align-items:center;gap:15px}
.crest{
  width:48px;height:48px;
  border:1px solid #777;
  border-radius:5px;
  display:grid;place-items:center;
  font-size:11px;font-weight:900;letter-spacing:-1px;
  color:#fff;
  background:linear-gradient(145deg,#272a2e,#111);
  box-shadow:0 4px 14px rgba(213,0,28,.18);
}
.kicker{font-size:10px;letter-spacing:3px;color:var(--red2);font-weight:800;text-transform:uppercase}
.brand h1{font-size:18px;letter-spacing:2px;margin:3px 0 0;font-weight:800}
.brand p{margin:4px 0 0;color:var(--muted);font-size:11px;letter-spacing:1px}
.header-right{text-align:right;color:var(--muted);font-size:11px;letter-spacing:1.3px}
.header-right strong{display:block;color:#eee;font-size:12px;margin-top:5px}
.intro{display:flex;justify-content:space-between;align-items:end;margin:0 0 20px;gap:20px}
.intro h2{font-size:26px;letter-spacing:.5px;margin:0 0 6px;font-weight:650}
.intro p{margin:0;color:var(--muted);font-size:12px}
.period{border:1px solid var(--line);padding:9px 12px;border-radius:3px;color:#c7cbd0;font-size:11px;white-space:nowrap}

.filters{
  display:grid;
  grid-template-columns:1.3fr .8fr 1fr 1fr auto;
  gap:12px;
  padding:16px;
  background:var(--panel);
  border:1px solid var(--line);
  border-radius:5px;
  margin-bottom:18px;
}
.field label{display:block;color:var(--muted);font-size:10px;letter-spacing:1.5px;text-transform:uppercase;margin:0 0 8px}
select{
  width:100%;
  background:#0f1113;
  color:#f1f1f1;
  border:1px solid #353a40;
  padding:11px 12px;
  border-radius:3px;
  font-size:12px;
  outline:none;
}
select:focus{border-color:var(--red)}
.reset{
  align-self:end;
  padding:11px 16px;
  background:transparent;
  border:1px solid #555;
  color:#ddd;
  border-radius:3px;
  cursor:pointer;
  font-size:11px;
  letter-spacing:1px;
  text-transform:uppercase;
  transition:.2s;
}
.reset:hover{border-color:var(--red);color:#fff;background:rgba(213,0,28,.12)}

.kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:18px}
.kpi{
  background:var(--panel);
  border:1px solid var(--line);
  border-radius:5px;
  padding:18px 20px;
  position:relative;
  overflow:hidden;
}
.kpi:before{content:"";position:absolute;top:0;left:0;width:3px;height:100%;background:var(--red)}
.kpi .label{font-size:10px;color:var(--muted);letter-spacing:1.5px;text-transform:uppercase}
.kpi .value{font-size:27px;font-weight:650;margin:10px 0 5px;letter-spacing:-.7px}
.kpi .sub{font-size:11px;color:var(--muted)}

.grid2{display:grid;grid-template-columns:1.15fr .85fr;gap:16px;margin-bottom:16px}
.grid2-even{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px}
.grid3{display:grid;grid-template-columns:1.1fr 1.1fr .8fr;gap:16px;margin-bottom:16px}

.panel{
  background:var(--panel);
  border:1px solid var(--line);
  border-radius:5px;
  padding:19px 20px;
  min-width:0;
}
.panel-head{display:flex;justify-content:space-between;align-items:start;gap:10px;margin-bottom:16px}
.panel h3{font-size:13px;letter-spacing:1px;text-transform:uppercase;margin:0;font-weight:700}
.panel .desc{font-size:11px;color:var(--muted);margin-top:5px}
.tag{font-size:9px;color:#ff6476;border:1px solid #61202a;padding:5px 7px;letter-spacing:1px;white-space:nowrap}
.tag.gold{color:var(--gold);border-color:#5b4a2e}

.chart{width:100%;min-height:230px}
.canvas-box{position:relative;height:280px;width:100%}
.canvas-box.donut{height:280px;display:flex;align-items:center;justify-content:center}

.barrow{display:grid;grid-template-columns:minmax(95px,1.2fr) 3fr 44px;align-items:center;gap:10px;margin:13px 0;font-size:11px}
.barlabel{color:#d5d8dc;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.track{height:8px;background:#292d32;border-radius:2px;overflow:hidden}
.fill{height:100%;background:linear-gradient(90deg,var(--red),#ff5366);border-radius:2px}
.barval{text-align:right;color:#c8cbd0;font-variant-numeric:tabular-nums}

.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:11px}
th{text-align:left;color:var(--muted);font-weight:600;letter-spacing:1px;text-transform:uppercase;font-size:9px;padding:10px;border-bottom:1px solid #343940}
td{padding:11px 10px;border-bottom:1px solid #262a2f;color:#e2e4e7}
td.num{text-align:right;font-variant-numeric:tabular-nums}
.pill{border:1px solid #444;padding:3px 7px;border-radius:2px;color:#ddd;font-size:9px}
.pill.delivered{border-color:#265b3d;color:#6ee7a4;background:rgba(38,91,61,.16)}
.pill.cancelled{border-color:#6e242d;color:#ff7b8a;background:rgba(110,36,45,.16)}

.insights{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.insight{background:var(--panel2);border:1px solid #30353b;padding:15px;border-radius:4px}
.insight .eyebrow{font-size:9px;letter-spacing:1.5px;color:var(--red2);text-transform:uppercase}
.insight strong{display:block;font-size:14px;margin:8px 0 5px}
.insight p{font-size:11px;line-height:1.55;color:#aeb4bb;margin:0}

.foot{border-top:1px solid var(--line);margin-top:22px;padding-top:14px;color:#777f88;font-size:10px;line-height:1.6;display:flex;justify-content:space-between;gap:15px}
.empty{color:var(--muted);font-size:12px;padding:25px;text-align:center}

@media(max-width:1100px){
  .grid3{grid-template-columns:1fr}
}
@media(max-width:950px){
  .filters{grid-template-columns:repeat(2,1fr)}
  .reset{grid-column:span 2}
  .grid2,.grid2-even{grid-template-columns:1fr}
  .kpis{grid-template-columns:repeat(2,1fr)}
  .insights{grid-template-columns:1fr}
}
@media(max-width:600px){
  .wrap{padding:18px 14px}
  .header-right{display:none}
  .intro{display:block}
  .period{display:inline-block;margin-top:12px}
  .filters{grid-template-columns:1fr}
  .reset{grid-column:auto}
  .kpis{gap:8px}
  .kpi{padding:14px}
  .kpi .value{font-size:21px}
  .barrow{grid-template-columns:90px 1fr 30px}
  .foot{display:block}
}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div class="brand">
      <div class="crest">PORSCHE</div>
      <div>
        <div class="kicker">Sales intelligence</div>
        <h1>PORSCHE PERFORMANCE</h1>
        <p>BUSINESS INTELLIGENCE · SALES OVERVIEW</p>
      </div>
    </div>
    <div class="header-right">EXECUTIVE DASHBOARD<strong>DATASET · 100 TRANSAÇÕES</strong></div>
  </header>

  <section class="intro">
    <div>
      <h2>Visão geral de vendas</h2>
      <p>Desempenho comercial, faturamento por modelo e distribuição geográfica por mercado.</p>
    </div>
    <div class="period">BASE ANALISADA · TODOS OS REGISTROS</div>
  </section>

  <!-- 4 FILTROS INTERATIVOS -->
  <section class="filters">
    <div class="field">
      <label for="model">Modelo Porsche</label>
      <select id="model"><option value="">Todos os modelos</option></select>
    </div>
    <div class="field">
      <label for="year">Model year</label>
      <select id="year"><option value="">Todos os anos</option></select>
    </div>
    <div class="field">
      <label for="city">City</label>
      <select id="city"><option value="">Todas as cidades</option></select>
    </div>
    <div class="field">
      <label for="pay">Pay method</label>
      <select id="pay"><option value="">Todas as formas</option></select>
    </div>
    <button class="reset" id="reset">Limpar filtros ↺</button>
  </section>

  <!-- KPIs -->
  <section class="kpis">
    <div class="kpi">
      <div class="label">Total de vendas</div>
      <div class="value" id="sales">—</div>
      <div class="sub" id="salesSub">Unidades comercializadas</div>
    </div>
    <div class="kpi">
      <div class="label">Receita total</div>
      <div class="value" id="revenue">—</div>
      <div class="sub">Soma do valor das vendas · USD</div>
    </div>
    <div class="kpi">
      <div class="label">Ticket médio</div>
      <div class="value" id="ticket">—</div>
      <div class="sub">Receita média por transação</div>
    </div>
    <div class="kpi">
      <div class="label">Model year líder</div>
      <div class="value" id="topYear">—</div>
      <div class="sub" id="topYearSub">Ano com maior volume de vendas</div>
    </div>
  </section>

  <!-- NOVOS GRÁFICOS INTERATIVOS (RECEITA POR MODELO, VENDAS POR CIDADE E DONUT DE PAGAMENTO) -->
  <section class="grid3">
    <div class="panel">
      <div class="panel-head">
        <div>
          <h3>Receita por modelo (USD)</h3>
          <div class="desc">Top modelos que geram maior faturamento</div>
        </div>
        <span class="tag gold">REVENUE</span>
      </div>
      <div class="canvas-box">
        <canvas id="revenueModelCanvas"></canvas>
      </div>
    </div>

    <div class="panel">
      <div class="panel-head">
        <div>
          <h3>Vendas por cidade</h3>
          <div class="desc">Comparativo de volume nas principais praças</div>
        </div>
        <span class="tag">CITIES</span>
      </div>
      <div class="canvas-box">
        <canvas id="cityVolumeCanvas"></canvas>
      </div>
    </div>

    <div class="panel">
      <div class="panel-head">
        <div>
          <h3>Formas de pagamento</h3>
          <div class="desc">Participação percentual por meio de pagamento</div>
        </div>
        <span class="tag">DONUT MIX</span>
      </div>
      <div class="canvas-box donut">
        <canvas id="payDonutCanvas"></canvas>
      </div>
    </div>
  </section>

  <!-- GRÁFICOS DE BARRAS DE VOLUME -->
  <section class="grid2">
    <div class="panel">
      <div class="panel-head">
        <div>
          <h3>Modelos mais vendidos (Volume)</h3>
          <div class="desc">Ranking por quantidade de unidades comercializadas</div>
        </div>
        <span class="tag">TOP 8</span>
      </div>
      <div id="modelChart" class="chart"></div>
    </div>

    <div class="panel">
      <div class="panel-head">
        <div>
          <h3>Vendas por model year</h3>
          <div class="desc">Distribuição das unidades por ano-modelo</div>
        </div>
        <span class="tag">VOLUME</span>
      </div>
      <div id="yearChart" class="chart"></div>
    </div>
  </section>

  <!-- TABELA DE PERFORMANCE POR CIDADE -->
  <section class="panel" style="margin-bottom:16px">
    <div class="panel-head">
      <div>
        <h3>Performance detalhada por cidade</h3>
        <div class="desc">Cidades com maior volume, modelo mais recorrente e receita acumulada</div>
      </div>
      <span class="tag">CITY INSIGHTS</span>
    </div>
    <div id="cityTable" class="table-wrap"></div>
  </section>

  <!-- INSIGHTS AUTOMÁTICOS -->
  <section class="panel" style="margin-bottom:16px">
    <div class="panel-head">
      <div>
        <h3>Insights de negócio</h3>
        <div class="desc">Leituras automáticas recalculadas conforme os filtros selecionados</div>
      </div>
      <span class="tag">LIVE ANALYSIS</span>
    </div>
    <div class="insights" id="insights"></div>
  </section>

  <!-- TABELA COMPLETA -->
  <section class="panel">
    <div class="panel-head">
      <div>
        <h3>Detalhamento das vendas</h3>
        <div class="desc">Registros considerados na seleção atual</div>
      </div>
      <span class="tag" id="rowCount">100 REGISTROS</span>
    </div>
    <div class="table-wrap">
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Modelo</th>
            <th>Model year</th>
            <th>Cidade</th>
            <th>Pagamento</th>
            <th>Status</th>
            <th style="text-align:right">Valor (USD)</th>
          </tr>
        </thead>
        <tbody id="detailRows"></tbody>
      </table>
    </div>
  </section>

  <footer class="foot">
    <span>Fonte: arquivo fornecido · aba “Sanitized”. Valores apresentados na moeda original da planilha (USD).</span>
    <span>Uso gerencial · Porsche Sales Intelligence</span>
  </footer>
</div>

<script>
const DATA = [
  {"id": 6, "date": "INVALID", "model": "718 Cayman", "year": 2022, "price": 79500.0, "mileage": 9800, "pay": "Credit Card", "city": "Boston", "state": "MA", "status": "Delivered"},
  {"id": 7, "date": "2024-03-14", "model": "911 Turbo S", "year": 2024, "price": 235000.0, "mileage": 1200, "pay": "Wire Transfer", "city": "Seattle", "state": "WA", "status": "Delivered"},
  {"id": 8, "date": "2024-04-18", "model": "Cayenne Coupe", "year": 2023, "price": 112750.0, "mileage": 6400, "pay": "Financing", "city": "Austin", "state": "TX", "status": "In Transit"},
  {"id": 9, "date": "INVALID", "model": "Macan S", "year": 2021, "price": 68900.0, "mileage": 28, "pay": "Cash", "city": "Denver", "state": "CO", "status": "Pending"},
  {"id": 10, "date": "2024-05-22", "model": "Taycan 4S", "year": 2024, "price": 121000.0, "mileage": 0, "pay": "Bank Transfer", "city": "Los Angeles", "state": "CA", "status": "Delivered"},
  {"id": 11, "date": "2024-08-06", "model": "Panamera 4", "year": 2023, "price": 104500.0, "mileage": 14500, "pay": "Credit Card", "city": "Miami", "state": "FL", "status": "Cancelled"},
  {"id": 12, "date": "2024-07-11", "model": "911 Carrera S", "year": 2020, "price": 96300.0, "mileage": 41000, "pay": "Lease", "city": "New York", "state": "NY", "status": "Delivered"},
  {"id": 13, "date": "INVALID", "model": "Cayenne E-Hybrid", "year": 2022, "price": 89750.0, "mileage": 11744, "pay": "Wire Transfer", "city": "San Diego", "state": "CA", "status": "Pending Approval"},
  {"id": 14, "date": "2024-08-19", "model": "718 Boxster", "year": 2021, "price": 73500.0, "mileage": 22300, "pay": "Debit Card", "city": "Chicago", "state": "IL", "status": "Shipped"},
  {"id": 15, "date": "2024-09-02", "model": "Macan GTS", "year": 2024, "price": 95000.0, "mileage": 3500, "pay": "Financing", "city": "Phoenix", "state": "AZ", "status": "In Transit"},
  {"id": 16, "date": "2024-09-17", "model": "Taycan Turbo", "year": 2023, "price": 153200.5, "mileage": 11, "pay": "ACH Payment", "city": "Dallas", "state": "TX", "status": "Delivered"},
  {"id": 17, "date": "INVALID", "model": "911 GT3", "year": 2024, "price": 241000.0, "mileage": 750, "pay": "Wire Transfer", "city": "Las Vegas", "state": "NV", "status": "Pending"},
  {"id": 18, "date": "2024-05-11", "model": "Panamera Turbo S", "year": 2022, "price": 132000.0, "mileage": 19250, "pay": "Cash", "city": "San Jose", "state": "CA", "status": "Delivered"},
  {"id": 19, "date": "2024-12-12", "model": "Cayenne Turbo GT", "year": 2024, "price": 188000.0, "mileage": 2100, "pay": "Crypto Payment", "city": "Houston", "state": "TX", "status": "Awaiting Delivery"},
  {"id": 20, "date": "2024-12-25", "model": "911 Carrera Cabriolet", "year": 2023, "price": 127800.0, "mileage": 12000, "pay": "Credit Card", "city": "Atlanta", "state": "GA", "status": "Delivered"},
  {"id": 21, "date": "2025-01-06", "model": "Macan", "year": 2021, "price": 58900.0, "mileage": 33700, "pay": "Bank Transfer", "city": "Orlando", "state": "FL", "status": "Pending"},
  {"id": 22, "date": "INVALID", "model": "718 Spyder RS", "year": 2024, "price": 164000.0, "mileage": 900, "pay": "Financing", "city": "Portland", "state": "OR", "status": "In Transit"},
  {"id": 23, "date": "2025-02-14", "model": "Taycan Cross Turismo", "year": 2023, "price": 118500.0, "mileage": 7800, "pay": "Wire Transfer", "city": "Charlotte", "state": "NC", "status": "Delivered"},
  {"id": 24, "date": "INVALID", "model": "Cayenne S", "year": 2022, "price": 91300.0, "mileage": 16000, "pay": "Credit Card", "city": "Nashville", "state": "TN", "status": "Pending"},
  {"id": 25, "date": "2025-03-21", "model": "911 Targa 4S", "year": 2024, "price": 158750.0, "mileage": 2500, "pay": "Lease", "city": "Minneapolis", "state": "MN", "status": "Delivered"},
  {"id": 26, "date": "2025-03-28", "model": "Panamera", "year": 2020, "price": 72000.0, "mileage": 49000, "pay": "Bank Transfer", "city": "Philadelphia", "state": "PA", "status": "Cancelled"},
  {"id": 27, "date": "2025-04-09", "model": "Macan Electric", "year": 2025, "price": 86500.0, "mileage": 0, "pay": "Wire Transfer", "city": "San Antonio", "state": "TX", "status": "Delivered"},
  {"id": 28, "date": "INVALID", "model": "911 Dakar", "year": 2024, "price": 270000.0, "mileage": 1050, "pay": "Cash", "city": "Salt Lake City", "state": "UT", "status": "Awaiting Pickup"},
  {"id": 29, "date": "2025-05-12", "model": "Taycan GTS", "year": 2023, "price": 139000.0, "mileage": 6250, "pay": "Financing", "city": "Raleigh", "state": "NC", "status": "In Transit"},
  {"id": 30, "date": "2025-06-18", "model": "Cayenne", "year": 2021, "price": 76800.0, "mileage": 38400, "pay": "Credit Card", "city": "Detroit", "state": "MI", "status": "Delivered"},
  {"id": 31, "date": "INVALID", "model": "718 Cayman GT4 RS", "year": 2024, "price": 173600.0, "mileage": 400, "pay": "Wire Transfer", "city": "Columbus", "state": "OH", "status": "Pending"},
  {"id": 32, "date": "2025-07-07", "model": "911 Carrera GTS", "year": 2022, "price": 119900.0, "mileage": 13600, "pay": "Cash", "city": "Indianapolis", "state": "IN", "status": "Delivered"},
  {"id": 33, "date": "2025-07-22", "model": "Panamera 4 E-Hybrid", "year": 2023, "price": 109250.0, "mileage": 8900, "pay": "Lease", "city": "Fort Worth", "state": "TX", "status": "In Transit"},
  {"id": 34, "date": "2025-08-14", "model": "Macan T", "year": 2022, "price": 82000.0, "mileage": 21750, "pay": "Wire Transfer", "city": "Jacksonville", "state": "FL", "status": "Delivered"},
  {"id": 35, "date": "2025-09-01", "model": "Taycan Turbo S", "year": 2025, "price": 214000.0, "mileage": 0, "pay": "Crypto Payment", "city": "San Diego", "state": "CA", "status": "Pending Review"},
  {"id": 36, "date": "INVALID", "model": "911 Carrera", "year": 2024, "price": 124500.0, "mileage": 4800, "pay": "Credit Card", "city": "Tampa", "state": "FL", "status": "Delivered"},
  {"id": 37, "date": "2025-09-18", "model": "Cayenne S", "year": 2023, "price": 98200.0, "mileage": 12200, "pay": "Bank Transfer", "city": "Sacramento", "state": "CA", "status": "Pending"},
  {"id": 38, "date": "2025-10-04", "model": "Macan", "year": 2022, "price": 67500.0, "mileage": 24100, "pay": "Financing", "city": "Cleveland", "state": "OH", "status": "Delivered"},
  {"id": 39, "date": "2025-10-12", "model": "Taycan", "year": 2025, "price": 116900.0, "mileage": 0, "pay": "Wire Transfer", "city": "Milwaukee", "state": "WI", "status": "In Transit"},
  {"id": 40, "date": "2025-10-29", "model": "Panamera 4S", "year": 2024, "price": 112000.0, "mileage": 10, "pay": "Cash", "city": "Kansas City", "state": "MO", "status": "Delivered"},
  {"id": 41, "date": "2025-02-11", "model": "718 Boxster", "year": 2021, "price": 74000.0, "mileage": 31000, "pay": "Debit Card", "city": "Omaha", "state": "NE", "status": "Cancelled"},
  {"id": 42, "date": "2025-11-16", "model": "911 Turbo", "year": 2024, "price": 198300.0, "mileage": 2400, "pay": "Wire Transfer", "city": "Albuquerque", "state": "NM", "status": "Awaiting Delivery"},
  {"id": 43, "date": "INVALID", "model": "Cayenne Coupe", "year": 2023, "price": 103750.0, "mileage": 6773, "pay": "Wire Transfer", "city": "Tucson", "state": "AZ", "status": "Pending Approval"},
  {"id": 44, "date": "INVALID", "model": "Macan GTS", "year": 2024, "price": 93600.0, "mileage": 5800, "pay": "Financing", "city": "Fresno", "state": "CA", "status": "Shipped"},
  {"id": 45, "date": "2025-12-07", "model": "Taycan 4S", "year": 2025, "price": 129000.0, "mileage": 0, "pay": "ACH Payment", "city": "Virginia Beach", "state": "VA", "status": "In Transit"},
  {"id": 46, "date": "2025-12-22", "model": "Panamera Turbo", "year": 2022, "price": 136000.0, "mileage": 18400, "pay": "Credit Card", "city": "Colorado Springs", "state": "CO", "status": "Delivered"},
  {"id": 47, "date": "INVALID", "model": "911 GT3 RS", "year": 2024, "price": 286500.0, "mileage": 700, "pay": "Wire Transfer", "city": "Arlington", "state": "TX", "status": "Pending"},
  {"id": 48, "date": "2026-01-08", "model": "Cayenne E-Hybrid", "year": 2023, "price": 92800.0, "mileage": 15000, "pay": "Lease", "city": "Bakersfield", "state": "CA", "status": "Delivered"},
  {"id": 49, "date": "2026-01-15", "model": "Macan T", "year": 2022, "price": 72400.0, "mileage": 19750, "pay": "Cash", "city": "Mesa", "state": "AZ", "status": "Awaiting Pickup"},
  {"id": 50, "date": "2026-01-28", "model": "Taycan Turbo", "year": 2025, "price": 158500.0, "mileage": 3200, "pay": "Crypto Payment", "city": "Atlanta", "state": "GA", "status": "Delivered"},
  {"id": 51, "date": "2026-02-03", "model": "718 Cayman", "year": 2021, "price": 69900.0, "mileage": 27600, "pay": "Bank Transfer", "city": "Long Beach", "state": "CA", "status": "Pending"},
  {"id": 52, "date": "INVALID", "model": "911 Targa 4", "year": 2024, "price": 141250.0, "mileage": 2100, "pay": "Financing", "city": "Oakland", "state": "CA", "status": "In Transit"},
  {"id": 53, "date": "2026-02-19", "model": "Panamera", "year": 2020, "price": 71500.0, "mileage": 52000, "pay": "Wire Transfer", "city": "Tulsa", "state": "OK", "status": "Delivered"},
  {"id": 54, "date": "2026-02-25", "model": "Cayenne Turbo", "year": 2023, "price": 146800.0, "mileage": 11300, "pay": "Credit Card", "city": "Wichita", "state": "KS", "status": "Pending"},
  {"id": 55, "date": "2026-03-01", "model": "Macan Electric", "year": 2025, "price": 89700.0, "mileage": 0, "pay": "Lease", "city": "New Orleans", "state": "LA", "status": "Delivered"},
  {"id": 56, "date": "2026-03-14", "model": "911 Carrera S", "year": 2022, "price": 104600.0, "mileage": 15900, "pay": "Bank Transfer", "city": "Honolulu", "state": "HI", "status": "Cancelled"},
  {"id": 57, "date": "INVALID", "model": "Taycan GTS", "year": 2024, "price": 142000.0, "mileage": 6600, "pay": "Wire Transfer", "city": "Anaheim", "state": "CA", "status": "Delivered"},
  {"id": 58, "date": "2026-04-08", "model": "Cayenne", "year": 2021, "price": 78400.0, "mileage": 40250, "pay": "Cash", "city": "Henderson", "state": "NV", "status": "Awaiting Review"},
  {"id": 59, "date": "INVALID", "model": "718 Spyder RS", "year": 2025, "price": 169000.0, "mileage": 850, "pay": "Financing", "city": "Lexington", "state": "KY", "status": "In Transit"},
  {"id": 60, "date": "2026-04-21", "model": "911 Dakar", "year": 2024, "price": 268900.0, "mileage": 1400, "pay": "Credit Card", "city": "Riverside", "state": "CA", "status": "Delivered"},
  {"id": 61, "date": "2026-04-29", "model": "Panamera 4", "year": 2023, "price": 101300.0, "mileage": 12700, "pay": "Wire Transfer", "city": "Corpus Christi", "state": "TX", "status": "Pending"},
  {"id": 62, "date": "2026-05-05", "model": "Macan S", "year": 2021, "price": 66750.0, "mileage": 29800, "pay": "Cash", "city": "St. Louis", "state": "MO", "status": "Delivered"},
  {"id": 63, "date": "2026-05-14", "model": "Taycan Cross Turismo", "year": 2024, "price": 127900.0, "mileage": 7500, "pay": "Lease", "city": "Pittsburgh", "state": "PA", "status": "In Transit"},
  {"id": 64, "date": "2026-05-23", "model": "Cayenne Turbo GT", "year": 2025, "price": 200000.0, "mileage": 1950, "pay": "Wire Transfer", "city": "Cincinnati", "state": "OH", "status": "Delivered"},
  {"id": 65, "date": "2026-06-02", "model": "911 Carrera Cabriolet", "year": 2023, "price": 132000.0, "mileage": 8800, "pay": "Crypto Payment", "city": "Anchorage", "state": "AK", "status": "Pending Review"},
  {"id": 66, "date": "2026-06-15", "model": "718 Cayman GT4 RS", "year": 2024, "price": 176400.0, "mileage": 600, "pay": "Credit Card", "city": "Plano", "state": "TX", "status": "Delivered"},
  {"id": 67, "date": "INVALID", "model": "Panamera 4 E-Hybrid", "year": 2022, "price": 108500.0, "mileage": 16200, "pay": "Bank Transfer", "city": "Newark", "state": "NJ", "status": "Cancelled"},
  {"id": 68, "date": "2026-07-07", "model": "Macan", "year": 2021, "price": 59000.0, "mileage": 36000, "pay": "Financing", "city": "Greensboro", "state": "NC", "status": "Awaiting Delivery"},
  {"id": 69, "date": "2026-07-20", "model": "Taycan Turbo S", "year": 2025, "price": 218000.0, "mileage": 0, "pay": "Wire Transfer", "city": "Lincoln", "state": "NE", "status": "Pending"},
  {"id": 70, "date": "INVALID", "model": "Cayenne S", "year": 2024, "price": 99950.0, "mileage": 5300, "pay": "Debit Card", "city": "Jersey City", "state": "NJ", "status": "Delivered"},
  {"id": 71, "date": "2026-04-08", "model": "911 Carrera GTS", "year": 2024, "price": 121750.0, "mileage": 5872, "pay": "Credit Card", "city": "Chandler", "state": "AZ", "status": "Delivered"},
  {"id": 72, "date": "2026-08-18", "model": "718 Boxster GTS", "year": 2023, "price": 91500.0, "mileage": 13300, "pay": "Lease", "city": "Reno", "state": "NV", "status": "Shipped"},
  {"id": 73, "date": "2026-08-31", "model": "Panamera Turbo S", "year": 2022, "price": 134000.0, "mileage": 20100, "pay": "Wire Transfer", "city": "Buffalo", "state": "NY", "status": "In Transit"},
  {"id": 74, "date": "2026-09-09", "model": "Macan GTS", "year": 2024, "price": 96800.0, "mileage": 0, "pay": "ACH Payment", "city": "Durham", "state": "NC", "status": "Delivered"},
  {"id": 75, "date": "2026-09-17", "model": "Taycan 4S", "year": 2025, "price": 131600.0, "mileage": 2900, "pay": "Wire Transfer", "city": "Laredo", "state": "TX", "status": "Pending Approval"},
  {"id": 76, "date": "2026-09-28", "model": "Cayenne E-Hybrid", "year": 2023, "price": 94300.0, "mileage": 13100, "pay": "Cash", "city": "Madison", "state": "WI", "status": "Delivered"},
  {"id": 77, "date": "2026-10-06", "model": "911 Turbo S", "year": 2025, "price": 242000.0, "mileage": 1100, "pay": "Crypto Payment", "city": "Lubbock", "state": "TX", "status": "Awaiting Pickup"},
  {"id": 78, "date": "2026-10-16", "model": "718 Cayman S", "year": 2022, "price": 82750.0, "mileage": 22500, "pay": "Credit Card", "city": "Toledo", "state": "OH", "status": "Cancelled"},
  {"id": 79, "date": "2026-10-29", "model": "Macan Electric", "year": 2026, "price": 91300.0, "mileage": 0, "pay": "Wire Transfer", "city": "Irvine", "state": "CA", "status": "Delivered"},
  {"id": 80, "date": "2026-11-03", "model": "Panamera", "year": 2021, "price": 79900.0, "mileage": 44800, "pay": "Financing", "city": "Garland", "state": "TX", "status": "Pending"},
  {"id": 81, "date": "INVALID", "model": "Cayenne Coupe", "year": 2024, "price": 111000.0, "mileage": 6700, "pay": "Bank Transfer", "city": "Irving", "state": "TX", "status": "In Transit"},
  {"id": 82, "date": "2026-12-11", "model": "911 Targa 4S", "year": 2023, "price": 156500.0, "mileage": 4200, "pay": "Cash", "city": "Chesapeake", "state": "VA", "status": "Delivered"},
  {"id": 83, "date": "2026-12-24", "model": "Taycan", "year": 2025, "price": 119900.0, "mileage": 1250, "pay": "Lease", "city": "Scottsdale", "state": "AZ", "status": "Pending"},
  {"id": 84, "date": "INVALID", "model": "Macan T", "year": 2022, "price": 73200.0, "mileage": 18600, "pay": "Wire Transfer", "city": "Norfolk", "state": "VA", "status": "Delivered"},
  {"id": 85, "date": "2026-12-28", "model": "911 GT3", "year": 2024, "price": 224000.0, "mileage": 3000, "pay": "Wire Transfer", "city": "Boise", "state": "ID", "status": "Awaiting Delivery"},
  {"id": 86, "date": "2027-01-15", "model": "911 Carrera", "year": 2024, "price": 126900.0, "mileage": 7200, "pay": "Credit Card", "city": "Orlando", "state": "FL", "status": "Delivered"},
  {"id": 87, "date": "2027-01-29", "model": "Cayenne", "year": 2023, "price": 84500.0, "mileage": 21400, "pay": "Bank Transfer", "city": "San Jose", "state": "CA", "status": "Pending"},
  {"id": 88, "date": "2027-02-11", "model": "Macan S", "year": 2022, "price": 69800.0, "mileage": 26300, "pay": "Financing", "city": "Tampa", "state": "FL", "status": "Delivered"},
  {"id": 89, "date": "INVALID", "model": "Taycan 4S", "year": 2025, "price": 132700.0, "mileage": 0, "pay": "Wire Transfer", "city": "Denver", "state": "CO", "status": "In Transit"},
  {"id": 90, "date": "2027-03-05", "model": "Panamera", "year": 2021, "price": 81000.0, "mileage": 42, "pay": "Cash", "city": "Austin", "state": "TX", "status": "Delivered"},
  {"id": 91, "date": "2027-03-18", "model": "718 Cayman", "year": 2023, "price": 78900.0, "mileage": 17500, "pay": "Debit Card", "city": "Seattle", "state": "WA", "status": "Cancelled"},
  {"id": 92, "date": "2027-04-02", "model": "911 Turbo S", "year": 2026, "price": 249300.0, "mileage": 900, "pay": "Wire Transfer", "city": "Boston", "state": "MA", "status": "Awaiting Delivery"},
  {"id": 93, "date": "INVALID", "model": "Cayenne Coupe", "year": 2024, "price": 108750.0, "mileage": 5530, "pay": "Wire Transfer", "city": "Phoenix", "state": "AZ", "status": "Pending Approval"},
  {"id": 94, "date": "INVALID", "model": "Macan Electric", "year": 2026, "price": 92600.0, "mileage": 0, "pay": "Financing", "city": "Chicago", "state": "IL", "status": "Shipped"},
  {"id": 95, "date": "2027-05-12", "model": "Taycan Turbo", "year": 2025, "price": 164000.0, "mileage": 0, "pay": "ACH Payment", "city": "Dallas", "state": "TX", "status": "In Transit"},
  {"id": 96, "date": "2027-05-27", "model": "Panamera 4S", "year": 2024, "price": 119000.0, "mileage": 13400, "pay": "Credit Card", "city": "San Francisco", "state": "CA", "status": "Delivered"},
  {"id": 97, "date": "INVALID", "model": "911 GT3", "year": 2026, "price": 232500.0, "mileage": 1700, "pay": "Wire Transfer", "city": "Las Vegas", "state": "NV", "status": "Pending"},
  {"id": 98, "date": "2027-06-18", "model": "Cayenne E-Hybrid", "year": 2023, "price": 96800.0, "mileage": 14000, "pay": "Lease", "city": "Charlotte", "state": "NC", "status": "Delivered"},
  {"id": 99, "date": "2027-07-03", "model": "Macan T", "year": 2022, "price": 74400.0, "mileage": 20750, "pay": "Cash", "city": "Mesa", "state": "AZ", "status": "Awaiting Pickup"},
  {"id": 100, "date": "2027-07-22", "model": "Taycan GTS", "year": 2025, "price": 148500.0, "mileage": 5200, "pay": "Crypto Payment", "city": "Atlanta", "state": "GA", "status": "Delivered"},
  {"id": 101, "date": "2027-08-08", "model": "718 Boxster", "year": 2021, "price": 71900.0, "mileage": 29600, "pay": "Bank Transfer", "city": "Long Beach", "state": "CA", "status": "Pending"},
  {"id": 102, "date": "INVALID", "model": "911 Targa 4", "year": 2024, "price": 143250.0, "mileage": 3100, "pay": "Financing", "city": "Oakland", "state": "CA", "status": "In Transit"},
  {"id": 103, "date": "2027-09-19", "model": "Panamera Turbo", "year": 2020, "price": 137500.0, "mileage": 48000, "pay": "Wire Transfer", "city": "Tulsa", "state": "OK", "status": "Delivered"},
  {"id": 104, "date": "2027-09-25", "model": "Cayenne Turbo GT", "year": 2025, "price": 204800.0, "mileage": 3300, "pay": "Credit Card", "city": "Wichita", "state": "KS", "status": "Pending"},
  {"id": 105, "date": "2027-10-01", "model": "911 Dakar", "year": 2024, "price": 271700.0, "mileage": 1050, "pay": "Lease", "city": "New Orleans", "state": "LA", "status": "Delivered"}
];

const $ = id => document.getElementById(id);
const money = n => new Intl.NumberFormat('en-US',{style:'currency',currency:'USD',maximumFractionDigits:0}).format(n);
const esc = s => String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const unique = key => [...new Set(DATA.map(d=>d[key]))].sort((a,b)=>String(a).localeCompare(String(b),'pt-BR',{numeric:true}));

function fillSelect(id,values){
  const el=$(id);
  values.forEach(v=>{
    const o=document.createElement('option');
    o.value=v;
    o.textContent=v;
    el.appendChild(o);
  });
}

fillSelect('model',unique('model'));
fillSelect('year',unique('year'));
fillSelect('city',unique('city'));
fillSelect('pay',unique('pay'));

function filtered(){
  return DATA.filter(d =>
    (!$('model').value || d.model === $('model').value) &&
    (!$('year').value  || String(d.year) === $('year').value) &&
    (!$('city').value  || d.city === $('city').value) &&
    (!$('pay').value   || d.pay === $('pay').value)
  );
}

function groupCount(rows, key){
  const m = {};
  rows.forEach(d => m[d[key]] = (m[d[key]] || 0) + 1);
  return Object.entries(m).sort((a,b) => b[1] - a[1] || String(a[0]).localeCompare(String(b[0])));
}

function groupSum(rows, key, valKey){
  const m = {};
  rows.forEach(d => m[d[key]] = (m[d[key]] || 0) + d[valKey]);
  return Object.entries(m).sort((a,b) => b[1] - a[1]);
}

function chartBars(target, entries, maxItems=8){
  if(!entries.length){
    $(target).innerHTML = '<div class="empty">Sem dados para os filtros selecionados.</div>';
    return;
  }
  const maxVal = Math.max(...entries.map(x => x[1]));
  $(target).innerHTML = entries.slice(0, maxItems).map(([name, val]) => `
    <div class="barrow">
      <div class="barlabel" title="${esc(name)}">${esc(name)}</div>
      <div class="track"><div class="fill" style="width:${Math.max(3, (val / maxVal) * 100)}%"></div></div>
      <div class="barval">${val}</div>
    </div>
  `).join('');
}

// Instâncias globais do Chart.js para destruição e atualização limpa ao filtrar
let chartRevModel = null;
let chartCityVol = null;
let chartPayDonut = null;

Chart.defaults.color = '#9299a2';
Chart.defaults.font.family = 'Inter, ui-sans-serif, system-ui, sans-serif';
Chart.defaults.font.size = 11;

function renderCharts(rows){
  // 1. Gráfico de Receita por Modelo (Barras Horizontais)
  const revByModel = groupSum(rows, 'model', 'price').slice(0, 8);
  const ctxRev = $('revenueModelCanvas').getContext('2d');
  if(chartRevModel) chartRevModel.destroy();
  chartRevModel = new Chart(ctxRev, {
    type: 'bar',
    data: {
      labels: revByModel.map(x => x[0]),
      datasets: [{
        label: 'Receita (USD)',
        data: revByModel.map(x => x[1]),
        backgroundColor: revByModel.map((_, i) => i === 0 ? '#d5001c' : 'rgba(213, 0, 28, 0.55)'),
        borderColor: '#ff334b',
        borderWidth: 1,
        borderRadius: 3
      }]
    },
    options: {
      indexAxis: 'y',
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#141619',
          borderColor: '#2a2e33',
          borderWidth: 1,
          callbacks: {
            label: ctx => ` Faturamento: ${money(ctx.parsed.x)}`
          }
        }
      },
      scales: {
        x: {
          grid: { color: '#22262b' },
          ticks: {
            callback: v => '$' + (v / 1000).toFixed(0) + 'k'
          }
        },
        y: {
          grid: { display: false },
          ticks: { color: '#e2e4e7' }
        }
      }
    }
  });

  // 2. Gráfico de Vendas por Cidade (Barras Verticais)
  const volByCity = groupCount(rows, 'city').slice(0, 10);
  const ctxCity = $('cityVolumeCanvas').getContext('2d');
  if(chartCityVol) chartCityVol.destroy();
  chartCityVol = new Chart(ctxCity, {
    type: 'bar',
    data: {
      labels: volByCity.map(x => x[0]),
      datasets: [{
        label: 'Unidades Vendidas',
        data: volByCity.map(x => x[1]),
        backgroundColor: volByCity.map((_, i) => i === 0 ? '#c9a66b' : 'rgba(201, 166, 107, 0.45)'),
        borderColor: '#c9a66b',
        borderWidth: 1,
        borderRadius: 3
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#141619',
          borderColor: '#2a2e33',
          borderWidth: 1,
          callbacks: {
            label: ctx => ` Volume: ${ctx.parsed.y} ${ctx.parsed.y === 1 ? 'unidade' : 'unidades'}`
          }
        }
      },
      scales: {
        x: {
          grid: { display: false },
          ticks: { color: '#e2e4e7', maxRotation: 35, minRotation: 20 }
        },
        y: {
          beginAtZero: true,
          grid: { color: '#22262b' },
          ticks: { stepSize: 1, precision: 0 }
        }
      }
    }
  });

  // 3. Gráfico de Participação por Forma de Pagamento (Donut Chart)
  const payMix = groupCount(rows, 'pay');
  const palette = [
    '#d5001c', // Porsche Guards Red
    '#c9a66b', // Champagne Gold
    '#e2e4e7', // Platinum Silver
    '#7b1fa2', // Amethyst Accent
    '#3d434c', // Slate Grey
    '#ff5366', // Crimson Light
    '#8c734b', // Bronze
    '#5c6470'  // Steel
  ];
  const ctxPay = $('payDonutCanvas').getContext('2d');
  if(chartPayDonut) chartPayDonut.destroy();
  chartPayDonut = new Chart(ctxPay, {
    type: 'doughnut',
    data: {
      labels: payMix.map(x => x[0]),
      datasets: [{
        data: payMix.map(x => x[1]),
        backgroundColor: payMix.map((_, i) => palette[i % palette.length]),
        borderColor: '#141619',
        borderWidth: 2,
        hoverOffset: 6
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      cutout: '64%',
      plugins: {
        legend: {
          position: 'right',
          labels: {
            color: '#d5d8dc',
            boxWidth: 10,
            padding: 12,
            font: { size: 11 }
          }
        },
        tooltip: {
          backgroundColor: '#141619',
          borderColor: '#2a2e33',
          borderWidth: 1,
          callbacks: {
            label: ctx => {
              const total = rows.length || 1;
              const val = ctx.parsed;
              const pct = ((val / total) * 100).toFixed(1);
              return ` ${ctx.label}: ${val} (${pct}%)`;
            }
          }
        }
      }
    }
  });
}

function render(){
  const rows = filtered();
  const n = rows.length;
  const rev = rows.reduce((a,d) => a + d.price, 0);
  const avg = n ? rev / n : 0;

  // Atualiza KPIs
  $('sales').textContent = n.toLocaleString('pt-BR');
  $('salesSub').textContent = n === 1 ? 'Unidade comercializada' : 'Unidades comercializadas';
  $('revenue').textContent = money(rev);
  $('ticket').textContent = money(avg);

  const yrs = groupCount(rows, 'year');
  const leadYear = yrs[0];
  $('topYear').textContent = leadYear ? leadYear[0] : '—';
  $('topYearSub').textContent = leadYear ? `${leadYear[1]} vendas · ${Math.round((leadYear[1] / n) * 100)}% do total` : 'Sem dados';

  // Atualiza barras HTML
  const modelsCount = groupCount(rows, 'model');
  chartBars('modelChart', modelsCount, 8);
  chartBars('yearChart', yrs, 10);

  // Atualiza gráficos Chart.js
  renderCharts(rows);

  // Tabela Performance por Cidade
  const cities = {};
  rows.forEach(d => {
    if(!cities[d.city]) cities[d.city] = [];
    cities[d.city].push(d);
  });

  const cityEntries = Object.entries(cities).map(([city, arr]) => {
    const models = {};
    arr.forEach(d => models[d.model] = (models[d.model] || 0) + 1);
    const best = Object.entries(models).sort((a,b) => b[1] - a[1])[0];
    const cityRev = arr.reduce((s,d) => s + d.price, 0);
    return { city, count: arr.length, bestModel: best[0], rev: cityRev };
  }).sort((a,b) => b.count - a.count || b.rev - a.rev);

  $('cityTable').innerHTML = cityEntries.length
    ? `<table>
        <thead>
          <tr>
            <th>Cidade</th>
            <th>Modelo mais recorrente</th>
            <th style="text-align:right">Vendas</th>
            <th style="text-align:right">Receita</th>
          </tr>
        </thead>
        <tbody>
          ${cityEntries.slice(0, 10).map(c => `
            <tr>
              <td><strong>${esc(c.city)}</strong></td>
              <td>${esc(c.bestModel)}</td>
              <td class="num">${c.count}</td>
              <td class="num">${money(c.rev)}</td>
            </tr>
          `).join('')}
        </tbody>
      </table>`
    : '<div class="empty">Nenhuma cidade encontrada para os filtros selecionados.</div>';

  // Insights de Negócio Dinâmicos
  if(n > 0){
    const topRevModel = groupSum(rows, 'model', 'price')[0];
    const topPay = groupCount(rows, 'pay')[0];
    const deliveredCount = rows.filter(d => d.status === 'Delivered').length;
    const deliveredPct = Math.round((deliveredCount / n) * 100);

    $('insights').innerHTML = `
      <div class="insight">
        <div class="eyebrow">Líder de Faturamento</div>
        <strong>${esc(topRevModel[0])}</strong>
        <p>Gera <b>${money(topRevModel[1])}</b> na seleção atual, representando ${Math.round((topRevModel[1]/rev)*100)}% da receita total filtrada.</p>
      </div>
      <div class="insight">
        <div class="eyebrow">Preferência Financeira</div>
        <strong>${esc(topPay[0])}</strong>
        <p>Método predominante com <b>${topPay[1]} ${topPay[1]===1?'transação':'transações'}</b> (${Math.round((topPay[1]/n)*100)}% do volume selecionado).</p>
      </div>
      <div class="insight">
        <div class="eyebrow">Taxa de Entrega</div>
        <strong>${deliveredPct}% Concluídas</strong>
        <p>${deliveredCount} de ${n} ${n===1?'pedido está':'pedidos estão'} com status <b>Delivered</b> na seleção atual.</p>
      </div>
    `;
  } else {
    $('insights').innerHTML = '<div class="empty" style="grid-column:1/-1">Selecione outros filtros para visualizar os insights automáticos.</div>';
  }

  // Tabela Detalhamento das Vendas
  $('rowCount').textContent = `${n} ${n === 1 ? 'REGISTRO' : 'REGISTROS'}`;
  $('detailRows').innerHTML = rows.length
    ? rows.map(d => {
        let statusClass = '';
        if(d.status === 'Delivered') statusClass = 'delivered';
        if(d.status === 'Cancelled') statusClass = 'cancelled';
        return `
          <tr>
            <td>#${d.id}</td>
            <td><strong>${esc(d.model)}</strong></td>
            <td>${d.year}</td>
            <td>${esc(d.city)} (${esc(d.state)})</td>
            <td>${esc(d.pay)}</td>
            <td><span class="pill ${statusClass}">${esc(d.status)}</span></td>
            <td class="num">${money(d.price)}</td>
          </tr>
        `;
      }).join('')
    : '<tr><td colspan="7" class="empty">Nenhum registro encontrado para a combinação de filtros selecionada.</td></tr>';
}

['model','year','city','pay'].forEach(id => $(id).addEventListener('change', render));
$('reset').addEventListener('click', () => {
  ['model','year','city','pay'].forEach(id => $(id).value = '');
  render();
});

render();
</script>
</body>
</html>
