<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Selenium TestNG Framework — Complete Documentation</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=JetBrains+Mono:wght@300;400;500&family=Instrument+Sans:wght@400;500;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg:        #0a0c10;
    --surface:   #0f1318;
    --card:      #141920;
    --border:    #1e2730;
    --accent:    #00e5a0;
    --accent2:   #00aaff;
    --accent3:   #ff6b35;
    --warn:      #ffd166;
    --text:      #e2e8f0;
    --muted:     #64748b;
    --code-bg:   #0d1117;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Instrument Sans', sans-serif;
    font-size: 15px;
    line-height: 1.75;
    overflow-x: hidden;
  }

  /* ── Grid noise overlay ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      repeating-linear-gradient(0deg, transparent, transparent 39px, rgba(0,229,160,.03) 39px, rgba(0,229,160,.03) 40px),
      repeating-linear-gradient(90deg, transparent, transparent 39px, rgba(0,229,160,.03) 39px, rgba(0,229,160,.03) 40px);
    pointer-events: none;
    z-index: 0;
  }

  /* ── Glow blobs ── */
  .blob {
    position: fixed;
    border-radius: 50%;
    filter: blur(120px);
    opacity: .15;
    pointer-events: none;
    z-index: 0;
  }
  .blob-1 { width: 600px; height: 600px; background: var(--accent);  top: -200px; left: -200px; }
  .blob-2 { width: 500px; height: 500px; background: var(--accent2); bottom: -100px; right: -150px; }
  .blob-3 { width: 400px; height: 400px; background: var(--accent3); top: 40%; left: 50%; transform: translate(-50%,-50%); }

  /* ── Sidebar nav ── */
  .sidebar {
    position: fixed;
    top: 0; left: 0;
    width: 260px;
    height: 100vh;
    background: rgba(15,19,24,.97);
    border-right: 1px solid var(--border);
    overflow-y: auto;
    z-index: 100;
    padding: 32px 0 32px;
    backdrop-filter: blur(20px);
  }

  .sidebar-logo {
    padding: 0 24px 28px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 20px;
  }
  .sidebar-logo .badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: rgba(0,229,160,.1);
    border: 1px solid rgba(0,229,160,.25);
    border-radius: 6px;
    padding: 6px 12px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    letter-spacing: .05em;
    margin-bottom: 12px;
  }
  .sidebar-logo h2 {
    font-family: 'Syne', sans-serif;
    font-size: 16px;
    font-weight: 800;
    color: #fff;
    line-height: 1.3;
  }
  .sidebar-logo p {
    font-size: 12px;
    color: var(--muted);
    margin-top: 4px;
  }

  .nav-group { margin-bottom: 6px; }
  .nav-group-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    letter-spacing: .12em;
    text-transform: uppercase;
    color: var(--muted);
    padding: 8px 24px 4px;
  }
  .nav-link {
    display: block;
    padding: 7px 24px;
    font-size: 13px;
    color: #94a3b8;
    text-decoration: none;
    border-left: 2px solid transparent;
    transition: all .2s;
    position: relative;
  }
  .nav-link:hover, .nav-link.active {
    color: var(--accent);
    border-left-color: var(--accent);
    background: rgba(0,229,160,.05);
  }

  /* ── Main content ── */
  .main {
    margin-left: 260px;
    min-height: 100vh;
    position: relative;
    z-index: 1;
  }

  /* ── Hero ── */
  .hero {
    padding: 80px 64px 64px;
    border-bottom: 1px solid var(--border);
    position: relative;
    overflow: hidden;
  }
  .hero::after {
    content: 'SELENIUM';
    position: absolute;
    right: -20px;
    top: 50%;
    transform: translateY(-50%) rotate(-90deg);
    font-family: 'Syne', sans-serif;
    font-size: 100px;
    font-weight: 800;
    color: rgba(0,229,160,.03);
    letter-spacing: .1em;
    pointer-events: none;
    white-space: nowrap;
  }

  .hero-eyebrow {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 20px;
  }
  .pill {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 4px 12px;
    border-radius: 100px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: .04em;
  }
  .pill-green  { background: rgba(0,229,160,.12); color: var(--accent);  border: 1px solid rgba(0,229,160,.2); }
  .pill-blue   { background: rgba(0,170,255,.12); color: var(--accent2); border: 1px solid rgba(0,170,255,.2); }
  .pill-orange { background: rgba(255,107,53,.12); color: var(--accent3); border: 1px solid rgba(255,107,53,.2); }
  .pill-yellow { background: rgba(255,209,102,.12); color: var(--warn);  border: 1px solid rgba(255,209,102,.2); }
  .pill-dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; }

  .hero h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(36px, 5vw, 58px);
    font-weight: 800;
    line-height: 1.1;
    color: #fff;
    margin-bottom: 20px;
  }
  .hero h1 span { color: var(--accent); }

  .hero-desc {
    font-size: 17px;
    color: #94a3b8;
    max-width: 680px;
    line-height: 1.7;
    margin-bottom: 36px;
  }

  .hero-stats {
    display: flex;
    gap: 0;
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
    max-width: 620px;
    background: var(--card);
  }
  .stat {
    flex: 1;
    padding: 20px 24px;
    border-right: 1px solid var(--border);
    text-align: center;
  }
  .stat:last-child { border-right: none; }
  .stat-num {
    font-family: 'Syne', sans-serif;
    font-size: 28px;
    font-weight: 800;
    color: var(--accent);
    line-height: 1;
    margin-bottom: 4px;
  }
  .stat-label {
    font-size: 11px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: .06em;
  }

  /* ── Section ── */
  section {
    padding: 64px 64px;
    border-bottom: 1px solid var(--border);
  }

  .section-header {
    display: flex;
    align-items: flex-start;
    gap: 20px;
    margin-bottom: 40px;
  }
  .section-icon {
    width: 44px;
    height: 44px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
    flex-shrink: 0;
    margin-top: 2px;
  }
  .icon-green  { background: rgba(0,229,160,.15); border: 1px solid rgba(0,229,160,.2); }
  .icon-blue   { background: rgba(0,170,255,.15); border: 1px solid rgba(0,170,255,.2); }
  .icon-orange { background: rgba(255,107,53,.15); border: 1px solid rgba(255,107,53,.2); }
  .icon-yellow { background: rgba(255,209,102,.15); border: 1px solid rgba(255,209,102,.2); }
  .icon-purple { background: rgba(168,85,247,.15); border: 1px solid rgba(168,85,247,.2); }

  .section-title-group h2 {
    font-family: 'Syne', sans-serif;
    font-size: 26px;
    font-weight: 800;
    color: #fff;
    margin-bottom: 6px;
  }
  .section-title-group p {
    color: var(--muted);
    font-size: 14px;
    max-width: 600px;
  }

  /* ── Cards ── */
  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 16px;
  }
  .card-grid-2 { grid-template-columns: repeat(auto-fill, minmax(380px, 1fr)); }

  .card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 24px;
    transition: border-color .2s, transform .2s;
    position: relative;
    overflow: hidden;
  }
  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), transparent);
    opacity: 0;
    transition: opacity .2s;
  }
  .card:hover { border-color: rgba(0,229,160,.3); transform: translateY(-2px); }
  .card:hover::before { opacity: 1; }

  .card-icon { font-size: 28px; margin-bottom: 14px; }
  .card h3 {
    font-family: 'Syne', sans-serif;
    font-size: 16px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 8px;
  }
  .card p { font-size: 13px; color: #94a3b8; line-height: 1.65; }

  .card-tag {
    display: inline-block;
    margin-top: 12px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    padding: 3px 10px;
    border-radius: 4px;
    border: 1px solid var(--border);
    color: var(--muted);
  }

  /* ── Code blocks ── */
  .code-block {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
    margin: 16px 0;
  }
  .code-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 16px;
    border-bottom: 1px solid var(--border);
    background: rgba(255,255,255,.02);
  }
  .code-header .dots { display: flex; gap: 6px; }
  .dot { width: 10px; height: 10px; border-radius: 50%; }
  .dot-r { background: #ff5f57; }
  .dot-y { background: #febc2e; }
  .dot-g { background: #28c840; }
  .code-lang {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: .06em;
  }
  .code-body {
    padding: 20px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12.5px;
    line-height: 1.7;
    overflow-x: auto;
    color: #e2e8f0;
    white-space: pre;
  }
  .kw  { color: #ff79c6; }
  .cls { color: #8be9fd; }
  .fn  { color: #50fa7b; }
  .str { color: #f1fa8c; }
  .cmt { color: #6272a4; font-style: italic; }
  .ann { color: #ff79c6; }
  .num { color: #bd93f9; }

  /* ── File tree ── */
  .file-tree {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 24px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12.5px;
    line-height: 2;
    overflow-x: auto;
  }
  .tree-dir  { color: var(--accent2); font-weight: 500; }
  .tree-file { color: #e2e8f0; }
  .tree-note { color: var(--muted); font-size: 11px; margin-left: 8px; }
  .tree-indent { color: var(--border); }

  /* ── Table ── */
  .data-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    margin: 16px 0;
  }
  .data-table th {
    text-align: left;
    padding: 12px 16px;
    background: rgba(255,255,255,.03);
    border: 1px solid var(--border);
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 700;
    color: var(--accent);
    letter-spacing: .05em;
    text-transform: uppercase;
  }
  .data-table td {
    padding: 12px 16px;
    border: 1px solid var(--border);
    color: #94a3b8;
    vertical-align: top;
  }
  .data-table tr:hover td { background: rgba(255,255,255,.02); }
  .data-table td:first-child {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--accent2);
  }

  /* ── Feature list ── */
  .feature-list { list-style: none; display: flex; flex-direction: column; gap: 10px; }
  .feature-list li {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 14px 18px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    font-size: 13.5px;
    color: #cbd5e1;
    transition: border-color .2s;
  }
  .feature-list li:hover { border-color: rgba(0,229,160,.25); }
  .feature-list li::before {
    content: '✓';
    color: var(--accent);
    font-weight: 700;
    font-size: 14px;
    flex-shrink: 0;
    margin-top: 1px;
  }
  .feature-list li strong { color: #fff; }

  /* ── Command blocks ── */
  .cmd-grid { display: flex; flex-direction: column; gap: 10px; }
  .cmd-row {
    display: flex;
    align-items: center;
    gap: 16px;
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px 18px;
  }
  .cmd-label {
    flex-shrink: 0;
    width: 160px;
    font-size: 12px;
    color: var(--muted);
  }
  .cmd-code {
    font-family: 'JetBrains Mono', monospace;
    font-size: 12.5px;
    color: var(--accent);
    flex: 1;
  }
  .cmd-dollar {
    color: var(--muted);
    margin-right: 8px;
    font-family: 'JetBrains Mono', monospace;
  }

  /* ── Flow diagram ── */
  .flow {
    display: flex;
    align-items: center;
    gap: 0;
    overflow-x: auto;
    padding: 24px 0;
  }
  .flow-step {
    flex-shrink: 0;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px 20px;
    text-align: center;
    min-width: 120px;
  }
  .flow-step .step-icon { font-size: 24px; margin-bottom: 6px; }
  .flow-step .step-name {
    font-family: 'Syne', sans-serif;
    font-size: 12px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 4px;
  }
  .flow-step .step-desc { font-size: 10px; color: var(--muted); }
  .flow-arrow {
    flex-shrink: 0;
    width: 40px;
    text-align: center;
    color: var(--accent);
    font-size: 20px;
  }

  /* ── Layer diagram ── */
  .layers { display: flex; flex-direction: column; gap: 8px; }
  .layer {
    border-radius: 10px;
    padding: 18px 24px;
    border: 1px solid;
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .layer-tests   { background: rgba(0,229,160,.05); border-color: rgba(0,229,160,.2); }
  .layer-pages   { background: rgba(0,170,255,.05); border-color: rgba(0,170,255,.2); }
  .layer-base    { background: rgba(255,107,53,.05); border-color: rgba(255,107,53,.2); }
  .layer-utils   { background: rgba(255,209,102,.05); border-color: rgba(255,209,102,.2); }
  .layer-driver  { background: rgba(168,85,247,.05); border-color: rgba(168,85,247,.2); }
  .layer-icon { font-size: 22px; flex-shrink: 0; }
  .layer-content h4 { font-family: 'Syne', sans-serif; font-size: 15px; font-weight: 700; color: #fff; margin-bottom: 2px; }
  .layer-content p  { font-size: 12px; color: var(--muted); }
  .layer-files {
    margin-left: auto;
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
    justify-content: flex-end;
  }
  .layer-file {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    padding: 3px 8px;
    border-radius: 4px;
    background: rgba(255,255,255,.06);
    color: #94a3b8;
    border: 1px solid rgba(255,255,255,.08);
  }

  /* ── Group badges ── */
  .group-grid { display: flex; flex-direction: column; gap: 12px; }
  .group-row {
    display: grid;
    grid-template-columns: 120px 1fr auto;
    align-items: center;
    gap: 20px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px 20px;
  }
  .group-name {
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 800;
  }
  .group-desc { font-size: 13px; color: var(--muted); }
  .group-count {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    padding: 4px 10px;
    border-radius: 6px;
    background: rgba(255,255,255,.05);
    color: var(--muted);
    white-space: nowrap;
  }
  .grp-smoke   { color: var(--accent); }
  .grp-reg     { color: var(--accent2); }
  .grp-e2e     { color: var(--warn); }
  .grp-sec     { color: var(--accent3); }

  /* ── Scrollbar ── */
  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

  /* ── Animations ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .hero > * { animation: fadeUp .6s ease both; }
  .hero > *:nth-child(1) { animation-delay: .1s; }
  .hero > *:nth-child(2) { animation-delay: .2s; }
  .hero > *:nth-child(3) { animation-delay: .3s; }
  .hero > *:nth-child(4) { animation-delay: .4s; }
  .hero > *:nth-child(5) { animation-delay: .5s; }

  /* ── Tech stack badges ── */
  .tech-stack { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 16px; }
  .tech-badge {
    display: flex;
    align-items: center;
    gap: 8px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 16px;
    font-size: 13px;
    color: #cbd5e1;
    transition: border-color .2s;
  }
  .tech-badge:hover { border-color: rgba(0,229,160,.3); }
  .tech-badge .tech-icon { font-size: 18px; }
  .tech-badge .tech-ver {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    margin-left: 4px;
  }

  /* ── Footer ── */
  .footer {
    padding: 40px 64px;
    text-align: center;
    color: var(--muted);
    font-size: 13px;
  }
  .footer span { color: var(--accent); }

  @media (max-width: 900px) {
    .sidebar { display: none; }
    .main    { margin-left: 0; }
    .hero    { padding: 40px 24px; }
    section  { padding: 40px 24px; }
    .footer  { padding: 24px; }
    .group-row { grid-template-columns: 1fr; }
    .layer-files { margin-left: 0; margin-top: 8px; }
    .layer { flex-wrap: wrap; }
  }
</style>
</head>
<body>

<div class="blob blob-1"></div>
<div class="blob blob-2"></div>
<div class="blob blob-3"></div>

<!-- ── SIDEBAR ── -->
<nav class="sidebar">
  <div class="sidebar-logo">
    <div class="badge"><span class="pill-dot"></span> v1.0.0 STABLE</div>
    <h2>Selenium TestNG<br>Framework</h2>
    <p>Java · Selenium 4 · TestNG 7</p>
  </div>

  <div class="nav-group">
    <div class="nav-group-label">Overview</div>
    <a href="#overview"    class="nav-link">📋 Introduction</a>
    <a href="#techstack"   class="nav-link">🔧 Tech Stack</a>
    <a href="#structure"   class="nav-link">📁 Project Structure</a>
    <a href="#architecture" class="nav-link">🏗 Architecture</a>
  </div>

  <div class="nav-group">
    <div class="nav-group-label">Core Features</div>
    <a href="#pom"         class="nav-link">📄 Page Object Model</a>
    <a href="#driver"      class="nav-link">🌐 Driver Management</a>
    <a href="#waits"       class="nav-link">⏳ Smart Waits</a>
    <a href="#config"      class="nav-link">⚙️ Configuration</a>
    <a href="#data"        class="nav-link">🎲 Test Data</a>
    <a href="#reporting"   class="nav-link">📊 Reporting</a>
    <a href="#logging"     class="nav-link">📝 Logging</a>
    <a href="#screenshots" class="nav-link">📸 Screenshots</a>
    <a href="#retry"       class="nav-link">🔁 Retry & Listeners</a>
    <a href="#parallel"    class="nav-link">⚡ Parallel Execution</a>
  </div>

  <div class="nav-group">
    <div class="nav-group-label">Test Coverage</div>
    <a href="#tests-login"    class="nav-link">🔐 Login Tests</a>
    <a href="#tests-form"     class="nav-link">📝 Form Tests</a>
    <a href="#tests-e2e"      class="nav-link">🚀 E2E Workflows</a>
    <a href="#groups"         class="nav-link">🏷 Test Groups</a>
  </div>

  <div class="nav-group">
    <div class="nav-group-label">CI/CD</div>
    <a href="#jenkins"   class="nav-link">🔧 Jenkins Pipeline</a>
    <a href="#github"    class="nav-link">🐙 GitHub Actions</a>
    <a href="#commands"  class="nav-link">💻 Run Commands</a>
  </div>

  <div class="nav-group">
    <div class="nav-group-label">Extend</div>
    <a href="#add-page" class="nav-link">➕ Add a Page</a>
    <a href="#add-test" class="nav-link">➕ Add a Test</a>
  </div>
</nav>

<!-- ── MAIN ── -->
<main class="main">

  <!-- HERO -->
  <div class="hero" id="overview">
    <div class="hero-eyebrow">
      <span class="pill pill-green"><span class="pill-dot"></span> Production Ready</span>
      <span class="pill pill-blue">Java 11</span>
      <span class="pill pill-orange">Selenium 4.x</span>
      <span class="pill pill-yellow">TestNG 7.x</span>
    </div>

    <h1>Selenium TestNG<br><span>Automation Framework</span></h1>

    <p class="hero-desc">
      A complete, enterprise-grade test automation framework built on <strong>Java</strong>, <strong>Selenium WebDriver 4</strong>, and <strong>TestNG</strong>. Implements the Page Object Model, ThreadLocal parallel execution, data-driven testing, environment-aware configuration, ExtentReports HTML reporting, and full CI/CD integration with Jenkins and GitHub Actions.
    </p>

    <div class="hero-stats">
      <div class="stat"><div class="stat-num">26</div><div class="stat-label">Source Files</div></div>
      <div class="stat"><div class="stat-num">3</div><div class="stat-label">Test Classes</div></div>
      <div class="stat"><div class="stat-num">20+</div><div class="stat-label">Test Cases</div></div>
      <div class="stat"><div class="stat-num">4</div><div class="stat-label">Test Suites</div></div>
    </div>
  </div>

  <!-- TECH STACK -->
  <section id="techstack">
    <div class="section-header">
      <div class="section-icon icon-blue">🔧</div>
      <div class="section-title-group">
        <h2>Technology Stack</h2>
        <p>Every dependency has a specific role. No bloat — only what's needed for a professional automation framework.</p>
      </div>
    </div>

    <table class="data-table">
      <thead>
        <tr><th>Library / Tool</th><th>Version</th><th>Role</th></tr>
      </thead>
      <tbody>
        <tr><td>Selenium WebDriver</td><td>4.15.0</td><td>Browser automation engine — clicks, typing, navigation, waits</td></tr>
        <tr><td>TestNG</td><td>7.8.0</td><td>Test runner with annotations, groups, data providers, parallel support</td></tr>
        <tr><td>WebDriverManager</td><td>5.6.3</td><td>Auto-downloads correct browser driver binaries — no manual setup</td></tr>
        <tr><td>ExtentReports</td><td>5.1.1</td><td>Rich HTML test reports with charts, screenshots, system info</td></tr>
        <tr><td>Log4j2</td><td>2.21.1</td><td>Structured logging to console + rolling file appenders</td></tr>
        <tr><td>Jackson Databind</td><td>2.15.3</td><td>JSON parsing for config and test data files</td></tr>
        <tr><td>AssertJ</td><td>3.24.2</td><td>Fluent, readable assertions with rich failure messages</td></tr>
        <tr><td>JavaFaker</td><td>1.0.2</td><td>Generates realistic random test data (names, emails, phones…)</td></tr>
        <tr><td>Maven Surefire</td><td>3.1.2</td><td>Executes TestNG suites via <code>mvn test</code></td></tr>
        <tr><td>Java</td><td>11 LTS</td><td>Core language — streams, lambdas, ThreadLocal, try-with-resources</td></tr>
      </tbody>
    </table>
  </section>

  <!-- STRUCTURE -->
  <section id="structure">
    <div class="section-header">
      <div class="section-icon icon-green">📁</div>
      <div class="section-title-group">
        <h2>Project Structure</h2>
        <p>Standard Maven layout with clean separation of framework code, page objects, test classes, and resources.</p>
      </div>
    </div>

    <div class="file-tree">
<span class="tree-dir">selenium-testng-framework/</span>
├── <span class="tree-dir">src/main/java/com/automation/</span>
│   ├── <span class="tree-dir">base/</span>
│   │   ├── <span class="tree-file">BasePage.java</span>       <span class="tree-note">← 30+ reusable Selenium actions</span>
│   │   ├── <span class="tree-file">BaseTest.java</span>       <span class="tree-note">← TestNG lifecycle + report hooks</span>
│   │   └── <span class="tree-file">DriverManager.java</span>  <span class="tree-note">← ThreadLocal WebDriver factory</span>
│   ├── <span class="tree-dir">config/</span>
│   │   └── <span class="tree-file">ConfigReader.java</span>   <span class="tree-note">← Singleton env-aware config loader</span>
│   ├── <span class="tree-dir">pages/</span>
│   │   ├── <span class="tree-file">LoginPage.java</span>      <span class="tree-note">← POM: Login page actions & states</span>
│   │   ├── <span class="tree-file">DashboardPage.java</span>  <span class="tree-note">← POM: Dashboard verification</span>
│   │   └── <span class="tree-file">FormPage.java</span>       <span class="tree-note">← POM: Full form interactions</span>
│   └── <span class="tree-dir">utils/</span>
│       ├── <span class="tree-file">ExtentReportManager.java</span>  <span class="tree-note">← HTML report lifecycle</span>
│       ├── <span class="tree-file">ScreenshotUtil.java</span>       <span class="tree-note">← Timestamped PNG capture</span>
│       ├── <span class="tree-file">TestDataUtil.java</span>         <span class="tree-note">← JavaFaker random data builder</span>
│       └── <span class="tree-file">WaitUtil.java</span>             <span class="tree-note">← Static explicit wait helpers</span>
│
├── <span class="tree-dir">src/test/java/com/automation/</span>
│   ├── <span class="tree-dir">tests/</span>
│   │   ├── <span class="tree-file">LoginTest.java</span>         <span class="tree-note">← 7 login scenarios + DataProvider</span>
│   │   ├── <span class="tree-file">FormTest.java</span>          <span class="tree-note">← 8 form tests incl. security</span>
│   │   └── <span class="tree-file">UserWorkflowTest.java</span>  <span class="tree-note">← 5 end-to-end journey tests</span>
│   └── <span class="tree-dir">listeners/</span>
│       ├── <span class="tree-file">TestListener.java</span>      <span class="tree-note">← ITestListener + ISuiteListener</span>
│       └── <span class="tree-file">RetryAnalyzer.java</span>     <span class="tree-note">← Auto-retry flaky tests (×2)</span>
│
├── <span class="tree-dir">src/test/resources/</span>
│   ├── <span class="tree-file">testng.xml</span>                <span class="tree-note">← 4 suites: smoke/regression/e2e/security</span>
│   ├── <span class="tree-file">config.properties</span>         <span class="tree-note">← Default environment config</span>
│   ├── <span class="tree-file">config-staging.properties</span>  <span class="tree-note">← Staging overrides</span>
│   └── <span class="tree-file">log4j2.xml</span>                <span class="tree-note">← Console + file logging config</span>
│
├── <span class="tree-dir">.github/workflows/</span>
│   └── <span class="tree-file">ci.yml</span>          <span class="tree-note">← GitHub Actions pipeline</span>
├── <span class="tree-file">Jenkinsfile</span>         <span class="tree-note">← Jenkins declarative pipeline</span>
├── <span class="tree-file">pom.xml</span>             <span class="tree-note">← Maven build + all dependencies</span>
└── <span class="tree-file">README.md</span>           <span class="tree-note">← Full usage documentation</span>
    </div>
  </section>

  <!-- ARCHITECTURE -->
  <section id="architecture">
    <div class="section-header">
      <div class="section-icon icon-orange">🏗</div>
      <div class="section-title-group">
        <h2>Framework Architecture</h2>
        <p>Clean layered design — each layer has one responsibility. Tests never touch Selenium directly.</p>
      </div>
    </div>

    <div class="layers">
      <div class="layer layer-tests">
        <div class="layer-icon">🧪</div>
        <div class="layer-content">
          <h4>Layer 5 — Test Layer</h4>
          <p>Business-readable test cases using TestNG annotations. Tests only call Page Object methods — no locators, no raw Selenium.</p>
        </div>
        <div class="layer-files">
          <span class="layer-file">LoginTest.java</span>
          <span class="layer-file">FormTest.java</span>
          <span class="layer-file">UserWorkflowTest.java</span>
        </div>
      </div>
      <div class="layer layer-pages">
        <div class="layer-icon">📄</div>
        <div class="layer-content">
          <h4>Layer 4 — Page Object Layer (POM)</h4>
          <p>Each web page has a dedicated class with all locators and actions. Locator changes only affect one file.</p>
        </div>
        <div class="layer-files">
          <span class="layer-file">LoginPage.java</span>
          <span class="layer-file">DashboardPage.java</span>
          <span class="layer-file">FormPage.java</span>
        </div>
      </div>
      <div class="layer layer-base">
        <div class="layer-icon">⚙️</div>
        <div class="layer-content">
          <h4>Layer 3 — Base Layer</h4>
          <p>Reusable Selenium wrapper methods. All pages inherit from BasePage. All tests inherit from BaseTest.</p>
        </div>
        <div class="layer-files">
          <span class="layer-file">BasePage.java</span>
          <span class="layer-file">BaseTest.java</span>
        </div>
      </div>
      <div class="layer layer-utils">
        <div class="layer-icon">🛠</div>
        <div class="layer-content">
          <h4>Layer 2 — Utilities Layer</h4>
          <p>Cross-cutting concerns: reports, screenshots, waits, test data generation, logging.</p>
        </div>
        <div class="layer-files">
          <span class="layer-file">ExtentReportManager</span>
          <span class="layer-file">ScreenshotUtil</span>
          <span class="layer-file">TestDataUtil</span>
          <span class="layer-file">WaitUtil</span>
        </div>
      </div>
      <div class="layer layer-driver">
        <div class="layer-icon">🌐</div>
        <div class="layer-content">
          <h4>Layer 1 — Driver Layer</h4>
          <p>ThreadLocal WebDriver lifecycle — init, configure, and quit per test thread. Supports Chrome, Firefox, Edge.</p>
        </div>
        <div class="layer-files">
          <span class="layer-file">DriverManager.java</span>
          <span class="layer-file">ConfigReader.java</span>
        </div>
      </div>
    </div>
  </section>

  <!-- PAGE OBJECT MODEL -->
  <section id="pom">
    <div class="section-header">
      <div class="section-icon icon-green">📄</div>
      <div class="section-title-group">
        <h2>Page Object Model (POM)</h2>
        <p>Every web page is represented by a dedicated Java class. Locators and actions live in one place — changing a locator never breaks tests.</p>
      </div>
    </div>

    <ul class="feature-list">
      <li><strong>@FindBy annotations</strong> with PageFactory — keeps locators cleanly declared at the top of each page class</li>
      <li><strong>Fluent method chaining</strong> — page actions return <code>this</code> or the next page, enabling readable one-liner flows</li>
      <li><strong>Separation of concerns</strong> — tests only call high-level actions like <code>loginAs()</code>, never raw <code>driver.findElement()</code></li>
      <li><strong>Easy maintenance</strong> — when a locator changes in the UI, you update exactly one file, not every test that used it</li>
      <li><strong>Page chaining</strong> — <code>LoginPage.clickLogin()</code> returns a <code>DashboardPage</code>, modeling the real navigation flow</li>
    </ul>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">JAVA — LoginPage.java (POM example)</span>
      </div>
      <div class="code-body"><span class="ann">@FindBy</span>(id = <span class="str">"username"</span>)
<span class="kw">private</span> <span class="cls">WebElement</span> usernameInput;

<span class="ann">@FindBy</span>(css = <span class="str">"button[type='submit']"</span>)
<span class="kw">private</span> <span class="cls">WebElement</span> loginButton;

<span class="cmt">// Fluent chain — test reads like a user story</span>
<span class="kw">public</span> <span class="cls">DashboardPage</span> <span class="fn">loginAs</span>(<span class="cls">String</span> user, <span class="cls">String</span> pass) {
    <span class="kw">return</span> <span class="fn">enterUsername</span>(user)
        .<span class="fn">enterPassword</span>(pass)
        .<span class="fn">clickLogin</span>();   <span class="cmt">// returns DashboardPage</span>
}</div>
    </div>
  </section>

  <!-- DRIVER MANAGEMENT -->
  <section id="driver">
    <div class="section-header">
      <div class="section-icon icon-blue">🌐</div>
      <div class="section-title-group">
        <h2>Driver Management</h2>
        <p>DriverManager uses ThreadLocal to give each parallel test thread its own isolated WebDriver instance.</p>
      </div>
    </div>

    <div class="card-grid">
      <div class="card">
        <div class="card-icon">🧵</div>
        <h3>ThreadLocal WebDriver</h3>
        <p>Each test thread gets its own <code>WebDriver</code> stored in <code>ThreadLocal&lt;WebDriver&gt;</code>. Parallel tests never share or interfere with each other's browser instance.</p>
        <span class="card-tag">Thread-Safe</span>
      </div>
      <div class="card">
        <div class="card-icon">🔄</div>
        <h3>Auto Driver Binaries</h3>
        <p>WebDriverManager automatically downloads the correct ChromeDriver, GeckoDriver, or EdgeDriver for the installed browser version. No manual binary management ever.</p>
        <span class="card-tag">WebDriverManager 5.x</span>
      </div>
      <div class="card">
        <div class="card-icon">🖥</div>
        <h3>Multi-Browser Support</h3>
        <p>Switch browsers with a single flag: <code>-Dbrowser=firefox</code>. Supported: Chrome, Firefox, Edge. Each has its own options configured (headless, window size, sandbox).</p>
        <span class="card-tag">Chrome · Firefox · Edge</span>
      </div>
      <div class="card">
        <div class="card-icon">👻</div>
        <h3>Headless Mode</h3>
        <p>Pass <code>-Dheadless=true</code> to run all browsers without opening a window. Essential for CI/CD pipelines where no display server is available.</p>
        <span class="card-tag">CI/CD Ready</span>
      </div>
    </div>
  </section>

  <!-- WAITS -->
  <section id="waits">
    <div class="section-header">
      <div class="section-icon icon-yellow">⏳</div>
      <div class="section-title-group">
        <h2>Smart Wait Strategy</h2>
        <p>The framework uses a layered wait strategy — no hard-coded <code>Thread.sleep()</code> in tests.</p>
      </div>
    </div>

    <table class="data-table">
      <thead><tr><th>Wait Type</th><th>Where Used</th><th>Configured Via</th></tr></thead>
      <tbody>
        <tr><td>Implicit Wait</td><td>Global fallback on every <code>findElement</code> call</td><td><code>implicit.wait</code> in config.properties</td></tr>
        <tr><td>Explicit Wait (WebDriverWait)</td><td>In BasePage for all click/type/visible methods</td><td><code>explicit.wait</code> in config.properties</td></tr>
        <tr><td>Page Load Timeout</td><td>Browser navigation timeout</td><td><code>page.load.timeout</code> in config.properties</td></tr>
        <tr><td>WaitUtil (static)</td><td>Custom conditions from any utility class</td><td>Per-call timeout override</td></tr>
      </tbody>
    </table>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">JAVA — BasePage wait methods</span>
      </div>
      <div class="code-body"><span class="cmt">// Waits up to explicit.wait seconds for element to be visible</span>
<span class="kw">public</span> <span class="cls">WebElement</span> <span class="fn">waitForVisible</span>(<span class="cls">By</span> locator) {
    <span class="kw">return</span> wait.<span class="fn">until</span>(<span class="cls">ExpectedConditions</span>.<span class="fn">visibilityOfElementLocated</span>(locator));
}

<span class="cmt">// Waits for URL to contain fragment before proceeding</span>
<span class="kw">public</span> <span class="kw">void</span> <span class="fn">waitForUrl</span>(<span class="cls">String</span> urlFragment) {
    wait.<span class="fn">until</span>(<span class="cls">ExpectedConditions</span>.<span class="fn">urlContains</span>(urlFragment));
}</div>
    </div>
  </section>

  <!-- CONFIG -->
  <section id="config">
    <div class="section-header">
      <div class="section-icon icon-green">⚙️</div>
      <div class="section-title-group">
        <h2>Environment-Aware Configuration</h2>
        <p>ConfigReader is a thread-safe singleton that loads properties from file and allows any value to be overridden from the command line.</p>
      </div>
    </div>

    <ul class="feature-list">
      <li><strong>Singleton pattern</strong> — ConfigReader is initialized once and shared safely across all threads</li>
      <li><strong>Environment layering</strong> — <code>config.properties</code> sets defaults, <code>config-staging.properties</code> overrides only what changes</li>
      <li><strong>System property override</strong> — any property can be overridden from Maven: <code>-Dbase.url=https://myapp.com</code></li>
      <li><strong>Typed accessors</strong> — <code>getInt()</code>, <code>getBoolean()</code>, <code>get()</code> with default value fallback</li>
      <li><strong>Zero config changes to switch envs</strong> — just pass <code>-Denv=staging</code> at runtime</li>
    </ul>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">PROPERTIES — config.properties</span>
      </div>
      <div class="code-body">base.url=https://the-internet.herokuapp.com
app.username=tomsmith
app.password=SuperSecretPassword!
browser=chrome
headless=false
implicit.wait=5
explicit.wait=15
page.load.timeout=30</div>
    </div>
  </section>

  <!-- TEST DATA -->
  <section id="data">
    <div class="section-header">
      <div class="section-icon icon-purple">🎲</div>
      <div class="section-title-group">
        <h2>Test Data Generation</h2>
        <p>TestDataUtil wraps JavaFaker to produce realistic, randomized data on every test run — no hardcoded names or emails.</p>
      </div>
    </div>

    <div class="card-grid">
      <div class="card">
        <div class="card-icon">👤</div>
        <h3>Random User Profiles</h3>
        <p><code>generateUser()</code> returns a full map with firstName, lastName, email, phone, password, city, and country — all realistic and random each run.</p>
      </div>
      <div class="card">
        <div class="card-icon">📋</div>
        <h3>Form Data Sets</h3>
        <p><code>generateFormData()</code> builds a complete set of form field values, including multi-line comments, ready to pass directly into page object methods.</p>
      </div>
      <div class="card">
        <div class="card-icon">🔒</div>
        <h3>Security Payloads</h3>
        <p>Built-in <code>sqlInjectionPayload()</code> and <code>xssPayload()</code> methods for security testing — verifies the app doesn't crash or succeed on malicious input.</p>
      </div>
      <div class="card">
        <div class="card-icon">📏</div>
        <h3>Boundary Values</h3>
        <p><code>longString(255)</code> generates max-length strings for boundary testing. Verifies field limits, database column lengths, and UI truncation behavior.</p>
      </div>
    </div>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">JAVA — TestDataUtil usage in a test</span>
      </div>
      <div class="code-body"><span class="cls">Map</span>&lt;<span class="cls">String</span>, <span class="cls">String</span>&gt; data = <span class="cls">TestDataUtil</span>.<span class="fn">generateUser</span>();

formPage.<span class="fn">fillAndSubmit</span>(
    data.<span class="fn">get</span>(<span class="str">"firstName"</span>),   <span class="cmt">// e.g. "Emily"</span>
    data.<span class="fn">get</span>(<span class="str">"lastName"</span>),    <span class="cmt">// e.g. "Carrington"</span>
    data.<span class="fn">get</span>(<span class="str">"email"</span>),       <span class="cmt">// e.g. "emily.c@fakeemail.net"</span>
    data.<span class="fn">get</span>(<span class="str">"phone"</span>),       <span class="cmt">// e.g. "(555) 821-4492"</span>
    <span class="str">"United States"</span>, <span class="str">"female"</span>, <span class="str">"Automated entry"</span>
);</div>
    </div>
  </section>

  <!-- REPORTING -->
  <section id="reporting">
    <div class="section-header">
      <div class="section-icon icon-orange">📊</div>
      <div class="section-title-group">
        <h2>Test Reporting</h2>
        <p>Two complementary reports generated automatically after every run — no manual setup required.</p>
      </div>
    </div>

    <div class="card-grid card-grid-2">
      <div class="card">
        <div class="card-icon">🌐</div>
        <h3>ExtentReports HTML Report</h3>
        <p>A rich, interactive dark-themed HTML report saved to <code>reports/TestReport_&lt;timestamp&gt;.html</code>. Includes: pass/fail/skip counts with donut chart, per-test step logs, failure screenshots embedded inline, system info panel (browser, OS, Java, env), and test duration timings. Opens in any browser — shareable with the whole team.</p>
        <span class="card-tag">reports/TestReport_*.html</span>
      </div>
      <div class="card">
        <div class="card-icon">📋</div>
        <h3>TestNG Native XML Report</h3>
        <p>Standard <code>testng-results.xml</code> generated by Maven Surefire to <code>target/surefire-reports/</code>. Used by Jenkins Test Results plugin, GitHub Actions test-reporter, and most CI dashboards for trend graphs across builds. Also readable as an HTML index file.</p>
        <span class="card-tag">target/surefire-reports/</span>
      </div>
    </div>
  </section>

  <!-- LOGGING -->
  <section id="logging">
    <div class="section-header">
      <div class="section-icon icon-blue">📝</div>
      <div class="section-title-group">
        <h2>Structured Logging</h2>
        <p>Log4j2 configured with three appenders — each serving a distinct purpose in debugging and monitoring.</p>
      </div>
    </div>

    <table class="data-table">
      <thead><tr><th>Appender</th><th>Level</th><th>Output</th><th>Purpose</th></tr></thead>
      <tbody>
        <tr><td>Console</td><td>INFO+</td><td>Terminal / stdout</td><td>Real-time test progress during local runs</td></tr>
        <tr><td>FileAppender</td><td>DEBUG+</td><td>reports/logs/automation.log</td><td>Full debug trail — every action logged for failure investigation</td></tr>
        <tr><td>ErrorAppender</td><td>ERROR only</td><td>reports/logs/errors.log</td><td>Isolated error log for quick post-run failure scanning</td></tr>
      </tbody>
    </table>

    <ul class="feature-list">
      <li><strong>Rolling file strategy</strong> — log files rotate at 10 MB and compress old files to .gz, preventing disk bloat on long CI runs</li>
      <li><strong>Package-level control</strong> — Selenium and WebDriverManager logs are silenced at WARN level to reduce noise in output</li>
      <li><strong>Timestamped entries</strong> — every line includes millisecond-precision timestamp and thread name for parallel run analysis</li>
      <li><strong>logStep() / logPass() / logFail()</strong> — BaseTest helpers write to both Log4j2 AND the ExtentReport simultaneously</li>
    </ul>
  </section>

  <!-- SCREENSHOTS -->
  <section id="screenshots">
    <div class="section-header">
      <div class="section-icon icon-purple">📸</div>
      <div class="section-title-group">
        <h2>Automatic Screenshot Capture</h2>
        <p>Screenshots are captured automatically on test failure — no manual calls needed in test code.</p>
      </div>
    </div>

    <ul class="feature-list">
      <li><strong>Auto-triggered on failure</strong> — BaseTest's <code>@AfterMethod</code> detects <code>ITestResult.FAILURE</code> and calls ScreenshotUtil automatically</li>
      <li><strong>Timestamped filenames</strong> — files are named <code>TestName_yyyyMMdd_HHmmss.png</code>, preventing overwrites across runs</li>
      <li><strong>Embedded in HTML report</strong> — the screenshot path is added to ExtentReports, visible inline when expanding the failed test</li>
      <li><strong>Safe handling</strong> — screenshot failure never crashes the teardown; exceptions are caught and logged as warnings</li>
      <li><strong>Configurable directory</strong> — output path controlled by <code>screenshot.dir</code> in config.properties</li>
    </ul>
  </section>

  <!-- RETRY & LISTENERS -->
  <section id="retry">
    <div class="section-header">
      <div class="section-icon icon-orange">🔁</div>
      <div class="section-title-group">
        <h2>Retry Analyzer & Listeners</h2>
        <p>Two TestNG extension points that improve reliability and visibility across the entire suite.</p>
      </div>
    </div>

    <div class="card-grid">
      <div class="card">
        <div class="card-icon">🔁</div>
        <h3>RetryAnalyzer</h3>
        <p>Implements <code>IRetryAnalyzer</code> — retries any failed test up to 2 times before marking it as a final failure. Each retry attempt is logged with the attempt number. Eliminates false failures from network flakiness or timing issues. Attach per-test with <code>retryAnalyzer = RetryAnalyzer.class</code>.</p>
      </div>
      <div class="card">
        <div class="card-icon">👂</div>
        <h3>TestListener</h3>
        <p>Implements both <code>ITestListener</code> and <code>ISuiteListener</code>. Logs ✅ / ❌ / ⏭ with emoji on every test event. Prints a summary table at suite end: total tests, passed, failed, skipped. Hooks into ExtentReports to record Status on every event without touching test code.</p>
      </div>
    </div>
  </section>

  <!-- PARALLEL -->
  <section id="parallel">
    <div class="section-header">
      <div class="section-icon icon-green">⚡</div>
      <div class="section-title-group">
        <h2>Parallel Test Execution</h2>
        <p>The framework is architected from the ground up to support parallel execution safely.</p>
      </div>
    </div>

    <ul class="feature-list">
      <li><strong>ThreadLocal WebDriver</strong> — each thread gets its own browser; <code>DriverManager.getDriver()</code> always returns the thread-local instance</li>
      <li><strong>ThreadLocal ExtentTest</strong> — each test thread logs to its own report node with no cross-contamination</li>
      <li><strong>testng.xml parallel config</strong> — <code>parallel="methods" thread-count="3"</code> runs up to 3 tests simultaneously out of the box</li>
      <li><strong>No static driver references</strong> — the framework never stores <code>driver</code> in a static field, which would cause race conditions</li>
      <li><strong>Scalable</strong> — increase <code>thread-count</code> in testng.xml to match the number of available browser instances on the machine or CI agent</li>
    </ul>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">XML — testng.xml parallel configuration</span>
      </div>
      <div class="code-body">&lt;<span class="kw">suite</span> name=<span class="str">"Selenium Automation Suite"</span>
       <span class="cls">parallel</span>=<span class="str">"methods"</span>
       <span class="cls">thread-count</span>=<span class="str">"3"</span>
       <span class="cls">verbose</span>=<span class="str">"2"</span>&gt;</div>
    </div>
  </section>

  <!-- LOGIN TESTS -->
  <section id="tests-login">
    <div class="section-header">
      <div class="section-icon icon-green">🔐</div>
      <div class="section-title-group">
        <h2>Login Test Coverage</h2>
        <p>LoginTest covers the full spectrum of authentication scenarios including security edge cases.</p>
      </div>
    </div>

    <table class="data-table">
      <thead><tr><th>Test Method</th><th>Group</th><th>What It Verifies</th></tr></thead>
      <tbody>
        <tr><td>testSuccessfulLogin</td><td>smoke, regression</td><td>Valid credentials → dashboard URL contains /secure, logout link visible</td></tr>
        <tr><td>testLoginSuccessBannerDisplayed</td><td>regression</td><td>Success flash message appears and contains "logged in"</td></tr>
        <tr><td>testLoginWithInvalidCredentials</td><td>smoke, regression</td><td>Wrong creds → error message shown, stays on /login</td></tr>
        <tr><td>testLoginWithEmptyUsername</td><td>regression</td><td>Empty username → error shown, no navigation</td></tr>
        <tr><td>testLoginWithEmptyPassword</td><td>regression</td><td>Empty password → error shown</td></tr>
        <tr><td>testLogout</td><td>regression</td><td>Login → logout → redirected back to login page</td></tr>
        <tr><td>testVariousInvalidCredentials</td><td>regression, security</td><td>7-row DataProvider: wrong user, XSS, SQL injection, 1000-char username, empty, common defaults</td></tr>
      </tbody>
    </table>
  </section>

  <!-- FORM TESTS -->
  <section id="tests-form">
    <div class="section-header">
      <div class="section-icon icon-blue">📝</div>
      <div class="section-title-group">
        <h2>Form Test Coverage</h2>
        <p>FormTest validates all input types, field constraints, and security handling.</p>
      </div>
    </div>

    <table class="data-table">
      <thead><tr><th>Test Method</th><th>Group</th><th>What It Verifies</th></tr></thead>
      <tbody>
        <tr><td>testValidFormSubmission</td><td>smoke, regression</td><td>All fields filled with Faker data → success message appears</td></tr>
        <tr><td>testEmptyFirstName</td><td>regression</td><td>Missing required field → form rejected or error shown</td></tr>
        <tr><td>testInvalidEmailFormat</td><td>regression</td><td>Non-email string → form does not succeed</td></tr>
        <tr><td>testCountryDropdownSelection</td><td>regression</td><td>Select dropdown item by visible text without error</td></tr>
        <tr><td>testTermsCheckboxRequired</td><td>regression</td><td>Submitting without checkbox → form blocked</td></tr>
        <tr><td>testMaxLengthFirstName</td><td>regression</td><td>255-char input → page remains stable, no crash</td></tr>
        <tr><td>testXssInFirstName</td><td>regression, security</td><td>&lt;script&gt; payload → page title not altered, no JS executed</td></tr>
        <tr><td>testSqlInjectionInEmail</td><td>regression, security</td><td>SQL payload in email → form not submitted successfully</td></tr>
        <tr><td>testFormWithMultipleValidDatasets</td><td>regression</td><td>3-row DataProvider: different names/countries/genders all succeed</td></tr>
      </tbody>
    </table>
  </section>

  <!-- E2E TESTS -->
  <section id="tests-e2e">
    <div class="section-header">
      <div class="section-icon icon-yellow">🚀</div>
      <div class="section-title-group">
        <h2>End-to-End Workflow Tests</h2>
        <p>UserWorkflowTest simulates real user journeys across multiple pages — the closest thing to a real user's experience.</p>
      </div>
    </div>

    <div class="flow">
      <div class="flow-step">
        <div class="step-icon">🔑</div>
        <div class="step-name">Login</div>
        <div class="step-desc">Valid creds</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="step-icon">📋</div>
        <div class="step-name">Dashboard</div>
        <div class="step-desc">Verify state</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="step-icon">📝</div>
        <div class="step-name">Fill Form</div>
        <div class="step-desc">Random data</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="step-icon">✅</div>
        <div class="step-name">Submit</div>
        <div class="step-desc">Verify success</div>
      </div>
      <div class="flow-arrow">→</div>
      <div class="flow-step">
        <div class="step-icon">🚪</div>
        <div class="step-name">Logout</div>
        <div class="step-desc">Session ends</div>
      </div>
    </div>

    <ul class="feature-list" style="margin-top:16px">
      <li><strong>Login → Dashboard → Logout</strong> — full session lifecycle with heading verification at each step</li>
      <li><strong>Login → Form → Submit</strong> — crosses two page boundaries with random data, verifies success</li>
      <li><strong>Session management</strong> — after logout, directly accessing <code>/secure</code> URL should redirect to login</li>
      <li><strong>Browser back after logout</strong> — verifies session is truly destroyed even if browser history is used</li>
      <li><strong>Page title verification</strong> — checks both login and dashboard page titles are present and non-empty</li>
    </ul>
  </section>

  <!-- GROUPS -->
  <section id="groups">
    <div class="section-header">
      <div class="section-icon icon-green">🏷</div>
      <div class="section-title-group">
        <h2>Test Groups & Suites</h2>
        <p>Run exactly what you need — from a 30-second smoke check to a full 4-suite regression.</p>
      </div>
    </div>

    <div class="group-grid">
      <div class="group-row">
        <div class="group-name grp-smoke">smoke</div>
        <div class="group-desc">Core sanity checks — runs in under 2 minutes. Run on every commit to catch critical regressions immediately. Includes happy-path login and basic E2E flow.</div>
        <div class="group-count">-Dgroups=smoke</div>
      </div>
      <div class="group-row">
        <div class="group-name grp-reg">regression</div>
        <div class="group-desc">Full functional coverage — all positive, negative, boundary, and data-driven tests across all pages. Run before every release or nightly via cron.</div>
        <div class="group-count">-Dgroups=regression</div>
      </div>
      <div class="group-row">
        <div class="group-name grp-e2e">e2e</div>
        <div class="group-desc">Complete user journeys across multiple pages. Slower but most realistic — validates that the entire application flow works together end-to-end.</div>
        <div class="group-count">-Dgroups=e2e</div>
      </div>
      <div class="group-row">
        <div class="group-name grp-sec">security</div>
        <div class="group-desc">XSS payloads, SQL injection strings, oversized inputs, and default credential attacks. Run before security audits or after any input-handling changes.</div>
        <div class="group-count">-Dgroups=security</div>
      </div>
    </div>
  </section>

  <!-- JENKINS -->
  <section id="jenkins">
    <div class="section-header">
      <div class="section-icon icon-orange">🔧</div>
      <div class="section-title-group">
        <h2>Jenkins CI/CD Pipeline</h2>
        <p>A full declarative Jenkinsfile with parameterized builds, reports, and email notifications.</p>
      </div>
    </div>

    <ul class="feature-list">
      <li><strong>Parameterized builds</strong> — BROWSER, ENV, SUITE, HEADLESS are all selectable from the Jenkins UI on each run</li>
      <li><strong>Nightly cron trigger</strong> — <code>cron('0 2 * * *')</code> runs full regression automatically at 2 AM every night</li>
      <li><strong>5 pipeline stages</strong> — Checkout → Build → Static Analysis → Run Tests → Publish Reports → Notify</li>
      <li><strong>TestNG results plugin</strong> — publishes test results to Jenkins for trend graphs and failure history</li>
      <li><strong>HTML Extent report</strong> — published as a Jenkins HTML artifact, linked from the build page</li>
      <li><strong>Email notifications</strong> — sends ✅ or ❌ email with build URL, branch, suite, and browser after every run</li>
      <li><strong>Screenshot archiving</strong> — failure screenshots and log files are archived as Jenkins build artifacts</li>
      <li><strong>Workspace cleanup</strong> — <code>cleanWs()</code> in post-always ensures no stale files pollute the next build</li>
    </ul>
  </section>

  <!-- GITHUB ACTIONS -->
  <section id="github">
    <div class="section-header">
      <div class="section-icon icon-purple">🐙</div>
      <div class="section-title-group">
        <h2>GitHub Actions Pipeline</h2>
        <p>A complete <code>ci.yml</code> workflow that runs on push, pull request, schedule, and manual trigger.</p>
      </div>
    </div>

    <ul class="feature-list">
      <li><strong>Trigger on push to main/develop</strong> — every push runs smoke tests automatically, catching regressions at the source</li>
      <li><strong>Trigger on pull requests</strong> — PRs targeting main are gated on passing tests before merge</li>
      <li><strong>Nightly schedule</strong> — cron at 01:00 UTC runs full regression, independent of commits</li>
      <li><strong>workflow_dispatch</strong> — manual trigger from GitHub UI with dropdown selectors for browser, suite, and environment</li>
      <li><strong>Maven dependency caching</strong> — <code>actions/cache</code> caches <code>~/.m2</code> between runs for fast builds</li>
      <li><strong>Chrome setup</strong> — <code>browser-actions/setup-chrome</code> installs stable Chrome on the Ubuntu runner</li>
      <li><strong>Test report artifact</strong> — entire <code>reports/</code> folder uploaded as a downloadable artifact, retained for 14 days</li>
      <li><strong>Failure screenshots artifact</strong> — screenshots uploaded separately on failure only, retained for 7 days</li>
      <li><strong>TestNG reporter integration</strong> — <code>dorny/test-reporter</code> renders TestNG results as a native GitHub check</li>
    </ul>
  </section>

  <!-- RUN COMMANDS -->
  <section id="commands">
    <div class="section-header">
      <div class="section-icon icon-blue">💻</div>
      <div class="section-title-group">
        <h2>All Run Commands</h2>
        <p>Every way to execute the framework from the command line.</p>
      </div>
    </div>

    <div class="cmd-grid">
      <div class="cmd-row">
        <div class="cmd-label">Default (smoke)</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn clean test</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Smoke tests</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dgroups=smoke</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Full regression</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dgroups=regression</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">E2E only</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dgroups=e2e</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Security tests</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dgroups=security</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Firefox browser</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dbrowser=firefox</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Headless mode</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dheadless=true</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Staging env</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Denv=staging</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Single test class</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dtest=LoginTest</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Single test method</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dtest=LoginTest#testSuccessfulLogin</div>
      </div>
      <div class="cmd-row">
        <div class="cmd-label">Full CI combo</div>
        <div class="cmd-code"><span class="cmd-dollar">$</span>mvn test -Dbrowser=chrome -Dheadless=true -Denv=staging -Dgroups=regression</div>
      </div>
    </div>
  </section>

  <!-- ADD A PAGE -->
  <section id="add-page">
    <div class="section-header">
      <div class="section-icon icon-green">➕</div>
      <div class="section-title-group">
        <h2>Adding a New Page Object</h2>
        <p>Three steps to add any new page to the framework — takes about 5 minutes.</p>
      </div>
    </div>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">JAVA — New page template</span>
      </div>
      <div class="code-body"><span class="kw">package</span> com.automation.pages;

<span class="kw">import</span> com.automation.base.BasePage;
<span class="kw">import</span> org.openqa.selenium.*;
<span class="kw">import</span> org.openqa.selenium.support.FindBy;

<span class="kw">public class</span> <span class="cls">ProductPage</span> <span class="kw">extends</span> <span class="cls">BasePage</span> {

    <span class="cmt">// Step 1: Declare locators with @FindBy</span>
    <span class="ann">@FindBy</span>(css = <span class="str">".add-to-cart-btn"</span>)
    <span class="kw">private</span> <span class="cls">WebElement</span> addToCartButton;

    <span class="ann">@FindBy</span>(css = <span class="str">".product-title"</span>)
    <span class="kw">private</span> <span class="cls">WebElement</span> productTitle;

    <span class="cmt">// Step 2: Constructor</span>
    <span class="kw">public</span> <span class="cls">ProductPage</span>(<span class="cls">WebDriver</span> driver) {
        <span class="kw">super</span>(driver); <span class="cmt">// PageFactory initialized in BasePage</span>
    }

    <span class="cmt">// Step 3: Fluent actions</span>
    <span class="kw">public</span> <span class="cls">CartPage</span> <span class="fn">addToCart</span>() {
        <span class="fn">click</span>(addToCartButton);
        <span class="kw">return new</span> <span class="cls">CartPage</span>(driver);
    }

    <span class="kw">public</span> <span class="cls">String</span> <span class="fn">getTitle</span>() {
        <span class="kw">return</span> <span class="fn">getText</span>(productTitle);
    }
}</div>
    </div>
  </section>

  <!-- ADD A TEST -->
  <section id="add-test">
    <div class="section-header">
      <div class="section-icon icon-blue">➕</div>
      <div class="section-title-group">
        <h2>Adding a New Test</h2>
        <p>Extend BaseTest, use the page objects, assert, and add the class to testng.xml.</p>
      </div>
    </div>

    <div class="code-block">
      <div class="code-header">
        <div class="dots"><div class="dot dot-r"></div><div class="dot dot-y"></div><div class="dot dot-g"></div></div>
        <span class="code-lang">JAVA — New test class template</span>
      </div>
      <div class="code-body"><span class="kw">public class</span> <span class="cls">ProductTest</span> <span class="kw">extends</span> <span class="cls">BaseTest</span> {

    <span class="ann">@Test</span>(
        groups      = {<span class="str">"smoke"</span>, <span class="str">"regression"</span>},
        description = <span class="str">"Verify product can be added to cart"</span>
    )
    <span class="kw">public void</span> <span class="fn">testAddProductToCart</span>() {
        logStep(<span class="str">"Step 1: Login"</span>);
        <span class="kw">new</span> <span class="cls">LoginPage</span>(driver).<span class="fn">open</span>()
            .<span class="fn">loginAs</span>(config.<span class="fn">getUsername</span>(), config.<span class="fn">getPassword</span>());

        logStep(<span class="str">"Step 2: Navigate to product"</span>);
        <span class="cls">ProductPage</span> product = <span class="kw">new</span> <span class="cls">ProductPage</span>(driver).<span class="fn">open</span>();

        logStep(<span class="str">"Step 3: Add to cart"</span>);
        <span class="cls">CartPage</span> cart = product.<span class="fn">addToCart</span>();

        logStep(<span class="str">"Step 4: Verify"</span>);
        <span class="cls">Assert</span>.<span class="fn">assertTrue</span>(cart.<span class="fn">hasItems</span>(), <span class="str">"Cart should have items"</span>);
        logPass(<span class="str">"Add to cart verified ✓"</span>);
    }
}</div>
    </div>

    <p style="color: var(--muted); font-size: 13px; margin-top: 16px;">Then add <code style="color: var(--accent2); font-family: JetBrains Mono, monospace; font-size: 12px;">&lt;class name="com.automation.tests.ProductTest"/&gt;</code> to the relevant suite in <code style="color: var(--accent2); font-family: JetBrains Mono, monospace; font-size: 12px;">testng.xml</code>.</p>
  </section>

  <footer class="footer">
    Built with <span>Java 11 · Selenium WebDriver 4 · TestNG 7 · ExtentReports 5</span><br>
    <span style="color: var(--muted); font-size: 12px; margin-top: 6px; display: block;">Selenium TestNG Automation Framework · v1.0.0</span>
  </footer>

</main>

<script>
  // Active nav highlight on scroll
  const sections = document.querySelectorAll('section[id], div[id]');
  const navLinks = document.querySelectorAll('.nav-link');

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        navLinks.forEach(l => l.classList.remove('active'));
        const active = document.querySelector(`.nav-link[href="#${entry.target.id}"]`);
        if (active) active.classList.add('active');
      }
    });
  }, { rootMargin: '-20% 0px -70% 0px' });

  sections.forEach(s => observer.observe(s));
</script>
</body>
</html>
