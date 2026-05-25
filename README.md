<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GOAT KIDS — Store Network</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
  <style>
    /* ─── RESET & TOKENS ─── */
    *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
    :root {
      --primary:#667eea; --primary-d:#5a67d8; --primary-l:#ebedff;
      --secondary:#ed64a6; --success:#48bb78; --warning:#ecc94b;
      --danger:#f56565; --info:#4299e1;
      --dark:#2d3748; --mid:#4a5568; --gray:#718096; --gray-l:#e2e8f0; --light:#f7fafc;
      --r:8px; --rl:12px; --rxl:18px;
      --sh:0 1px 4px rgba(0,0,0,.09);
      --shd:0 8px 24px rgba(0,0,0,.12);
      --font:'Segoe UI',system-ui,sans-serif;
    }
    body { font-family:var(--font); background:var(--light); color:var(--dark); min-height:100vh; }
    .hidden { display:none !important; }
    button { cursor:pointer; font-family:inherit; }
    input,select,textarea { font-family:inherit; }

    /* ─── TOPBAR ─── */
    .topbar {
      position:sticky; top:0; z-index:200;
      background:#fff; border-bottom:1px solid var(--gray-l);
      display:flex; align-items:center; justify-content:space-between;
      padding:.75rem 1.25rem; gap:.75rem;
    }
    .topbar-logo { font-size:1.2rem; font-weight:800; color:var(--primary); display:flex; align-items:center; gap:.4rem; }
    .topbar-logo span.goat { font-size:1.4rem; }
    .store-pill {
      display:flex; align-items:center; gap:.5rem;
      background:var(--primary-l); border-radius:2rem;
      padding:.35rem .9rem; font-size:.82rem; font-weight:600; color:var(--primary-d);
    }
    .store-pill .dot { width:8px; height:8px; border-radius:50%; background:var(--success); }
    .topbar-right { display:flex; align-items:center; gap:.5rem; }
    .icon-btn {
      width:38px; height:38px; border:none; border-radius:50%;
      background:var(--light); color:var(--mid); font-size:1rem;
      display:flex; align-items:center; justify-content:center;
      transition:background .2s;
    }
    .icon-btn:hover { background:var(--gray-l); }
    .badge-wrap { position:relative; }
    .notif-dot {
      position:absolute; top:2px; right:2px;
      width:10px; height:10px; border-radius:50%; background:var(--secondary);
      border:2px solid #fff;
    }

    /* ─── NAV TABS ─── */
    .nav-tabs {
      display:flex; background:#fff; border-bottom:2px solid var(--gray-l);
      overflow-x:auto; scrollbar-width:none;
      position:sticky; top:57px; z-index:199;
    }
    .nav-tabs::-webkit-scrollbar { display:none; }
    .nav-tab {
      flex:1; min-width:80px; padding:.8rem .5rem;
      border:none; background:none; font-size:.78rem; font-weight:600;
      color:var(--gray); display:flex; flex-direction:column; align-items:center; gap:.2rem;
      border-bottom:3px solid transparent; margin-bottom:-2px;
      transition:all .2s; white-space:nowrap;
    }
    .nav-tab i { font-size:1.1rem; }
    .nav-tab:hover { color:var(--primary); }
    .nav-tab.active { color:var(--primary); border-bottom-color:var(--primary); }
    .tab-badge {
      background:var(--secondary); color:#fff;
      font-size:.6rem; padding:.1rem .35rem; border-radius:8px; font-weight:700;
    }

    /* ─── LAYOUT ─── */
    .page { display:none; padding:1rem; max-width:900px; margin:0 auto; }
    .page.active { display:block; }

    /* ─── CARDS ─── */
    .card {
      background:#fff; border-radius:var(--rxl);
      box-shadow:var(--sh); margin-bottom:1rem;
      border:1px solid var(--gray-l);
    }
    .card-header {
      display:flex; align-items:center; justify-content:space-between;
      padding:.9rem 1.1rem; border-bottom:1px solid var(--gray-l);
    }
    .card-title { font-weight:700; font-size:.95rem; display:flex; align-items:center; gap:.5rem; }
    .card-body { padding:1rem 1.1rem; }

    /* ─── BUTTONS ─── */
    .btn {
      display:inline-flex; align-items:center; gap:.4rem;
      padding:.5rem 1rem; border:none; border-radius:var(--r);
      font-size:.85rem; font-weight:600; transition:all .2s;
    }
    .btn-primary { background:var(--primary); color:#fff; }
    .btn-primary:hover { background:var(--primary-d); }
    .btn-outline { background:transparent; border:2px solid var(--primary); color:var(--primary); }
    .btn-outline:hover { background:var(--primary); color:#fff; }
    .btn-success { background:var(--success); color:#fff; }
    .btn-danger { background:var(--danger); color:#fff; }
    .btn-warning { background:var(--warning); color:var(--dark); }
    .btn-ghost { background:transparent; border:1px solid var(--gray-l); color:var(--mid); }
    .btn-ghost:hover { background:var(--gray-l); }
    .btn-sm { padding:.3rem .7rem; font-size:.78rem; }
    .btn-full { width:100%; justify-content:center; }
    .btn:disabled { opacity:.5; cursor:not-allowed; }

    /* ─── FORM ─── */
    .form-group { margin-bottom:.85rem; }
    .form-group label { display:block; font-size:.8rem; font-weight:600; margin-bottom:.3rem; color:var(--mid); }
    .form-control {
      width:100%; padding:.6rem .85rem;
      border:2px solid var(--gray-l); border-radius:var(--r);
      font-size:.9rem; transition:border .2s;
    }
    .form-control:focus { outline:none; border-color:var(--primary); }
    textarea.form-control { resize:vertical; min-height:60px; }

    /* ─── STATUS PILLS ─── */
    .pill {
      display:inline-flex; align-items:center; gap:.3rem;
      padding:.2rem .6rem; border-radius:2rem; font-size:.72rem; font-weight:700;
    }
    .pill-green { background:rgba(72,187,120,.12); color:#276749; }
    .pill-yellow { background:rgba(236,201,75,.15); color:#7d6608; }
    .pill-red { background:rgba(245,101,101,.12); color:#9b2c2c; }
    .pill-blue { background:rgba(66,153,225,.12); color:#2b6cb0; }
    .pill-purple { background:rgba(102,126,234,.12); color:var(--primary-d); }

    /* ─────────────────────────────────────────
       PAGE 1 — MY STORE
    ───────────────────────────────────────── */
    .my-store-header {
      background:linear-gradient(135deg, var(--primary), #a78bfa);
      color:#fff; border-radius:var(--rxl); padding:1.5rem;
      margin-bottom:1rem; position:relative; overflow:hidden;
    }
    .my-store-header::before {
      content:'🐐'; position:absolute; right:-10px; top:-10px;
      font-size:6rem; opacity:.15; transform:rotate(-10deg);
    }
    .my-store-header h2 { font-size:1.4rem; margin-bottom:.2rem; }
    .my-store-header p { font-size:.85rem; opacity:.85; }
    .store-stats { display:grid; grid-template-columns:repeat(3,1fr); gap:.75rem; margin-bottom:1rem; }
    .stat-box {
      background:#fff; border-radius:var(--rl); padding:.85rem;
      text-align:center; box-shadow:var(--sh); border:1px solid var(--gray-l);
    }
    .stat-box .val { font-size:1.6rem; font-weight:800; color:var(--primary); line-height:1; }
    .stat-box .lbl { font-size:.7rem; color:var(--gray); margin-top:.2rem; }

    .inv-table { width:100%; border-collapse:collapse; font-size:.82rem; }
    .inv-table th { background:var(--light); padding:.5rem .6rem; text-align:left;
      font-size:.72rem; text-transform:uppercase; letter-spacing:.4px;
      border-bottom:2px solid var(--gray-l); color:var(--mid); }
    .inv-table td { padding:.55rem .6rem; border-bottom:1px solid var(--gray-l); vertical-align:middle; }
    .inv-table tr:hover td { background:rgba(102,126,234,.02); }
    .img-thumb { width:38px; height:38px; border-radius:6px; object-fit:cover; background:var(--gray-l); }

    /* Stock-in panel */
    .stock-in-panel {
      background:var(--primary-l); border-radius:var(--rl);
      padding:1rem; margin-bottom:1rem;
    }
    .stock-in-panel h4 { font-size:.9rem; margin-bottom:.75rem; color:var(--primary-d); display:flex; align-items:center; gap:.4rem; }
    .stock-in-row { display:grid; grid-template-columns:1fr auto auto; gap:.5rem; align-items:end; }

    /* ─────────────────────────────────────────
       PAGE 2 — MARKETPLACE
    ───────────────────────────────────────── */
    .mkt-search {
      display:flex; gap:.5rem; margin-bottom:.75rem;
    }
    .mkt-search input { flex:1; }
    .listing-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(240px,1fr)); gap:.85rem; }
    .listing-card {
      background:#fff; border-radius:var(--rxl);
      box-shadow:var(--sh); border:1px solid var(--gray-l);
      overflow:hidden; transition:transform .2s, box-shadow .2s;
    }
    .listing-card:hover { transform:translateY(-3px); box-shadow:var(--shd); }
    .listing-img {
      width:100%; height:160px; object-fit:cover; background:var(--gray-l);
      display:flex; align-items:center; justify-content:center; font-size:2.5rem; color:var(--gray);
    }
    .listing-body { padding:.75rem; }
    .listing-store { font-size:.7rem; color:var(--gray); margin-bottom:.2rem; display:flex; align-items:center; gap:.3rem; }
    .listing-name { font-weight:700; font-size:.9rem; margin-bottom:.2rem; }
    .listing-price { font-size:1.05rem; font-weight:800; color:var(--primary); }
    .listing-meta { display:flex; justify-content:space-between; align-items:center; margin:.4rem 0; }
    .listing-actions { display:flex; gap:.4rem; margin-top:.6rem; }

    /* Post listing form */
    .post-listing-card {
      background:linear-gradient(135deg,#fff,var(--primary-l));
      border:2px dashed var(--primary); border-radius:var(--rxl);
      padding:1.25rem; margin-bottom:1rem; text-align:center;
    }
    .post-listing-card h4 { color:var(--primary-d); margin-bottom:.3rem; }
    .post-listing-card p { font-size:.82rem; color:var(--gray); margin-bottom:.85rem; }

    /* ─────────────────────────────────────────
       PAGE 3 — ORDERS / TRANSFERS
    ───────────────────────────────────────── */
    .order-tabs { display:flex; gap:.5rem; margin-bottom:1rem; }
    .order-tab-btn {
      flex:1; padding:.5rem; border:2px solid var(--gray-l); border-radius:var(--r);
      background:#fff; font-size:.82rem; font-weight:600; color:var(--mid); transition:all .2s;
    }
    .order-tab-btn.active { border-color:var(--primary); color:var(--primary); background:var(--primary-l); }

    .transfer-card {
      border:1px solid var(--gray-l); border-radius:var(--rl);
      overflow:hidden; margin-bottom:.75rem;
    }
    .transfer-card-head {
      display:flex; align-items:center; justify-content:space-between;
      padding:.6rem .9rem; background:var(--light);
      border-bottom:1px solid var(--gray-l); flex-wrap:wrap; gap:.4rem;
    }
    .transfer-card-body { padding:.75rem .9rem; }
    .transfer-route {
      display:flex; align-items:center; gap:.5rem; font-size:.85rem;
      margin-bottom:.4rem;
    }
    .transfer-route .arrow { color:var(--primary); }
    .transfer-items { font-size:.8rem; color:var(--mid); }
    .transfer-actions { display:flex; gap:.4rem; margin-top:.6rem; flex-wrap:wrap; }

    /* ─────────────────────────────────────────
       PAGE 4 — CHAT
    ───────────────────────────────────────── */
    .chat-layout { display:grid; grid-template-columns:240px 1fr; gap:0; height:calc(100vh - 160px); min-height:400px; background:#fff; border-radius:var(--rxl); overflow:hidden; box-shadow:var(--shd); border:1px solid var(--gray-l); }
    @media(max-width:600px){ .chat-layout { grid-template-columns:1fr; } .chat-sidebar { display:none; } .chat-sidebar.show { display:flex; } }
    .chat-sidebar { display:flex; flex-direction:column; border-right:1px solid var(--gray-l); }
    .chat-sidebar-header { padding:.85rem 1rem; border-bottom:1px solid var(--gray-l); font-weight:700; font-size:.9rem; display:flex; align-items:center; justify-content:space-between; }
    .chat-list { flex:1; overflow-y:auto; }
    .chat-list-item {
      display:flex; align-items:center; gap:.6rem;
      padding:.65rem .9rem; cursor:pointer; border-bottom:1px solid var(--gray-l);
      transition:background .15s;
    }
    .chat-list-item:hover { background:var(--light); }
    .chat-list-item.active { background:var(--primary-l); }
    .chat-avatar {
      width:38px; height:38px; border-radius:50%;
      background:linear-gradient(135deg,var(--primary),#a78bfa);
      display:flex; align-items:center; justify-content:center;
      font-size:1rem; color:#fff; font-weight:700; flex-shrink:0;
    }
    .chat-avatar.online::after { content:''; position:absolute; width:10px; height:10px; border-radius:50%; background:var(--success); border:2px solid #fff; bottom:0; right:0; }
    .chat-list-info { flex:1; min-width:0; }
    .chat-list-name { font-size:.82rem; font-weight:600; }
    .chat-list-preview { font-size:.72rem; color:var(--gray); white-space:nowrap; overflow:hidden; text-overflow:ellipsis; }
    .chat-list-meta { display:flex; flex-direction:column; align-items:flex-end; gap:.2rem; }
    .chat-list-time { font-size:.65rem; color:var(--gray); }
    .unread-badge { background:var(--primary); color:#fff; font-size:.6rem; font-weight:700; padding:.1rem .35rem; border-radius:8px; }

    .chat-main { display:flex; flex-direction:column; }
    .chat-header {
      padding:.75rem 1rem; border-bottom:1px solid var(--gray-l);
      display:flex; align-items:center; gap:.7rem;
    }
    .chat-header-info { flex:1; }
    .chat-header-name { font-weight:700; font-size:.9rem; }
    .chat-header-status { font-size:.72rem; color:var(--success); }
    .chat-messages { flex:1; overflow-y:auto; padding:.9rem; display:flex; flex-direction:column; gap:.6rem; background:#fafbff; }
    .chat-empty-state { flex:1; display:flex; flex-direction:column; align-items:center; justify-content:center; color:var(--gray); }
    .chat-empty-state i { font-size:3rem; margin-bottom:.75rem; opacity:.4; }
    .msg-row { display:flex; gap:.5rem; align-items:flex-end; max-width:80%; }
    .msg-row.mine { align-self:flex-end; flex-direction:row-reverse; }
    .msg-avatar-sm { width:26px; height:26px; border-radius:50%; background:linear-gradient(135deg,var(--primary),#a78bfa); display:flex; align-items:center; justify-content:center; font-size:.7rem; color:#fff; font-weight:700; flex-shrink:0; }
    .msg-bubble {
      padding:.5rem .75rem; border-radius:14px 14px 14px 4px;
      background:#fff; box-shadow:0 1px 3px rgba(0,0,0,.07);
      font-size:.85rem; line-height:1.4; max-width:320px;
      border:1px solid var(--gray-l);
    }
    .msg-row.mine .msg-bubble { background:var(--primary); color:#fff; border-radius:14px 14px 4px 14px; border:none; }
    .msg-time { font-size:.62rem; color:var(--gray); margin-top:.2rem; }
    .msg-row.mine .msg-time { text-align:right; }
    .msg-special {
      background:rgba(102,126,234,.08); border:1px solid var(--primary-l);
      border-radius:var(--r); padding:.5rem .75rem; font-size:.78rem;
      margin:.3rem 0;
    }
    .msg-special .ms-title { font-weight:700; color:var(--primary-d); margin-bottom:.2rem; }
    .chat-input-area { padding:.75rem; border-top:1px solid var(--gray-l); display:flex; gap:.5rem; align-items:flex-end; }
    .chat-input-area textarea { flex:1; resize:none; min-height:40px; max-height:100px; padding:.5rem .75rem; border:2px solid var(--gray-l); border-radius:var(--r); font-size:.88rem; }
    .chat-input-area textarea:focus { outline:none; border-color:var(--primary); }
    .chat-input-actions { display:flex; flex-direction:column; gap:.3rem; }

    /* ─── MODALS ─── */
    .modal-overlay {
      position:fixed; inset:0; background:rgba(0,0,0,.45);
      display:flex; align-items:center; justify-content:center;
      z-index:999; padding:1rem;
      animation:fadeIn .2s ease;
    }
    @keyframes fadeIn { from{opacity:0} to{opacity:1} }
    .modal {
      background:#fff; border-radius:var(--rxl);
      width:100%; max-width:480px; max-height:90vh; overflow-y:auto;
      box-shadow:0 20px 60px rgba(0,0,0,.2);
      animation:slideUp .25s ease;
    }
    @keyframes slideUp { from{transform:translateY(20px);opacity:0} to{transform:translateY(0);opacity:1} }
    .modal-header { display:flex; align-items:center; justify-content:space-between; padding:1rem 1.25rem; border-bottom:1px solid var(--gray-l); }
    .modal-header h3 { font-size:1rem; }
    .modal-close { background:none; border:none; font-size:1.4rem; color:var(--gray); line-height:1; }
    .modal-body { padding:1.25rem; }
    .modal-footer { padding:.85rem 1.25rem; border-top:1px solid var(--gray-l); display:flex; gap:.5rem; justify-content:flex-end; }

    /* ─── TOASTS ─── */
    .toast-container { position:fixed; bottom:1.5rem; left:50%; transform:translateX(-50%); z-index:9999; display:flex; flex-direction:column; align-items:center; gap:.4rem; pointer-events:none; }
    .toast {
      background:var(--dark); color:#fff; padding:.6rem 1.1rem;
      border-radius:2rem; font-size:.83rem; box-shadow:var(--shd);
      animation:toastIn .3s ease;
    }
    @keyframes toastIn { from{transform:translateY(10px);opacity:0} to{transform:translateY(0);opacity:1} }
    .toast.success { background:var(--success); }
    .toast.error { background:var(--danger); }
    .toast.info { background:var(--info); }

    /* ─── SETUP SCREEN ─── */
    .setup-overlay {
      position:fixed; inset:0; background:linear-gradient(135deg,var(--primary),#a78bfa);
      display:flex; align-items:center; justify-content:center; z-index:9999; padding:1rem;
    }
    .setup-box {
      background:#fff; border-radius:var(--rxl); padding:2rem;
      width:100%; max-width:400px; box-shadow:0 20px 60px rgba(0,0,0,.25);
    }
    .setup-box .logo-big { font-size:2.5rem; text-align:center; margin-bottom:.25rem; }
    .setup-box h2 { text-align:center; margin-bottom:.25rem; font-size:1.3rem; }
    .setup-box .sub { text-align:center; color:var(--gray); font-size:.83rem; margin-bottom:1.5rem; }

    /* ─── MISC ─── */
    .divider { border:none; border-top:1px solid var(--gray-l); margin:.75rem 0; }
    .text-gray { color:var(--gray); }
    .text-sm { font-size:.82rem; }
    .fw-bold { font-weight:700; }
    .flex { display:flex; } .items-center { align-items:center; } .gap-2 { gap:.5rem; }
    .mt-1 { margin-top:.25rem; } .mt-2 { margin-top:.5rem; } .mt-3 { margin-top:.75rem; }
    .empty-state { text-align:center; padding:2.5rem 1rem; color:var(--gray); }
    .empty-state i { font-size:2.5rem; display:block; margin-bottom:.6rem; opacity:.4; }
    .empty-state p { font-size:.85rem; }
    .section-label { font-size:.72rem; font-weight:700; text-transform:uppercase; letter-spacing:.6px; color:var(--gray); margin-bottom:.5rem; }
    .price-tag { font-size:1.1rem; font-weight:800; color:var(--primary); }
    .store-tag { display:inline-flex; align-items:center; gap:.3rem; background:var(--primary-l); color:var(--primary-d); padding:.15rem .5rem; border-radius:2rem; font-size:.7rem; font-weight:600; }

    @media(max-width:500px) {
      .store-stats { grid-template-columns:repeat(2,1fr); }
      .listing-grid { grid-template-columns:1fr 1fr; }
      .chat-layout { height:calc(100vh - 130px); }
    }
  </style>
</head>
<body>

<!-- ═══════════════════════════════════════
     SETUP OVERLAY (shown if no Firebase / store config)
════════════════════════════════════════ -->
<div class="setup-overlay" id="setupOverlay">
  <div class="setup-box">
    <div class="logo-big">🐐</div>
    <h2>GOAT KIDS Network</h2>
    <p class="sub">Set up your store to join the network</p>
    <div class="form-group">
      <label>Your Store Name</label>
      <input type="text" class="form-control" id="setupStoreName" placeholder="e.g. Goat Kids Phnom Penh">
    </div>
    <div class="form-group">
      <label>Your Name / Owner</label>
      <input type="text" class="form-control" id="setupOwner" placeholder="e.g. Sophorn">
    </div>
    <div class="form-group">
      <label>City / Area</label>
      <input type="text" class="form-control" id="setupCity" placeholder="e.g. Phnom Penh, Siem Reap…">
    </div>
    <div class="form-group">
      <label>Firebase Config JSON <small class="text-gray">(same as main app)</small></label>
      <textarea class="form-control" id="setupFirebase" rows="5" placeholder="Paste your firebaseConfig { ... } here"></textarea>
      <div id="setupFormatTip" class="hidden" style="background:#fff8e1;border:1px solid #ecc94b;border-radius:8px;padding:.75rem;font-size:.78rem;margin-top:.5rem;line-height:1.6">
        <strong>&#x1F4A1; Where to get it:</strong> Firebase Console &#x2192; &#x2699;&#xFE0F; Project Settings &#x2192; Your Apps &#x2192; Config<br><br>
        Paste the <strong>whole block</strong> including <code>{ }</code>. Both JS and JSON formats work.
      </div>
    </div>
    <button class="btn btn-primary btn-full" onclick="completeSetup()" style="margin-top:.75rem; padding:.75rem;">
      <i class="fas fa-rocket"></i> Join Network
    </button>
  </div>
</div>

<!-- ═══════════════════════════════════════
     TOPBAR
════════════════════════════════════════ -->
<div class="topbar">
  <div class="topbar-logo"><span class="goat">🐐</span> Network</div>
  <div class="store-pill" id="storePill">
    <div class="dot"></div>
    <span id="storeNameDisplay">My Store</span>
  </div>
  <div class="topbar-right">
    <div class="badge-wrap">
      <button class="icon-btn" onclick="showPage('chat')"><i class="fas fa-comment-dots"></i></button>
      <div class="notif-dot hidden" id="chatDot"></div>
    </div>
    <div class="badge-wrap">
      <button class="icon-btn" onclick="showPage('orders')"><i class="fas fa-inbox"></i></button>
      <div class="notif-dot hidden" id="orderDot"></div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════
     NAV TABS
════════════════════════════════════════ -->
<div class="nav-tabs">
  <button class="nav-tab active" onclick="showPage('mystore')" id="tab-mystore">
    <i class="fas fa-store"></i> My Store
  </button>
  <button class="nav-tab" onclick="showPage('marketplace')" id="tab-marketplace">
    <i class="fas fa-globe"></i> Market
  </button>
  <button class="nav-tab" onclick="showPage('orders')" id="tab-orders">
    <i class="fas fa-exchange-alt"></i> Transfers
    <span class="tab-badge hidden" id="orders-badge">0</span>
  </button>
  <button class="nav-tab" onclick="showPage('chat')" id="tab-chat">
    <i class="fas fa-comments"></i> Chat
    <span class="tab-badge hidden" id="chat-badge">0</span>
  </button>
</div>

<!-- ═══════════════════════════════════════
     PAGE: MY STORE
════════════════════════════════════════ -->
<div class="page active" id="page-mystore">
  <div class="my-store-header">
    <h2 id="msHeaderName">My Store</h2>
    <p id="msHeaderCity"><i class="fas fa-map-marker-alt"></i> —</p>
  </div>

  <div class="store-stats">
    <div class="stat-box"><div class="val" id="statItems">0</div><div class="lbl">Products</div></div>
    <div class="stat-box"><div class="val" id="statListings">0</div><div class="lbl">Listed</div></div>
    <div class="stat-box"><div class="val" id="statTransfers">0</div><div class="lbl">Transfers</div></div>
  </div>

  <!-- Stock In Panel -->
  <div class="stock-in-panel">
    <h4><i class="fas fa-box-open"></i> Stock In — Add Inventory</h4>
    <div class="form-group">
      <label>Product Name</label>
      <input type="text" class="form-control" id="siName" placeholder="e.g. Baby Floral Dress">
    </div>
    <div class="stock-in-row">
      <div class="form-group" style="margin:0">
        <label>Category</label>
        <select class="form-control" id="siCat">
          <option value="Dresses">Dresses</option>
          <option value="Girl Clothes">Girl Clothes</option>
          <option value="Boy Clothes">Boy Clothes</option>
          <option value="Baby Shirt">Baby Shirts</option>
          <option value="Baby Shoes">Baby Shoes</option>
          <option value="Baby Accessories">Accessories</option>
        </select>
      </div>
      <div class="form-group" style="margin:0">
        <label>Buy Price $</label>
        <input type="number" class="form-control" id="siCost" placeholder="0.00" min="0" step="0.01">
      </div>
      <div class="form-group" style="margin:0">
        <label>Sell Price $</label>
        <input type="number" class="form-control" id="siPrice" placeholder="0.00" min="0" step="0.01">
      </div>
    </div>
    <div class="stock-in-row" style="grid-template-columns:1fr 80px auto">
      <div class="form-group" style="margin:0">
        <label>Size</label>
        <input type="text" class="form-control" id="siSize" placeholder="e.g. 3-6M, S, 2T">
      </div>
      <div class="form-group" style="margin:0">
        <label>Qty</label>
        <input type="number" class="form-control" id="siQty" value="1" min="1">
      </div>
      <div style="padding-top:1.4rem">
        <button class="btn btn-primary" onclick="stockIn()"><i class="fas fa-plus"></i> Add</button>
      </div>
    </div>
    <div id="siSizeTags" style="display:flex;flex-wrap:wrap;gap:.35rem;margin-top:.3rem"></div>
    <div style="display:flex;gap:.5rem;margin-top:.75rem">
      <button class="btn btn-success btn-full" onclick="saveInventoryItem()"><i class="fas fa-save"></i> Save to Inventory</button>
    </div>
  </div>

  <!-- Inventory Table -->
  <div class="card">
    <div class="card-header">
      <div class="card-title"><i class="fas fa-boxes" style="color:var(--primary)"></i> Inventory</div>
      <div style="display:flex;gap:.4rem">
        <input type="text" class="form-control" id="invSearch" placeholder="Search…" style="width:140px;padding:.35rem .6rem;font-size:.8rem" oninput="renderInventory()">
      </div>
    </div>
    <div style="overflow-x:auto">
      <table class="inv-table" id="invTable">
        <thead>
          <tr>
            <th>#</th><th>Item</th><th>Cat</th><th>Sizes / Qty</th>
            <th>Cost</th><th>Price</th><th>Action</th>
          </tr>
        </thead>
        <tbody id="invBody"></tbody>
      </table>
    </div>
    <div class="empty-state hidden" id="invEmpty">
      <i class="fas fa-boxes"></i><p>No inventory yet. Add items above.</p>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE: MARKETPLACE
════════════════════════════════════════ -->
<div class="page" id="page-marketplace">

  <!-- Post Your Listing CTA -->
  <div class="post-listing-card">
    <h4><i class="fas fa-tag"></i> List Your Old Stock</h4>
    <p>Post unsold or excess inventory so other stores in the network can buy it.</p>
    <button class="btn btn-primary" onclick="openPostModal()">
      <i class="fas fa-plus-circle"></i> Post a Listing
    </button>
  </div>

  <!-- Filter Row -->
  <div class="mkt-search">
    <input type="text" class="form-control" id="mktSearch" placeholder="Search listings…" oninput="renderMarket()">
    <select class="form-control" id="mktCatFilter" style="width:150px" onchange="renderMarket()">
      <option value="">All Categories</option>
      <option value="Dresses">Dresses</option>
      <option value="Girl Clothes">Girl Clothes</option>
      <option value="Boy Clothes">Boy Clothes</option>
      <option value="Baby Shirt">Baby Shirts</option>
      <option value="Baby Shoes">Baby Shoes</option>
      <option value="Baby Accessories">Accessories</option>
    </select>
  </div>
  <p class="section-label">Network Listings</p>
  <div class="listing-grid" id="listingGrid"></div>
  <div class="empty-state hidden" id="mktEmpty">
    <i class="fas fa-store-slash"></i>
    <p>No listings yet. Be the first to post!</p>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE: ORDERS / TRANSFERS
════════════════════════════════════════ -->
<div class="page" id="page-orders">
  <div class="order-tabs">
    <button class="order-tab-btn active" onclick="switchOrderTab('incoming')" id="otab-incoming">
      <i class="fas fa-arrow-down"></i> Incoming
    </button>
    <button class="order-tab-btn" onclick="switchOrderTab('outgoing')" id="otab-outgoing">
      <i class="fas fa-arrow-up"></i> Outgoing
    </button>
    <button class="order-tab-btn" onclick="switchOrderTab('history')" id="otab-history">
      <i class="fas fa-history"></i> History
    </button>
  </div>
  <div id="transferList"></div>
  <div class="empty-state hidden" id="transferEmpty">
    <i class="fas fa-exchange-alt"></i>
    <p>No transfer requests here yet.</p>
  </div>
</div>

<!-- ═══════════════════════════════════════
     PAGE: CHAT
════════════════════════════════════════ -->
<div class="page" id="page-chat" style="padding:0;max-width:900px;">
  <div class="chat-layout">
    <!-- Sidebar -->
    <div class="chat-sidebar" id="chatSidebar">
      <div class="chat-sidebar-header">
        <span>Messages</span>
        <button class="btn btn-sm btn-ghost" onclick="openNewChatModal()"><i class="fas fa-edit"></i></button>
      </div>
      <div class="chat-list" id="chatList">
        <div class="empty-state" style="padding:2rem .5rem"><i class="fas fa-comment-slash"></i><p>No chats yet</p></div>
      </div>
    </div>
    <!-- Main chat -->
    <div class="chat-main" id="chatMain">
      <div class="chat-empty-state" id="chatEmptyState">
        <i class="fas fa-comments"></i>
        <p>Select a conversation to start chatting</p>
        <button class="btn btn-primary mt-2" onclick="openNewChatModal()"><i class="fas fa-edit"></i> New Message</button>
      </div>
      <div class="hidden" id="chatWindow" style="display:flex;flex-direction:column;height:100%;">
        <div class="chat-header">
          <div class="chat-avatar" id="chatWinAvatar">?</div>
          <div class="chat-header-info">
            <div class="chat-header-name" id="chatWinName">—</div>
            <div class="chat-header-status" id="chatWinStatus">online</div>
          </div>
          <button class="btn btn-sm btn-ghost" onclick="openTransferFromChat()"><i class="fas fa-exchange-alt"></i> Request Transfer</button>
        </div>
        <div class="chat-messages" id="chatMessages"></div>
        <div class="chat-input-area">
          <textarea id="chatInput" placeholder="Type a message…" rows="1"
            onkeydown="if(event.key==='Enter'&&!event.shiftKey){event.preventDefault();sendMessage()}"
            oninput="autoResizeTextarea(this)"></textarea>
          <div class="chat-input-actions">
            <button class="icon-btn" onclick="sendMessage()" style="background:var(--primary);color:#fff;border-radius:var(--r)">
              <i class="fas fa-paper-plane"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════
     MODAL: POST LISTING
════════════════════════════════════════ -->
<div class="modal-overlay hidden" id="postModal" onclick="if(event.target===this)closePostModal()">
  <div class="modal">
    <div class="modal-header">
      <h3><i class="fas fa-tag" style="color:var(--primary)"></i> Post Listing</h3>
      <button class="modal-close" onclick="closePostModal()">&times;</button>
    </div>
    <div class="modal-body">
      <div class="form-group">
        <label>Select from your inventory (or enter new)</label>
        <select class="form-control" id="postFromInv" onchange="fillPostFromInv()">
          <option value="">— New item —</option>
        </select>
      </div>
      <div class="form-group">
        <label>Item Name *</label>
        <input type="text" class="form-control" id="postName" placeholder="Product name">
      </div>
      <div class="form-group">
        <label>Category</label>
        <select class="form-control" id="postCat">
          <option value="Dresses">Dresses</option>
          <option value="Girl Clothes">Girl Clothes</option>
          <option value="Boy Clothes">Boy Clothes</option>
          <option value="Baby Shirt">Baby Shirts</option>
          <option value="Baby Shoes">Baby Shoes</option>
          <option value="Baby Accessories">Accessories</option>
        </select>
      </div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:.5rem">
        <div class="form-group">
          <label>Ask Price $</label>
          <input type="number" class="form-control" id="postPrice" placeholder="0.00" min="0" step="0.01">
        </div>
        <div class="form-group">
          <label>Total Qty</label>
          <input type="number" class="form-control" id="postQty" value="1" min="1">
        </div>
      </div>
      <div class="form-group">
        <label>Sizes Available</label>
        <input type="text" class="form-control" id="postSizes" placeholder="e.g. 3M, 6M, 1T, 2T">
      </div>
      <div class="form-group">
        <label>Description / Condition</label>
        <textarea class="form-control" id="postDesc" placeholder="Describe condition, reason for selling…"></textarea>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="closePostModal()">Cancel</button>
      <button class="btn btn-primary" onclick="submitListing()"><i class="fas fa-upload"></i> Post Listing</button>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════
     MODAL: BUY / REQUEST TRANSFER
════════════════════════════════════════ -->
<div class="modal-overlay hidden" id="buyModal" onclick="if(event.target===this)closeBuyModal()">
  <div class="modal">
    <div class="modal-header">
      <h3><i class="fas fa-exchange-alt" style="color:var(--success)"></i> Request Stock Transfer</h3>
      <button class="modal-close" onclick="closeBuyModal()">&times;</button>
    </div>
    <div class="modal-body">
      <div id="buyItemSummary" style="background:var(--primary-l);border-radius:var(--r);padding:.75rem;margin-bottom:1rem;font-size:.85rem"></div>
      <div class="form-group">
        <label>Quantity you want *</label>
        <input type="number" class="form-control" id="buyQty" value="1" min="1">
      </div>
      <div class="form-group">
        <label>Sizes needed</label>
        <input type="text" class="form-control" id="buySizes" placeholder="e.g. 1T, 2T, 3T">
      </div>
      <div class="form-group">
        <label>Offer Price $ (optional — or accept asking price)</label>
        <input type="number" class="form-control" id="buyOffer" placeholder="Leave blank to accept ask price" min="0" step="0.01">
      </div>
      <div class="form-group">
        <label>Note to seller</label>
        <textarea class="form-control" id="buyNote" placeholder="Any special requests…"></textarea>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="closeBuyModal()">Cancel</button>
      <button class="btn btn-success" onclick="submitTransferRequest()"><i class="fas fa-paper-plane"></i> Send Request</button>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════
     MODAL: NEW CHAT
════════════════════════════════════════ -->
<div class="modal-overlay hidden" id="newChatModal" onclick="if(event.target===this)closeNewChatModal()">
  <div class="modal">
    <div class="modal-header">
      <h3><i class="fas fa-edit" style="color:var(--primary)"></i> New Conversation</h3>
      <button class="modal-close" onclick="closeNewChatModal()">&times;</button>
    </div>
    <div class="modal-body">
      <p class="text-sm text-gray" style="margin-bottom:.75rem">Select a store to start chatting:</p>
      <div id="storePickerList"></div>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════
     MODAL: TRANSFER FROM CHAT
════════════════════════════════════════ -->
<div class="modal-overlay hidden" id="chatTransferModal" onclick="if(event.target===this)this.classList.add('hidden')">
  <div class="modal">
    <div class="modal-header">
      <h3><i class="fas fa-exchange-alt" style="color:var(--success)"></i> Request Transfer from Chat</h3>
      <button class="modal-close" onclick="document.getElementById('chatTransferModal').classList.add('hidden')">&times;</button>
    </div>
    <div class="modal-body">
      <div class="form-group">
        <label>Item name *</label>
        <input type="text" class="form-control" id="ctItem" placeholder="What do you need?">
      </div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:.5rem">
        <div class="form-group">
          <label>Qty</label>
          <input type="number" class="form-control" id="ctQty" value="1" min="1">
        </div>
        <div class="form-group">
          <label>Offer Price $</label>
          <input type="number" class="form-control" id="ctPrice" placeholder="0.00">
        </div>
      </div>
      <div class="form-group">
        <label>Note</label>
        <textarea class="form-control" id="ctNote" placeholder="Sizes, condition, urgency…"></textarea>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="document.getElementById('chatTransferModal').classList.add('hidden')">Cancel</button>
      <button class="btn btn-success" onclick="sendChatTransferRequest()"><i class="fas fa-paper-plane"></i> Send</button>
    </div>
  </div>
</div>

<!-- Toast container -->
<div class="toast-container" id="toastContainer"></div>

<!-- ═══════════════════════════════════════
     JAVASCRIPT
════════════════════════════════════════ -->
<script>
// ─── STATE ──────────────────────────────────────────────────────────────
let db = null;
let myStore = null; // { id, name, owner, city }
let myInventory = []; // local items
let siSizes = []; // temp sizes for stock-in
let listings = []; // marketplace
let transfers = []; // transfer requests
let conversations = []; // chat threads
let currentConvId = null;
let currentListingForBuy = null;
let messagesUnsub = null;
let listingsUnsub = null;
let transfersUnsub = null;
let convUnsub = null;
let currentOrderTab = 'incoming';

// ─── SETUP ──────────────────────────────────────────────────────────────
function loadMyStore() {
  const s = localStorage.getItem('goatNetworkStore');
  if (s) { myStore = JSON.parse(s); return true; }
  return false;
}

function completeSetup() {
  const name = document.getElementById('setupStoreName').value.trim();
  const owner = document.getElementById('setupOwner').value.trim();
  const city = document.getElementById('setupCity').value.trim();
  const fbRaw = document.getElementById('setupFirebase').value.trim();
  if (!name || !owner || !fbRaw) { showToast('Please fill all required fields','error'); return; }
  let cfg;
  try {
    // Extract just the { ... } block
    let cleaned = fbRaw;
    // Remove variable assignment: const firebaseConfig = { ... };
    cleaned = cleaned.replace(/^[\s\S]*?(const|var|let)\s+\w+\s*=\s*/,'');
    // Remove trailing semicolon or extra text after last }
    cleaned = cleaned.replace(/};?[\s\S]*$/,'}');
    // Extract from first { to last }
    const start = cleaned.indexOf('{');
    const end = cleaned.lastIndexOf('}');
    if (start === -1 || end === -1) throw new Error('No JSON object found');
    cleaned = cleaned.slice(start, end + 1);
    // Convert JS object keys (unquoted) to JSON keys (quoted)
    // e.g.  apiKey: "..."  →  "apiKey": "..."
    cleaned = cleaned.replace(/([{,]\s*)([a-zA-Z_]\w*)(\s*:)/g, '$1"$2"$3');
    // Remove trailing commas before } or ]
    cleaned = cleaned.replace(/,(\s*[}\]])/g, '$1');
    cfg = JSON.parse(cleaned);
  } catch(e) {
    showToast('Could not read Firebase config — see format tip below', 'error');
    document.getElementById('setupFormatTip').classList.remove('hidden');
    return;
  }
  if (!cfg.apiKey || !cfg.projectId) { showToast('Firebase config missing apiKey/projectId','error'); return; }

  // Store firebase config
  localStorage.setItem('goatB2BFirebaseConfig', JSON.stringify(cfg));
  myStore = { id: 'store_' + Date.now() + '_' + Math.random().toString(36).slice(2,7), name, owner, city };
  localStorage.setItem('goatNetworkStore', JSON.stringify(myStore));
  document.getElementById('setupOverlay').style.display = 'none';
  initFirebase(cfg);
  initUI();
}

function initFirebase(cfg) {
  try {
    if (!firebase.apps.length) firebase.initializeApp(cfg);
    db = firebase.firestore();
    db.settings({ cacheSizeBytes: firebase.firestore.CACHE_SIZE_UNLIMITED });
    db.enablePersistence().catch(()=>{});
    registerStore();
    subscribeListings();
    subscribeTransfers();
    subscribeConversations();
  } catch(e) { showToast('Firebase init error: ' + e.message, 'error'); }
}

async function registerStore() {
  if (!db || !myStore) return;
  try {
    await db.collection('network_stores').doc(myStore.id).set({
      id: myStore.id, name: myStore.name, owner: myStore.owner, city: myStore.city,
      lastSeen: firebase.firestore.FieldValue.serverTimestamp()
    }, { merge: true });
  } catch(e) {}
}

// ─── UI INIT ────────────────────────────────────────────────────────────
function initUI() {
  document.getElementById('storeNameDisplay').textContent = myStore.name;
  document.getElementById('msHeaderName').textContent = myStore.name;
  document.getElementById('msHeaderCity').innerHTML = `<i class="fas fa-map-marker-alt"></i> ${myStore.city}`;
  loadLocalInventory();
  renderInventory();
  updateStats();
}

// ─── PAGE NAV ────────────────────────────────────────────────────────────
function showPage(page) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-' + page).classList.add('active');
  document.getElementById('tab-' + page).classList.add('active');
  if (page === 'marketplace') renderMarket();
  if (page === 'orders') renderTransfers();
  if (page === 'chat') renderChatList();
}

// ─── MY STORE / INVENTORY ────────────────────────────────────────────────
function loadLocalInventory() {
  const raw = localStorage.getItem('goatNetworkInventory_' + (myStore?.id || 'default'));
  myInventory = raw ? JSON.parse(raw) : [];
}
function saveLocalInventory() {
  localStorage.setItem('goatNetworkInventory_' + (myStore?.id || 'default'), JSON.stringify(myInventory));
}

function stockIn() {
  const size = document.getElementById('siSize').value.trim();
  const qty = parseInt(document.getElementById('siQty').value) || 0;
  if (!size || qty <= 0) { showToast('Enter size and qty','error'); return; }
  const exists = siSizes.find(s => s.size === size);
  if (exists) { exists.qty += qty; } else { siSizes.push({ size, qty }); }
  document.getElementById('siSize').value = '';
  document.getElementById('siQty').value = '1';
  renderSiTags();
}

function renderSiTags() {
  const el = document.getElementById('siSizeTags');
  el.innerHTML = siSizes.map((s,i) =>
    `<span style="background:var(--primary-l);color:var(--primary-d);padding:.2rem .55rem;border-radius:2rem;font-size:.75rem;font-weight:600;display:inline-flex;align-items:center;gap:.3rem">
      ${s.size}: ${s.qty} <button onclick="removeSiTag(${i})" style="background:none;border:none;color:var(--danger);font-size:.9rem;line-height:1;padding:0;">&times;</button>
    </span>`).join('');
}

function removeSiTag(i) { siSizes.splice(i,1); renderSiTags(); }

function saveInventoryItem() {
  const name = document.getElementById('siName').value.trim();
  const cat = document.getElementById('siCat').value;
  const cost = parseFloat(document.getElementById('siCost').value) || 0;
  const price = parseFloat(document.getElementById('siPrice').value) || 0;
  if (!name) { showToast('Enter product name','error'); return; }
  if (siSizes.length === 0) { showToast('Add at least one size','error'); return; }
  const item = {
    id: 'item_' + Date.now(),
    name, cat, cost, price,
    sizes: [...siSizes],
    totalQty: siSizes.reduce((a,s)=>a+s.qty,0),
    createdAt: Date.now()
  };
  myInventory.unshift(item);
  saveLocalInventory();
  siSizes = [];
  renderSiTags();
  document.getElementById('siName').value = '';
  document.getElementById('siCost').value = '';
  document.getElementById('siPrice').value = '';
  renderInventory();
  updateStats();
  showToast('Item added to inventory ✓','success');
}

function renderInventory() {
  const q = (document.getElementById('invSearch')?.value || '').toLowerCase();
  const filtered = myInventory.filter(i => i.name.toLowerCase().includes(q) || i.cat.toLowerCase().includes(q));
  const tbody = document.getElementById('invBody');
  const empty = document.getElementById('invEmpty');
  if (!filtered.length) { tbody.innerHTML=''; empty.classList.remove('hidden'); return; }
  empty.classList.add('hidden');
  tbody.innerHTML = filtered.map((item,idx) => `
    <tr>
      <td>${idx+1}</td>
      <td><div style="font-weight:600;font-size:.82rem">${escHtml(item.name)}</div></td>
      <td><span class="pill pill-purple">${escHtml(item.cat)}</span></td>
      <td>
        <div style="display:flex;flex-wrap:wrap;gap:.2rem">
          ${item.sizes.map(s=>`<span style="background:var(--light);border:1px solid var(--gray-l);padding:.1rem .4rem;border-radius:4px;font-size:.72rem">${s.size}:${s.qty}</span>`).join('')}
        </div>
      </td>
      <td class="text-sm">$${item.cost.toFixed(2)}</td>
      <td class="price-tag" style="font-size:.9rem">$${item.price.toFixed(2)}</td>
      <td>
        <div style="display:flex;gap:.3rem">
          <button class="btn btn-sm btn-primary" onclick="listFromInventory('${item.id}')"><i class="fas fa-upload"></i></button>
          <button class="btn btn-sm btn-danger" onclick="deleteInvItem('${item.id}')"><i class="fas fa-trash"></i></button>
        </div>
      </td>
    </tr>
  `).join('');
}

function deleteInvItem(id) {
  myInventory = myInventory.filter(i => i.id !== id);
  saveLocalInventory();
  renderInventory();
  updateStats();
  showToast('Item removed');
}

function updateStats() {
  document.getElementById('statItems').textContent = myInventory.length;
  document.getElementById('statListings').textContent = listings.filter(l => l.storeId === myStore?.id).length;
  document.getElementById('statTransfers').textContent = transfers.filter(t => t.sellerId === myStore?.id || t.buyerId === myStore?.id).length;
}

function listFromInventory(id) {
  const item = myInventory.find(i => i.id === id);
  if (!item) return;
  openPostModal();
  setTimeout(()=>{
    document.getElementById('postName').value = item.name;
    document.getElementById('postCat').value = item.cat;
    document.getElementById('postPrice').value = item.price;
    document.getElementById('postQty').value = item.totalQty;
    document.getElementById('postSizes').value = item.sizes.map(s=>s.size).join(', ');
    // store item id for reference
    document.getElementById('postModal').dataset.invId = item.id;
  }, 50);
  showPage('marketplace');
}

// ─── MARKETPLACE ────────────────────────────────────────────────────────
function subscribeListings() {
  if (!db) return;
  listingsUnsub = db.collection('network_listings')
    .where('status','==','active')
    .orderBy('createdAt','desc')
    .onSnapshot(snap => {
      listings = snap.docs.map(d => ({ id: d.id, ...d.data() }));
      renderMarket();
      updateStats();
    }, err => {
      // fallback: try without orderBy (missing index)
      db.collection('network_listings').where('status','==','active').onSnapshot(snap2 => {
        listings = snap2.docs.map(d=>({id:d.id,...d.data()})).sort((a,b)=>(b.createdAt||0)-(a.createdAt||0));
        renderMarket(); updateStats();
      }, ()=>{});
    });
}

function renderMarket() {
  const q = (document.getElementById('mktSearch')?.value || '').toLowerCase();
  const cat = document.getElementById('mktCatFilter')?.value || '';
  let filtered = listings.filter(l => {
    const matchQ = !q || l.name?.toLowerCase().includes(q) || l.storeName?.toLowerCase().includes(q);
    const matchC = !cat || l.cat === cat;
    return matchQ && matchC;
  });
  const grid = document.getElementById('listingGrid');
  const empty = document.getElementById('mktEmpty');
  if (!filtered.length) { grid.innerHTML=''; empty.classList.remove('hidden'); return; }
  empty.classList.add('hidden');
  grid.innerHTML = filtered.map(l => {
    const isMine = l.storeId === myStore?.id;
    return `
    <div class="listing-card">
      <div class="listing-img">${l.emoji || '👗'}</div>
      <div class="listing-body">
        <div class="listing-store"><i class="fas fa-store"></i> ${escHtml(l.storeName||'?')} · ${escHtml(l.city||'')}</div>
        <div class="listing-name">${escHtml(l.name||'')}</div>
        <div class="listing-meta">
          <span class="listing-price">$${parseFloat(l.price||0).toFixed(2)}</span>
          <span class="pill pill-green">${l.qty||0} pcs</span>
        </div>
        ${l.sizes ? `<div class="text-sm text-gray" style="margin-bottom:.3rem">Sizes: ${escHtml(l.sizes)}</div>` : ''}
        ${l.desc ? `<div class="text-sm text-gray" style="margin-bottom:.4rem">${escHtml(l.desc)}</div>` : ''}
        <div class="listing-actions">
          ${isMine
            ? `<button class="btn btn-sm btn-danger btn-full" onclick="removeListing('${l.id}')"><i class="fas fa-trash"></i> Remove</button>`
            : `<button class="btn btn-sm btn-success" style="flex:1" onclick="openBuyModal('${l.id}')"><i class="fas fa-exchange-alt"></i> Request</button>
               <button class="btn btn-sm btn-ghost" onclick="openChatWithStore('${l.storeId}','${escHtml(l.storeName||'Store')}')"><i class="fas fa-comment"></i></button>`
          }
        </div>
      </div>
    </div>`;
  }).join('');
}

function openPostModal() {
  document.getElementById('postModal').classList.remove('hidden');
  // Populate inventory dropdown
  const sel = document.getElementById('postFromInv');
  sel.innerHTML = '<option value="">— New item —</option>' +
    myInventory.map(i=>`<option value="${i.id}">${escHtml(i.name)} (${i.totalQty} pcs)</option>`).join('');
}
function closePostModal() { document.getElementById('postModal').classList.add('hidden'); }

function fillPostFromInv() {
  const id = document.getElementById('postFromInv').value;
  if (!id) return;
  const item = myInventory.find(i => i.id === id);
  if (!item) return;
  document.getElementById('postName').value = item.name;
  document.getElementById('postCat').value = item.cat;
  document.getElementById('postPrice').value = item.price;
  document.getElementById('postQty').value = item.totalQty;
  document.getElementById('postSizes').value = item.sizes.map(s=>s.size).join(', ');
}

const catEmoji = { 'Dresses':'👗','Girl Clothes':'👕','Boy Clothes':'👕','Baby Shirt':'👶','Baby Shoes':'👟','Baby Accessories':'🎀' };

async function submitListing() {
  if (!db) { showToast('Firebase not connected','error'); return; }
  const name = document.getElementById('postName').value.trim();
  const price = parseFloat(document.getElementById('postPrice').value) || 0;
  const qty = parseInt(document.getElementById('postQty').value) || 1;
  if (!name || !price) { showToast('Name and price required','error'); return; }
  const cat = document.getElementById('postCat').value;
  const listing = {
    name, price, qty, cat,
    sizes: document.getElementById('postSizes').value.trim(),
    desc: document.getElementById('postDesc').value.trim(),
    storeId: myStore.id,
    storeName: myStore.name,
    city: myStore.city,
    status: 'active',
    emoji: catEmoji[cat] || '📦',
    createdAt: firebase.firestore.FieldValue.serverTimestamp()
  };
  try {
    await db.collection('network_listings').add(listing);
    closePostModal();
    showToast('Listing posted to network ✓','success');
  } catch(e) { showToast('Error: ' + e.message,'error'); }
}

async function removeListing(id) {
  if (!db) return;
  try {
    await db.collection('network_listings').doc(id).update({ status: 'removed' });
    showToast('Listing removed');
  } catch(e) { showToast('Error','error'); }
}

// ─── TRANSFER / ORDERS ──────────────────────────────────────────────────
function subscribeTransfers() {
  if (!db || !myStore) return;
  // Subscribe as buyer
  db.collection('network_transfers')
    .where('buyerId','==', myStore.id)
    .onSnapshot(snap => {
      const buyerOnes = snap.docs.map(d=>({id:d.id,...d.data()}));
      // Merge with seller ones
      db.collection('network_transfers').where('sellerId','==', myStore.id).onSnapshot(snap2 => {
        const sellerOnes = snap2.docs.map(d=>({id:d.id,...d.data()}));
        const merged = [...buyerOnes];
        sellerOnes.forEach(s => { if (!merged.find(b=>b.id===s.id)) merged.push(s); });
        transfers = merged.sort((a,b)=>(b.createdAt?.seconds||0)-(a.createdAt?.seconds||0));
        renderTransfers();
        updateStats();
        const incoming = transfers.filter(t=>t.sellerId===myStore.id && t.status==='pending').length;
        setOrderBadge(incoming);
      });
    });
}

function setOrderBadge(n) {
  const badge = document.getElementById('orders-badge');
  const dot = document.getElementById('orderDot');
  if (n > 0) { badge.textContent=n; badge.classList.remove('hidden'); dot.classList.remove('hidden'); }
  else { badge.classList.add('hidden'); dot.classList.add('hidden'); }
}

function switchOrderTab(tab) {
  currentOrderTab = tab;
  document.querySelectorAll('.order-tab-btn').forEach(b=>b.classList.remove('active'));
  document.getElementById('otab-' + tab).classList.add('active');
  renderTransfers();
}

function renderTransfers() {
  const list = document.getElementById('transferList');
  const empty = document.getElementById('transferEmpty');
  let filtered;
  if (currentOrderTab === 'incoming') filtered = transfers.filter(t => t.sellerId === myStore?.id && t.status === 'pending');
  else if (currentOrderTab === 'outgoing') filtered = transfers.filter(t => t.buyerId === myStore?.id && t.status === 'pending');
  else filtered = transfers.filter(t => t.status !== 'pending');

  if (!filtered.length) { list.innerHTML=''; empty.classList.remove('hidden'); return; }
  empty.classList.add('hidden');
  list.innerHTML = filtered.map(t => {
    const isSeller = t.sellerId === myStore?.id;
    const statusPill = t.status === 'pending'
      ? `<span class="pill pill-yellow"><i class="fas fa-clock"></i> Pending</span>`
      : t.status === 'accepted'
      ? `<span class="pill pill-green"><i class="fas fa-check"></i> Accepted</span>`
      : `<span class="pill pill-red"><i class="fas fa-times"></i> Declined</span>`;
    return `
    <div class="transfer-card">
      <div class="transfer-card-head">
        <div>
          <div class="text-sm fw-bold">${escHtml(t.itemName||'')}</div>
          <div class="text-sm text-gray">#${t.id.slice(-6).toUpperCase()}</div>
        </div>
        ${statusPill}
      </div>
      <div class="transfer-card-body">
        <div class="transfer-route">
          <span class="store-tag"><i class="fas fa-store"></i> ${escHtml(t.buyerName||'Buyer')}</span>
          <span class="arrow"><i class="fas fa-long-arrow-alt-right"></i></span>
          <span class="store-tag"><i class="fas fa-store"></i> ${escHtml(t.sellerName||'Seller')}</span>
        </div>
        <div class="transfer-items">
          Qty: ${t.qty||1} · Sizes: ${escHtml(t.sizes||'—')} · 
          Offer: <strong>$${parseFloat(t.offerPrice||t.askPrice||0).toFixed(2)}</strong>
        </div>
        ${t.note ? `<div class="text-sm text-gray mt-1" style="font-style:italic">"${escHtml(t.note)}"</div>` : ''}
        <div class="transfer-actions">
          ${isSeller && t.status === 'pending' ? `
            <button class="btn btn-sm btn-success" onclick="respondTransfer('${t.id}','accepted')"><i class="fas fa-check"></i> Accept</button>
            <button class="btn btn-sm btn-danger" onclick="respondTransfer('${t.id}','declined')"><i class="fas fa-times"></i> Decline</button>
          ` : ''}
          <button class="btn btn-sm btn-ghost" onclick="openChatWithStore('${isSeller?t.buyerId:t.sellerId}','${escHtml(isSeller?t.buyerName||'Buyer':t.sellerName||'Seller')}')">
            <i class="fas fa-comment"></i> Chat
          </button>
        </div>
      </div>
    </div>`;
  }).join('');
}

function openBuyModal(listingId) {
  const l = listings.find(x=>x.id===listingId);
  if (!l) return;
  currentListingForBuy = l;
  document.getElementById('buyItemSummary').innerHTML = `
    <div style="font-weight:700;margin-bottom:.3rem">${escHtml(l.name)}</div>
    <div>From: <strong>${escHtml(l.storeName)}</strong> · ${escHtml(l.city||'')}</div>
    <div>Ask: <strong style="color:var(--primary)">$${parseFloat(l.price).toFixed(2)}</strong> · Qty available: ${l.qty}</div>
    ${l.sizes ? `<div>Sizes: ${escHtml(l.sizes)}</div>` : ''}
  `;
  document.getElementById('buyQty').value = 1;
  document.getElementById('buyOffer').value = '';
  document.getElementById('buySizes').value = l.sizes || '';
  document.getElementById('buyNote').value = '';
  document.getElementById('buyModal').classList.remove('hidden');
}
function closeBuyModal() { document.getElementById('buyModal').classList.add('hidden'); currentListingForBuy=null; }

async function submitTransferRequest() {
  if (!db || !currentListingForBuy) return;
  const l = currentListingForBuy;
  const qty = parseInt(document.getElementById('buyQty').value)||1;
  const offer = parseFloat(document.getElementById('buyOffer').value)||l.price;
  const sizes = document.getElementById('buySizes').value.trim();
  const note = document.getElementById('buyNote').value.trim();
  const req = {
    listingId: l.id,
    itemName: l.name,
    buyerId: myStore.id, buyerName: myStore.name,
    sellerId: l.storeId, sellerName: l.storeName,
    qty, sizes, askPrice: l.price, offerPrice: offer, note,
    status: 'pending',
    createdAt: firebase.firestore.FieldValue.serverTimestamp()
  };
  try {
    const ref = await db.collection('network_transfers').add(req);
    closeBuyModal();
    showToast('Transfer request sent ✓','success');
    // Also send a chat message notification
    sendSystemChatMessage(l.storeId, l.storeName,
      `📦 Transfer Request: **${l.name}** × ${qty} @ $${offer.toFixed(2)}\nRef: #${ref.id.slice(-6).toUpperCase()}`);
  } catch(e) { showToast('Error: '+e.message,'error'); }
}

async function respondTransfer(id, status) {
  if (!db) return;
  try {
    await db.collection('network_transfers').doc(id).update({ status, respondedAt: firebase.firestore.FieldValue.serverTimestamp() });
    const t = transfers.find(x=>x.id===id);
    if (t) {
      const msg = status==='accepted'
        ? `✅ Transfer accepted! Ready to ship **${t.itemName}** × ${t.qty}`
        : `❌ Transfer declined for **${t.itemName}**`;
      sendSystemChatMessage(t.buyerId, t.buyerName, msg);
    }
    showToast(status === 'accepted' ? 'Transfer accepted ✓' : 'Transfer declined', status==='accepted'?'success':'info');
  } catch(e) { showToast('Error','error'); }
}

// ─── CHAT ────────────────────────────────────────────────────────────────
function subscribeConversations() {
  if (!db || !myStore) return;
  convUnsub = db.collection('network_conversations')
    .where('participants','array-contains', myStore.id)
    .orderBy('lastMessageAt','desc')
    .onSnapshot(snap => {
      conversations = snap.docs.map(d=>({id:d.id,...d.data()}));
      renderChatList();
      const unread = conversations.filter(c => (c.unread||{})[myStore.id] > 0).length;
      setChatBadge(unread);
    }, () => {
      db.collection('network_conversations')
        .where('participants','array-contains', myStore.id)
        .onSnapshot(snap2=>{
          conversations = snap2.docs.map(d=>({id:d.id,...d.data()})).sort((a,b)=>(b.lastMessageAt?.seconds||0)-(a.lastMessageAt?.seconds||0));
          renderChatList();
        }, ()=>{});
    });
}

function setChatBadge(n) {
  const badge = document.getElementById('chat-badge');
  const dot = document.getElementById('chatDot');
  if (n>0) { badge.textContent=n; badge.classList.remove('hidden'); dot.classList.remove('hidden'); }
  else { badge.classList.add('hidden'); dot.classList.add('hidden'); }
}

function renderChatList() {
  const list = document.getElementById('chatList');
  if (!conversations.length) {
    list.innerHTML = '<div class="empty-state" style="padding:2rem .5rem"><i class="fas fa-comment-slash"></i><p>No chats yet</p></div>';
    return;
  }
  list.innerHTML = conversations.map(c => {
    const other = getOtherParticipant(c);
    const unread = (c.unread||{})[myStore.id] || 0;
    const initials = (other.name||'?').split(' ').map(w=>w[0]).join('').slice(0,2).toUpperCase();
    const active = c.id === currentConvId ? 'active' : '';
    return `
    <div class="chat-list-item ${active}" onclick="openConversation('${c.id}')">
      <div class="chat-avatar" style="position:relative">${initials}</div>
      <div class="chat-list-info">
        <div class="chat-list-name">${escHtml(other.name||'Store')}</div>
        <div class="chat-list-preview">${escHtml(c.lastMessage||'')}</div>
      </div>
      <div class="chat-list-meta">
        <div class="chat-list-time">${formatTime(c.lastMessageAt)}</div>
        ${unread > 0 ? `<span class="unread-badge">${unread}</span>` : ''}
      </div>
    </div>`;
  }).join('');
}

function getOtherParticipant(conv) {
  const otherId = (conv.participants||[]).find(p => p !== myStore.id);
  const names = conv.participantNames || {};
  return { id: otherId, name: names[otherId] || 'Store' };
}

function openConversation(convId) {
  currentConvId = convId;
  const conv = conversations.find(c=>c.id===convId);
  if (!conv) return;
  const other = getOtherParticipant(conv);
  const initials = (other.name||'?').split(' ').map(w=>w[0]).join('').slice(0,2).toUpperCase();

  document.getElementById('chatEmptyState').classList.add('hidden');
  const win = document.getElementById('chatWindow');
  win.classList.remove('hidden');
  win.style.display='flex';
  document.getElementById('chatWinAvatar').textContent = initials;
  document.getElementById('chatWinName').textContent = other.name;
  document.getElementById('chatWinStatus').textContent = 'active on network';
  renderChatList(); // update active state

  // Mark as read
  if (db && (conv.unread||{})[myStore.id] > 0) {
    db.collection('network_conversations').doc(convId).update({
      [`unread.${myStore.id}`]: 0
    }).catch(()=>{});
  }

  // Subscribe to messages
  if (messagesUnsub) messagesUnsub();
  messagesUnsub = db.collection('network_conversations').doc(convId)
    .collection('messages').orderBy('createdAt','asc')
    .onSnapshot(snap => {
      const msgs = snap.docs.map(d=>({id:d.id,...d.data()}));
      renderMessages(msgs);
    }, () => {
      db.collection('network_conversations').doc(convId).collection('messages')
        .onSnapshot(snap2=>{
          const msgs = snap2.docs.map(d=>({id:d.id,...d.data()})).sort((a,b)=>(a.createdAt?.seconds||0)-(b.createdAt?.seconds||0));
          renderMessages(msgs);
        }, ()=>{});
    });
}

function renderMessages(msgs) {
  const container = document.getElementById('chatMessages');
  if (!msgs.length) {
    container.innerHTML = '<div style="text-align:center;color:var(--gray);font-size:.82rem;padding:2rem">No messages yet. Say hello! 👋</div>';
    return;
  }
  container.innerHTML = msgs.map(m => {
    const mine = m.senderId === myStore.id;
    const initials = (m.senderName||'?').split(' ').map(w=>w[0]).join('').slice(0,2).toUpperCase();
    const isSpecial = m.type === 'transfer_request' || m.type === 'system';
    if (isSpecial) {
      return `<div class="msg-special">
        <div class="ms-title"><i class="fas fa-exchange-alt"></i> ${escHtml(m.senderName||'System')}</div>
        <div>${escHtml(m.text||'').replace(/\*\*(.*?)\*\*/g,'<strong>$1</strong>')}</div>
        <div style="font-size:.65rem;color:var(--gray);margin-top:.3rem">${formatTime(m.createdAt)}</div>
      </div>`;
    }
    return `
    <div class="msg-row ${mine?'mine':''}">
      ${!mine ? `<div class="msg-avatar-sm">${initials}</div>` : ''}
      <div>
        <div class="msg-bubble">${escHtml(m.text||'')}</div>
        <div class="msg-time">${formatTime(m.createdAt)}</div>
      </div>
    </div>`;
  }).join('');
  container.scrollTop = container.scrollHeight;
}

async function sendMessage() {
  if (!db || !currentConvId) return;
  const input = document.getElementById('chatInput');
  const text = input.value.trim();
  if (!text) return;
  input.value = '';
  autoResizeTextarea(input);
  try {
    await db.collection('network_conversations').doc(currentConvId)
      .collection('messages').add({
        text, senderId: myStore.id, senderName: myStore.name,
        type: 'text', createdAt: firebase.firestore.FieldValue.serverTimestamp()
      });
    const conv = conversations.find(c=>c.id===currentConvId);
    const otherId = (conv?.participants||[]).find(p=>p!==myStore.id);
    await db.collection('network_conversations').doc(currentConvId).update({
      lastMessage: text,
      lastMessageAt: firebase.firestore.FieldValue.serverTimestamp(),
      [`unread.${otherId}`]: firebase.firestore.FieldValue.increment(1)
    });
  } catch(e) { showToast('Send failed','error'); }
}

async function sendSystemChatMessage(otherStoreId, otherStoreName, text) {
  if (!db) return;
  const convId = [myStore.id, otherStoreId].sort().join('_');
  const convRef = db.collection('network_conversations').doc(convId);
  try {
    await convRef.set({
      participants: [myStore.id, otherStoreId],
      participantNames: { [myStore.id]: myStore.name, [otherStoreId]: otherStoreName },
      lastMessage: text, lastMessageAt: firebase.firestore.FieldValue.serverTimestamp(),
      [`unread.${otherStoreId}`]: firebase.firestore.FieldValue.increment(1)
    }, { merge: true });
    await convRef.collection('messages').add({
      text, senderId: myStore.id, senderName: myStore.name,
      type: 'transfer_request', createdAt: firebase.firestore.FieldValue.serverTimestamp()
    });
  } catch(e) {}
}

function openNewChatModal() {
  if (!db) { showToast('Connect Firebase first','error'); return; }
  document.getElementById('newChatModal').classList.remove('hidden');
  // Load all network stores
  db.collection('network_stores').get().then(snap => {
    const stores = snap.docs.map(d=>d.data()).filter(s=>s.id!==myStore.id);
    const list = document.getElementById('storePickerList');
    if (!stores.length) { list.innerHTML='<p class="text-gray text-sm">No other stores on the network yet.</p>'; return; }
    list.innerHTML = stores.map(s=>`
      <div style="display:flex;align-items:center;justify-content:space-between;padding:.65rem .5rem;border-bottom:1px solid var(--gray-l)">
        <div>
          <div style="font-weight:600;font-size:.88rem">${escHtml(s.name||'')}</div>
          <div class="text-sm text-gray"><i class="fas fa-map-marker-alt"></i> ${escHtml(s.city||'')}</div>
        </div>
        <button class="btn btn-sm btn-primary" onclick="openChatWithStore('${s.id}','${escHtml(s.name||'')}');closeNewChatModal()">
          <i class="fas fa-comment"></i> Chat
        </button>
      </div>
    `).join('');
  }).catch(()=>{ document.getElementById('storePickerList').innerHTML='<p class="text-gray text-sm">Could not load stores.</p>'; });
}
function closeNewChatModal() { document.getElementById('newChatModal').classList.add('hidden'); }

async function openChatWithStore(storeId, storeName) {
  if (!db) return;
  const convId = [myStore.id, storeId].sort().join('_');
  const convRef = db.collection('network_conversations').doc(convId);
  try {
    await convRef.set({
      participants: [myStore.id, storeId],
      participantNames: { [myStore.id]: myStore.name, [storeId]: storeName },
      lastMessageAt: firebase.firestore.FieldValue.serverTimestamp()
    }, { merge: true });
  } catch(e) {}
  showPage('chat');
  setTimeout(() => openConversation(convId), 400);
}

function openTransferFromChat() {
  if (!currentConvId) return;
  document.getElementById('ctItem').value = '';
  document.getElementById('ctQty').value = 1;
  document.getElementById('ctPrice').value = '';
  document.getElementById('ctNote').value = '';
  document.getElementById('chatTransferModal').classList.remove('hidden');
}

async function sendChatTransferRequest() {
  if (!db || !currentConvId) return;
  const item = document.getElementById('ctItem').value.trim();
  const qty = parseInt(document.getElementById('ctQty').value)||1;
  const price = parseFloat(document.getElementById('ctPrice').value)||0;
  const note = document.getElementById('ctNote').value.trim();
  if (!item) { showToast('Enter item name','error'); return; }
  const text = `📦 Transfer Request: **${item}** × ${qty}${price?` @ $${price.toFixed(2)}`:''}${note?`\n${note}`:''}`;
  document.getElementById('chatTransferModal').classList.add('hidden');
  const input = document.getElementById('chatInput');
  input.value = '';
  if (!currentConvId) return;
  try {
    await db.collection('network_conversations').doc(currentConvId).collection('messages').add({
      text, senderId: myStore.id, senderName: myStore.name,
      type: 'transfer_request', createdAt: firebase.firestore.FieldValue.serverTimestamp()
    });
    const conv = conversations.find(c=>c.id===currentConvId);
    const otherId = (conv?.participants||[]).find(p=>p!==myStore.id);
    await db.collection('network_conversations').doc(currentConvId).update({
      lastMessage: text, lastMessageAt: firebase.firestore.FieldValue.serverTimestamp(),
      [`unread.${otherId}`]: firebase.firestore.FieldValue.increment(1)
    });
  } catch(e) { showToast('Error','error'); }
}

// ─── HELPERS ────────────────────────────────────────────────────────────
function escHtml(str) {
  return String(str||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

function formatTime(ts) {
  if (!ts) return '';
  const d = ts.toDate ? ts.toDate() : new Date(ts);
  const now = new Date();
  const diff = now - d;
  if (diff < 60000) return 'just now';
  if (diff < 3600000) return Math.floor(diff/60000) + 'm ago';
  if (diff < 86400000) return d.getHours()+':'+String(d.getMinutes()).padStart(2,'0');
  return d.getDate()+'/'+(d.getMonth()+1);
}

function autoResizeTextarea(el) {
  el.style.height = 'auto';
  el.style.height = Math.min(el.scrollHeight, 100) + 'px';
}

function showToast(msg, type='') {
  const c = document.getElementById('toastContainer');
  const t = document.createElement('div');
  t.className = 'toast ' + type;
  t.textContent = msg;
  c.appendChild(t);
  setTimeout(()=>t.remove(), 3000);
}

// ─── BOOT ────────────────────────────────────────────────────────────────
window.addEventListener('load', function() {
  const hasStore = loadMyStore();
  const fbRaw = localStorage.getItem('goatB2BFirebaseConfig');

  if (hasStore && fbRaw) {
    try {
      const cfg = JSON.parse(fbRaw);
      document.getElementById('setupOverlay').style.display = 'none';
      initFirebase(cfg);
      initUI();
    } catch(e) {
      document.getElementById('setupOverlay').style.display = 'flex';
    }
  } else if (hasStore && !fbRaw) {
    // pre-fill store fields
    document.getElementById('setupStoreName').value = myStore.name;
    document.getElementById('setupOwner').value = myStore.owner;
    document.getElementById('setupCity').value = myStore.city;
  }
});
</script>
</body>
</html>

