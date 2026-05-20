<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Laboratorio de De Broglie · Simulador Cuántico Interactivo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,400;0,9..144,500;0,9..144,600;1,9..144,400&family=JetBrains+Mono:wght@400;500;600&family=Manrope:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0e1a;
    --surface: #131829;
    --surface-2: #1a2038;
    --surface-3: #232a45;
    --border: rgba(255, 255, 255, 0.07);
    --border-strong: rgba(255, 255, 255, 0.14);
    --text: #e8e6e1;
    --text-2: #9a9da8;
    --text-3: #6b6e7a;
    --accent: #5eead4;
    --accent-glow: rgba(94, 234, 212, 0.18);
    --accent-warm: #fbbf24;
    --accent-warm-glow: rgba(251, 191, 36, 0.2);
    --accent-pink: #f472b6;
    --accent-violet: #a78bfa;
    --danger: #fb7185;
    --font-display: 'Fraunces', Georgia, serif;
    --font-body: 'Manrope', system-ui, sans-serif;
    --font-mono: 'JetBrains Mono', ui-monospace, monospace;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { background: var(--bg); }

  body {
    color: var(--text);
    font-family: var(--font-body);
    font-size: 16px;
    line-height: 1.65;
    min-height: 100vh;
    padding: 3rem 1.5rem 5rem;
    background:
      radial-gradient(ellipse 80% 50% at 50% 0%, rgba(94, 234, 212, 0.05), transparent 60%),
      radial-gradient(ellipse 60% 30% at 80% 70%, rgba(167, 139, 250, 0.04), transparent 60%),
      var(--bg);
    background-attachment: fixed;
    overflow-x: hidden;
  }

  /* Floating particles backdrop */
  .backdrop {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
  }
  .backdrop canvas { width: 100%; height: 100%; opacity: 0.4; }

  .container {
    max-width: 820px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  /* ============ HERO ============ */
  .hero { margin-bottom: 2.5rem; }
  .eyebrow {
    font-family: var(--font-mono);
    font-size: 11px;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--accent);
    margin-bottom: 1.5rem;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .eyebrow::before {
    content: '';
    width: 24px;
    height: 1px;
    background: var(--accent);
  }
  .hero h1 {
    font-family: var(--font-display);
    font-size: clamp(2.2rem, 6vw, 3.4rem);
    font-weight: 400;
    line-height: 1.02;
    letter-spacing: -0.025em;
    margin-bottom: 1.25rem;
    font-variation-settings: 'opsz' 96;
  }
  .hero h1 em {
    font-style: italic;
    color: var(--accent-warm);
    font-weight: 300;
  }
  .hero p.lead {
    color: var(--text-2);
    max-width: 60ch;
    font-size: 1.05rem;
    line-height: 1.7;
  }

  /* ============ NAV CHIPS ============ */
  .nav-chips {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
    margin-bottom: 2.5rem;
    padding: 8px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 999px;
    overflow-x: auto;
  }
  .nav-chip {
    font-family: var(--font-mono);
    font-size: 10px;
    padding: 8px 14px;
    border-radius: 999px;
    color: var(--text-2);
    text-decoration: none;
    transition: all 0.15s;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    white-space: nowrap;
    cursor: pointer;
    border: none;
    background: transparent;
    font-weight: 500;
  }
  .nav-chip:hover {
    background: var(--surface-2);
    color: var(--accent);
  }

  /* ============ FORMULA ============ */
  .formula-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 2.25rem 1.5rem;
    text-align: center;
    margin-bottom: 2rem;
    position: relative;
    overflow: hidden;
  }
  .formula-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at center, var(--accent-glow), transparent 70%);
    opacity: 0.4;
    pointer-events: none;
  }
  .formula-label {
    font-family: var(--font-mono);
    font-size: 10px;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--text-3);
    margin-bottom: 1rem;
    position: relative;
  }
  .formula {
    font-family: var(--font-display);
    font-size: clamp(2.4rem, 7vw, 3.6rem);
    font-weight: 400;
    line-height: 1;
    margin-bottom: 1rem;
    position: relative;
    letter-spacing: -0.01em;
  }
  .formula .var { font-style: italic; color: var(--accent-warm); }
  .formula .lambda { color: var(--accent); }
  .formula .op { color: var(--text-3); margin: 0 0.15em; }
  .formula-legend {
    color: var(--text-2);
    font-size: 0.92rem;
    max-width: 50ch;
    margin: 0 auto;
    position: relative;
  }
  .formula-legend em { font-style: italic; color: var(--text); }

  /* ============ SECTIONS ============ */
  .section { margin-bottom: 4rem; scroll-margin-top: 1rem; }
  .section-title {
    font-family: var(--font-display);
    font-size: clamp(1.4rem, 3vw, 1.75rem);
    font-weight: 400;
    letter-spacing: -0.01em;
    margin-bottom: 0.5rem;
    display: flex;
    align-items: baseline;
    gap: 0.85rem;
    line-height: 1.1;
  }
  .section-title .num {
    font-family: var(--font-mono);
    font-size: 0.7rem;
    color: var(--accent);
    letter-spacing: 0.15em;
    font-weight: 500;
    background: rgba(94, 234, 212, 0.1);
    padding: 4px 8px;
    border-radius: 4px;
    flex-shrink: 0;
  }
  .section-intro {
    color: var(--text-2);
    margin-bottom: 1.5rem;
    max-width: 60ch;
    font-size: 0.97rem;
  }

  /* ============ MAIN SIMULATOR ============ */
  .viz-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.25rem;
    margin-bottom: 1.5rem;
  }
  #wave-canvas {
    width: 100%;
    height: 240px;
    display: block;
    border-radius: 8px;
    background: var(--bg);
  }
  .canvas-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 0.85rem;
    padding: 0 4px;
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--text-3);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }
  .canvas-meta .live-tag {
    color: var(--accent);
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .canvas-meta .live-tag::before {
    content: '';
    width: 6px; height: 6px;
    background: var(--accent);
    border-radius: 50%;
    box-shadow: 0 0 8px var(--accent);
    animation: pulse 1.5s ease-in-out infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.4; }
  }

  .control-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }
  .control-row { margin-bottom: 1.5rem; }
  .control-row:last-child { margin-bottom: 0; }
  .control-head {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 10px;
    flex-wrap: wrap;
    gap: 6px;
  }
  .control-label {
    font-family: var(--font-mono);
    font-size: 11px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--text-3);
  }
  .control-value {
    font-family: var(--font-mono);
    font-size: 14px;
    font-weight: 500;
    color: var(--text);
  }
  input[type="range"] {
    -webkit-appearance: none;
    appearance: none;
    width: 100%;
    height: 4px;
    background: rgba(255,255,255,0.08);
    border-radius: 2px;
    outline: none;
    cursor: pointer;
  }
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 20px;
    height: 20px;
    background: var(--accent);
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 0 0 4px rgba(94, 234, 212, 0.15), 0 0 12px rgba(94, 234, 212, 0.4);
    transition: box-shadow 0.15s, transform 0.15s;
  }
  input[type="range"]::-webkit-slider-thumb:hover {
    box-shadow: 0 0 0 6px rgba(94, 234, 212, 0.25), 0 0 16px rgba(94, 234, 212, 0.6);
    transform: scale(1.08);
  }
  input[type="range"]::-moz-range-thumb {
    width: 20px;
    height: 20px;
    background: var(--accent);
    border-radius: 50%;
    cursor: pointer;
    border: none;
    box-shadow: 0 0 0 4px rgba(94, 234, 212, 0.15);
  }
  input[type="range"].warm::-webkit-slider-thumb {
    background: var(--accent-warm);
    box-shadow: 0 0 0 4px rgba(251, 191, 36, 0.15), 0 0 12px rgba(251, 191, 36, 0.4);
  }
  input[type="range"].warm::-moz-range-thumb {
    background: var(--accent-warm);
  }

  /* Stats */
  .stat-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-bottom: 2rem;
  }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1.1rem 1.25rem;
  }
  .stat-label {
    font-family: var(--font-mono);
    font-size: 10px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--text-3);
    margin-bottom: 8px;
  }
  .stat-value {
    font-family: var(--font-mono);
    font-size: 1.2rem;
    font-weight: 500;
    color: var(--text);
    word-break: break-all;
  }
  .regime-pill {
    display: inline-block;
    padding: 5px 12px;
    border-radius: 999px;
    font-family: var(--font-mono);
    font-size: 12px;
    font-weight: 500;
    border: 1px solid currentColor;
    margin-top: 4px;
  }
  .r-purple { color: var(--accent-violet); background: rgba(167, 139, 250, 0.1); }
  .r-teal { color: var(--accent); background: rgba(94, 234, 212, 0.1); }
  .r-amber { color: var(--accent-warm); background: rgba(251, 191, 36, 0.1); }
  .r-gray { color: var(--text-3); background: rgba(107, 114, 128, 0.1); }

  /* ============ ESCALA UNIVERSAL ============ */
  .scale-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 2rem 1.5rem 1.5rem;
    position: relative;
  }
  .scale-track-wrap {
    position: relative;
    height: 200px;
    margin: 1rem 0 0.5rem;
  }
  .scale-track {
    position: absolute;
    top: 100px;
    left: 4%;
    right: 4%;
    height: 2px;
    background: linear-gradient(to right,
      rgba(167, 139, 250, 0.4),
      rgba(94, 234, 212, 0.6),
      rgba(251, 191, 36, 0.4));
  }
  .scale-ref {
    position: absolute;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    max-width: 80px;
  }
  .scale-ref.above { top: 30px; }
  .scale-ref.below { top: 110px; }
  .scale-ref .dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: var(--text-3);
    border: 2px solid var(--surface);
  }
  .scale-ref.above .dot { order: 2; margin-top: 2px; }
  .scale-ref.below .dot { order: 0; margin-bottom: 2px; }
  .scale-ref .name {
    font-size: 11px;
    color: var(--text-2);
    text-align: center;
    line-height: 1.2;
  }
  .scale-ref .size {
    font-family: var(--font-mono);
    font-size: 9px;
    color: var(--text-3);
    text-align: center;
    letter-spacing: 0.04em;
  }
  .scale-marker {
    position: absolute;
    top: 70px;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: left 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    z-index: 5;
  }
  .scale-marker .pin {
    width: 0;
    height: 0;
    border-left: 7px solid transparent;
    border-right: 7px solid transparent;
    border-top: 12px solid var(--accent-warm);
    filter: drop-shadow(0 0 6px var(--accent-warm));
  }
  .scale-marker .dot-marker {
    width: 12px;
    height: 12px;
    background: var(--accent-warm);
    border-radius: 50%;
    margin-top: -2px;
    box-shadow: 0 0 12px var(--accent-warm);
    animation: pulse-marker 2s ease-in-out infinite;
  }
  @keyframes pulse-marker {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.2); }
  }
  .scale-marker .marker-label {
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--accent-warm);
    margin-top: 6px;
    background: var(--bg);
    padding: 3px 8px;
    border-radius: 4px;
    border: 1px solid var(--accent-warm);
    white-space: nowrap;
  }
  .scale-context {
    text-align: center;
    margin-top: 1rem;
    padding: 1rem;
    background: var(--surface-2);
    border-radius: 8px;
    font-size: 0.92rem;
    color: var(--text-2);
    min-height: 50px;
  }
  .scale-context strong { color: var(--accent-warm); font-weight: 500; }

  /* ============ PRESETS ============ */
  .preset-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 10px;
  }
  .preset-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 16px;
    cursor: pointer;
    text-align: left;
    font-family: inherit;
    color: var(--text);
    transition: all 0.18s ease;
    position: relative;
    overflow: hidden;
  }
  .preset-btn::before {
    content: '';
    position: absolute;
    left: 0; top: 0; bottom: 0;
    width: 3px;
    background: var(--accent);
    transform: scaleY(0);
    transition: transform 0.2s;
  }
  .preset-btn:hover {
    background: var(--surface-2);
    border-color: var(--border-strong);
    transform: translateY(-2px);
  }
  .preset-btn:hover::before { transform: scaleY(1); }
  .preset-btn.active {
    border-color: var(--accent);
    background: var(--surface-2);
  }
  .preset-btn.active::before { transform: scaleY(1); }
  .preset-icon {
    font-family: var(--font-mono);
    font-size: 18px;
    color: var(--accent);
    margin-bottom: 6px;
  }
  .preset-name {
    font-size: 14px;
    font-weight: 500;
    margin-bottom: 4px;
  }
  .preset-detail {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--text-3);
    letter-spacing: 0.04em;
  }

  /* ============ DOUBLE SLIT ============ */
  .slit-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.25rem;
  }
  #slit-canvas {
    width: 100%;
    height: 320px;
    background: #050811;
    border-radius: 8px;
    display: block;
  }
  .slit-controls {
    display: flex;
    gap: 8px;
    margin-top: 1rem;
    flex-wrap: wrap;
    align-items: center;
  }
  .btn {
    background: transparent;
    border: 1px solid var(--border-strong);
    color: var(--text);
    padding: 10px 16px;
    border-radius: 8px;
    font-family: var(--font-mono);
    font-size: 11px;
    letter-spacing: 0.1em;
    cursor: pointer;
    transition: all 0.15s;
    text-transform: uppercase;
    font-weight: 500;
  }
  .btn:hover { background: var(--surface-2); border-color: var(--accent); color: var(--accent); }
  .btn-primary {
    background: var(--accent);
    color: var(--bg);
    border-color: var(--accent);
  }
  .btn-primary:hover { background: #7ff0d8; color: var(--bg); border-color: #7ff0d8; }
  .btn-warm {
    background: var(--accent-warm);
    color: var(--bg);
    border-color: var(--accent-warm);
  }
  .btn-warm:hover { background: #fcc94a; color: var(--bg); border-color: #fcc94a; }
  .slit-counter {
    margin-left: auto;
    font-family: var(--font-mono);
    font-size: 12px;
    color: var(--text-2);
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }
  .slit-counter strong {
    color: var(--accent);
    font-weight: 500;
    font-size: 15px;
  }

  /* ============ VOLTAGE MODE ============ */
  .voltage-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.5rem;
  }
  .voltage-display {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
    gap: 12px;
    margin-top: 1.25rem;
  }
  .voltage-display .stat-card {
    background: var(--surface-2);
    text-align: center;
    border: none;
  }
  .voltage-display .stat-value {
    font-size: 1.05rem;
  }
  .voltage-compare {
    margin-top: 1.5rem;
    padding: 1.25rem;
    background: var(--surface-2);
    border-radius: 10px;
  }
  .compare-title {
    font-family: var(--font-mono);
    font-size: 10px;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--text-3);
    margin-bottom: 1rem;
  }
  .compare-bars { display: flex; flex-direction: column; gap: 10px; }
  .compare-bar {
    display: grid;
    grid-template-columns: 140px 1fr 90px;
    gap: 12px;
    align-items: center;
  }
  .compare-bar .label {
    font-size: 12px;
    color: var(--text-2);
  }
  .compare-bar .track {
    height: 8px;
    background: rgba(255,255,255,0.04);
    border-radius: 4px;
    position: relative;
    overflow: hidden;
  }
  .compare-bar .fill {
    height: 100%;
    border-radius: 4px;
    transition: width 0.4s ease;
  }
  .compare-bar .val {
    font-family: var(--font-mono);
    font-size: 11px;
    color: var(--text);
    text-align: right;
  }

  /* ============ REALITY SLIDER ============ */
  .reality-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.5rem;
    position: relative;
    overflow: hidden;
  }
  #reality-canvas {
    width: 100%;
    height: 220px;
    background: #050811;
    border-radius: 8px;
    display: block;
    margin-bottom: 1rem;
  }
  .reality-narrator {
    background: var(--surface-2);
    border-left: 3px solid var(--accent-warm);
    padding: 1rem 1.25rem;
    border-radius: 6px;
    margin-top: 1rem;
    font-size: 0.95rem;
    line-height: 1.65;
    min-height: 80px;
  }
  .reality-narrator strong { color: var(--accent-warm); font-weight: 500; }

  /* ============ THEORY ============ */
  .prose {
    color: var(--text-2);
    line-height: 1.75;
  }
  .prose p { margin-bottom: 1rem; max-width: 64ch; }
  .prose p strong { color: var(--text); font-weight: 500; }
  .prose p em { color: var(--accent-warm); font-style: italic; }

  .explain-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 12px;
    margin-top: 1rem;
  }
  .explain-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-left: 2px solid var(--accent);
    border-radius: 8px;
    padding: 1.25rem 1.5rem;
  }
  .explain-card:nth-child(2n) { border-left-color: var(--accent-warm); }
  .explain-card:nth-child(3n) { border-left-color: var(--accent-violet); }
  .explain-card h3 {
    font-family: var(--font-display);
    font-size: 1.1rem;
    font-weight: 500;
    margin-bottom: 0.5rem;
    color: var(--text);
  }
  .explain-card p {
    color: var(--text-2);
    font-size: 0.93rem;
    line-height: 1.65;
  }

  /* ============ QUIZ ============ */
  .quiz-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.75rem;
  }
  .quiz-progress {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
  }
  .quiz-step-tag {
    font-family: var(--font-mono);
    font-size: 11px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--accent);
  }
  .quiz-score {
    font-family: var(--font-mono);
    font-size: 12px;
    color: var(--text-2);
  }
  .quiz-score strong { color: var(--accent-warm); }
  .quiz-progress-bar {
    height: 3px;
    background: rgba(255,255,255,0.05);
    border-radius: 2px;
    overflow: hidden;
    margin-bottom: 1.5rem;
  }
  .quiz-progress-fill {
    height: 100%;
    background: var(--accent);
    transition: width 0.3s ease;
    border-radius: 2px;
  }
  .quiz-question {
    font-family: var(--font-display);
    font-size: 1.2rem;
    line-height: 1.4;
    margin-bottom: 1.25rem;
    color: var(--text);
    font-weight: 400;
  }
  .quiz-options { display: flex; flex-direction: column; gap: 8px; }
  .quiz-option {
    background: var(--surface-2);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 14px 18px;
    border-radius: 8px;
    text-align: left;
    cursor: pointer;
    font-family: inherit;
    font-size: 0.95rem;
    transition: all 0.15s;
    line-height: 1.4;
  }
  .quiz-option:hover:not(:disabled) {
    background: var(--surface-3);
    border-color: var(--accent);
    transform: translateX(2px);
  }
  .quiz-option:disabled { cursor: not-allowed; }
  .quiz-option.correct {
    background: rgba(94, 234, 212, 0.12);
    border-color: var(--accent);
    color: var(--accent);
  }
  .quiz-option.wrong {
    background: rgba(251, 113, 133, 0.12);
    border-color: var(--danger);
    color: var(--danger);
  }
  .quiz-feedback {
    margin-top: 1rem;
    padding: 1rem 1.25rem;
    background: var(--surface-2);
    border-radius: 8px;
    font-size: 0.92rem;
    color: var(--text-2);
    line-height: 1.6;
    display: none;
    border-left: 3px solid var(--accent);
  }
  .quiz-feedback.visible { display: block; animation: slideIn 0.3s ease; }
  @keyframes slideIn {
    from { opacity: 0; transform: translateY(-4px); }
    to { opacity: 1; transform: translateY(0); }
  }
  .quiz-feedback strong { color: var(--text); font-weight: 500; }
  .quiz-result {
    text-align: center;
    padding: 2rem 1rem;
  }
  .quiz-result .score-big {
    font-family: var(--font-display);
    font-size: 4rem;
    font-weight: 400;
    color: var(--accent);
    line-height: 1;
    margin-bottom: 0.5rem;
  }
  .quiz-result .score-label {
    font-family: var(--font-mono);
    font-size: 12px;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--text-3);
    margin-bottom: 1rem;
  }
  .quiz-result .verdict {
    font-family: var(--font-display);
    font-size: 1.4rem;
    color: var(--text);
    margin-bottom: 0.5rem;
  }
  .quiz-result .verdict-sub {
    color: var(--text-2);
    margin-bottom: 1.5rem;
  }

  /* ============ FOOTER ============ */
  .footer {
    margin-top: 5rem;
    padding-top: 2rem;
    border-top: 1px solid var(--border);
    font-family: var(--font-mono);
    font-size: 10px;
    color: var(--text-3);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 1rem;
  }

  /* ============ RESPONSIVE ============ */
  @media (max-width: 600px) {
    body { padding: 2rem 1rem 3rem; }
    .stat-grid { grid-template-columns: 1fr; }
    .control-head { flex-direction: column; align-items: flex-start; }
    .compare-bar { grid-template-columns: 1fr; gap: 4px; }
    .compare-bar .val { text-align: left; }
    .scale-ref { max-width: 60px; }
    .scale-ref .name { font-size: 9px; }
    .slit-counter { margin-left: 0; width: 100%; }
  }
</style>
</head>
<body>

<div class="backdrop"><canvas id="backdrop-canvas"></canvas></div>

<div class="container">

  <!-- ============ HERO ============ -->
  <header class="hero">
    <div class="eyebrow">Física moderna · Laboratorio interactivo</div>
    <h1>La hipótesis de <em>De Broglie</em><br>en cinco experimentos</h1>
    <p class="lead">
      En 1924 un estudiante de doctorado francés propuso que toda la materia se comporta como onda. Su tribunal pensó que estaba loco y pidió la opinión de Einstein. Tres años después, los electrones se difractaban en un laboratorio de Bell. Aquí puedes reproducir esa revolución.
    </p>
  </header>

  <!-- ============ NAV ============ -->
  <nav class="nav-chips">
    <button class="nav-chip" data-target="simulador">01 · Onda</button>
    <button class="nav-chip" data-target="escala">02 · Escala</button>
    <button class="nav-chip" data-target="presets">03 · Partículas</button>
    <button class="nav-chip" data-target="doble-rendija">04 · Doble rendija</button>
    <button class="nav-chip" data-target="microscopio">05 · Microscopio</button>
    <button class="nav-chip" data-target="realidad">06 · Realidad alterna</button>
    <button class="nav-chip" data-target="teoria">07 · Teoría</button>
    <button class="nav-chip" data-target="quiz">08 · Quiz</button>
  </nav>

  <!-- ============ FORMULA ============ -->
  <div class="formula-card">
    <div class="formula-label">Ecuación fundamental</div>
    <div class="formula">
      <span class="lambda">λ</span><span class="op">=</span><span class="var">h</span><span class="op">/</span>(<span class="var">m</span><span class="op">·</span><span class="var">v</span>)
    </div>
    <div class="formula-legend">
      donde <em>h</em> es la constante de Planck (6.626 × 10⁻³⁴ J·s), <em>m</em> la masa y <em>v</em> la velocidad de la partícula
    </div>
  </div>

  <!-- ============ SECTION 01: SIMULADOR ============ -->
  <div class="section" id="simulador">
    <h2 class="section-title"><span class="num">01</span> Visualización de la onda</h2>
    <p class="section-intro">
      Mueve los sliders para cambiar la masa y la velocidad de la partícula. La onda animada representa la onda de materia asociada; la esfera amarilla es la partícula misma. Cuando la longitud de onda real es atómica, la ves bien; cuando es macroscópica, se aplana porque λ es absurdamente pequeña.
    </p>

    <div class="viz-card">
      <canvas id="wave-canvas" width="780" height="240"></canvas>
      <div class="canvas-meta">
        <span class="live-tag">En vivo</span>
        <span id="wavelength-tag">λ ≈ 7.27 × 10⁻¹⁰ m</span>
      </div>
    </div>

    <div class="control-block">
      <div class="control-row">
        <div class="control-head">
          <span class="control-label">Masa de la partícula</span>
          <span class="control-value" id="mass-val">9.11e-31 kg</span>
        </div>
        <input type="range" id="mass-slider" min="0" max="100" value="0" step="0.5" aria-label="Masa">
      </div>
      <div class="control-row">
        <div class="control-head">
          <span class="control-label">Velocidad</span>
          <span class="control-value" id="vel-val">1.00e+06 m/s</span>
        </div>
        <input type="range" id="vel-slider" min="0" max="100" value="80" step="0.5" aria-label="Velocidad">
      </div>
    </div>

    <div class="stat-grid">
      <div class="stat-card">
        <div class="stat-label">Longitud de onda · λ</div>
        <div class="stat-value" id="lambda-val">7.27e-10 m</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">Régimen físico</div>
        <div><span class="regime-pill r-teal" id="regime-pill">Cuántico observable</span></div>
      </div>
    </div>
  </div>

  <!-- ============ SECTION 02: ESCALA UNIVERSAL ============ -->
  <div class="section" id="escala">
    <h2 class="section-title"><span class="num">02</span> Escala del universo</h2>
    <p class="section-intro">
      ¿Qué tan grande es realmente la longitud de onda que calculaste? Aquí está comparada con todo lo que conocemos, desde la longitud de Planck (la distancia más pequeña con sentido físico) hasta el universo observable. El marcador amarillo se mueve en vivo con tus controles.
    </p>

    <div class="scale-card">
      <div class="scale-track-wrap" id="scale-track-wrap">
        <div class="scale-track"></div>
        <!-- Referencias se insertan con JS -->
        <div class="scale-marker" id="scale-marker" style="left: 50%;">
          <div class="pin"></div>
          <div class="dot-marker"></div>
          <div class="marker-label" id="marker-label">λ actual</div>
        </div>
      </div>
      <div class="scale-context" id="scale-context">
        La longitud de onda del electrón es <strong>comparable al tamaño de un átomo</strong>. Por eso los electrones se difractan al pasar por una red cristalina, como en el experimento de Davisson-Germer.
      </div>
    </div>
  </div>

  <!-- ============ SECTION 03: PRESETS ============ -->
  <div class="section" id="presets">
    <h2 class="section-title"><span class="num">03</span> Partículas para explorar</h2>
    <p class="section-intro">
      Saltos rápidos entre escenarios típicos. Compara cómo cambia el régimen al pasar de un electrón a una pelota de tenis.
    </p>
    <div class="preset-grid">
      <button class="preset-btn" data-m="9.11e-31" data-v="1e6">
        <div class="preset-icon">e⁻</div>
        <div class="preset-name">Electrón</div>
        <div class="preset-detail">v = 10⁶ m/s</div>
      </button>
      <button class="preset-btn" data-m="1.67e-27" data-v="1e4">
        <div class="preset-icon">p⁺</div>
        <div class="preset-name">Protón</div>
        <div class="preset-detail">v = 10⁴ m/s</div>
      </button>
      <button class="preset-btn" data-m="6.64e-27" data-v="1500">
        <div class="preset-icon">He</div>
        <div class="preset-name">Átomo de helio</div>
        <div class="preset-detail">v = 1500 m/s</div>
      </button>
      <button class="preset-btn" data-m="2.66e-26" data-v="500">
        <div class="preset-icon">C₆₀</div>
        <div class="preset-name">Fullereno</div>
        <div class="preset-detail">v = 500 m/s</div>
      </button>
      <button class="preset-btn" data-m="1e-15" data-v="0.01">
        <div class="preset-icon">·</div>
        <div class="preset-name">Virus</div>
        <div class="preset-detail">v = 1 cm/s</div>
      </button>
      <button class="preset-btn" data-m="0.058" data-v="50">
        <div class="preset-icon">●</div>
        <div class="preset-name">Pelota de tenis</div>
        <div class="preset-detail">v = 50 m/s</div>
      </button>
      <button class="preset-btn" data-m="70" data-v="1.4">
        <div class="preset-icon">⚇</div>
        <div class="preset-name">Persona caminando</div>
        <div class="preset-detail">v = 1.4 m/s</div>
      </button>
      <button class="preset-btn" data-m="1500" data-v="30">
        <div class="preset-icon">⚙</div>
        <div class="preset-name">Automóvil</div>
        <div class="preset-detail">v = 108 km/h</div>
      </button>
    </div>
  </div>

  <!-- ============ SECTION 04: DOBLE RENDIJA ============ -->
  <div class="section" id="doble-rendija">
    <h2 class="section-title"><span class="num">04</span> Experimento de doble rendija</h2>
    <p class="section-intro">
      Esto es lo que Davisson y Germer vieron en 1927: si disparas partículas una por una contra una barrera con dos rendijas, no se acumulan en dos franjas como esperarías de balas, sino en un patrón de interferencia. Cada partícula "decide" dónde caer según su propia onda. La separación de las franjas depende directamente de λ. Prueba primero con un electrón, luego con una pelota.
    </p>

    <div class="slit-card">
      <canvas id="slit-canvas" width="780" height="320"></canvas>
      <div class="slit-controls">
        <button class="btn btn-primary" id="fire-one">Disparar 1</button>
        <button class="btn" id="fire-many">Disparar 100</button>
        <button class="btn" id="fire-auto">Auto · disparo continuo</button>
        <button class="btn" id="slit-reset">Reiniciar</button>
        <div class="slit-counter">
          Partículas detectadas: <strong id="slit-count">0</strong>
        </div>
      </div>
    </div>
  </div>

  <!-- ============ SECTION 05: MICROSCOPIO ELECTRÓNICO ============ -->
  <div class="section" id="microscopio">
    <h2 class="section-title"><span class="num">05</span> Modo microscopio electrónico</h2>
    <p class="section-intro">
      En un microscopio electrónico real no controlas la velocidad directamente: aplicas un voltaje que acelera los electrones. La energía cinética resultante es <em>E = eV</em>, y la longitud de onda es <em>λ = h/√(2m·eV)</em>. Por eso un electrón a 100 kV "ve" cosas 100,000 veces más pequeñas que la luz visible.
    </p>

    <div class="voltage-card">
      <div class="control-row">
        <div class="control-head">
          <span class="control-label">Voltaje de aceleración</span>
          <span class="control-value" id="voltage-val">100 V</span>
        </div>
        <input type="range" id="voltage-slider" class="warm" min="0" max="100" value="40" step="0.5" aria-label="Voltaje">
      </div>

      <div class="voltage-display">
        <div class="stat-card">
          <div class="stat-label">Energía cinética</div>
          <div class="stat-value" id="ke-val">100 eV</div>
        </div>
        <div class="stat-card">
          <div class="stat-label">Velocidad electrón</div>
          <div class="stat-value" id="ev-val">5.93e+06 m/s</div>
        </div>
        <div class="stat-card">
          <div class="stat-label">Longitud de onda</div>
          <div class="stat-value" id="el-val">0.123 nm</div>
        </div>
      </div>

      <div class="voltage-compare">
        <div class="compare-title">Comparado con otras "luces"</div>
        <div class="compare-bars" id="compare-bars">
          <!-- bars insertadas con JS -->
        </div>
      </div>
    </div>
  </div>

  <!-- ============ SECTION 06: REALIDAD ALTERNA ============ -->
  <div class="section" id="realidad">
    <h2 class="section-title"><span class="num">06</span> ¿Y si la constante de Planck fuera más grande?</h2>
    <p class="section-intro">
      Un experimento mental: ¿qué pasaría si el universo tuviera una <em>h</em> diferente? Mueve el slider y observa cómo una simple pelota de tenis empieza a mostrar comportamiento ondulatorio. Esto es exactamente lo que vivimos los electrones todo el tiempo.
    </p>

    <div class="reality-card">
      <canvas id="reality-canvas" width="780" height="220"></canvas>
      <div class="control-row">
        <div class="control-head">
          <span class="control-label">h efectiva × </span>
          <span class="control-value" id="h-mult-val">1 × (la realidad)</span>
        </div>
        <input type="range" id="h-mult-slider" class="warm" min="0" max="100" value="0" step="0.5">
      </div>
      <div class="reality-narrator" id="reality-narrator">
        En el universo real, la pelota de tenis tiene <strong>λ ≈ 1.1 × 10⁻³⁴ m</strong>, trillones de veces más pequeña que un protón. Imposible de medir. Por eso parece comportarse como un punto sólido y la física clásica funciona.
      </div>
    </div>
  </div>

  <!-- ============ SECTION 07: TEORÍA ============ -->
  <div class="section" id="teoria">
    <h2 class="section-title"><span class="num">07</span> La teoría detrás</h2>
    <div class="prose">
      <p>
        Antes de 1924 ya se sabía que la luz se comportaba como onda (interferencia, difracción) pero también como partícula (efecto fotoeléctrico). De Broglie hizo una pregunta inversa: <strong>si las ondas pueden ser partículas, ¿pueden las partículas ser ondas?</strong>
      </p>
      <p>
        Su respuesta fue una sola ecuación. A toda partícula de momento <em>p = mv</em> le corresponde una longitud de onda <em>λ = h/p</em>. No es una analogía: la partícula realmente se difracta, interfiere y se comporta como onda en condiciones apropiadas.
      </p>
    </div>

    <div class="explain-grid">
      <div class="explain-card">
        <h3>Davisson-Germer · 1927</h3>
        <p>Dispararon electrones contra un cristal de níquel. En vez de rebotar como balas, formaron un patrón de difracción idéntico al de los rayos X. La longitud de onda medida coincidió exactamente con la predicción de de Broglie.</p>
      </div>

      <div class="explain-card">
        <h3>Forma alternativa</h3>
        <p>Cuando conoces la energía cinética en lugar de la velocidad: <em>λ = h / √(2mE)</em>. Para electrones acelerados con voltaje V hay un atajo útil: <em>λ ≈ 1.226 / √V nm</em>.</p>
      </div>

      <div class="explain-card">
        <h3>Por qué Newton sigue funcionando</h3>
        <p>La constante de Planck es ridículamente pequeña (10⁻³⁴). Una pelota de tenis tiene λ ≈ 10⁻³⁴ m, billones de veces menor que un protón. Por eso los efectos cuánticos son invisibles a nuestra escala.</p>
      </div>

      <div class="explain-card">
        <h3>El récord actual</h3>
        <p>En 2019 científicos en Viena hicieron interferir moléculas de 25,000 unidades de masa atómica. Ese es el objeto más grande conocido en mostrar comportamiento ondulatorio. La frontera entre lo cuántico y lo clásico sigue moviéndose hacia arriba.</p>
      </div>

      <div class="explain-card">
        <h3>El microscopio electrónico</h3>
        <p>Un microscopio óptico no puede resolver detalles más pequeños que la longitud de onda de la luz visible (~500 nm). Un electrón a 100 kV tiene λ ≈ 0.0037 nm: 130,000 veces más resolución.</p>
      </div>

      <div class="explain-card">
        <h3>Condensados de Bose-Einstein</h3>
        <p>Si enfrías átomos a nanokelvins, su velocidad cae tanto que λ se vuelve macroscópica y miles de átomos se sincronizan en una sola onda. Es la consecuencia más espectacular y visible de la dualidad.</p>
      </div>
    </div>
  </div>

  <!-- ============ SECTION 08: QUIZ ============ -->
  <div class="section" id="quiz">
    <h2 class="section-title"><span class="num">08</span> Pon a prueba tu intuición</h2>
    <p class="section-intro">
      Seis preguntas para confirmar que entendiste. No hay puntos perdidos por equivocarte, pero cada respuesta tiene una explicación corta.
    </p>

    <div class="quiz-card" id="quiz-card">
      <!-- contenido del quiz lo maneja JS -->
    </div>
  </div>

  <!-- ============ FOOTER ============ -->
  <div class="footer">
    <span>Laboratorio De Broglie · ITFIP El Espinal</span>
    <span>λ = h / mv</span>
  </div>

</div>

<script>
// ==================================================================
// CONSTANTES FÍSICAS
// ==================================================================
const h_const = 6.62607015e-34;
const m_e = 9.1093837e-31;
const e_const = 1.602176634e-19;

// Estado global compartido
let currentLambda = 7.27e-10;
let currentScreenLambda = 100;

// ==================================================================
// UTILIDADES
// ==================================================================
function fmtSci(n, digits) {
  if (n === 0 || !isFinite(n)) return '0';
  digits = digits || 2;
  const exp = Math.floor(Math.log10(Math.abs(n)));
  const mant = n / Math.pow(10, exp);
  const sign = exp >= 0 ? '+' : '';
  return mant.toFixed(digits) + 'e' + sign + exp;
}

function fmtSciNice(n) {
  if (n === 0 || !isFinite(n)) return '0';
  const exp = Math.floor(Math.log10(Math.abs(n)));
  const mant = n / Math.pow(10, exp);
  const supMap = { '-':'⁻', '0':'⁰','1':'¹','2':'²','3':'³','4':'⁴','5':'⁵','6':'⁶','7':'⁷','8':'⁸','9':'⁹' };
  const supExp = String(exp).split('').map(c => supMap[c] || c).join('');
  return mant.toFixed(2) + ' × 10' + supExp;
}

function logMap(value, fromMin, fromMax, toMin, toMax) {
  const t = (value - fromMin) / (fromMax - fromMin);
  return toMin + t * (toMax - toMin);
}

function dpiCanvas(canvas) {
  const ctx = canvas.getContext('2d');
  const dpr = window.devicePixelRatio || 1;
  const w = canvas.offsetWidth;
  const hgt = canvas.offsetHeight;
  canvas.width = w * dpr;
  canvas.height = hgt * dpr;
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  return { ctx, W: w, H: hgt };
}

// ==================================================================
// FONDO ANIMADO (partículas flotantes)
// ==================================================================
(function() {
  const canvas = document.getElementById('backdrop-canvas');
  const ctx = canvas.getContext('2d');
  let particles = [];
  
  function resize() {
    const dpr = window.devicePixelRatio || 1;
    canvas.width = window.innerWidth * dpr;
    canvas.height = window.innerHeight * dpr;
    canvas.style.width = window.innerWidth + 'px';
    canvas.style.height = window.innerHeight + 'px';
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  }
  
  function init() {
    particles = [];
    const count = Math.min(40, Math.floor(window.innerWidth / 30));
    for (let i = 0; i < count; i++) {
      particles.push({
        x: Math.random() * window.innerWidth,
        y: Math.random() * window.innerHeight,
        vx: (Math.random() - 0.5) * 0.15,
        vy: (Math.random() - 0.5) * 0.15,
        r: Math.random() * 1.5 + 0.3,
        a: Math.random() * 0.5 + 0.2
      });
    }
  }
  
  function tick() {
    ctx.clearRect(0, 0, window.innerWidth, window.innerHeight);
    for (const p of particles) {
      p.x += p.vx;
      p.y += p.vy;
      if (p.x < 0) p.x = window.innerWidth;
      if (p.x > window.innerWidth) p.x = 0;
      if (p.y < 0) p.y = window.innerHeight;
      if (p.y > window.innerHeight) p.y = 0;
      
      ctx.fillStyle = 'rgba(94, 234, 212, ' + p.a + ')';
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fill();
    }
    requestAnimationFrame(tick);
  }
  
  resize();
  init();
  tick();
  window.addEventListener('resize', () => { resize(); init(); });
})();

// ==================================================================
// NAVEGACIÓN
// ==================================================================
document.querySelectorAll('.nav-chip').forEach(chip => {
  chip.addEventListener('click', () => {
    const target = document.getElementById(chip.dataset.target);
    if (target) {
      target.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
  });
});

// ==================================================================
// SIMULADOR PRINCIPAL (sección 01)
// ==================================================================
(function() {
  const canvas = document.getElementById('wave-canvas');
  const ctx = canvas.getContext('2d');
  const massSlider = document.getElementById('mass-slider');
  const velSlider = document.getElementById('vel-slider');
  const massVal = document.getElementById('mass-val');
  const velVal = document.getElementById('vel-val');
  const lambdaVal = document.getElementById('lambda-val');
  const wavelengthTag = document.getElementById('wavelength-tag');
  const regimePill = document.getElementById('regime-pill');
  const presetBtns = document.querySelectorAll('.preset-btn');

  function resize() {
    const r = dpiCanvas(canvas);
    return r;
  }
  let dims = resize();

  function getMass() {
    return Math.pow(10, -31 + 33 * (massSlider.value / 100));
  }
  function getVelocity() {
    return Math.pow(10, -2 + 10 * (velSlider.value / 100));
  }

  function getRegime(lambda) {
    if (lambda > 1e-9) return { cls: 'r-purple', text: 'Cuántico amplio' };
    if (lambda > 1e-12) return { cls: 'r-teal', text: 'Cuántico observable' };
    if (lambda > 1e-18) return { cls: 'r-amber', text: 'Transición' };
    return { cls: 'r-gray', text: 'Clásico no observable' };
  }

  let animTime = 0;
  let lastUpdate = performance.now();

  function computeScreenLambda(lambda) {
    const logL = Math.log10(lambda);
    if (logL < -36) return 0.5;
    if (logL > -7) return 320;
    const t = (logL + 36) / 29;
    return Math.pow(10, 0.1 + 2.5 * t);
  }

  function draw(lambda) {
    const { W, H } = dims;
    ctx.clearRect(0, 0, W, H);

    // grid
    ctx.strokeStyle = 'rgba(255, 255, 255, 0.03)';
    ctx.lineWidth = 1;
    for (let x = 0; x < W; x += 40) {
      ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, H); ctx.stroke();
    }
    for (let y = 0; y < H; y += 40) {
      ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(W, y); ctx.stroke();
    }

    // axis
    ctx.strokeStyle = 'rgba(255, 255, 255, 0.1)';
    ctx.lineWidth = 1;
    ctx.beginPath();
    ctx.moveTo(0, H/2); ctx.lineTo(W, H/2); ctx.stroke();

    const screenLambda = computeScreenLambda(lambda);
    currentScreenLambda = screenLambda;
    const amp = Math.min(75, H/2 - 30);
    const phase = animTime * 50;

    // glow
    if (screenLambda >= 1.2) {
      ctx.strokeStyle = 'rgba(94, 234, 212, 0.2)';
      ctx.lineWidth = 9;
      ctx.beginPath();
      for (let x = 0; x <= W; x++) {
        const y = H/2 - amp * Math.sin(2 * Math.PI * (x - phase) / screenLambda);
        if (x === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
      }
      ctx.stroke();
    }

    // main wave
    ctx.strokeStyle = '#5eead4';
    ctx.lineWidth = 2;
    ctx.beginPath();
    if (screenLambda < 1.2) {
      ctx.moveTo(0, H/2); ctx.lineTo(W, H/2);
    } else {
      for (let x = 0; x <= W; x++) {
        const y = H/2 - amp * Math.sin(2 * Math.PI * (x - phase) / screenLambda);
        if (x === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
      }
    }
    ctx.stroke();

    // particle
    const particleX = (animTime * 75) % W;
    const particleY = screenLambda < 1.2 ? H/2 : H/2 - amp * Math.sin(2 * Math.PI * (particleX - phase) / screenLambda);
    ctx.fillStyle = 'rgba(251, 191, 36, 0.3)';
    ctx.beginPath();
    ctx.arc(particleX, particleY, 14, 0, Math.PI * 2);
    ctx.fill();
    ctx.fillStyle = '#fbbf24';
    ctx.beginPath();
    ctx.arc(particleX, particleY, 5, 0, Math.PI * 2);
    ctx.fill();

    // bracket
    if (screenLambda >= 14 && screenLambda <= W * 0.8) {
      const startX = 24;
      const endX = startX + screenLambda;
      const y = H - 22;
      ctx.strokeStyle = 'rgba(255, 255, 255, 0.35)';
      ctx.lineWidth = 1;
      ctx.beginPath();
      ctx.moveTo(startX, y - 5); ctx.lineTo(startX, y);
      ctx.lineTo(endX, y); ctx.lineTo(endX, y - 5);
      ctx.stroke();
      ctx.fillStyle = 'rgba(255, 255, 255, 0.6)';
      ctx.font = '11px "JetBrains Mono", monospace';
      ctx.textAlign = 'center';
      ctx.fillText('λ', (startX + endX) / 2, y - 7);
    } else if (screenLambda < 14) {
      ctx.fillStyle = 'rgba(255, 255, 255, 0.4)';
      ctx.font = '11px "JetBrains Mono", monospace';
      ctx.textAlign = 'center';
      ctx.fillText('λ demasiado pequeña para visualizar', W/2, H - 12);
    }
  }

  function update() {
    const m = getMass();
    const v = getVelocity();
    const lambda = h_const / (m * v);
    currentLambda = lambda;

    massVal.textContent = fmtSci(m) + ' kg';
    velVal.textContent = fmtSci(v) + ' m/s';
    lambdaVal.textContent = fmtSci(lambda) + ' m';
    wavelengthTag.textContent = 'λ ≈ ' + fmtSciNice(lambda) + ' m';

    const r = getRegime(lambda);
    regimePill.textContent = r.text;
    regimePill.className = 'regime-pill ' + r.cls;

    draw(lambda);
    if (window.updateScale) window.updateScale(lambda);
  }

  function tick(now) {
    const dt = (now - lastUpdate) / 1000;
    lastUpdate = now;
    animTime += dt;
    update();
    requestAnimationFrame(tick);
  }

  window.setMassVel = function(m, v) {
    massSlider.value = 100 * (Math.log10(m) + 31) / 33;
    velSlider.value = 100 * (Math.log10(v) + 2) / 10;
    update();
  };

  massSlider.addEventListener('input', () => {
    presetBtns.forEach(b => b.classList.remove('active'));
    update();
  });
  velSlider.addEventListener('input', () => {
    presetBtns.forEach(b => b.classList.remove('active'));
    update();
  });

  presetBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      presetBtns.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      window.setMassVel(parseFloat(btn.dataset.m), parseFloat(btn.dataset.v));
    });
  });

  window.addEventListener('resize', () => { dims = resize(); update(); });

  window.setMassVel(9.11e-31, 1e6);
  presetBtns[0].classList.add('active');
  requestAnimationFrame(tick);
})();

// ==================================================================
// ESCALA UNIVERSAL (sección 02)
// ==================================================================
(function() {
  const wrap = document.getElementById('scale-track-wrap');
  const marker = document.getElementById('scale-marker');
  const markerLabel = document.getElementById('marker-label');
  const ctxBox = document.getElementById('scale-context');

  const refs = [
    { log: -35, name: 'Long. de Planck', size: '10⁻³⁵ m', pos: 'below' },
    { log: -18, name: 'Quark', size: '< 10⁻¹⁸ m', pos: 'above' },
    { log: -15, name: 'Protón', size: '10⁻¹⁵ m', pos: 'below' },
    { log: -10, name: 'Átomo', size: '10⁻¹⁰ m', pos: 'above' },
    { log: -8, name: 'ADN', size: '10⁻⁸ m', pos: 'below' },
    { log: -7, name: 'Virus', size: '10⁻⁷ m', pos: 'above' },
    { log: -5, name: 'Célula', size: '10⁻⁵ m', pos: 'below' },
    { log: -4, name: 'Cabello', size: '10⁻⁴ m', pos: 'above' },
    { log: 0, name: 'Humano', size: '1 m', pos: 'below' },
    { log: 7, name: 'Tierra', size: '10⁷ m', pos: 'above' },
    { log: 21, name: 'Galaxia', size: '10²¹ m', pos: 'below' },
    { log: 26, name: 'Universo', size: '10²⁶ m', pos: 'above' }
  ];

  const minLog = -38;
  const maxLog = 28;

  function logToPercent(logVal) {
    const clamped = Math.max(minLog, Math.min(maxLog, logVal));
    return 4 + ((clamped - minLog) / (maxLog - minLog)) * 92;
  }

  refs.forEach(ref => {
    const el = document.createElement('div');
    el.className = 'scale-ref ' + ref.pos;
    el.style.left = logToPercent(ref.log) + '%';
    el.innerHTML = '<div class="name">' + ref.name + '</div><div class="dot"></div><div class="size">' + ref.size + '</div>';
    wrap.appendChild(el);
  });

  function contextFor(logL) {
    if (logL < -35) return 'Estás <strong>por debajo de la longitud de Planck</strong>. No tiene sentido físico hablar de distancias más cortas según nuestra teoría actual.';
    if (logL < -18) return 'Más pequeño que cualquier partícula conocida. Aún la física moderna no sabe lo que pasa aquí.';
    if (logL < -15) return 'Escala <strong>sub-nuclear</strong>. Más pequeño que un protón. Físicamente imposible de medir con tecnología actual.';
    if (logL < -11) return 'Escala <strong>nuclear</strong>. Comparable al núcleo de un átomo.';
    if (logL < -8) return 'Escala <strong>atómica</strong>. Por eso los electrones se difractan al pasar por cristales.';
    if (logL < -6) return 'Escala <strong>molecular</strong>. Comparable al ADN o a moléculas grandes. ¡Frontera del experimento real!';
    if (logL < -3) return 'Escala <strong>celular</strong>. Si una partícula tuviera esta λ, la verías difractarse al pasar por un microscopio óptico.';
    if (logL < 0) return 'Escala <strong>visible</strong>. Esto sería imposible en la realidad pero te muestra cómo se vería el mundo cuántico ampliado.';
    return 'Escala <strong>astronómica</strong>. Más grande que un humano. Físicamente absurdo pero matemáticamente válido.';
  }

  window.updateScale = function(lambda) {
    const logL = Math.log10(lambda);
    const pct = logToPercent(logL);
    marker.style.left = pct + '%';
    markerLabel.textContent = 'λ = ' + fmtSciNice(lambda) + ' m';
    ctxBox.innerHTML = contextFor(logL);
  };
})();

// ==================================================================
// DOBLE RENDIJA (sección 04)
// ==================================================================
(function() {
  const canvas = document.getElementById('slit-canvas');
  const ctx = canvas.getContext('2d');
  const fireOneBtn = document.getElementById('fire-one');
  const fireManyBtn = document.getElementById('fire-many');
  const fireAutoBtn = document.getElementById('fire-auto');
  const resetBtn = document.getElementById('slit-reset');
  const counter = document.getElementById('slit-count');

  // Estado (declarar ANTES de cualquier función que los use)
  let hits = []; // {y, age}
  let particles = []; // partículas en vuelo
  let dims;
  let autoFire = false;
  let lastFire = 0;

  function resize() { dims = dpiCanvas(canvas); }
  resize();

  // Geometría
  function getGeom() {
    const W = dims.W;
    const H = dims.H;
    return {
      sourceX: 50,
      barrierX: W * 0.4,
      screenX: W * 0.92,
      slitGap: 28,
      slitWidth: 8,
      cy: H / 2,
      H: H,
      W: W
    };
  }

  function drawStatic() {
    const g = getGeom();
    ctx.clearRect(0, 0, g.W, g.H);

    // background grid
    ctx.fillStyle = '#050811';
    ctx.fillRect(0, 0, g.W, g.H);

    // Fuente
    ctx.fillStyle = '#5eead4';
    ctx.shadowColor = '#5eead4';
    ctx.shadowBlur = 12;
    ctx.beginPath();
    ctx.arc(g.sourceX, g.cy, 8, 0, Math.PI * 2);
    ctx.fill();
    ctx.shadowBlur = 0;
    ctx.fillStyle = 'rgba(255,255,255,0.6)';
    ctx.font = '10px "JetBrains Mono", monospace';
    ctx.textAlign = 'center';
    ctx.fillText('FUENTE', g.sourceX, g.cy - 18);

    // Barrera con rendijas
    ctx.fillStyle = '#2a3147';
    ctx.fillRect(g.barrierX - 4, 0, 8, g.cy - g.slitGap - g.slitWidth/2);
    ctx.fillRect(g.barrierX - 4, g.cy - g.slitGap + g.slitWidth/2, 8, 2*g.slitGap - g.slitWidth);
    ctx.fillRect(g.barrierX - 4, g.cy + g.slitGap + g.slitWidth/2, 8, g.H - (g.cy + g.slitGap + g.slitWidth/2));
    ctx.fillStyle = 'rgba(255,255,255,0.5)';
    ctx.fillText('BARRERA', g.barrierX, 14);

    // Pantalla detectora
    ctx.fillStyle = 'rgba(255,255,255,0.08)';
    ctx.fillRect(g.screenX - 2, 0, 4, g.H);
    ctx.fillStyle = 'rgba(255,255,255,0.5)';
    ctx.fillText('PANTALLA', g.screenX, 14);

    // Línea guía sutil
    ctx.strokeStyle = 'rgba(94, 234, 212, 0.08)';
    ctx.lineWidth = 1;
    ctx.beginPath();
    ctx.moveTo(g.sourceX, g.cy - g.slitGap);
    ctx.lineTo(g.barrierX, g.cy - g.slitGap);
    ctx.moveTo(g.sourceX, g.cy + g.slitGap);
    ctx.lineTo(g.barrierX, g.cy + g.slitGap);
    ctx.stroke();
  }

  function drawHits() {
    const g = getGeom();
    // dibujar histograma + hits
    // primero histograma como acumulación de intensidad
    const binSize = 4;
    const bins = {};
    for (const hit of hits) {
      const bin = Math.floor(hit.y / binSize);
      bins[bin] = (bins[bin] || 0) + 1;
    }
    const maxBin = Math.max(1, ...Object.values(bins));
    
    // Dibujar histograma en el lado izquierdo de la pantalla
    ctx.fillStyle = 'rgba(94, 234, 212, 0.15)';
    Object.keys(bins).forEach(b => {
      const y = parseInt(b) * binSize;
      const width = (bins[b] / maxBin) * 40;
      ctx.fillRect(g.screenX - width - 4, y, width, binSize);
    });

    // hits como puntos brillantes
    for (const hit of hits) {
      const alpha = Math.min(1, hit.age * 3);
      ctx.fillStyle = 'rgba(94, 234, 212, ' + (0.7 * alpha) + ')';
      ctx.beginPath();
      ctx.arc(g.screenX + 2, hit.y, 2, 0, Math.PI * 2);
      ctx.fill();
    }
  }

  // Distribución de interferencia
  function sampleInterference() {
    const g = getGeom();
    // Mapear screenLambda a fringe spacing en el detector
    // Usar currentScreenLambda como referencia base
    const fringeSpacing = Math.max(2, currentScreenLambda * 0.6);
    const sigma = g.H / 5; // envelope gaussiano

    let attempts = 0;
    while (attempts++ < 200) {
      const y = (Math.random() - 0.5) * g.H * 0.95;
      const envelope = Math.exp(-y*y / (2*sigma*sigma));
      const cos = Math.cos(Math.PI * y / fringeSpacing);
      const I = envelope * cos * cos;
      if (Math.random() < I) return g.cy + y;
    }
    // Fallback: gaussiana pura
    let y = 0;
    for (let i = 0; i < 6; i++) y += Math.random() - 0.5;
    return g.cy + y * sigma;
  }

  function fireParticle() {
    const g = getGeom();
    const targetY = sampleInterference();
    // 50/50 cuál rendija "atravesar" (en realidad pasa por ambas, esto es para la animación)
    const slitY = Math.random() < 0.5 ? g.cy - g.slitGap : g.cy + g.slitGap;
    particles.push({
      x: g.sourceX,
      y: g.cy,
      slitY: slitY,
      targetY: targetY,
      progress: 0,
      speed: 0.04 + Math.random() * 0.03
    });
  }

  function animate(now) {
    drawStatic();
    
    const g = getGeom();
    
    // Avanzar partículas
    particles = particles.filter(p => {
      p.progress += p.speed;
      
      let x, y;
      if (p.progress < 0.5) {
        // de fuente a rendija
        const t = p.progress / 0.5;
        x = g.sourceX + (g.barrierX - g.sourceX) * t;
        y = g.cy + (p.slitY - g.cy) * t;
      } else if (p.progress < 1) {
        // de rendija a pantalla
        const t = (p.progress - 0.5) / 0.5;
        x = g.barrierX + (g.screenX - g.barrierX) * t;
        y = p.slitY + (p.targetY - p.slitY) * t;
      } else {
        // llegó
        hits.push({ y: p.targetY, age: 0 });
        counter.textContent = hits.length;
        return false;
      }
      
      // Dibujar partícula en vuelo
      ctx.fillStyle = 'rgba(251, 191, 36, 0.4)';
      ctx.beginPath();
      ctx.arc(x, y, 5, 0, Math.PI * 2);
      ctx.fill();
      ctx.fillStyle = '#fbbf24';
      ctx.beginPath();
      ctx.arc(x, y, 2.5, 0, Math.PI * 2);
      ctx.fill();
      
      return true;
    });
    
    // Hits envejecer y dibujar
    hits.forEach(h => { if (h.age < 1) h.age = Math.min(1, h.age + 0.05); });
    drawHits();

    // Auto-fire
    if (autoFire && (now - lastFire > 30)) {
      fireParticle();
      lastFire = now;
    }

    requestAnimationFrame(animate);
  }

  fireOneBtn.addEventListener('click', () => fireParticle());
  fireManyBtn.addEventListener('click', () => {
    for (let i = 0; i < 100; i++) {
      setTimeout(fireParticle, i * 15);
    }
  });
  fireAutoBtn.addEventListener('click', () => {
    autoFire = !autoFire;
    fireAutoBtn.textContent = autoFire ? 'Detener auto' : 'Auto · disparo continuo';
    fireAutoBtn.classList.toggle('btn-warm', autoFire);
  });
  resetBtn.addEventListener('click', () => {
    hits = [];
    particles = [];
    counter.textContent = '0';
  });

  window.addEventListener('resize', () => { resize(); });
  requestAnimationFrame(animate);
})();

// ==================================================================
// MICROSCOPIO ELECTRÓNICO (sección 05)
// ==================================================================
(function() {
  const slider = document.getElementById('voltage-slider');
  const voltageVal = document.getElementById('voltage-val');
  const keVal = document.getElementById('ke-val');
  const evVal = document.getElementById('ev-val');
  const elVal = document.getElementById('el-val');
  const compareBars = document.getElementById('compare-bars');

  function getVoltage() {
    // 0..100 -> log 0 (1V) a 6 (1MV)
    return Math.pow(10, slider.value / 100 * 6);
  }

  function fmtNm(lambda_m) {
    const nm = lambda_m * 1e9;
    if (nm > 1) return nm.toFixed(2) + ' nm';
    if (nm > 0.01) return nm.toFixed(3) + ' nm';
    return (nm * 1000).toFixed(2) + ' pm';
  }

  function fmtVoltage(V) {
    if (V >= 1e6) return (V / 1e6).toFixed(2) + ' MV';
    if (V >= 1e3) return (V / 1e3).toFixed(2) + ' kV';
    return V.toFixed(1) + ' V';
  }

  function fmtEnergy(eV) {
    if (eV >= 1e6) return (eV / 1e6).toFixed(2) + ' MeV';
    if (eV >= 1e3) return (eV / 1e3).toFixed(2) + ' keV';
    return eV.toFixed(1) + ' eV';
  }

  function updateBars(lambdaE) {
    // Bars normalizadas en escala logarítmica
    const things = [
      { name: 'Luz roja visible', lambda: 700e-9, color: '#fb7185' },
      { name: 'Luz verde visible', lambda: 550e-9, color: '#5eead4' },
      { name: 'Luz UV', lambda: 200e-9, color: '#a78bfa' },
      { name: 'Tu electrón', lambda: lambdaE, color: '#fbbf24', highlight: true },
      { name: 'Rayos X duros', lambda: 1e-11, color: '#f472b6' },
      { name: 'Tamaño átomo', lambda: 1e-10, color: '#9a9da8' }
    ];
    things.sort((a, b) => b.lambda - a.lambda);
    
    const maxL = Math.max(...things.map(t => Math.log10(t.lambda)));
    const minL = Math.min(...things.map(t => Math.log10(t.lambda)));
    
    compareBars.innerHTML = '';
    things.forEach(t => {
      const logL = Math.log10(t.lambda);
      const pct = ((logL - minL) / (maxL - minL)) * 100;
      const row = document.createElement('div');
      row.className = 'compare-bar';
      row.innerHTML = 
        '<div class="label">' + t.name + '</div>' +
        '<div class="track"><div class="fill" style="width: ' + pct + '%; background: ' + t.color + '; ' + (t.highlight ? 'box-shadow: 0 0 8px ' + t.color : '') + '"></div></div>' +
        '<div class="val" style="' + (t.highlight ? 'color: ' + t.color : '') + '">' + fmtNm(t.lambda) + '</div>';
      compareBars.appendChild(row);
    });
  }

  function update() {
    const V = getVoltage();
    const E_J = V * e_const;
    const v = Math.sqrt(2 * E_J / m_e);
    const lambda = h_const / Math.sqrt(2 * m_e * E_J);

    voltageVal.textContent = fmtVoltage(V);
    keVal.textContent = fmtEnergy(V);
    evVal.textContent = fmtSci(v) + ' m/s';
    elVal.textContent = fmtNm(lambda);

    updateBars(lambda);
  }

  slider.addEventListener('input', update);
  update();
})();

// ==================================================================
// REALIDAD ALTERNA (sección 06)
// ==================================================================
(function() {
  const canvas = document.getElementById('reality-canvas');
  const ctx = canvas.getContext('2d');
  const slider = document.getElementById('h-mult-slider');
  const multVal = document.getElementById('h-mult-val');
  const narrator = document.getElementById('reality-narrator');

  let dims;
  function resize() { dims = dpiCanvas(canvas); }
  resize();

  const ballMass = 0.058; // pelota tenis
  const ballVel = 10; // m/s

  function getMultiplier() {
    // 0..100 -> log 0 a 33
    return Math.pow(10, slider.value / 100 * 33);
  }

  function getEffectiveLambda() {
    const mult = getMultiplier();
    const h_eff = h_const * mult;
    return h_eff / (ballMass * ballVel);
  }

  function narrativeFor(lambda, mult) {
    if (mult < 10) return 'En el universo real, la pelota de tenis tiene <strong>λ ≈ 1.1 × 10⁻³⁴ m</strong>, billones de veces más pequeña que un protón. Imposible de medir. Por eso parece comportarse como un punto sólido.';
    if (lambda < 1e-20) return 'Aún con <strong>h</strong> millones de veces más grande, λ sigue siendo subatómica. La pelota aún se comportaría clásicamente.';
    if (lambda < 1e-12) return 'λ ahora es del orden <strong>de un núcleo atómico</strong>. La pelota empezaría a mostrar incertidumbre cuántica en su posición, pero aún no la verías.';
    if (lambda < 1e-9) return 'λ ya es <strong>atómica</strong>. La pelota se difractaría al pasar por una rendija del tamaño de un cabello. Empezarías a verla "borrosa".';
    if (lambda < 1e-6) return 'λ <strong>visible bajo microscopio</strong>. La pelota se comportaría como un electrón en un microscopio electrónico.';
    if (lambda < 1e-3) return 'λ <strong>del tamaño de una bacteria</strong>. La pelota se difractaría al pasar por una puerta. Verías franjas de interferencia con el ojo desnudo.';
    if (lambda < 1) return 'λ del orden de <strong>centímetros a metros</strong>. La pelota se vuelve onda macroscópica. Pasaría por dos puertas a la vez.';
    return 'λ del tamaño de <strong>una habitación o más</strong>. La pelota literalmente sería una onda. No hay objetos sólidos en este universo, solo nubes de probabilidad.';
  }

  function draw() {
    const { W, H } = dims;
    ctx.clearRect(0, 0, W, H);

    const lambda = getEffectiveLambda();
    
    // Mapeo logarítmico a longitud en pantalla
    const logL = Math.log10(lambda);
    let screenLambda;
    if (logL < -30) screenLambda = 0.5;
    else if (logL > 2) screenLambda = 400;
    else {
      const t = (logL + 30) / 32;
      screenLambda = Math.pow(10, 0.1 + 2.5 * t);
    }

    // grid
    ctx.strokeStyle = 'rgba(255,255,255,0.03)';
    ctx.lineWidth = 1;
    for (let x = 0; x < W; x += 40) { ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,H); ctx.stroke(); }
    for (let y = 0; y < H; y += 40) { ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(W,y); ctx.stroke(); }

    // axis
    ctx.strokeStyle = 'rgba(255,255,255,0.1)';
    ctx.beginPath(); ctx.moveTo(0, H/2); ctx.lineTo(W, H/2); ctx.stroke();

    const amp = Math.min(60, H/2 - 30);
    const t = performance.now() / 1000;
    const phase = t * 50;

    // wave
    if (screenLambda >= 1.5) {
      ctx.strokeStyle = 'rgba(251, 191, 36, 0.2)';
      ctx.lineWidth = 9;
      ctx.beginPath();
      for (let x = 0; x <= W; x++) {
        const y = H/2 - amp * Math.sin(2 * Math.PI * (x - phase) / screenLambda);
        if (x === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
      }
      ctx.stroke();
      
      ctx.strokeStyle = '#fbbf24';
      ctx.lineWidth = 2;
      ctx.beginPath();
      for (let x = 0; x <= W; x++) {
        const y = H/2 - amp * Math.sin(2 * Math.PI * (x - phase) / screenLambda);
        if (x === 0) ctx.moveTo(x, y); else ctx.lineTo(x, y);
      }
      ctx.stroke();
    } else {
      ctx.strokeStyle = '#fbbf24';
      ctx.lineWidth = 2;
      ctx.beginPath(); ctx.moveTo(0, H/2); ctx.lineTo(W, H/2); ctx.stroke();
    }

    // pelota de tenis bouncing
    const ballX = (t * 80) % W;
    const ballY = screenLambda < 1.5 ? H/2 : H/2 - amp * Math.sin(2 * Math.PI * (ballX - phase) / screenLambda);
    
    // glow
    ctx.fillStyle = 'rgba(251, 191, 36, 0.25)';
    ctx.beginPath(); ctx.arc(ballX, ballY, 18, 0, Math.PI * 2); ctx.fill();
    // ball
    ctx.fillStyle = '#fef3c7';
    ctx.beginPath(); ctx.arc(ballX, ballY, 9, 0, Math.PI * 2); ctx.fill();
    // line on ball (tennis style)
    ctx.strokeStyle = '#fbbf24';
    ctx.lineWidth = 1.5;
    ctx.beginPath();
    ctx.arc(ballX, ballY, 9, -Math.PI/4, Math.PI/4);
    ctx.stroke();
    ctx.beginPath();
    ctx.arc(ballX, ballY, 9, 3*Math.PI/4, 5*Math.PI/4);
    ctx.stroke();

    // λ label
    ctx.fillStyle = 'rgba(255,255,255,0.5)';
    ctx.font = '11px "JetBrains Mono", monospace';
    ctx.textAlign = 'left';
    ctx.fillText('λ ≈ ' + fmtSciNice(lambda) + ' m', 12, 20);
  }

  function update() {
    const mult = getMultiplier();
    const lambda = getEffectiveLambda();
    
    if (mult < 10) {
      multVal.textContent = mult.toFixed(1) + ' × (la realidad)';
    } else if (mult < 1e6) {
      multVal.textContent = fmtSci(mult, 1) + ' ×';
    } else {
      multVal.textContent = fmtSciNice(mult) + ' ×';
    }
    
    narrator.innerHTML = narrativeFor(lambda, mult);
  }

  function animate() {
    draw();
    requestAnimationFrame(animate);
  }

  slider.addEventListener('input', update);
  window.addEventListener('resize', resize);
  
  update();
  animate();
})();

// ==================================================================
// QUIZ (sección 08)
// ==================================================================
(function() {
  const card = document.getElementById('quiz-card');
  
  const questions = [
    {
      q: '¿Quién propuso que toda partícula tiene una onda asociada?',
      options: ['Niels Bohr', 'Louis de Broglie', 'Werner Heisenberg', 'Erwin Schrödinger'],
      correct: 1,
      explain: 'Louis de Broglie lo propuso en su tesis doctoral de 1924. Su tribunal estaba tan dudoso que pidió la opinión de Einstein antes de aprobarla.'
    },
    {
      q: 'Si duplicas la velocidad de una partícula, ¿qué le pasa a su longitud de onda?',
      options: ['Se duplica', 'Se reduce a la mitad', 'Se cuadruplica', 'No cambia'],
      correct: 1,
      explain: 'λ = h/(mv), así que λ es inversamente proporcional a v. Más rápido = onda más corta. Es la misma intuición que con cualquier onda: más energía, más frecuencia, menor longitud.'
    },
    {
      q: 'A misma velocidad, ¿cuál tiene mayor longitud de onda: un electrón o un protón?',
      options: ['El electrón', 'El protón', 'Tienen la misma λ', 'Depende de la temperatura'],
      correct: 0,
      explain: 'El electrón. Como λ = h/(mv) y el protón tiene ~1836 veces más masa, su longitud de onda es ~1836 veces menor a la misma velocidad.'
    },
    {
      q: '¿Qué experimento confirmó por primera vez la hipótesis de De Broglie?',
      options: ['El experimento de Michelson-Morley', 'Davisson-Germer con electrones en un cristal de níquel', 'El efecto fotoeléctrico de Einstein', 'El experimento de la gota de aceite de Millikan'],
      correct: 1,
      explain: 'En 1927 Davisson y Germer dispararon electrones contra un cristal de níquel y observaron un patrón de difracción idéntico al de los rayos X. La λ medida coincidió exactamente con la fórmula de de Broglie.'
    },
    {
      q: '¿Por qué no observamos comportamiento ondulatorio en una pelota de tenis?',
      options: [
        'Porque las pelotas no son cuánticas',
        'Porque h es muy pequeña, así que λ es absurdamente diminuta para objetos masivos',
        'Porque la gravedad lo impide',
        'Porque la pelota tiene rozamiento con el aire'
      ],
      correct: 1,
      explain: 'h ≈ 6.6 × 10⁻³⁴ es tan pequeña que para una pelota con masa de gramos y velocidad de m/s, λ resulta ~10⁻³⁴ m. Billones de veces menor que un protón. Ningún instrumento concebible podría detectarla.'
    },
    {
      q: 'Si aceleras un electrón con 100 V, su longitud de onda es aproximadamente:',
      options: ['1.2 mm', '1.2 μm', '0.12 nm', '0.12 fm'],
      correct: 2,
      explain: 'Usando λ ≈ 1.226/√V nm (atajo útil), con V=100: λ ≈ 1.226/10 = 0.123 nm. Esto es ~4000 veces más pequeño que la luz visible, razón por la que el microscopio electrónico ve cosas que el óptico no.'
    }
  ];

  let current = 0;
  let score = 0;
  let answered = false;

  function render() {
    if (current >= questions.length) {
      renderResult();
      return;
    }
    
    const q = questions[current];
    const progress = ((current) / questions.length) * 100;
    
    card.innerHTML = 
      '<div class="quiz-progress">' +
        '<span class="quiz-step-tag">Pregunta ' + (current + 1) + ' de ' + questions.length + '</span>' +
        '<span class="quiz-score">Aciertos: <strong>' + score + '</strong></span>' +
      '</div>' +
      '<div class="quiz-progress-bar"><div class="quiz-progress-fill" style="width: ' + progress + '%"></div></div>' +
      '<div class="quiz-question">' + q.q + '</div>' +
      '<div class="quiz-options">' +
        q.options.map((o, i) => '<button class="quiz-option" data-i="' + i + '">' + o + '</button>').join('') +
      '</div>' +
      '<div class="quiz-feedback" id="quiz-feedback"></div>';
    
    answered = false;
    card.querySelectorAll('.quiz-option').forEach(btn => {
      btn.addEventListener('click', () => {
        if (answered) return;
        answered = true;
        const i = parseInt(btn.dataset.i);
        const correct = i === q.correct;
        if (correct) score++;
        
        card.querySelectorAll('.quiz-option').forEach((b, idx) => {
          b.disabled = true;
          if (idx === q.correct) b.classList.add('correct');
          else if (idx === i) b.classList.add('wrong');
        });
        
        const isLast = current === questions.length - 1;
        const fb = document.getElementById('quiz-feedback');
        fb.innerHTML = '<strong>' + (correct ? '✓ Correcto.' : '✗ La respuesta era: ' + q.options[q.correct] + '.') + '</strong> ' + q.explain +
          '<div style="margin-top: 1rem; text-align: right;">' +
            '<button class="btn btn-primary" id="quiz-next">' + (isLast ? 'Ver resultado' : 'Siguiente pregunta') + ' →</button>' +
          '</div>';
        fb.classList.add('visible');
        // Actualizar contador inmediatamente
        document.querySelector('.quiz-score strong').textContent = score;
        
        document.getElementById('quiz-next').addEventListener('click', () => {
          current++;
          render();
        });
      });
    });
  }

  function renderResult() {
    const pct = Math.round((score / questions.length) * 100);
    let verdict, sub;
    if (pct === 100) {
      verdict = 'Dominas la dualidad onda-partícula';
      sub = 'Honestamente, podrías dar la clase tú mismo.';
    } else if (pct >= 80) {
      verdict = 'Sólido entendimiento cuántico';
      sub = 'Te falta poco para ser un experto.';
    } else if (pct >= 60) {
      verdict = 'Vas por buen camino';
      sub = 'Repasa la teoría y vuelve a intentarlo.';
    } else if (pct >= 40) {
      verdict = 'Conceptos en construcción';
      sub = 'Juega un rato más con los simuladores antes de volver.';
    } else {
      verdict = 'Hay material por explorar';
      sub = 'La buena noticia: ahora sabes exactamente qué repasar.';
    }
    
    card.innerHTML = 
      '<div class="quiz-result">' +
        '<div class="score-big">' + score + '/' + questions.length + '</div>' +
        '<div class="score-label">' + pct + '% correcto</div>' +
        '<div class="verdict">' + verdict + '</div>' +
        '<div class="verdict-sub">' + sub + '</div>' +
        '<button class="btn btn-primary" id="quiz-restart">Volver a intentar</button>' +
      '</div>';
    
    document.getElementById('quiz-restart').addEventListener('click', () => {
      current = 0;
      score = 0;
      render();
    });
  }

  render();
})();

</script>

</body>
</html>
