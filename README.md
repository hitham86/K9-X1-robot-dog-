import { useState } from "react";

const styles = `
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&display=swap');

  * { margin: 0; padding: 0; box-sizing: border-box; }

  .k9-wrap {
    background: #050810;
    color: #c8d8f0;
    font-family: 'Rajdhani', 'Courier New', monospace;
    min-height: 100vh;
    position: relative;
    overflow-x: hidden;
  }

  .k9-wrap::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,212,255,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,212,255,0.04) 1px, transparent 1px);
    background-size: 36px 36px;
    pointer-events: none;
    z-index: 0;
  }

  .k9-inner { max-width: 900px; margin: 0 auto; padding: 0 24px 60px; position: relative; z-index: 1; }

  /* HERO */
  .hero { padding: 64px 0 48px; text-align: center; position: relative; }
  .dog-emoji { font-size: 96px; display: block; margin-bottom: 16px;
    filter: drop-shadow(0 0 32px rgba(0,212,255,0.7));
    animation: float 4s ease-in-out infinite; }
  @keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-14px)} }

  .badge-row { display: flex; gap: 8px; justify-content: center; flex-wrap: wrap; margin-bottom: 28px; }
  .badge { font-family: 'Share Tech Mono', monospace; font-size: 10px; padding: 3px 10px;
    border-radius: 2px; border: 1px solid; letter-spacing: 0.08em; text-transform: uppercase; }
  .bc { border-color: #00d4ff; color: #00d4ff; background: rgba(0,212,255,0.08); }
  .bo { border-color: #ff6b00; color: #ff6b00; background: rgba(255,107,0,0.08); }
  .bg { border-color: #00ff88; color: #00ff88; background: rgba(0,255,136,0.08); }

  .hero-title { font-family: 'Orbitron', monospace; font-size: clamp(40px,8vw,80px); font-weight: 900;
    letter-spacing: 0.1em; line-height: 1; margin-bottom: 8px;
    background: linear-gradient(135deg, #00d4ff 0%, #ffffff 45%, #ff6b00 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text; }
  .hero-sub { font-family: 'Share Tech Mono', monospace; font-size: 11px; color: #4a6080;
    letter-spacing: 0.25em; margin-bottom: 20px; text-transform: uppercase; }
  .hero-desc { font-size: 17px; color: #a0b8d8; max-width: 560px; margin: 0 auto; font-weight: 300; line-height: 1.7; }

  .divider { height: 1px; background: linear-gradient(90deg,transparent,#00d4ff,#ff6b00,transparent);
    margin: 48px 0; opacity: 0.35; }

  /* STATS */
  .stat-strip { display: grid; grid-template-columns: repeat(4,1fr); gap: 1px;
    background: #1a2a4a; border: 1px solid #1a2a4a; border-radius: 4px; overflow: hidden; margin-bottom: 56px; }
  .stat { background: #0d1428; padding: 22px 12px; text-align: center; transition: background 0.2s; }
  .stat:hover { background: #111d35; }
  .stat-num { font-family: 'Orbitron', monospace; font-size: 26px; font-weight: 700;
    color: #00d4ff; display: block; line-height: 1; margin-bottom: 6px; }
  .stat-lbl { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: #4a6080;
    text-transform: uppercase; letter-spacing: 0.12em; }

  /* SECTION */
  .section { margin-bottom: 52px; }
  .sec-hdr { display: flex; align-items: center; gap: 14px; margin-bottom: 24px; }
  .sec-icon { width: 34px; height: 34px; border: 1px solid #00d4ff; border-radius: 3px;
    display: flex; align-items: center; justify-content: center; font-size: 16px;
    background: rgba(0,212,255,0.06); flex-shrink: 0;
    box-shadow: 0 0 16px rgba(0,212,255,0.25); }
  .sec-title { font-family: 'Orbitron', monospace; font-size: 15px; font-weight: 700;
    color: #00d4ff; letter-spacing: 0.12em; text-transform: uppercase; }
  .sec-line { flex:1; height:1px; background: linear-gradient(90deg,#1a2a4a,transparent); }

  /* HW GRID */
  .hw-grid { display: grid; grid-template-columns: repeat(2,1fr); gap: 12px; }
  .hw-item { display: flex; gap: 12px; background: #0d1428; border: 1px solid #1a2a4a;
    border-radius: 4px; padding: 14px; transition: border-color 0.2s; }
  .hw-item:hover { border-color: rgba(0,212,255,0.3); }
  .hw-ico { font-size: 20px; flex-shrink: 0; margin-top: 1px; }
  .hw-name { font-family: 'Share Tech Mono', monospace; font-size: 12px; color: #00d4ff; margin-bottom: 3px; }
  .hw-spec { font-size: 13px; color: #4a6080; line-height: 1.4; }

  /* WIRING */
  .wiring { background: #020509; border: 1px solid #1a2a4a; border-radius: 4px;
    padding: 28px 20px; text-align: center; }
  .wire-row { display: flex; align-items: center; justify-content: center; gap: 10px;
    margin: 10px 0; flex-wrap: wrap; }
  .wn { display: inline-block; padding: 8px 16px; border-radius: 3px; font-family: 'Share Tech Mono', monospace;
    font-size: 11px; letter-spacing: 0.07em; text-transform: uppercase; }
  .wn-main { border: 2px solid #00d4ff; color: #00d4ff; background: rgba(0,212,255,0.1); font-size: 13px; padding: 10px 22px; }
  .wn-sub { border: 1px solid #00ff88; color: #00ff88; background: rgba(0,255,136,0.06); }
  .wn-leaf { border: 1px solid #1a2a4a; color: #4a6080; font-size: 10px; padding: 5px 10px; }
  .wn-servo { border: 1px solid #ff6b00; color: #ff6b00; background: rgba(255,107,0,0.07); }
  .wire-sep { color: #1a2a4a; font-size: 18px; }

  /* CODE */
  .code-block { background: #020509; border: 1px solid #1a2a4a; border-radius: 4px;
    overflow: hidden; margin-bottom: 16px; }
  .code-hdr { display: flex; align-items: center; justify-content: space-between;
    padding: 8px 14px; background: #0a0f1e; border-bottom: 1px solid #1a2a4a; }
  .code-lang { font-family: 'Share Tech Mono', monospace; font-size: 10px; color: #00ff88;
    text-transform: uppercase; letter-spacing: 0.1em; }
  .code-dots { display: flex; gap: 5px; }
  .code-dots span { width: 8px; height: 8px; border-radius: 50%; }
  pre { padding: 18px; overflow-x: auto; font-family: 'Share Tech Mono', monospace;
    font-size: 12px; line-height: 1.85; color: #7a9ab8; }
  .kw{color:#c792ea} .fn{color:#82aaff} .str{color:#c3e88d} .num{color:#f78c6c}
  .cm{color:#3d5a70;font-style:italic} .cl{color:#ffcb6b} .op{color:#89ddff} .vr{color:#eeffff}

  /* FEATURES */
  .feat-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; }
  .feat { background: #0d1428; border: 1px solid #1a2a4a; border-radius: 4px;
    padding: 16px 12px; text-align: center; transition: all 0.25s; }
  .feat:hover { border-color: rgba(255,107,0,0.4); box-shadow: 0 0 16px rgba(255,107,0,0.15); }
  .feat-ico { font-size: 26px; display: block; margin-bottom: 7px; }
  .feat-name { font-family: 'Orbitron', monospace; font-size: 9px; color: #ff6b00;
    text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 5px; }
  .feat-desc { font-size: 12px; color: #4a6080; line-height: 1.4; }

  /* TECH */
  .tech-row { display: flex; flex-wrap: wrap; gap: 8px; }
  .tech-pill { font-family: 'Share Tech Mono', monospace; font-size: 11px; padding: 5px 12px;
    border-radius: 2px; border: 1px solid #1a2a4a; color: #c8d8f0; background: #0d1428;
    letter-spacing: 0.04em; cursor: default; transition: all 0.2s; }
  .tech-pill:hover { border-color: #00d4ff; color: #00d4ff; background: rgba(0,212,255,0.08); transform: translateY(-1px); }

  /* TABLE */
  .gait-table { width: 100%; border-collapse: collapse; font-family: 'Share Tech Mono', monospace; font-size: 12px; }
  .gait-table th { text-align: left; padding: 9px 14px; background: rgba(0,212,255,0.07);
    border: 1px solid #1a2a4a; color: #00d4ff; text-transform: uppercase; letter-spacing: 0.08em; font-size: 10px; }
  .gait-table td { padding: 9px 14px; border: 1px solid #1a2a4a; color: #c8d8f0; transition: background 0.15s; }
  .gait-table tr:hover td { background: rgba(0,212,255,0.04); }
  .tg{color:#00ff88} .to{color:#ff6b00} .tc{color:#00d4ff}

  /* ROADMAP */
  .roadmap { padding-left: 26px; position: relative; }
  .roadmap::before { content:''; position:absolute; left:6px; top:10px; bottom:10px;
    width:1px; background: linear-gradient(180deg,#00d4ff,#ff6b00,#1a2a4a); }
  .rm-item { position: relative; margin-bottom: 16px; padding: 13px 16px;
    background: #0d1428; border: 1px solid #1a2a4a; border-radius: 4px; }
  .rm-item::before { content:''; position:absolute; left:-21px; top:16px;
    width:10px; height:10px; border-radius:50%; border:2px solid #00d4ff; background:#050810; }
  .rm-done::before { background:#00d4ff; }
  .rm-wip::before { background:#ff6b00; border-color:#ff6b00; }
  .rm-lbl { font-family:'Share Tech Mono',monospace; font-size:9px; letter-spacing:0.15em;
    text-transform:uppercase; margin-bottom:3px; }
  .lbl-d{color:#00ff88} .lbl-w{color:#ff6b00} .lbl-t{color:#4a6080}
  .rm-title { font-size:15px; font-weight:600; color:#c8d8f0; margin-bottom:3px; }
  .rm-desc { font-size:12px; color:#4a6080; }

  /* FOOTER */
  .footer { border-top:1px solid #1a2a4a; padding:36px 0; text-align:center; }
  .footer-dog { font-size:44px; display:block; margin-bottom:10px; opacity:0.5; animation:float 6s ease-in-out infinite; }
  .footer-txt { font-family:'Share Tech Mono',monospace; font-size:11px; color:#4a6080; letter-spacing:0.15em; text-transform:uppercase; }
  .footer-txt span { color:#00d4ff; }

  @media(max-width:600px){
    .stat-strip{grid-template-columns:repeat(2,1fr)}
    .hw-grid{grid-template-columns:1fr}
    .feat-grid{grid-template-columns:repeat(2,1fr)}
  }
`;

export default function K9Repo() {
  const [activeTab, setActiveTab] = useState("ik");

  const codeIK = `<span class="cm"># Inverse Kinematics — 3-DOF leg solver</span>
<span class="kw">import</span> <span class="vr">numpy</span> <span class="kw">as</span> <span class="vr">np</span>

<span class="kw">class</span> <span class="cl">LegIKSolver</span>:
    <span class="kw">def</span> <span class="fn">__init__</span>(<span class="vr">self</span>, L1<span class="op">=</span><span class="num">0.10</span>, L2<span class="op">=</span><span class="num">0.12</span>, L3<span class="op">=</span><span class="num">0.08</span>):
        <span class="cm"># hip → thigh → shin link lengths (m)</span>
        <span class="vr">self</span>.L1, <span class="vr">self</span>.L2, <span class="vr">self</span>.L3 <span class="op">=</span> L1, L2, L3

    <span class="kw">def</span> <span class="fn">solve</span>(<span class="vr">self</span>, x, y, z):
        <span class="cm">"""Return [θ1, θ2, θ3] in degrees for foot at (x,y,z)."""</span>
        theta1 <span class="op">=</span> np.<span class="fn">arctan2</span>(y, x)
        R   <span class="op">=</span> np.<span class="fn">sqrt</span>(x<span class="op">**</span><span class="num">2</span> <span class="op">+</span> y<span class="op">**</span><span class="num">2</span>) <span class="op">-</span> <span class="vr">self</span>.L1
        D   <span class="op">=</span> np.<span class="fn">sqrt</span>(R<span class="op">**</span><span class="num">2</span> <span class="op">+</span> z<span class="op">**</span><span class="num">2</span>)
        cos2 <span class="op">=</span> (D<span class="op">**</span><span class="num">2</span> <span class="op">-</span> <span class="vr">self</span>.L2<span class="op">**</span><span class="num">2</span> <span class="op">-</span> <span class="vr">self</span>.L3<span class="op">**</span><span class="num">2</span>) <span class="op">/</span> (<span class="num">2</span><span class="op">*</span><span class="vr">self</span>.L2<span class="op">*</span><span class="vr">self</span>.L3)
        theta3 <span class="op">=</span> np.<span class="fn">arccos</span>(np.<span class="fn">clip</span>(cos2, <span class="op">-</span><span class="num">1</span>, <span class="num">1</span>))
        beta   <span class="op">=</span> np.<span class="fn">arctan2</span>(<span class="vr">self</span>.L3<span class="op">*</span>np.<span class="fn">sin</span>(theta3),
                            <span class="vr">self</span>.L2<span class="op">+</span><span class="vr">self</span>.L3<span class="op">*</span>np.<span class="fn">cos</span>(theta3))
        theta2 <span class="op">=</span> np.<span class="fn">arctan2</span>(z, R) <span class="op">-</span> beta
        <span class="kw">return</span> np.<span class="fn">degrees</span>([theta1, theta2, theta3])`;

  const codeGait = `<span class="cm"># Trot gait — diagonal pairs in phase</span>
<span class="kw">class</span> <span class="cl">TrotGait</span>:
    <span class="vr">PHASE</span> <span class="op">=</span> {<span class="str">'FL'</span>:<span class="num">0.0</span>, <span class="str">'BR'</span>:<span class="num">0.0</span>, <span class="str">'FR'</span>:<span class="num">0.5</span>, <span class="str">'BL'</span>:<span class="num">0.5</span>}

    <span class="kw">def</span> <span class="fn">trajectory</span>(<span class="vr">self</span>, t, leg, stride<span class="op">=</span><span class="num">0.05</span>, lift<span class="op">=</span><span class="num">0.03</span>):
        phase <span class="op">=</span> (t <span class="op">+</span> <span class="vr">self</span>.PHASE[leg]) <span class="op">%</span> <span class="num">1.0</span>
        <span class="kw">if</span> phase <span class="op">&lt;</span> <span class="num">0.5</span>:  <span class="cm"># swing</span>
            x <span class="op">=</span> stride <span class="op">*</span> np.<span class="fn">sin</span>(np.pi <span class="op">*</span> phase <span class="op">*</span> <span class="num">2</span>)
            z <span class="op">=</span> lift   <span class="op">*</span> np.<span class="fn">sin</span>(np.pi <span class="op">*</span> phase <span class="op">*</span> <span class="num">2</span>)
        <span class="kw">else</span>:            <span class="cm"># stance</span>
            x <span class="op">=</span> stride <span class="op">*</span> (<span class="num">1</span> <span class="op">-</span> <span class="num">2</span> <span class="op">*</span> (phase <span class="op">-</span> <span class="num">0.5</span>))
            z <span class="op">=</span> <span class="num">0.0</span>
        <span class="kw">return</span> x, <span class="num">0.0</span>, z`;

  const codeServo = `<span class="cm"># Servo control via PCA9685</span>
<span class="kw">import</span> <span class="vr">board</span>, <span class="vr">busio</span>
<span class="kw">from</span> <span class="vr">adafruit_pca9685</span> <span class="kw">import</span> <span class="cl">PCA9685</span>
<span class="kw">from</span> <span class="vr">adafruit_motor</span> <span class="kw">import</span> <span class="vr">servo</span>

<span class="vr">i2c</span> <span class="op">=</span> busio.<span class="fn">I2C</span>(board.SCL, board.SDA)
<span class="vr">pca</span> <span class="op">=</span> <span class="cl">PCA9685</span>(<span class="vr">i2c</span>)
<span class="vr">pca</span>.frequency <span class="op">=</span> <span class="num">50</span>  <span class="cm"># Hz</span>

<span class="kw">def</span> <span class="fn">set_angle</span>(channel, angle):
    <span class="cm">"""Set servo on channel to angle (0–180°)."""</span>
    <span class="vr">s</span> <span class="op">=</span> servo.<span class="cl">Servo</span>(<span class="vr">pca</span>.channels[channel],
                    min_pulse<span class="op">=</span><span class="num">500</span>, max_pulse<span class="op">=</span><span class="num">2500</span>)
    <span class="vr">s</span>.angle <span class="op">=</span> <span class="fn">max</span>(<span class="num">0</span>, <span class="fn">min</span>(<span class="num">180</span>, angle))

<span class="cm"># Map leg joints to PCA channels</span>
<span class="vr">LEG_CHANNELS</span> <span class="op">=</span> {
    <span class="str">'FL'</span>: [<span class="num">0</span>,<span class="num">1</span>,<span class="num">2</span>], <span class="str">'FR'</span>: [<span class="num">3</span>,<span class="num">4</span>,<span class="num">5</span>],
    <span class="str">'BL'</span>: [<span class="num">6</span>,<span class="num">7</span>,<span class="num">8</span>], <span class="str">'BR'</span>: [<span class="num">9</span>,<span class="num">10</span>,<span class="num">11</span>]
}`;

  const tabs = [
    { id: "ik", label: "ik_solver.py", code: codeIK },
    { id: "gait", label: "gait_controller.py", code: codeGait },
    { id: "servo", label: "servo_driver.py", code: codeServo },
  ];

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: styles }} />
      <div className="k9-wrap">
        <div className="k9-inner">

          {/* HERO */}
          <div className="hero">
            <span className="dog-emoji">🤖</span>
            <div className="badge-row">
              <span className="badge bc">Python 3.11</span>
              <span className="badge bo">ROS2 Humble</span>
              <span className="badge bg">Open Source</span>
              <span className="badge bc">Raspberry Pi 5</span>
              <span className="badge bo">12-DOF</span>
              <span className="badge bg">MIT License</span>
            </div>
            <h1 className="hero-title">K9-X1</h1>
            <p className="hero-sub">Autonomous Quadruped Robot · Build Log & Source Code</p>
            <p className="hero-desc">
              A fully custom-built, 3D-printed quadruped robot dog with onboard computer vision,
              inverse kinematics, and gait planning. Every servo, circuit, and line of code — built from scratch.
            </p>
          </div>

          <div className="divider" />

          {/* STATS */}
          <div className="stat-strip">
            {[["12","Servo DOF"],["2.1 kg","Weight"],["~45 cm","Body Length"],["90 min","Battery"]].map(([n,l])=>(
              <div key={l} className="stat">
                <span className="stat-num">{n}</span>
                <span className="stat-lbl">{l}</span>
              </div>
            ))}
          </div>

          {/* HARDWARE */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">🔧</div>
              <span className="sec-title">Hardware</span>
              <div className="sec-line" />
            </div>
            <div className="hw-grid">
              {[
                ["🧠","Raspberry Pi 5 (8GB)","Main compute — runs ROS2, vision pipeline & gait controller"],
                ["⚙️","MG996R Servos × 12","3 per leg: hip abduction, hip flexion, knee. 10 kg·cm torque"],
                ["👁️","OAK-D Lite Depth Cam","Stereo depth + neural inference for obstacle detection"],
                ["🎛️","PCA9685 PWM Driver","16-ch I²C PWM board — drives all 12 servos from Pi"],
                ["📡","MPU-6050 IMU","6-axis accel + gyro for balance feedback loop"],
                ["🔋","3S LiPo 5000 mAh","11.1V regulated via buck converters to 5V / 6V rails"],
              ].map(([ico,name,spec])=>(
                <div key={name} className="hw-item">
                  <span className="hw-ico">{ico}</span>
                  <div>
                    <div className="hw-name">{name}</div>
                    <div className="hw-spec">{spec}</div>
                  </div>
                </div>
              ))}
            </div>
          </div>

          {/* ARCHITECTURE */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">🏗️</div>
              <span className="sec-title">System Architecture</span>
              <div className="sec-line" />
            </div>
            <div className="wiring">
              <div className="wire-row"><span className="wn wn-main">Raspberry Pi 5</span></div>
              <div className="wire-row">
                <span className="wn wn-sub">ROS2 Core</span>
                <span className="wire-sep">·</span>
                <span className="wn wn-sub">Vision Node</span>
                <span className="wire-sep">·</span>
                <span className="wn wn-sub">IMU Node</span>
              </div>
              <div className="wire-row">
                <span className="wn wn-leaf">Gait Planner</span>
                <span className="wn wn-leaf">IK Solver</span>
                <span className="wn wn-leaf">OAK-D Lite</span>
                <span className="wn wn-leaf">MPU-6050</span>
                <span className="wn wn-leaf">PCA9685</span>
              </div>
              <div className="wire-row">
                <span className="wn wn-servo">Servo × 12 (FL · FR · BL · BR)</span>
              </div>
            </div>
          </div>

          {/* CODE */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">💻</div>
              <span className="sec-title">Core Code</span>
              <div className="sec-line" />
            </div>
            <div style={{display:"flex",gap:"8px",marginBottom:"12px",flexWrap:"wrap"}}>
              {tabs.map(t=>(
                <button key={t.id} onClick={()=>setActiveTab(t.id)} style={{
                  fontFamily:"'Share Tech Mono',monospace", fontSize:"11px", padding:"6px 14px",
                  border:"1px solid", borderRadius:"2px", cursor:"pointer", letterSpacing:"0.05em",
                  transition:"all 0.2s",
                  borderColor: activeTab===t.id ? "#00d4ff" : "#1a2a4a",
                  color: activeTab===t.id ? "#00d4ff" : "#4a6080",
                  background: activeTab===t.id ? "rgba(0,212,255,0.1)" : "#0d1428",
                }}>{t.label}</button>
              ))}
            </div>
            {tabs.filter(t=>t.id===activeTab).map(t=>(
              <div key={t.id} className="code-block">
                <div className="code-hdr">
                  <span className="code-lang">Python · {t.label}</span>
                  <div className="code-dots">
                    <span style={{background:"#ff5f57"}}/>
                    <span style={{background:"#febc2e"}}/>
                    <span style={{background:"#28c840"}}/>
                  </div>
                </div>
                <pre dangerouslySetInnerHTML={{__html: t.code}} />
              </div>
            ))}
          </div>

          {/* GAIT TABLE */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">🐾</div>
              <span className="sec-title">Gait Modes</span>
              <div className="sec-line" />
            </div>
            <div style={{background:"#0d1428",border:"1px solid #1a2a4a",borderRadius:"4px",overflow:"hidden"}}>
              <table className="gait-table">
                <thead>
                  <tr><th>Gait</th><th>Speed</th><th>Stability</th><th>Use Case</th><th>Status</th></tr>
                </thead>
                <tbody>
                  <tr><td>Crawl (Static Walk)</td><td>~0.15 m/s</td><td>★★★★★</td><td>Rough terrain, slopes</td><td className="tg">✓ Done</td></tr>
                  <tr><td>Trot (Diagonal)</td><td>~0.50 m/s</td><td>★★★★</td><td>Default locomotion</td><td className="tg">✓ Done</td></tr>
                  <tr><td>Pace</td><td>~0.60 m/s</td><td>★★★</td><td>Narrow corridors</td><td className="to">⟳ WIP</td></tr>
                  <tr><td>Bound / Gallop</td><td>&gt;1.0 m/s</td><td>★★</td><td>Speed runs</td><td className="tc">◈ Planned</td></tr>
                </tbody>
              </table>
            </div>
          </div>

          {/* FEATURES */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">⚡</div>
              <span className="sec-title">Capabilities</span>
              <div className="sec-line" />
            </div>
            <div className="feat-grid">
              {[
                ["📐","Inverse Kinematics","Analytical 3-DOF IK for precise foot placement in 3D space"],
                ["🏔️","Terrain Adapt","IMU-driven body leveling on slopes up to 15°"],
                ["🎯","Object Detection","YOLOv8-nano on OAK-D for real-time obstacle avoidance"],
                ["🌐","ROS2 Integration","Full node graph, URDF model, Rviz2 visualization"],
                ["📱","Remote Control","Joystick via WebSocket + custom web dashboard"],
                ["🖨️","3D Printed Body","Full STEP files — PLA+ chassis, TPU foot pads"],
              ].map(([ico,name,desc])=>(
                <div key={name} className="feat">
                  <span className="feat-ico">{ico}</span>
                  <div className="feat-name">{name}</div>
                  <div className="feat-desc">{desc}</div>
                </div>
              ))}
            </div>
          </div>

          {/* TECH STACK */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">🛠️</div>
              <span className="sec-title">Tech Stack</span>
              <div className="sec-line" />
            </div>
            <div className="tech-row">
              {["Python 3.11","ROS2 Humble","NumPy","OpenCV","DepthAI SDK","YOLOv8-nano",
                "Raspberry Pi OS","smbus2","adafruit-pca9685","FastAPI","WebSockets",
                "Rviz2","Fusion 360","PrusaSlicer","KiCad"].map(t=>(
                <span key={t} className="tech-pill">{t}</span>
              ))}
            </div>
          </div>

          {/* QUICK START */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">🚀</div>
              <span className="sec-title">Quick Start</span>
              <div className="sec-line" />
            </div>
            <div className="code-block">
              <div className="code-hdr">
                <span className="code-lang">Bash</span>
                <div className="code-dots">
                  <span style={{background:"#ff5f57"}}/><span style={{background:"#febc2e"}}/><span style={{background:"#28c840"}}/>
                </div>
              </div>
              <pre dangerouslySetInnerHTML={{__html:`<span class="cm"># 1. Clone the repo</span>
<span class="fn">git</span> clone https://github.com/yourusername/k9-x1 && <span class="fn">cd</span> k9-x1

<span class="cm"># 2. Install Python dependencies</span>
<span class="fn">pip</span> install -r requirements.txt

<span class="cm"># 3. Build ROS2 workspace</span>
<span class="fn">source</span> /opt/ros/humble/setup.bash
<span class="fn">colcon</span> build --symlink-install && <span class="fn">source</span> install/setup.bash

<span class="cm"># 4. Launch the robot</span>
<span class="fn">ros2</span> launch k9_bringup full_dog.launch.py

<span class="cm"># 5. Open web dashboard (separate terminal)</span>
<span class="fn">python</span> dashboard/server.py   <span class="cm"># → http://localhost:8080</span>`}} />
            </div>
          </div>

          {/* ROADMAP */}
          <div className="section">
            <div className="sec-hdr">
              <div className="sec-icon">🗺️</div>
              <span className="sec-title">Roadmap</span>
              <div className="sec-line" />
            </div>
            <div className="roadmap">
              {[
                {s:"done",lbl:"✓ Complete",lc:"lbl-d",title:"Mechanical Build + Wiring",desc:"All 12 servos calibrated, power distribution, chassis assembled"},
                {s:"done",lbl:"✓ Complete",lc:"lbl-d",title:"IK Solver + Static Stand",desc:"Analytical IK, body height control, stable standing pose"},
                {s:"done",lbl:"✓ Complete",lc:"lbl-d",title:"Trot Gait Locomotion",desc:"Smooth forward / backward / turn at up to 0.5 m/s"},
                {s:"wip", lbl:"⟳ In Progress",lc:"lbl-w",title:"Terrain Adaptation + IMU Feedback",desc:"Closed-loop body leveling using MPU-6050 pitch / roll data"},
                {s:"",   lbl:"◈ Planned",lc:"lbl-t",title:"SLAM + Autonomous Navigation",desc:"2D occupancy map with Nav2, autonomous path following"},
                {s:"",   lbl:"◈ Planned",lc:"lbl-t",title:"RL Gait Policy",desc:"Sim-to-real with Isaac Gym / MuJoCo trained locomotion policy"},
              ].map((r,i)=>(
                <div key={i} className={`rm-item rm-${r.s||"todo"}`}>
                  <div className={`rm-lbl ${r.lc}`}>{r.lbl}</div>
                  <div className="rm-title">{r.title}</div>
                  <div className="rm-desc">{r.desc}</div>
                </div>
              ))}
            </div>
          </div>

          <div className="divider" />
          <div className="footer">
            <span className="footer-dog">🤖</span>
            <p className="footer-txt">Built with <span>♥</span> and too many late nights · <span>K9-X1</span> · MIT License</p>
          </div>

        </div>
      </div>
    </>
  );
}
