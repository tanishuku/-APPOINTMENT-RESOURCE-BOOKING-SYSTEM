<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#090d18">
  <meta name="description" content="Orbit — an appointment and resource booking dashboard for student projects.">
  <title>Orbit | Appointment & Resource Booking</title>
  <style>
    :root {
      --bg: #080c16;
      --panel: rgba(17, 24, 42, .88);
      --panel-2: #121a2d;
      --panel-3: #18233a;
      --text: #eef3ff;
      --muted: #8f9bb7;
      --line: rgba(150, 169, 214, .14);
      --cyan: #5ee7f5;
      --blue: #7b8cff;
      --violet: #b28aff;
      --green: #75e6b1;
      --amber: #ffd27a;
      --red: #ff8f9a;
      --radius: 20px;
      --shadow: 0 18px 55px rgba(0,0,0,.24);
    }
    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      min-height: 100vh;
      color: var(--text);
      background:
        radial-gradient(ellipse at 16% 0%, rgba(78, 107, 255, .15), transparent 34%),
        radial-gradient(ellipse at 88% 10%, rgba(63, 221, 232, .10), transparent 30%),
        var(--bg);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      line-height: 1.5;
    }
    button, input, select, textarea { font: inherit; }
    button { cursor: pointer; }
    .app { display: grid; grid-template-columns: 248px minmax(0,1fr); min-height: 100vh; }
    .sidebar {
      border-right: 1px solid var(--line);
      padding: 26px 17px;
      background: rgba(9, 13, 25, .76);
      display: flex; flex-direction: column; gap: 28px;
    }
    .brand { display: flex; align-items: center; gap: 12px; padding: 0 10px; }
    .brand-mark {
      width: 39px; height: 39px; border-radius: 13px; display: grid; place-items: center;
      color: #08101d; font-weight: 900; font-size: 21px;
      background: linear-gradient(135deg, var(--cyan), var(--blue));
      box-shadow: 0 0 26px rgba(94,231,245,.22);
    }
    .brand-name { font-size: 19px; font-weight: 800; letter-spacing: -.6px; }
    .brand-sub { font-size: 10px; color: var(--muted); letter-spacing: 1.8px; text-transform: uppercase; }
    .nav-label { color: #65718e; font-size: 10px; letter-spacing: 1.7px; font-weight: 800; padding: 0 12px; margin-bottom: 8px; }
    .nav { display: grid; gap: 6px; }
    .nav button {
      border: 1px solid transparent; background: transparent; color: var(--muted);
      padding: 11px 13px; border-radius: 12px; text-align: left; display: flex; align-items: center; gap: 12px;
      transition: .2s ease;
    }
    .nav button:hover, .nav button.active {
      color: var(--text); background: linear-gradient(90deg, rgba(94,231,245,.12), rgba(123,140,255,.08));
      border-color: rgba(94,231,245,.16);
    }
    .nav-icon { width: 21px; text-align: center; font-size: 16px; }
    .sidebar-bottom { margin-top: auto; }
    .help-card {
      border: 1px solid var(--line); background: linear-gradient(145deg, rgba(123,140,255,.12), rgba(94,231,245,.05));
      padding: 16px; border-radius: 16px;
    }
    .help-card strong { display: block; font-size: 13px; margin-bottom: 5px; }
    .help-card p { margin: 0 0 12px; font-size: 12px; color: var(--muted); }
    .help-card button { background: transparent; border: 1px solid #364461; color: var(--text); border-radius: 9px; padding: 8px 10px; font-size: 12px; }
    main { min-width: 0; padding: 28px clamp(18px, 3vw, 42px) 40px; max-width: 1650px; width: 100%; margin: 0 auto; }
    .topbar { display: flex; align-items: center; justify-content: space-between; gap: 18px; margin-bottom: 30px; }
    .eyebrow { color: var(--cyan); text-transform: uppercase; letter-spacing: 1.8px; font-size: 10px; font-weight: 800; }
    h1 { margin: 4px 0 0; font-size: clamp(25px, 3vw, 35px); letter-spacing: -1.3px; line-height: 1.15; }
    .subheading { color: var(--muted); margin: 8px 0 0; font-size: 13px; }
    .top-actions { display: flex; gap: 10px; align-items: center; }
    .date-chip { border: 1px solid var(--line); background: rgba(17,24,42,.8); padding: 10px 13px; border-radius: 12px; font-size: 12px; color: #c4cee4; white-space: nowrap; }
    .avatar { display: grid; place-items: center; width: 40px; height: 40px; border-radius: 13px; background: linear-gradient(140deg, #263b61, #6551a3); font-weight: 800; border: 1px solid rgba(255,255,255,.12); }
    .btn {
      border: 1px solid transparent; border-radius: 12px; padding: 11px 15px; color: #08111d; font-weight: 800;
      background: linear-gradient(110deg, var(--cyan), #8ca0ff); box-shadow: 0 8px 25px rgba(94,231,245,.12);
      transition: transform .18s, box-shadow .18s;
    }
    .btn:hover { transform: translateY(-2px); box-shadow: 0 10px 30px rgba(94,231,245,.2); }
    .btn.secondary { color: var(--text); background: #182239; border-color: var(--line); box-shadow: none; }
    .btn.small { padding: 8px 11px; font-size: 12px; }
    .hero {
      position: relative; overflow: hidden; border: 1px solid rgba(132,157,255,.2); border-radius: 24px;
      padding: clamp(22px, 3vw, 34px); margin-bottom: 20px;
      background: linear-gradient(110deg, rgba(30,46,83,.96), rgba(20,31,55,.93) 55%, rgba(15,35,51,.96));
      box-shadow: var(--shadow);
    }
    .hero:before { content: ""; position: absolute; width: 300px; height: 300px; border-radius: 50%; right: 5%; top: -190px; border: 1px solid rgba(94,231,245,.2); box-shadow: 0 0 0 30px rgba(94,231,245,.025), 0 0 0 65px rgba(94,231,245,.02); }
    .hero-content { position: relative; display: flex; justify-content: space-between; align-items: center; gap: 25px; }
    .hero h2 { font-size: clamp(20px, 2.5vw, 28px); letter-spacing: -.8px; margin: 7px 0; }
    .hero p { margin: 0; color: #aebbd7; max-width: 560px; font-size: 13px; }
    .hero .btn { margin-top: 19px; }
    .hero-orbit { width: 125px; height: 125px; flex: 0 0 auto; border-radius: 50%; display: grid; place-items: center; border: 1px solid rgba(94,231,245,.4); box-shadow: 0 0 35px rgba(94,231,245,.1), inset 0 0 30px rgba(94,231,245,.08); color: var(--cyan); font-size: 42px; position: relative; }
    .hero-orbit:before,.hero-orbit:after { content:""; position:absolute; border:1px solid rgba(178,138,255,.32); border-radius:50%; inset:12px; transform:rotate(45deg) scaleX(1.3); }
    .hero-orbit:after { transform:rotate(-45deg) scaleX(1.3); border-color:rgba(94,231,245,.28); }
    .stats { display: grid; grid-template-columns: repeat(4, minmax(0,1fr)); gap: 14px; margin-bottom: 24px; }
    .stat-card,.panel { background: var(--panel); border: 1px solid var(--line); border-radius: var(--radius); box-shadow: var(--shadow); }
    .stat-card { padding: 18px 19px; transition: border-color .2s, transform .2s; }
    .stat-card:hover { border-color: rgba(94,231,245,.28); transform: translateY(-3px); }
    .stat-top { display: flex; align-items: center; justify-content: space-between; color: var(--muted); font-size: 12px; }
    .stat-icon { width: 34px; height: 34px; display: grid; place-items: center; border-radius: 11px; background: rgba(94,231,245,.1); color: var(--cyan); font-size: 16px; }
    .stat-number { font-size: 29px; font-weight: 800; letter-spacing: -1px; margin-top: 12px; }
    .stat-note { color: #7785a4; font-size: 11px; margin-top: 2px; }
    .layout { display: grid; grid-template-columns: minmax(0,1.5fr) minmax(285px,.85fr); gap: 20px; align-items: start; }
    .panel { padding: 21px; min-width: 0; }
    .panel-head { display: flex; align-items: center; justify-content: space-between; gap: 14px; margin-bottom: 18px; }
    .panel-head h3 { margin: 0; font-size: 16px; letter-spacing: -.3px; }
    .panel-head p { margin: 4px 0 0; font-size: 11px; color: var(--muted); }
    .text-link { border: 0; color: var(--cyan); background: transparent; font-size: 12px; font-weight: 700; }
    .form-grid { display: grid; grid-template-columns: repeat(2,minmax(0,1fr)); gap: 14px; }
    .field { display: grid; gap: 7px; }
    .field.full { grid-column: 1 / -1; }
    label { font-size: 11px; color: #b3bfd8; font-weight: 700; }
    input, select, textarea {
      width: 100%; min-width: 0; color: var(--text); background: #0c1323; border: 1px solid #27334c;
      border-radius: 11px; padding: 11px 12px; outline: none; transition: border .2s, box-shadow .2s;
    }
    input::placeholder, textarea::placeholder { color: #65718e; }
    input:focus,select:focus,textarea:focus { border-color: rgba(94,231,245,.7); box-shadow: 0 0 0 3px rgba(94,231,245,.08); }
    select option { background: #0c1323; }
    textarea { resize: vertical; min-height: 76px; }
    .form-foot { display: flex; align-items: center; justify-content: space-between; gap: 12px; margin-top: 17px; }
    .hint { font-size: 11px; color: var(--muted); }
    .resource-list { display: grid; gap: 10px; }
    .resource {
      display: flex; align-items: center; gap: 12px; padding: 13px; border: 1px solid var(--line);
      border-radius: 14px; background: rgba(23,33,54,.65); transition: .2s;
    }
    .resource:hover { border-color: rgba(94,231,245,.28); background: rgba(30,44,69,.8); }
    .resource-symbol { width: 42px; height: 42px; flex: 0 0 auto; display: grid; place-items: center; border-radius: 13px; font-size: 19px; background: rgba(123,140,255,.12); }
    .resource-info { flex: 1; min-width: 0; }
    .resource-info strong { font-size: 12px; display: block; }
    .resource-info span { color: var(--muted); font-size: 10px; display: block; margin-top: 3px; }
    .availability { font-size: 10px; color: var(--green); white-space: nowrap; }
    .resource-pick { accent-color: var(--cyan); width: 15px; }
    .lower { margin-top: 20px; }
    .filters { display: flex; gap: 8px; flex-wrap: wrap; }
    .filter-btn { border: 1px solid var(--line); border-radius: 9px; background: #111a2b; color: var(--muted); padding: 7px 10px; font-size: 11px; }
    .filter-btn.active { color: var(--cyan); border-color: rgba(94,231,245,.3); background: rgba(94,231,245,.08); }
    .booking-list { display: grid; gap: 10px; }
    .booking {
      display: grid; grid-template-columns: 54px minmax(0,1fr) auto; gap: 13px; align-items: center;
      border: 1px solid var(--line); border-radius: 14px; padding: 13px; background: rgba(13,20,35,.65);
    }
    .date-block { background: #19243b; border-radius: 11px; padding: 8px 4px; text-align: center; }
    .date-block .day { display: block; font-size: 20px; font-weight: 800; line-height: 1.1; }
    .date-block .month { color: var(--cyan); font-size: 9px; text-transform: uppercase; letter-spacing: 1px; }
    .booking-main { min-width: 0; }
    .booking-main strong { font-size: 12px; display: block; overflow-wrap: anywhere; }
    .booking-main p { margin: 3px 0 0; color: var(--muted); font-size: 10px; }
    .booking-meta { display: flex; align-items: center; gap: 7px; flex-wrap: wrap; margin-top: 6px; }
    .pill { border-radius: 20px; font-size: 9px; padding: 4px 8px; background: rgba(117,230,177,.1); color: var(--green); }
    .pill.pending { color: var(--amber); background: rgba(255,210,122,.1); }
    .pill.cancelled { color: var(--red); background: rgba(255,143,154,.1); }
    .booking-actions { display: flex; align-items: center; gap: 6px; }
    .icon-btn { width: 30px; height: 30px; display: grid; place-items: center; border: 1px solid var(--line); border-radius: 9px; color: var(--muted); background: #151f33; }
    .icon-btn:hover { color: var(--cyan); border-color: rgba(94,231,245,.35); }
    .empty { padding: 25px 10px; text-align: center; color: var(--muted); font-size: 12px; }
    .empty .empty-icon { font-size: 25px; margin-bottom: 6px; }
    .side-stack { display: grid; gap: 20px; }
    .mini-calendar { display: grid; grid-template-columns: repeat(7,1fr); gap: 5px; text-align: center; }
    .weekday { color: #72809e; font-size: 9px; padding-bottom: 4px; }
    .cal-day { border: 1px solid transparent; color: #c1cce2; border-radius: 9px; min-height: 32px; display: grid; place-items: center; font-size: 11px; background: transparent; }
    .cal-day:hover { border-color: var(--line); }
    .cal-day.today { color: #07111c; background: var(--cyan); font-weight: 900; }
    .cal-day.has-booking { border-color: rgba(178,138,255,.45); position: relative; }
    .cal-day.has-booking:after { content:""; width:4px; height:4px; border-radius:50%; background:var(--violet); position:absolute; bottom:3px; }
    .calendar-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 13px; }
    .calendar-top strong { font-size: 13px; }
    .calendar-top button { background: #172138; border: 1px solid var(--line); color: var(--text); width: 27px; height: 27px; border-radius: 8px; }
    .activity { display: grid; gap: 15px; }
    .activity-item { display: flex; gap: 11px; align-items: flex-start; }
    .activity-dot { width: 9px; height: 9px; margin-top: 5px; border-radius: 50%; background: var(--cyan); box-shadow: 0 0 12px rgba(94,231,245,.35); flex: 0 0 auto; }
    .activity-item strong { display: block; font-size: 11px; }
    .activity-item p { margin: 3px 0 0; color: var(--muted); font-size: 10px; }
    footer { text-align: center; color: #5f6c89; font-size: 10px; padding: 28px 0 0; }
    .toast { position: fixed; right: 22px; bottom: 22px; max-width: calc(100% - 44px); padding: 13px 17px; background: #172740; border: 1px solid rgba(94,231,245,.35); color: var(--text); border-radius: 13px; box-shadow: var(--shadow); opacity: 0; transform: translateY(10px); pointer-events: none; transition: .25s; z-index: 20; font-size: 12px; }
    .toast.show { opacity: 1; transform: translateY(0); }
    .mobile-menu { display: none; }
    @media (max-width: 1100px) {
      .app { grid-template-columns: 210px minmax(0,1fr); }
      .stats { grid-template-columns: repeat(2,minmax(0,1fr)); }
      .layout { grid-template-columns: minmax(0,1fr); }
      .side-stack { grid-template-columns: repeat(2,minmax(0,1fr)); }
    }
    @media (max-width: 720px) {
      .app { display: block; }
      .sidebar { padding: 14px 16px; border-right: 0; border-bottom: 1px solid var(--line); gap: 15px; }
      .sidebar .nav-label, .sidebar-bottom { display: none; }
      .brand { padding: 0; }
      .nav { display: flex; overflow-x: auto; padding-bottom: 2px; }
      .nav button { white-space: nowrap; padding: 9px 11px; }
      main { padding: 22px 15px 30px; }
      .topbar { align-items: flex-start; flex-wrap: wrap; margin-bottom: 22px; }
      .top-actions { width: 100%; justify-content: space-between; }
      .hero-content { align-items: flex-start; }
      .hero-orbit { width: 70px; height: 70px; font-size: 27px; }
      .hero-orbit:before,.hero-orbit:after { inset: 7px; }
      .stats { gap: 10px; }
      .stat-card { padding: 14px; }
      .stat-number { font-size: 24px; }
      .panel { padding: 16px; }
      .form-grid { grid-template-columns: 1fr; }
      .field.full { grid-column: auto; }
      .side-stack { grid-template-columns: 1fr; }
      .booking { grid-template-columns: 46px minmax(0,1fr); }
      .booking-actions { grid-column: 2; justify-content: flex-end; }
      .form-foot { align-items: flex-start; flex-direction: column; }
      .form-foot .btn { width: 100%; }
    }
    @media (prefers-reduced-motion: reduce) { *,*::before,*::after { scroll-behavior: auto !important; transition: none !important; } }
  </style>
</head>
<body>
  <div class="app">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-mark">O</div>
        <div><div class="brand-name">orbit<span style="color:var(--cyan)">.</span></div><div class="brand-sub">booking workspace</div></div>
      </div>
      <div>
        <div class="nav-label">WORKSPACE</div>
        <nav class="nav" aria-label="Main navigation">
          <button class="active" data-scroll="overview"><span class="nav-icon">⌂</span> Overview</button>
          <button data-scroll="new-booking"><span class="nav-icon">＋</span> New booking</button>
          <button data-scroll="appointments"><span class="nav-icon">▤</span> Appointments</button>
          <button data-scroll="resources"><span class="nav-icon">▦</span> Resources</button>
        </nav>
      </div>
      <div class="sidebar-bottom">
        <div class="help-card">
          <strong>Built for smoother days.</strong>
          <p>Organise meetings, labs and shared resources in one place.</p>
          <button id="demoHelp">How this demo works ↗</button>
        </div>
      </div>
    </aside>

    <main>
      <header class="topbar" id="overview">
        <div>
          <div class="eyebrow">Workspace / Dashboard</div>
          <h1>Good day, <span id="greetingName">there</span> <span aria-hidden="true">✦</span></h1>
          <p class="subheading">Plan your time. Reserve what you need. Keep everything moving.</p>
        </div>
        <div class="top-actions">
          <div class="date-chip" id="todayLabel">◷ Loading date…</div>
          <div class="avatar" title="Demo profile">TG</div>
          <button class="btn" data-scroll="new-booking">＋ Create booking</button>
        </div>
      </header>

      <section class="hero">
        <div class="hero-content">
          <div>
            <div class="eyebrow">Your day, organised</div>
            <h2>Make room for what matters.</h2>
            <p>Book a mentor meeting, reserve a lab, or plan a focused work session. Your demo bookings are saved locally in this browser.</p>
            <button class="btn" data-scroll="new-booking">Create a reservation <span aria-hidden="true">↗</span></button>
          </div>
          <div class="hero-orbit" aria-hidden="true">✳</div>
        </div>
      </section>

      <section class="stats" aria-label="Booking summary">
        <article class="stat-card"><div class="stat-top"><span>Total bookings</span><span class="stat-icon">▤</span></div><div class="stat-number" id="statTotal">0</div><div class="stat-note">All saved reservations</div></article>
        <article class="stat-card"><div class="stat-top"><span>Upcoming</span><span class="stat-icon" style="color:var(--green);background:rgba(117,230,177,.1)">◷</span></div><div class="stat-number" id="statUpcoming">0</div><div class="stat-note">Not yet completed</div></article>
        <article class="stat-card"><div class="stat-top"><span>Resources</span><span class="stat-icon" style="color:var(--violet);background:rgba(178,138,255,.1)">▦</span></div><div class="stat-number">06</div><div class="stat-note">Spaces and services</div></article>
        <article class="stat-card"><div class="stat-top"><span>Pending</span><span class="stat-icon" style="color:var(--amber);background:rgba(255,210,122,.1)">⌛</span></div><div class="stat-number" id="statPending">0</div><div class="stat-note">Awaiting confirmation</div></article>
      </section>

      <div class="layout">
        <section class="panel" id="new-booking">
          <div class="panel-head"><div><h3>Create a booking</h3><p>Fill in the details to reserve a slot.</p></div><span class="eyebrow">01 / 02</span></div>
          <form id="bookingForm">
            <div class="form-grid">
              <div class="field"><label for="fullName">Full name *</label><input id="fullName" name="fullName" placeholder="e.g. Tanishq Gautam" required maxlength="70"></div>
              <div class="field"><label for="email">Email address *</label><input id="email" name="email" type="email" placeholder="you@example.com" required maxlength="100"></div>
              <div class="field"><label for="provider">Appointment with *</label><select id="provider" name="provider" required><option value="">Choose a person</option><option>Project Mentor</option><option>Faculty Advisor</option><option>Lab Coordinator</option><option>Career Counsellor</option><option>Team Meeting</option></select></div>
              <div class="field"><label for="resource">Resource *</label><select id="resource" name="resource" required><option value="">Choose a resource</option><option>Innovation Lab</option><option>Meeting Room A</option><option>Computer Lab 02</option><option>Conference Room</option><option>Online Meeting Room</option><option>Study Pod 04</option></select></div>
              <div class="field"><label for="date">Date *</label><input id="date" name="date" type="date" required></div>
              <div class="field"><label for="time">Time *</label><select id="time" name="time" required><option value="">Choose a time</option><option>09:00 AM</option><option>10:00 AM</option><option>11:00 AM</option><option>12:00 PM</option><option>01:00 PM</option><option>02:00 PM</option><option>03:00 PM</option><option>04:00 PM</option><option>05:00 PM</option></select></div>
              <div class="field full"><label for="notes">Purpose / notes <span style="color:var(--muted);font-weight:400">(optional)</span></label><textarea id="notes" name="notes" placeholder="What is this booking for? Add any helpful context… " maxlength="400"></textarea></div>
            </div>
            <div class="form-foot"><span class="hint">✦ Demo mode — no email or server connection required.</span><button class="btn" type="submit">Save reservation ↗</button></div>
          </form>
        </section>

        <div class="side-stack">
          <section class="panel" id="resources">
            <div class="panel-head"><div><h3>Popular resources</h3><p>Pick a space for your next session.</p></div><span class="eyebrow">06 SPACES</span></div>
            <div class="resource-list">
              <div class="resource"><div class="resource-symbol">⌘</div><div class="resource-info"><strong>Innovation Lab</strong><span>Projects · 12 seats</span></div><span class="availability">Available</span><input class="resource-pick" type="radio" name="quickResource" value="Innovation Lab" aria-label="Select Innovation Lab"></div>
              <div class="resource"><div class="resource-symbol" style="background:rgba(117,230,177,.1)">▣</div><div class="resource-info"><strong>Meeting Room A</strong><span>Teamwork · 6 seats</span></div><span class="availability">Available</span><input class="resource-pick" type="radio" name="quickResource" value="Meeting Room A" aria-label="Select Meeting Room A"></div>
              <div class="resource"><div class="resource-symbol" style="background:rgba(255,210,122,.1)">⌨</div><div class="resource-info"><strong>Computer Lab 02</strong><span>Computing · 30 seats</span></div><span class="availability">Available</span><input class="resource-pick" type="radio" name="quickResource" value="Computer Lab 02" aria-label="Select Computer Lab 02"></div>
              <div class="resource"><div class="resource-symbol" style="background:rgba(178,138,255,.1)">◉</div><div class="resource-info"><strong>Conference Room</strong><span>Presentations · 10 seats</span></div><span class="availability">Available</span><input class="resource-pick" type="radio" name="quickResource" value="Conference Room" aria-label="Select Conference Room"></div>
              <div class="resource"><div class="resource-symbol" style="background:rgba(94,231,245,.1)">⌁</div><div class="resource-info"><strong>Online Meeting Room</strong><span>Remote · Video call</span></div><span class="availability">Available</span><input class="resource-pick" type="radio" name="quickResource" value="Online Meeting Room" aria-label="Select Online Meeting Room"></div>
            </div>
          </section>

          <section class="panel">
            <div class="panel-head"><div><h3>Booking calendar</h3><p>Navigate dates at a glance.</p></div></div>
            <div class="calendar-top"><button id="prevMonth" aria-label="Previous month">‹</button><strong id="calendarMonth">Month Year</strong><button id="nextMonth" aria-label="Next month">›</button></div>
            <div class="mini-calendar" id="miniCalendar" aria-label="Monthly calendar"></div>
          </section>
        </div>
      </div>

      <section class="panel lower" id="appointments">
        <div class="panel-head" style="flex-wrap:wrap">
          <div><h3>Your appointments</h3><p>Review, filter, or remove your saved reservations.</p></div>
          <div class="filters" role="group" aria-label="Filter bookings">
            <button class="filter-btn active" data-filter="all">All bookings</button>
            <button class="filter-btn" data-filter="upcoming">Upcoming</button>
            <button class="filter-btn" data-filter="pending">Pending</button>
          </div>
        </div>
        <div class="booking-list" id="bookingList"></div>
      </section>

      <section class="panel lower">
        <div class="panel-head"><div><h3>Recent activity</h3><p>A lightweight history of actions in this browser.</p></div><span class="eyebrow">LIVE LOG</span></div>
        <div class="activity" id="activityList">
          <div class="activity-item"><span class="activity-dot"></span><div><strong>Workspace is ready</strong><p>Create a booking to see your activity here.</p></div></div>
        </div>
      </section>
      <footer>ORBIT BOOKING WORKSPACE · STUDENT PROJECT DEMO · DATA STAYS IN THIS BROWSER</footer>
    </main>
  </div>
  <div class="toast" id="toast" role="status" aria-live="polite"></div>

  <script>
    // ORBIT DEMO: booking records are stored in localStorage on this device only.
    const STORAGE_KEY = "orbitBookings_v1";
    let bookings = loadBookings();
    let currentFilter = "all";
    let calendarDate = new Date();
    let activities = [];

    const $ = (id) => document.getElementById(id);
    const todayISO = () => {
      const d = new Date();
      return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,"0")}-${String(d.getDate()).padStart(2,"0")}`;
    };
    function loadBookings() {
      try {
        const saved = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
        return Array.isArray(saved) ? saved : [];
      } catch (error) {
        return [];
      }
    }
    function saveBookings() {
      try { localStorage.setItem(STORAGE_KEY, JSON.stringify(bookings)); }
      catch (error) { showToast("Browser storage is unavailable. Your booking may not persist after refresh."); }
    }
    function escapeHTML(value) {
      return String(value ?? "").replace(/[&<>"']/g, char => ({
        "&":"&amp;", "<":"&lt;", ">":"&gt;", '"':"&quot;", "'":"&#39;"
      })[char]);
    }
    function showToast(message) {
      const toast = $("toast");
      toast.textContent = message;
      toast.classList.add("show");
      clearTimeout(showToast.timer);
      showToast.timer = setTimeout(() => toast.classList.remove("show"), 3000);
    }
    function logActivity(title, detail) {
      activities.unshift({title, detail, time: new Date()});
      activities = activities.slice(0, 5);
      renderActivity();
    }
    function renderActivity() {
      $("activityList").innerHTML = activities.length ? activities.map(item =>
        `<div class="activity-item"><span class="activity-dot"></span><div><strong>${escapeHTML(item.title)}</strong><p>${escapeHTML(item.detail)} · ${item.time.toLocaleTimeString([], {hour:"2-digit",minute:"2-digit"})}</p></div></div>`
      ).join("") : `<div class="activity-item"><span class="activity-dot"></span><div><strong>Workspace is ready</strong><p>Create a booking to see your activity here.</p></div></div>`;
    }
    function dateObj(dateString) {
      const [y,m,d] = dateString.split("-").map(Number);
      return new Date(y, m-1, d);
    }
    function isUpcoming(booking) { return booking.date >= todayISO() && booking.status !== "Cancelled"; }
    function renderStats() {
      $("statTotal").textContent = bookings.length;
      $("statUpcoming").textContent = bookings.filter(isUpcoming).length;
      $("statPending").textContent = bookings.filter(b => b.status === "Pending").length;
    }
    function renderBookings() {
      const list = $("bookingList");
      let shown = bookings.slice().sort((a,b) => (a.date + a.time).localeCompare(b.date + b.time));
      if (currentFilter === "upcoming") shown = shown.filter(isUpcoming);
      if (currentFilter === "pending") shown = shown.filter(b => b.status === "Pending");
      if (!shown.length) {
        list.innerHTML = `<div class="empty"><div class="empty-icon">✧</div><strong>Nothing here just yet</strong><div style="margin-top:5px">Create a reservation above to populate your dashboard.</div></div>`;
        return;
      }
      list.innerHTML = shown.map(b => {
        const d = dateObj(b.date);
        const statusClass = b.status === "Pending" ? "pending" : b.status === "Cancelled" ? "cancelled" : "";
        return `<article class="booking">
          <div class="date-block"><span class="day">${d.getDate()}</span><span class="month">${d.toLocaleString("en",{month:"short"})}</span></div>
          <div class="booking-main">
            <strong>${escapeHTML(b.provider)} · ${escapeHTML(b.resource)}</strong>
            <p>${escapeHTML(b.fullName)} · ${escapeHTML(b.time)} · ${escapeHTML(b.email)}</p>
            ${b.notes ? `<p>“${escapeHTML(b.notes)}”</p>` : ""}
            <div class="booking-meta"><span class="pill ${statusClass}">${escapeHTML(b.status)}</span><span class="hint">${d.toLocaleDateString("en",{weekday:"short",month:"short",day:"numeric",year:"numeric"})}</span></div>
          </div>
          <div class="booking-actions">
            ${b.status !== "Cancelled" ? `<button class="icon-btn" title="Mark as confirmed" aria-label="Confirm booking" data-confirm="${b.id}">✓</button>` : ""}
            <button class="icon-btn" title="Delete booking" aria-label="Delete booking" data-delete="${b.id}">×</button>
          </div>
        </article>`;
      }).join("");
    }
    function renderCalendar() {
      const year = calendarDate.getFullYear(), month = calendarDate.getMonth();
      $("calendarMonth").textContent = calendarDate.toLocaleString("en",{month:"long",year:"numeric"});
      const firstDay = new Date(year,month,1).getDay();
      const days = new Date(year,month+1,0).getDate();
      const weekdays = ["Su","Mo","Tu","We","Th","Fr","Sa"];
      const bookedDates = new Set(bookings.filter(b => b.status !== "Cancelled").map(b => b.date));
      let html = weekdays.map(day => `<div class="weekday">${day}</div>`).join("");
      for(let i=0;i<firstDay;i++) html += `<div class="cal-day" aria-hidden="true"></div>`;
      for(let day=1;day<=days;day++) {
        const iso = `${year}-${String(month+1).padStart(2,"0")}-${String(day).padStart(2,"0")}`;
        const classes = ["cal-day"];
        if(iso === todayISO()) classes.push("today");
        if(bookedDates.has(iso)) classes.push("has-booking");
        html += `<button class="${classes.join(" ")}" data-calendar-date="${iso}" aria-label="${iso}${bookedDates.has(iso) ? ", has booking" : ""}">${day}</button>`;
      }
      $("miniCalendar").innerHTML = html;
    }
    function refresh() { renderStats(); renderBookings(); renderCalendar(); }
    $("bookingForm").addEventListener("submit", event => {
      event.preventDefault();
      const form = new FormData(event.currentTarget);
      const booking = {
        id: (crypto && crypto.randomUUID) ? crypto.randomUUID() : String(Date.now()) + Math.random().toString(16).slice(2),
        fullName: String(form.get("fullName") || "").trim(),
        email: String(form.get("email") || "").trim(),
        provider: String(form.get("provider") || ""),
        resource: String(form.get("resource") || ""),
        date: String(form.get("date") || ""),
        time: String(form.get("time") || ""),
        notes: String(form.get("notes") || "").trim(),
        status: "Pending"
      };
      if (!booking.fullName || !booking.email || !booking.provider || !booking.resource || !booking.date || !booking.time) {
        showToast("Please complete all required fields.");
        return;
      }
      if (booking.date < todayISO()) { showToast("Please choose today or a future date."); $("date").focus(); return; }
      const duplicate = bookings.some(b => b.date === booking.date && b.time === booking.time && b.resource === booking.resource && b.status !== "Cancelled");
      if (duplicate) { showToast("That resource already has a booking at this date and time in this demo."); return; }
      bookings.push(booking);
      saveBookings();
      refresh();
      logActivity("New reservation created", `${booking.resource} · ${booking.date} at ${booking.time}`);
      event.currentTarget.reset();
      $("date").min = todayISO();
      currentFilter = "all";
      document.querySelectorAll("[data-filter]").forEach(btn => btn.classList.toggle("active", btn.dataset.filter === "all"));
      renderBookings();
      showToast("Reservation saved successfully.");
    });
    document.addEventListener("click", event => {
      const scrollButton = event.target.closest("[data-scroll]");
      if (scrollButton) {
        const target = $(scrollButton.dataset.scroll);
        if (target) target.scrollIntoView({behavior:"smooth",block:"start"});
      }
      const deleteButton = event.target.closest("[data-delete]");
      if (deleteButton) {
        const booking = bookings.find(b => b.id === deleteButton.dataset.delete);
        if (!booking) return;
        if (!confirm(`Delete the booking for ${booking.resource} on ${booking.date}?`)) return;
        bookings = bookings.filter(b => b.id !== booking.id);
        saveBookings(); refresh(); logActivity("Reservation deleted", `${booking.resource} · ${booking.date}`);
        showToast("Booking deleted.");
      }
      const confirmButton = event.target.closest("[data-confirm]");
      if (confirmButton) {
        const booking = bookings.find(b => b.id === confirmButton.dataset.confirm);
        if (!booking) return;
        booking.status = "Confirmed";
        saveBookings(); refresh(); logActivity("Reservation confirmed", `${booking.resource} · ${booking.date}`);
        showToast("Booking marked as confirmed.");
      }
      const filterButton = event.target.closest("[data-filter]");
      if (filterButton) {
        currentFilter = filterButton.dataset.filter;
        document.querySelectorAll("[data-filter]").forEach(btn => btn.classList.toggle("active", btn === filterButton));
        renderBookings();
      }
      const calendarButton = event.target.closest("[data-calendar-date]");
      if (calendarButton) {
        $("date").value = calendarButton.dataset.calendarDate;
        $("date").focus();
        $("new-booking").scrollIntoView({behavior:"smooth",block:"start"});
        showToast(`Date selected: ${calendarButton.dataset.calendarDate}`);
      }
    });
    document.querySelectorAll("[name='quickResource']").forEach(radio => {
      radio.addEventListener("change", () => {
        $("resource").value = radio.value;
        $("new-booking").scrollIntoView({behavior:"smooth",block:"start"});
        showToast(`${radio.value} selected for your booking.`);
      });
    });
    document.querySelectorAll(".nav button").forEach(button => {
      button.addEventListener("click", () => {
        document.querySelectorAll(".nav button").forEach(item => item.classList.toggle("active", item === button));
      });
    });
    $("prevMonth").addEventListener("click", () => { calendarDate = new Date(calendarDate.getFullYear(),calendarDate.getMonth()-1,1); renderCalendar(); });
    $("nextMonth").addEventListener("click", () => { calendarDate = new Date(calendarDate.getFullYear(),calendarDate.getMonth()+1,1); renderCalendar(); });
    $("demoHelp").addEventListener("click", () => showToast("Create a booking, confirm or delete it, and filter your list. Data stays in this browser."));
    $("todayLabel").textContent = "◷ " + new Date().toLocaleDateString("en",{weekday:"short",month:"short",day:"numeric",year:"numeric"});
    $("date").min = todayISO();
    $("date").value = todayISO();
    refresh();
  </script>
</body>
</html>
