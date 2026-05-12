<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MedEvent — Plataforma de Eventos Médicos</title>
<style>
  :root {
    --primary: #0066CC;
    --primary-dark: #004A99;
    --accent: #00C9A7;
    --accent2: #FF6B35;
    --dark: #0D1B2A;
    --dark2: #1A2E45;
    --light: #F0F6FF;
    --white: #ffffff;
    --gray: #6B7A8D;
    --border: rgba(0,102,204,0.15);
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  html { scroll-behavior: smooth; }
  body { font-family: 'Segoe UI', system-ui, sans-serif; background: var(--white); color: var(--dark); overflow-x: hidden; }

  /* NAV */
  nav {
    position: fixed; top:0; left:0; right:0; z-index:1000;
    background: rgba(13,27,42,0.95); backdrop-filter: blur(12px);
    border-bottom: 1px solid rgba(255,255,255,0.08);
    display: flex; align-items: center; justify-content: space-between;
    padding: 0 5%; height: 70px;
  }
  .nav-logo { display:flex; align-items:center; gap:10px; text-decoration:none; }
  .nav-logo .logo-icon {
    width:38px; height:38px; background: linear-gradient(135deg,var(--primary),var(--accent));
    border-radius:10px; display:flex; align-items:center; justify-content:center;
    font-size:20px;
  }
  .nav-logo span { color:var(--white); font-size:1.3rem; font-weight:700; }
  .nav-logo span b { color:var(--accent); }
  .nav-links { display:flex; gap:28px; list-style:none; }
  .nav-links a { color:rgba(255,255,255,0.75); text-decoration:none; font-size:.92rem; transition:.2s; }
  .nav-links a:hover { color:var(--white); }
  .nav-btns { display:flex; gap:10px; }
  .btn { padding:9px 20px; border-radius:8px; font-size:.9rem; font-weight:600; cursor:pointer; border:none; transition:.2s; text-decoration:none; display:inline-flex; align-items:center; gap:6px; }
  .btn-outline { background:transparent; border:1.5px solid rgba(255,255,255,0.3); color:var(--white); }
  .btn-outline:hover { border-color:var(--accent); color:var(--accent); }
  .btn-primary { background: linear-gradient(135deg,var(--primary),var(--accent)); color:var(--white); }
  .btn-primary:hover { transform:translateY(-1px); box-shadow:0 6px 20px rgba(0,102,204,.4); }
  .btn-accent { background: linear-gradient(135deg,var(--accent),#00a88e); color:var(--white); }
  .btn-accent:hover { transform:translateY(-1px); box-shadow:0 6px 20px rgba(0,201,167,.4); }
  .btn-lg { padding:14px 32px; font-size:1rem; border-radius:10px; }
  .btn-white { background:var(--white); color:var(--primary); font-weight:700; }
  .btn-white:hover { transform:translateY(-2px); box-shadow:0 8px 25px rgba(0,0,0,.2); }

  /* HERO */
  .hero {
    min-height: 100vh;
    background: linear-gradient(135deg, var(--dark) 0%, var(--dark2) 50%, #0a2240 100%);
    display: flex; flex-direction:column; align-items:center; justify-content:center;
    text-align:center; padding: 100px 5% 60px; position:relative; overflow:hidden;
  }
  .hero-bg-circles { position:absolute; inset:0; pointer-events:none; overflow:hidden; }
  .circle {
    position:absolute; border-radius:50%;
    background: radial-gradient(circle, rgba(0,102,204,0.15), transparent 70%);
    animation: pulse 4s ease-in-out infinite;
  }
  .circle:nth-child(1) { width:600px; height:600px; top:-100px; left:-100px; }
  .circle:nth-child(2) { width:400px; height:400px; bottom:-50px; right:-50px; animation-delay:2s; background: radial-gradient(circle,rgba(0,201,167,0.12),transparent 70%); }
  .circle:nth-child(3) { width:300px; height:300px; top:40%; left:60%; animation-delay:1s; }
  @keyframes pulse { 0%,100%{transform:scale(1);opacity:.6} 50%{transform:scale(1.1);opacity:1} }

  .hero-badge {
    display:inline-flex; align-items:center; gap:8px;
    background:rgba(0,201,167,0.15); border:1px solid rgba(0,201,167,0.3);
    color:var(--accent); padding:6px 16px; border-radius:50px; font-size:.82rem; font-weight:600;
    margin-bottom:28px; animation: fadeDown .6s ease;
  }
  .hero h1 {
    font-size: clamp(2.2rem, 5vw, 4rem); font-weight:800; color:var(--white);
    line-height:1.15; margin-bottom:22px; animation: fadeDown .7s ease;
  }
  .hero h1 span { background: linear-gradient(135deg,var(--accent),#7BFFE0); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
  .hero p { font-size:1.15rem; color:rgba(255,255,255,.65); max-width:600px; line-height:1.7; margin-bottom:40px; animation: fadeDown .8s ease; }
  .hero-btns { display:flex; gap:14px; flex-wrap:wrap; justify-content:center; animation: fadeDown .9s ease; }
  .hero-stats {
    display:flex; gap:40px; margin-top:70px; flex-wrap:wrap; justify-content:center;
    animation: fadeUp 1s ease;
  }
  .hero-stat { text-align:center; }
  .hero-stat strong { display:block; font-size:2rem; font-weight:800; color:var(--white); }
  .hero-stat span { font-size:.82rem; color:rgba(255,255,255,.5); }
  .hero-stat strong b { color:var(--accent); }
  @keyframes fadeDown { from{opacity:0;transform:translateY(-20px)} to{opacity:1;transform:translateY(0)} }
  @keyframes fadeUp { from{opacity:0;transform:translateY(20px)} to{opacity:1;transform:translateY(0)} }

  /* FLOATING CARDS */
  .floating-cards { position:absolute; right:5%; top:50%; transform:translateY(-50%); display:flex; flex-direction:column; gap:12px; }
  .fcard {
    background:rgba(255,255,255,.07); backdrop-filter:blur(10px);
    border:1px solid rgba(255,255,255,.12); border-radius:14px;
    padding:14px 18px; display:flex; align-items:center; gap:12px;
    animation: floatCard 3s ease-in-out infinite;
    min-width:220px;
  }
  .fcard:nth-child(2) { animation-delay:.8s; }
  .fcard:nth-child(3) { animation-delay:1.6s; }
  @keyframes floatCard { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-8px)} }
  .fcard-icon { width:40px; height:40px; border-radius:10px; display:flex; align-items:center; justify-content:center; font-size:1.2rem; flex-shrink:0; }
  .fcard-icon.blue { background:rgba(0,102,204,.25); }
  .fcard-icon.green { background:rgba(0,201,167,.25); }
  .fcard-icon.orange { background:rgba(255,107,53,.25); }
  .fcard strong { display:block; color:var(--white); font-size:.88rem; }
  .fcard span { color:rgba(255,255,255,.5); font-size:.75rem; }

  /* MARQUEE */
  .marquee-wrap { background:var(--primary-dark); padding:12px 0; overflow:hidden; }
  .marquee { display:flex; gap:50px; animation: marquee 20s linear infinite; white-space:nowrap; }
  .marquee span { color:rgba(255,255,255,.7); font-size:.85rem; display:flex; align-items:center; gap:8px; }
  .marquee span::before { content:'✦'; color:var(--accent); }
  @keyframes marquee { from{transform:translateX(0)} to{transform:translateX(-50%)} }

  /* SECTION BASE */
  section { padding:90px 5%; }
  .section-tag { display:inline-block; background:rgba(0,102,204,.1); color:var(--primary); font-size:.78rem; font-weight:700; text-transform:uppercase; letter-spacing:1.5px; padding:5px 14px; border-radius:50px; margin-bottom:14px; }
  .section-tag.green { background:rgba(0,201,167,.1); color:var(--accent); }
  .section-title { font-size:clamp(1.8rem,3.5vw,2.8rem); font-weight:800; color:var(--dark); line-height:1.2; }
  .section-title span { color:var(--primary); }
  .section-sub { color:var(--gray); font-size:1.02rem; line-height:1.7; margin-top:12px; max-width:560px; }
  .text-center { text-align:center; }
  .text-center .section-sub { margin:12px auto 0; }

  /* EVENTS */
  .events-section { background:var(--light); }
  .filters {
    display:flex; gap:10px; flex-wrap:wrap; margin:40px 0 32px;
  }
  .filter-btn {
    padding:8px 18px; border-radius:50px; border:1.5px solid var(--border);
    background:var(--white); color:var(--gray); font-size:.85rem; cursor:pointer;
    transition:.2s; font-weight:500;
  }
  .filter-btn.active, .filter-btn:hover { background:var(--primary); color:var(--white); border-color:var(--primary); }
  .events-grid { display:grid; grid-template-columns: repeat(auto-fill, minmax(300px,1fr)); gap:24px; }
  .event-card {
    background:var(--white); border-radius:18px; overflow:hidden;
    box-shadow:0 2px 20px rgba(0,0,0,.06); transition:.3s; cursor:pointer;
    border:1px solid var(--border);
  }
  .event-card:hover { transform:translateY(-6px); box-shadow:0 12px 40px rgba(0,102,204,.15); }
  .event-card.featured { border:2px solid var(--primary); }
  .event-img {
    height:160px; display:flex; align-items:center; justify-content:center;
    font-size:3rem; position:relative;
  }
  .event-img.blue { background: linear-gradient(135deg,#0066CC22,#0066CC44); }
  .event-img.green { background: linear-gradient(135deg,#00C9A722,#00C9A744); }
  .event-img.orange { background: linear-gradient(135deg,#FF6B3522,#FF6B3544); }
  .event-img.purple { background: linear-gradient(135deg,#7C3AED22,#7C3AED44); }
  .event-img.pink { background: linear-gradient(135deg,#EC489922,#EC489944); }
  .event-img.teal { background: linear-gradient(135deg,#0D948022,#0D948044); }
  .badge-feat {
    position:absolute; top:12px; right:12px;
    background: linear-gradient(135deg,var(--accent2),#ff8a50);
    color:var(--white); font-size:.7rem; font-weight:700; padding:4px 10px; border-radius:50px;
  }
  .badge-online {
    position:absolute; top:12px; left:12px;
    background:rgba(0,201,167,.9); color:var(--white); font-size:.7rem; font-weight:700; padding:4px 10px; border-radius:50px;
  }
  .event-body { padding:18px; }
  .event-spec { font-size:.72rem; font-weight:700; text-transform:uppercase; letter-spacing:1px; color:var(--primary); margin-bottom:6px; }
  .event-title { font-size:1rem; font-weight:700; color:var(--dark); margin-bottom:10px; line-height:1.3; }
  .event-meta { display:flex; flex-direction:column; gap:5px; }
  .event-meta-row { display:flex; align-items:center; gap:7px; font-size:.8rem; color:var(--gray); }
  .event-footer { display:flex; align-items:center; justify-content:space-between; margin-top:16px; padding-top:14px; border-top:1px solid var(--border); }
  .event-price { font-weight:800; color:var(--primary); font-size:1.05rem; }
  .event-price.free { color:var(--accent); }
  .btn-inscr { padding:8px 16px; background:var(--primary); color:var(--white); border:none; border-radius:8px; font-size:.82rem; font-weight:600; cursor:pointer; transition:.2s; }
  .btn-inscr:hover { background:var(--primary-dark); }
  .spots { font-size:.72rem; color:var(--gray); }
  .spots b { color:var(--accent2); }

  /* FOR CLINICS */
  .clinics-section { background:var(--white); }
  .two-col { display:grid; grid-template-columns:1fr 1fr; gap:70px; align-items:center; }
  .features-list { display:flex; flex-direction:column; gap:22px; margin-top:30px; }
  .feat-item { display:flex; gap:16px; align-items:flex-start; }
  .feat-icon { width:46px; height:46px; border-radius:12px; display:flex; align-items:center; justify-content:center; font-size:1.3rem; flex-shrink:0; }
  .feat-icon.blue { background:rgba(0,102,204,.1); }
  .feat-icon.green { background:rgba(0,201,167,.1); }
  .feat-icon.orange { background:rgba(255,107,53,.1); }
  .feat-icon.purple { background:rgba(124,58,237,.1); }
  .feat-text strong { font-weight:700; color:var(--dark); display:block; margin-bottom:4px; }
  .feat-text p { color:var(--gray); font-size:.88rem; line-height:1.6; }

  /* MOCKUP */
  .dashboard-mock {
    background: linear-gradient(135deg,var(--dark),var(--dark2));
    border-radius:20px; padding:24px; position:relative; overflow:hidden;
  }
  .mock-header { display:flex; align-items:center; gap:8px; margin-bottom:20px; }
  .mock-dot { width:12px; height:12px; border-radius:50%; }
  .mock-stat-row { display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-bottom:16px; }
  .mock-stat { background:rgba(255,255,255,.07); border-radius:12px; padding:14px; }
  .mock-stat .val { font-size:1.5rem; font-weight:800; color:var(--white); }
  .mock-stat .lbl { font-size:.72rem; color:rgba(255,255,255,.4); margin-top:2px; }
  .mock-stat .trend { font-size:.72rem; color:var(--accent); }
  .mock-chart { background:rgba(255,255,255,.05); border-radius:12px; padding:16px; margin-bottom:16px; height:100px; display:flex; align-items:flex-end; gap:6px; }
  .bar { flex:1; border-radius:6px 6px 0 0; transition:.3s; }
  .bar:hover { opacity:.8; }
  .mock-events { display:flex; flex-direction:column; gap:8px; }
  .mock-ev { background:rgba(255,255,255,.07); border-radius:10px; padding:10px 14px; display:flex; align-items:center; justify-content:space-between; }
  .mock-ev span { color:rgba(255,255,255,.7); font-size:.8rem; }
  .mock-ev b { color:var(--accent); font-size:.8rem; }

  /* PRICING */
  .pricing-section { background: linear-gradient(135deg,var(--dark) 0%,var(--dark2) 100%); }
  .pricing-section .section-title { color:var(--white); }
  .pricing-section .section-sub { color:rgba(255,255,255,.5); }
  .pricing-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(260px,1fr)); gap:24px; margin-top:50px; }
  .price-card {
    background:rgba(255,255,255,.05); border:1px solid rgba(255,255,255,.1);
    border-radius:20px; padding:32px 28px; transition:.3s; position:relative; overflow:hidden;
  }
  .price-card.popular {
    background:rgba(0,102,204,.2); border-color:var(--primary);
    transform:scale(1.03);
  }
  .price-card:hover { transform:translateY(-6px); }
  .price-card.popular:hover { transform:scale(1.03) translateY(-6px); }
  .pop-badge {
    position:absolute; top:0; right:24px;
    background: linear-gradient(135deg,var(--primary),var(--accent));
    color:var(--white); font-size:.72rem; font-weight:700; padding:4px 14px;
    border-radius:0 0 10px 10px;
  }
  .price-name { color:rgba(255,255,255,.6); font-size:.85rem; font-weight:600; text-transform:uppercase; letter-spacing:1px; }
  .price-val { font-size:2.8rem; font-weight:900; color:var(--white); margin:10px 0 4px; }
  .price-val sup { font-size:1.2rem; }
  .price-val span { font-size:1rem; font-weight:400; color:rgba(255,255,255,.4); }
  .price-desc { color:rgba(255,255,255,.45); font-size:.82rem; margin-bottom:24px; }
  .price-features { list-style:none; display:flex; flex-direction:column; gap:10px; margin-bottom:28px; }
  .price-features li { display:flex; align-items:center; gap:10px; color:rgba(255,255,255,.75); font-size:.87rem; }
  .price-features li::before { content:'✓'; color:var(--accent); font-weight:700; font-size:1rem; flex-shrink:0; }
  .price-features li.no { color:rgba(255,255,255,.3); }
  .price-features li.no::before { content:'✗'; color:rgba(255,255,255,.2); }

  /* TESTIMONIALS */
  .testimonials-section { background:var(--light); }
  .test-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(280px,1fr)); gap:22px; margin-top:50px; }
  .test-card {
    background:var(--white); border-radius:18px; padding:28px;
    box-shadow:0 2px 20px rgba(0,0,0,.06); border:1px solid var(--border);
  }
  .test-stars { color:#FFC107; font-size:1rem; margin-bottom:14px; }
  .test-text { color:var(--gray); font-size:.9rem; line-height:1.7; font-style:italic; margin-bottom:18px; }
  .test-author { display:flex; align-items:center; gap:12px; }
  .test-avatar { width:42px; height:42px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:1.1rem; flex-shrink:0; }
  .test-avatar.blue { background:rgba(0,102,204,.15); }
  .test-avatar.green { background:rgba(0,201,167,.15); }
  .test-avatar.orange { background:rgba(255,107,53,.15); }
  .test-author strong { display:block; font-size:.88rem; color:var(--dark); }
  .test-author span { font-size:.77rem; color:var(--gray); }

  /* HOW IT WORKS */
  .how-section { background:var(--white); }
  .steps { display:grid; grid-template-columns:repeat(auto-fit,minmax(200px,1fr)); gap:30px; margin-top:50px; position:relative; }
  .step { text-align:center; position:relative; }
  .step-num {
    width:56px; height:56px; border-radius:50%;
    background: linear-gradient(135deg,var(--primary),var(--accent));
    color:var(--white); font-size:1.3rem; font-weight:800;
    display:flex; align-items:center; justify-content:center; margin:0 auto 16px;
    position:relative; z-index:1;
  }
  .step h4 { font-size:1rem; font-weight:700; color:var(--dark); margin-bottom:8px; }
  .step p { font-size:.85rem; color:var(--gray); line-height:1.6; }

  /* CTA BANNER */
  .cta-section {
    background: linear-gradient(135deg,var(--primary-dark),var(--primary),#0099CC);
    text-align:center; padding:80px 5%;
  }
  .cta-section h2 { font-size:clamp(1.8rem,4vw,2.8rem); font-weight:800; color:var(--white); margin-bottom:14px; }
  .cta-section p { color:rgba(255,255,255,.75); font-size:1.05rem; margin-bottom:34px; }
  .cta-btns { display:flex; gap:14px; justify-content:center; flex-wrap:wrap; }

  /* NEWSLETTER */
  .newsletter {
    background:var(--dark); padding:60px 5%; display:flex;
    align-items:center; justify-content:space-between; gap:30px; flex-wrap:wrap;
  }
  .newsletter h3 { color:var(--white); font-size:1.4rem; font-weight:700; }
  .newsletter p { color:rgba(255,255,255,.5); font-size:.88rem; margin-top:6px; }
  .nl-form { display:flex; gap:10px; flex-wrap:wrap; }
  .nl-form input {
    padding:12px 20px; border-radius:10px; border:1px solid rgba(255,255,255,.15);
    background:rgba(255,255,255,.08); color:var(--white); font-size:.9rem;
    width:280px; outline:none;
  }
  .nl-form input::placeholder { color:rgba(255,255,255,.35); }

  /* FOOTER */
  footer {
    background:var(--dark); border-top:1px solid rgba(255,255,255,.08);
    padding:50px 5% 30px;
  }
  .footer-top { display:grid; grid-template-columns:2fr 1fr 1fr 1fr; gap:40px; margin-bottom:40px; }
  .footer-brand p { color:rgba(255,255,255,.45); font-size:.85rem; line-height:1.7; margin:14px 0 20px; }
  .footer-social { display:flex; gap:10px; }
  .soc-btn {
    width:36px; height:36px; border-radius:9px; background:rgba(255,255,255,.08);
    display:flex; align-items:center; justify-content:center; font-size:1rem;
    cursor:pointer; transition:.2s; border:none; color:rgba(255,255,255,.6);
  }
  .soc-btn:hover { background:var(--primary); color:var(--white); }
  .footer-col h4 { color:var(--white); font-size:.9rem; font-weight:700; margin-bottom:16px; }
  .footer-col ul { list-style:none; display:flex; flex-direction:column; gap:9px; }
  .footer-col ul a { color:rgba(255,255,255,.45); font-size:.84rem; text-decoration:none; transition:.2s; }
  .footer-col ul a:hover { color:var(--white); }
  .footer-bottom { border-top:1px solid rgba(255,255,255,.08); padding-top:24px; display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:10px; }
  .footer-bottom span { color:rgba(255,255,255,.3); font-size:.8rem; }

  /* MODAL */
  .modal-overlay {
    position:fixed; inset:0; background:rgba(0,0,0,.7); backdrop-filter:blur(6px);
    z-index:2000; display:flex; align-items:center; justify-content:center;
    opacity:0; pointer-events:none; transition:.3s;
  }
  .modal-overlay.open { opacity:1; pointer-events:all; }
  .modal {
    background:var(--white); border-radius:24px; padding:40px; max-width:500px; width:90%;
    transform:scale(.9); transition:.3s; position:relative; max-height:90vh; overflow-y:auto;
  }
  .modal-overlay.open .modal { transform:scale(1); }
  .modal-close {
    position:absolute; top:16px; right:16px; width:32px; height:32px;
    border-radius:50%; background:var(--light); border:none; cursor:pointer;
    font-size:1rem; display:flex; align-items:center; justify-content:center;
  }
  .modal h3 { font-size:1.5rem; font-weight:800; color:var(--dark); margin-bottom:6px; }
  .modal p { color:var(--gray); font-size:.88rem; margin-bottom:24px; }
  .form-group { margin-bottom:16px; }
  .form-group label { display:block; font-size:.82rem; font-weight:600; color:var(--dark); margin-bottom:6px; }
  .form-group input, .form-group select {
    width:100%; padding:11px 14px; border:1.5px solid var(--border);
    border-radius:10px; font-size:.9rem; color:var(--dark); outline:none; transition:.2s;
  }
  .form-group input:focus, .form-group select:focus { border-color:var(--primary); }
  .form-row { display:grid; grid-template-columns:1fr 1fr; gap:12px; }
  .success-msg { text-align:center; padding:20px; }
  .success-msg .check { font-size:3rem; margin-bottom:10px; }
  .success-msg h4 { color:var(--dark); font-size:1.2rem; font-weight:700; }
  .success-msg p { color:var(--gray); }
  .tabs { display:flex; gap:0; margin-bottom:24px; border:1.5px solid var(--border); border-radius:10px; overflow:hidden; }
  .tab { flex:1; padding:10px; text-align:center; cursor:pointer; font-size:.88rem; font-weight:600; color:var(--gray); background:var(--white); border:none; transition:.2s; }
  .tab.active { background:var(--primary); color:var(--white); }

  @media(max-width:900px) {
    .floating-cards { display:none; }
    .two-col { grid-template-columns:1fr; gap:40px; }
    .footer-top { grid-template-columns:1fr 1fr; }
    .nav-links { display:none; }
  }
  @media(max-width:600px) {
    .footer-top { grid-template-columns:1fr; }
    .hero-stats { gap:24px; }
  }
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <a class="nav-logo" href="#">
    <div class="logo-icon">🏥</div>
    <span>Med<b>Event</b></span>
  </a>
  <ul class="nav-links">
    <li><a href="#events">Eventos</a></li>
    <li><a href="#clinics">Para Clínicas</a></li>
    <li><a href="#pricing">Planos</a></li>
    <li><a href="#how">Como Funciona</a></li>
  </ul>
  <div class="nav-btns">
    <a class="btn btn-outline" href="#" onclick="openModal('login')">Entrar</a>
    <a class="btn btn-primary" href="#" onclick="openModal('register')">Registar Grátis</a>
  </div>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-bg-circles">
    <div class="circle"></div><div class="circle"></div><div class="circle"></div>
  </div>
  <div class="hero-badge">🚀 Mais de 500 eventos publicados este mês</div>
  <h1>A plataforma que conecta<br><span>Clínicas & Profissionais</span><br>da Medicina</h1>
  <p>Publique eventos médicos, gerencie inscrições e alcance milhares de profissionais de saúde em toda Angola e além fronteiras.</p>
  <div class="hero-btns">
    <a class="btn btn-primary btn-lg" href="#" onclick="openModal('clinic')">🏥 Registar Clínica</a>
    <a class="btn btn-white btn-lg" href="#events">🔍 Explorar Eventos</a>
  </div>
  <div class="hero-stats">
    <div class="hero-stat"><strong>2<b>K+</b></strong><span>Profissionais</span></div>
    <div class="hero-stat"><strong>150<b>+</b></strong><span>Clínicas</span></div>
    <div class="hero-stat"><strong>500<b>+</b></strong><span>Eventos</span></div>
    <div class="hero-stat"><strong>98<b>%</b></strong><span>Satisfação</span></div>
  </div>
  <div class="floating-cards">
    <div class="fcard">
      <div class="fcard-icon blue">📅</div>
      <div><strong>Congresso de Cardiologia</strong><span>45 inscrições hoje</span></div>
    </div>
    <div class="fcard">
      <div class="fcard-icon green">✅</div>
      <div><strong>Nova inscrição confirmada</strong><span>há 2 minutos</span></div>
    </div>
    <div class="fcard">
      <div class="fcard-icon orange">📈</div>
      <div><strong>+320% de alcance</strong><span>Clínica São Lucas</span></div>
    </div>
  </div>
</section>

<!-- MARQUEE -->
<div class="marquee-wrap">
  <div class="marquee" id="marquee">
    <span>Cardiologia</span><span>Neurologia</span><span>Pediatria</span>
    <span>Oncologia</span><span>Cirurgia</span><span>Dermatologia</span>
    <span>Ortopedia</span><span>Ginecologia</span><span>Psiquiatria</span>
    <span>Radiologia</span><span>Urgências</span><span>Endocrinologia</span>
    <span>Cardiologia</span><span>Neurologia</span><span>Pediatria</span>
    <span>Oncologia</span><span>Cirurgia</span><span>Dermatologia</span>
    <span>Ortopedia</span><span>Ginecologia</span><span>Psiquiatria</span>
    <span>Radiologia</span><span>Urgências</span><span>Endocrinologia</span>
  </div>
</div>

<!-- EVENTS -->
<section class="events-section" id="events">
  <div class="text-center">
    <div class="section-tag">Próximos Eventos</div>
    <h2 class="section-title">Encontre o evento <span>ideal para si</span></h2>
    <p class="section-sub">Congressos, workshops, formações e muito mais — filtrados para a sua especialidade.</p>
  </div>
  <div class="filters">
    <button class="filter-btn active" onclick="filterEvents(this,'all')">Todos</button>
    <button class="filter-btn" onclick="filterEvents(this,'congresso')">Congressos</button>
    <button class="filter-btn" onclick="filterEvents(this,'workshop')">Workshops</button>
    <button class="filter-btn" onclick="filterEvents(this,'formacao')">Formações</button>
    <button class="filter-btn" onclick="filterEvents(this,'online')">Online</button>
  </div>
  <div class="events-grid" id="eventsGrid">
    <!-- Cards injetados por JS -->
  </div>
  <div style="text-align:center;margin-top:40px">
    <a class="btn btn-primary btn-lg" href="#" onclick="openModal('register')">Ver Todos os Eventos →</a>
  </div>
</section>

<!-- FOR CLINICS -->
<section class="clinics-section" id="clinics">
  <div class="two-col">
    <div>
      <div class="section-tag">Para Clínicas & Instituições</div>
      <h2 class="section-title">Aumente o seu <span>alcance e impacto</span></h2>
      <p class="section-sub">A MedEvent dá-lhe as ferramentas para promover os seus eventos a milhares de profissionais qualificados.</p>
      <div class="features-list">
        <div class="feat-item">
          <div class="feat-icon blue">📢</div>
          <div class="feat-text">
            <strong>Publicidade Segmentada</strong>
            <p>Os seus eventos são promovidos a profissionais da especialidade certa, aumentando as conversões.</p>
          </div>
        </div>
        <div class="feat-item">
          <div class="feat-icon green">📊</div>
          <div class="feat-text">
            <strong>Dashboard de Analytics</strong>
            <p>Acompanhe inscrições, visualizações, receitas e o desempenho de cada evento em tempo real.</p>
          </div>
        </div>
        <div class="feat-item">
          <div class="feat-icon orange">💳</div>
          <div class="feat-text">
            <strong>Pagamentos Integrados</strong>
            <p>Receba pagamentos de forma segura directamente na plataforma, sem complicações.</p>
          </div>
        </div>
        <div class="feat-item">
          <div class="feat-icon purple">🏆</div>
          <div class="feat-text">
            <strong>Selo de Clínica Verificada</strong>
            <p>Ganhe credibilidade e destaque nos resultados de pesquisa com o nosso selo de verificação.</p>
          </div>
        </div>
      </div>
      <div style="margin-top:30px;display:flex;gap:12px;flex-wrap:wrap">
        <a class="btn btn-primary btn-lg" href="#" onclick="openModal('clinic')">Começar Agora</a>
        <a class="btn btn-outline" style="border-color:var(--border);color:var(--dark)" href="#pricing">Ver Planos</a>
      </div>
    </div>
    <div class="dashboard-mock">
      <div class="mock-header">
        <div class="mock-dot" style="background:#FF5F57"></div>
        <div class="mock-dot" style="background:#FFBD2E"></div>
        <div class="mock-dot" style="background:#28CA41"></div>
        <span style="color:rgba(255,255,255,.3);font-size:.78rem;margin-left:10px">Dashboard — Clínica São Lucas</span>
      </div>
      <div class="mock-stat-row">
        <div class="mock-stat">
          <div class="val">1.240</div>
          <div class="lbl">Visualizações</div>
          <div class="trend">↑ +32% esta semana</div>
        </div>
        <div class="mock-stat">
          <div class="val">89</div>
          <div class="lbl">Inscrições</div>
          <div class="trend">↑ +18% esta semana</div>
        </div>
        <div class="mock-stat">
          <div class="val">KZ 450K</div>
          <div class="lbl">Receita</div>
          <div class="trend">↑ +24% este mês</div>
        </div>
        <div class="mock-stat">
          <div class="val">4</div>
          <div class="lbl">Eventos Activos</div>
          <div class="trend" style="color:var(--accent2)">2 a terminar</div>
        </div>
      </div>
      <div class="mock-chart" id="mockChart"></div>
      <div class="mock-events">
        <div class="mock-ev"><span>🫀 Congresso de Cardiologia</span><b>32 inscritos</b></div>
        <div class="mock-ev"><span>🧠 Workshop de Neurologia</span><b>21 inscritos</b></div>
        <div class="mock-ev"><span>👶 Formação em Pediatria</span><b>36 inscritos</b></div>
      </div>
    </div>
  </div>
</section>

<!-- HOW IT WORKS -->
<section class="how-section" id="how" style="background:var(--light)">
  <div class="text-center">
    <div class="section-tag">Como Funciona</div>
    <h2 class="section-title">Simples, rápido e <span>eficaz</span></h2>
    <p class="section-sub">Em apenas 3 passos começa a publicar ou a inscrever-se em eventos médicos.</p>
  </div>
  <div class="tabs" style="max-width:360px;margin:40px auto 0">
    <button class="tab active" onclick="switchTab(this,'clinicas')">Para Clínicas</button>
    <button class="tab" onclick="switchTab(this,'users')">Para Utilizadores</button>
  </div>
  <div class="steps" id="stepsContent">
    <div class="step">
      <div class="step-num">1</div>
      <h4>Crie a sua Conta</h4>
      <p>Registe a sua clínica em menos de 5 minutos e escolha o plano que melhor se adapta às suas necessidades.</p>
    </div>
    <div class="step">
      <div class="step-num">2</div>
      <h4>Publique o seu Evento</h4>
      <p>Adicione detalhes, imagens, palestrantes, agenda e defina o preço ou torne-o gratuito.</p>
    </div>
    <div class="step">
      <div class="step-num">3</div>
      <h4>Alcance Profissionais</h4>
      <p>O seu evento é automaticamente promovido a profissionais da área certa, gerando inscrições imediatas.</p>
    </div>
    <div class="step">
      <div class="step-num">4</div>
      <h4>Gerencie & Cresça</h4>
      <p>Acompanhe tudo no dashboard, emita certificados e construa a reputação da sua instituição.</p>
    </div>
  </div>
</section>

<!-- PRICING -->
<section class="pricing-section" id="pricing">
  <div class="text-center">
    <div class="section-tag green">Planos & Preços</div>
    <h2 class="section-title">Invista no crescimento <span style="color:var(--accent)">da sua clínica</span></h2>
    <p class="section-sub">Escolha o plano ideal para as suas necessidades. Sem surpresas, sem taxas escondidas.</p>
  </div>
  <div class="pricing-grid">
    <div class="price-card">
      <div class="price-name">Básico</div>
      <div class="price-val"><sup>KZ</sup>0<span>/mês</span></div>
      <div class="price-desc">Comece gratuitamente e explore a plataforma.</div>
      <ul class="price-features">
        <li>1 evento activo por mês</li>
        <li>Até 50 inscrições por evento</li>
        <li>Perfil de clínica básico</li>
        <li>Relatórios simples</li>
        <li class="no">Destaque na pesquisa</li>
        <li class="no">Certificados automáticos</li>
        <li class="no">Suporte prioritário</li>
      </ul>
      <button class="btn btn-outline" style="width:100%;border-color:rgba(255,255,255,.2);color:var(--white)" onclick="openModal('clinic')">Começar Grátis</button>
    </div>
    <div class="price-card popular">
      <div class="pop-badge">⭐ Mais Popular</div>
      <div class="price-name">Profissional</div>
      <div class="price-val"><sup>KZ</sup>25.000<span>/mês</span></div>
      <div class="price-desc">Para clínicas que querem crescer consistentemente.</div>
      <ul class="price-features">
        <li>Eventos ilimitados</li>
        <li>Inscrições ilimitadas</li>
        <li>Destaque na pesquisa</li>
        <li>Certificados automáticos</li>
        <li>Analytics avançados</li>
        <li>Pagamentos integrados</li>
        <li>Suporte prioritário 24/7</li>
      </ul>
      <button class="btn btn-primary" style="width:100%" onclick="openModal('clinic')">Escolher Plano</button>
    </div>
    <div class="price-card">
      <div class="price-name">Premium</div>
      <div class="price-val"><sup>KZ</sup>60.000<span>/mês</span></div>
      <div class="price-desc">Para instituições que querem o máximo de visibilidade.</div>
      <ul class="price-features">
        <li>Tudo do Profissional</li>
        <li>Posição nº1 garantida</li>
        <li>Banner publicitário no site</li>
        <li>Notificações push para users</li>
        <li>Gestor de conta dedicado</li>
        <li>Campanhas de email marketing</li>
        <li>Relatórios personalizados</li>
      </ul>
      <button class="btn btn-accent" style="width:100%" onclick="openModal('clinic')">Escolher Premium</button>
    </div>
  </div>
</section>

<!-- TESTIMONIALS -->
<section class="testimonials-section">
  <div class="text-center">
    <div class="section-tag">Testemunhos</div>
    <h2 class="section-title">O que dizem as nossas <span>clínicas</span></h2>
  </div>
  <div class="test-grid">
    <div class="test-card">
      <div class="test-stars">★★★★★</div>
      <p class="test-text">"A MedEvent revolucionou a forma como promovemos os nossos congressos. Em 3 meses triplicámos as inscrições e reduzimos o tempo de gestão em 70%."</p>
      <div class="test-author">
        <div class="test-avatar blue">👨‍⚕️</div>
        <div><strong>Dr. António Carvalho</strong><span>Clínica Girassol, Luanda</span></div>
      </div>
    </div>
    <div class="test-card">
      <div class="test-stars">★★★★★</div>
      <p class="test-text">"O dashboard de analytics é incrível. Sabemos exactamente quem se inscreveu, de onde vêm e podemos ajustar a nossa estratégia em tempo real."</p>
      <div class="test-author">
        <div class="test-avatar green">👩‍⚕️</div>
        <div><strong>Dra. Fernanda Lima</strong><span>Hospital Sagrada Família</span></div>
      </div>
    </div>
    <div class="test-card">
      <div class="test-stars">★★★★★</div>
      <p class="test-text">"Nunca foi tão fácil alcançar profissionais da nossa especialidade. O sistema de publicidade segmentada faz toda a diferença."</p>
      <div class="test-author">
        <div class="test-avatar orange">👨‍⚕️</div>
        <div><strong>Dr. Manuel Gaspar</strong><span>Centro Médico Bem-Estar</span></div>
      </div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section">
  <h2>Pronto para transformar<br>os seus eventos médicos?</h2>
  <p>Junte-se a mais de 150 clínicas que já usam a MedEvent para crescer.</p>
  <div class="cta-btns">
    <a class="btn btn-white btn-lg" href="#" onclick="openModal('clinic')">🏥 Registar a Minha Clínica</a>
    <a class="btn btn-outline btn-lg" href="#" onclick="openModal('register')">👤 Criar Conta Gratuita</a>
  </div>
</section>

<!-- NEWSLETTER -->
<div class="newsletter">
  <div>
    <h3>📬 Fique sempre actualizado</h3>
    <p>Receba os melhores eventos médicos directamente no seu email.</p>
  </div>
  <div class="nl-form">
    <input type="email" placeholder="O seu melhor email..." id="nlEmail">
    <button class="btn btn-accent" onclick="subscribeNL()">Subscrever</button>
  </div>
</div>

<!-- FOOTER -->
<footer>
  <div class="footer-top">
    <div class="footer-brand">
      <a class="nav-logo" href="#" style="margin-bottom:14px;display:inline-flex">
        <div class="logo-icon">🏥</div>
        <span style="color:white">Med<b>Event</b></span>
      </a>
      <p>A plataforma líder de eventos médicos em Angola. Conectamos clínicas e profissionais de saúde para um futuro mais saudável.</p>
      <div class="footer-social">
        <button class="soc-btn">𝕏</button>
        <button class="soc-btn">in</button>
        <button class="soc-btn">f</button>
        <button class="soc-btn">▶</button>
      </div>
    </div>
    <div class="footer-col">
      <h4>Plataforma</h4>
      <ul>
        <li><a href="#">Explorar Eventos</a></li>
        <li><a href="#">Para Clínicas</a></li>
        <li><a href="#">Planos & Preços</a></li>
        <li><a href="#">Como Funciona</a></li>
        <li><a href="#">Certificados</a></li>
      </ul>
    </div>
    <div class="footer-col">
      <h4>Especialidades</h4>
      <ul>
        <li><a href="#">Cardiologia</a></li>
        <li><a href="#">Neurologia</a></li>
        <li><a href="#">Pediatria</a></li>
        <li><a href="#">Oncologia</a></li>
        <li><a href="#">Ver todas →</a></li>
      </ul>
    </div>
    <div class="footer-col">
      <h4>Empresa</h4>
      <ul>
        <li><a href="#">Sobre Nós</a></li>
        <li><a href="#">Contacto</a></li>
        <li><a href="#">Parceiros</a></li>
        <li><a href="#">Termos de Uso</a></li>
        <li><a href="#">Privacidade</a></li>
      </ul>
    </div>
  </div>
  <div class="footer-bottom">
    <span>© 2025 MedEvent. Todos os direitos reservados.</span>
    <span>Feito com ❤️ para a saúde em Angola</span>
  </div>
</footer>

<!-- MODAL -->
<div class="modal-overlay" id="modal" onclick="closeModalOutside(event)">
  <div class="modal" id="modalBox">
    <button class="modal-close" onclick="closeModal()">✕</button>
    <div id="modalContent"></div>
  </div>
</div>

<script>
// ========== EVENTS DATA ==========
const events = [
  { type:'congresso', title:'XII Congresso Nacional de Cardiologia', spec:'Cardiologia', date:'15 Jun 2025', local:'Luanda, CICF', price:'KZ 15.000', free:false, feat:true, online:false, emoji:'🫀', color:'blue', spots:12 },
  { type:'workshop', title:'Workshop de Cirurgia Minimamente Invasiva', spec:'Cirurgia', date:'22 Jun 2025', local:'Online', price:'KZ 8.000', free:false, feat:false, online:true, emoji:'🔬', color:'green', spots:5 },
  { type:'formacao', title:'Formação em Cuidados Paliativos', spec:'Oncologia', date:'1 Jul 2025', local:'Benguela', price:'Gratuito', free:true, feat:false, online:false, emoji:'🩺', color:'orange', spots:30 },
  { type:'congresso', title:'Simpósio Internacional de Neurologia', spec:'Neurologia', date:'10 Jul 2025', local:'Online', price:'KZ 20.000', free:false, feat:true, online:true, emoji:'🧠', color:'purple', spots:8 },
  { type:'formacao', title:'Actualização em Pediatria 2025', spec:'Pediatria', date:'18 Jul 2025', local:'Luanda, Hotel Epic Sana', price:'KZ 5.000', free:false, feat:false, online:false, emoji:'👶', color:'pink', spots:22 },
  { type:'workshop', title:'Gestão Clínica e Liderança em Saúde', spec:'Gestão', date:'25 Jul 2025', local:'Online', price:'Gratuito', free:true, feat:false, online:true, emoji:'📊', color:'teal', spots:50 },
];

let currentFilter = 'all';

function filterEvents(btn, type) {
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  currentFilter = type;
  renderEvents();
}

function renderEvents() {
  const grid = document.getElementById('eventsGrid');
  const filtered = currentFilter === 'all' ? events :
    currentFilter === 'online' ? events.filter(e => e.online) :
    events.filter(e => e.type === currentFilter);
  grid.innerHTML = filtered.map(e => `
    <div class="event-card ${e.feat?'featured':''}" onclick="openModal('event','${e.title}')">
      <div class="event-img ${e.color}">
        <span>${e.emoji}</span>
        ${e.feat ? '<div class="badge-feat">⭐ Destaque</div>' : ''}
        ${e.online ? '<div class="badge-online">🌐 Online</div>' : ''}
      </div>
      <div class="event-body">
        <div class="event-spec">${e.spec}</div>
        <div class="event-title">${e.title}</div>
        <div class="event-meta">
          <div class="event-meta-row">📅 ${e.date}</div>
          <div class="event-meta-row">📍 ${e.local}</div>
        </div>
        <div class="event-footer">
          <div>
            <div class="event-price ${e.free?'free':''}">${e.price}</div>
            <div class="spots">Restam <b>${e.spots}</b> vagas</div>
          </div>
          <button class="btn-inscr">Inscrever →</button>
        </div>
      </div>
    </div>
  `).join('');
}

renderEvents();

// ========== MOCK CHART ==========
const bars = [40,65,45,80,55,90,70,85,60,95,75,88];
const colors = ['#0066CC','#00C9A7','#0066CC','#00C9A7','#0066CC','#00C9A7','#0066CC','#00C9A7','#0066CC','#00C9A7','#0066CC','#00C9A7'];
const chart = document.getElementById('mockChart');
bars.forEach((h,i) => {
  const b = document.createElement('div');
  b.className = 'bar';
  b.style.height = h + '%';
  b.style.background = colors[i];
  b.style.opacity = '.7';
  chart.appendChild(b);
});

// ========== MODAL ==========
function openModal(type, data) {
  const modal = document.getElementById('modal');
  const content = document.getElementById('modalContent');
  if (type === 'login') {
    content.innerHTML = `
      <h3>👋 Bem-vindo de volta</h3>
      <p>Entre na sua conta MedEvent</p>
      <div class="tabs">
        <button class="tab active">Utilizador</button>
        <button class="tab">Clínica</button>
      </div>
      <div class="form-group"><label>Email</label><input type="email" placeholder="email@exemplo.com"></div>
      <div class="form-group"><label>Palavra-passe</label><input type="password" placeholder="••••••••"></div>
      <button class="btn btn-primary" style="width:100%;margin-top:8px;padding:13px" onclick="showSuccess('Sessão iniciada com sucesso! Bem-vindo.')">Entrar</button>
      <p style="text-align:center;margin-top:16px;font-size:.83rem;color:var(--gray)">Não tem conta? <a href="#" onclick="openModal('register')" style="color:var(--primary)">Registe-se grátis</a></p>
    `;
  } else if (type === 'register') {
    content.innerHTML = `
      <h3>🚀 Crie a sua conta</h3>
      <p>Aceda a todos os eventos médicos de Angola</p>
      <div class="form-row">
        <div class="form-group"><label>Nome</label><input type="text" placeholder="João"></div>
        <div class="form-group"><label>Apelido</label><input type="text" placeholder="Silva"></div>
      </div>
      <div class="form-group"><label>Email</label><input type="email" placeholder="email@exemplo.com"></div>
      <div class="form-group"><label>Especialidade</label>
        <select><option>Cardiologia</option><option>Neurologia</option><option>Pediatria</option><option>Cirurgia</option><option>Oncologia</option><option>Outra</option></select>
      </div>
      <div class="form-group"><label>Palavra-passe</label><input type="password" placeholder="••••••••"></div>
      <button class="btn btn-primary" style="width:100%;margin-top:8px;padding:13px" onclick="showSuccess('Conta criada! Verifique o seu email.')">Criar Conta Grátis</button>
    `;
  } else if (type === 'clinic') {
    content.innerHTML = `
      <h3>🏥 Registar Clínica</h3>
      <p>Comece a publicar eventos e alcançar milhares de profissionais</p>
      <div class="form-group"><label>Nome da Clínica/Instituição</label><input type="text" placeholder="Ex: Clínica São Lucas"></div>
      <div class="form-group"><label>Especialidade Principal</label>
        <select><option>Geral / Multidisciplinar</option><option>Cardiologia</option><option>Neurologia</option><option>Oncologia</option><option>Pediatria</option></select>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Telefone</label><input type="tel" placeholder="+244 9XX XXX XXX"></div>
        <div class="form-group"><label>Província</label>
          <select><option>Luanda</option><option>Benguela</option><option>Huambo</option><option>Outra</option></select>
        </div>
      </div>
      <div class="form-group"><label>Email Institucional</label><input type="email" placeholder="info@clinica.ao"></div>
      <div class="form-group"><label>Plano</label>
        <select><option>Básico (Grátis)</option><option>Profissional — KZ 25.000/mês</option><option>Premium — KZ 60.000/mês</option></select>
      </div>
      <button class="btn btn-primary" style="width:100%;margin-top:8px;padding:13px" onclick="showSuccess('Pedido enviado! A nossa equipa entrará em contacto em 24h.')">Registar Clínica</button>
    `;
  } else if (type === 'event') {
    const ev = events.find(e => e.title === data);
    if (!ev) return;
    content.innerHTML = `
      <div style="text-align:center;font-size:3rem;margin-bottom:10px">${ev.emoji}</div>
      <div class="section-tag" style="margin-bottom:10px">${ev.spec}</div>
      <h3 style="margin-bottom:8px">${ev.title}</h3>
      <p style="color:var(--gray);margin-bottom:20px">Uma oportunidade única de aprendizagem e networking com os melhores especialistas da área.</p>
      <div style="background:var(--light);border-radius:12px;padding:16px;margin-bottom:20px">
        <div style="display:flex;flex-direction:column;gap:8px">
          <div style="display:flex;gap:10px;font-size:.88rem"><span>📅</span><span><b>Data:</b> ${ev.date}</span></div>
          <div style="display:flex;gap:10px;font-size:.88rem"><span>📍</span><span><b>Local:</b> ${ev.local}</span></div>
          <div style="display:flex;gap:10px;font-size:.88rem"><span>💰</span><span><b>Valor:</b> ${ev.price}</span></div>
          <div style="display:flex;gap:10px;font-size:.88rem"><span>🪑</span><span><b>Vagas disponíveis:</b> ${ev.spots}</span></div>
        </div>
      </div>
      <div class="form-group"><label>Nome Completo</label><input type="text" placeholder="O seu nome"></div>
      <div class="form-group"><label>Email</label><input type="email" placeholder="email@exemplo.com"></div>
      <button class="btn btn-primary" style="width:100%;padding:13px" onclick="showSuccess('Inscrição realizada com sucesso! Verifique o seu email.')">
        ${ev.free ? '✅ Inscrever Gratuitamente' : `💳 Inscrever — ${ev.price}`}
      </button>
    `;
  }
  modal.classList.add('open');
}

function showSuccess(msg) {
  document.getElementById('modalContent').innerHTML = `
    <div class="success-msg">
      <div class="check">✅</div>
      <h4>Sucesso!</h4>
      <p style="margin-top:8px">${msg}</p>
      <button class="btn btn-primary" style="margin-top:20px" onclick="closeModal()">Fechar</button>
    </div>
  `;
}

function closeModal() { document.getElementById('modal').classList.remove('open'); }
function closeModalOutside(e) { if(e.target.id==='modal') closeModal(); }

// ========== STEPS TAB ==========
const stepsData = {
  clinicas: [
    {n:1, t:'Crie a sua Conta', p:'Registe a sua clínica em menos de 5 minutos e escolha o plano que melhor se adapta.'},
    {n:2, t:'Publique o seu Evento', p:'Adicione detalhes, imagens, palestrantes, agenda e defina o preço do evento.'},
    {n:3, t:'Alcance Profissionais', p:'O evento é promovido automaticamente a profissionais da área certa.'},
    {n:4, t:'Gerencie & Cresça', p:'Acompanhe tudo no dashboard, emita certificados e construa reputação.'},
  ],
  users: [
    {n:1, t:'Registe-se Grátis', p:'Crie a sua conta de profissional em segundos com o seu email.'},
    {n:2, t:'Explore Eventos', p:'Filtre por especialidade, data, localização ou formato online/presencial.'},
    {n:3, t:'Inscreva-se', p:'Inscreva-se em segundos e receba a confirmação no seu email.'},
    {n:4, t:'Aprenda & Certifique-se', p:'Participe no evento e receba o seu certificado digital automaticamente.'},
  ],
};

function switchTab(btn, type) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
  const s = document.getElementById('stepsContent');
  s.innerHTML = stepsData[type].map(st => `
    <div class="step">
      <div class="step-num">${st.n}</div>
      <h4>${st.t}</h4>
      <p>${st.p}</p>
    </div>
  `).join('');
}

// ========== NEWSLETTER ==========
function subscribeNL() {
  const v = document.getElementById('nlEmail').value;
  if (!v.includes('@')) { alert('Insira um email válido.'); return; }
  document.getElementById('nlEmail').value = '';
  alert('✅ Subscrito com sucesso! Obrigado.');
}

// ========== SCROLL ANIMATIONS ==========
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => { if(e.isIntersecting) { e.target.style.opacity='1'; e.target.style.transform='translateY(0)'; } });
}, { threshold: 0.1 });

document.querySelectorAll('.event-card,.price-card,.test-card,.feat-item,.step').forEach(el => {
  el.style.opacity='0'; el.style.transform='translateY(24px)'; el.style.transition='opacity .5s ease, transform .5s ease';
  obs.observe(el);
});
</script>
</body>
</html>
