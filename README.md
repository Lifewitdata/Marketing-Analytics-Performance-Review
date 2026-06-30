# Marketing-Analytics-Performance-Review

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ascendeum Analytics — Pipeline Overview</title>
<style>
  :root{
    --bg: #0a0e14;
    --bg-panel: #11151d;
    --bg-panel-2: #161b25;
    --line: #232b38;
    --line-bright: #2e3848;
    --ink: #e8edf4;
    --ink-dim: #8b97aa;
    --ink-faint: #5a6577;
    --signal-blue: #4d8dff;
    --signal-green: #3ecf8e;
    --signal-amber: #f0a93e;
    --signal-red: #ef6464;
    --mono: "SF Mono", "JetBrains Mono", "Roboto Mono", Consolas, monospace;
    --sans: -apple-system, "Inter", "Segoe UI", sans-serif;
  }

  *{ box-sizing: border-box; margin:0; padding:0; }
  html{ scroll-behavior: smooth; }

  body{
    background: var(--bg);
    color: var(--ink);
    font-family: var(--sans);
    line-height: 1.55;
    overflow-x: hidden;
  }

  @media (prefers-reduced-motion: reduce){
    *{ animation-duration: 0.01ms !important; animation-iteration-count: 1 !important; transition-duration: 0.01ms !important; scroll-behavior: auto !important; }
  }

  /* ───────────────────────── Background grid + scanline ───────────────────────── */
  .bg-grid{
    position: fixed; inset:0; z-index:0; pointer-events:none;
    background-image:
      linear-gradient(var(--line) 1px, transparent 1px),
      linear-gradient(90deg, var(--line) 1px, transparent 1px);
    background-size: 64px 64px;
    opacity: 0.35;
    mask-image: radial-gradient(ellipse 80% 60% at 50% 0%, black 30%, transparent 75%);
  }

  .scanline{
    position: fixed; left:0; right:0; height: 2px; z-index: 1; pointer-events:none;
    background: linear-gradient(90deg, transparent, rgba(77,141,255,0.5), transparent);
    animation: scan 7s linear infinite;
    opacity: 0.5;
  }
  @keyframes scan{
    0%{ top: -2%; }
    100%{ top: 102%; }
  }

  .wrap{ position:relative; z-index:2; max-width: 1080px; margin: 0 auto; padding: 0 28px; }

  /* ───────────────────────── Top nav ───────────────────────── */
  nav{
    position: sticky; top:0; z-index: 50;
    backdrop-filter: blur(10px);
    background: rgba(10,14,20,0.78);
    border-bottom: 1px solid var(--line);
  }
  nav .wrap{ display:flex; align-items:center; justify-content:space-between; height: 56px; }
  .nav-brand{ font-family: var(--mono); font-size: 13px; color: var(--ink-dim); letter-spacing: 0.5px; }
  .nav-brand b{ color: var(--ink); }
  .nav-links{ display:flex; gap: 24px; list-style:none; }
  .nav-links a{ color: var(--ink-dim); text-decoration:none; font-size: 13px; font-family: var(--mono); transition: color .2s; }
  .nav-links a:hover{ color: var(--signal-blue); }
  .status-dot{ width:6px; height:6px; border-radius:50%; background: var(--signal-green); display:inline-block; margin-right:6px; box-shadow: 0 0 8px var(--signal-green); animation: pulse 2s ease-in-out infinite; }
  @keyframes pulse{ 0%,100%{ opacity:1; } 50%{ opacity:0.4; } }

  /* ───────────────────────── Hero ───────────────────────── */
  .hero{ padding: 88px 0 64px; position:relative; }

  .eyebrow{
    font-family: var(--mono); font-size: 12px; color: var(--signal-blue);
    text-transform: uppercase; letter-spacing: 2px; margin-bottom: 18px;
    display:flex; align-items:center; gap:10px;
    opacity:0; animation: rise 0.6s ease forwards 0.1s;
  }
  .eyebrow::before{ content:""; width:18px; height:1px; background: var(--signal-blue); }

  h1{
    font-size: clamp(34px, 5.4vw, 56px);
    font-weight: 700;
    letter-spacing: -0.02em;
    line-height: 1.08;
    max-width: 820px;
    opacity:0; animation: rise 0.7s ease forwards 0.22s;
  }
  h1 .strike{ color: var(--ink-faint); text-decoration: line-through; text-decoration-color: var(--signal-red); text-decoration-thickness: 2px; }
  h1 .arrow{ color: var(--signal-green); }

  .hero-sub{
    margin-top: 22px; max-width: 620px; color: var(--ink-dim); font-size: 16px;
    opacity:0; animation: rise 0.7s ease forwards 0.36s;
  }
  .hero-sub b{ color: var(--ink); font-weight: 600; }

  @keyframes rise{ from{ opacity:0; transform: translateY(14px);} to{ opacity:1; transform: translateY(0);} }

  .hero-cta{ margin-top: 32px; display:flex; gap:14px; flex-wrap:wrap; opacity:0; animation: rise 0.7s ease forwards 0.48s; }
  .btn{
    font-family: var(--mono); font-size: 13px; padding: 11px 20px; border-radius: 6px;
    text-decoration:none; display:inline-flex; align-items:center; gap:8px;
    border: 1px solid var(--line-bright); transition: all .2s;
  }
  .btn-primary{ background: var(--signal-blue); color: #04101f; border-color: var(--signal-blue); font-weight:600; }
  .btn-primary:hover{ background:#6aa1ff; transform: translateY(-1px); }
  .btn-ghost{ color: var(--ink-dim); }
  .btn-ghost:hover{ color: var(--ink); border-color: var(--ink-dim); }

  /* ───────────────────────── KPI counters ───────────────────────── */
  .kpi-grid{
    margin-top: 56px;
    display:grid; grid-template-columns: repeat(4, 1fr); gap:1px;
    background: var(--line); border: 1px solid var(--line); border-radius: 10px; overflow:hidden;
    opacity:0; animation: rise 0.7s ease forwards 0.6s;
  }
  @media (max-width: 720px){ .kpi-grid{ grid-template-columns: repeat(2,1fr); } }

  .kpi-cell{ background: var(--bg-panel); padding: 24px 20px; position:relative; }
  .kpi-label{ font-family: var(--mono); font-size: 11px; color: var(--ink-faint); text-transform:uppercase; letter-spacing: 1px; margin-bottom:8px; }
  .kpi-value{ font-size: 30px; font-weight:700; font-family: var(--mono); color: var(--ink); }
  .kpi-cell.positive .kpi-value{ color: var(--signal-green); }
  .kpi-tag{ position:absolute; top:18px; right:18px; font-size:10px; font-family:var(--mono); color: var(--ink-faint); }

  /* ───────────────────────── Section shell ───────────────────────── */
  section{ padding: 72px 0; border-top: 1px solid var(--line); }
  .section-head{ margin-bottom: 38px; }
  .section-num{ font-family: var(--mono); font-size: 12px; color: var(--signal-blue); letter-spacing: 1.5px; margin-bottom: 10px; }
  .section-title{ font-size: 26px; font-weight: 700; letter-spacing: -0.01em; }
  .section-desc{ color: var(--ink-dim); margin-top: 10px; max-width: 600px; font-size: 14.5px; }

  .reveal{ opacity:0; transform: translateY(20px); transition: opacity .7s ease, transform .7s ease; }
  .reveal.in{ opacity:1; transform: translateY(0); }

  /* ───────────────────────── Pipeline flow ───────────────────────── */
  .pipeline{
    display:flex; align-items:stretch; gap:0; overflow-x:auto; padding: 8px 0 16px;
    scrollbar-width: thin;
  }
  .pipe-stage{
    flex: 0 0 200px; background: var(--bg-panel); border: 1px solid var(--line); border-radius: 8px;
    padding: 18px; position:relative; margin-right: 36px;
  }
  .pipe-stage:last-child{ margin-right:0; }
  .pipe-stage::after{
    content:"→"; position:absolute; right:-30px; top:50%; transform: translateY(-50%);
    color: var(--ink-faint); font-size: 20px; font-family: var(--mono);
  }
  .pipe-stage:last-child::after{ content:""; }
  .pipe-icon{ font-family: var(--mono); font-size: 11px; color: var(--signal-blue); margin-bottom: 10px; }
  .pipe-title{ font-weight: 600; font-size: 14px; margin-bottom: 6px; }
  .pipe-detail{ font-size: 12px; color: var(--ink-dim); }

  /* ───────────────────────── Terminal block ───────────────────────── */
  .terminal{
    background: #060809; border: 1px solid var(--line); border-radius: 10px;
    overflow:hidden; margin-top: 28px;
  }
  .terminal-bar{ background: var(--bg-panel-2); padding: 10px 16px; display:flex; align-items:center; gap:8px; border-bottom: 1px solid var(--line); }
  .tdot{ width:10px; height:10px; border-radius:50%; }
  .terminal-bar .tdot:nth-child(1){ background:#ef6464; }
  .terminal-bar .tdot:nth-child(2){ background:#f0a93e; }
  .terminal-bar .tdot:nth-child(3){ background:#3ecf8e; }
  .terminal-label{ margin-left: 8px; font-family: var(--mono); font-size: 11px; color: var(--ink-faint); }
  .terminal-body{ padding: 20px 22px; font-family: var(--mono); font-size: 12.5px; line-height: 1.85; color: #b9c4d4; min-height: 220px; }
  .terminal-body .ok{ color: var(--signal-green); }
  .terminal-body .num{ color: var(--signal-amber); }
  .terminal-body .dim{ color: var(--ink-faint); }
  .cursor{ display:inline-block; width:7px; height:14px; background: var(--signal-green); vertical-align: -2px; animation: blink 1s step-end infinite; }
  @keyframes blink{ 50%{ opacity:0; } }

  /* ───────────────────────── Schema diagram ───────────────────────── */
  .schema-wrap{ display:flex; gap: 18px; flex-wrap: wrap; margin-top: 24px; }
  .schema-col{ flex:1; min-width: 240px; }
  .schema-col-title{ font-family: var(--mono); font-size: 11px; color: var(--ink-faint); text-transform:uppercase; letter-spacing:1px; margin-bottom: 12px; }
  .schema-node{
    background: var(--bg-panel); border: 1px solid var(--line); border-radius: 7px;
    padding: 12px 14px; margin-bottom: 10px; font-size: 13px; font-family: var(--mono);
    display:flex; justify-content:space-between; align-items:center;
    transition: border-color .2s, transform .2s;
  }
  .schema-node:hover{ border-color: var(--signal-blue); transform: translateX(3px); }
  .schema-node.fact{ border-left: 3px solid var(--signal-blue); }
  .schema-node.dim{ border-left: 3px solid var(--signal-green); }
  .schema-node small{ color: var(--ink-faint); font-size: 10px; }

  /* ───────────────────────── Perf table ───────────────────────── */
  .perf-table{ width:100%; border-collapse: collapse; margin-top: 24px; font-size: 13.5px; }
  .perf-table th{ text-align:left; font-family: var(--mono); font-size: 11px; color: var(--ink-faint); text-transform:uppercase; letter-spacing:1px; padding: 10px 14px; border-bottom: 1px solid var(--line-bright); }
  .perf-table td{ padding: 14px; border-bottom: 1px solid var(--line); }
  .perf-table tr:hover td{ background: var(--bg-panel); }
  .bar-track{ background: var(--line); border-radius: 4px; height: 6px; overflow:hidden; width: 100%; min-width: 100px; }
  .bar-fill{ height:100%; border-radius:4px; width:0; transition: width 1.2s cubic-bezier(.2,.8,.2,1); }
  .bar-fill.before{ background: var(--signal-red); }
  .bar-fill.after{ background: var(--signal-green); }
  .perf-time{ font-family: var(--mono); font-size: 12px; color: var(--ink-dim); white-space:nowrap; }

  /* ───────────────────────── Findings ───────────────────────── */
  .findings{ display:grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-top: 24px; }
  @media (max-width: 720px){ .findings{ grid-template-columns: 1fr; } }
  .finding-card{
    background: var(--bg-panel); border: 1px solid var(--line); border-radius: 9px; padding: 22px;
    border-top: 2px solid var(--line-bright); transition: border-color .25s, transform .25s;
  }
  .finding-card:hover{ transform: translateY(-3px); }
  .finding-card.green{ border-top-color: var(--signal-green); }
  .finding-card.red{ border-top-color: var(--signal-red); }
  .finding-tag{ font-family: var(--mono); font-size: 10px; text-transform:uppercase; letter-spacing:1px; margin-bottom:10px; display:inline-block; padding: 3px 8px; border-radius: 4px; }
  .finding-card.green .finding-tag{ color: var(--signal-green); background: rgba(62,207,142,0.1); }
  .finding-card.red .finding-tag{ color: var(--signal-red); background: rgba(239,100,100,0.1); }
  .finding-card.amber .finding-tag{ color: var(--signal-amber); background: rgba(240,169,62,0.1); }
  .finding-title{ font-weight: 600; font-size: 15px; margin-bottom: 8px; }
  .finding-body{ font-size: 13.5px; color: var(--ink-dim); }

  /* ───────────────────────── Footer ───────────────────────── */
  footer{ padding: 48px 0 64px; border-top: 1px solid var(--line); }
  .footer-row{ display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap: 16px; }
  .footer-note{ font-size: 12px; color: var(--ink-faint); font-family: var(--mono); max-width: 480px; }
  .footer-links{ display:flex; gap: 18px; }
  .footer-links a{ color: var(--ink-dim); text-decoration:none; font-family: var(--mono); font-size: 12px; }
  .footer-links a:hover{ color: var(--signal-blue); }
</style>
</head>
<body>

<div class="bg-grid"></div>
<div class="scanline"></div>

<nav>
  <div class="wrap">
    <div class="nav-brand"><span class="status-dot"></span><b>ascendeum-analytics</b> / pipeline overview</div>
    <ul class="nav-links">
      <li><a href="#problem">problem</a></li>
      <li><a href="#schema">schema</a></li>
      <li><a href="#performance">performance</a></li>
      <li><a href="#findings">findings</a></li>
    </ul>
  </div>
</nav>

<main class="wrap">

  <!-- ─────────────── HERO ─────────────── -->
  <section class="hero" style="border:none; padding-top: 72px;">
    <div class="eyebrow">marketing analytics platform · synthetic dataset, 304,200 records</div>
    <h1>From <span class="strike">8 hours of Excel</span> <span class="arrow">→</span> a same-day pipeline</h1>
    <p class="hero-sub">A digital marketing agency ran four ad platforms through one manual spreadsheet, every week. This rebuilds it as a real analytics stack — <b>MySQL star schema</b>, <b>40+ documented SQL queries</b>, and a <b>Python cleaning pipeline</b> — and surfaces a budget reallocation worth millions along the way.</p>
    <div class="hero-cta">
      <a class="btn btn-primary" href="#schema">View the schema ↓</a>
      <a class="btn btn-ghost" href="../sql/analysis/02_analysis_queries.sql">Browse SQL queries</a>
    </div>

    <div class="kpi-grid">
      <div class="kpi-cell"><div class="kpi-tag">USD</div><div class="kpi-label">Revenue Tracked</div><div class="kpi-value" data-target="71464705" data-prefix="$" data-format="money">$0</div></div>
      <div class="kpi-cell positive"><div class="kpi-tag">×</div><div class="kpi-label">Blended ROAS</div><div class="kpi-value" data-target="1.86" data-format="decimal" data-suffix="×">0.00×</div></div>
      <div class="kpi-cell"><div class="kpi-tag">n</div><div class="kpi-label">Customers Profiled</div><div class="kpi-value" data-target="14180" data-format="int">0</div></div>
      <div class="kpi-cell positive"><div class="kpi-tag">%</div><div class="kpi-label">Repeat Purchase Rate</div><div class="kpi-value" data-target="33.2" data-format="decimal" data-suffix="%">0.0%</div></div>
    </div>
  </section>

  <!-- ─────────────── PROBLEM / PIPELINE ─────────────── -->
  <section id="problem">
    <div class="section-head reveal">
      <div class="section-num">01 / THE PIPELINE</div>
      <div class="section-title">Twelve messy CSVs in, a star schema out</div>
      <div class="section-desc">Raw exports carry duplicate customers, invalid dates, orphaned campaign IDs, and revenue outliers — by design. The pipeline finds and fixes them, not assumes them away.</div>
    </div>

    <div class="pipeline reveal">
      <div class="pipe-stage"><div class="pipe-icon">[ 01 ]</div><div class="pipe-title">Raw CSVs</div><div class="pipe-detail">12 files, 304,200 rows, intentionally dirty</div></div>
      <div class="pipe-stage"><div class="pipe-icon">[ 02 ]</div><div class="pipe-title">Clean</div><div class="pipe-detail">Dedup · impute · outlier capping</div></div>
      <div class="pipe-stage"><div class="pipe-icon">[ 03 ]</div><div class="pipe-title">Star Schema</div><div class="pipe-detail">4 fact + 7 dimension tables, MySQL 8.0</div></div>
      <div class="pipe-stage"><div class="pipe-icon">[ 04 ]</div><div class="pipe-title">SQL Layer</div><div class="pipe-detail">40+ queries · views · stored procs</div></div>
      <div class="pipe-stage"><div class="pipe-icon">[ 05 ]</div><div class="pipe-title">Dashboards</div><div class="pipe-detail">Tableau / Looker + client report</div></div>
    </div>

    <div class="terminal reveal">
      <div class="terminal-bar">
        <div class="tdot"></div><div class="tdot"></div><div class="tdot"></div>
        <div class="terminal-label">python data_cleaning_pipeline.py</div>
      </div>
      <div class="terminal-body" id="termBody"></div>
    </div>
  </section>

  <!-- ─────────────── SCHEMA ─────────────── -->
  <section id="schema">
    <div class="section-head reveal">
      <div class="section-num">02 / DATABASE DESIGN</div>
      <div class="section-title">Star schema, sized to the question being asked</div>
      <div class="section-desc">Four fact tables, each at its own grain — daily spend, sessions, conversions, order lines — joined through seven shared dimensions.</div>
    </div>

    <div class="schema-wrap reveal">
      <div class="schema-col">
        <div class="schema-col-title">Fact Tables</div>
        <div class="schema-node fact">fact_orders <small>order line grain</small></div>
        <div class="schema-node fact">fact_ad_spend <small>daily · campaign</small></div>
        <div class="schema-node fact">fact_sessions <small>session grain</small></div>
        <div class="schema-node fact">fact_conversions <small>event grain</small></div>
      </div>
      <div class="schema-col">
        <div class="schema-col-title">Dimension Tables</div>
        <div class="schema-node dim">dim_customer <small>segment · LTV</small></div>
        <div class="schema-node dim">dim_campaign <small>platform · budget</small></div>
        <div class="schema-node dim">dim_product <small>category · margin</small></div>
        <div class="schema-node dim">dim_date <small>year · quarter · week</small></div>
        <div class="schema-node dim">dim_country / dim_device / dim_traffic_source <small>geo · UX · channel</small></div>
      </div>
    </div>
  </section>

  <!-- ─────────────── PERFORMANCE ─────────────── -->
  <section id="performance">
    <div class="section-head reveal">
      <div class="section-num">03 / QUERY OPTIMIZATION</div>
      <div class="section-title">EXPLAIN-driven, not theoretical</div>
      <div class="section-desc">Every fix below was measured with <code style="font-family:var(--mono); color:var(--signal-blue);">EXPLAIN</code> before and after — not assumed to help.</div>
    </div>

    <table class="perf-table reveal">
      <thead><tr><th>Query</th><th>Before</th><th></th><th>After</th><th>Fix</th></tr></thead>
      <tbody>
        <tr>
          <td>Campaign performance view</td>
          <td class="perf-time">8.7s</td>
          <td style="width:160px;"><div class="bar-track"><div class="bar-fill before" data-pct="100"></div></div></td>
          <td class="perf-time" style="color:var(--signal-green);">0.4s</td>
          <td style="color:var(--ink-dim); font-size:12.5px;">Composite index <code style="font-family:var(--mono);">(campaign_id, is_attributed)</code></td>
        </tr>
        <tr>
          <td>Monthly revenue trend</td>
          <td class="perf-time">4.1s</td>
          <td><div class="bar-track"><div class="bar-fill before" data-pct="47"></div></div></td>
          <td class="perf-time" style="color:var(--signal-green);">0.3s</td>
          <td style="color:var(--ink-dim); font-size:12.5px;">Index + nightly snapshot table</td>
        </tr>
        <tr>
          <td>Customer 360 lookup</td>
          <td class="perf-time">1.9s</td>
          <td><div class="bar-track"><div class="bar-fill before" data-pct="22"></div></div></td>
          <td class="perf-time" style="color:var(--signal-green);">0.02s</td>
          <td style="color:var(--ink-dim); font-size:12.5px;">Predicate pushdown before join</td>
        </tr>
        <tr>
          <td>Funnel session→conversion</td>
          <td class="perf-time">6.2s</td>
          <td><div class="bar-track"><div class="bar-fill before" data-pct="71"></div></div></td>
          <td class="perf-time" style="color:var(--signal-green);">0.7s</td>
          <td style="color:var(--ink-dim); font-size:12.5px;">Index on <code style="font-family:var(--mono);">fact_conversions(session_id)</code></td>
        </tr>
      </tbody>
    </table>
  </section>

  <!-- ─────────────── FINDINGS ─────────────── -->
  <section id="findings">
    <div class="section-head reveal">
      <div class="section-num">04 / KEY FINDINGS</div>
      <div class="section-title">What the data actually said</div>
      <div class="section-desc">Four findings that change where the next marketing dollar should go.</div>
    </div>

    <div class="findings reveal">
      <div class="finding-card green">
        <div class="finding-tag">Underfunded</div>
        <div class="finding-title">Display has the strongest ROAS of any platform</div>
        <div class="finding-body">Treated internally as a brand-awareness channel — but the numbers say it's the most efficient dollar in the budget.</div>
      </div>
      <div class="finding-card red">
        <div class="finding-tag">Review</div>
        <div class="finding-title">LinkedIn carries the weakest unit economics</div>
        <div class="finding-body">Highest CPC, lowest LTV-to-CAC ratio of the four paid channels — the clearest candidate for reallocation.</div>
      </div>
      <div class="finding-card amber">
        <div class="finding-tag">CRO priority</div>
        <div class="finding-title">Mobile converts at ~half the desktop rate</div>
        <div class="finding-body">Despite carrying the most session volume — the single highest-leverage fix available in the funnel.</div>
      </div>
      <div class="finding-card amber">
        <div class="finding-tag">Under-leveraged</div>
        <div class="finding-title">Repeat buyers drive outsized revenue share</div>
        <div class="finding-body">~1/5 of the customer base, but close to half of total revenue — retention is cheaper than acquisition here.</div>
      </div>
    </div>
  </section>

  <footer>
    <div class="footer-row">
      <div class="footer-note">Portfolio project on synthetic data, built to mirror the structure and data-quality challenges of a real marketing agency's reporting stack. All figures are pipeline output, not placeholders.</div>
      <div class="footer-links">
        <a href="../README.md">README</a>
        <a href="../sql/schema/01_create_schema.sql">Schema</a>
        <a href="interview_questions.md">Interview Prep</a>
      </div>
    </div>
  </footer>

</main>

<script>
  // ── Counter animation ──
  function formatVal(val, fmt, prefix, suffix){
    if(fmt === 'money'){
      return (prefix||'') + Math.round(val).toLocaleString('en-US');
    }
    if(fmt === 'int'){
      return Math.round(val).toLocaleString('en-US');
    }
    if(fmt === 'decimal'){
      return val.toFixed(2) + (suffix||'');
    }
    return val;
  }

  function animateCounter(el){
    const target = parseFloat(el.dataset.target);
    const fmt = el.dataset.format;
    const prefix = el.dataset.prefix || '';
    const suffix = el.dataset.suffix || '';
    const duration = 1800;
    const start = performance.now();

    function tick(now){
      const elapsed = now - start;
      const progress = Math.min(elapsed / duration, 1);
      const eased = 1 - Math.pow(1 - progress, 3); // ease-out-cubic
      const current = target * eased;
      el.textContent = formatVal(current, fmt, prefix, suffix);
      if(progress < 1){ requestAnimationFrame(tick); }
      else { el.textContent = formatVal(target, fmt, prefix, suffix); }
    }
    requestAnimationFrame(tick);
  }

  // ── Terminal typing sequence ──
  const termLines = [
    {text: "&gt; Loading raw data...", cls: "dim"},
    {text: "  &check; customers.csv         <span class='num'>14,790</span> rows", cls: "ok"},
    {text: "  &check; campaigns.csv          <span class='num'>500</span> rows", cls: "ok"},
    {text: "  &check; marketing_spend.csv  <span class='num'>35,000</span> rows", cls: "ok"},
    {text: "  &check; website_sessions.csv <span class='num'>120,000</span> rows", cls: "ok"},
    {text: "  &check; orders.csv            <span class='num'>18,000</span> rows", cls: "ok"},
    {text: "", cls: ""},
    {text: "&gt; Cleaning &amp; validating...", cls: "dim"},
    {text: "  duplicate emails quarantined     <span class='num'>278</span>", cls: ""},
    {text: "  invalid campaign IDs removed     <span class='num'>15</span>", cls: ""},
    {text: "  orphan spend rows removed        <span class='num'>700</span>", cls: ""},
    {text: "  revenue outliers corrected       <span class='num'>97</span>", cls: ""},
    {text: "", cls: ""},
    {text: "&gt; KPI summary", cls: "dim"},
    {text: "  Total Revenue        <span class='num'>$71,464,705</span>", cls: "ok"},
    {text: "  Blended ROAS         <span class='num'>1.86x</span>", cls: "ok"},
    {text: "  Repeat Purchase Rate <span class='num'>33.2%</span>", cls: "ok"},
    {text: "", cls: ""},
    {text: "&gt; Pipeline complete <span class='cursor'></span>", cls: "dim"},
  ];

  function typeTerminal(){
    const body = document.getElementById('termBody');
    let i = 0;
    function nextLine(){
      if(i >= termLines.length) return;
      const line = termLines[i];
      const p = document.createElement('div');
      p.innerHTML = line.text;
      if(line.cls) p.classList.add(line.cls);
      p.style.opacity = '0';
      p.style.transform = 'translateY(4px)';
      p.style.transition = 'opacity .25s ease, transform .25s ease';
      body.appendChild(p);
      requestAnimationFrame(()=>{ p.style.opacity='1'; p.style.transform='translateY(0)'; });
      i++;
      setTimeout(nextLine, line.text === "" ? 120 : 220);
    }
    nextLine();
  }

  // ── Scroll reveal ──
  const revealEls = document.querySelectorAll('.reveal');
  const io = new IntersectionObserver((entries)=>{
    entries.forEach(entry=>{
      if(entry.isIntersecting){
        entry.target.classList.add('in');
        io.unobserve(entry.target);
      }
    });
  }, { threshold: 0.15 });
  revealEls.forEach(el => io.observe(el));

  // ── Perf bars fill on scroll into view ──
  const perfTable = document.querySelector('.perf-table');
  let perfDone = false;
  const perfIo = new IntersectionObserver((entries)=>{
    entries.forEach(entry=>{
      if(entry.isIntersecting && !perfDone){
        perfDone = true;
        document.querySelectorAll('.bar-fill').forEach(bar=>{
          setTimeout(()=>{ bar.style.width = bar.dataset.pct + '%'; }, 150);
        });
      }
    });
  }, { threshold: 0.3 });
  if(perfTable) perfIo.observe(perfTable);

  // ── Kick off on load ──
  window.addEventListener('DOMContentLoaded', ()=>{
    document.querySelectorAll('.kpi-value').forEach(animateCounter);
    setTimeout(typeTerminal, 700);
  });
</script>

</body>
</html>
