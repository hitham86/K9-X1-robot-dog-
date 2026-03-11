<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>K9-X1 — Robot Dog Project</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #050810;
    --surface: #0a0f1e;
    --card: #0d1428;
    --border: #1a2a4a;
    --accent: #00d4ff;
    --accent2: #ff6b00;
    --accent3: #00ff88;
    --text: #c8d8f0;
    --muted: #4a6080;
    --glow: 0 0 20px rgba(0, 212, 255, 0.3);
    --glow2: 0 0 20px rgba(255, 107, 0, 0.3);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Rajdhani', sans-serif;
    font-size: 16px;
    line-height: 1.6;
    overflow-x: hidden;
  }

  /* Animated grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0, 212, 255, 0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0, 212, 255, 0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    z-index: 0;
    animation: gridShift 20s linear infinite;
  }

  @keyframes gridShift {
    0% { transform: perspective(500px) rotateX(0deg) translateY(0); }
    100% { transform: perspective(500px) rotateX(0deg) translateY(40px); }
  }

  .container {
    max-width: 960px;
    margin: 0 auto;
    padding: 0 24px;
    position: relative;
    z-index: 1;
  }

  /* ── HERO ── */
  .hero {
    padding: 80px 0 60px;
    text-align: center;
    position: relative;
  }

  .hero-dog {
    font-size: 120px;
    display: block;
    margin-bottom: 16px;
    filter: drop-shadow(0 0 30px rgba(0, 212, 255, 0.6));
    animation: float 4s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-12px); }
  }

  .badge-row {
    display: flex;
    gap: 8px;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 32px;
  }

  .badge {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    padding: 4px 12px;
    border-radius: 2px;
    border: 1px solid;
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }

  .badge-cyan { border-color: var(--accent); color: var(--accent); background: rgba(0,212,255,0.08); }
  .badge-orange { border-color: var(--accent2); color: var(--accent2); background: rgba(255,107,0,0.08); }
  .badge-green { border-color: var(--accent3); color: var(--accent3); background: rgba(0,255,136,0.08); }

  .hero-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(36px, 7vw, 72px);
    font-weight: 900;
    letter-spacing: 0.08em;
    line-height: 1;
    margin-bottom: 8px;
    background: linear-gradient(135deg, #00d4ff 0%, #ffffff 50%, #ff6b00 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-sub {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--muted);
    letter-spacing: 0.2em;
    margin-bottom: 24px;
    text-transform: uppercase;
  }

  .hero-desc {
    font-size: 18px;
    color: var(--text);
    max-width: 600px;
    margin: 0 auto 40px;
    font-weight: 300;
    line-height: 1.7;
  }

  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent), var(--accent2), transparent);
    margin: 0 0 60px;
    opacity: 0.4;
  }

  /* ── STAT STRIP ── */
  .stat-strip {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1px;
    background: var(--border);
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 60px;
  }

  .stat {
    background: var(--card);
    padding: 24px 16px;
    text-align: center;
    position: relative;
    overflow: hidden;
    transition: background 0.3s;
  }

  .stat:hover { background: #111d35; }

  .stat::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--accent);
    transform: scaleX(0);
    transition: transform 0.3s;
  }

  .stat:hover::before { transform: scaleX(1); }

  .stat-num {
    font-family: 'Orbitron', monospace;
    font-size: 28px;
    font-weight: 700;
    color: var(--accent);
    display: block;
    line-height: 1;
    margin-bottom: 6px;
  }

  .stat-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.15em;
  }

  /* ── SECTION ── */
  .section { margin-bottom: 60px; }

  .section-header {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 28px;
  }

  .section-icon {
    width: 36px;
    height: 36px;
    border: 1px solid var(--accent);
    border-radius: 3px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    background: rgba(0,212,255,0.06);
    flex-shrink: 0;
    box-shadow: var(--glow);
  }

  .section-title {
    font-family: 'Orbitron', monospace;
    font-size: 18px;
    font-weight: 700;
    color: var(--accent);
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  .section-line {
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--border), transparent);
  }

  /* ── CARDS ── */
  .card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 24px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, transform 0.2s;
  }

  .card:hover {
    border-color: rgba(0, 212, 255, 0.3);
    transform: translateY(-2px);
  }

  .card::after {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(0,212,255,0.03), transparent);
    pointer-events: none;
  }

  /* ── HARDWARE GRID ── */
  .hw-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
  }

  .hw-item {
    display: flex;
    gap: 14px;
    align-items: flex-start;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 16px;
    transition: border-color 0.3s;
  }

  .hw-item:hover { border-color: rgba(0, 212, 255, 0.25); }

  .hw-icon {
    font-size: 22px;
    line-height: 1;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .hw-name {
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    color: var(--accent);
    margin-bottom: 3px;
  }

  .hw-spec {
    font-size: 13px;
    color: var(--muted);
    line-height: 1.5;
  }

  /* ── CODE BLOCK ── */
  .code-block {
    background: #020509;
    border: 1px solid var(--border);
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 20px;
  }

  .code-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 16px;
    background: var(--surface);
    border-bottom: 1px solid var(--border);
  }

  .code-lang {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    color: var(--accent3);
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }

  .code-dots {
    display: flex;
    gap: 6px;
  }

  .code-dots span {
    width: 8px; height: 8px;
    border-radius: 50%;
    display: block;
  }

  pre {
    padding: 20px;
    overflow-x: auto;
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
    line-height: 1.8;
    color: #8ab0d0;
  }

  .kw { color: #c792ea; }
  .fn { color: #82aaff; }
  .str { color: #c3e88d; }
  .num { color: #f78c6c; }
  .cm { color: #546e8a; font-style: italic; }
  .cl { color: #ffcb6b; }
  .op { color: #89ddff; }
  .var { color: #eeffff; }

  /* ── FEATURE LIST ── */
  .feature-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }

  .feature-item {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 16px;
    text-align: center;
    transition: all 0.3s;
  }

  .feature-item:hover {
    border-color: rgba(255, 107, 0, 0.4);
    box-shadow: var(--glow2);
  }

  .feature-emoji {
    font-size: 28px;
    display: block;
    margin-bottom: 8px;
  }

  .feature-name {
    font-family: 'Orbitron', monospace;
    font-size: 11px;
    color: var(--accent2);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 6px;
  }

  .feature-desc {
    font-size: 13px;
    color: var(--muted);
    line-height: 1.5;
  }

  /* ── TECH STACK ── */
  .tech-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .tech-pill {
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    padding: 6px 14px;
    border-radius: 2px;
    border: 1px solid var(--border);
    color: var(--text);
    background: var(--card);
    letter-spacing: 0.05em;
    transition: all 0.2s;
    cursor: default;
  }

  .tech-pill:hover {
    border-color: var(--accent);
    color: var(--accent);
    background: rgba(0, 212, 255, 0.08);
    transform: translateY(-1px);
  }

  /* ── GAIT TABLE ── */
  .gait-table {
    width: 100%;
    border-collapse: collapse;
    font-family: 'Share Tech Mono', monospace;
    font-size: 13px;
  }

  .gait-table th {
    text-align: left;
    padding: 10px 16px;
    background: rgba(0, 212, 255, 0.08);
    border: 1px solid var(--border);
    color: var(--accent);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    font-size: 11px;
  }

  .gait-table td {
    padding: 10px 16px;
    border: 1px solid var(--border);
    color: var(--text);
    transition: background 0.2s;
  }

  .gait-table tr:hover td { background: rgba(0, 212, 255, 0.04); }

  .tag-green { color: var(--accent3); }
  .tag-orange { color: var(--accent2); }
  .tag-cyan { color: var(--accent); }

  /* ── WIRING DIAGRAM ── */
  .wiring {
    background: #020509;
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 32px;
    text-align: center;
    font-family: 'Share Tech Mono', monospace;
    position: relative;
  }

  .wire-node {
    display: inline-block;
    padding: 10px 20px;
    border-radius: 3px;
    font-size: 12px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .wn-main { border: 2px solid var(--accent); color: var(--accent); background: rgba(0,212,255,0.1); }
  .wn-sub { border: 1px solid var(--accent3); color: var(--accent3); background: rgba(0,255,136,0.06); font-size: 11px; padding: 6px 14px; }
  .wn-leaf { border: 1px solid var(--border); color: var(--muted); font-size: 10px; padding: 5px 12px; }

  .wire-row {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    margin: 12px 0;
    flex-wrap: wrap;
  }

  .wire-arrow { color: var(--border); font-size: 16px; }

  /* ── ROADMAP ── */
  .roadmap {
    position: relative;
    padding-left: 28px;
  }

  .roadmap::before {
    content: '';
    position: absolute;
    left: 7px; top: 8px; bottom: 8px;
    width: 1px;
    background: linear-gradient(180deg, var(--accent), var(--accent2), var(--border));
  }

  .roadmap-item {
    position: relative;
    margin-bottom: 20px;
    padding: 14px 18px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 4px;
  }

  .roadmap-item::before {
    content: '';
    position: absolute;
    left: -24px;
    top: 18px;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    border: 2px solid var(--accent);
    background: var(--bg);
  }

  .roadmap-item.done::before { background: var(--accent); }
  .roadmap-item.inprogress::before { background: var(--accent2); border-color: var(--accent2); }

  .rm-label {
    font-family: 'Share Tech Mono', monospace;
    font-size: 10px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-bottom: 4px;
  }

  .rm-done { color: var(--accent3); }
  .rm-wip { color: var(--accent2); }
  .rm-todo { color: var(--muted); }

  .rm-title {
    font-family: 'Rajdhani', sans-serif;
    font-size: 16px;
    font-weight: 600;
    color: var(--text);
    margin-bottom: 4px;
  }

  .rm-desc {
    font-size: 13px;
    color: var(--muted);
  }

  /* ── FOOTER ── */
  .footer {
    border-top: 1px solid var(--border);
    padding: 40px 0;
    text-align: center;
    margin-top: 20px;
  }

  .footer-dog {
    font-size: 48px;
    display: block;
    margin-bottom: 12px;
    opacity: 0.5;
    animation: float 6s ease-in-out infinite;
  }

  .footer-text {
    font-family: 'Share Tech Mono', monospace;
    font-size: 12px;
    color: var(--muted);
    letter-spacing: 0.15em;
    text-transform: uppercase;
  }

  .footer-text span { color: var(--accent); }

  /* ── ANIMATIONS ── */
  .fade-in {
    opacity: 0;
    transform: translateY(20px);
    animation: fadeIn 0.6s ease forwards;
  }

  @keyframes fadeIn {
    to { opacity: 1; transform: translateY(0); }
  }

  .delay-1 { animation-delay: 0.1s; }
  .delay-2 { animation-delay: 0.2s; }
  .delay-3 { animation-delay: 0.3s; }
  .delay-4 { animation-delay: 0.4s; }

  /* Scanning line effect */
  .hero::after {
    content: '';
    position: absolute;
    left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    top: 0;
    animation: scan 4s ease-in-out infinite;
    opacity: 0.4;
  }

  @keyframes scan {
    0% { top: 0; opacity: 0; }
    10% { opacity: 0.4; }
    90% { opacity: 0.4; }
    100% { top: 100%; opacity: 0; }
  }

  @media (max-width: 680px) {
    .stat-strip { grid-template-columns: repeat(2, 1fr); }
    .hw-grid { grid-template-columns: 1fr; }
    .feature-grid { grid-template-columns: repeat(2, 1fr); }
  }
</style>
</head>
<body>

<div class="container">

  <!-- HERO -->
  <section class="hero fade-in">
    <span class="hero-dog">🤖</span>
    <div class="badge-row">
      <span class="badge badge-cyan">Python 3.11</span>
      <span class="badge badge-orange">ROS2 Humble</span>
      <span class="badge badge-green">Open Source</span>
      <span class="badge badge-cyan">Raspberry Pi 5</span>
      <span class="badge badge-orange">12-DOF</span>
    </div>
    <h1 class="hero-title">K9-X1</h1>
    <p class="hero-sub">Autonomous Quadruped Robot — Build Log &amp; Source Code</p>
    <p class="hero-desc">
      A fully custom-built, 3D-printed quadruped robot dog with onboard computer vision,
      inverse kinematics, and gait planning. Every servo, circuit, and line of code — built from scratch.
    </p>
  </section>

  <div class="divider"></div>

  <!-- STATS -->
  <div class="stat-strip fade-in delay-1">
    <div class="stat">
      <span class="stat-num">12</span>
      <span class="stat-label">Servo DOF</span>
    </div>
    <div class="stat">
      <span class="stat-num">2.1<small style="font-size:16px">kg</small></span>
      <span class="stat-label">Total Weight</span>
    </div>
    <div class="stat">
      <span class="stat-num">~45</span>
      <span class="stat-label">cm Body Length</span>
    </div>
    <div class="stat">
      <span class="stat-num">90<small style="font-size:16px">min</small></span>
      <span class="stat-label">Battery Life</span>
    </div>
  </div>

  <!-- HARDWARE -->
  <section class="section fade-in delay-2">
    <div class="section-header">
      <div class="section-icon">🔧</div>
      <span class="section-title">Hardware</span>
      <div class="section-line"></div>
    </div>
    <div class="hw-grid">
      <div class="hw-item">
        <span class="hw-icon">🧠</span>
        <div>
          <div class="hw-name">Raspberry Pi 5 (8GB)</div>
          <div class="hw-spec">Main compute — runs ROS2, vision pipeline, gait controller</div>
        </div>
      </div>
      <div class="hw-item">
        <span class="hw-icon">⚙️</span>
        <div>
          <div class="hw-name">MG996R Servos × 12</div>
          <div class="hw-spec">3 per leg — hip abduction, hip flexion, knee flex. 10kg·cm torque</div>
        </div>
      </div>
      <div class="hw-item">
        <span class="hw-icon">👁️</span>
        <div>
          <div class="hw-name">OAK-D Lite Depth Cam</div>
          <div class="hw-spec">Stereo depth + neural inference for obstacle detection</div>
        </div>
      </div>
      <div class="hw-item">
        <span class="hw-icon">🎛️</span>
        <div>
          <div class="hw-name">PCA9685 PWM Driver</div>
          <div class="hw-spec">16-channel I²C PWM — drives all servos from single board</div>
        </div>
      </div>
      <div class="hw-item">
        <span class="hw-icon">📡</span>
        <div>
          <div class="hw-name">MPU-6050 IMU</div>
          <div class="hw-spec">6-axis accelerometer + gyroscope for balance feedback</div>
        </div>
      </div>
      <div class="hw-item">
        <span class="hw-icon">🔋</span>
        <div>
          <div class="hw-name">3S LiPo 5000mAh</div>
          <div class="hw-spec">11.1V, regulated via buck converter to 5V / 6V rails</div>
        </div>
      </div>
    </div>
  </section>

  <!-- ARCHITECTURE -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">🏗️</div>
      <span class="section-title">System Architecture</span>
      <div class="section-line"></div>
    </div>
    <div class="wiring">
      <div class="wire-row">
        <span class="wire-node wn-main">Raspberry Pi 5</span>
      </div>
      <div class="wire-row">
        <span class="wire-node wn-sub">ROS2 Core</span>
        <span class="wire-arrow">│</span>
        <span class="wire-node wn-sub">Vision Node</span>
        <span class="wire-arrow">│</span>
        <span class="wire-node wn-sub">IMU Node</span>
      </div>
      <div class="wire-row">
        <span class="wire-node wn-leaf">Gait Planner</span>
        <span class="wire-node wn-leaf">IK Solver</span>
        <span class="wire-node wn-leaf">OAK-D Lite</span>
        <span class="wire-node wn-leaf">MPU-6050</span>
        <span class="wire-node wn-leaf">PCA9685</span>
      </div>
      <div class="wire-row" style="margin-top:4px;">
        <span class="wire-node wn-leaf" style="border-color:#ff6b00;color:#ff6b00;">Servo × 12</span>
      </div>
    </div>
  </section>

  <!-- CODE -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">💻</div>
      <span class="section-title">Core Code</span>
      <div class="section-line"></div>
    </div>

    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">Python · ik_solver.py</span>
        <div class="code-dots">
          <span style="background:#ff5f57;"></span>
          <span style="background:#febc2e;"></span>
          <span style="background:#28c840;"></span>
        </div>
      </div>
<pre><span class="cm"># Inverse Kinematics — 3-DOF leg solver</span>
<span class="kw">import</span> <span class="var">numpy</span> <span class="kw">as</span> <span class="var">np</span>

<span class="kw">class</span> <span class="cl">LegIKSolver</span>:
    <span class="kw">def</span> <span class="fn">__init__</span>(<span class="var">self</span>, <span class="var">L1</span><span class="op">=</span><span class="num">0.10</span>, <span class="var">L2</span><span class="op">=</span><span class="num">0.12</span>, <span class="var">L3</span><span class="op">=</span><span class="num">0.08</span>):
        <span class="cm"># Link lengths: hip → thigh → shin (meters)</span>
        <span class="var">self</span><span class="op">.</span><span class="var">L1</span>, <span class="var">self</span><span class="op">.</span><span class="var">L2</span>, <span class="var">self</span><span class="op">.</span><span class="var">L3</span> <span class="op">=</span> <span class="var">L1</span>, <span class="var">L2</span>, <span class="var">L3</span>

    <span class="kw">def</span> <span class="fn">solve</span>(<span class="var">self</span>, <span class="var">x</span>, <span class="var">y</span>, <span class="var">z</span>):
        <span class="cm">"""Solve for joint angles given foot target (x, y, z)."""</span>
        <span class="var">theta1</span> <span class="op">=</span> <span class="var">np</span><span class="op">.</span><span class="fn">arctan2</span>(<span class="var">y</span>, <span class="var">x</span>)

        <span class="var">R</span>   <span class="op">=</span> <span class="var">np</span><span class="op">.</span><span class="fn">sqrt</span>(<span class="var">x</span><span class="op">**</span><span class="num">2</span> <span class="op">+</span> <span class="var">y</span><span class="op">**</span><span class="num">2</span>) <span class="op">-</span> <span class="var">self</span><span class="op">.</span><span class="var">L1</span>
        <span class="var">D</span>   <span class="op">=</span> <span class="var">np</span><span class="op">.</span><span class="fn">sqrt</span>(<span class="var">R</span><span class="op">**</span><span class="num">2</span> <span class="op">+</span> <span class="var">z</span><span class="op">**</span><span class="num">2</span>)
        <span class="var">cos2</span> <span class="op">=</span> (<span class="var">D</span><span class="op">**</span><span class="num">2</span> <span class="op">-</span> <span class="var">self</span><span class="op">.</span><span class="var">L2</span><span class="op">**</span><span class="num">2</span> <span class="op">-</span> <span class="var">self</span><span class="op">.</span><span class="var">L3</span><span class="op">**</span><span class="num">2</span>) <span class="op">/</span> (<span class="num">2</span> <span class="op">*</span> <span class="var">self</span><span class="op">.</span><span class="var">L2</span> <span class="op">*</span> <span class="var">self</span><span class="op">.</span><span class="var">L3</span>)
        <span class="var">theta3</span> <span class="op">=</span> <span class="var">np</span><span class="op">.</span><span class="fn">arccos</span>(<span class="var">np</span><span class="op">.</span><span class="fn">clip</span>(<span class="var">cos2</span>, <span class="op">-</span><span class="num">1</span>, <span class="num">1</span>))

        <span class="var">alpha</span>  <span class="op">=</span> <span class="var">np</span><span class="op">.</span><span class="fn">arctan2</span>(<span class="var">z</span>, <span class="var">R</span>)
        <span class="var">beta</span>   <span class="op">=</span> <span class="var">np</span><span class="op">.</span><span class="fn">arctan2</span>(<span class="var">self</span><span class="op">.</span><span class="var">L3</span> <span class="op">*</span> <span class="var">np</span><span class="op">.</span><span class="fn">sin</span>(<span class="var">theta3</span>),
                            <span class="var">self</span><span class="op">.</span><span class="var">L2</span> <span class="op">+</span> <span class="var">self</span><span class="op">.</span><span class="var">L3</span> <span class="op">*</span> <span class="var">np</span><span class="op">.</span><span class="fn">cos</span>(<span class="var">theta3</span>))
        <span class="var">theta2</span> <span class="op">=</span> <span class="var">alpha</span> <span class="op">-</span> <span class="var">beta</span>

        <span class="kw">return</span> <span class="var">np</span><span class="op">.</span><span class="fn">degrees</span>([<span class="var">theta1</span>, <span class="var">theta2</span>, <span class="var">theta3</span>])</pre>
    </div>

    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">Python · gait_controller.py</span>
        <div class="code-dots">
          <span style="background:#ff5f57;"></span>
          <span style="background:#febc2e;"></span>
          <span style="background:#28c840;"></span>
        </div>
      </div>
<pre><span class="cm"># Trot gait generator — diagonal pairs move together</span>
<span class="kw">class</span> <span class="cl">TrotGait</span>:
    <span class="var">PHASE</span> <span class="op">=</span> {<span class="str">'FL'</span>: <span class="num">0.0</span>, <span class="str">'BR'</span>: <span class="num">0.0</span>, <span class="str">'FR'</span>: <span class="num">0.5</span>, <span class="str">'BL'</span>: <span class="num">0.5</span>}

    <span class="kw">def</span> <span class="fn">step_trajectory</span>(<span class="var">self</span>, <span class="var">t</span>, <span class="var">leg</span>, <span class="var">stride</span><span class="op">=</span><span class="num">0.05</span>, <span class="var">lift</span><span class="op">=</span><span class="num">0.03</span>):
        <span class="var">phase</span> <span class="op">=</span> (<span class="var">t</span> <span class="op">+</span> <span class="var">self</span><span class="op">.</span><span class="var">PHASE</span>[<span class="var">leg</span>]) <span class="op">%</span> <span class="num">1.0</span>
        <span class="kw">if</span> <span class="var">phase</span> <span class="op">&lt;</span> <span class="num">0.5</span>:  <span class="cm"># swing phase</span>
            <span class="var">x</span> <span class="op">=</span> <span class="var">stride</span> <span class="op">*</span> <span class="var">np</span><span class="op">.</span><span class="fn">sin</span>(<span class="var">np</span><span class="op">.</span><span class="var">pi</span> <span class="op">*</span> <span class="var">phase</span> <span class="op">*</span> <span class="num">2</span>)
            <span class="var">z</span> <span class="op">=</span> <span class="var">lift</span>   <span class="op">*</span> <span class="var">np</span><span class="op">.</span><span class="fn">sin</span>(<span class="var">np</span><span class="op">.</span><span class="var">pi</span> <span class="op">*</span> <span class="var">phase</span> <span class="op">*</span> <span class="num">2</span>)
        <span class="kw">else</span>:             <span class="cm"># stance phase</span>
            <span class="var">x</span> <span class="op">=</span> <span class="var">stride</span> <span class="op">*</span> (<span class="num">1</span> <span class="op">-</span> <span class="num">2</span> <span class="op">*</span> (<span class="var">phase</span> <span class="op">-</span> <span class="num">0.5</span>))
            <span class="var">z</span> <span class="op">=</span> <span class="num">0.0</span>
        <span class="kw">return</span> <span class="var">x</span>, <span class="num">0.0</span>, <span class="var">z</span></pre>
    </div>
  </section>

  <!-- GAIT TABLE -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">🐾</div>
      <span class="section-title">Gait Modes</span>
      <div class="section-line"></div>
    </div>
    <div class="card" style="padding:0; overflow:hidden;">
      <table class="gait-table">
        <tr>
          <th>Gait</th>
          <th>Speed</th>
          <th>Stability</th>
          <th>Use Case</th>
          <th>Status</th>
        </tr>
        <tr>
          <td>Crawl (Static Walk)</td>
          <td>~0.15 m/s</td>
          <td>⭐⭐⭐⭐⭐</td>
          <td>Rough terrain, slopes</td>
          <td class="tag-green">✓ Done</td>
        </tr>
        <tr>
          <td>Trot (Diagonal)</td>
          <td>~0.5 m/s</td>
          <td>⭐⭐⭐⭐</td>
          <td>Default locomotion</td>
          <td class="tag-green">✓ Done</td>
        </tr>
        <tr>
          <td>Pace</td>
          <td>~0.6 m/s</td>
          <td>⭐⭐⭐</td>
          <td>Narrow corridors</td>
          <td class="tag-orange">⟳ WIP</td>
        </tr>
        <tr>
          <td>Bound / Gallop</td>
          <td>&gt;1.0 m/s</td>
          <td>⭐⭐</td>
          <td>Speed runs</td>
          <td class="tag-cyan">◈ Planned</td>
        </tr>
      </table>
    </div>
  </section>

  <!-- FEATURES -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">⚡</div>
      <span class="section-title">Capabilities</span>
      <div class="section-line"></div>
    </div>
    <div class="feature-grid">
      <div class="feature-item">
        <span class="feature-emoji">📐</span>
        <div class="feature-name">Inverse Kinematics</div>
        <div class="feature-desc">Analytical 3-DOF IK for precise foot placement in 3D space</div>
      </div>
      <div class="feature-item">
        <span class="feature-emoji">🏔️</span>
        <div class="feature-name">Terrain Adapt</div>
        <div class="feature-desc">IMU-driven body leveling on uneven surfaces up to 15°</div>
      </div>
      <div class="feature-item">
        <span class="feature-emoji">🎯</span>
        <div class="feature-name">Object Detection</div>
        <div class="feature-desc">YOLOv8-nano on OAK-D for real-time obstacle avoidance</div>
      </div>
      <div class="feature-item">
        <span class="feature-emoji">🌐</span>
        <div class="feature-name">ROS2 Integration</div>
        <div class="feature-desc">Full ROS2 node graph, URDF model, and Rviz2 visualization</div>
      </div>
      <div class="feature-item">
        <span class="feature-emoji">📱</span>
        <div class="feature-name">Remote Control</div>
        <div class="feature-desc">Joystick via WebSocket + custom web dashboard</div>
      </div>
      <div class="feature-item">
        <span class="feature-emoji">🖨️</span>
        <div class="feature-name">3D Printed Body</div>
        <div class="feature-desc">Full STEP files included — PLA+ chassis, TPU foot pads</div>
      </div>
    </div>
  </section>

  <!-- TECH STACK -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">🛠️</div>
      <span class="section-title">Tech Stack</span>
      <div class="section-line"></div>
    </div>
    <div class="tech-grid">
      <span class="tech-pill">Python 3.11</span>
      <span class="tech-pill">ROS2 Humble</span>
      <span class="tech-pill">NumPy</span>
      <span class="tech-pill">OpenCV</span>
      <span class="tech-pill">DepthAI SDK</span>
      <span class="tech-pill">YOLOv8-nano</span>
      <span class="tech-pill">Raspberry Pi OS</span>
      <span class="tech-pill">smbus2</span>
      <span class="tech-pill">adafruit-circuitpython-pca9685</span>
      <span class="tech-pill">FastAPI</span>
      <span class="tech-pill">WebSockets</span>
      <span class="tech-pill">Rviz2</span>
      <span class="tech-pill">Fusion 360</span>
      <span class="tech-pill">PrusaSlicer</span>
      <span class="tech-pill">KiCad</span>
    </div>
  </section>

  <!-- QUICK START -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">🚀</div>
      <span class="section-title">Quick Start</span>
      <div class="section-line"></div>
    </div>
    <div class="code-block">
      <div class="code-header">
        <span class="code-lang">Bash</span>
        <div class="code-dots">
          <span style="background:#ff5f57;"></span>
          <span style="background:#febc2e;"></span>
          <span style="background:#28c840;"></span>
        </div>
      </div>
<pre><span class="cm"># Clone the repo</span>
<span class="fn">git</span> clone https://github.com/yourusername/k9-x1
<span class="fn">cd</span> k9-x1

<span class="cm"># Install Python dependencies</span>
<span class="fn">pip</span> install -r requirements.txt

<span class="cm"># Source ROS2 and build workspace</span>
<span class="fn">source</span> /opt/ros/humble/setup.bash
<span class="fn">colcon</span> build --symlink-install
<span class="fn">source</span> install/setup.bash

<span class="cm"># Launch the robot</span>
<span class="fn">ros2</span> launch k9_bringup full_dog.launch.py

<span class="cm"># In a second terminal — web dashboard</span>
<span class="fn">python</span> dashboard/server.py  <span class="cm"># opens on :8080</span></pre>
    </div>
  </section>

  <!-- ROADMAP -->
  <section class="section fade-in">
    <div class="section-header">
      <div class="section-icon">🗺️</div>
      <span class="section-title">Roadmap</span>
      <div class="section-line"></div>
    </div>
    <div class="roadmap">
      <div class="roadmap-item done">
        <div class="rm-label rm-done">✓ Complete</div>
        <div class="rm-title">Mechanical Build + Wiring</div>
        <div class="rm-desc">All 12 servos calibrated, power distribution, chassis assembled</div>
      </div>
      <div class="roadmap-item done">
        <div class="rm-label rm-done">✓ Complete</div>
        <div class="rm-title">IK Solver + Static Stand</div>
        <div class="rm-desc">Analytical IK, body height control, and stable standing pose</div>
      </div>
      <div class="roadmap-item done">
        <div class="rm-label rm-done">✓ Complete</div>
        <div class="rm-title">Trot Gait Locomotion</div>
        <div class="rm-desc">Smooth forward/backward/turn trot at up to 0.5 m/s</div>
      </div>
      <div class="roadmap-item inprogress">
        <div class="rm-label rm-wip">⟳ In Progress</div>
        <div class="rm-title">Terrain Adaptation + IMU Feedback</div>
        <div class="rm-desc">Closed-loop body leveling using MPU-6050 pitch/roll data</div>
      </div>
      <div class="roadmap-item">
        <div class="rm-label rm-todo">◈ Planned</div>
        <div class="rm-title">SLAM + Autonomous Navigation</div>
        <div class="rm-desc">2D occupancy mapping with Nav2, autonomous path following</div>
      </div>
      <div class="roadmap-item">
        <div class="rm-label rm-todo">◈ Planned</div>
        <div class="rm-title">Reinforcement Learning Gait</div>
        <div class="rm-desc">Sim-to-real with Isaac Gym / MuJoCo trained locomotion policy</div>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <div class="divider"></div>
  <footer class="footer">
    <span class="footer-dog">🤖</span>
    <p class="footer-text">Built with <span>♥</span> and too many late nights · <span>K9-X1</span> · MIT License</p>
  </footer>

</div>

<script>
  // Intersection Observer for fade-in animations
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.style.opacity = '1';
        e.target.style.transform = 'translateY(0)';
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.fade-in').forEach(el => {
    el.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
    observer.observe(el);
  });
</script>
</body>
</html>
