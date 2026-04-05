<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JULIUS HAAN — Architectural Photography</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@300;400&display=swap" rel="stylesheet">
<style>
  :root {
    --concrete: #c8c4bc;
    --stone:    #f0ede8;
    --void:     #141210;
    --steel:    #6b6760;
    --blue:     #1a2840;
    --accent:   #4a7fa5;
    --line:     rgba(20,18,16,0.12);
  }

  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
  html { scroll-behavior:smooth; }

  body {
    background: var(--stone);
    color: var(--void);
    font-family: 'DM Mono', monospace;
    font-weight: 300;
    overflow-x: hidden;
    cursor: none;
  }

  /* ── CURSOR ── */
  #cur {
    position: fixed; width: 1px; height: 24px;
    background: var(--void);
    pointer-events: none; z-index: 9999;
    transform: translate(-50%,-50%);
    transition: height .2s, background .2s, opacity .2s;
  }
  #cur::after {
    content: '';
    position: absolute; top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    width: 24px; height: 1px;
    background: var(--void);
    transition: width .2s, background .2s;
  }
  #cur.hover { height: 32px; background: var(--accent); }
  #cur.hover::after { width: 32px; background: var(--accent); }

  /* ── GRID OVERLAY (decorative) ── */
  .grid-overlay {
    position: fixed; inset: 0; pointer-events: none; z-index: 0;
    background-image:
      linear-gradient(var(--line) 1px, transparent 1px),
      linear-gradient(90deg, var(--line) 1px, transparent 1px);
    background-size: 80px 80px;
    opacity: 0.5;
  }

  /* ── NAV ── */
  nav {
    position: fixed; top: 0; left: 0; right: 0;
    z-index: 500;
    display: grid;
    grid-template-columns: 60px 1fr auto 60px;
    border-bottom: 1px solid var(--line);
    background: rgba(240,237,232,0.92);
    backdrop-filter: blur(12px);
  }
  .nav-rule { border-right: 1px solid var(--line); }
  .nav-logo-cell {
    grid-column: 2;
    padding: 1.1rem 1.5rem;
    display: flex; align-items: center;
    border-right: 1px solid var(--line);
  }
  .nav-logo {
    font-family: 'DM Serif Display', serif;
    font-size: 0.95rem;
    letter-spacing: 0.02em;
    text-decoration: none;
    color: var(--void);
  }
  .nav-logo em { font-style: italic; color: var(--accent); }
  .nav-links-cell {
    display: flex; align-items: stretch;
    border-right: 1px solid var(--line);
  }
  .nav-links { display: flex; list-style: none; }
  .nav-links li { border-right: 1px solid var(--line); }
  .nav-links a {
    display: flex; align-items: center;
    padding: 0 1.4rem;
    font-size: 0.58rem; letter-spacing: 0.2em; text-transform: uppercase;
    text-decoration: none; color: var(--steel);
    transition: color .2s, background .2s;
    height: 100%;
  }
  .nav-links a:hover { color: var(--void); background: rgba(20,18,16,0.04); }

  /* ── HERO ── */
  .hero {
    min-height: 100vh;
    display: grid;
    grid-template-columns: 60px 1fr 60px;
    grid-template-rows: 60px 1fr auto 80px;
    border-bottom: 1px solid var(--line);
    position: relative;
  }
  /* left rule */
  .hero-rule-l {
    grid-column: 1; grid-row: 1/-1;
    border-right: 1px solid var(--line);
  }
  /* right rule */
  .hero-rule-r {
    grid-column: 3; grid-row: 1/-1;
    border-left: 1px solid var(--line);
  }
  /* top spacer */
  .hero-top { grid-column: 2; grid-row: 1; border-bottom: 1px solid var(--line); }

  .hero-main {
    grid-column: 2; grid-row: 2;
    display: grid;
    grid-template-columns: 1fr 1px 1fr;
    min-height: 60vh;
  }
  .hero-text {
    padding: 4rem 3rem;
    display: flex; flex-direction: column; justify-content: space-between;
  }
  .hero-label {
    font-size: 0.55rem; letter-spacing: 0.3em; text-transform: uppercase;
    color: var(--steel); margin-bottom: 4rem;
  }
  .hero-label span { color: var(--accent); }
  .hero-title {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(3.5rem, 6.5vw, 7rem);
    line-height: 0.95;
    letter-spacing: -0.02em;
  }
  .hero-title em { font-style: italic; color: var(--accent); display: block; }
  .hero-caption {
    font-size: 0.68rem; line-height: 1.9;
    color: var(--steel); max-width: 320px;
    margin-top: 3rem;
  }
  .hero-divider { background: var(--line); }
  .hero-image-pane {
    background: var(--void);
    position: relative;
    overflow: hidden;
    min-height: 420px;
  }
  /* Architectural scene — abstract concrete planes */
  .hero-scene {
    position: absolute; inset: 0;
    background:
      linear-gradient(105deg, #1a1814 0%, #242220 35%, #0e0c0a 100%);
  }
  .hero-scene::before {
    content: '';
    position: absolute;
    top: 0; left: 38%;
    width: 1px; height: 100%;
    background: rgba(255,255,255,0.06);
  }
  .hero-scene::after {
    content: '';
    position: absolute;
    top: 0; left: 62%;
    width: 1px; height: 100%;
    background: rgba(255,255,255,0.04);
  }
  /* Horizontal beam of light */
  .hero-light {
    position: absolute;
    top: 38%; left: 0; right: 0;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.12) 30%, rgba(255,255,255,0.18) 50%, rgba(255,255,255,0.12) 70%, transparent);
  }
  .hero-light-2 {
    position: absolute;
    top: 62%; left: 0; right: 0;
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(74,127,165,0.15) 40%, rgba(74,127,165,0.2) 55%, transparent);
  }
  /* Column shadow */
  .hero-col {
    position: absolute;
    top: 0; left: 55%;
    width: 18%; height: 100%;
    background: linear-gradient(180deg, rgba(20,18,16,0.6) 0%, rgba(10,10,8,0.8) 100%);
  }
  .hero-watermark {
    position: absolute; bottom: 2rem; right: 2rem;
    font-size: 0.5rem; letter-spacing: 0.25em; text-transform: uppercase;
    color: rgba(255,255,255,0.15);
    writing-mode: vertical-rl;
  }
  /* Coordinate marks */
  .coord {
    position: absolute;
    font-size: 0.45rem; letter-spacing: 0.1em;
    color: rgba(74,127,165,0.5);
    font-family: 'DM Mono', monospace;
  }
  .coord-tl { top: 1rem; left: 1rem; }
  .coord-br { bottom: 1rem; right: 4rem; }

  .hero-bottom-bar {
    grid-column: 2; grid-row: 3;
    display: grid; grid-template-columns: repeat(4, 1fr);
    border-top: 1px solid var(--line);
  }
  .hbb-cell {
    padding: 1.5rem;
    border-right: 1px solid var(--line);
    display: flex; flex-direction: column; gap: 0.4rem;
  }
  .hbb-cell:last-child { border-right: none; }
  .hbb-label { font-size: 0.48rem; letter-spacing: 0.2em; text-transform: uppercase; color: var(--steel); }
  .hbb-val { font-family: 'DM Serif Display', serif; font-size: 1.4rem; font-style: italic; }

  /* ── SECTION WRAPPER ── */
  .section-wrap {
    display: grid;
    grid-template-columns: 60px 1fr 60px;
    border-bottom: 1px solid var(--line);
  }
  .sw-rule-l { border-right: 1px solid var(--line); }
  .sw-rule-r { border-left: 1px solid var(--line); }
  .sw-inner { position: relative; }

  /* ── SECTION HEADER ── */
  .s-header {
    display: flex; align-items: baseline;
    justify-content: space-between;
    padding: 2rem 3rem;
    border-bottom: 1px solid var(--line);
  }
  .s-heading {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.8rem, 3vw, 2.8rem);
    line-height: 1;
  }
  .s-heading em { font-style: italic; color: var(--accent); }
  .s-meta { font-size: 0.52rem; letter-spacing: 0.2em; text-transform: uppercase; color: var(--steel); }

  /* ── PROJECTS GRID ── */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
  }
  .project-cell {
    border-right: 1px solid var(--line);
    border-bottom: 1px solid var(--line);
    cursor: none;
    position: relative; overflow: hidden;
  }
  .project-cell:nth-child(3n) { border-right: none; }
  .project-cell:nth-last-child(-n+3) { border-bottom: none; }

  .project-photo {
    aspect-ratio: 4/3;
    overflow: hidden;
    position: relative;
  }
  .project-bg {
    width: 100%; height: 100%;
    transition: transform .7s cubic-bezier(.25,.46,.45,.94);
  }
  .project-cell:hover .project-bg { transform: scale(1.05); }

  /* Architectural photo palettes */
  .a1 { background: linear-gradient(160deg, #1c2030 0%, #2c3448 40%, #181c28 100%); }
  .a2 { background: linear-gradient(120deg, #2a2420 0%, #3c3028 40%, #1c1814 100%); }
  .a3 { background: linear-gradient(200deg, #101418 0%, #1c2430 40%, #0c1018 100%); }
  .a4 { background: linear-gradient(150deg, #282420 0%, #181410 60%, #343028 100%); }
  .a5 { background: linear-gradient(170deg, #1a1c24 0%, #2a2c38 50%, #141620 100%); }
  .a6 { background: linear-gradient(130deg, #201c18 0%, #382e24 50%, #141210 100%); }

  /* Fake architectural details */
  .a1::after { content:''; position:absolute; bottom:30%; left:20%; width:60%; height:1px; background:rgba(255,255,255,0.08); }
  .a2::after { content:''; position:absolute; left:40%; top:0; bottom:0; width:1px; background:rgba(255,255,255,0.06); }
  .a3::after { content:''; position:absolute; top:40%; left:0; right:0; height:1px; background:rgba(74,127,165,0.12); }

  .project-info {
    padding: 1.25rem 1.5rem;
    border-top: 1px solid var(--line);
    display: flex; justify-content: space-between; align-items: baseline;
  }
  .project-name {
    font-family: 'DM Serif Display', serif;
    font-size: 1rem; font-style: italic;
  }
  .project-tag {
    font-size: 0.48rem; letter-spacing: 0.18em; text-transform: uppercase;
    color: var(--accent);
  }
  .project-num {
    position: absolute; top: 0.75rem; left: 0.75rem;
    font-size: 0.45rem; letter-spacing: 0.15em;
    color: rgba(255,255,255,0.3);
    z-index: 2;
  }

  /* ── MANIFESTO ── */
  .manifesto {
    padding: 6rem 3rem;
    display: grid;
    grid-template-columns: 1fr 2px 1fr;
    gap: 0;
    border-bottom: 1px solid var(--line);
    background: var(--void);
    color: var(--stone);
  }
  .manifesto-rule { background: rgba(255,255,255,0.06); }
  .manifesto-left { padding-right: 5rem; }
  .manifesto-right { padding-left: 5rem; display: flex; flex-direction: column; justify-content: flex-end; }
  .manifesto-label {
    font-size: 0.52rem; letter-spacing: 0.3em; text-transform: uppercase;
    color: var(--accent); margin-bottom: 2rem;
  }
  .manifesto-quote {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.6rem, 2.5vw, 2.4rem);
    line-height: 1.3; font-style: italic;
    margin-bottom: 2rem;
  }
  .manifesto-body {
    font-size: 0.7rem; line-height: 2;
    color: rgba(240,237,232,0.45);
  }
  .manifesto-stat-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 0;
  }
  .m-stat {
    padding: 2rem;
    border: 1px solid rgba(255,255,255,0.06);
    border-bottom: none; border-right: none;
  }
  .m-stat:nth-child(2n) { border-right: 1px solid rgba(255,255,255,0.06); }
  .m-stat:nth-child(3), .m-stat:nth-child(4) { border-bottom: 1px solid rgba(255,255,255,0.06); }
  .m-stat-n {
    font-family: 'DM Serif Display', serif;
    font-size: 2.8rem; line-height: 1; font-style: italic;
    color: var(--stone);
  }
  .m-stat-n sup { font-size: 1rem; color: var(--accent); font-style: normal; }
  .m-stat-l {
    font-size: 0.5rem; letter-spacing: 0.18em; text-transform: uppercase;
    color: rgba(240,237,232,0.35); margin-top: 0.5rem;
  }

  /* ── SELECTED SERIES ── */
  .series-strip {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    border-bottom: 1px solid var(--line);
  }
  .series-item {
    border-right: 1px solid var(--line);
    cursor: none; overflow: hidden;
    position: relative;
  }
  .series-item:last-child { border-right: none; }
  .series-photo {
    aspect-ratio: 9/16;
    overflow: hidden;
  }
  .series-photo-bg {
    width: 100%; height: 100%;
    transition: transform .7s cubic-bezier(.25,.46,.45,.94);
  }
  .series-item:hover .series-photo-bg { transform: scale(1.06); }
  .s1 { background: linear-gradient(180deg, #0c1020 0%, #1a2438 50%, #0a0e18 100%); }
  .s2 { background: linear-gradient(180deg, #1a1410 0%, #302418 50%, #0e0c08 100%); }
  .s3 { background: linear-gradient(180deg, #141820 0%, #202838 50%, #0c1018 100%); }
  .s4 { background: linear-gradient(180deg, #101210 0%, #202818 50%, #0c1008 100%); }

  .series-label {
    padding: 1.25rem;
    border-top: 1px solid var(--line);
  }
  .series-n { font-size: 0.45rem; letter-spacing: 0.2em; color: var(--accent); margin-bottom: 0.4rem; }
  .series-title { font-family: 'DM Serif Display', serif; font-size: 0.95rem; font-style: italic; }
  .series-loc { font-size: 0.5rem; letter-spacing: 0.12em; color: var(--steel); margin-top: 0.25rem; }

  /* ── ABOUT ── */
  .about-layout {
    display: grid;
    grid-template-columns: 5fr 4px 3fr;
    border-bottom: 1px solid var(--line);
  }
  .about-rule { background: var(--line); }
  .about-left {
    display: grid;
    grid-template-columns: 1fr 1fr;
    border-right: 1px solid var(--line);
  }
  .about-portrait {
    background: var(--void);
    position: relative; overflow: hidden;
    min-height: 480px;
    border-right: 1px solid var(--line);
  }
  .portrait-bg {
    position: absolute; inset: 0;
    background: linear-gradient(170deg, #1a1814, #0c0a08);
  }
  .portrait-bg::before {
    content: '';
    position: absolute; top: 0; left: 50%;
    width: 1px; height: 100%;
    background: rgba(255,255,255,0.04);
  }
  /* Blueprint-style corner marks */
  .bp-corner {
    position: absolute; width: 18px; height: 18px;
    z-index: 2;
  }
  .bp-corner.tl { top: 1rem; left: 1rem; border-top: 1px solid rgba(74,127,165,0.5); border-left: 1px solid rgba(74,127,165,0.5); }
  .bp-corner.tr { top: 1rem; right: 1rem; border-top: 1px solid rgba(74,127,165,0.5); border-right: 1px solid rgba(74,127,165,0.5); }
  .bp-corner.bl { bottom: 1rem; left: 1rem; border-bottom: 1px solid rgba(74,127,165,0.5); border-left: 1px solid rgba(74,127,165,0.5); }
  .bp-corner.br { bottom: 1rem; right: 1rem; border-bottom: 1px solid rgba(74,127,165,0.5); border-right: 1px solid rgba(74,127,165,0.5); }
  .portrait-label {
    position: absolute; bottom: 1.25rem; left: 1.25rem;
    font-size: 0.45rem; letter-spacing: 0.2em; text-transform: uppercase;
    color: rgba(74,127,165,0.5);
  }

  .about-text-block {
    padding: 3rem;
    display: flex; flex-direction: column; justify-content: space-between;
  }
  .about-heading {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(1.8rem, 2.5vw, 2.5rem);
    line-height: 1.1; margin-bottom: 1.5rem;
  }
  .about-heading em { font-style: italic; color: var(--accent); }
  .about-body {
    font-size: 0.68rem; line-height: 2; color: var(--steel);
    margin-bottom: 1.2rem;
  }
  .about-clients {
    margin-top: 2rem;
    padding-top: 2rem;
    border-top: 1px solid var(--line);
  }
  .clients-label {
    font-size: 0.48rem; letter-spacing: 0.2em; text-transform: uppercase;
    color: var(--steel); margin-bottom: 1rem;
  }
  .clients-list {
    display: flex; flex-wrap: wrap; gap: 0.6rem;
  }
  .client-tag {
    font-size: 0.5rem; letter-spacing: 0.12em; text-transform: uppercase;
    border: 1px solid var(--line);
    padding: 0.3rem 0.6rem;
    color: var(--steel);
  }

  .about-right { padding: 3rem; display: flex; flex-direction: column; justify-content: center; gap: 2rem; }
  .about-spec-row {
    padding-bottom: 1.5rem;
    border-bottom: 1px solid var(--line);
  }
  .about-spec-row:last-child { border-bottom: none; padding-bottom: 0; }
  .spec-label-sm { font-size: 0.48rem; letter-spacing: 0.22em; text-transform: uppercase; color: var(--accent); margin-bottom: 0.4rem; }
  .spec-val-sm { font-family: 'DM Serif Display', serif; font-size: 1.1rem; font-style: italic; }

  /* ── CONTACT ── */
  .contact-band {
    display: grid;
    grid-template-columns: 1fr 1px 1fr;
    border-bottom: 1px solid var(--line);
    background: var(--blue);
    color: var(--stone);
  }
  .cb-rule { background: rgba(255,255,255,0.08); }
  .contact-left {
    padding: 5rem 4rem;
    display: flex; flex-direction: column; justify-content: space-between;
  }
  .contact-heading {
    font-family: 'DM Serif Display', serif;
    font-size: clamp(2.8rem, 5vw, 5.5rem);
    line-height: 0.95;
  }
  .contact-heading em { font-style: italic; color: var(--accent); }
  .contact-email {
    font-family: 'DM Serif Display', serif;
    font-size: 1.1rem; font-style: italic;
    color: rgba(240,237,232,0.55);
    text-decoration: none;
    transition: color .3s;
  }
  .contact-email:hover { color: var(--stone); }
  .contact-right { padding: 5rem 4rem; }
  .c-row {
    padding: 1.4rem 0;
    border-bottom: 1px solid rgba(255,255,255,0.08);
    display: flex; flex-direction: column; gap: 0.5rem;
  }
  .c-row:first-of-type { border-top: 1px solid rgba(255,255,255,0.08); }
  .c-lab { font-size: 0.48rem; letter-spacing: 0.22em; text-transform: uppercase; color: rgba(74,127,165,0.8); }
  .c-inp, .c-ta {
    background: none; border: none; outline: none;
    font-family: 'DM Mono', monospace; font-weight: 300;
    font-size: 0.82rem; color: var(--stone);
    resize: none;
  }
  .c-inp::placeholder, .c-ta::placeholder { color: rgba(240,237,232,0.2); }
  .c-ta { height: 80px; }
  .c-submit {
    margin-top: 2.5rem;
    display: flex; align-items: center; gap: 1.5rem;
    background: none; border: 1px solid rgba(255,255,255,0.15);
    color: var(--stone); font-family: 'DM Mono', monospace;
    font-weight: 300; font-size: 0.58rem; letter-spacing: 0.22em; text-transform: uppercase;
    padding: 1rem 1.75rem; cursor: none;
    transition: border-color .3s, background .3s;
  }
  .c-submit:hover { border-color: var(--accent); background: rgba(74,127,165,0.12); }
  .c-arrow { transition: transform .3s; }
  .c-submit:hover .c-arrow { transform: translateX(5px); }

  /* ── FOOTER ── */
  footer {
    display: grid;
    grid-template-columns: 60px 1fr 1fr 60px;
    border-top: 1px solid var(--line);
  }
  .ft-rule-l { border-right: 1px solid var(--line); }
  .ft-rule-r { border-left: 1px solid var(--line); }
  .ft-left {
    padding: 2rem 1.5rem;
    border-right: 1px solid var(--line);
    display: flex; align-items: center;
  }
  .ft-logo {
    font-family: 'DM Serif Display', serif;
    font-size: 0.85rem; font-style: italic; color: var(--steel);
  }
  .ft-right {
    padding: 2rem 1.5rem;
    display: flex; justify-content: flex-end;
    align-items: center; gap: 3rem;
  }
  .ft-link {
    font-size: 0.5rem; letter-spacing: 0.18em; text-transform: uppercase;
    color: var(--concrete); text-decoration: none;
    transition: color .2s;
  }
  .ft-link:hover { color: var(--accent); }
  .ft-copy {
    font-size: 0.5rem; letter-spacing: 0.12em; color: var(--concrete);
  }

  /* ── REVEAL ── */
  .rv {
    opacity: 0; transform: translateY(20px);
    transition: opacity .8s ease, transform .8s ease;
  }
  .rv.on { opacity: 1; transform: none; }
  .rv-l {
    opacity: 0; transform: translateX(-16px);
    transition: opacity .8s ease, transform .8s ease;
  }
  .rv-l.on { opacity: 1; transform: none; }

  @media (max-width: 900px) {
    .hero { grid-template-columns: 1fr; }
    .hero-rule-l, .hero-rule-r { display: none; }
    .hero-main { grid-template-columns: 1fr; }
    .hero-divider { display: none; }
    .hero-image-pane { min-height: 260px; }
    .hero-bottom-bar { grid-template-columns: 1fr 1fr; }
    .projects-grid { grid-template-columns: 1fr 1fr; }
    .project-cell:nth-child(2n) { border-right: none; }
    .manifesto { grid-template-columns: 1fr; gap: 3rem; padding: 4rem 1.5rem; }
    .manifesto-left { padding-right: 0; }
    .manifesto-right { padding-left: 0; }
    .series-strip { grid-template-columns: 1fr 1fr; }
    .series-item:nth-child(2n) { border-right: none; }
    .about-layout { grid-template-columns: 1fr; }
    .about-left { grid-template-columns: 1fr; }
    .about-portrait { min-height: 300px; }
    .contact-band { grid-template-columns: 1fr; }
    .contact-left { padding: 3rem 2rem; }
    .contact-right { padding: 3rem 2rem; }
    .section-wrap { grid-template-columns: 1fr; }
    .sw-rule-l, .sw-rule-r { display: none; }
    nav { grid-template-columns: 1fr auto; }
    .nav-rule { display: none; }
    .nav-logo-cell { grid-column: 1; }
    .nav-links-cell { grid-column: 2; }
    footer { grid-template-columns: 1fr; }
    .ft-rule-l, .ft-rule-r { display: none; }
    .ft-right { justify-content: flex-start; flex-wrap: wrap; }
  }
</style>
</head>
<body>

<div class="grid-overlay"></div>
<div id="cur"></div>

<!-- NAV -->
<nav>
  <div class="nav-rule"></div>
  <div class="nav-logo-cell">
    <a href="#" class="nav-logo">Julius <em>Haan</em></a>
  </div>
  <div class="nav-links-cell">
    <ul class="nav-links">
      <li><a href="#projects">Projects</a></li>
      <li><a href="#about">Studio</a></li>
      <li><a href="#contact">Commission</a></li>
    </ul>
  </div>
  <div class="nav-rule"></div>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-rule-l"></div>
  <div class="hero-top"></div>
  <div class="hero-main">
    <div class="hero-text">
      <p class="hero-label">Architectural Photography &nbsp;·&nbsp; <span>2009–2026</span></p>
      <div>
        <h1 class="hero-title">
          Structure<br>
          <em>is</em><br>
          silence.
        </h1>
        <p class="hero-caption">Julius Haan documents the built environment — its geometries, textures, and the play of light across surfaces that architects dream and concrete remembers.</p>
      </div>
    </div>
    <div class="hero-divider"></div>
    <div class="hero-image-pane">
      <div class="hero-scene">
        <div class="hero-light"></div>
        <div class="hero-light-2"></div>
        <div class="hero-col"></div>
      </div>
      <div class="coord coord-tl">52°31'N 13°24'E</div>
      <div class="coord coord-br">ISO 100 · f/11 · 1/125s</div>
      <div class="hero-watermark">Julius Haan 2026</div>
    </div>
  </div>
  <div class="hero-bottom-bar">
    <div class="hbb-cell">
      <span class="hbb-label">Speciality</span>
      <span class="hbb-val">Architectural</span>
    </div>
    <div class="hbb-cell">
      <span class="hbb-label">Format</span>
      <span class="hbb-val">Large Format</span>
    </div>
    <div class="hbb-cell">
      <span class="hbb-label">Based</span>
      <span class="hbb-val">Amsterdam</span>
    </div>
    <div class="hbb-cell">
      <span class="hbb-label">Available</span>
      <span class="hbb-val">Worldwide</span>
    </div>
  </div>
  <div class="hero-rule-r"></div>
</section>

<!-- PROJECTS -->
<div class="section-wrap" id="projects">
  <div class="sw-rule-l"></div>
  <div class="sw-inner">
    <div class="s-header rv">
      <h2 class="s-heading">Selected <em>Projects</em></h2>
      <span class="s-meta">06 of 48 — 2022/26</span>
    </div>
    <div class="projects-grid">
      <div class="project-cell rv">
        <div class="project-photo"><div class="project-bg a1"></div></div>
        <div class="project-num">001</div>
        <div class="project-info">
          <span class="project-name">Fondation Louis Vuitton</span>
          <span class="project-tag">Cultural · Paris</span>
        </div>
      </div>
      <div class="project-cell rv" style="transition-delay:.08s">
        <div class="project-photo"><div class="project-bg a2"></div></div>
        <div class="project-num">002</div>
        <div class="project-info">
          <span class="project-name">Morpheus Hotel</span>
          <span class="project-tag">Hospitality · Macau</span>
        </div>
      </div>
      <div class="project-cell rv" style="transition-delay:.16s">
        <div class="project-photo"><div class="project-bg a3"></div></div>
        <div class="project-num">003</div>
        <div class="project-info">
          <span class="project-name">Elbphilharmonie</span>
          <span class="project-tag">Concert Hall · Hamburg</span>
        </div>
      </div>
      <div class="project-cell rv" style="transition-delay:.08s">
        <div class="project-photo"><div class="project-bg a4"></div></div>
        <div class="project-num">004</div>
        <div class="project-info">
          <span class="project-name">Casa Milà Interior</span>
          <span class="project-tag">Heritage · Barcelona</span>
        </div>
      </div>
      <div class="project-cell rv" style="transition-delay:.16s">
        <div class="project-photo"><div class="project-bg a5"></div></div>
        <div class="project-num">005</div>
        <div class="project-info">
          <span class="project-name">National Museum Qatar</span>
          <span class="project-tag">Museum · Doha</span>
        </div>
      </div>
      <div class="project-cell rv" style="transition-delay:.24s">
        <div class="project-photo"><div class="project-bg a6"></div></div>
        <div class="project-num">006</div>
        <div class="project-info">
          <span class="project-name">Breuer Building</span>
          <span class="project-tag">Brutalist · New York</span>
        </div>
      </div>
    </div>
  </div>
  <div class="sw-rule-r"></div>
</div>

<!-- MANIFESTO -->
<div class="section-wrap">
  <div class="sw-rule-l" style="border-right-color: rgba(255,255,255,0.05)"></div>
  <div class="sw-inner">
    <div class="manifesto">
      <div class="manifesto-left rv">
        <p class="manifesto-label">Philosophy</p>
        <p class="manifesto-quote">"A building exists twice — once when it is built, and once when it is seen."</p>
        <p class="manifesto-body">Every commission begins with hours of stillness inside the space. Light is not chased — it is waited for. The camera reveals what the architect intended and what time, weather, and shadow have added. Haan works exclusively with large-format film, creating archival-quality images that outlast the buildings they document.</p>
      </div>
      <div class="manifesto-rule"></div>
      <div class="manifesto-right rv">
        <div class="manifesto-stat-grid">
          <div class="m-stat">
            <div class="m-stat-n">48<sup>+</sup></div>
            <div class="m-stat-l">Commissions</div>
          </div>
          <div class="m-stat">
            <div class="m-stat-n">22</div>
            <div class="m-stat-l">Countries</div>
          </div>
          <div class="m-stat">
            <div class="m-stat-n">17<sup>yr</sup></div>
            <div class="m-stat-l">Experience</div>
          </div>
          <div class="m-stat">
            <div class="m-stat-n">6<sup>k</sup></div>
            <div class="m-stat-l">Film Sheets</div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="sw-rule-r" style="border-left-color: rgba(255,255,255,0.05)"></div>
</div>

<!-- SERIES -->
<div class="section-wrap">
  <div class="sw-rule-l"></div>
  <div class="sw-inner">
    <div class="s-header rv">
      <h2 class="s-heading">Ongoing <em>Series</em></h2>
      <span class="s-meta">Long-form studies</span>
    </div>
    <div class="series-strip">
      <div class="series-item rv">
        <div class="series-photo"><div class="series-photo-bg s1"></div></div>
        <div class="series-label">
          <div class="series-n">I —</div>
          <div class="series-title">Vertical Cities</div>
          <div class="series-loc">Hong Kong · Dubai · NYC</div>
        </div>
      </div>
      <div class="series-item rv" style="transition-delay:.1s">
        <div class="series-photo"><div class="series-photo-bg s2"></div></div>
        <div class="series-label">
          <div class="series-n">II —</div>
          <div class="series-title">Brutalist Remains</div>
          <div class="series-loc">Eastern Europe</div>
        </div>
      </div>
      <div class="series-item rv" style="transition-delay:.2s">
        <div class="series-photo"><div class="series-photo-bg s3"></div></div>
        <div class="series-label">
          <div class="series-n">III —</div>
          <div class="series-title">Threshold</div>
          <div class="series-loc">Doorways, 14 countries</div>
        </div>
      </div>
      <div class="series-item rv" style="transition-delay:.3s">
        <div class="series-photo"><div class="series-photo-bg s4"></div></div>
        <div class="series-label">
          <div class="series-n">IV —</div>
          <div class="series-title">Sacred Geometry</div>
          <div class="series-loc">Religious Architecture</div>
        </div>
      </div>
    </div>
  </div>
  <div class="sw-rule-r"></div>
</div>

<!-- ABOUT -->
<div class="section-wrap" id="about">
  <div class="sw-rule-l"></div>
  <div class="sw-inner">
    <div class="s-header rv">
      <h2 class="s-heading">The <em>Studio</em></h2>
    </div>
    <div class="about-layout">
      <div class="about-left">
        <div class="about-portrait rv-l">
          <div class="portrait-bg"></div>
          <div class="bp-corner tl"></div>
          <div class="bp-corner tr"></div>
          <div class="bp-corner bl"></div>
          <div class="bp-corner br"></div>
          <div class="portrait-label">Linhof Technika · Kodak Ektar 100</div>
        </div>
        <div class="about-text-block rv">
          <div>
            <h2 class="about-heading">Precision.<br><em>Patience.</em><br>Film.</h2>
            <p class="about-body">Julius Haan studied at the Gerrit Rietveld Academie in Amsterdam and the AA School of Architecture in London. He has shot for firms including Herzog & de Meuron, Zaha Hadid Architects, and Kengo Kuma & Associates.</p>
            <p class="about-body">His prints are held in museum collections in Amsterdam, Berlin, and Shenzen. Commissions are accepted on a selective basis with lead times of 6–12 weeks.</p>
          </div>
          <div class="about-clients">
            <p class="clients-label">Clients & Publications</p>
            <div class="clients-list">
              <span class="client-tag">Wallpaper*</span>
              <span class="client-tag">Dezeen</span>
              <span class="client-tag">Domus</span>
              <span class="client-tag">Architectural Digest</span>
              <span class="client-tag">Frame</span>
              <span class="client-tag">El Croquis</span>
            </div>
          </div>
        </div>
      </div>
      <div class="about-rule"></div>
      <div class="about-right rv">
        <div class="about-spec-row">
          <div class="spec-label-sm">Camera Systems</div>
          <div class="spec-val-sm">Linhof Technika 4×5, Deardorff 8×10</div>
        </div>
        <div class="about-spec-row">
          <div class="spec-label-sm">Film Stocks</div>
          <div class="spec-val-sm">Kodak Ektar 100, Ilford FP4+, Fuji Provia</div>
        </div>
        <div class="about-spec-row">
          <div class="spec-label-sm">Print Processes</div>
          <div class="spec-val-sm">C-type, Pigment, Silver Gelatin</div>
        </div>
        <div class="about-spec-row">
          <div class="spec-label-sm">Studio Location</div>
          <div class="spec-val-sm">Amsterdam-Noord, Netherlands</div>
        </div>
        <div class="about-spec-row">
          <div class="spec-label-sm">Commission Lead Time</div>
          <div class="spec-val-sm">6 – 12 weeks</div>
        </div>
        <div class="about-spec-row">
          <div class="spec-label-sm">Availability</div>
          <div class="spec-val-sm">Accepting Q3 2026 onwards</div>
        </div>
      </div>
    </div>
  </div>
  <div class="sw-rule-r"></div>
</div>

<!-- CONTACT -->
<div class="section-wrap" id="contact">
  <div class="sw-rule-l" style="border-right-color: rgba(255,255,255,0.05)"></div>
  <div class="sw-inner">
    <div class="contact-band">
      <div class="contact-left rv">
        <div>
          <p style="font-size:.52rem;letter-spacing:.28em;text-transform:uppercase;color:var(--accent);margin-bottom:2rem;">Commission a Project</p>
          <h2 class="contact-heading">
            Let's<br>
            document<br>
            <em>your work.</em>
          </h2>
        </div>
        <a href="mailto:studio@juliushaan.com" class="contact-email">studio@juliushaan.com</a>
      </div>
      <div class="cb-rule"></div>
      <div class="contact-right rv">
        <div class="c-row">
          <label class="c-lab">Full Name</label>
          <input class="c-inp" type="text" placeholder="Your name">
        </div>
        <div class="c-row">
          <label class="c-lab">Email</label>
          <input class="c-inp" type="email" placeholder="studio@firm.com">
        </div>
        <div class="c-row">
          <label class="c-lab">Project / Building</label>
          <input class="c-inp" type="text" placeholder="Name and location of the building">
        </div>
        <div class="c-row">
          <label class="c-lab">Brief</label>
          <textarea class="c-ta" placeholder="Scope, timeline, intended use..."></textarea>
        </div>
        <button class="c-submit">Send Enquiry <span class="c-arrow">→</span></button>
      </div>
    </div>
  </div>
  <div class="sw-rule-r" style="border-left-color: rgba(255,255,255,0.05)"></div>
</div>

<!-- FOOTER -->
<footer>
  <div class="ft-rule-l"></div>
  <div class="ft-left">
    <span class="ft-logo">Julius Haan</span>
  </div>
  <div class="ft-right">
    <span class="ft-copy">© 2026 Julius Haan Studio</span>
    <a href="#" class="ft-link">Instagram</a>
    <a href="#" class="ft-link">Behance</a>
    <a href="#" class="ft-link">Press Kit</a>
  </div>
  <div class="ft-rule-r"></div>
</footer>

<script>
  // Crosshair cursor
  const cur = document.getElementById('cur');
  document.addEventListener('mousemove', e => {
    cur.style.left = e.clientX + 'px';
    cur.style.top = e.clientY + 'px';
  });
  document.querySelectorAll('a, button, .project-cell, .series-item').forEach(el => {
    el.addEventListener('mouseenter', () => cur.classList.add('hover'));
    el.addEventListener('mouseleave', () => cur.classList.remove('hover'));
  });

  // Scroll reveal
  const obs = new IntersectionObserver(entries => {
    entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('on'); });
  }, { threshold: 0.1 });
  document.querySelectorAll('.rv, .rv-l').forEach(el => obs.observe(el));

  // Nav highlight
  const secs = document.querySelectorAll('[id]');
  const links = document.querySelectorAll('.nav-links a');
  window.addEventListener('scroll', () => {
    let cur2 = '';
    secs.forEach(s => { if (window.scrollY >= s.offsetTop - 120) cur2 = s.id; });
    links.forEach(a => { a.style.color = a.getAttribute('href') === '#' + cur2 ? 'var(--accent)' : ''; });
  });
</script>
</body>
</html>
