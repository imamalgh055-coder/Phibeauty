<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FisioHub — Pendaftaran Pasien</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --teal: #0f766e;
    --teal-light: #14b8a6;
    --teal-pale: #ccfbf1;
    --navy: #0f172a;
    --slate: #334155;
    --muted: #94a3b8;
    --bg: #f0fdfa;
    --white: #ffffff;
    --border: #e2e8f0;
    --error: #ef4444;
    --success: #22c55e;
    --shadow: 0 4px 24px rgba(15,118,110,0.10);
    --radius: 16px;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--navy);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  /* ── HEADER ── */
  header {
    background: var(--white);
    border-bottom: 1px solid var(--border);
    padding: 0 32px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 64px;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 1px 8px rgba(0,0,0,0.04);
  }
  .logo {
    display: flex;
    align-items: center;
    gap: 10px;
    text-decoration: none;
  }
  .logo-icon {
    width: 36px; height: 36px;
    background: linear-gradient(135deg, var(--teal), var(--teal-light));
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
  }
  .logo-text {
    font-family: 'DM Serif Display', serif;
    font-size: 20px;
    color: var(--navy);
    letter-spacing: -0.3px;
  }
  .logo-text span { color: var(--teal); }
  nav { display: flex; gap: 4px; }
  nav button {
    background: none; border: none; cursor: pointer;
    padding: 8px 14px; border-radius: 8px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px; font-weight: 500;
    color: var(--slate); transition: all .2s;
  }
  nav button:hover { background: var(--teal-pale); color: var(--teal); }
  nav button.active { background: var(--teal-pale); color: var(--teal); }
  .header-right { display: flex; align-items: center; gap: 12px; }
  .badge {
    background: var(--teal); color: #fff;
    font-size: 11px; font-weight: 600;
    padding: 3px 9px; border-radius: 20px;
  }
  .avatar {
    width: 36px; height: 36px; border-radius: 50%;
    background: linear-gradient(135deg, #0f766e, #14b8a6);
    display: flex; align-items: center; justify-content: center;
    color: #fff; font-weight: 600; font-size: 13px;
    cursor: pointer;
  }

  /* ── LAYOUT ── */
  .app { display: flex; flex: 1; }

  /* ── SIDEBAR ── */
  aside {
    width: 240px; min-height: calc(100vh - 64px);
    background: var(--white);
    border-right: 1px solid var(--border);
    padding: 24px 16px;
    display: flex; flex-direction: column; gap: 4px;
    position: sticky; top: 64px; height: calc(100vh - 64px);
    overflow-y: auto;
  }
  .sidebar-section {
    font-size: 10px; font-weight: 700;
    color: var(--muted); letter-spacing: 1.2px;
    text-transform: uppercase;
    padding: 12px 12px 6px;
  }
  .sidebar-item {
    display: flex; align-items: center; gap: 10px;
    padding: 10px 12px; border-radius: 10px;
    cursor: pointer; transition: all .2s;
    font-size: 14px; font-weight: 500; color: var(--slate);
    border: none; background: none; width: 100%; text-align: left;
  }
  .sidebar-item:hover { background: var(--teal-pale); color: var(--teal); }
  .sidebar-item.active { background: var(--teal); color: #fff; }
  .sidebar-item .icon { font-size: 17px; width: 22px; text-align: center; }
  .sidebar-count {
    margin-left: auto;
    background: rgba(15,118,110,.12); color: var(--teal);
    font-size: 11px; font-weight: 700;
    padding: 2px 7px; border-radius: 20px;
  }
  .sidebar-item.active .sidebar-count { background: rgba(255,255,255,.2); color: #fff; }

  /* ── MAIN ── */
  main {
    flex: 1; padding: 32px;
    overflow-y: auto;
    animation: fadeIn .4s ease;
  }
  @keyframes fadeIn { from { opacity:0; transform: translateY(8px); } to { opacity:1; transform: none; } }

  .page-header {
    display: flex; align-items: flex-start;
    justify-content: space-between; margin-bottom: 28px;
  }
  .page-title {
    font-family: 'DM Serif Display', serif;
    font-size: 28px; color: var(--navy);
    letter-spacing: -0.5px;
  }
  .page-subtitle { font-size: 14px; color: var(--muted); margin-top: 4px; }
  .btn {
    padding: 10px 20px; border-radius: 10px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px; font-weight: 600;
    cursor: pointer; transition: all .2s;
    border: none; display: inline-flex; align-items: center; gap: 8px;
  }
  .btn-primary {
    background: var(--teal); color: #fff;
    box-shadow: 0 4px 12px rgba(15,118,110,.3);
  }
  .btn-primary:hover { background: #0d6460; transform: translateY(-1px); }
  .btn-outline { background: #fff; color: var(--teal); border: 1.5px solid var(--teal); }
  .btn-outline:hover { background: var(--teal-pale); }

  /* ── STATS CARDS ── */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px; margin-bottom: 28px;
  }
  .stat-card {
    background: var(--white);
    border-radius: var(--radius);
    padding: 20px 22px;
    border: 1px solid var(--border);
    box-shadow: var(--shadow);
    transition: transform .2s;
  }
  .stat-card:hover { transform: translateY(-2px); }
  .stat-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 12px; }
  .stat-icon {
    width: 42px; height: 42px; border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px;
  }
  .stat-icon.teal { background: var(--teal-pale); }
  .stat-icon.blue { background: #dbeafe; }
  .stat-icon.amber { background: #fef3c7; }
  .stat-icon.rose { background: #ffe4e6; }
  .stat-change {
    font-size: 11px; font-weight: 600; padding: 3px 8px; border-radius: 20px;
  }
  .stat-change.up { background: #dcfce7; color: #16a34a; }
  .stat-change.down { background: #fee2e2; color: #dc2626; }
  .stat-value {
    font-family: 'DM Serif Display', serif;
    font-size: 30px; color: var(--navy); line-height: 1;
  }
  .stat-label { font-size: 13px; color: var(--muted); margin-top: 4px; }

  /* ── TABS ── */
  .tabs { display: flex; gap: 4px; margin-bottom: 20px; }
  .tab {
    padding: 8px 16px; border-radius: 8px;
    font-size: 13px; font-weight: 500;
    cursor: pointer; transition: all .2s;
    border: none; background: none; color: var(--muted);
  }
  .tab.active { background: var(--teal); color: #fff; }
  .tab:hover:not(.active) { background: var(--teal-pale); color: var(--teal); }

  /* ── TABLE SECTION ── */
  .table-card {
    background: var(--white);
    border-radius: var(--radius);
    border: 1px solid var(--border);
    box-shadow: var(--shadow);
    overflow: hidden;
    margin-bottom: 28px;
  }
  .table-toolbar {
    display: flex; align-items: center; gap: 12px;
    padding: 16px 20px;
    border-bottom: 1px solid var(--border);
  }
  .search-box {
    flex: 1; max-width: 280px;
    display: flex; align-items: center; gap: 8px;
    background: var(--bg); border: 1.5px solid var(--border);
    border-radius: 10px; padding: 8px 12px;
    transition: border-color .2s;
  }
  .search-box:focus-within { border-color: var(--teal-light); }
  .search-box input {
    border: none; background: none; outline: none;
    font-family: 'DM Sans', sans-serif;
    font-size: 13px; color: var(--navy); flex: 1;
  }
  .search-box span { color: var(--muted); font-size: 15px; }
  .filter-btn {
    padding: 8px 14px; border-radius: 10px;
    border: 1.5px solid var(--border);
    background: #fff; color: var(--slate);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px; cursor: pointer;
    display: flex; align-items: center; gap: 6px;
    transition: all .2s;
    outline: none;
  }
  .filter-btn:hover { border-color: var(--teal); color: var(--teal); }

  table { width: 100%; border-collapse: collapse; }
  thead { background: var(--bg); }
  thead th {
    padding: 12px 16px; text-align: left;
    font-size: 11px; font-weight: 700;
    color: var(--muted); text-transform: uppercase;
    letter-spacing: .8px;
  }
  tbody tr {
    border-top: 1px solid var(--border);
    transition: background .15s;
    cursor: pointer;
  }
  tbody tr:hover { background: #f8fffe; }
  tbody td { padding: 14px 16px; font-size: 14px; }
  .patient-cell { display: flex; align-items: center; gap: 10px; }
  .patient-avatar {
    width: 34px; height: 34px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-weight: 700; font-size: 13px; flex-shrink: 0;
  }
  .patient-name { font-weight: 600; color: var(--navy); font-size: 14px; }
  .patient-id { font-size: 11px; color: var(--muted); margin-top: 1px; }
  .status-badge {
    padding: 4px 10px; border-radius: 20px;
    font-size: 11px; font-weight: 700;
    display: inline-flex; align-items: center; gap: 5px;
  }
  .status-badge::before {
    content: ''; width: 6px; height: 6px;
    border-radius: 50%;
    background: currentColor;
  }
  .status-menunggu { background: #fef3c7; color: #d97706; }
  .status-proses   { background: #dbeafe; color: #2563eb; }
  .status-selesai  { background: #dcfce7; color: #16a34a; }
  .status-batal    { background: #fee2e2; color: #dc2626; }
  .action-btn {
    padding: 6px 12px; border-radius: 7px;
    font-size: 12px; font-weight: 600;
    cursor: pointer; transition: all .2s;
    border: none;
  }
  .action-detail { background: var(--teal-pale); color: var(--teal); }
  .action-detail:hover { background: var(--teal); color: #fff; }

  /* ── PAGINATION ── */
  .pagination {
    display: flex; align-items: center;
    justify-content: space-between;
    padding: 14px 20px;
    border-top: 1px solid var(--border);
  }
  .pagination-info { font-size: 13px; color: var(--muted); }
  .pagination-btns { display: flex; gap: 4px; }
  .page-btn {
    width: 32px; height: 32px; border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    border: 1.5px solid var(--border);
    background: #fff; font-size: 13px; font-weight: 600;
    cursor: pointer; transition: all .2s;
    color: var(--slate);
  }
  .page-btn.active { background: var(--teal); border-color: var(--teal); color: #fff; }
  .page-btn:hover:not(.active) { border-color: var(--teal); color: var(--teal); }

  /* ── GRID BOTTOM ── */
  .bottom-grid {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 20px;
  }
  .card {
    background: var(--white);
    border-radius: var(--radius);
    border: 1px solid var(--border);
    box-shadow: var(--shadow);
    padding: 22px;
  }
  .card-title {
    font-weight: 700; font-size: 15px; color: var(--navy);
    margin-bottom: 16px; display: flex; align-items: center;
    justify-content: space-between;
  }
  .card-title a {
    font-size: 12px; font-weight: 500;
    color: var(--teal); text-decoration: none;
  }

  /* ── JADWAL ── */
  .schedule-item {
    display: flex; align-items: center; gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }
  .schedule-item:last-child { border-bottom: none; }
  .schedule-time {
    min-width: 52px; font-size: 12px; font-weight: 700;
    color: var(--teal); text-align: right;
  }
  .schedule-dot {
    width: 10px; height: 10px; border-radius: 50%;
    flex-shrink: 0;
  }
  .schedule-info { flex: 1; }
  .schedule-name { font-size: 13px; font-weight: 600; color: var(--navy); }
  .schedule-type { font-size: 11px; color: var(--muted); margin-top: 2px; }
  .schedule-therapist {
    font-size: 11px; padding: 3px 8px; border-radius: 20px;
    background: var(--teal-pale); color: var(--teal); font-weight: 600;
    white-space: nowrap;
  }

  /* ── AKTIVITAS ── */
  .activity-item {
    display: flex; gap: 12px; margin-bottom: 14px;
  }
  .activity-icon {
    width: 34px; height: 34px; border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px; flex-shrink: 0;
  }
  .activity-text { font-size: 13px; color: var(--slate); line-height: 1.5; }
  .activity-text strong { color: var(--navy); }
  .activity-time { font-size: 11px; color: var(--muted); margin-top: 3px; }

  /* ── SUBPAGE FORMATS ── */
  .subpage-container {
    padding: 8px 4px;
  }
  .subpage-title {
    font-family: 'DM Serif Display', serif;
    font-size: 24px; color: var(--navy);
    margin-bottom: 20px;
  }
  .custom-table {
    width: 100%; border-collapse: collapse; background: var(--white);
    border-radius: var(--radius); border: 1px solid var(--border); overflow: hidden;
  }
  .custom-table th { background: var(--bg); padding: 14px 16px; text-align: left; font-size: 12px; color: var(--muted); text-transform: uppercase; letter-spacing: .5px;}
  .custom-table td { padding: 14px 16px; border-top: 1px solid var(--border); font-size: 14px;}

  /* ── MODAL ── */
  .modal-overlay {
    position: fixed; inset: 0;
    background: rgba(15,23,42,.45);
    backdrop-filter: blur(4px);
    display: flex; align-items: center; justify-content: center;
    z-index: 500;
    opacity: 0; pointer-events: none;
    transition: opacity .25s;
  }
  .modal-overlay.open { opacity: 1; pointer-events: all; }
  .modal {
    background: #fff;
    border-radius: 20px;
    width: 520px; max-width: 95vw;
    max-height: 90vh; overflow-y: auto;
    box-shadow: 0 24px 64px rgba(0,0,0,.2);
    transform: translateY(20px) scale(.97);
    transition: transform .25s;
    padding: 30px;
  }
  .modal-overlay.open .modal { transform: none; }
  .modal-header {
    display: flex; justify-content: space-between; align-items: center;
    margin-bottom: 24px;
  }
  .modal-title {
    font-family: 'DM Serif Display', serif;
    font-size: 22px; color: var(--navy);
  }
  .modal-close {
    width: 32px; height: 32px; border-radius: 8px;
    border: none; background: var(--bg); cursor: pointer;
    font-size: 18px; color: var(--muted);
    display: flex; align-items: center; justify-content: center;
    transition: background .2s;
  }
  .modal-close:hover { background: #fee2e2; color: var(--error); }
  .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .form-full { grid-column: 1 / -1; }
  .form-group { display: flex; flex-direction: column; gap: 6px; }
  .form-label { font-size: 12px; font-weight: 700; color: var(--slate); letter-spacing: .3px; }
  .form-input, .form-select, .form-textarea {
    padding: 10px 13px;
    border: 1.5px solid var(--border);
    border-radius: 10px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px; color: var(--navy);
    outline: none; transition: border-color .2s;
    background: #fff;
  }
  .form-input:focus, .form-select:focus, .form-textarea:focus { border-color: var(--teal-light); }
  .form-textarea { resize: vertical; min-height: 72px; }
  .form-divider {
    font-size: 11px; font-weight: 700; color: var(--muted);
    letter-spacing: 1px; text-transform: uppercase;
    margin: 8px 0 4px;
    grid-column: 1/-1;
    padding-top: 8px; border-top: 1px solid var(--border);
  }
  .modal-footer { display: flex; justify-content: flex-end; gap: 10px; margin-top: 24px; }

  /* ── TOAST ── */
  .toast {
    position: fixed; bottom: 24px; right: 24px;
    background: var(--navy); color: #fff;
    padding: 12px 20px; border-radius: 12px;
    font-size: 13px; font-weight: 500;
    box-shadow: 0 8px 24px rgba(0,0,0,.2);
    transform: translateY(80px); opacity: 0;
    transition: all .3s cubic-bezier(.34,1.56,.64,1);
    z-index: 999; display: flex; align-items: center; gap: 8px;
  }
  .toast.show { transform: none; opacity: 1; }
  .toast .toast-icon { font-size: 16px; }

  @media (max-width: 1100px) {
    .stats-grid { grid-template-columns: repeat(2,1fr); }
    .bottom-grid { grid-template-columns: 1fr; }
  }
  @media (max-width: 768px) {
    aside { display: none; }
    main { padding: 20px; }
    .stats-grid { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>

<header>
  <a href="#" class="logo">
    <div class="logo-icon">🏥</div>
    <span class="logo-text">Fisio<span>Hub</span></span>
  </a>
  <nav>
    <button class="active" onclick="showPage('dashboard')">Dashboard</button>
    <button onclick="showPage('pasien')">Pasien</button>
    <button onclick="showPage('terapis')">Terapis</button>
    <button onclick="showPage('laporan')">Laporan</button>
  </nav>
  <div class="header-right">
    <span class="badge">3 Antrian</span>
    <div class="avatar">AD</div>
  </div>
</header>

<div class="app">
  <aside>
    <span class="sidebar-section">Menu Utama</span>
    <button class="sidebar-item active" onclick="showPage('dashboard')">
      <span class="icon">📊</span> Dashboard
    </button>
    <button class="sidebar-item" onclick="showPage('pasien')">
      <span class="icon">📋</span> Pendaftaran
      <span class="sidebar-count">12</span>
    </button>
    <button class="sidebar-item" onclick="showPage('pasien')">
      <span class="icon">🧑‍⚕️</span> Data Pasien
      <span class="sidebar-count">248</span>
    </button>
    <button class="sidebar-item" onclick="showPage('dashboard')">
      <span class="icon">📅</span> Jadwal Sesi
    </button>
    <button class="sidebar-item" onclick="showPage('laporan')">
      <span class="icon">💊</span> Rekam Medis
    </button>

    <span class="sidebar-section">Manajemen</span>
    <button class="sidebar-item" onclick="showPage('terapis')">
      <span class="icon">👩‍⚕️</span> Terapis
    </button>
    <button class="sidebar-item" onclick="showPage('dashboard')">
      <span class="icon">🏷️</span> Jenis Terapi
    </button>
    <button class="sidebar-item" onclick="showPage('laporan')">
      <span class="icon">💰</span> Pembayaran
    </button>
    <button class="sidebar-item" onclick="showPage('laporan')">
      <span class="icon">📈</span> Laporan
    </button>

    <span class="sidebar-section">Sistem</span>
    <button class="sidebar-item">
      <span class="icon">⚙️</span> Pengaturan
    </button>
  </aside>

  <main>
    
    <div id="dashboardPage">
      <div class="page-header">
        <div>
          <h1 class="page-title">Selamat Pagi, Admin 👋</h1>
          <p class="page-subtitle">Senin, 9 Juni 2025 · Klinik Fisioterapi Sehat Prima</p>
        </div>
        <button class="btn btn-primary" onclick="openModal()">
          ＋ Daftarkan Pasien
        </button>
      </div>

      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-top">
            <div class="stat-icon teal">🧑‍🦽</div>
            <span class="stat-change up">↑ 8%</span>
          </div>
          <div class="stat-value">248</div>
          <div class="stat-label">Total Pasien Terdaftar</div>
        </div>
        <div class="stat-card">
          <div class="stat-top">
            <div class="stat-icon blue">📅</div>
            <span class="stat-change up">↑ 3%</span>
          </div>
          <div class="stat-value">18</div>
          <div class="stat-label">Sesi Hari Ini</div>
        </div>
        <div class="stat-card">
          <div class="stat-top">
            <div class="stat-icon amber">⏳</div>
            <span class="stat-change down">↓ 1</span>
          </div>
          <div class="stat-value">3</div>
          <div class="stat-label">Menunggu Antrian</div>
        </div>
        <div class="stat-card">
          <div class="stat-top">
            <div class="stat-icon rose">✅</div>
            <span class="stat-change up">↑ 5%</span>
          </div>
          <div class="stat-value">15</div>
          <div class="stat-label">Sesi Selesai Hari Ini</div>
        </div>
      </div>

      <div class="table-card">
        <div class="table-toolbar">
          <div class="search-box">
            <span>🔍</span>
            <input type="text" placeholder="Cari nama atau ID pasien…" oninput="filterTable(this.value)">
          </div>
          <div class="tabs" style="margin-bottom:0">
            <button class="tab active" onclick="setTab(this,'Semua')">Semua</button>
            <button class="tab" onclick="setTab(this,'Menunggu')">Menunggu</button>
            <button class="tab" onclick="setTab(this,'Proses')">Dalam Proses</button>
            <button class="tab" onclick="setTab(this,'Selesai')">Selesai</button>
          </div>
          <select class="filter-btn" onchange="filterTherapist(this.value)" style="margin-left:auto">
            <option value="">Semua Terapis</option>
            <option>Ftr. Al Subur</option>
            <option>Ftr. Dittany Sechan</option>
            <option>Ftr. Ayanami Rei</option>
            <option>Ftr. Satrio Adi</option>
          </select>
          <button class="filter-btn">⬇ Export</button>
        </div>

        <table id="patientTable">
          <thead>
            <tr>
              <th>Pasien</th>
              <th>Jenis Terapi</th>
              <th>Terapis</th>
              <th>Tanggal</th>
              <th>Jam</th>
              <th>Status</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody id="tableBody"></tbody>
        </table>

        <div class="pagination">
          <span class="pagination-info">Menampilkan 1–6 dari 248 pasien</span>
          <div class="pagination-btns">
            <button class="page-btn">‹</button>
            <button class="page-btn active">1</button>
            <button class="page-btn">2</button>
            <button class="page-btn">3</button>
            <button class="page-btn">›</button>
          </div>
        </div>
      </div>

      <div class="bottom-grid">
        <div class="card">
          <div class="card-title">
            📅 Jadwal Sesi Hari Ini
            <a href="#">Lihat Semua →</a>
          </div>
          <div id="scheduleList"></div>
        </div>

        <div class="card">
          <div class="card-title">
            🔔 Aktivitas Terbaru
            <a href="#">Semua →</a>
          </div>
          <div id="activityList"></div>
        </div>
      </div>
    </div>

    <div id="pasienPage" style="display:none;" class="subpage-container">
      <h2 class="subpage-title">Data Pasien</h2>
      <table class="custom-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Nama</th>
            <th>Terapi</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody id="pasienList"></tbody>
      </table>
    </div>

    <div id="terapisPage" style="display:none;" class="subpage-container">
      <h2 class="subpage-title">Data Terapis</h2>
      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-value" style="font-size:20px; font-family:'DM Sans',sans-serif; font-weight:600; margin-bottom:8px;">Ftr. Al Subur</div>
          <div class="stat-label">Spesialis Muskuloskeletal & Olahraga</div>
        </div>
        <div class="stat-card">
          <div class="stat-value" style="font-size:20px; font-family:'DM Sans',sans-serif; font-weight:600; margin-bottom:8px;">Ftr. Dittany Sechan</div>
          <div class="stat-label">Spesialis Neurologis</div>
        </div>
        <div class="stat-card">
          <div class="stat-value" style="font-size:20px; font-family:'DM Sans',sans-serif; font-weight:600; margin-bottom:8px;">Ftr. Ayanami Rei</div>
          <div class="stat-label">Spesialis Pediatri</div>
        </div>
        <div class="stat-card">
          <div class="stat-value" style="font-size:20px; font-family:'DM Sans',sans-serif; font-weight:600; margin-bottom:8px;">Ftr. Satrio Adi</div>
          <div class="stat-label">Spesialis Geriatri & Kardiorespirasi</div>
        </div>
      </div>
    </div>

    <div id="laporanPage" style="display:none;" class="subpage-container">
      <h2 class="subpage-title">Laporan Klinik</h2>
      <div class="card" style="line-height: 2;">
        <p><strong>Total Pasien:</strong> 248</p>
        <p><strong>Sesi Hari Ini:</strong> 18</p>
        <p><strong>Selesai:</strong> 15</p>
        <p><strong>Menunggu:</strong> 3</p>
        <p><strong>Tingkat Kehadiran:</strong> 92%</p>
        <p><strong>Terapi Terbanyak:</strong> Muskuloskeletal</p>
      </div>
    </div>

  </main>
</div>

<div class="modal-overlay" id="modalOverlay" onclick="closeModalOutside(event)">
  <div class="modal" id="modalBox">
    <div class="modal-header">
      <h2 class="modal-title">Daftarkan Pasien Baru</h2>
      <button class="modal-close" onclick="closeModal()">✕</button>
    </div>

    <div class="form-grid">
      <div class="form-group form-full">
        <span class="form-divider">Data Pribadi</span>
      </div>
      <div class="form-group">
        <label class="form-label">Nama Lengkap *</label>
        <input type="text" class="form-input" placeholder="Budi Santoso" id="fNama">
      </div>
      <div class="form-group">
        <label class="form-label">No. KTP / NIK</label>
        <input type="text" class="form-input" placeholder="3271xxxxxxxxxxxx">
      </div>
      <div class="form-group">
        <label class="form-label">Tanggal Lahir</label>
        <input type="date" class="form-input">
      </div>
      <div class="form-group">
        <label class="form-label">Jenis Kelamin</label>
        <select class="form-select">
          <option value="">Pilih…</option>
          <option>Laki-laki</option>
          <option>Perempuan</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">No. HP *</label>
        <input type="tel" class="form-input" placeholder="08xx-xxxx-xxxx">
      </div>
      <div class="form-group">
        <label class="form-label">Email</label>
        <input type="email" class="form-input" placeholder="email@contoh.com">
      </div>

      <div class="form-group form-full">
        <span class="form-divider">Data Kunjungan</span>
      </div>
      <div class="form-group">
        <label class="form-label">Jenis Terapi *</label>
        <select class="form-select" id="fTerapi">
          <option value="">Pilih Terapi…</option>
          <option>Terapi Muskuloskeletal</option>
          <option>Terapi Neurologis</option>
          <option>Terapi Pediatri</option>
          <option>Terapi Olahraga</option>
          <option>Terapi Geriatri</option>
          <option>Terapi Kardiorespirasi</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">Terapis *</label>
        <select class="form-select" id="fTerapis">
          <option value="">Pilih Terapis…</option>
          <option>Ftr. Al Subur</option>
          <option>Ftr. Dittany Sechan</option>
          <option>Ftr. Ayanami Rei</option>
          <option>Ftr. Satrio Adi</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">Tanggal Kunjungan *</label>
        <input type="date" class="form-input" id="fTanggal">
      </div>
      <div class="form-group">
        <label class="form-label">Jam Kunjungan *</label>
        <select class="form-select" id="fJam">
          <option value="">Pilih Jam…</option>
          <option>08:00</option><option>08:30</option>
          <option>09:00</option><option>09:30</option>
          <option>10:00</option><option>10:30</option>
          <option>11:00</option><option>13:00</option>
          <option>13:30</option><option>14:00</option>
          <option>14:30</option><option>15:00</option>
        </select>
      </div>
      <div class="form-group form-full">
        <label class="form-label">Keluhan / Catatan</label>
        <textarea class="form-textarea" placeholder="Jelaskan keluhan pasien secara singkat…"></textarea>
      </div>
    </div>

    <div class="modal-footer">
      <button class="btn btn-outline" onclick="closeModal()">Batal</button>
      <button class="btn btn-primary" onclick="submitForm()">Simpan Pendaftaran</button>
    </div>
  </div>
</div>

<div class="toast" id="toast">
  <span class="toast-icon">✅</span>
  <span id="toastMsg">Pasien berhasil didaftarkan!</span>
</div>

<script>
/* ── DATA ── */
const colors = ['#0f766e','#2563eb','#7c3aed','#db2777','#ea580c','#16a34a','#0284c7'];
const initials = n => n.split(' ').map(w=>w[0]).join('').slice(0,2).toUpperCase();

const patients = [
  { id:'PT-2501', name:'Siti Rahayu', terapi:'Muskuloskeletal', terapis:'Ftr. Al Subur', tanggal:'09/06/2025', jam:'08:00', status:'selesai' },
  { id:'PT-2502', name:'Budi Hartono', terapi:'Neurologis', terapis:'Ftr. Dittany Sechan', tanggal:'09/06/2025', jam:'08:30', status:'selesai' },
  { id:'PT-2503', name:'Mega Wulandari', terapi:'Pediatri', terapis:'Ftr. Ayanami Rei', tanggal:'09/06/2025', jam:'09:00', status:'proses' },
  { id:'PT-2504', name:'Hendra Kusuma', terapi:'Olahraga', terapis:'Ftr. Satrio Adi', tanggal:'09/06/2025', jam:'09:30', status:'menunggu' },
  { id:'PT-2505', name:'Rina Agustina', terapi:'Geriatri', terapis:'Ftr. Al Subur', tanggal:'09/06/2025', jam:'10:00', status:'menunggu' },
  { id:'PT-2506', name:'Doni Prasetyo', terapi:'Kardiorespirasi', terapis:'Ftr. Dittany Sechan', tanggal:'09/06/2025', jam:'10:30', status:'menunggu' }
];

const statusLabel = { selesai:'Selesai', proses:'Dalam Proses', menunggu:'Menunggu', batal:'Dibatalkan' };

let allPatients = [...patients];
let currentFilter = 'Semua';

function renderTable(data) {
  const tbody = document.getElementById('tableBody');
  tbody.innerHTML = data.map((p,i) => {
    const color = colors[i % colors.length];
    return `<tr>
      <td>
        <div class="patient-cell">
          <div class="patient-avatar" style="background:${color}18;color:${color}">${initials(p.name)}</div>
          <div>
            <div class="patient-name">${p.name}</div>
            <div class="patient-id">${p.id}</div>
          </div>
        </div>
      </td>
      <td>${p.terapi}</td>
      <td>${p.terapis}</td>
      <td>${p.tanggal}</td>
      <td><strong>${p.jam}</strong></td>
      <td><span class="status-badge status-${p.status}">${statusLabel[p.status]}</span></td>
      <td><button class="action-btn action-detail" onclick="showDetail('${p.id}')">Detail</button></td>
    </tr>`;
  }).join('');
}

function filterTable(query) {
  let data = allPatients;
  if (currentFilter !== 'Semua') {
    const map = { Menunggu:'menunggu', Proses:'proses', Selesai:'selesai' };
    data = data.filter(p => p.status === map[currentFilter]);
  }
  if (query) data = data.filter(p =>
    p.name.toLowerCase().includes(query.toLowerCase()) ||
    p.id.toLowerCase().includes(query.toLowerCase())
  );
  renderTable(data);
}

function setTab(btn, label) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
  currentFilter = label;
  filterTable(document.querySelector('.search-box input').value);
}

function filterTherapist(name){
  if(name===""){
    renderTable(allPatients);
    return;
  }
  const filtered = allPatients.filter(p => p.terapis === name);
  renderTable(filtered);
}

/* ── SCHEDULE ── */
const schedules = [
  { time:'08:00', name:'Siti Rahayu',    type:'Muskuloskeletal', therapist:'Al Subur', color:'#0f766e' },
  { time:'08:30', name:'Budi Hartono',   type:'Neurologis',      therapist:'Dittany', color:'#2563eb' },
  { time:'09:00', name:'Mega Wulandari', type:'Pediatri',        therapist:'Ayanami', color:'#7c3aed' },
  { time:'09:30', name:'Hendra Kusuma',  type:'Olahraga',        therapist:'Satrio', color:'#ea580c' },
  { time:'10:00', name:'Rina Agustina',  type:'Geriatri',        therapist:'Al Subur', color:'#db2777' },
];
document.getElementById('scheduleList').innerHTML = schedules.map(s => `
  <div class="schedule-item">
    <span class="schedule-time">${s.time}</span>
    <div class="schedule-dot" style="background:${s.color}"></div>
    <div class="schedule-info">
      <div class="schedule-name">${s.name}</div>
      <div class="schedule-type">${s.type}</div>
    </div>
    <span class="schedule-therapist">${s.therapist}</span>
  </div>`).join('');

/* ── ACTIVITY ── */
const activities = [
  { icon:'📋', bg:'#ccfbf1', text:'<strong>Pasien baru</strong> Siti Rahayu berhasil didaftarkan', time:'5 menit lalu' },
  { icon:'✅', bg:'#dcfce7', text:'<strong>Sesi selesai</strong> Budi Hartono — Terapi Neurologis', time:'32 menit lalu' },
  { icon:'⏰', bg:'#fef3c7', text:'<strong>Pengingat:</strong> Hendra Kusuma antrian 09:30', time:'1 jam lalu' },
  { icon:'🔄', bg:'#dbeafe', text:'<strong>Jadwal diubah</strong> Rina Agustina dari 11:00 → 10:00', time:'2 jam lalu' },
];
document.getElementById('activityList').innerHTML = activities.map(a => `
  <div class="activity-item">
    <div class="activity-icon" style="background:${a.bg}">${a.icon}</div>
    <div>
      <div class="activity-text">${a.text}</div>
      <div class="activity-time">${a.time}</div>
    </div>
  </div>`).join('');

/* ── MODAL ── */
function openModal() {
  document.getElementById('modalOverlay').classList.add('open');
}
function closeModal() {
  document.getElementById('modalOverlay').classList.remove('open');
}
function closeModalOutside(e) {
  if (e.target === document.getElementById('modalOverlay')) closeModal();
}
function submitForm() {
  const nama = document.getElementById('fNama').value.trim();
  const terapi = document.getElementById('fTerapi').value;
  const terapis = document.getElementById('fTerapis').value;
  if (!nama || !terapi || !terapis) {
    showToast('⚠️ Lengkapi data wajib terlebih dahulu!'); return;
  }
  const newId = 'PT-' + (2500 + allPatients.length + 1);
  const tanggal = document.getElementById('fTanggal').value || '09/06/2025';
  const jam = document.getElementById('fJam').value || '08:00';
  allPatients.unshift({ id: newId, name: nama, terapi, terapis, tanggal, jam, status: 'menunggu' });
  renderTable(allPatients);
  closeModal();
  showToast('✅ ' + nama + ' berhasil didaftarkan!');
}
function showDetail(id) {
  const p = allPatients.find(x => x.id === id);
  showToast('📋 Detail: ' + p.name + ' — ' + p.terapi);
}

/* ── TOAST ── */
function showToast(msg) {
  const t = document.getElementById('toast');
  document.getElementById('toastMsg').textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3200);
}

/* ── PAGE NAVIGATION ── */
function showPage(page){
  document.getElementById('dashboardPage').style.display='none';
  document.getElementById('pasienPage').style.display='none';
  document.getElementById('terapisPage').style.display='none';
  document.getElementById('laporanPage').style.display='none';

  // Sinkronisasi status active pada navbar header & sidebar
  document.querySelectorAll('nav button, aside .sidebar-item').forEach(btn => {
    btn.classList.remove('active');
    if(btn.textContent.toLowerCase().includes(page)) {
      btn.classList.add('active');
    }
  });

  if(page==='dashboard'){
    document.getElementById('dashboardPage').style.display='block';
  }

  if(page==='pasien'){
    document.getElementById('pasienPage').style.display='block';
    const tbody = document.getElementById('pasienList');
    tbody.innerHTML = allPatients.map(p=>`
      <tr>
        <td><strong>${p.id}</strong></td>
        <td>${p.name}</td>
        <td>${p.terapi}</td>
        <td><span class="status-badge status-${p.status}">${statusLabel[p.status]}</span></td>
      </tr>
    `).join('');
  }

  if(page==='terapis'){
    document.getElementById('terapisPage').style.display='block';
  }

  if(page==='laporan'){
    document.getElementById('laporanPage').style.display='block';
  }
}

/* ── INIT ── */
renderTable(allPatients);
</script>
</body>
</html>
