# reporte-gas-anh
reporte-gas-anh
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ANH Bolivia - Dirección de Transporte por Ductos (DTD)</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2.2.0/dist/chartjs-plugin-datalabels.min.js"></script>
  <style>
    :root {
      --anh-dark: #16382e;
      --anh-primary: #1f4e40;
      --anh-medium: #2c6b58;
      --anh-light: #e8f1ee;
      --anh-gold: #c9972b;
      --anh-gold-light: #f9f1df;
      --text-main: #1b2623;
      --text-muted: #5a6b65;
      --border: #d0ddd8;
      --bg-body: #f2f5f4;
      --white: #ffffff;
      --danger: #b91c1c;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
      background-color: var(--bg-body);
      color: var(--text-main);
      line-height: 1.45;
    }

    /* Header Institucional */
    header {
      background: linear-gradient(135deg, var(--anh-dark) 0%, var(--anh-primary) 100%);
      color: var(--white);
      padding: 14px 28px;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      border-bottom: 4px solid var(--anh-gold);
      box-shadow: 0 3px 10px rgba(0,0,0,0.18);
    }
    .brand {
      display: flex;
      align-items: center;
      gap: 16px;
    }
    .brand-logo {
      width: 68px;
      height: 68px;
      border-radius: 10px;
      background: #1a3b32;
      border: 2px solid rgba(255,255,255,0.25);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      padding: 4px;
    }
    .brand-logo svg { width: 100%; height: 100%; }
    .brand-text h1 {
      font-size: 1.25rem;
      font-weight: 700;
      letter-spacing: 0.4px;
    }
    .brand-text h2 {
      font-size: 0.92rem;
      font-weight: 600;
      color: var(--anh-gold);
      margin-top: 2px;
    }
    .brand-text p {
      font-size: 0.82rem;
      color: #cde0d9;
    }

    .toolbar {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 8px;
    }
    .btn {
      padding: 8px 14px;
      border: none;
      border-radius: 6px;
      font-size: 0.83rem;
      font-weight: 600;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: all 0.2s;
    }
    .btn-gold { background: var(--anh-gold); color: #16241f; }
    .btn-gold:hover { background: #dda836; }
    .btn-light { background: rgba(255,255,255,0.14); color: var(--white); border: 1px solid rgba(255,255,255,0.3); }
    .btn-light:hover { background: rgba(255,255,255,0.24); }
    .btn-primary { background: var(--anh-primary); color: var(--white); }
    .btn-primary:hover { background: var(--anh-dark); }
    .btn-outline { background: var(--white); color: var(--anh-dark); border: 1px solid var(--anh-primary); }
    .btn-outline:hover { background: var(--anh-light); }
    .btn-danger { background: #fee2e2; color: var(--danger); border: 1px solid #fca5a5; }
    .btn-danger:hover { background: #fecaca; }

    /* Navigation Tabs */
    nav.tabs {
      background: var(--white);
      padding: 0 28px;
      border-bottom: 1px solid var(--border);
      display: flex;
      gap: 6px;
      overflow-x: auto;
    }
    .tab-btn {
      padding: 13px 20px;
      border: none;
      background: transparent;
      font-size: 0.9rem;
      font-weight: 600;
      color: var(--text-muted);
      cursor: pointer;
      border-bottom: 3px solid transparent;
      white-space: nowrap;
    }
    .tab-btn.active {
      color: var(--anh-dark);
      border-bottom-color: var(--anh-gold);
      background: var(--anh-light);
    }

    main {
      max-width: 1480px;
      margin: 22px auto;
      padding: 0 22px 40px;
    }
    .tab-content { display: none; }
    .tab-content.active { display: block; }

    /* KPI Cards */
    .kpi-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 16px;
      margin-bottom: 22px;
    }
    .kpi-card {
      background: var(--white);
      border-radius: 10px;
      padding: 16px 18px;
      border-left: 5px solid var(--anh-primary);
      box-shadow: 0 2px 6px rgba(0,0,0,0.05);
    }
    .kpi-card.gold { border-left-color: var(--anh-gold); }
    .kpi-title { font-size: 0.78rem; text-transform: uppercase; color: var(--text-muted); font-weight: 700; }
    .kpi-value { font-size: 1.65rem; font-weight: 800; color: var(--anh-dark); margin: 4px 0; }
    .kpi-sub { font-size: 0.78rem; color: var(--text-muted); }

    /* Panels & Filters */
    .card {
      background: var(--white);
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      margin-bottom: 22px;
      border: 1px solid var(--border);
    }
    .card-header {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
      padding-bottom: 10px;
      border-bottom: 1px solid var(--border);
      gap: 10px;
    }
    .card-header h2 {
      font-size: 1.03rem;
      color: var(--anh-dark);
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .card-header select {
      padding: 5px 10px;
      border: 1px solid var(--anh-primary);
      border-radius: 6px;
      font-size: 0.82rem;
      font-weight: 600;
      color: var(--anh-dark);
      background: var(--anh-light);
    }
    .filter-bar {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 12px;
      background: var(--anh-light);
      padding: 12px 16px;
      border-radius: 8px;
      margin-bottom: 20px;
      border: 1px solid #c3d6cf;
    }
    .filter-bar label { font-size: 0.83rem; font-weight: 600; color: var(--anh-dark); display: flex; align-items: center; gap: 6px; }
    .filter-bar input[type="date"], .filter-bar select {
      padding: 6px 10px;
      border: 1px solid var(--border);
      border-radius: 5px;
      font-size: 0.85rem;
      background: var(--white);
    }

    /* Charts Grid */
    .charts-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(560px, 1fr));
      gap: 22px;
    }
    @media (max-width: 640px) {
      .charts-grid { grid-template-columns: 1fr; }
    }
    .chart-box {
      position: relative;
      height: 365px;
      width: 100%;
    }
    .chart-insight {
      margin-top: 12px;
      padding: 10px 14px;
      background: #f6faf8;
      border-left: 4px solid var(--anh-gold);
      border-radius: 6px;
      font-size: 0.82rem;
      color: var(--anh-dark);
    }
    .chart-insight strong { color: var(--anh-primary); }

    /* Form Grid */
    .form-section-title {
      background: var(--anh-primary);
      color: var(--white);
      padding: 7px 14px;
      border-radius: 6px;
      font-size: 0.86rem;
      font-weight: 700;
      margin: 18px 0 12px;
      letter-spacing: 0.3px;
    }
    .form-section-title.calc { background: var(--anh-gold); color: #16241f; }
    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(215px, 1fr));
      gap: 12px;
    }
    .field {
      display: flex;
      flex-direction: column;
      gap: 4px;
    }
    .field label {
      font-size: 0.76rem;
      font-weight: 700;
      color: var(--anh-dark);
    }
    .field input {
      padding: 8px 10px;
      border: 1px solid var(--border);
      border-radius: 5px;
      font-size: 0.88rem;
      font-family: inherit;
    }
    .field input:focus {
      outline: none;
      border-color: var(--anh-primary);
      box-shadow: 0 0 0 2px rgba(31, 78, 64, 0.15);
    }
    .field input[readonly] {
      background: var(--anh-gold-light);
      border-color: #e2c57e;
      font-weight: 700;
      color: var(--anh-dark);
    }

    /* Data Table */
    .table-wrapper {
      overflow-x: auto;
      max-height: 560px;
      border: 1px solid var(--border);
      border-radius: 8px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.8rem;
      white-space: nowrap;
    }
    thead th {
      position: sticky;
      top: 0;
      background: var(--anh-dark);
      color: var(--white);
      padding: 9px 10px;
      text-align: right;
      font-weight: 600;
      border-right: 1px solid rgba(255,255,255,0.12);
      z-index: 2;
    }
    thead th:first-child, tbody td:first-child {
      position: sticky;
      left: 0;
      z-index: 3;
      text-align: center;
    }
    tbody td:first-child { background: #f8faf9; font-weight: 700; }
    tbody td {
      padding: 7px 10px;
      border-bottom: 1px solid var(--border);
      border-right: 1px solid #edf2f0;
      text-align: right;
    }
    tbody tr:hover td { background: var(--anh-light); }
    td.calc-col { background: #fdfaf2; font-weight: 600; color: var(--anh-dark); }

    /* Executive Presentation Sheet */
    .exec-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
      gap: 18px;
    }
    .exec-table { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
    .exec-table th { background: var(--anh-primary); color: var(--white); padding: 8px 12px; text-align: left; }
    .exec-table td { padding: 7px 12px; border-bottom: 1px solid var(--border); text-align: left; }
    .exec-table td.num { text-align: right; font-weight: 700; font-family: monospace; font-size: 0.92rem; }
    .exec-total { background: var(--anh-gold-light); font-weight: 800; }

    .toast {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: var(--anh-dark);
      color: var(--white);
      padding: 12px 20px;
      border-radius: 8px;
      border-left: 5px solid var(--anh-gold);
      box-shadow: 0 5px 15px rgba(0,0,0,0.25);
      font-size: 0.88rem;
      font-weight: 600;
      display: none;
      z-index: 999;
    }
  
    /* Interactive Chart Cards & Fullscreen Modal */
    .chart-card {
      position: relative;
      transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
      cursor: pointer;
    }
    .chart-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 22px rgba(22, 56, 46, 0.14);
      border-color: var(--anh-gold);
    }
    .expand-badge {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      background: var(--anh-gold-light);
      color: var(--anh-dark);
      border: 1px solid #dfb962;
      padding: 4px 10px;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: 700;
      transition: all 0.2s;
    }
    .chart-card:hover .expand-badge {
      background: var(--anh-gold);
      color: #16241f;
    }

    /* Fullscreen Modal Overlay */
    .chart-modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(15, 34, 28, 0.86);
      backdrop-filter: blur(5px);
      z-index: 2000;
      display: none;
      align-items: center;
      justify-content: center;
      padding: 14px;
      animation: fadeInModal 0.22s ease-out;
    }
    .chart-modal-overlay.open {
      display: flex;
    }
    @keyframes fadeInModal {
      from { opacity: 0; transform: scale(0.98); }
      to { opacity: 1; transform: scale(1); }
    }
    .chart-modal-window {
      background: var(--white);
      width: 98vw;
      height: 95vh;
      border-radius: 14px;
      border: 2px solid var(--anh-gold);
      box-shadow: 0 20px 50px rgba(0,0,0,0.45);
      display: flex;
      flex-direction: column;
      overflow: hidden;
    }
    .chart-modal-header {
      background: linear-gradient(135deg, var(--anh-dark) 0%, var(--anh-primary) 100%);
      color: var(--white);
      padding: 12px 22px;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      border-bottom: 3px solid var(--anh-gold);
    }
    .chart-modal-title-group h3 {
      font-size: 1.12rem;
      font-weight: 700;
      margin: 0;
    }
    .chart-modal-title-group span {
      font-size: 0.8rem;
      color: #d1e3dc;
    }
    .chart-modal-controls {
      display: flex;
      align-items: center;
      gap: 8px;
      flex-wrap: wrap;
    }
    .chart-modal-body {
      flex: 1;
      display: grid;
      grid-template-columns: 1.65fr 1fr;
      overflow: hidden;
      background: var(--bg-body);
    }
    @media (max-width: 1024px) {
      .chart-modal-body {
        grid-template-columns: 1fr;
        overflow-y: auto;
      }
    }
    .modal-chart-pane {
      background: var(--white);
      padding: 18px 22px;
      display: flex;
      flex-direction: column;
      border-right: 1px solid var(--border);
      min-height: 360px;
    }
    .modal-chart-toolbar {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      margin-bottom: 10px;
      padding-bottom: 8px;
      border-bottom: 1px dashed var(--border);
    }
    .modal-canvas-wrapper {
      flex: 1;
      position: relative;
      width: 100%;
      min-height: 300px;
    }
    .modal-tech-pane {
      padding: 18px 22px;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 14px;
      background: #f7faf9;
    }
    .tech-section {
      background: var(--white);
      border-radius: 9px;
      padding: 14px 16px;
      border: 1px solid var(--border);
      box-shadow: 0 1px 4px rgba(0,0,0,0.04);
    }
    .tech-section-header {
      font-size: 0.82rem;
      font-weight: 800;
      text-transform: uppercase;
      letter-spacing: 0.4px;
      color: var(--anh-primary);
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      gap: 6px;
      border-bottom: 1px solid #e6efec;
      padding-bottom: 5px;
    }
    .tech-kpi-mini-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 9px;
    }
    .tech-kpi-box {
      background: var(--anh-light);
      border-left: 4px solid var(--anh-primary);
      padding: 8px 10px;
      border-radius: 6px;
    }
    .tech-kpi-box.gold {
      background: var(--anh-gold-light);
      border-left-color: var(--anh-gold);
    }
    .tech-kpi-box .lbl {
      font-size: 0.7rem;
      font-weight: 700;
      color: var(--text-muted);
      text-transform: uppercase;
    }
    .tech-kpi-box .val {
      font-size: 1.08rem;
      font-weight: 800;
      color: var(--anh-dark);
      margin-top: 2px;
    }
    .tech-kpi-box .sub {
      font-size: 0.72rem;
      color: var(--text-muted);
    }
    .didactic-callout {
      background: #fffbeb;
      border: 1px solid #fde68a;
      border-left: 4px solid var(--anh-gold);
      border-radius: 7px;
      padding: 10px 12px;
      font-size: 0.83rem;
      color: #3b3014;
    }
    .didactic-callout ul {
      margin: 6px 0 0 18px;
    }
    .didactic-callout li {
      margin-bottom: 4px;
    }
    .tech-analysis-text {
      font-size: 0.85rem;
      color: var(--text-main);
      line-height: 1.5;
    }
    .tech-analysis-text p {
      margin-bottom: 8px;
    }
    .mini-breakdown-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.79rem;
    }
    .mini-breakdown-table th {
      background: var(--anh-primary);
      color: var(--white);
      padding: 6px 8px;
      text-align: left;
    }
    .mini-breakdown-table td {
      padding: 6px 8px;
      border-bottom: 1px solid var(--border);
    }
    .mini-breakdown-table td.num {
      text-align: right;
      font-weight: 700;
      font-family: monospace;
    }
    .badge-delta-up {
      background: #dcfce7;
      color: #166534;
      padding: 2px 6px;
      border-radius: 4px;
      font-weight: 700;
      font-size: 0.74rem;
    }
    .badge-delta-down {
      background: #fee2e2;
      color: #991b1b;
      padding: 2px 6px;
      border-radius: 4px;
      font-weight: 700;
      font-size: 0.74rem;
    }
    .badge-delta-flat {
      background: #f1f5f9;
      color: #475569;
      padding: 2px 6px;
      border-radius: 4px;
      font-weight: 700;
      font-size: 0.74rem;
    }

  </style>
</head>
<body>

<header>
  <div class="brand">
    <div class="brand-logo" title="Agencia Nacional de Hidrocarburos - Bolivia">
      <!-- Logotipo Institucional ANH Bolivia -->
      <svg viewBox="0 0 120 100" xmlns="http://www.w3.org/2000/svg">
        <rect width="120" height="100" fill="#1a3b32"/>
        <g fill="none" stroke="#ffffff" stroke-width="5.5" stroke-dasharray="3.2 2.2">
          <path d="M 48 16 A 32 32 0 1 0 48 84" />
        </g>
        <g fill="none" stroke="#c9972b" stroke-width="2.5" stroke-dasharray="2.5 2.5">
          <path d="M 46 23 A 25 25 0 1 0 46 77" />
        </g>
        <text x="34" y="55" fill="#ffffff" font-family="Arial Black, sans-serif" font-weight="900" font-size="25" letter-spacing="1">ANH</text>
        <text x="36" y="67" fill="#dce8e3" font-family="Arial, sans-serif" font-size="6.4" font-weight="bold">Agencia Nacional</text>
        <text x="36" y="75" fill="#dce8e3" font-family="Arial, sans-serif" font-size="6.4" font-weight="bold">de Hidrocarburos</text>
      </svg>
    </div>
    <div class="brand-text">
      <h1>AGENCIA NACIONAL DE HIDROCARBUROS — BOLIVIA</h1>
      <h2>DIRECCIÓN DE TRANSPORTE POR DUCTOS (DTD)</h2>
      <p>Sistema Consolidado de Reporte Diario de Gas Natural • Balance Operativo y Gráficos Dinámicos</p>
    </div>
  </div>

  <div class="toolbar">
    <button class="btn btn-gold" onclick="exportJSON()" title="Descargar base de datos para llevar a otra PC">💾 Exportar Consolidado (JSON)</button>
    <button class="btn btn-light" onclick="document.getElementById('importFile').click()" title="Cargar archivo JSON desde otra PC">📂 Importar Base (JSON)</button>
    <input type="file" id="importFile" accept=".json" style="display:none" onchange="importJSON(event)" />
    <button class="btn btn-light" onclick="exportCSV()">📊 Exportar Excel (CSV)</button>
    <button class="btn btn-light" onclick="exportStandaloneHTML()" title="Descarga este mismo HTML con todos los datos nuevos ya incrustados">🖥️ Guardar HTML Portable</button>
  </div>
</header>

<nav class="tabs">
  <button class="tab-btn active" onclick="switchTab('dashboard', this)">📈 Dashboard Dinámico</button>
  <button class="tab-btn" onclick="switchTab('registro', this)">➕ Registro Día Siguiente</button>
  <button class="tab-btn" onclick="switchTab('presentacion', this)">📋 Cuadro Resumen (Presentación)</button>
  <button class="tab-btn" onclick="switchTab('tabla', this)">🗄️ Base Consolidada (<span id="countBadge">0</span> días)</button>
</nav>

<main>
  <!-- TAB 1: DASHBOARD DINÁMICO -->
  <section id="tab-dashboard" class="tab-content active">
    <div class="filter-bar">
      <label>Desde: <input type="date" id="filterFrom" onchange="onDateFilterChange()" /></label>
      <label>Hasta: <input type="date" id="filterTo" onchange="onDateFilterChange()" /></label>
      <button class="btn btn-outline" onclick="setQuickRange(1)">Solo Último Día</button>
      <button class="btn btn-outline" onclick="setQuickRange(7)">Últimos 7 días</button>
      <button class="btn btn-outline" onclick="setQuickRange(30)">Últimos 30 días</button>
      <button class="btn btn-primary" onclick="setQuickRange(0)">Todo el Histórico</button>
      <label style="margin-left: 8px; cursor: pointer;">
        <input type="checkbox" id="toggleLineLabels" onchange="updateDashboard()" /> Mostrar valores en curvas
      </label>
      <span style="margin-left:auto; font-size:0.82rem; color:var(--anh-dark); font-weight:700;" id="rangeInfo"></span>
    </div>

    <div class="kpi-grid">
      <div class="kpi-card gold">
        <div class="kpi-title" id="kpiFechaTitle">Fecha / Periodo Activo</div>
        <div class="kpi-value" id="kpiFecha" style="font-size:1.32rem;">-</div>
        <div class="kpi-sub" id="kpiFechaSub">Fecha de reporte operativo</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-title" id="kpiTotalSisTitle">Total Sistema</div>
        <div class="kpi-value" id="kpiTotalSis">0.00</div>
        <div class="kpi-sub" id="kpiTotalSisSub">MMmcd (Externo + Interno)</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-title" id="kpiExtTitle">Mercado Externo</div>
        <div class="kpi-value" id="kpiExt">0.00</div>
        <div class="kpi-sub" id="kpiExtSub">MMmcd</div>
      </div>
      <div class="kpi-card">
        <div class="kpi-title" id="kpiIntTitle">Mercado Interno</div>
        <div class="kpi-value" id="kpiInt">0.00</div>
        <div class="kpi-sub" id="kpiIntSub">MMmcd</div>
      </div>
      <div class="kpi-card gold">
        <div class="kpi-title">Promedio Sistema (Periodo)</div>
        <div class="kpi-value" id="kpiAvgSis">0.00</div>
        <div class="kpi-sub" id="kpiAvgSisSub">MMmcd promedio en rango filtrado</div>
      </div>
    </div>

    <div style="margin-bottom: 12px; font-size: 0.82rem; color: var(--anh-primary); font-weight: 600; display: flex; align-items: center; gap: 6px;">
      <span>🖱️ <strong>Sugerencia interactiva:</strong> Haga clic sobre cualquier gráfico para ampliarlo a pantalla completa con su <em>Resumen Técnico y Guía Didáctica</em>.</span>
    </div>

    <div class="charts-grid">
      <!-- GRÁFICO 1 -->
      <div class="card chart-card" onclick="openChartModal('chartSistema')" title="Clic para ampliar a pantalla completa y ver resumen técnico">
        <div class="card-header">
          <h2>1. Balance Total del Sistema: Externo vs. Interno (MMmcd)</h2>
          <div style="display:flex; align-items:center; gap:8px;" onclick="event.stopPropagation()">
            <select id="chart1Type" onchange="updateDashboard()">
              <option value="line">Líneas + Promedio</option>
              <option value="bar">Barras Apiladas</option>
            </select>
            <button type="button" class="expand-badge" onclick="openChartModal('chartSistema')">🔍 Ampliar</button>
          </div>
        </div>
        <div class="chart-box"><canvas id="chartSistema"></canvas></div>
        <div class="chart-insight" id="insightSistema"></div>
      </div>

      <!-- GRÁFICO 2 -->
      <div class="card chart-card" onclick="openChartModal('chartExterno')" title="Clic para ampliar a pantalla completa y ver resumen técnico">
        <div class="card-header">
          <h2>2. Exportación por Punto y Gas en Tránsito (MMmcd)</h2>
          <div style="display:flex; align-items:center; gap:8px;" onclick="event.stopPropagation()">
            <select id="chart2Type" onchange="updateDashboard()">
              <option value="line">Curvas Comparativas</option>
              <option value="bar">Barras Agrupadas</option>
            </select>
            <button type="button" class="expand-badge" onclick="openChartModal('chartExterno')">🔍 Ampliar</button>
          </div>
        </div>
        <div class="chart-box"><canvas id="chartExterno"></canvas></div>
        <div class="chart-insight" id="insightExterno"></div>
      </div>

      <!-- GRÁFICO 3: TORTA CON PORCENTAJE, NOMBRES Y COLORES LLAMATIVOS -->
      <div class="card chart-card" onclick="openChartModal('chartSectores')" title="Clic para ampliar a pantalla completa y ver resumen técnico">
        <div class="card-header">
          <div>
            <h2>3. Participación Mercado Interno por Sector (% y MMmcd)</h2>
            <div id="chart3DateBadge" style="display:inline-block; margin-top:4px; font-size:0.78rem; font-weight:700; color:var(--anh-primary); background:var(--anh-light); border:1px solid #b9d1c8; padding:2px 9px; border-radius:5px;">📅 Fecha del gráfico: -</div>
          </div>
          <div style="display:flex; align-items:center; gap:6px; flex-wrap:wrap;" onclick="event.stopPropagation()">
            <select id="chart3Mode" onchange="updateDashboard()" title="Elegir entre el último día del filtro o el promedio de todas las fechas filtradas">
              <option value="latest">Último día del rango</option>
              <option value="average">Promedio fechas seleccionadas</option>
            </select>
            <select id="chart3Type" onchange="updateDashboard()">
              <option value="pie">Torta Clásica (% y Nombres)</option>
              <option value="doughnut">Anillo / Dona (% y Nombres)</option>
              <option value="bar">Barras Horizontales</option>
            </select>
            <button type="button" class="expand-badge" onclick="openChartModal('chartSectores')">🔍 Ampliar</button>
          </div>
        </div>
        <div class="chart-box"><canvas id="chartSectores"></canvas></div>
        <div class="chart-insight" id="insightSectores"></div>
      </div>

      <!-- GRÁFICO 4 -->
      <div class="card chart-card" onclick="openChartModal('chartCarrasco')" title="Clic para ampliar a pantalla completa y ver resumen técnico">
        <div class="card-header">
          <h2>4. Flujo Operativo Nodo Carrasco y Entrega GAA+GCC (MMmcd)</h2>
          <div style="display:flex; align-items:center; gap:8px;" onclick="event.stopPropagation()">
            <button type="button" class="expand-badge" onclick="openChartModal('chartCarrasco')">🔍 Ampliar</button>
          </div>
        </div>
        <div class="chart-box"><canvas id="chartCarrasco"></canvas></div>
        <div class="chart-insight" id="insightCarrasco"></div>
      </div>
    </div>
  </section>

  <!-- TAB 2: REGISTRO DÍA SIGUIENTE -->
  <section id="tab-registro" class="tab-content">
    <div class="card">
      <div class="card-header">
        <h2>Registro Operativo Diario — Carga de Nuevo Día</h2>
        <div style="display:flex; gap:8px;">
          <button class="btn btn-outline" onclick="prepareNextDay()">📅 Sugerir Día Siguiente</button>
          <button class="btn btn-outline" onclick="copyPreviousDayValues()">📋 Copiar Valores del Día Anterior</button>
        </div>
      </div>

      <form id="dailyForm" onsubmit="saveRecord(event)">
        <div class="form-grid">
          <div class="field">
            <label>FECHA DE REPORTE *</label>
            <input type="date" id="f_fecha" required onchange="loadExistingDateIfAny()" />
          </div>
        </div>

        <div class="form-section-title">1. MERCADO EXTERNO</div>
        <div class="form-grid">
          <div class="field"><label>Mutún (MMmcd)</label><input type="number" step="any" id="f_mutun" oninput="recalcForm()" required /></div>
          <div class="field"><label>San Matías (MMmcd)</label><input type="number" step="any" id="f_san_matias" value="0" oninput="recalcForm()" required /></div>
          <div class="field"><label>Gas en Tránsito (PCD)</label><input type="number" step="any" id="f_gas_transito_mmpcd" value="0" oninput="recalcForm()" required /></div>
          <div class="field"><label>GIJA (MMmcd)</label><input type="number" step="any" id="f_gija" value="0" oninput="recalcForm()" required /></div>
        </div>

        <div class="form-section-title">2. MERCADO INTERNO (TOTALES Y SECTORES EN MMmcd)</div>
        <div class="form-grid">
          <div class="field"><label>Gas Combustible (MMmcd)</label><input type="number" step="any" id="f_gas_comb" oninput="recalcForm()" required /></div>
          <div class="field"><label>Entregas Mercado Interno (MMmcd)</label><input type="number" step="any" id="f_entregas_int" oninput="recalcForm()" required /></div>
          <div class="field"><label>Sector Eléctrico SIN + Aislados (MMmcd)</label><input type="number" step="any" id="f_sector_electrico" oninput="recalcForm()" /></div>
          <div class="field"><label>Distribución Redes EMTAGAS+YPFB (MMmcd)</label><input type="number" step="any" id="f_distribucion_redes" oninput="recalcForm()" /></div>
          <div class="field"><label>Plantas Proc. PSL+VG+Kanata (MMmcd)</label><input type="number" step="any" id="f_plantas_proc" oninput="recalcForm()" /></div>
          <div class="field"><label>Consumidores Directos y Otros (MMmcd)</label><input type="number" step="any" id="f_consumidores_dir" oninput="recalcForm()" /></div>
          <div class="field"><label>Gas Comb. Est. Compresión + JV (MMmcd)</label><input type="number" step="any" id="f_gas_comb_comp" oninput="recalcForm()" /></div>
        </div>

        <div class="form-section-title">3. DATOS DE INTERÉS — NODO CARRASCO Y RÍO GRANDE (PCD)</div>
        <div class="form-grid">
          <div class="field"><label>Carrasco (PCD)</label><input type="number" step="any" id="f_carrasco_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>Yapacaní (PCD)</label><input type="number" step="any" id="f_yapacani_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>Santa Rosa (PCD)</label><input type="number" step="any" id="f_santa_rosa_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>ENDE Entre Ríos (PCD)</label><input type="number" step="any" id="f_ende_entre_rios_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>PAU (PCD)</label><input type="number" step="any" id="f_pau_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>GCC (PCD)</label><input type="number" step="any" id="f_gcc_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>Absorción Río Grande (PCD)</label><input type="number" step="any" id="f_absorcion_rio_grande_pcd" oninput="recalcForm()" /></div>
          <div class="field"><label>GTC a GAA (PCD)</label><input type="number" step="any" id="f_gtc_a_gaa_pcd" value="0" oninput="recalcForm()" /></div>
          <div class="field"><label>Percheles (PCD)</label><input type="number" step="any" id="f_percheles_pcd" oninput="recalcForm()" /></div>
        </div>

        <div class="form-section-title calc">4. CÁLCULOS AUTOMÁTICOS CONSOLIDADOS (FÓRMULAS EXCEL)</div>
        <div class="form-grid">
          <div class="field"><label>Gas en Tránsito (MMmcd)</label><input type="text" id="c_gas_transito_mmmcd" readonly /></div>
          <div class="field"><label>Total Mercado Externo (MMmcd)</label><input type="text" id="c_total_ext" readonly /></div>
          <div class="field"><label>Total Mercado Interno (MMmcd)</label><input type="text" id="c_total_int" readonly /></div>
          <div class="field"><label>TOTAL SISTEMA (MMmcd)</label><input type="text" id="c_total_sistema" readonly /></div>
          <div class="field"><label>Recep. Nodo Carrasco (MMmcd)</label><input type="text" id="c_recep_nodo_carrasco" readonly /></div>
          <div class="field"><label>Entregas Carrasco (MMmcd)</label><input type="text" id="c_entregas_carrasco" readonly /></div>
          <div class="field"><label>Entrega GAA + GCC (MMmcd)</label><input type="text" id="c_entrega_gaa_gcc" readonly /></div>
        </div>

        <div style="margin-top:20px; display:flex; gap:12px; flex-wrap:wrap;">
          <button type="submit" class="btn btn-primary" style="padding:11px 24px; font-size:0.95rem;">✅ Guardar y Consolidar Día</button>
          <button type="button" class="btn btn-outline" onclick="resetFormClean()">Limpiar Campos</button>
        </div>
      </form>
    </div>
  </section>

  <!-- TAB 3: CUADRO PARA PRESENTACIÓN -->
  <section id="tab-presentacion" class="tab-content">
    <div class="card">
      <div class="card-header">
        <h2>Reporte Ejecutivo Diario (Hoja "PARA PRESENTACION")</h2>
        <div>
          <label style="font-weight:700; font-size:0.85rem;">Seleccionar Fecha: 
            <select id="presDateSelect" onchange="renderPresentation()" style="padding:6px 12px; border-radius:6px; border:1px solid var(--anh-primary); font-weight:700;"></select>
          </label>
        </div>
      </div>

      <div class="exec-grid" id="presentationContainer"></div>
    </div>
  </section>

  <!-- TAB 4: TABLA CONSOLIDADA -->
  <section id="tab-tabla" class="tab-content">
    <div class="card">
      <div class="card-header">
        <h2>Histórico Consolidado de Registro Diario</h2>
        <div style="display:flex; gap:8px;">
          <input type="text" id="searchTable" placeholder="Buscar fecha (ej. 2026-10)..." onkeyup="renderTable()" style="padding:7px 12px; border:1px solid var(--border); border-radius:6px;" />
          <button class="btn btn-danger" onclick="resetToFactoryData()">Restaurar Datos Iniciales</button>
        </div>
      </div>
      <div class="table-wrapper">
        <table>
          <thead>
            <tr>
              <th>Fecha</th>
              <th>Acciones</th>
              <th>Mutún</th>
              <th>San Matías</th>
              <th>Gas Tráns. (PCD)</th>
              <th>Gas Tráns. (MMmcd)</th>
              <th>GIJA</th>
              <th>Total Ext.</th>
              <th>Gas Comb.</th>
              <th>Entregas Int.</th>
              <th>Total Int.</th>
              <th>TOTAL SISTEMA</th>
              <th>Carrasco (PCD)</th>
              <th>Yapacaní (PCD)</th>
              <th>Santa Rosa (PCD)</th>
              <th>Recep. Carrasco</th>
              <th>ENDE E. Ríos</th>
              <th>PAU (PCD)</th>
              <th>GCC (PCD)</th>
              <th>Entregas Carrasco</th>
              <th>Abs. Río Grande</th>
              <th>GTC a GAA</th>
              <th>Percheles</th>
              <th>Entrega GAA+GCC</th>
              <th>Sec. Eléctrico</th>
              <th>Dist. Redes</th>
              <th>Plantas Proc.</th>
              <th>Cons. Directos</th>
              <th>Gas Comb. Comp.</th>
            </tr>
          </thead>
          <tbody id="tableBody"></tbody>
        </table>
      </div>
    </div>
  </section>
</main>

<!-- MODAL PANTALLA COMPLETA CON RESUMEN TÉCNICO Y DIDÁCTICO -->
<div class="chart-modal-overlay" id="chartModal" onclick="handleModalBackdropClick(event)">
  <div class="chart-modal-window" onclick="event.stopPropagation()">
    <div class="chart-modal-header">
      <div class="chart-modal-title-group">
        <h3 id="modalChartTitle">Análisis Detallado</h3>
        <span id="modalChartSubtitle">Dirección de Transporte por Ductos (DTD) • ANH Bolivia</span>
      </div>
      <div class="chart-modal-controls">
        <button type="button" class="btn btn-light" onclick="navigateModalChart(-1)" title="Gráfico Anterior (Flecha Izquierda)">⬅️ Anterior</button>
        <button type="button" class="btn btn-light" onclick="navigateModalChart(1)" title="Siguiente Gráfico (Flecha Derecha)">Siguiente ➡️</button>
        <button type="button" class="btn btn-gold" onclick="closeChartModal()" title="Cerrar vista ampliada (ESC)">✖ Cerrar Pantalla Completa</button>
      </div>
    </div>

    <div class="chart-modal-body">
      <!-- PANEL IZQUIERDO: GRÁFICO AMPLIADO INTERACTIVO -->
      <div class="modal-chart-pane">
        <div class="modal-chart-toolbar">
          <div style="display:flex; align-items:center; gap:10px; flex-wrap:wrap;">
            <label style="font-size:0.82rem; font-weight:700; color:var(--anh-dark);">
              Tipo de Visualización:
              <select id="modalChartTypeSelect" onchange="changeModalChartType()" style="padding:5px 10px; border-radius:6px; border:1px solid var(--anh-primary); background:var(--anh-light); font-weight:700; color:var(--anh-dark);"></select>
            </label>
            <label id="modalPieModeWrap" style="display:none; font-size:0.82rem; font-weight:700; color:var(--anh-dark);">
              Cálculo de Datos:
              <select id="modalChart3ModeSelect" onchange="changeModalChart3Mode()" style="padding:5px 10px; border-radius:6px; border:1px solid var(--anh-primary); background:var(--anh-gold-light); font-weight:700; color:var(--anh-dark);">
                <option value="latest">Último día del rango</option>
                <option value="average">Promedio fechas seleccionadas</option>
              </select>
            </label>
            <label id="modalLabelsToggleWrap" style="font-size:0.82rem; font-weight:600; color:var(--anh-dark); cursor:pointer; display:flex; align-items:center; gap:5px;">
              <input type="checkbox" id="modalToggleLabels" onchange="renderModalContent()" /> Mostrar etiquetas numéricas
            </label>
          </div>
          <span id="modalPeriodBadge" style="font-size:0.78rem; font-weight:700; background:var(--anh-light); color:var(--anh-primary); padding:4px 10px; border-radius:6px; border:1px solid #c3d6cf;"></span>
        </div>
        <div class="modal-canvas-wrapper">
          <canvas id="modalChartCanvas"></canvas>
        </div>
      </div>

      <!-- PANEL DERECHO: RESUMEN TÉCNICO Y DIDÁCTICO DINÁMICO -->
      <div class="modal-tech-pane" id="modalTechSummary">
        <!-- Contenido inyectado dinámicamente -->
      </div>
    </div>
  </div>
</div>


<div class="toast" id="toast"></div>

<script>
// Registrar plugin de etiquetas de datos
Chart.register(ChartDataLabels);

const FACTOR_CONV = 35.314;

// Datos históricos precargados desde Excel (01/09/2026 al 16/11/2026)
const INITIAL_DATA = [{"fecha":"2026-09-01","mutun":9.456685999999998,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.79913,"gas_comb":0.31047872197350024,"entregas_int":11.690057692582286,"carrasco_pcd":19503031.0,"yapacani_pcd":88443891.0,"santa_rosa_pcd":26294102.0,"ende_entre_rios_pcd":11598104.0,"pau_pcd":29238.32511901855,"gcc_pcd":64698270.0,"absorcion_rio_grande_pcd":60540789.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15224574.0,"sector_electrico":4.733542727530235,"distribucion_redes":5.2509676287801135,"plantas_proc":1.1039106319144678,"consumidores_dir":0.5814295836072567,"gas_comb_comp":0.3306858427237137},{"fecha":"2026-09-02","mutun":11.944694000000002,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.31614550842399375,"entregas_int":11.880564640991405,"carrasco_pcd":20920063.0,"yapacani_pcd":87994852.0,"santa_rosa_pcd":26221264.0,"ende_entre_rios_pcd":12135133.0,"pau_pcd":0.0,"gcc_pcd":56378000.0,"absorcion_rio_grande_pcd":59231684.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15101709.0,"sector_electrico":4.96286217611069,"distribucion_redes":5.2934798263361085,"plantas_proc":1.0358783220099457,"consumidores_dir":0.5681371957844462,"gas_comb_comp":0.3363526291742073},{"fecha":"2026-09-03","mutun":12.834499999999998,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.2367859058210277,"entregas_int":12.482076817730706,"carrasco_pcd":22169906.0,"yapacani_pcd":88890875.0,"santa_rosa_pcd":26261254.0,"ende_entre_rios_pcd":12810678.0,"pau_pcd":0.0,"gcc_pcd":61136871.0,"absorcion_rio_grande_pcd":57113949.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15166247.0,"sector_electrico":5.404406818285484,"distribucion_redes":5.349459618997675,"plantas_proc":1.1017714100319846,"consumidores_dir":0.6062318496653482,"gas_comb_comp":0.2569930265712412},{"fecha":"2026-09-04","mutun":11.395483000000002,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.3257297557516431,"entregas_int":13.297211223771672,"carrasco_pcd":22325094.0,"yapacani_pcd":88173633.0,"santa_rosa_pcd":25953029.0,"ende_entre_rios_pcd":16967334.0,"pau_pcd":0.0,"gcc_pcd":64517074.0,"absorcion_rio_grande_pcd":55554680.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15210007.0,"sector_electrico":6.035191420602709,"distribucion_redes":5.544118602757396,"plantas_proc":1.1287023397874778,"consumidores_dir":0.5689917398738759,"gas_comb_comp":0.3459368765018566},{"fecha":"2026-09-05","mutun":8.778231,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":1.959384,"gas_comb":0.2943507954961494,"entregas_int":11.548788226688272,"carrasco_pcd":21758531.0,"yapacani_pcd":86378359.0,"santa_rosa_pcd":26320896.0,"ende_entre_rios_pcd":11633883.0,"pau_pcd":44584.24758911133,"gcc_pcd":85082664.0,"absorcion_rio_grande_pcd":57874121.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15163854.0,"sector_electrico":4.483627869809634,"distribucion_redes":5.350640848272054,"plantas_proc":1.1163678448416097,"consumidores_dir":0.5779445430147587,"gas_comb_comp":0.3145579162463628},{"fecha":"2026-09-06","mutun":7.679880000000001,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":3.682747,"gas_comb":0.29201297414969446,"entregas_int":9.416693825436292,"carrasco_pcd":21028906.0,"yapacani_pcd":86953242.0,"santa_rosa_pcd":26407031.0,"ende_entre_rios_pcd":10376432.0,"pau_pcd":85362.34283447266,"gcc_pcd":40570156.0,"absorcion_rio_grande_pcd":56920387.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15212342.0,"sector_electrico":3.6776889262665122,"distribucion_redes":4.066622723501687,"plantas_proc":1.1120271959592714,"consumidores_dir":0.5401478589586072,"gas_comb_comp":0.31222009489990793},{"fecha":"2026-09-07","mutun":7.906798000000001,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":3.917494,"gas_comb":0.2796730296895804,"entregas_int":12.0511706885456,"carrasco_pcd":20534781.0,"yapacani_pcd":86984984.0,"santa_rosa_pcd":26344525.0,"ende_entre_rios_pcd":12847911.0,"pau_pcd":0.0,"gcc_pcd":57114359.0,"absorcion_rio_grande_pcd":57751199.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15240270.0,"sector_electrico":4.973419285292568,"distribucion_redes":5.448121834869942,"plantas_proc":1.1096769653970013,"consumidores_dir":0.49974548223587495,"gas_comb_comp":0.29988015043979394},{"fecha":"2026-09-08","mutun":8.255935,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":4.505153,"gas_comb":0.2969489685907577,"entregas_int":12.812533989046324,"carrasco_pcd":20218031.0,"yapacani_pcd":88361477.0,"santa_rosa_pcd":26382098.0,"ende_entre_rios_pcd":15160145.0,"pau_pcd":0.0,"gcc_pcd":73940047.0,"absorcion_rio_grande_pcd":58515438.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15254055.0,"sector_electrico":5.541158248595569,"distribucion_redes":5.6239199363100765,"plantas_proc":1.096026883541051,"consumidores_dir":0.5312217998494118,"gas_comb_comp":0.31715608934097117},{"fecha":"2026-09-09","mutun":8.239128000000001,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":4.572236,"gas_comb":0.3233515361803181,"entregas_int":12.642500906951758,"carrasco_pcd":21235844.0,"yapacani_pcd":90708594.0,"santa_rosa_pcd":26197652.0,"ende_entre_rios_pcd":15099222.0,"pau_pcd":150046.6461181641,"gcc_pcd":78856367.0,"absorcion_rio_grande_pcd":56314680.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15107617.0,"sector_electrico":5.494871051303615,"distribucion_redes":5.6329745446886665,"plantas_proc":0.921493269691051,"consumidores_dir":0.5734090056849087,"gas_comb_comp":0.3431045717638327},{"fecha":"2026-09-10","mutun":8.236093,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":3.615976,"gas_comb":0.32661935688411964,"entregas_int":13.075220732855833,"carrasco_pcd":16928344.0,"yapacani_pcd":91737266.0,"santa_rosa_pcd":26127775.0,"ende_entre_rios_pcd":16079124.0,"pau_pcd":0.0,"gcc_pcd":73782438.0,"absorcion_rio_grande_pcd":55799414.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15140279.0,"sector_electrico":6.037797898942681,"distribucion_redes":5.55235621151254,"plantas_proc":0.8725514097971143,"consumidores_dir":0.5927621770199828,"gas_comb_comp":0.3463723924676342},{"fecha":"2026-09-11","mutun":8.245433999999998,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":2.095406,"gas_comb":0.32537175161427506,"entregas_int":13.490079319560005,"carrasco_pcd":16957094.0,"yapacani_pcd":91010219.0,"santa_rosa_pcd":26139355.0,"ende_entre_rios_pcd":14369941.0,"pau_pcd":712291.0,"gcc_pcd":65033457.0,"absorcion_rio_grande_pcd":54182758.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15191687.0,"sector_electrico":6.220515671559524,"distribucion_redes":5.614160985154696,"plantas_proc":0.8954760712637616,"consumidores_dir":0.740173555998508,"gas_comb_comp":0.34512478719778966},{"fecha":"2026-09-12","mutun":7.771177,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":2.217948,"gas_comb":0.3259585841093418,"entregas_int":11.504810174902058,"carrasco_pcd":21469469.0,"yapacani_pcd":91305273.0,"santa_rosa_pcd":26247445.0,"ende_entre_rios_pcd":17514314.0,"pau_pcd":2083008.056640625,"gcc_pcd":85352820.0,"absorcion_rio_grande_pcd":56296281.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15194014.0,"sector_electrico":4.425139242311241,"distribucion_redes":5.251324029627587,"plantas_proc":0.9458696051034443,"consumidores_dir":0.8627242622762697,"gas_comb_comp":0.3457116196928564},{"fecha":"2026-09-13","mutun":7.750323000000002,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":4.079937999999999,"gas_comb":0.29476505513674073,"entregas_int":10.427337424743026,"carrasco_pcd":18599750.0,"yapacani_pcd":91274602.0,"santa_rosa_pcd":26139631.0,"ende_entre_rios_pcd":24421156.0,"pau_pcd":1939523.315429688,"gcc_pcd":71740148.0,"absorcion_rio_grande_pcd":54349094.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15204475.0,"sector_electrico":3.748845416076506,"distribucion_redes":4.875454727820133,"plantas_proc":0.9401858546726404,"consumidores_dir":0.8430983905902324,"gas_comb_comp":0.31451809072025533},{"fecha":"2026-09-14","mutun":7.7378719999999985,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":3.851159,"gas_comb":0.30750279965183086,"entregas_int":11.562318725235187,"carrasco_pcd":24104250.0,"yapacani_pcd":91076883.0,"santa_rosa_pcd":26227145.0,"ende_entre_rios_pcd":28370443.0,"pau_pcd":1586264.526367188,"gcc_pcd":55349098.0,"absorcion_rio_grande_pcd":53737531.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15171637.0,"sector_electrico":4.581141510684928,"distribucion_redes":5.188048299039774,"plantas_proc":0.942747239793658,"consumidores_dir":0.8306286401333143,"gas_comb_comp":0.32725583523534546},{"fecha":"2026-09-15","mutun":7.652172000000001,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":4.995457999999999,"gas_comb":0.2819726316240381,"entregas_int":12.029408123707874,"carrasco_pcd":24545688.0,"yapacani_pcd":90821938.0,"santa_rosa_pcd":25946559.0,"ende_entre_rios_pcd":28186658.0,"pau_pcd":1750040.771484375,"gcc_pcd":59376973.0,"absorcion_rio_grande_pcd":57065945.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15164063.0,"sector_electrico":4.59899554229022,"distribucion_redes":5.422582875340214,"plantas_proc":1.1570473887332247,"consumidores_dir":0.8310292817606987,"gas_comb_comp":0.3017256672075527},{"fecha":"2026-09-16","mutun":8.098409,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":3.955484,"gas_comb":0.2785548257458376,"entregas_int":12.191447797927305,"carrasco_pcd":21024875.0,"yapacani_pcd":90463898.0,"santa_rosa_pcd":26101648.0,"ende_entre_rios_pcd":29447912.0,"pau_pcd":1756391.967773438,"gcc_pcd":63000000.0,"absorcion_rio_grande_pcd":56288871.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15065635.0,"sector_electrico":4.7552379720323845,"distribucion_redes":5.41079173694331,"plantas_proc":1.1548397062204616,"consumidores_dir":0.8508253471476376,"gas_comb_comp":0.2983078613293522},{"fecha":"2026-09-17","mutun":8.540216000000001,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.8344659999999999,"gas_comb":0.3019072650494406,"entregas_int":12.381981114829024,"carrasco_pcd":19525563.0,"yapacani_pcd":90460625.0,"santa_rosa_pcd":26092533.0,"ende_entre_rios_pcd":28722129.0,"pau_pcd":1725907.958984375,"gcc_pcd":64500000.0,"absorcion_rio_grande_pcd":57768785.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15034120.0,"sector_electrico":5.0438053748655785,"distribucion_redes":5.2984248254513435,"plantas_proc":1.1562957463645733,"consumidores_dir":0.8637021325640177,"gas_comb_comp":0.32166030063295525},{"fecha":"2026-09-18","mutun":9.449205,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.122493,"gas_comb":0.2827587405932679,"entregas_int":13.131198452930782,"carrasco_pcd":27736438.0,"yapacani_pcd":90823945.0,"santa_rosa_pcd":21881156.0,"ende_entre_rios_pcd":32201264.0,"pau_pcd":1568800.170898438,"gcc_pcd":55699512.0,"absorcion_rio_grande_pcd":58386309.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15068732.0,"sector_electrico":5.588681370496951,"distribucion_redes":5.556613561747594,"plantas_proc":1.150077571454898,"consumidores_dir":0.8160729136478264,"gas_comb_comp":0.3025117761767825},{"fecha":"2026-09-19","mutun":10.34656,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.3084638384380976,"entregas_int":11.844879389332922,"carrasco_pcd":30429563.0,"yapacani_pcd":85855266.0,"santa_rosa_pcd":22016994.0,"ende_entre_rios_pcd":29021064.0,"pau_pcd":2469590.087890625,"gcc_pcd":59685332.0,"absorcion_rio_grande_pcd":57833598.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14936022.0,"sector_electrico":4.5000000526221,"distribucion_redes":5.382247202887707,"plantas_proc":1.1677680252312908,"consumidores_dir":0.7751110730083096,"gas_comb_comp":0.32821687402161226},{"fecha":"2026-09-20","mutun":14.082441000000001,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.3894527201691015,"entregas_int":11.312035433834167,"carrasco_pcd":29561313.0,"yapacani_pcd":90305039.0,"santa_rosa_pcd":25166563.0,"ende_entre_rios_pcd":18807621.0,"pau_pcd":3435477.294921875,"gcc_pcd":58404945.0,"absorcion_rio_grande_pcd":55341117.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15113076.0,"sector_electrico":4.612392033031992,"distribucion_redes":4.683276874534563,"plantas_proc":1.176228813660394,"consumidores_dir":0.8203846770237015,"gas_comb_comp":0.4092057557526161},{"fecha":"2026-09-21","mutun":8.564305999999998,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.31856656046055876,"entregas_int":12.86367457922011,"carrasco_pcd":28083250.0,"yapacani_pcd":89966430.0,"santa_rosa_pcd":26380329.0,"ende_entre_rios_pcd":30238082.0,"pau_pcd":4997076.66015625,"gcc_pcd":58407766.0,"absorcion_rio_grande_pcd":52386129.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15124342.0,"sector_electrico":5.555240703583891,"distribucion_redes":5.251362271057512,"plantas_proc":1.203546571512517,"consumidores_dir":0.8337719974826773,"gas_comb_comp":0.3383195960440734},{"fecha":"2026-09-22","mutun":8.127531999999999,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.86,"gas_comb":0.3174346487955116,"entregas_int":12.085811585693875,"carrasco_pcd":28908688.0,"yapacani_pcd":89908063.0,"santa_rosa_pcd":26309865.0,"ende_entre_rios_pcd":28229762.0,"pau_pcd":20960975.0,"gcc_pcd":60090367.0,"absorcion_rio_grande_pcd":56148422.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15115686.0,"sector_electrico":4.414187766026308,"distribucion_redes":5.218181365280542,"plantas_proc":1.6378654554713772,"consumidores_dir":0.7958239633321349,"gas_comb_comp":0.3371876843790262},{"fecha":"2026-09-23","mutun":9.042064000000002,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.3294409016620881,"entregas_int":12.88249897938542,"carrasco_pcd":40975500.0,"yapacani_pcd":90089859.0,"santa_rosa_pcd":26280508.0,"ende_entre_rios_pcd":29197684.0,"pau_pcd":37843664.0,"gcc_pcd":63576954.0,"absorcion_rio_grande_pcd":60067637.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14987898.0,"sector_electrico":4.555888987977112,"distribucion_redes":5.31448101715992,"plantas_proc":2.1399508453413985,"consumidores_dir":0.8524250933234744,"gas_comb_comp":0.34919393724560266},{"fecha":"2026-09-24","mutun":9.540827,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.3015658746514437,"entregas_int":12.672920750972144,"carrasco_pcd":46026875.0,"yapacani_pcd":90484609.0,"santa_rosa_pcd":26316375.0,"ende_entre_rios_pcd":28929010.0,"pau_pcd":42221758.0,"gcc_pcd":59422262.0,"absorcion_rio_grande_pcd":57016828.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14956607.0,"sector_electrico":4.3475234694059015,"distribucion_redes":5.1683186889796255,"plantas_proc":2.2358732860570365,"consumidores_dir":0.9014522709460667,"gas_comb_comp":0.3213189102349583},{"fecha":"2026-09-25","mutun":9.825248,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.36577255995287833,"entregas_int":13.259765856696752,"carrasco_pcd":46060625.0,"yapacani_pcd":90200758.0,"santa_rosa_pcd":26197320.0,"ende_entre_rios_pcd":29454340.0,"pau_pcd":42840656.0,"gcc_pcd":59822250.0,"absorcion_rio_grande_pcd":55842875.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14905534.0,"sector_electrico":4.815404745532113,"distribucion_redes":5.176286811951733,"plantas_proc":2.246425986794666,"consumidores_dir":1.0018952768347265,"gas_comb_comp":0.385525595536393},{"fecha":"2026-09-26","mutun":10.621644,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.36582864393726056,"entregas_int":13.228673981861492,"carrasco_pcd":45406188.0,"yapacani_pcd":89300391.0,"santa_rosa_pcd":25976801.0,"ende_entre_rios_pcd":29598090.0,"pau_pcd":42943555.0,"gcc_pcd":70097195.0,"absorcion_rio_grande_pcd":56418871.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14910713.0,"sector_electrico":5.047670618253803,"distribucion_redes":4.878971889611887,"plantas_proc":2.3158969855272877,"consumidores_dir":0.9663814528850011,"gas_comb_comp":0.38558167952077516},{"fecha":"2026-09-27","mutun":10.628066,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.3466165572423262,"entregas_int":12.762641263907206,"carrasco_pcd":45535938.0,"yapacani_pcd":89582445.0,"santa_rosa_pcd":26283914.0,"ende_entre_rios_pcd":29895494.0,"pau_pcd":43902316.0,"gcc_pcd":69326961.0,"absorcion_rio_grande_pcd":56621867.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":15012191.0,"sector_electrico":4.982796164314355,"distribucion_redes":4.534268782724491,"plantas_proc":2.1644335341899157,"consumidores_dir":1.0613897470949278,"gas_comb_comp":0.36636959282584086},{"fecha":"2026-09-28","mutun":9.300826,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.21780109266083333,"entregas_int":14.114043146578698,"carrasco_pcd":48280207.0,"yapacani_pcd":89796844.0,"santa_rosa_pcd":26255334.0,"ende_entre_rios_pcd":32387643.0,"pau_pcd":43940551.0,"gcc_pcd":65483496.0,"absorcion_rio_grande_pcd":54221371.0,"gtc_a_gaa_pcd":4722000.0,"percheles_pcd":15061558.0,"sector_electrico":6.024791692697656,"distribucion_redes":4.873335378313869,"plantas_proc":2.1044262813167696,"consumidores_dir":1.0917367586668867,"gas_comb_comp":0.23755412824434788},{"fecha":"2026-09-29","mutun":10.260222,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.41441010836888753,"entregas_int":14.232534686204481,"carrasco_pcd":62935516.0,"yapacani_pcd":89691180.0,"santa_rosa_pcd":26158791.0,"ende_entre_rios_pcd":30856791.0,"pau_pcd":46958461.0,"gcc_pcd":62935516.0,"absorcion_rio_grande_pcd":52656699.0,"gtc_a_gaa_pcd":8314000.0,"percheles_pcd":15026833.0,"sector_electrico":6.028826818227415,"distribucion_redes":5.20707949322196,"plantas_proc":1.9963055432775276,"consumidores_dir":0.9805697958940666,"gas_comb_comp":0.43416314395240213},{"fecha":"2026-09-30","mutun":9.855201,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.476421861598701,"entregas_int":13.170182662561762,"carrasco_pcd":48616188.0,"yapacani_pcd":89677750.0,"santa_rosa_pcd":26210666.0,"ende_entre_rios_pcd":28611707.0,"pau_pcd":49413516.0,"gcc_pcd":73698531.0,"absorcion_rio_grande_pcd":47224445.0,"gtc_a_gaa_pcd":7043000.0,"percheles_pcd":14973490.0,"sector_electrico":4.646129116253308,"distribucion_redes":5.28897984150507,"plantas_proc":2.1349681430891017,"consumidores_dir":1.0803525261307678,"gas_comb_comp":0.49617489718221564},{"fecha":"2026-10-01","mutun":8.7,"san_matias":0.0,"gas_transito_mmpcd":0.0,"gija":0.0,"gas_comb":0.41,"entregas_int":13.54,"carrasco_pcd":48722516.0,"yapacani_pcd":88990305.0,"santa_rosa_pcd":26122188.0,"ende_entre_rios_pcd":15008839.0,"pau_pcd":49494105.0,"gcc_pcd":76731375.0,"absorcion_rio_grande_pcd":45426969.0,"gtc_a_gaa_pcd":3296000.0,"percheles_pcd":14980062.0,"sector_electrico":4.7217277943523674,"distribucion_redes":5.44,"plantas_proc":2.3451255102801905,"consumidores_dir":1.0109131653551695,"gas_comb_comp":0.43},{"fecha":"2026-10-02","mutun":8.486300999999997,"san_matias":0.0,"gas_transito_mmpcd":21293967.0,"gija":0.0,"gas_comb":0.3763824974685332,"entregas_int":13.44271120651585,"carrasco_pcd":35145859.0,"yapacani_pcd":89355781.0,"santa_rosa_pcd":26051189.0,"ende_entre_rios_pcd":29074742.0,"pau_pcd":34610230.0,"gcc_pcd":79504711.0,"absorcion_rio_grande_pcd":51025465.0,"gtc_a_gaa_pcd":1321000.0,"percheles_pcd":14806886.0,"sector_electrico":4.937052183513421,"distribucion_redes":5.573733541727698,"plantas_proc":1.8812790891883526,"consumidores_dir":1.030893356502865,"gas_comb_comp":0.3961355330520478},{"fecha":"2026-10-03","mutun":8.073887,"san_matias":0.0,"gas_transito_mmpcd":35824586.0,"gija":0.0,"gas_comb":0.37073647716710323,"entregas_int":13.18168115739272,"carrasco_pcd":41916578.0,"yapacani_pcd":85889523.0,"santa_rosa_pcd":25911846.0,"ende_entre_rios_pcd":29380094.0,"pau_pcd":42581805.0,"gcc_pcd":76948984.0,"absorcion_rio_grande_pcd":58113797.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14901627.0,"sector_electrico":4.6828641736559335,"distribucion_redes":5.351088636668729,"plantas_proc":2.0647285175470578,"consumidores_dir":1.0632467939374821,"gas_comb_comp":0.3904895127506179},{"fecha":"2026-10-04","mutun":9.990575,"san_matias":0.0,"gas_transito_mmpcd":35223262.0,"gija":0.0,"gas_comb":0.38697809504443814,"entregas_int":12.314694922835395,"carrasco_pcd":41203609.0,"yapacani_pcd":89151555.0,"santa_rosa_pcd":24891838.0,"ende_entre_rios_pcd":29208828.0,"pau_pcd":41763207.0,"gcc_pcd":71009563.0,"absorcion_rio_grande_pcd":59583211.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14873816.0,"sector_electrico":4.406716844131935,"distribucion_redes":4.771048541531753,"plantas_proc":2.0686134230217434,"consumidores_dir":1.0485630785664501,"gas_comb_comp":0.4067311306279527},{"fecha":"2026-10-05","mutun":8.453739,"san_matias":0.0,"gas_transito_mmpcd":33422109.0,"gija":0.0,"gas_comb":0.4760229179130892,"entregas_int":14.080615038880179,"carrasco_pcd":42057688.0,"yapacani_pcd":89270086.0,"santa_rosa_pcd":24655494.0,"ende_entre_rios_pcd":34319238.0,"pau_pcd":42393918.0,"gcc_pcd":68012086.0,"absorcion_rio_grande_pcd":58647012.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14827580.0,"sector_electrico":5.735968617995446,"distribucion_redes":5.264527825040362,"plantas_proc":2.1033801403495893,"consumidores_dir":0.9569854199112657,"gas_comb_comp":0.4957759534966038},{"fecha":"2026-10-06","mutun":9.041639,"san_matias":0.0,"gas_transito_mmpcd":33429621.0,"gija":0.0,"gas_comb":0.3199345781671569,"entregas_int":13.95347936944638,"carrasco_pcd":44259031.0,"yapacani_pcd":89457016.0,"santa_rosa_pcd":24747283.0,"ende_entre_rios_pcd":33140836.0,"pau_pcd":44264066.0,"gcc_pcd":66944805.0,"absorcion_rio_grande_pcd":58266359.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14910758.0,"sector_electrico":5.253924246290897,"distribucion_redes":5.3097072196890736,"plantas_proc":2.350540689773373,"consumidores_dir":1.019554178109523,"gas_comb_comp":0.3396876137506714},{"fecha":"2026-10-07","mutun":10.627895999999996,"san_matias":0.40936283442428717,"gas_transito_mmpcd":33374668.0,"gija":0.0,"gas_comb":0.42660812297178036,"entregas_int":13.772910862812015,"carrasco_pcd":42686000.0,"yapacani_pcd":89403398.0,"santa_rosa_pcd":25047248.0,"ende_entre_rios_pcd":31514830.0,"pau_pcd":39968852.0,"gcc_pcd":73199266.0,"absorcion_rio_grande_pcd":56416652.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14199266.0,"sector_electrico":5.097581932946604,"distribucion_redes":5.3742054010852325,"plantas_proc":2.2412790198043218,"consumidores_dir":1.0400914733923405,"gas_comb_comp":0.44636115855529496},{"fecha":"2026-10-08","mutun":8.671211999999999,"san_matias":0.0,"gas_transito_mmpcd":43031535.15625,"gija":0.0,"gas_comb":0.41158953392348113,"entregas_int":14.395892444547362,"carrasco_pcd":38893969.0,"yapacani_pcd":89331656.0,"santa_rosa_pcd":25578471.0,"ende_entre_rios_pcd":32522416.015625,"pau_pcd":36224296.875,"gcc_pcd":72725914.0,"absorcion_rio_grande_pcd":56771051.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14814898.0,"sector_electrico":5.889689892132306,"distribucion_redes":5.341304714762875,"plantas_proc":2.0748736033925503,"consumidores_dir":1.0702711986761175,"gas_comb_comp":0.43134256950699573},{"fecha":"2026-10-09","mutun":10.628876000000002,"san_matias":0.0,"gas_transito_mmpcd":42601812.5,"gija":0.0,"gas_comb":0.3531286398405403,"entregas_int":14.110262026521445,"carrasco_pcd":36368063.0,"yapacani_pcd":89260906.0,"santa_rosa_pcd":25840889.0,"ende_entre_rios_pcd":32034003.90625,"pau_pcd":35256410.15625,"gcc_pcd":68614390.625,"absorcion_rio_grande_pcd":59256714.84375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14756185.546875,"sector_electrico":5.850977331726595,"distribucion_redes":5.154085480107001,"plantas_proc":2.072189032050895,"consumidores_dir":1.0132571470534435,"gas_comb_comp":0.3728816754240549},{"fecha":"2026-10-10","mutun":9.078477,"san_matias":0.0,"gas_transito_mmpcd":46200855.46875,"gija":0.0,"gas_comb":0.3307696263067778,"entregas_int":11.373739476883166,"carrasco_pcd":36055094.0,"yapacani_pcd":89111664.0,"santa_rosa_pcd":25890844.0,"ende_entre_rios_pcd":30583843.75,"pau_pcd":36194363.28125,"gcc_pcd":19746046.875,"absorcion_rio_grande_pcd":59347046.875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14942862.3046875,"sector_electrico":4.239455716406118,"distribucion_redes":4.158540991889786,"plantas_proc":2.1187130326391275,"consumidores_dir":0.8372767003646202,"gas_comb_comp":0.35052266189029235},{"fecha":"2026-10-11","mutun":9.735069000000001,"san_matias":0.0,"gas_transito_mmpcd":46193769.53125,"gija":0.0,"gas_comb":0.3760624771161792,"entregas_int":11.16835217525707,"carrasco_pcd":35936500.0,"yapacani_pcd":89283531.0,"santa_rosa_pcd":25935076.0,"ende_entre_rios_pcd":37598804.6875,"pau_pcd":36194363.0,"gcc_pcd":63598253.90625,"absorcion_rio_grande_pcd":57837472.65625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14895251.953125,"sector_electrico":4.1282941151466535,"distribucion_redes":4.0676191008501155,"plantas_proc":2.1454458460924766,"consumidores_dir":0.8072400775843122,"gas_comb_comp":0.39581551269969384},{"fecha":"2026-10-12","mutun":10.891285999999997,"san_matias":0.0,"gas_transito_mmpcd":49682488.28125,"gija":0.0,"gas_comb":0.3788856234367377,"entregas_int":12.62936117983549,"carrasco_pcd":35406750.0,"yapacani_pcd":89280391.0,"santa_rosa_pcd":25791238.0,"ende_entre_rios_pcd":39225195.3125,"pau_pcd":32030189.453125,"gcc_pcd":62644125.0,"absorcion_rio_grande_pcd":55418744.75097656,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14077308.59375,"sector_electrico":4.644871513823128,"distribucion_redes":5.080640300003161,"plantas_proc":1.9329746262566523,"consumidores_dir":0.9511217041690326,"gas_comb_comp":0.3986386590202523},{"fecha":"2026-10-13","mutun":10.367005,"san_matias":0.0,"gas_transito_mmpcd":42081857.472,"gija":0.0,"gas_comb":0.3977970509926465,"entregas_int":12.974845498693158,"carrasco_pcd":34723188.0,"yapacani_pcd":88867508.0,"santa_rosa_pcd":25711729.0,"ende_entre_rios_pcd":39506687.5,"pau_pcd":35248476.5625,"gcc_pcd":62431429.6875,"absorcion_rio_grande_pcd":51577207.03125,"gtc_a_gaa_pcd":0.0,"percheles_pcd":13916727.5390625,"sector_electrico":4.7519060389031225,"distribucion_redes":5.144500376903838,"plantas_proc":2.01669367059766,"consumidores_dir":1.041992376705024,"gas_comb_comp":0.41755008657616105},{"fecha":"2026-10-14","mutun":9.823550000000001,"san_matias":0.0,"gas_transito_mmpcd":42064482.984,"gija":0.0,"gas_comb":0.3967157505623618,"entregas_int":12.988587942606946,"carrasco_pcd":34733313.0,"yapacani_pcd":88806703.0,"santa_rosa_pcd":25956086.0,"ende_entre_rios_pcd":38773621.09375,"pau_pcd":35261035.15625,"gcc_pcd":66837429.6875,"absorcion_rio_grande_pcd":51251812.5,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14006833.0078125,"sector_electrico":4.6181622343649655,"distribucion_redes":5.277837319810539,"plantas_proc":1.997455068424323,"consumidores_dir":1.0748928651275633,"gas_comb_comp":0.4169562054419165},{"fecha":"2026-10-15","mutun":11.924631000000002,"san_matias":0.0,"gas_transito_mmpcd":35069662.434,"gija":0.0,"gas_comb":0.3890190326076314,"entregas_int":12.938959564021655,"carrasco_pcd":36110563.0,"yapacani_pcd":88393680.0,"santa_rosa_pcd":25992230.0,"ende_entre_rios_pcd":39723343.75,"pau_pcd":36667796.875,"gcc_pcd":63786347.65625,"absorcion_rio_grande_pcd":58327503.90625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14082190.4296875,"sector_electrico":4.4282323319604195,"distribucion_redes":5.376116911821796,"plantas_proc":2.0634191617359723,"consumidores_dir":1.0509507036239094,"gas_comb_comp":0.40925948748718605},{"fecha":"2026-10-16","mutun":10.481622,"san_matias":0.0,"gas_transito_mmpcd":44152140.722,"gija":0.0,"gas_comb":0.36189504331047917,"entregas_int":13.248642225092022,"carrasco_pcd":37551563.0,"yapacani_pcd":88404156.0,"santa_rosa_pcd":25932936.0,"ende_entre_rios_pcd":40659226.5625,"pau_pcd":38107546.875,"gcc_pcd":65952914.0625,"absorcion_rio_grande_pcd":57538691.40625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14019034.1796875,"sector_electrico":4.467549780960076,"distribucion_redes":5.575754975564277,"plantas_proc":2.1193208214309367,"consumidores_dir":1.0657761922571758,"gas_comb_comp":0.38213549819003384},{"fecha":"2026-10-17","mutun":10.081218,"san_matias":0.0,"gas_transito_mmpcd":44273656.196,"gija":0.0,"gas_comb":0.33050259594148856,"entregas_int":12.808893130375509,"carrasco_pcd":35210063.0,"yapacani_pcd":88907109.0,"santa_rosa_pcd":26060332.0,"ende_entre_rios_pcd":39135558.59375,"pau_pcd":35405859.375,"gcc_pcd":68541765.625,"absorcion_rio_grande_pcd":56840464.84375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":13878179.6875,"sector_electrico":4.373213117012581,"distribucion_redes":5.291634085332549,"plantas_proc":2.0881882752851664,"consumidores_dir":1.0356171978656548,"gas_comb_comp":0.3507430508210433},{"fecha":"2026-10-18","mutun":10.594621999999998,"san_matias":0.0,"gas_transito_mmpcd":45718422.564,"gija":0.0,"gas_comb":0.3205229480740825,"entregas_int":11.238964741219862,"carrasco_pcd":23880375.0,"yapacani_pcd":88869813.0,"santa_rosa_pcd":25994586.0,"ende_entre_rios_pcd":32741951.171875,"pau_pcd":14903429.6875,"gcc_pcd":71030867.1875,"absorcion_rio_grande_pcd":55928179.6875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":13968825.1953125,"sector_electrico":4.027473653761932,"distribucion_redes":4.670409184086405,"plantas_proc":1.5400797036398504,"consumidores_dir":0.9807617448521211,"gas_comb_comp":0.3407634029536373},{"fecha":"2026-10-19","mutun":12.42488,"san_matias":0.0,"gas_transito_mmpcd":31503972.54,"gija":0.0,"gas_comb":0.3556038654480552,"entregas_int":12.60867777923609,"carrasco_pcd":20625813.0,"yapacani_pcd":88801609.0,"santa_rosa_pcd":26033268.0,"ende_entre_rios_pcd":39373531.25,"pau_pcd":3676419.921875,"gcc_pcd":58513425.78125,"absorcion_rio_grande_pcd":57128007.8125,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14465161.1328125,"sector_electrico":5.08721368838625,"distribucion_redes":5.279526443394437,"plantas_proc":1.2210401735311567,"consumidores_dir":1.000657019044691,"gas_comb_comp":0.3758443203276099},{"fecha":"2026-10-20","mutun":10.460643999999998,"san_matias":0.0,"gas_transito_mmpcd":31554930.642,"gija":0.0,"gas_comb":0.35077201202813263,"entregas_int":13.239904804694003,"carrasco_pcd":23538813.0,"yapacani_pcd":88812844.0,"santa_rosa_pcd":26017277.0,"ende_entre_rios_pcd":39359437.5,"pau_pcd":2731153.076171875,"gcc_pcd":63032875.0,"absorcion_rio_grande_pcd":55271449.21875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14639022.4609375,"sector_electrico":5.54909996643947,"distribucion_redes":5.3849352077194315,"plantas_proc":1.25549045319249,"consumidores_dir":1.0301387224630592,"gas_comb_comp":0.3710124669076873},{"fecha":"2026-10-21","mutun":10.354461,"san_matias":0.0,"gas_transito_mmpcd":38935591.956,"gija":0.0,"gas_comb":0.33457915827013357,"entregas_int":13.851280165384733,"carrasco_pcd":16786563.0,"yapacani_pcd":88495055.0,"santa_rosa_pcd":26054605.0,"ende_entre_rios_pcd":41629542.96875,"pau_pcd":2909449.462890625,"gcc_pcd":70035062.5,"absorcion_rio_grande_pcd":57169867.1875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14613913.0859375,"sector_electrico":6.111406034043264,"distribucion_redes":5.406944928866208,"plantas_proc":1.2439706754955568,"consumidores_dir":1.068718072100148,"gas_comb_comp":0.3548196131496883},{"fecha":"2026-10-22","mutun":10.262120000000001,"san_matias":0.0,"gas_transito_mmpcd":46021063.544,"gija":0.700114,"gas_comb":0.3381942859282089,"entregas_int":13.667263396928996,"carrasco_pcd":20185250.0,"yapacani_pcd":89083516.0,"santa_rosa_pcd":25946609.0,"ende_entre_rios_pcd":40304742.1875,"pau_pcd":1675923.583984375,"gcc_pcd":81673210.9375,"absorcion_rio_grande_pcd":56259289.0625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14639207.03125,"sector_electrico":6.02744454151179,"distribucion_redes":5.404288189978567,"plantas_proc":1.1418113268343824,"consumidores_dir":1.0734788837247007,"gas_comb_comp":0.3584347408077636},{"fecha":"2026-10-23","mutun":10.370115999999998,"san_matias":0.0,"gas_transito_mmpcd":45865929.142,"gija":0.551258,"gas_comb":0.35140042527333903,"entregas_int":13.35889164512374,"carrasco_pcd":21731625.0,"yapacani_pcd":88495469.0,"santa_rosa_pcd":25890170.0,"ende_entre_rios_pcd":39503984.375,"pau_pcd":2949859.619140625,"gcc_pcd":74911210.9375,"absorcion_rio_grande_pcd":56175609.375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14635222.65625,"sector_electrico":5.645437067771853,"distribucion_redes":5.442029706717018,"plantas_proc":1.1869890099527376,"consumidores_dir":1.0641954058025742,"gas_comb_comp":0.37164088015289376},{"fecha":"2026-10-24","mutun":8.543615,"san_matias":0.0,"gas_transito_mmpcd":44057322.63199999,"gija":0.0,"gas_comb":0.39179199814468635,"entregas_int":12.967231123953452,"carrasco_pcd":21350375.0,"yapacani_pcd":84146008.0,"santa_rosa_pcd":26244098.0,"ende_entre_rios_pcd":38804726.5625,"pau_pcd":2842573.2421875,"gcc_pcd":76594625.0,"absorcion_rio_grande_pcd":52024566.40625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14649284.1796875,"sector_electrico":5.376916794202867,"distribucion_redes":5.30765381086646,"plantas_proc":1.1890361770135758,"consumidores_dir":1.0733838869909946,"gas_comb_comp":0.4120324530242411},{"fecha":"2026-10-25","mutun":10.073656000000001,"san_matias":0.0,"gas_transito_mmpcd":46295453.324,"gija":1.931003,"gas_comb":0.31757482373138474,"entregas_int":10.832829023998599,"carrasco_pcd":25643438.0,"yapacani_pcd":90223523.0,"santa_rosa_pcd":26153898.0,"ende_entre_rios_pcd":32245035.15625,"pau_pcd":2780453.61328125,"gcc_pcd":74172687.5,"absorcion_rio_grande_pcd":54294808.59375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14755805.6640625,"sector_electrico":3.901489242807673,"distribucion_redes":4.7174262469883175,"plantas_proc":1.1911162949173848,"consumidores_dir":1.0025567844056704,"gas_comb_comp":0.33781527861093946},{"fecha":"2026-10-26","mutun":10.211134,"san_matias":0.0,"gas_transito_mmpcd":46269497.533999994,"gija":2.00022,"gas_comb":0.3186505790603167,"entregas_int":11.84044228042332,"carrasco_pcd":24080375.0,"yapacani_pcd":89993922.0,"santa_rosa_pcd":26250264.0,"ende_entre_rios_pcd":38782445.3125,"pau_pcd":2549147.94921875,"gcc_pcd":57259199.21875,"absorcion_rio_grande_pcd":56186617.1875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14718811.5234375,"sector_electrico":4.590836168460358,"distribucion_redes":5.415543826199634,"plantas_proc":1.1829740302865868,"consumidores_dir":0.6308478005971859,"gas_comb_comp":0.3388910339398714},{"fecha":"2026-10-27","mutun":10.866351999999997,"san_matias":0.0,"gas_transito_mmpcd":45126348.04000001,"gija":1.800226,"gas_comb":0.3177996120203485,"entregas_int":11.859883569325458,"carrasco_pcd":23922857.0,"yapacani_pcd":90009883.0,"santa_rosa_pcd":26174695.0,"ende_entre_rios_pcd":37611070.0,"pau_pcd":2587174.0,"gcc_pcd":60762808.59375,"absorcion_rio_grande_pcd":56186617.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14689507.8125,"sector_electrico":4.547502780209345,"distribucion_redes":5.471006787079317,"plantas_proc":1.179807067131626,"consumidores_dir":0.6413264800256174,"gas_comb_comp":0.33804006689990324},{"fecha":"2026-10-28","mutun":13.428588999999999,"san_matias":0.0,"gas_transito_mmpcd":31661720.178,"gija":2.300259,"gas_comb":0.3673336385118198,"entregas_int":11.507551897625413,"carrasco_pcd":24178467.0,"yapacani_pcd":88350945.0,"santa_rosa_pcd":26282727.0,"ende_entre_rios_pcd":33365324.21875,"pau_pcd":2687495.1171875,"gcc_pcd":70309773.4375,"absorcion_rio_grande_pcd":56232476.5625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14717353.515625,"sector_electrico":3.9927565344210607,"distribucion_redes":5.584398526451633,"plantas_proc":1.1895137342562943,"consumidores_dir":0.7206426476168712,"gas_comb_comp":0.38757409339137455},{"fecha":"2026-10-29","mutun":12.801521,"san_matias":0.0,"gas_transito_mmpcd":42618065.247999996,"gija":4.102067,"gas_comb":0.33557142805231827,"entregas_int":11.596202603203274,"carrasco_pcd":25578449.0,"yapacani_pcd":90300789.0,"santa_rosa_pcd":26109969.0,"ende_entre_rios_pcd":34147273.4375,"pau_pcd":2571839.599609375,"gcc_pcd":60992207.03125,"absorcion_rio_grande_pcd":58891730.46875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14665561.5234375,"sector_electrico":3.97326686883079,"distribucion_redes":5.598535282546812,"plantas_proc":1.1968332675941522,"consumidores_dir":0.8073267293519629,"gas_comb_comp":0.35581188293187305},{"fecha":"2026-10-30","mutun":12.04,"san_matias":0.0,"gas_transito_mmpcd":42208246.28,"gija":4.0,"gas_comb":0.33,"entregas_int":11.58,"carrasco_pcd":25961430.0,"yapacani_pcd":90719820.0,"santa_rosa_pcd":25927133.0,"ende_entre_rios_pcd":33937043.0,"pau_pcd":2595311.0,"gcc_pcd":61794035.0,"absorcion_rio_grande_pcd":56987828.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14667214.0,"sector_electrico":4.07,"distribucion_redes":5.52,"plantas_proc":1.18,"consumidores_dir":0.8,"gas_comb_comp":0.35},{"fecha":"2026-10-31","mutun":11.99,"san_matias":0.0,"gas_transito_mmpcd":40227342.76,"gija":0.0,"gas_comb":0.32,"entregas_int":11.22,"carrasco_pcd":26310641.0,"yapacani_pcd":90652977.0,"santa_rosa_pcd":26207492.0,"ende_entre_rios_pcd":34937820.0,"pau_pcd":2679118.0,"gcc_pcd":58673648.0,"absorcion_rio_grande_pcd":57406434.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14590173.0,"sector_electrico":3.88,"distribucion_redes":5.35,"plantas_proc":1.18,"consumidores_dir":0.79,"gas_comb_comp":0.34},{"fecha":"2026-11-01","mutun":9.267071,"san_matias":0.0,"gas_transito_mmpcd":43945453.125,"gija":0.0,"gas_comb":0.31827949012039336,"entregas_int":10.036217918842638,"carrasco_pcd":26096766.0,"yapacani_pcd":90520750.0,"santa_rosa_pcd":26083719.0,"ende_entre_rios_pcd":35138184.0,"pau_pcd":2538294.0,"gcc_pcd":48498555.0,"absorcion_rio_grande_pcd":54675160.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14635319.0,"sector_electrico":3.5702187219676795,"distribucion_redes":4.535316211396391,"plantas_proc":1.1462426925634752,"consumidores_dir":0.7641998380355374,"gas_comb_comp":0.33851994499994803},{"fecha":"2026-11-02","mutun":10.340187,"san_matias":0.0,"gas_transito_mmpcd":44610132.8125,"gija":0.0,"gas_comb":0.42826601358874744,"entregas_int":10.363037136656947,"carrasco_pcd":26849625.0,"yapacani_pcd":90421164.0,"santa_rosa_pcd":26057945.0,"ende_entre_rios_pcd":37814504.0,"pau_pcd":2585448.0,"gcc_pcd":29933672.0,"absorcion_rio_grande_pcd":55151125.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14587651.0,"sector_electrico":4.13399472852794,"distribucion_redes":4.357056144430483,"plantas_proc":1.13572577763312,"consumidores_dir":0.7160200311858501,"gas_comb_comp":0.44850646846830217},{"fecha":"2026-11-03","mutun":9.865254999999998,"san_matias":0.0,"gas_transito_mmpcd":45823257.8125,"gija":0.0,"gas_comb":0.34614005555649024,"entregas_int":12.464630920215999,"carrasco_pcd":21176203.0,"yapacani_pcd":90669570.0,"santa_rosa_pcd":25942547.0,"ende_entre_rios_pcd":42558437.5,"pau_pcd":3111265.625,"gcc_pcd":64588617.1875,"absorcion_rio_grande_pcd":57010382.8125,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14569486.328125,"sector_electrico":5.597139733308161,"distribucion_redes":5.07477658175684,"plantas_proc":1.1333066520927146,"consumidores_dir":0.6391674981787288,"gas_comb_comp":0.36638051043604497},{"fecha":"2026-11-04","mutun":9.222587999999998,"san_matias":0.0,"gas_transito_mmpcd":49312023.4375,"gija":0.0,"gas_comb":0.37,"entregas_int":13.239296097707241,"carrasco_pcd":21275547.0,"yapacani_pcd":90336602.0,"santa_rosa_pcd":25946307.0,"ende_entre_rios_pcd":44370253.90625,"pau_pcd":2463292.236328125,"gcc_pcd":51014519.53125,"absorcion_rio_grande_pcd":54178304.6875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14506788.0859375,"sector_electrico":6.023911321895281,"distribucion_redes":5.279350960437873,"plantas_proc":1.1379971041847854,"consumidores_dir":0.6493510411814234,"gas_comb_comp":0.3865983631498285},{"fecha":"2026-11-05","mutun":8.392302,"san_matias":0.0,"gas_transito_mmpcd":46705007.8125,"gija":0.0,"gas_comb":0.3589331462018674,"entregas_int":12.205798200933119,"carrasco_pcd":21644922.0,"yapacani_pcd":90196117.0,"santa_rosa_pcd":26017236.0,"ende_entre_rios_pcd":41506707.03125,"pau_pcd":2853650.634765625,"gcc_pcd":73444320.3125,"absorcion_rio_grande_pcd":54429246.09375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14588729.4921875,"sector_electrico":4.76578490636185,"distribucion_redes":5.372339446217695,"plantas_proc":1.1412042583136386,"consumidores_dir":0.9062291351603813,"gas_comb_comp":0.3791736010814221},{"fecha":"2026-11-06","mutun":9.269724999999998,"san_matias":0.0,"gas_transito_mmpcd":49431301.21,"gija":0.0,"gas_comb":0.4255116097465786,"entregas_int":13.046089175902429,"carrasco_pcd":19467672.0,"yapacani_pcd":90240273.0,"santa_rosa_pcd":25847570.0,"ende_entre_rios_pcd":45209750.0,"pau_pcd":2607458.740234375,"gcc_pcd":64838980.46875,"absorcion_rio_grande_pcd":55064519.53125,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14503980.46875,"sector_electrico":5.604367084678401,"distribucion_redes":5.461246963843981,"plantas_proc":1.1805264991663202,"consumidores_dir":0.7797081733341691,"gas_comb_comp":0.44575206462613337},{"fecha":"2026-11-07","mutun":9.309867999999998,"san_matias":0.0,"gas_transito_mmpcd":47289365.854,"gija":0.0,"gas_comb":0.3844828037496288,"entregas_int":12.594273406075633,"carrasco_pcd":21930234.0,"yapacani_pcd":85215984.0,"santa_rosa_pcd":25010912.0,"ende_entre_rios_pcd":42644406.25,"pau_pcd":2461842.529296875,"gcc_pcd":62938089.84375,"absorcion_rio_grande_pcd":57872195.3125,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14595531.25,"sector_electrico":5.411488725044631,"distribucion_redes":5.0782114773532,"plantas_proc":1.2185078088004306,"consumidores_dir":0.8658249399978157,"gas_comb_comp":0.4047232586291835},{"fecha":"2026-11-08","mutun":9.921792000000002,"san_matias":0.0,"gas_transito_mmpcd":47370835.252,"gija":0.0,"gas_comb":0.32121348936732813,"entregas_int":10.511088471642612,"carrasco_pcd":26005250.0,"yapacani_pcd":89891977.0,"santa_rosa_pcd":25467814.0,"ende_entre_rios_pcd":34159449.21875,"pau_pcd":2779330.322265625,"gcc_pcd":60873664.0625,"absorcion_rio_grande_pcd":59589554.6875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14543638.671875,"sector_electrico":3.8317584691437694,"distribucion_redes":4.659737785272075,"plantas_proc":1.1788128340677722,"consumidores_dir":0.8205389282794388,"gas_comb_comp":0.3414539442468829},{"fecha":"2026-11-09","mutun":11.437438999999998,"san_matias":0.0,"gas_transito_mmpcd":48383852.656,"gija":0.0,"gas_comb":0.3817677137824833,"entregas_int":12.448063221119845,"carrasco_pcd":22669531.0,"yapacani_pcd":90343578.0,"santa_rosa_pcd":25985125.0,"ende_entre_rios_pcd":43122550.78125,"pau_pcd":2588593.994140625,"gcc_pcd":58662925.78125,"absorcion_rio_grande_pcd":56067582.03125,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14584792.96875,"sector_electrico":5.124633284877236,"distribucion_redes":5.204648674056772,"plantas_proc":1.1115951116303782,"consumidores_dir":0.9869456956759044,"gas_comb_comp":0.40200816866203803},{"fecha":"2026-11-10","mutun":10.813192,"san_matias":0.0,"gas_transito_mmpcd":42398129.656,"gija":0.0,"gas_comb":0.3997801688722337,"entregas_int":12.898144263628202,"carrasco_pcd":21408125.0,"yapacani_pcd":90136195.0,"santa_rosa_pcd":25699639.0,"ende_entre_rios_pcd":43552925.78125,"pau_pcd":2802511.474609375,"gcc_pcd":68483390.625,"absorcion_rio_grande_pcd":52458460.9375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14543408.203125,"sector_electrico":5.424788181734061,"distribucion_redes":5.32412391977025,"plantas_proc":1.1112958862704092,"consumidores_dir":1.017695820973923,"gas_comb_comp":0.4200206237517884},{"fecha":"2026-11-11","mutun":10.631545000000001,"san_matias":0.0,"gas_transito_mmpcd":46260351.208,"gija":0.0,"gas_comb":0.42341690266830767,"entregas_int":13.50465577644924,"carrasco_pcd":18218531.0,"yapacani_pcd":88111039.0,"santa_rosa_pcd":25886652.0,"ende_entre_rios_pcd":45981859.375,"pau_pcd":2733218.017578125,"gcc_pcd":74515781.25,"absorcion_rio_grande_pcd":53694773.4375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14481748.046875,"sector_electrico":6.070774531065815,"distribucion_redes":5.327136481413005,"plantas_proc":1.1020641531810595,"consumidores_dir":0.9844401559098069,"gas_comb_comp":0.4436573575478624},{"fecha":"2026-11-12","mutun":8.957404,"san_matias":0.0,"gas_transito_mmpcd":46384868.372,"gija":0.0,"gas_comb":0.3647059784536276,"entregas_int":13.459741464509959,"carrasco_pcd":18256688.0,"yapacani_pcd":89727117.0,"santa_rosa_pcd":26054207.0,"ende_entre_rios_pcd":46316117.1875,"pau_pcd":2839614.501953125,"gcc_pcd":76584906.25,"absorcion_rio_grande_pcd":57319996.09375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14470970.703125,"sector_electrico":5.978841220913856,"distribucion_redes":5.41614338501419,"plantas_proc":1.1382563487681225,"consumidores_dir":0.9062600549342348,"gas_comb_comp":0.3849464333331824},{"fecha":"2026-11-13","mutun":9.785780999999997,"san_matias":0.0,"gas_transito_mmpcd":45030647.1,"gija":0.0,"gas_comb":0.3589966233486995,"entregas_int":13.83433422806094,"carrasco_pcd":15836656.0,"yapacani_pcd":90743563.0,"santa_rosa_pcd":25991789.0,"ende_entre_rios_pcd":48396207.03125,"pau_pcd":2800609.86328125,"gcc_pcd":71485476.5625,"absorcion_rio_grande_pcd":58720480.46875,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14414522.4609375,"sector_electrico":6.212236709180471,"distribucion_redes":5.545673223027622,"plantas_proc":1.12838590216417,"consumidores_dir":0.9277979388091203,"gas_comb_comp":0.37923707822825425},{"fecha":"2026-11-14","mutun":9.135321000000001,"san_matias":0.0,"gas_transito_mmpcd":20659466.908000004,"gija":0.0,"gas_comb":0.36261357163562197,"entregas_int":12.976038680727584,"carrasco_pcd":22362750.0,"yapacani_pcd":91548859.0,"santa_rosa_pcd":26208301.0,"ende_entre_rios_pcd":51183203.0,"pau_pcd":2747164.794921875,"gcc_pcd":68928921.875,"absorcion_rio_grande_pcd":55148777.34375,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14413317.3828125,"sector_electrico":5.661440852379696,"distribucion_redes":5.154894011704058,"plantas_proc":1.1487681455612444,"consumidores_dir":0.990695216203031,"gas_comb_comp":0.38285402651517664},{"fecha":"2026-11-15","mutun":8.786393,"san_matias":0.0,"gas_transito_mmpcd":44599321.904,"gija":0.0,"gas_comb":0.3416637775781893,"entregas_int":11.515172856109228,"carrasco_pcd":17368188.0,"yapacani_pcd":91714961.0,"santa_rosa_pcd":25742152.0,"ende_entre_rios_pcd":50101402.34375,"pau_pcd":2768496.58203125,"gcc_pcd":69975312.5,"absorcion_rio_grande_pcd":59316250.0,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14475446.2890625,"sector_electrico":4.911972738889945,"distribucion_redes":4.494833643856188,"plantas_proc":1.1247323825565803,"consumidores_dir":0.9633936359269586,"gas_comb_comp":0.36190423245774395},{"fecha":"2026-11-16","mutun":9.349908000000001,"san_matias":0.0,"gas_transito_mmpcd":45516991.508,"gija":0.0,"gas_comb":0.3431392518450106,"entregas_int":13.41358152382175,"carrasco_pcd":16385656.0,"yapacani_pcd":91697234.0,"santa_rosa_pcd":26120150.0,"ende_entre_rios_pcd":53141265.625,"pau_pcd":2744360.595703125,"gcc_pcd":69975313.0,"absorcion_rio_grande_pcd":56299222.65625,"gtc_a_gaa_pcd":0.0,"percheles_pcd":14331060.546875,"sector_electrico":6.386222462177069,"distribucion_redes":5.026222854460188,"plantas_proc":1.1317714828270116,"consumidores_dir":0.8491242694779256,"gas_comb_comp":0.3633797067245653}];

const STORAGE_KEY = 'ANH_REPORTE_GN_CONSOLIDADO_V3';
let db = [];
let charts = {};

function computeDerived(r) {
  const mutun = Number(r.mutun) || 0;
  const san_matias = Number(r.san_matias) || 0;
  const gas_transito_mmpcd = Number(r.gas_transito_mmpcd) || 0;
  const gija = Number(r.gija) || 0;
  const gas_comb = Number(r.gas_comb) || 0;
  const entregas_int = Number(r.entregas_int) || 0;

  const carrasco_pcd = Number(r.carrasco_pcd) || 0;
  const yapacani_pcd = Number(r.yapacani_pcd) || 0;
  const santa_rosa_pcd = Number(r.santa_rosa_pcd) || 0;

  const ende_entre_rios_pcd = Number(r.ende_entre_rios_pcd) || 0;
  const pau_pcd = Number(r.pau_pcd) || 0;
  const gcc_pcd = Number(r.gcc_pcd) || 0;

  const absorcion_rio_grande_pcd = Number(r.absorcion_rio_grande_pcd) || 0;
  const gtc_a_gaa_pcd = Number(r.gtc_a_gaa_pcd) || 0;
  const percheles_pcd = Number(r.percheles_pcd) || 0;

  const gas_transito_mmmcd = gas_transito_mmpcd / FACTOR_CONV / 1000000;
  const total_ext = mutun + san_matias + gija;
  const total_int = gas_comb + entregas_int;
  const total_sistema = total_ext + total_int;
  const recep_nodo_carrasco = (carrasco_pcd + yapacani_pcd + santa_rosa_pcd) / FACTOR_CONV / 1000000;
  const entregas_carrasco = (ende_entre_rios_pcd + pau_pcd + gcc_pcd) / FACTOR_CONV / 1000000;
  const entrega_gaa_gcc = (gcc_pcd + absorcion_rio_grande_pcd + gtc_a_gaa_pcd + percheles_pcd) / FACTOR_CONV / 1000000;

  return {
    ...r,
    mutun, san_matias, gas_transito_mmpcd, gas_transito_mmmcd, gija,
    total_ext, gas_comb, entregas_int, total_int, total_sistema,
    carrasco_pcd, yapacani_pcd, santa_rosa_pcd, recep_nodo_carrasco,
    ende_entre_rios_pcd, pau_pcd, gcc_pcd, entregas_carrasco,
    absorcion_rio_grande_pcd, gtc_a_gaa_pcd, percheles_pcd, entrega_gaa_gcc,
    sector_electrico: Number(r.sector_electrico) || 0,
    distribucion_redes: Number(r.distribucion_redes) || 0,
    plantas_proc: Number(r.plantas_proc) || 0,
    consumidores_dir: Number(r.consumidores_dir) || 0,
    gas_comb_comp: Number(r.gas_comb_comp) || 0
  };
}

function loadDatabase() {
  const saved = localStorage.getItem(STORAGE_KEY);
  const raw = saved ? JSON.parse(saved) : INITIAL_DATA;
  db = raw.map(computeDerived).sort((a, b) => a.fecha.localeCompare(b.fecha));
  saveToLocalStorage();
}

function saveToLocalStorage() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
  document.getElementById('countBadge').textContent = db.length;
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.display = 'block';
  setTimeout(() => { t.style.display = 'none'; }, 3400);
}

function switchTab(tabId, btn) {
  document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
  document.getElementById('tab-' + tabId).classList.add('active');
  btn.classList.add('active');
  if (tabId === 'dashboard') updateDashboard();
  if (tabId === 'presentacion') populatePresSelect();
  if (tabId === 'tabla') renderTable();
}

// --- FORMULARIO DÍA SIGUIENTE ---
function prepareNextDay() {
  if (!db.length) return;
  const last = db[db.length - 1];
  const d = new Date(last.fecha + 'T12:00:00');
  d.setDate(d.getDate() + 1);
  const nextStr = d.toISOString().slice(0, 10);
  document.getElementById('f_fecha').value = nextStr;
  showToast('Fecha sugerida para día siguiente: ' + nextStr);
}

function copyPreviousDayValues() {
  if (!db.length) return;
  const last = db[db.length - 1];
  const currentDate = document.getElementById('f_fecha').value;
  fillFormWithRecord(last);
  if (currentDate) document.getElementById('f_fecha').value = currentDate;
  showToast('Valores base copiados del ' + last.fecha + '. Modifique los datos del nuevo día.');
}

function loadExistingDateIfAny() {
  const f = document.getElementById('f_fecha').value;
  const found = db.find(x => x.fecha === f);
  if (found) {
    fillFormWithRecord(found);
    showToast('Fecha existente cargada para edición: ' + f);
  }
}

function fillFormWithRecord(r) {
  const fields = [
    'fecha','mutun','san_matias','gas_transito_mmpcd','gija',
    'gas_comb','entregas_int','sector_electrico','distribucion_redes',
    'plantas_proc','consumidores_dir','gas_comb_comp',
    'carrasco_pcd','yapacani_pcd','santa_rosa_pcd',
    'ende_entre_rios_pcd','pau_pcd','gcc_pcd',
    'absorcion_rio_grande_pcd','gtc_a_gaa_pcd','percheles_pcd'
  ];
  fields.forEach(k => {
    const el = document.getElementById('f_' + k);
    if (el) el.value = r[k] ?? 0;
  });
  recalcForm();
}

function recalcForm() {
  const temp = computeDerived({
    mutun: document.getElementById('f_mutun').value,
    san_matias: document.getElementById('f_san_matias').value,
    gas_transito_mmpcd: document.getElementById('f_gas_transito_mmpcd').value,
    gija: document.getElementById('f_gija').value,
    gas_comb: document.getElementById('f_gas_comb').value,
    entregas_int: document.getElementById('f_entregas_int').value,
    carrasco_pcd: document.getElementById('f_carrasco_pcd').value,
    yapacani_pcd: document.getElementById('f_yapacani_pcd').value,
    santa_rosa_pcd: document.getElementById('f_santa_rosa_pcd').value,
    ende_entre_rios_pcd: document.getElementById('f_ende_entre_rios_pcd').value,
    pau_pcd: document.getElementById('f_pau_pcd').value,
    gcc_pcd: document.getElementById('f_gcc_pcd').value,
    absorcion_rio_grande_pcd: document.getElementById('f_absorcion_rio_grande_pcd').value,
    gtc_a_gaa_pcd: document.getElementById('f_gtc_a_gaa_pcd').value,
    percheles_pcd: document.getElementById('f_percheles_pcd').value
  });

  document.getElementById('c_gas_transito_mmmcd').value = temp.gas_transito_mmmcd.toFixed(4) + ' MMmcd';
  document.getElementById('c_total_ext').value = temp.total_ext.toFixed(4) + ' MMmcd';
  document.getElementById('c_total_int').value = temp.total_int.toFixed(4) + ' MMmcd';
  document.getElementById('c_total_sistema').value = temp.total_sistema.toFixed(4) + ' MMmcd';
  document.getElementById('c_recep_nodo_carrasco').value = temp.recep_nodo_carrasco.toFixed(4) + ' MMmcd';
  document.getElementById('c_entregas_carrasco').value = temp.entregas_carrasco.toFixed(4) + ' MMmcd';
  document.getElementById('c_entrega_gaa_gcc').value = temp.entrega_gaa_gcc.toFixed(4) + ' MMmcd';
}

function saveRecord(e) {
  e.preventDefault();
  const fecha = document.getElementById('f_fecha').value;
  if (!fecha) return;

  const newObj = computeDerived({
    fecha,
    mutun: document.getElementById('f_mutun').value,
    san_matias: document.getElementById('f_san_matias').value,
    gas_transito_mmpcd: document.getElementById('f_gas_transito_mmpcd').value,
    gija: document.getElementById('f_gija').value,
    gas_comb: document.getElementById('f_gas_comb').value,
    entregas_int: document.getElementById('f_entregas_int').value,
    sector_electrico: document.getElementById('f_sector_electrico').value,
    distribucion_redes: document.getElementById('f_distribucion_redes').value,
    plantas_proc: document.getElementById('f_plantas_proc').value,
    consumidores_dir: document.getElementById('f_consumidores_dir').value,
    gas_comb_comp: document.getElementById('f_gas_comb_comp').value,
    carrasco_pcd: document.getElementById('f_carrasco_pcd').value,
    yapacani_pcd: document.getElementById('f_yapacani_pcd').value,
    santa_rosa_pcd: document.getElementById('f_santa_rosa_pcd').value,
    ende_entre_rios_pcd: document.getElementById('f_ende_entre_rios_pcd').value,
    pau_pcd: document.getElementById('f_pau_pcd').value,
    gcc_pcd: document.getElementById('f_gcc_pcd').value,
    absorcion_rio_grande_pcd: document.getElementById('f_absorcion_rio_grande_pcd').value,
    gtc_a_gaa_pcd: document.getElementById('f_gtc_a_gaa_pcd').value,
    percheles_pcd: document.getElementById('f_percheles_pcd').value
  });

  const idx = db.findIndex(x => x.fecha === fecha);
  if (idx >= 0) db[idx] = newObj;
  else db.push(newObj);

  db.sort((a, b) => a.fecha.localeCompare(b.fecha));
  saveToLocalStorage();
  setQuickRange(0);
  showToast('✅ Registro consolidado exitosamente para el ' + fecha);
}

function resetFormClean() {
  document.getElementById('dailyForm').reset();
  recalcForm();
}

// --- DASHBOARD Y GRÁFICOS DIDÁCTICOS ---
function formatDateLatam(isoDate) {
  if (!isoDate) return '-';
  const parts = isoDate.split('-');
  if (parts.length !== 3) return isoDate;
  return `${parts[2]}/${parts[1]}/${parts[0]}`;
}

function onDateFilterChange() {
  const from = document.getElementById('filterFrom').value;
  const to = document.getElementById('filterTo').value;
  const filtered = db.filter(r => (!from || r.fecha >= from) && (!to || r.fecha <= to));
  if (filtered.length > 1 && from && to && from !== to) {
    document.getElementById('chart3Mode').value = 'average';
  } else {
    document.getElementById('chart3Mode').value = 'latest';
  }
  updateDashboard();
}

function setQuickRange(days) {
  if (!db.length) return;
  const maxDate = db[db.length - 1].fecha;
  let minDate = db[0].fecha;
  if (days > 0) {
    const d = new Date(maxDate + 'T12:00:00');
    d.setDate(d.getDate() - (days - 1));
    minDate = d.toISOString().slice(0, 10);
  }
  document.getElementById('filterFrom').value = minDate;
  document.getElementById('filterTo').value = maxDate;
  if (days === 1 || days === 0) {
    document.getElementById('chart3Mode').value = 'latest';
  } else {
    document.getElementById('chart3Mode').value = 'average';
  }
  updateDashboard();
}

function computeAverageRecord(records) {
  const n = records.length || 1;
  const keys = [
    'mutun','san_matias','gas_transito_mmpcd','gas_transito_mmmcd','gija',
    'total_ext','gas_comb','entregas_int','total_int','total_sistema',
    'carrasco_pcd','yapacani_pcd','santa_rosa_pcd','recep_nodo_carrasco',
    'ende_entre_rios_pcd','pau_pcd','gcc_pcd','entregas_carrasco',
    'absorcion_rio_grande_pcd','gtc_a_gaa_pcd','percheles_pcd','entrega_gaa_gcc',
    'sector_electrico','distribucion_redes','plantas_proc','consumidores_dir','gas_comb_comp'
  ];
  const avgObj = {
    fecha: records.length > 1
      ? `Promedio (${formatDateLatam(records[0].fecha)} - ${formatDateLatam(records[records.length - 1].fecha)})`
      : formatDateLatam(records[0].fecha),
    isAverage: records.length > 1,
    countDays: records.length
  };
  keys.forEach(k => {
    avgObj[k] = records.reduce((acc, r) => acc + (Number(r[k]) || 0), 0) / n;
  });
  return avgObj;
}

function updateDashboard() {
  const from = document.getElementById('filterFrom').value;
  const to = document.getElementById('filterTo').value;
  const filtered = db.filter(r => (!from || r.fecha >= from) && (!to || r.fecha <= to));
  if (!filtered.length) return;

  const latest = filtered[filtered.length - 1];
  const isMultiDay = filtered.length > 1;
  const chart3Mode = document.getElementById('chart3Mode').value;
  const useAverageForKPIs = isMultiDay && chart3Mode === 'average';
  const activeStats = useAverageForKPIs ? computeAverageRecord(filtered) : latest;

  const avgSis = filtered.reduce((acc, r) => acc + r.total_sistema, 0) / filtered.length;
  const maxSisObj = filtered.reduce((a, b) => a.total_sistema > b.total_sistema ? a : b);
  const minSisObj = filtered.reduce((a, b) => a.total_sistema < b.total_sistema ? a : b);
  const showLineLabels = document.getElementById('toggleLineLabels').checked;

  document.getElementById('rangeInfo').textContent = isMultiDay
    ? `Analizando ${filtered.length} jornadas (${formatDateLatam(filtered[0].fecha)} al ${formatDateLatam(latest.fecha)})`
    : `Jornada seleccionada: ${formatDateLatam(latest.fecha)} (1 día)`;

  if (useAverageForKPIs) {
    document.getElementById('kpiFechaTitle').textContent = `Periodo Promediado (${filtered.length} días)`;
    document.getElementById('kpiFecha').textContent = `${formatDateLatam(filtered[0].fecha)} - ${formatDateLatam(latest.fecha)}`;
    document.getElementById('kpiFechaSub').textContent = `Promedio de ${filtered.length} jornadas seleccionadas`;
    document.getElementById('kpiTotalSisTitle').textContent = 'Total Sistema (Promedio)';
    document.getElementById('kpiExtTitle').textContent = 'Mercado Externo (Promedio)';
    document.getElementById('kpiIntTitle').textContent = 'Mercado Interno (Promedio)';
  } else {
    document.getElementById('kpiFechaTitle').textContent = isMultiDay ? 'Último Día del Rango' : 'Fecha Seleccionada';
    document.getElementById('kpiFecha').textContent = formatDateLatam(latest.fecha);
    document.getElementById('kpiFechaSub').textContent = `Reporte operativo (${latest.fecha})`;
    document.getElementById('kpiTotalSisTitle').textContent = isMultiDay ? 'Total Sistema (Último Día)' : 'Total Sistema (Día)';
    document.getElementById('kpiExtTitle').textContent = 'Mercado Externo';
    document.getElementById('kpiIntTitle').textContent = 'Mercado Interno';
  }

  document.getElementById('kpiTotalSis').textContent = activeStats.total_sistema.toFixed(2);
  document.getElementById('kpiTotalSisSub').textContent = `MMmcd (${(activeStats.total_sistema * FACTOR_CONV).toFixed(1)} MMpcd)`;
  document.getElementById('kpiExt').textContent = activeStats.total_ext.toFixed(2);
  document.getElementById('kpiExtSub').textContent = `MMmcd (${((activeStats.total_ext/(activeStats.total_sistema||1))*100).toFixed(1)}% del total)`;
  document.getElementById('kpiInt').textContent = activeStats.total_int.toFixed(2);
  document.getElementById('kpiIntSub').textContent = `MMmcd (${((activeStats.total_int/(activeStats.total_sistema||1))*100).toFixed(1)}% del total)`;
  document.getElementById('kpiAvgSis').textContent = avgSis.toFixed(2);
  document.getElementById('kpiAvgSisSub').textContent = `MMmcd promedio en ${filtered.length} ${filtered.length === 1 ? 'día' : 'días'}`;

  const labels = filtered.map(r => r.fecha.slice(5));
  const chart1Type = document.getElementById('chart1Type').value;

  // 1. GRÁFICO SISTEMA TOTAL (Línea de promedio incluida)
  renderChart('chartSistema', chart1Type, labels, [
    {
      label: 'Mercado Externo',
      data: filtered.map(r => +r.total_ext.toFixed(3)),
      borderColor: '#059669',
      backgroundColor: 'rgba(5, 150, 105, 0.78)',
      borderWidth: 2.5,
      pointRadius: 4,
      tension: 0.25
    },
    {
      label: 'Mercado Interno',
      data: filtered.map(r => +r.total_int.toFixed(3)),
      borderColor: '#f59e0b',
      backgroundColor: 'rgba(245, 158, 11, 0.85)',
      borderWidth: 2.5,
      pointRadius: 4,
      tension: 0.25
    },
    ...(chart1Type === 'line' ? [
      {
        label: 'Total Sistema',
        data: filtered.map(r => +r.total_sistema.toFixed(3)),
        borderColor: '#16382e',
        backgroundColor: '#16382e',
        borderWidth: 3,
        pointRadius: 4,
        tension: 0.25
      },
      {
        label: `Promedio Periodo (${avgSis.toFixed(2)} MMmcd)`,
        data: filtered.map(() => +avgSis.toFixed(3)),
        borderColor: '#ef4444',
        borderDash: [6, 5],
        borderWidth: 2,
        pointRadius: 0,
        datalabels: { display: false }
      }
    ] : [])
  ], { stacked: chart1Type === 'bar', showLabels: showLineLabels });

  document.getElementById('insightSistema').innerHTML =
    `💡 <strong>Lectura Rápida:</strong> El pico máximo fue de <strong>${maxSisObj.total_sistema.toFixed(2)} MMmcd</strong> (${maxSisObj.fecha}) y el mínimo de <strong>${minSisObj.total_sistema.toFixed(2)} MMmcd</strong> (${minSisObj.fecha}). En la última jornada (${latest.fecha}), el <strong>${((latest.total_int/latest.total_sistema)*100).toFixed(1)}%</strong> corresponde a Mercado Interno y el <strong>${((latest.total_ext/latest.total_sistema)*100).toFixed(1)}%</strong> a Exportación.`;

  // 2. GRÁFICO DETALLE EXTERNO
  const chart2Type = document.getElementById('chart2Type').value;
  renderChart('chartExterno', chart2Type, labels, [
    { label: 'Mutún (Brasil)', data: filtered.map(r => +r.mutun.toFixed(3)), borderColor: '#059669', backgroundColor: 'rgba(5, 150, 105, 0.22)', fill: chart2Type==='line', borderWidth: 2.5, tension: 0.25 },
    { label: 'GIJA (Argentina)', data: filtered.map(r => +r.gija.toFixed(3)), borderColor: '#2563eb', backgroundColor: '#2563eb', borderWidth: 2.5, tension: 0.25 },
    { label: 'Gas en Tránsito', data: filtered.map(r => +r.gas_transito_mmmcd.toFixed(3)), borderColor: '#f59e0b', backgroundColor: '#f59e0b', borderWidth: 2.5, tension: 0.25 },
    { label: 'San Matías', data: filtered.map(r => +r.san_matias.toFixed(3)), borderColor: '#e11d48', backgroundColor: '#e11d48', borderWidth: 2.5, tension: 0.25 }
  ], { showLabels: showLineLabels });

  document.getElementById('insightExterno').innerHTML =
    `💡 <strong>Exportación (${latest.fecha}):</strong> Mutún lidera con <strong>${latest.mutun.toFixed(2)} MMmcd</strong>, seguido por Gas en Tránsito (<strong>${latest.gas_transito_mmmcd.toFixed(2)} MMmcd</strong>), GIJA (<strong>${latest.gija.toFixed(2)} MMmcd</strong>) y San Matías (<strong>${latest.san_matias.toFixed(2)} MMmcd</strong>).`;

  // 3. GRÁFICO DE TORTA LLAMATIVO CON NOMBRES, PORCENTAJE, FECHA ACTUAL Y OPCIÓN DE PROMEDIO
  const chart3Type = document.getElementById('chart3Type').value;
  const isPieAveraged = (chart3Mode === 'average' && filtered.length > 1);
  const pieSource = isPieAveraged ? computeAverageRecord(filtered) : latest;
  const pieDateText = isPieAveraged
    ? `Promedio: ${formatDateLatam(filtered[0].fecha)} al ${formatDateLatam(latest.fecha)} (${filtered.length} días)`
    : `Fecha del gráfico: ${formatDateLatam(latest.fecha)}`;

  document.getElementById('chart3DateBadge').innerHTML = isPieAveraged
    ? `📊 <strong>${pieDateText}</strong>`
    : `📅 <strong>${pieDateText}</strong>`;

  const sectorLabels = [
    'Distribución Redes',
    'Sector Eléctrico',
    'Plantas Procesamiento',
    'Consumidores Directos',
    'Gas Comb. Compresión'
  ];
  const sectorValues = [
    +pieSource.distribucion_redes.toFixed(3),
    +pieSource.sector_electrico.toFixed(3),
    +pieSource.plantas_proc.toFixed(3),
    +pieSource.consumidores_dir.toFixed(3),
    +pieSource.gas_comb_comp.toFixed(3)
  ];
  const sectorColors = [
    '#00A878', // Verde Esmeralda Vibrante
    '#FFB703', // Amarillo Dorado Intenso
    '#2563EB', // Azul Eléctrico Brillante
    '#F97316', // Naranja Coral Llamativo
    '#8B5CF6'  // Violeta Real
  ];

  const totalSectores = sectorValues.reduce((a, b) => a + b, 0) || 1;
  const maxSectorIdx = sectorValues.indexOf(Math.max(...sectorValues));

  renderChart('chartSectores', chart3Type, sectorLabels, [{
    label: isPieAveraged ? 'Consumo Promedio (MMmcd)' : 'Consumo (MMmcd)',
    data: sectorValues,
    backgroundColor: sectorColors,
    borderColor: '#ffffff',
    borderWidth: 3,
    hoverOffset: 16
  }], {
    isPieLike: (chart3Type === 'pie' || chart3Type === 'doughnut'),
    horizontalBar: chart3Type === 'bar',
    totalSum: totalSectores,
    chartSubtitle: pieDateText
  });

  document.getElementById('insightSectores').innerHTML =
    `💡 <strong>Mayor Demanda Interna (${pieDateText}):</strong> El sector <strong>${sectorLabels[maxSectorIdx]}</strong> concentra el <strong>${((sectorValues[maxSectorIdx]/totalSectores)*100).toFixed(1)}%</strong> del consumo interno (${sectorValues[maxSectorIdx].toFixed(2)} MMmcd de un total sectorial de ${totalSectores.toFixed(2)} MMmcd).`;

  // 4. GRÁFICO NODO CARRASCO Y GAA+GCC
  renderChart('chartCarrasco', 'line', labels, [
    { label: 'Recep. Nodo Carrasco', data: filtered.map(r => +r.recep_nodo_carrasco.toFixed(3)), borderColor: '#059669', backgroundColor: '#059669', borderWidth: 2.5, tension: 0.25 },
    { label: 'Entregas Nodo Carrasco', data: filtered.map(r => +r.entregas_carrasco.toFixed(3)), borderColor: '#f59e0b', backgroundColor: '#f59e0b', borderWidth: 2.5, tension: 0.25 },
    { label: 'Entrega GAA + GCC', data: filtered.map(r => +r.entrega_gaa_gcc.toFixed(3)), borderColor: '#0284c7', backgroundColor: '#0284c7', borderWidth: 2.5, tension: 0.25 }
  ], { showLabels: showLineLabels });

  if (activeModalChartId) renderModalContent();
  document.getElementById('insightCarrasco').innerHTML =
    `💡 <strong>Balance GCY-GYC (${latest.fecha}):</strong> Recepción en Nodo Carrasco de <strong>${latest.recep_nodo_carrasco.toFixed(2)} MMmcd</strong> vs. Entregas de <strong>${latest.entregas_carrasco.toFixed(2)} MMmcd</strong>. Entrega conjunta GAA+GCC: <strong>${latest.entrega_gaa_gcc.toFixed(2)} MMmcd</strong>.`;
}

function renderChart(canvasId, type, labels, datasets, opts = {}) {
  if (charts[canvasId]) charts[canvasId].destroy();
  const ctx = document.getElementById(canvasId).getContext('2d');

  const isPie = opts.isPieLike;
  const isHoriz = opts.horizontalBar;

  charts[canvasId] = new Chart(ctx, {
    type,
    data: { labels, datasets },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      indexAxis: isHoriz ? 'y' : 'x',
      layout: {
        padding: isPie ? { top: 14, bottom: 14, left: 18, right: 18 } : { top: 10, right: 12 }
      },
      interaction: {
        mode: isPie ? 'nearest' : 'index',
        intersect: isPie
      },
      plugins: {
        title: opts.chartSubtitle ? {
          display: true,
          text: opts.chartSubtitle,
          color: '#16382e',
          font: { size: 13, weight: 'bold' },
          padding: { top: 2, bottom: 8 }
        } : { display: false },
        legend: {
          position: isPie ? 'right' : 'bottom',
          labels: {
            boxWidth: 14,
            padding: 12,
            font: { size: 11.5, weight: 'bold' },
            color: '#1b2623'
          }
        },
        tooltip: {
          backgroundColor: 'rgba(22, 56, 46, 0.95)',
          titleFont: { size: 13, weight: 'bold' },
          bodyFont: { size: 12 },
          padding: 11,
          callbacks: {
            label: function(context) {
              const val = Number(context.raw) || 0;
              const mmpcd = (val * FACTOR_CONV).toFixed(1);
              if (isPie) {
                const total = opts.totalSum || 1;
                const pct = ((val / total) * 100).toFixed(1);
                return ` ${context.label}: ${val.toFixed(3)} MMmcd (${pct}% | ${mmpcd} MMpcd)`;
              }
              return ` ${context.dataset.label}: ${val.toFixed(3)} MMmcd (${mmpcd} MMpcd)`;
            }
          }
        },
        datalabels: isPie ? {
          display: true,
          color: '#ffffff',
          backgroundColor: 'rgba(17, 24, 39, 0.72)',
          borderRadius: 5,
          padding: { top: 4, bottom: 4, left: 6, right: 6 },
          font: { weight: 'bold', size: 11 },
          textAlign: 'center',
          formatter: (value, ctx) => {
            const total = opts.totalSum || 1;
            const pct = ((value / total) * 100).toFixed(1);
            const name = ctx.chart.data.labels[ctx.dataIndex];
            const shortName = name.replace('Procesamiento', 'Proc.').replace('Consumidores', 'Cons.').replace('Distribución', 'Dist.');
            return `${shortName}\n${pct}%`;
          }
        } : (isHoriz ? {
          display: true,
          anchor: 'end',
          align: 'right',
          color: '#16382e',
          font: { weight: 'bold', size: 11 },
          formatter: (val) => {
            const total = opts.totalSum || 1;
            const pct = ((val / total) * 100).toFixed(1);
            return `${val.toFixed(2)} (${pct}%)`;
          }
        } : {
          display: !!opts.showLabels,
          align: 'top',
          color: '#16382e',
          font: { weight: 'bold', size: 9.5 },
          formatter: (v) => Number(v).toFixed(2)
        })
      },
      scales: isPie ? {} : {
        x: {
          stacked: !!opts.stacked,
          beginAtZero: isHoriz,
          title: isHoriz ? { display: true, text: 'MMmcd', font: { size: 11, weight: 'bold' } } : {}
        },
        y: {
          stacked: !!opts.stacked,
          beginAtZero: !isHoriz,
          title: !isHoriz ? { display: true, text: 'MMmcd', font: { size: 11, weight: 'bold' } } : {}
        }
      }
    }
  });
}

// --- HOJA PARA PRESENTACIÓN ---
function populatePresSelect() {
  const sel = document.getElementById('presDateSelect');
  sel.innerHTML = db.slice().reverse().map(r => `<option value="${r.fecha}">${r.fecha}</option>`).join('');
  renderPresentation();
}

function renderPresentation() {
  const f = document.getElementById('presDateSelect').value;
  const r = db.find(x => x.fecha === f);
  if (!r) return;

  const fmt = (n, d = 3) => Number(n || 0).toLocaleString('es-BO', { minimumFractionDigits: d, maximumFractionDigits: d });

  document.getElementById('presentationContainer').innerHTML = `
    <table class="exec-table">
      <thead><tr><th colspan="2">MERCADO EXTERNO (${r.fecha})</th></tr></thead>
      <tbody>
        <tr><td>Mutún (Brasil)</td><td class="num">${fmt(r.mutun)} MMmcd</td></tr>
        <tr><td>San Matías (Brasil)</td><td class="num">${fmt(r.san_matias)} MMmcd</td></tr>
        <tr><td>GIJA (Argentina)</td><td class="num">${fmt(r.gija)} MMmcd</td></tr>
        <tr><td>Gas en Tránsito (Argentina)</td><td class="num">${fmt(r.gas_transito_mmmcd)} MMmcd</td></tr>
        <tr class="exec-total"><td>TOTAL ENTREGAS MERCADO EXTERNO</td><td class="num">${fmt(r.total_ext)} MMmcd</td></tr>
      </tbody>
    </table>

    <table class="exec-table">
      <thead><tr><th colspan="2">MERCADO INTERNO Y TOTAL SISTEMA</th></tr></thead>
      <tbody>
        <tr><td>Gas Combustible</td><td class="num">${fmt(r.gas_comb)} MMmcd</td></tr>
        <tr><td>Entregas Mercado Interno</td><td class="num">${fmt(r.entregas_int)} MMmcd</td></tr>
        <tr class="exec-total"><td>TOTAL MERCADO INTERNO</td><td class="num">${fmt(r.total_int)} MMmcd</td></tr>
        <tr class="exec-total" style="background:#16382e; color:#fff;"><td>TOTAL ENTREGAS SISTEMA GASODUCTOS</td><td class="num">${fmt(r.total_sistema)} MMmcd</td></tr>
      </tbody>
    </table>

    <table class="exec-table">
      <thead><tr><th colspan="2">DATOS DE INTERÉS — GASODUCTO GCY - GYC</th></tr></thead>
      <tbody>
        <tr><td>Recepción Campos Nodo Carrasco</td><td class="num">${fmt(r.recep_nodo_carrasco)} MMmcd</td></tr>
        <tr><td>Entregas Nodo Carrasco (ENDE + PAU + GCC)</td><td class="num">${fmt(r.entregas_carrasco)} MMmcd</td></tr>
        <tr><td>Consumo PAU</td><td class="num">${fmt(r.pau_pcd / FACTOR_CONV / 1e6)} MMmcd</td></tr>
        <tr><td>Entrega a GCC</td><td class="num">${fmt(r.gcc_pcd / FACTOR_CONV / 1e6)} MMmcd</td></tr>
        <tr class="exec-total"><td>Entrega a GAA + GCC</td><td class="num">${fmt(r.entrega_gaa_gcc)} MMmcd</td></tr>
      </tbody>
    </table>

    <table class="exec-table">
      <thead><tr><th colspan="2">COMERCIALIZACIÓN MERCADO INTERNO</th></tr></thead>
      <tbody>
        <tr><td>Sector Eléctrico SIN + Aislado</td><td class="num">${fmt(r.sector_electrico)} MMmcd</td></tr>
        <tr><td>Distribución Redes (EMTAGAS + YPFB)</td><td class="num">${fmt(r.distribucion_redes)} MMmcd</td></tr>
        <tr><td>Plantas Procesamiento (PSL+PAU+Otras)</td><td class="num">${fmt(r.plantas_proc)} MMmcd</td></tr>
        <tr><td>Consumidores Directos y Otros</td><td class="num">${fmt(r.consumidores_dir)} MMmcd</td></tr>
        <tr><td>Gas Combustible Est. Compresión + JV</td><td class="num">${fmt(r.gas_comb_comp)} MMmcd</td></tr>
      </tbody>
    </table>
  `;
}

// --- TABLA CONSOLIDADA ---
function renderTable() {
  const q = document.getElementById('searchTable').value.trim();
  const rows = db.slice().reverse().filter(r => !q || r.fecha.includes(q));
  const f = (n, d = 2) => Number(n || 0).toFixed(d);

  document.getElementById('tableBody').innerHTML = rows.map(r => `
    <tr>
      <td>${r.fecha}</td>
      <td style="text-align:center;">
        <button class="btn btn-outline" style="padding:3px 8px; font-size:0.74rem;" onclick="editRow('${r.fecha}')">✏️ Editar</button>
        <button class="btn btn-danger" style="padding:3px 8px; font-size:0.74rem;" onclick="deleteRow('${r.fecha}')">🗑️</button>
      </td>
      <td>${f(r.mutun,3)}</td>
      <td>${f(r.san_matias,3)}</td>
      <td>${Math.round(r.gas_transito_mmpcd).toLocaleString()}</td>
      <td class="calc-col">${f(r.gas_transito_mmmcd,3)}</td>
      <td>${f(r.gija,3)}</td>
      <td class="calc-col">${f(r.total_ext,3)}</td>
      <td>${f(r.gas_comb,3)}</td>
      <td>${f(r.entregas_int,3)}</td>
      <td class="calc-col">${f(r.total_int,3)}</td>
      <td class="calc-col" style="background:#e8f1ee;">${f(r.total_sistema,3)}</td>
      <td>${Math.round(r.carrasco_pcd).toLocaleString()}</td>
      <td>${Math.round(r.yapacani_pcd).toLocaleString()}</td>
      <td>${Math.round(r.santa_rosa_pcd).toLocaleString()}</td>
      <td class="calc-col">${f(r.recep_nodo_carrasco,3)}</td>
      <td>${Math.round(r.ende_entre_rios_pcd).toLocaleString()}</td>
      <td>${Math.round(r.pau_pcd).toLocaleString()}</td>
      <td>${Math.round(r.gcc_pcd).toLocaleString()}</td>
      <td class="calc-col">${f(r.entregas_carrasco,3)}</td>
      <td>${Math.round(r.absorcion_rio_grande_pcd).toLocaleString()}</td>
      <td>${Math.round(r.gtc_a_gaa_pcd).toLocaleString()}</td>
      <td>${Math.round(r.percheles_pcd).toLocaleString()}</td>
      <td class="calc-col">${f(r.entrega_gaa_gcc,3)}</td>
      <td>${f(r.sector_electrico,3)}</td>
      <td>${f(r.distribucion_redes,3)}</td>
      <td>${f(r.plantas_proc,3)}</td>
      <td>${f(r.consumidores_dir,3)}</td>
      <td>${f(r.gas_comb_comp,3)}</td>
    </tr>
  `).join('');
}

function editRow(fecha) {
  const found = db.find(x => x.fecha === fecha);
  if (!found) return;
  fillFormWithRecord(found);
  document.querySelectorAll('.tab-btn')[1].click();
  showToast('Editando registro del ' + fecha);
}

function deleteRow(fecha) {
  if (!confirm(`¿Eliminar el registro del ${fecha}?`)) return;
  db = db.filter(x => x.fecha !== fecha);
  saveToLocalStorage();
  renderTable();
  showToast('Registro eliminado: ' + fecha);
}

function resetToFactoryData() {
  if (!confirm('¿Restaurar los datos originales precargados? Se perderán los cambios locales no exportados.')) return;
  localStorage.removeItem(STORAGE_KEY);
  loadDatabase();
  renderTable();
  setQuickRange(0);
  showToast('Base restaurada a los datos originales.');
}

// --- PORTABILIDAD ENTRE COMPUTADORAS ---
function exportJSON() {
  const blob = new Blob([JSON.stringify(db, null, 2)], { type: 'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `ANH_Consolidado_GasNatural_${new Date().toISOString().slice(0,10)}.json`;
  a.click();
  showToast('Archivo JSON consolidado descargado. Puede copiarlo a otra PC.');
}

function importJSON(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(evt) {
    try {
      const imported = JSON.parse(evt.target.result);
      if (!Array.isArray(imported)) throw new Error('Formato inválido');
      db = imported.map(computeDerived).sort((a, b) => a.fecha.localeCompare(b.fecha));
      saveToLocalStorage();
      setQuickRange(0);
      renderTable();
      showToast(`✅ Se importaron y consolidaron ${db.length} registros.`);
    } catch (err) {
      alert('Error al leer el archivo JSON: ' + err.message);
    }
  };
  reader.readAsText(file);
}

function exportCSV() {
  const headers = [
    'FECHA','Mutun_MMmcd','SanMatias_MMmcd','GasTransito_PCD','GasTransito_MMmcd','GIJA_MMmcd','TotalExt_MMmcd',
    'GasComb_MMmcd','EntregasInt_MMmcd','TotalInt_MMmcd','TotalSistema_MMmcd',
    'Carrasco_PCD','Yapacani_PCD','SantaRosa_PCD','RecepNodoCarrasco_MMmcd',
    'ENDE_EntreRios_PCD','PAU_PCD','GCC_PCD','EntregasCarrasco_MMmcd',
    'AbsorcionRioGrande_PCD','GTC_a_GAA_PCD','Percheles_PCD','Entrega_GAA_GCC_MMmcd',
    'SectorElectrico_MMmcd','DistribucionRedes_MMmcd','PlantasProc_MMmcd','ConsumidoresDir_MMmcd','GasCombComp_MMmcd'
  ];
  const lines = [headers.join(';')];
  db.forEach(r => {
    lines.push([
      r.fecha, r.mutun, r.san_matias, r.gas_transito_mmpcd, r.gas_transito_mmmcd, r.gija, r.total_ext,
      r.gas_comb, r.entregas_int, r.total_int, r.total_sistema,
      r.carrasco_pcd, r.yapacani_pcd, r.santa_rosa_pcd, r.recep_nodo_carrasco,
      r.ende_entre_rios_pcd, r.pau_pcd, r.gcc_pcd, r.entregas_carrasco,
      r.absorcion_rio_grande_pcd, r.gtc_a_gaa_pcd, r.percheles_pcd, r.entrega_gaa_gcc,
      r.sector_electrico, r.distribucion_redes, r.plantas_proc, r.consumidores_dir, r.gas_comb_comp
    ].join(';'));
  });
  const blob = new Blob(["\uFEFF" + lines.join('\n')], { type: 'text/csv;charset=utf-8;' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `ANH_Reporte_Consolidado_GN_${new Date().toISOString().slice(0,10)}.csv`;
  a.click();
}

function exportStandaloneHTML() {
  if (activeModalChartId) closeChartModal();
  let htmlSource = document.documentElement.outerHTML;
  const serialized = JSON.stringify(db);
  htmlSource = htmlSource.replace(/const INITIAL_DATA = \[[\s\S]*?\];/, `const INITIAL_DATA = ${serialized};`);
  const blob = new Blob(['<!DOCTYPE html>\n' + htmlSource], { type: 'text/html;charset=utf-8;' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `Sistema_Reporte_Diario_GN_ANH_${new Date().toISOString().slice(0,10)}.html`;
  a.click();
  showToast('HTML portable generado con todos los datos consolidados dentro.');
}


// --- VISTA AMPLIADA (PANTALLA COMPLETA) + RESUMEN TÉCNICO DIDÁCTICO ---
const CHART_ORDER = ['chartSistema', 'chartExterno', 'chartSectores', 'chartCarrasco'];
let activeModalChartId = null;
let modalChartInstance = null;

function getFilteredRecords() {
  const from = document.getElementById('filterFrom').value;
  const to = document.getElementById('filterTo').value;
  return db.filter(r => (!from || r.fecha >= from) && (!to || r.fecha <= to));
}

function formatDeltaBadge(curr, prev, unit = 'MMmcd') {
  if (prev === null || prev === undefined) return '<span class="badge-delta-flat">Sin día previo</span>';
  const diff = curr - prev;
  const pct = prev !== 0 ? (diff / prev) * 100 : 0;
  if (Math.abs(diff) < 0.001) {
    return `<span class="badge-delta-flat">0.00 ${unit} (0.0%)</span>`;
  }
  const sign = diff > 0 ? '+' : '';
  const cls = diff > 0 ? 'badge-delta-up' : 'badge-delta-down';
  const arrow = diff > 0 ? '▲' : '▼';
  return `<span class="${cls}">${arrow} ${sign}${diff.toFixed(3)} ${unit} (${sign}${pct.toFixed(1)}%)</span>`;
}

function openChartModal(chartId) {
  const filtered = getFilteredRecords();
  if (!filtered.length) {
    showToast('No hay datos en el rango seleccionado para ampliar.');
    return;
  }
  activeModalChartId = chartId;
  const modal = document.getElementById('chartModal');
  modal.classList.add('open');
  document.body.style.overflow = 'hidden';

  // Sync type selector options for this chart
  const typeSel = document.getElementById('modalChartTypeSelect');
  const labelsWrap = document.getElementById('modalLabelsToggleWrap');
  const modalPieModeWrap = document.getElementById('modalPieModeWrap');
  if (modalPieModeWrap) modalPieModeWrap.style.display = 'none';
  document.getElementById('modalToggleLabels').checked = document.getElementById('toggleLineLabels').checked;

  if (chartId === 'chartSistema') {
    typeSel.innerHTML = `
      <option value="line">Líneas + Promedio</option>
      <option value="bar">Barras Apiladas</option>
    `;
    typeSel.value = document.getElementById('chart1Type').value;
    labelsWrap.style.display = 'flex';
  } else if (chartId === 'chartExterno') {
    typeSel.innerHTML = `
      <option value="line">Curvas Comparativas</option>
      <option value="bar">Barras Agrupadas</option>
    `;
    typeSel.value = document.getElementById('chart2Type').value;
    labelsWrap.style.display = 'flex';
  } else if (chartId === 'chartSectores') {
    typeSel.innerHTML = `
      <option value="pie">Torta Clásica (% y Nombres)</option>
      <option value="doughnut">Anillo / Dona (% y Nombres)</option>
      <option value="bar">Barras Horizontales</option>
    `;
    typeSel.value = document.getElementById('chart3Type').value;
    labelsWrap.style.display = 'none';
    if (modalPieModeWrap) {
      modalPieModeWrap.style.display = 'inline-block';
      document.getElementById('modalChart3ModeSelect').value = document.getElementById('chart3Mode').value;
    }
  } else if (chartId === 'chartCarrasco') {
    typeSel.innerHTML = `
      <option value="line">Curvas de Flujo Operativo</option>
      <option value="bar">Barras Comparativas</option>
    `;
    typeSel.value = 'line';
    labelsWrap.style.display = 'flex';
  }

  renderModalContent();
}

function changeModalChartType() {
  const val = document.getElementById('modalChartTypeSelect').value;
  if (activeModalChartId === 'chartSistema') {
    document.getElementById('chart1Type').value = val;
    updateDashboard();
  } else if (activeModalChartId === 'chartExterno') {
    document.getElementById('chart2Type').value = val;
    updateDashboard();
  } else if (activeModalChartId === 'chartSectores') {
    document.getElementById('chart3Type').value = val;
    updateDashboard();
  }
  renderModalContent();
}

function changeModalChart3Mode() {
  const val = document.getElementById('modalChart3ModeSelect').value;
  document.getElementById('chart3Mode').value = val;
  updateDashboard();
  renderModalContent();
}

function navigateModalChart(step) {
  if (!activeModalChartId) return;
  const idx = CHART_ORDER.indexOf(activeModalChartId);
  const nextIdx = (idx + step + CHART_ORDER.length) % CHART_ORDER.length;
  openChartModal(CHART_ORDER[nextIdx]);
}

function closeChartModal() {
  const modal = document.getElementById('chartModal');
  modal.classList.remove('open');
  document.body.style.overflow = '';
  activeModalChartId = null;
  if (modalChartInstance) {
    modalChartInstance.destroy();
    modalChartInstance = null;
  }
}

function handleModalBackdropClick(e) {
  if (e.target.id === 'chartModal') closeChartModal();
}

document.addEventListener('keydown', (e) => {
  if (!activeModalChartId) return;
  if (e.key === 'Escape') closeChartModal();
  if (e.key === 'ArrowRight') navigateModalChart(1);
  if (e.key === 'ArrowLeft') navigateModalChart(-1);
});

function renderModalContent() {
  if (!activeModalChartId) return;
  const filtered = getFilteredRecords();
  if (!filtered.length) return;

  const latest = filtered[filtered.length - 1];
  const prev = filtered.length > 1 ? filtered[filtered.length - 2] : null;
  const labels = filtered.map(r => r.fecha);
  const selectedType = document.getElementById('modalChartTypeSelect').value;
  const showLabels = document.getElementById('modalToggleLabels').checked;

  document.getElementById('modalPeriodBadge').textContent = `Periodo: ${filtered[0].fecha} al ${latest.fecha} (${filtered.length} días)`;

  if (modalChartInstance) modalChartInstance.destroy();
  const ctx = document.getElementById('modalChartCanvas').getContext('2d');

  let datasets = [];
  let chartLabels = labels;
  let opts = { showLabels, isModal: true };
  let title = '';
  let subtitle = '';
  let techHTML = '';

  if (activeModalChartId === 'chartSistema') {
    title = '1. Balance Total del Sistema: Mercado Externo vs. Mercado Interno';
    subtitle = 'Evaluación integral de volumen transportado por el Sistema Troncal de Gasoductos (MMmcd y MMpcd)';

    const avgSis = filtered.reduce((s, r) => s + r.total_sistema, 0) / filtered.length;
    const avgExt = filtered.reduce((s, r) => s + r.total_ext, 0) / filtered.length;
    const avgInt = filtered.reduce((s, r) => s + r.total_int, 0) / filtered.length;
    const maxRec = filtered.reduce((a, b) => a.total_sistema > b.total_sistema ? a : b);
    const minRec = filtered.reduce((a, b) => a.total_sistema < b.total_sistema ? a : b);
    const pctExt = (latest.total_ext / (latest.total_sistema || 1)) * 100;
    const pctInt = (latest.total_int / (latest.total_sistema || 1)) * 100;

    datasets = [
      {
        label: 'Mercado Externo (Exportación)',
        data: filtered.map(r => +r.total_ext.toFixed(3)),
        borderColor: '#059669',
        backgroundColor: 'rgba(5, 150, 105, 0.78)',
        borderWidth: 3,
        pointRadius: 4.5,
        tension: 0.25
      },
      {
        label: 'Mercado Interno (Nacional)',
        data: filtered.map(r => +r.total_int.toFixed(3)),
        borderColor: '#f59e0b',
        backgroundColor: 'rgba(245, 158, 11, 0.85)',
        borderWidth: 3,
        pointRadius: 4.5,
        tension: 0.25
      },
      ...(selectedType === 'line' ? [
        {
          label: 'Total Sistema',
          data: filtered.map(r => +r.total_sistema.toFixed(3)),
          borderColor: '#16382e',
          backgroundColor: '#16382e',
          borderWidth: 3.5,
          pointRadius: 5,
          tension: 0.25
        },
        {
          label: `Promedio Periodo (${avgSis.toFixed(2)} MMmcd)`,
          data: filtered.map(() => +avgSis.toFixed(3)),
          borderColor: '#ef4444',
          borderDash: [7, 5],
          borderWidth: 2.2,
          pointRadius: 0,
          datalabels: { display: false }
        }
      ] : [])
    ];
    opts.stacked = (selectedType === 'bar');

    techHTML = `
      <div class="tech-section">
        <div class="tech-section-header">📊 Indicadores Clave del Periodo</div>
        <div class="tech-kpi-mini-grid">
          <div class="tech-kpi-box gold">
            <div class="lbl">Última Jornada (${latest.fecha})</div>
            <div class="val">${latest.total_sistema.toFixed(3)} MMmcd</div>
            <div class="sub">${(latest.total_sistema * FACTOR_CONV).toFixed(1)} MMpcd • ${formatDeltaBadge(latest.total_sistema, prev ? prev.total_sistema : null)}</div>
          </div>
          <div class="tech-kpi-box">
            <div class="lbl">Promedio del Periodo</div>
            <div class="val">${avgSis.toFixed(3)} MMmcd</div>
            <div class="sub">Ext: ${avgExt.toFixed(2)} | Int: ${avgInt.toFixed(2)} MMmcd</div>
          </div>
          <div class="tech-kpi-box">
            <div class="lbl">Pico Máximo Registrado</div>
            <div class="val">${maxRec.total_sistema.toFixed(3)} MMmcd</div>
            <div class="sub">Fecha: <strong>${maxRec.fecha}</strong></div>
          </div>
          <div class="tech-kpi-box">
            <div class="lbl">Mínimo Registrado</div>
            <div class="val">${minRec.total_sistema.toFixed(3)} MMmcd</div>
            <div class="sub">Fecha: <strong>${minRec.fecha}</strong></div>
          </div>
        </div>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🔬 Diagnóstico Técnico Operativo (DTD)</div>
        <div class="tech-analysis-text">
          <p>En la jornada del <strong>${latest.fecha}</strong>, el sistema nacional de transporte movilizó un volumen total de <strong>${latest.total_sistema.toFixed(3)} MMmcd</strong> (${(latest.total_sistema * FACTOR_CONV).toFixed(1)} MMpcd), ubicándose <strong>${latest.total_sistema >= avgSis ? 'por encima' : 'por debajo'}</strong> del promedio del rango analizado (${avgSis.toFixed(2)} MMmcd).</p>
          <p><strong>Estructura de Demanda:</strong> El <strong>Mercado Interno</strong> absorbió el <strong>${pctInt.toFixed(1)}%</strong> (${latest.total_int.toFixed(3)} MMmcd, sumando ${latest.entregas_int.toFixed(3)} MMmcd de entregas y ${latest.gas_comb.toFixed(3)} MMmcd de gas combustible), mientras que el <strong>Mercado Externo</strong> representó el <strong>${pctExt.toFixed(1)}%</strong> (${latest.total_ext.toFixed(3)} MMmcd).</p>
        </div>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🎓 Guía Didáctica: ¿Cómo interpretar este gráfico?</div>
        <div class="didactic-callout">
          <strong>Claves para la lectura operativa:</strong>
          <ul>
            <li><strong>Total Sistema:</strong> Es la suma exacta de las entregas de exportación firme (<em>Mercado Externo</em>) más el consumo nacional y gas combustible (<em>Mercado Interno</em>).</li>
            <li><strong>Línea Roja Punteada:</strong> Marca el promedio aritmético del periodo. Permite identificar rápidamente días de alta demanda (picos térmicos o nominaciones elevadas de Brasil) frente a fines de semana.</li>
            <li><strong>Equivalencia de Unidades:</strong> 1 MMmcd (Millón de metros cúbicos día) equivale a <strong>35.314 MMpcd</strong> (Millones de pies cúbicos día).</li>
          </ul>
        </div>
      </div>
    `;
  } else if (activeModalChartId === 'chartExterno') {
    title = '2. Exportación por Punto de Entrega y Gas en Tránsito';
    subtitle = 'Desagregación operativa hacia Brasil (Mutún, San Matías) y Argentina (GIJA, Gas en Tránsito)';

    const avgMutun = filtered.reduce((s, r) => s + r.mutun, 0) / filtered.length;
    const avgGija = filtered.reduce((s, r) => s + r.gija, 0) / filtered.length;
    const avgTrans = filtered.reduce((s, r) => s + r.gas_transito_mmmcd, 0) / filtered.length;
    const avgSanMat = filtered.reduce((s, r) => s + r.san_matias, 0) / filtered.length;

    datasets = [
      { label: 'Mutún (Brasil)', data: filtered.map(r => +r.mutun.toFixed(3)), borderColor: '#059669', backgroundColor: 'rgba(5, 150, 105, 0.25)', fill: selectedType === 'line', borderWidth: 3, tension: 0.25 },
      { label: 'GIJA (Argentina)', data: filtered.map(r => +r.gija.toFixed(3)), borderColor: '#2563eb', backgroundColor: '#2563eb', borderWidth: 2.8, tension: 0.25 },
      { label: 'Gas en Tránsito (MMmcd)', data: filtered.map(r => +r.gas_transito_mmmcd.toFixed(3)), borderColor: '#f59e0b', backgroundColor: '#f59e0b', borderWidth: 2.8, tension: 0.25 },
      { label: 'San Matías (Brasil)', data: filtered.map(r => +r.san_matias.toFixed(3)), borderColor: '#e11d48', backgroundColor: '#e11d48', borderWidth: 2.8, tension: 0.25 }
    ];

    techHTML = `
      <div class="tech-section">
        <div class="tech-section-header">📊 Balance por Punto Fronterizo (${latest.fecha})</div>
        <table class="mini-breakdown-table">
          <thead>
            <tr>
              <th>Punto de Medición</th>
              <th style="text-align:right;">Último Día</th>
              <th style="text-align:right;">Prom. Periodo</th>
              <th style="text-align:right;">Variación Día</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><strong>Mutún (Brasil)</strong></td>
              <td class="num">${latest.mutun.toFixed(3)}</td>
              <td class="num">${avgMutun.toFixed(3)}</td>
              <td style="text-align:right;">${formatDeltaBadge(latest.mutun, prev ? prev.mutun : null, '')}</td>
            </tr>
            <tr>
              <td><strong>San Matías (Brasil)</strong></td>
              <td class="num">${latest.san_matias.toFixed(3)}</td>
              <td class="num">${avgSanMat.toFixed(3)}</td>
              <td style="text-align:right;">${formatDeltaBadge(latest.san_matias, prev ? prev.san_matias : null, '')}</td>
            </tr>
            <tr>
              <td><strong>GIJA (Argentina)</strong></td>
              <td class="num">${latest.gija.toFixed(3)}</td>
              <td class="num">${avgGija.toFixed(3)}</td>
              <td style="text-align:right;">${formatDeltaBadge(latest.gija, prev ? prev.gija : null, '')}</td>
            </tr>
            <tr style="background:#fffbeb;">
              <td><strong>Gas en Tránsito*</strong></td>
              <td class="num">${latest.gas_transito_mmmcd.toFixed(3)}</td>
              <td class="num">${avgTrans.toFixed(3)}</td>
              <td style="text-align:right;">${formatDeltaBadge(latest.gas_transito_mmmcd, prev ? prev.gas_transito_mmmcd : null, '')}</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🔬 Análisis Técnico Operativo</div>
        <div class="tech-analysis-text">
          <p><strong>Exportación Firme (Total Externo):</strong> Alcanzó <strong>${latest.total_ext.toFixed(3)} MMmcd</strong> en la última jornada, impulsada principalmente por el punto de entrega <strong>Mutún</strong> con <strong>${latest.mutun.toFixed(3)} MMmcd</strong> (${latest.total_ext > 0 ? ((latest.mutun/latest.total_ext)*100).toFixed(1) : '0.0'}% de la exportación firme).</p>
          <p><strong>Operación de Gas en Tránsito:</strong> Se registraron <strong>${Math.round(latest.gas_transito_mmpcd).toLocaleString()} PCD</strong> equivalentes a <strong>${latest.gas_transito_mmmcd.toFixed(3)} MMmcd</strong> utilizando la capacidad de transporte del sistema boliviano.</p>
        </div>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🎓 Guía Didáctica: Puntos de Exportación y Tránsito</div>
        <div class="didactic-callout">
          <strong>Diferencia normativa y operativa:</strong>
          <ul>
            <li><strong>Mutún y San Matías:</strong> Puntos de medición fiscal de exportación de gas natural hacia el mercado brasileño (gasoducto Gasbol y ramal Cuiabá).</li>
            <li><strong>GIJA (Gasoducto de Integración Juana Azurduy):</strong> Interconexión internacional en frontera con Argentina (Madrejones / Campo Durán).</li>
            <li><strong>*Gas en Tránsito:</strong> Volumen transportado a través de ductos nacionales bajo modalidad de servicio de tránsito internacional; por convención contable del reporte DTD, se monitorea en paralelo sin duplicarse dentro de la suma de producción nacional exportada.</li>
          </ul>
        </div>
      </div>
    `;
  } else if (activeModalChartId === 'chartSectores') {
    const chart3Mode = document.getElementById('chart3Mode').value;
    const isPieAveraged = (chart3Mode === 'average' && filtered.length > 1);
    const pieSource = isPieAveraged ? computeAverageRecord(filtered) : latest;
    const pieDateText = isPieAveraged
      ? `Promedio: ${formatDateLatam(filtered[0].fecha)} al ${formatDateLatam(latest.fecha)} (${filtered.length} días)`
      : `Fecha del gráfico: ${formatDateLatam(latest.fecha)}`;

    title = `3. Participación Mercado Interno por Sector — ${pieDateText}`;
    subtitle = isPieAveraged
      ? `Promedio sectorial calculado sobre ${filtered.length} jornadas seleccionadas (${filtered[0].fecha} al ${latest.fecha})`
      : `Distribución sectorial detallada correspondiente a la jornada ${formatDateLatam(latest.fecha)} (MMmcd y % de participación)`;

    chartLabels = [
      'Distribución Redes (EMTAGAS + YPFB)',
      'Sector Eléctrico (SIN + Aislados)',
      'Plantas Procesamiento (PSL+VG+Kanata)',
      'Consumidores Directos y Otros',
      'Gas Combustible Est. Compresión + JV'
    ];
    const sectorValues = [
      +pieSource.distribucion_redes.toFixed(3),
      +pieSource.sector_electrico.toFixed(3),
      +pieSource.plantas_proc.toFixed(3),
      +pieSource.consumidores_dir.toFixed(3),
      +pieSource.gas_comb_comp.toFixed(3)
    ];
    const prevSectorValues = (!isPieAveraged && prev) ? [
      prev.distribucion_redes, prev.sector_electrico, prev.plantas_proc, prev.consumidores_dir, prev.gas_comb_comp
    ] : null;
    const sectorColors = ['#00A878', '#FFB703', '#2563EB', '#F97316', '#8B5CF6'];
    const totalSectores = sectorValues.reduce((a, b) => a + b, 0) || 1;

    datasets = [{
      label: isPieAveraged ? 'Consumo Promedio (MMmcd)' : 'Consumo Sectorial (MMmcd)',
      data: sectorValues,
      backgroundColor: sectorColors,
      borderColor: '#ffffff',
      borderWidth: 3,
      hoverOffset: 18
    }];
    opts.isPieLike = (selectedType === 'pie' || selectedType === 'doughnut');
    opts.horizontalBar = (selectedType === 'bar');
    opts.totalSum = totalSectores;
    opts.chartSubtitle = pieDateText;

    const rowsHTML = chartLabels.map((lbl, idx) => {
      const val = sectorValues[idx];
      const pct = ((val / totalSectores) * 100).toFixed(1);
      const mmpcd = (val * FACTOR_CONV).toFixed(1);
      const delta = prevSectorValues ? formatDeltaBadge(val, prevSectorValues[idx], '') : `<span class="badge-delta-flat">${isPieAveraged ? 'Prom. ' + filtered.length + 'd' : '-'}</span>`;
      return `
        <tr>
          <td><span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:${sectorColors[idx]};margin-right:6px;"></span><strong>${lbl.split('(')[0].trim()}</strong></td>
          <td class="num">${val.toFixed(3)}</td>
          <td class="num">${mmpcd}</td>
          <td class="num">${pct}%</td>
          <td style="text-align:right;">${delta}</td>
        </tr>
      `;
    }).join('');

    const maxIdx = sectorValues.indexOf(Math.max(...sectorValues));

    techHTML = `
      <div class="tech-section">
        <div class="tech-section-header">📊 Desglose Sectorial (${pieDateText})</div>
        <table class="mini-breakdown-table">
          <thead>
            <tr>
              <th>Sector</th>
              <th style="text-align:right;">MMmcd</th>
              <th style="text-align:right;">MMpcd</th>
              <th style="text-align:right;">Part. %</th>
              <th style="text-align:right;">Var. Día</th>
            </tr>
          </thead>
          <tbody>
            ${rowsHTML}
            <tr style="background:var(--anh-gold-light); font-weight:800;">
              <td>TOTAL SECTORIAL</td>
              <td class="num">${totalSectores.toFixed(3)}</td>
              <td class="num">${(totalSectores * FACTOR_CONV).toFixed(1)}</td>
              <td class="num">100.0%</td>
              <td></td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🔬 Análisis Técnico de Demanda Interna</div>
        <div class="tech-analysis-text">
          <p>El sector líder en consumo es <strong>${chartLabels[maxIdx]}</strong> con <strong>${sectorValues[maxIdx].toFixed(3)} MMmcd</strong> (<strong>${((sectorValues[maxIdx]/totalSectores)*100).toFixed(1)}%</strong> del total interno), seguido por el segundo bloque más relevante que en conjunto con el primero supera el <strong>${(((sectorValues[0]+sectorValues[1])/totalSectores)*100).toFixed(1)}%</strong> de la demanda nacional.</p>
          <p>La generación termoeléctrica (<strong>${latest.sector_electrico.toFixed(3)} MMmcd</strong>) actúa como principal variable de ajuste diario ante variaciones de despacho hídrico en el Sistema Interconectado Nacional (SIN).</p>
        </div>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🎓 Guía Didáctica: Sectores del Mercado Interno</div>
        <div class="didactic-callout">
          <strong>Composición del consumo nacional regulado por la ANH:</strong>
          <ul>
            <li><strong>Distribución por Redes:</strong> Consumo residencial, comercial, GNV vehicular e industrial conectado a redes de YPFB y EMTAGAS.</li>
            <li><strong>Sector Eléctrico:</strong> Gas natural entregado a plantas termoeléctricas del SIN y sistemas aislados.</li>
            <li><strong>Plantas de Procesamiento:</strong> Consumo y merma operativa en plantas de separación de líquidos (Río Grande, Gran Chaco) y licuefacción (PSL).</li>
            <li><strong>Gas Combustible Compresión:</strong> Gas utilizado como energía propia por los motocompresores de las estaciones para mantener la presión en los ductos.</li>
          </ul>
        </div>
      </div>
    `;
  } else if (activeModalChartId === 'chartCarrasco') {
    title = '4. Flujo Operativo Nodo Carrasco y Entrega GAA + GCC';
    subtitle = 'Monitoreo hidráulico y de balance en el corredor Cochabamba – Santa Cruz (Gasoductos GCY - GYC - GAA - GCC)';

    const avgRecep = filtered.reduce((s, r) => s + r.recep_nodo_carrasco, 0) / filtered.length;
    const avgEntCarr = filtered.reduce((s, r) => s + r.entregas_carrasco, 0) / filtered.length;
    const avgGaaGcc = filtered.reduce((s, r) => s + r.entrega_gaa_gcc, 0) / filtered.length;
    const balanceNodo = latest.recep_nodo_carrasco - latest.entregas_carrasco;

    datasets = [
      { label: 'Recep. Nodo Carrasco', data: filtered.map(r => +r.recep_nodo_carrasco.toFixed(3)), borderColor: '#059669', backgroundColor: '#059669', borderWidth: 3, tension: 0.25 },
      { label: 'Entregas Nodo Carrasco', data: filtered.map(r => +r.entregas_carrasco.toFixed(3)), borderColor: '#f59e0b', backgroundColor: '#f59e0b', borderWidth: 3, tension: 0.25 },
      { label: 'Entrega GAA + GCC', data: filtered.map(r => +r.entrega_gaa_gcc.toFixed(3)), borderColor: '#0284c7', backgroundColor: '#0284c7', borderWidth: 3, tension: 0.25 }
    ];

    techHTML = `
      <div class="tech-section">
        <div class="tech-section-header">📊 Balance Hidráulico del Nodo (${latest.fecha})</div>
        <div class="tech-kpi-mini-grid">
          <div class="tech-kpi-box">
            <div class="lbl">Recepción Nodo Carrasco</div>
            <div class="val">${latest.recep_nodo_carrasco.toFixed(3)} MMmcd</div>
            <div class="sub">Prom: ${avgRecep.toFixed(2)} • ${formatDeltaBadge(latest.recep_nodo_carrasco, prev ? prev.recep_nodo_carrasco : null, '')}</div>
          </div>
          <div class="tech-kpi-box gold">
            <div class="lbl">Entregas Nodo Carrasco</div>
            <div class="val">${latest.entregas_carrasco.toFixed(3)} MMmcd</div>
            <div class="sub">Prom: ${avgEntCarr.toFixed(2)} • ${formatDeltaBadge(latest.entregas_carrasco, prev ? prev.entregas_carrasco : null, '')}</div>
          </div>
          <div class="tech-kpi-box">
            <div class="lbl">Entrega Conjunta GAA + GCC</div>
            <div class="val">${latest.entrega_gaa_gcc.toFixed(3)} MMmcd</div>
            <div class="sub">Prom: ${avgGaaGcc.toFixed(2)} MMmcd</div>
          </div>
          <div class="tech-kpi-box">
            <div class="lbl">Diferencial Neto en Nodo</div>
            <div class="val">${balanceNodo >= 0 ? '+' : ''}${balanceNodo.toFixed(3)} MMmcd</div>
            <div class="sub">${balanceNodo >= 0 ? 'Excedente inyectado a troncal' : 'Demanda cubierta por línea'}</div>
          </div>
        </div>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🔬 Desglose de Aportes y Salidas (PCD → MMmcd)</div>
        <div class="tech-analysis-text">
          <p><strong>Inyecciones (Recepción):</strong> Campos Carrasco (${(latest.carrasco_pcd/FACTOR_CONV/1e6).toFixed(3)} MMmcd), Yapacaní (${(latest.yapacani_pcd/FACTOR_CONV/1e6).toFixed(3)} MMmcd) y Santa Rosa (${(latest.santa_rosa_pcd/FACTOR_CONV/1e6).toFixed(3)} MMmcd).</p>
          <p><strong>Salidas Nodo Carrasco:</strong> Termoeléctrica ENDE Entre Ríos (${(latest.ende_entre_rios_pcd/FACTOR_CONV/1e6).toFixed(3)} MMmcd), Planta de Amoniaco y Urea PAU (${(latest.pau_pcd/FACTOR_CONV/1e6).toFixed(3)} MMmcd) y Gasoducto al Altiplano/GCC (${(latest.gcc_pcd/FACTOR_CONV/1e6).toFixed(3)} MMmcd).</p>
        </div>
      </div>

      <div class="tech-section">
        <div class="tech-section-header">🎓 Guía Didáctica: Arquitectura del Nodo Carrasco</div>
        <div class="didactic-callout">
          <strong>Importancia estratégica en la red de ductos:</strong>
          <ul>
            <li><strong>Nodo Carrasco:</strong> Punto neurálgico donde confluye la producción del área central (Carrasco, Yapacaní, Santa Rosa) para abastecer el occidente del país y los polos industriales de Bulo Bulo.</li>
            <li><strong>GAA y GCC:</strong> Corresponden al <em>Gasoducto al Altiplano</em> (abastece La Paz, Oruro, Cochabamba) y al <em>Gasoducto Carrasco–Cochabamba</em>.</li>
            <li><strong>Relación Recepción vs. Entrega:</strong> Cuando la recepción supera las entregas locales en Carrasco, el volumen remanente fluye hacia Santa Cruz / Río Grande para equilibrar el sistema troncal.</li>
          </ul>
        </div>
      </div>
    `;
  }

  document.getElementById('modalChartTitle').textContent = title;
  document.getElementById('modalChartSubtitle').textContent = subtitle;
  document.getElementById('modalTechSummary').innerHTML = techHTML;

  // Render high-resolution chart in modal
  const isPie = opts.isPieLike;
  const isHoriz = opts.horizontalBar;

  modalChartInstance = new Chart(ctx, {
    type: selectedType,
    data: { labels: chartLabels, datasets },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      indexAxis: isHoriz ? 'y' : 'x',
      layout: {
        padding: isPie ? { top: 20, bottom: 20, left: 24, right: 24 } : { top: 16, right: 18 }
      },
      interaction: {
        mode: isPie ? 'nearest' : 'index',
        intersect: isPie
      },
      plugins: {
        title: opts.chartSubtitle ? {
          display: true,
          text: opts.chartSubtitle,
          color: '#16382e',
          font: { size: 15, weight: 'bold' },
          padding: { top: 4, bottom: 10 }
        } : { display: false },
        legend: {
          position: isPie ? 'right' : 'bottom',
          labels: {
            boxWidth: 16,
            padding: 14,
            font: { size: 12.5, weight: 'bold' },
            color: '#1b2623'
          }
        },
        tooltip: {
          backgroundColor: 'rgba(22, 56, 46, 0.96)',
          titleFont: { size: 14, weight: 'bold' },
          bodyFont: { size: 13 },
          padding: 12,
          callbacks: {
            label: function(context) {
              const val = Number(context.raw) || 0;
              const mmpcd = (val * FACTOR_CONV).toFixed(1);
              if (isPie) {
                const total = opts.totalSum || 1;
                const pct = ((val / total) * 100).toFixed(1);
                return ` ${context.label}: ${val.toFixed(3)} MMmcd (${pct}% | ${mmpcd} MMpcd)`;
              }
              return ` ${context.dataset.label}: ${val.toFixed(3)} MMmcd (${mmpcd} MMpcd)`;
            }
          }
        },
        datalabels: isPie ? {
          display: true,
          color: '#ffffff',
          backgroundColor: 'rgba(17, 24, 39, 0.78)',
          borderRadius: 6,
          padding: { top: 5, bottom: 5, left: 8, right: 8 },
          font: { weight: 'bold', size: 12 },
          textAlign: 'center',
          formatter: (value, c) => {
            const total = opts.totalSum || 1;
            const pct = ((value / total) * 100).toFixed(1);
            const name = c.chart.data.labels[c.dataIndex].split('(')[0].trim();
            return `${name}
${value.toFixed(2)} MMmcd (${pct}%)`;
          }
        } : (isHoriz ? {
          display: true,
          anchor: 'end',
          align: 'right',
          color: '#16382e',
          font: { weight: 'bold', size: 12 },
          formatter: (val) => {
            const total = opts.totalSum || 1;
            const pct = ((val / total) * 100).toFixed(1);
            return `${val.toFixed(3)} MMmcd (${pct}%)`;
          }
        } : {
          display: !!opts.showLabels,
          align: 'top',
          color: '#16382e',
          font: { weight: 'bold', size: 10.5 },
          formatter: (v) => Number(v).toFixed(2)
        })
      },
      scales: isPie ? {} : {
        x: {
          stacked: !!opts.stacked,
          beginAtZero: isHoriz,
          ticks: { font: { size: 11.5, weight: '600' } },
          title: isHoriz ? { display: true, text: 'Volumen (MMmcd)', font: { size: 12, weight: 'bold' } } : {}
        },
        y: {
          stacked: !!opts.stacked,
          beginAtZero: !isHoriz,
          ticks: { font: { size: 11.5, weight: '600' } },
          title: !isHoriz ? { display: true, text: 'Volumen (MMmcd)', font: { size: 12, weight: 'bold' } } : {}
        }
      }
    }
  });
}

// Inicialización
loadDatabase();
setQuickRange(0);
prepareNextDay();
</script>
</body>
</html>
