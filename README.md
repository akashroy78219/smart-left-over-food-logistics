<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Smart Mess Flow – Leftover Rescue</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    :root {
      --bg: #020617;
      --card: rgba(15, 23, 42, 0.8);
      --card-soft: rgba(15, 23, 42, 0.6);
      --glass-border: rgba(148, 163, 184, 0.4);
      --accent: #22d3ee;
      --accent-soft: rgba(34, 211, 238, 0.18);
      --accent-strong: #0ea5e9;
      --text-main: #e5f3ff;
      --text-muted: #94a3b8;
      --success: #22c55e;
      --danger: #fb7185;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      min-height: 100vh;
      background: radial-gradient(circle at top, #0b1120 0, #020617 55%);
      color: var(--text-main);
      overflow-x: hidden;
    }

    /* Soft futuristic background blobs (lightweight) */
    .bg-blobs {
      position: fixed;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
      z-index: -1;
    }

    .blob {
      position: absolute;
      border-radius: 999px;
      filter: blur(70px);
      opacity: 0.35;
      animation: blobFloat 22s ease-in-out infinite;
    }

    .blob-1 {
      width: 420px;
      height: 420px;
      background: radial-gradient(circle, rgba(34, 211, 238, 0.45) 0, transparent 60%);
      top: -10%;
      left: -5%;
    }

    .blob-2 {
      width: 520px;
      height: 520px;
      background: radial-gradient(circle, rgba(59, 130, 246, 0.4) 0, transparent 60%);
      top: 40%;
      right: -15%;
      animation-delay: -7s;
    }

    .blob-3 {
      width: 380px;
      height: 380px;
      background: radial-gradient(circle, rgba(168, 85, 247, 0.4) 0, transparent 60%);
      bottom: -12%;
      left: 25%;
      animation-delay: -12s;
    }

    @keyframes blobFloat {
      0%, 100% {
        transform: translate(0, 0) scale(1);
      }
      40% {
        transform: translate(20px, -30px) scale(1.06);
      }
      70% {
        transform: translate(-20px, 15px) scale(0.97);
      }
    }

    .noise {
      position: fixed;
      inset: 0;
      background-image: radial-gradient(circle, rgba(148, 163, 184, 0.08) 1px, transparent 1px);
      background-size: 65px 65px;
      opacity: 0.25;
      mix-blend-mode: soft-light;
      z-index: -1;
    }

    .page {
      max-width: 1120px;
      margin: 0 auto;
      padding: 16px 16px 64px;
    }

    /* Glassy nav */
    .nav {
      position: sticky;
      top: 0;
      z-index: 20;
      backdrop-filter: blur(18px);
      background: linear-gradient(to bottom, rgba(15, 23, 42, 0.95), rgba(15, 23, 42, 0.78));
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.45);
      margin-bottom: 28px;
      box-shadow:
        0 18px 45px rgba(15, 23, 42, 0.98),
        0 0 18px rgba(15, 23, 42, 0.9);
    }

    .nav-inner {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px 16px;
      gap: 12px;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo-orb {
      width: 34px;
      height: 34px;
      border-radius: 999px;
      background: radial-gradient(circle at 30% 30%, #e0f2fe, #22d3ee 45%, #020617);
      position: relative;
      box-shadow:
        0 0 22px rgba(34, 211, 238, 0.8),
        0 0 0 1px rgba(15, 23, 42, 1);
    }

    .logo-orb::after {
      content: "";
      position: absolute;
      inset: 5px;
      border-radius: inherit;
      border: 1px solid rgba(248, 250, 252, 0.8);
    }

    .brand-text {
      display: flex;
      flex-direction: column;
      gap: 1px;
    }

    .brand-main {
      font-weight: 600;
      letter-spacing: 0.04em;
      text-transform: uppercase;
      font-size: 0.9rem;
    }

    .brand-sub {
      font-size: 0.75rem;
      color: var(--text-muted);
    }

    .nav-right {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .pill {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 12px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.85);
      border: 1px solid var(--glass-border);
      font-size: 0.8rem;
    }

    .pill span.value {
      font-weight: 700;
      color: var(--accent);
      font-size: 0.95rem;
    }

    .badge-pill {
      padding: 6px 14px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      background: linear-gradient(135deg, rgba(15, 23, 42, 0.9), rgba(15, 23, 42, 0.75));
      font-size: 0.8rem;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    /* Hero */
    .hero {
      text-align: center;
      padding: 72px 12px 32px;
      animation: heroFloat 7s ease-in-out infinite;
    }

    @keyframes heroFloat {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-8px); }
    }

    .hero-title {
      font-size: clamp(2.4rem, 5vw, 3.4rem);
      line-height: 1.1;
      margin-bottom: 12px;
    }

    .hero-title span {
      background: linear-gradient(120deg, #22d3ee, #60a5fa, #a855f7);
      background-clip: text;
      -webkit-background-clip: text;
      color: transparent;
    }

    .hero-sub {
      max-width: 560px;
      margin: 0 auto 20px;
      color: var(--text-muted);
      font-size: 0.98rem;
    }

    .btn-primary {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 11px 20px;
      border-radius: 999px;
      border: none;
      cursor: pointer;
      background: radial-gradient(circle at 0 0, #e0f2fe, #22d3ee 40%, #0f172a 100%);
      color: #020617;
      font-weight: 600;
      letter-spacing: 0.03em;
      text-transform: uppercase;
      font-size: 0.85rem;
      box-shadow:
        0 12px 32px rgba(15, 23, 42, 0.95),
        0 0 24px rgba(34, 211, 238, 0.85);
      transition: transform 0.18s ease, box-shadow 0.18s ease;
    }

    .btn-primary:hover {
      transform: translateY(-1px) scale(1.01);
      box-shadow:
        0 18px 42px rgba(15, 23, 42, 1),
        0 0 32px rgba(34, 211, 238, 0.95);
    }

    .btn-primary span.chevron {
      font-size: 1.1rem;
    }

    /* Cards & sections */
    .section {
      margin-top: 32px;
    }

    .section-title {
      font-size: 1.8rem;
      margin-bottom: 18px;
      text-align: center;
    }

    .section-sub {
      text-align: center;
      color: var(--text-muted);
      font-size: 0.95rem;
      margin-bottom: 22px;
    }

    .card {
      border-radius: 24px;
      background: var(--card-soft);
      border: 1px solid var(--glass-border);
      backdrop-filter: blur(16px);
      padding: 18px 18px 20px;
      box-shadow: 0 18px 42px rgba(15, 23, 42, 0.9);
    }

    .card-strong {
      background: var(--card);
    }

    .grid {
      display: grid;
      gap: 16px;
    }

    @media (min-width: 900px) {
      .grid-3 {
        grid-template-columns: repeat(3, minmax(0, 1fr));
      }
      .grid-2-1 {
        grid-template-columns: minmax(0, 2.1fr) minmax(0, 1fr);
      }
      .grid-4 {
        grid-template-columns: repeat(4, minmax(0, 1fr));
      }
    }

    .stat-big {
      text-align: center;
      padding: 18px 12px;
      border-radius: 22px;
      background: radial-gradient(circle at 0 0, var(--accent-soft), transparent 70%);
      border: 1px solid rgba(34, 211, 238, 0.4);
    }

    .stat-big-value {
      font-size: 2.4rem;
      font-weight: 700;
      margin-bottom: 4px;
    }

    .stat-big-label {
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 0.12em;
      color: var(--text-muted);
    }

    .how-card {
      border-radius: 20px;
      padding: 16px 14px 16px;
      background: rgba(15, 23, 42, 0.7);
      border: 1px solid rgba(51, 65, 85, 0.9);
      transition: transform 0.18s ease, border-color 0.18s ease;
    }

    .how-card:hover {
      transform: translateY(-4px);
      border-color: rgba(34, 211, 238, 0.7);
    }

    .how-step {
      font-size: 0.78rem;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 6px;
    }

    .how-title {
      font-size: 0.98rem;
      font-weight: 600;
      margin-bottom: 4px;
    }

    .how-text {
      font-size: 0.85rem;
      color: var(--text-muted);
    }

    /* Claim section */
    .claim-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
      margin-bottom: 14px;
    }

    .claim-title {
      font-size: 1.3rem;
      font-weight: 600;
    }

    .tag {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 5px 10px;
      border-radius: 999px;
      font-size: 0.74rem;
      text-transform: uppercase;
      letter-spacing: 0.12em;
      background: rgba(8, 47, 73, 0.9);
      border: 1px solid rgba(56, 189, 248, 0.5);
      color: var(--accent);
    }

    .claim-main {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      margin-bottom: 14px;
      flex-wrap: wrap;
    }

    .claim-left h4 {
      font-size: 1.1rem;
      margin-bottom: 4px;
    }

    .claim-meta {
      font-size: 0.8rem;
      color: var(--accent);
    }

    .claim-right {
      text-align: right;
    }

    .claim-plates {
      font-size: 3rem;
      font-weight: 700;
      background: linear-gradient(135deg, #22d3ee, #38bdf8);
      -webkit-background-clip: text;
      color: transparent;
      text-shadow: 0 0 16px rgba(34, 211, 238, 0.7);
    }

    .claim-plates-label {
      font-size: 0.82rem;
      color: var(--accent);
    }

    .timer-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px 12px;
      border-radius: 18px;
      background: rgba(15, 23, 42, 0.8);
      border: 1px solid rgba(51, 65, 85, 0.9);
      margin-bottom: 10px;
      font-size: 0.85rem;
    }

    .timer-value {
      font-size: 1.4rem;
      font-weight: 600;
    }

    .timer-value.open {
      color: var(--accent);
    }

    .timer-value.closed {
      color: var(--danger);
    }

    .status-box {
      margin-top: 8px;
      margin-bottom: 10px;
      padding: 10px 12px;
      border-radius: 16px;
      background: rgba(22, 163, 74, 0.12);
      border: 1px solid rgba(34, 197, 94, 0.7);
      text-align: center;
      font-size: 0.9rem;
      animation: statusPop 0.45s ease;
    }

    .status-box.error {
      background: rgba(248, 113, 113, 0.12);
      border-color: rgba(248, 113, 113, 0.8);
    }

    @keyframes statusPop {
      0% { transform: scale(0.9); opacity: 0; }
      50% { transform: scale(1.03); }
      100% { transform: scale(1); opacity: 1; }
    }

    .btn-wide {
      width: 100%;
      border-radius: 18px;
      padding: 11px 12px;
      font-weight: 600;
      border: none;
      cursor: pointer;
      font-size: 0.95rem;
      transition: opacity 0.15s ease, transform 0.15s ease, box-shadow 0.15s ease;
    }

    .btn-claim-enabled {
      background: linear-gradient(135deg, #22d3ee, #38bdf8);
      color: #020617;
      box-shadow: 0 10px 30px rgba(34, 211, 238, 0.4);
    }

    .btn-claim-enabled:hover {
      transform: translateY(-1px);
      box-shadow: 0 14px 38px rgba(34, 211, 238, 0.65);
    }

    .btn-disabled {
      background: rgba(30, 41, 59, 0.9);
      color: var(--text-muted);
      cursor: not-allowed;
      opacity: 0.6;
    }

    .btn-pickup {
      margin-top: 8px;
      background: linear-gradient(135deg, #22c55e, #4ade80);
      color: #022c22;
      box-shadow: 0 10px 28px rgba(34, 197, 94, 0.45);
    }

    .btn-pickup:hover {
      transform: translateY(-1px);
      box-shadow: 0 14px 40px rgba(34, 197, 94, 0.7);
    }

    .priority-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin: 8px 0 6px;
      font-size: 0.82rem;
      gap: 12px;
      flex-wrap: wrap;
    }

    .priority-row select {
      background: rgba(15, 23, 42, 0.9);
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      padding: 6px 10px;
      color: var(--text-main);
      font-size: 0.8rem;
      outline: none;
    }

    /* Leaderboard */
    .lb-item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 10px;
      padding: 8px 10px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.85);
      border: 1px solid rgba(51, 65, 85, 0.9);
      font-size: 0.86rem;
    }

    .lb-left {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .lb-rank {
      width: 28px;
      height: 28px;
      border-radius: 999px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.86rem;
      font-weight: 700;
      background: rgba(30, 64, 175, 0.9);
    }

    .lb-rank-1 {
      background: linear-gradient(135deg, #facc15, #eab308);
      color: #1e1b4b;
      box-shadow: 0 0 14px rgba(250, 204, 21, 0.7);
    }

    .lb-rank-2 {
      background: linear-gradient(135deg, #e5e7eb, #9ca3af);
      color: #111827;
    }

    .lb-rank-3 {
      background: linear-gradient(135deg, #fdba74, #fb923c);
      color: #1f2937;
    }

    .lb-meta {
      font-size: 0.72rem;
      color: var(--text-muted);
    }

    .lb-coins {
      font-weight: 600;
      color: var(--accent);
    }

    /* Impact grid */
    .impact-card {
      text-align: center;
      padding: 18px 12px;
      border-radius: 22px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(51, 65, 85, 0.9);
      box-shadow: 0 14px 34px rgba(15, 23, 42, 0.95);
    }

    .impact-value {
      font-size: 2.2rem;
      font-weight: 700;
      margin-bottom: 4px;
    }

    .impact-label {
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 0.14em;
      color: var(--text-muted);
    }

    .icon {
      font-size: 1.4rem;
      margin-bottom: 6px;
    }

    footer {
      margin-top: 40px;
      padding-top: 16px;
      border-top: 1px solid rgba(30, 41, 59, 0.9);
      text-align: center;
      font-size: 0.8rem;
      color: var(--text-muted);
    }

    footer .brand-footer {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      margin-bottom: 6px;
      font-size: 0.9rem;
      color: var(--text-main);
    }

    @media (max-width: 640px) {
      .nav {
        border-radius: 22px;
      }
      .nav-inner {
        flex-direction: column;
        align-items: flex-start;
        gap: 8px;
      }
    }
  </style>
</head>
<body>
  <!-- Background -->
  <div class="bg-blobs">
    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>
  </div>
  <div class="noise"></div>

  <div class="page">
    <!-- Nav -->
    <header class="nav">
      <div class="nav-inner">
        <div class="brand">
          <div class="logo-orb"></div>
          <div class="brand-text">
            <span class="brand-main">Smart Mess Flow</span>
            <span class="brand-sub">Campus leftover rescue network</span>
          </div>
        </div>
        <div class="nav-right">
          <div class="pill">
            <span class="value" id="nav-coins">0</span>
            <span>coins</span>
          </div>
          <div class="badge-pill" id="badge-pill">
            <span id="badge-icon">⭐</span>
            <span id="badge-name">Newcomer</span>
          </div>
        </div>
      </div>
    </header>

    <!-- Hero -->
    <section class="hero">
      <h1 class="hero-title">
        Save mess food<br />
        <span>before it becomes waste.</span>
      </h1>
      <p class="hero-sub">
        Smart Mess Flow turns surplus hostel/mess meals into rescued plates,
        using priority-based distribution, coins, badges and a zero-waste ladder
        for students and staff.
      </p>
      <button class="btn-primary" id="scroll-to-claim">
        Start rescuing food
        <span class="chevron">›</span>
      </button>
    </section>

    <!-- Today's impact -->
    <section class="section">
      <h2 class="section-title">Today's impact</h2>
      <p class="section-sub">Live snapshot of what your campus saved today</p>
      <div class="card card-strong">
        <div class="grid grid-3">
          <div class="stat-big">
            <div class="stat-big-value" id="today-meals">12</div>
            <div class="stat-big-label">Meals saved today</div>
          </div>
          <div class="stat-big">
            <div class="stat-big-value" id="today-co2">14.4</div>
            <div class="stat-big-label">kg CO₂ avoided</div>
          </div>
          <div class="stat-big">
            <div class="stat-big-value" id="today-water">6,000</div>
            <div class="stat-big-label">litres water preserved</div>
          </div>
        </div>
      </div>
    </section>

    <!-- How it works -->
    <section class="section">
      <h2 class="section-title">How it works</h2>
      <p class="section-sub">A simple, fair and fast flow inside your mess</p>
      <div class="grid grid-3">
        <div class="how-card">
          <div class="how-step">Step 1</div>
          <div class="how-title">Mess posts surplus</div>
          <p class="how-text">
            Staff log how many plates are left. The system calculates leftover
            using: prepared − students who ate.
          </p>
        </div>
        <div class="how-card">
          <div class="how-step">Step 2</div>
          <div class="how-title">Priority-based claim</div>
          <p class="how-text">
            Students who missed a meal or have high workload get first access,
            then it's open to everyone else.
          </p>
        </div>
        <div class="how-card">
          <div class="how-step">Step 3</div>
          <div class="how-title">Zero-waste closure</div>
          <p class="how-text">
            Remaining plates go to workers or compost, ensuring nothing hits
            the dustbin.
          </p>
        </div>
      </div>
    </section>

    <!-- Claim + leaderboard -->
    <section class="section" id="claim-section">
      <div class="grid grid-2-1">
        <!-- Claim card -->
        <div class="card card-strong">
          <div class="claim-header">
            <div class="claim-title">Live leftover window</div>
            <div class="tag">Dinner surplus • demo</div>
          </div>
          <div class="claim-main">
            <div class="claim-left">
              <h4>Hostel Mess A</h4>
              <div class="claim-meta">Posted 5 minutes ago • veg thali</div>
            </div>
            <div class="claim-right">
              <div class="claim-plates" id="leftover-plates">37</div>
              <div class="claim-plates-label">plates available</div>
            </div>
          </div>

          <div class="timer-row">
            <div>
              <div style="font-size: 0.8rem; text-transform: uppercase; letter-spacing: 0.12em; color: var(--text-muted);">
                Pickup window
              </div>
              <div style="font-size: 0.85rem;">Collect before this countdown ends</div>
            </div>
            <div class="timer-value open" id="timer-value">20s</div>
          </div>

          <div class="priority-row">
            <div style="font-size: 0.8rem; color: var(--text-muted);">
              Your priority for this slot:
            </div>
            <select id="priority-select">
              <option value="missed">Missed previous meal</option>
              <option value="sports">Sports / high workload</option>
              <option value="general">General (first-come-first-serve)</option>
            </select>
          </div>

          <div id="status-box" style="display: none;"></div>

          <button class="btn-wide btn-claim-enabled" id="claim-btn">
            Claim leftover plate
          </button>

          <button class="btn-wide btn-pickup" id="pickup-btn" style="display: none;">
            ✓ Confirm pickup (bonus +3 coins)
          </button>

          <div id="all-claimed-box" style="display:none; margin-top: 10px; font-size: 0.9rem; text-align: center; padding: 10px 12px; border-radius: 16px; background: rgba(21, 128, 61, 0.18); border: 1px solid rgba(34, 197, 94, 0.8);">
            ♻️ All plates claimed! Remaining food sent to workers / compost as per zero-waste ladder.
          </div>
        </div>

        <!-- Leaderboard -->
        <div class="card">
          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px;">
            <div>
              <div style="font-size: 1.2rem; font-weight: 600; display: flex; align-items:center; gap:6px;">
                <span>🏆</span> Top savers
              </div>
              <div style="font-size: 0.8rem; color: var(--text-muted);">
                Sorted by meals rescued
              </div>
            </div>
            <div style="font-size: 0.8rem; color: var(--accent);">
              Your coins: <span id="side-coins">0</span>
            </div>
          </div>

          <div id="leaderboard-list" class="grid" style="gap: 8px;">
            <!-- Filled by JS -->
          </div>
        </div>
      </div>
    </section>

    <!-- Campus impact -->
    <section class="section">
      <h2 class="section-title">Our campus impact so far</h2>
      <p class="section-sub">Cumulative numbers from Smart Mess Flow</p>
      <div class="grid grid-4">
        <div class="impact-card">
          <div class="icon">🍽️</div>
          <div class="impact-value" id="global-meals">1247</div>
          <div class="impact-label">Meals saved</div>
        </div>
        <div class="impact-card">
          <div class="icon">🌿</div>
          <div class="impact-value" id="global-co2">1496.4</div>
          <div class="impact-label">kg CO₂ avoided</div>
        </div>
        <div class="impact-card">
          <div class="icon">💧</div>
          <div class="impact-value" id="global-water">623,500</div>
          <div class="impact-label">Litres of water</div>
        </div>
        <div class="impact-card">
          <div class="icon">👥</div>
          <div class="impact-value" id="global-students">1247</div>
          <div class="impact-label">Students helped</div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer>
      <div class="brand-footer">
        <div style="width: 18px; height: 18px; border-radius: 999px; background: radial-gradient(circle, #e0f2fe, #22d3ee 55%, #020617);"></div>
        <span>Smart Mess Flow</span>
      </div>
      <div>Reducing waste, one plate at a time • Demo prototype</div>
    </footer>
  </div>

  <script>
    // --- State ---
    const TOTAL_PREPARED = 200;
    const TOTAL_ATE = 163;
    let leftoverPlates = TOTAL_PREPARED - TOTAL_ATE; // 37
    let timeLeft = 20; // seconds (demo)
    let claimEnabled = true;
    let hasClaimed = false;

    // Persistent values via localStorage
    const loadInt = (key, fallback) => {
      const v = localStorage.getItem(key);
      const n = parseInt(v, 10);
      return isNaN(n) ? fallback : n;
    };

    let coins = loadInt("smfCoins", 0);
    let mealsSaved = loadInt("smfMeals", 0);

    // Today's and global stats (dummy baseline)
    let todayStats = {
      meals: 12,
      co2: 14.4,
      water: 6000
    };

    let globalStats = {
      meals: 1247,
      co2: 1496.4,
      water: 623500,
      students: 1247
    };

    // Leaderboard dummy data
    let leaderboard = [
      { name: "Priya Sharma", meals: 23, coins: 130 },
      { name: "Rahul Kumar", meals: 19, coins: 110 },
      { name: "Ananya Singh", meals: 17, coins: 95 },
      { name: "Arjun Patel", meals: 15, coins: 85 },
      { name: "Neha Gupta", meals: 12, coins: 70 }
    ];

    // --- DOM refs ---
    const navCoinsEl = document.getElementById("nav-coins");
    const sideCoinsEl = document.getElementById("side-coins");
    const badgeIconEl = document.getElementById("badge-icon");
    const badgeNameEl = document.getElementById("badge-name");

    const leftoverEl = document.getElementById("leftover-plates");
    const timerEl = document.getElementById("timer-value");
    const claimBtn = document.getElementById("claim-btn");
    const pickupBtn = document.getElementById("pickup-btn");
    const statusBox = document.getElementById("status-box");
    const allClaimedBox = document.getElementById("all-claimed-box");
    const prioritySelect = document.getElementById("priority-select");

    const todayMealsEl = document.getElementById("today-meals");
    const todayCo2El = document.getElementById("today-co2");
    const todayWaterEl = document.getElementById("today-water");

    const globalMealsEl = document.getElementById("global-meals");
    const globalCo2El = document.getElementById("global-co2");
    const globalWaterEl = document.getElementById("global-water");
    const globalStudentsEl = document.getElementById("global-students");

    const leaderboardList = document.getElementById("leaderboard-list");
    const scrollBtn = document.getElementById("scroll-to-claim");

    // --- Helpers ---
    function saveState() {
      localStorage.setItem("smfCoins", String(coins));
      localStorage.setItem("smfMeals", String(mealsSaved));
    }

    function getBadge(coinCount) {
      if (coinCount >= 75) return { name: "Hunger Hero", icon: "🦸" };
      if (coinCount >= 50) return { name: "Eco Guardian", icon: "🌍" };
      if (coinCount >= 25) return { name: "Food Saver", icon: "🍽️" };
      return { name: "Newcomer", icon: "⭐" };
    }

    function updateBadgeAndCoins() {
      navCoinsEl.textContent = coins;
      sideCoinsEl.textContent = coins;
      const badge = getBadge(coins);
      badgeIconEl.textContent = badge.icon;
      badgeNameEl.textContent = badge.name;
    }

    function formatNumber(num) {
      return num.toLocaleString();
    }

    function updateStatsUI() {
      leftoverEl.textContent = leftoverPlates;

      todayMealsEl.textContent = todayStats.meals;
      todayCo2El.textContent = todayStats.co2.toFixed(1);
      todayWaterEl.textContent = formatNumber(todayStats.water);

      globalMealsEl.textContent = globalStats.meals;
      globalCo2El.textContent = globalStats.co2.toFixed(1);
      globalWaterEl.textContent = formatNumber(globalStats.water);
      globalStudentsEl.textContent = globalStats.students;
    }

    function renderLeaderboard() {
      // Ensure "You" exists in leaderboard
      const idx = leaderboard.findIndex(u => u.name === "You");
      if (idx === -1) {
        leaderboard.push({ name: "You", meals: mealsSaved, coins });
      } else {
        leaderboard[idx].meals = mealsSaved;
        leaderboard[idx].coins = coins;
      }
      leaderboard.sort((a, b) => b.meals - a.meals || b.coins - a.coins);

      leaderboardList.innerHTML = "";
      leaderboard.slice(0, 5).forEach((user, i) => {
        const item = document.createElement("div");
        item.className = "lb-item";

        const left = document.createElement("div");
        left.className = "lb-left";

        const rank = document.createElement("div");
        rank.className = "lb-rank";
        if (i === 0) rank.classList.add("lb-rank-1");
        else if (i === 1) rank.classList.add("lb-rank-2");
        else if (i === 2) rank.classList.add("lb-rank-3");
        rank.textContent = i + 1;

        const info = document.createElement("div");
        const nameEl = document.createElement("div");
        nameEl.textContent = user.name;
        nameEl.style.fontWeight = "600";
        const meta = document.createElement("div");
        meta.className = "lb-meta";
        meta.textContent = user.meals + " meals saved";
        info.appendChild(nameEl);
        info.appendChild(meta);

        left.appendChild(rank);
        left.appendChild(info);

        const right = document.createElement("div");
        right.className = "lb-coins";
        right.textContent = user.coins;

        item.appendChild(left);
        item.appendChild(right);

        leaderboardList.appendChild(item);
      });
    }

    function showStatus(message, type) {
      statusBox.style.display = "block";
      statusBox.textContent = message;
      statusBox.className = "status-box";
      if (type === "error") {
        statusBox.classList.add("error");
      }
    }

    function hideStatus() {
      statusBox.style.display = "none";
    }

    function updateClaimButtonState() {
      if (!claimEnabled || leftoverPlates <= 0) {
        claimBtn.classList.remove("btn-claim-enabled");
        claimBtn.classList.add("btn-disabled");
        claimBtn.disabled = true;
      } else if (hasClaimed) {
        claimBtn.classList.remove("btn-claim-enabled");
        claimBtn.classList.add("btn-disabled");
        claimBtn.disabled = true;
      } else {
        claimBtn.classList.remove("btn-disabled");
        claimBtn.classList.add("btn-claim-enabled");
        claimBtn.disabled = false;
      }
    }

    function updateTimerUI() {
      if (timeLeft > 0) {
        timerEl.textContent = timeLeft + "s";
        timerEl.className = "timer-value open";
      } else {
        timerEl.textContent = "Closed";
        timerEl.className = "timer-value closed";
      }
    }

    // --- Timer ---
    function startTimer() {
      updateTimerUI();
      const interval = setInterval(() => {
        if (!claimEnabled || timeLeft <= 0) {
          clearInterval(interval);
          claimEnabled = false;
          updateClaimButtonState();
          updateTimerUI();
          if (!hasClaimed) {
            showStatus("Pickup window closed. Unclaimed food will move to workers / compost.", "error");
          }
          return;
        }
        timeLeft -= 1;
        updateTimerUI();
      }, 1000);
    }

    // --- Events ---
    claimBtn.addEventListener("click", () => {
      if (!claimEnabled || hasClaimed || leftoverPlates <= 0) return;

      let priorityLabel = "";
      const value = prioritySelect.value;
      if (value === "missed") priorityLabel = "Missed meal (Priority 1)";
      else if (value === "sports") priorityLabel = "Sports / high workload (Priority 2)";
      else priorityLabel = "General (Priority 3)";

      hasClaimed = true;
      leftoverPlates = Math.max(0, leftoverPlates - 1);
      coins += 5;
      mealsSaved += 1;

      todayStats.meals += 1;
      todayStats.co2 += 1.2;
      todayStats.water += 500;

      globalStats.meals += 1;
      globalStats.co2 += 1.2;
      globalStats.water += 500;
      globalStats.students += 1;

      saveState();
      updateBadgeAndCoins();
      updateStatsUI();
      renderLeaderboard();
      updateClaimButtonState();
      hideStatus();
      showStatus("✓ Plate claimed! Priority: " + priorityLabel + " • +5 coins", "success");
      pickupBtn.style.display = "block";

      if (leftoverPlates <= 0) {
        allClaimedBox.style.display = "block";
      }
    });

    pickupBtn.addEventListener("click", () => {
      if (!hasClaimed) return;
      coins += 3;
      saveState();
      updateBadgeAndCoins();
      renderLeaderboard();
      pickupBtn.style.display = "none";
      showStatus("✓ Pickup confirmed on time! +3 bonus coins", "success");
    });

    scrollBtn.addEventListener("click", () => {
      const sec = document.getElementById("claim-section");
      if (sec) sec.scrollIntoView({ behavior: "smooth" });
    });

    // --- Init ---
    function init() {
      leftoverPlates = TOTAL_PREPARED - TOTAL_ATE;
      updateStatsUI();
      updateBadgeAndCoins();
      renderLeaderboard();
      updateClaimButtonState();
      hideStatus();
      startTimer();
    }

    init();
  </script>
</body>
</html>
