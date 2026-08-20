<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vishal R — Data Analyst</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700;800&family=IBM+Plex+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#050708;
    --bg-panel:#080b0d;
    --red:#22d3ee;
    --red-bright:#7fe8fb;
    --red-dim:#0e5a66;
    --red-glow: rgba(34,211,238,0.5);
    --text:#c8e2e6;
    --text-dim:#5f8489;
    --line:#123037;
    --mono-d:'Space Grotesk', sans-serif;
    --mono-t:'IBM Plex Mono', monospace;
  }

  *{margin:0;padding:0;box-sizing:border-box;}

  html{scroll-behavior:smooth;}

  body{
    background:var(--bg);
    color:var(--text);
    font-family:var(--mono-t);
    overflow-x:hidden;
    position:relative;
  }

  /* ---------- CRT scanline + flicker overlay ---------- */
  .crt-overlay{
    position:fixed; inset:0;
    pointer-events:none;
    z-index:999;
    background:repeating-linear-gradient(
      to bottom,
      rgba(255,255,255,0.018) 0px,
      rgba(255,255,255,0.018) 1px,
      transparent 1px,
      transparent 3px
    );
    mix-blend-mode:overlay;
  }
  .vignette{
    position:fixed; inset:0;
    pointer-events:none;
    z-index:998;
    box-shadow: inset 0 0 200px rgba(0,0,0,0.85);
  }

  /* ---------- data-rain canvas ---------- */
  .chart-bg{
    position:fixed; inset:0;
    width:100%; height:100%;
    z-index:1;
    pointer-events:none;
    opacity:0.9;
  }

  a{color:var(--red-bright); text-decoration:none;}
  a:hover{text-decoration:underline;}

  section{
    position:relative;
    z-index:2;
    max-width:980px;
    margin:0 auto;
    padding:90px 28px;
    border-bottom:1px dashed var(--line);
  }
  section:last-of-type{border-bottom:none;}

  .prompt-label{
    display:inline-flex;
    align-items:center;
    gap:8px;
    font-family:var(--mono-d);
    font-size:13px;
    letter-spacing:2px;
    color:var(--red-dim);
    text-transform:uppercase;
    margin-bottom:22px;
  }
  .prompt-label::before{content:"$";color:var(--red);font-weight:700;}

  h1,h2,h3{
    font-family:var(--mono-d);
    color:var(--red);
    text-shadow: 0 0 12px var(--red-glow);
    font-weight:800;
  }

  /* ---------- NAV ---------- */
  header{
    position:fixed;
    top:0; left:0; right:0;
    z-index:100;
    display:flex;
    justify-content:space-between;
    align-items:center;
    padding:16px 32px;
    background:rgba(6,5,5,0.85);
    backdrop-filter:blur(6px);
    border-bottom:1px solid var(--line);
    font-family:var(--mono-d);
  }
  header .logo{
    color:var(--red-bright);
    font-weight:800;
    letter-spacing:1px;
    font-size:15px;
  }
  header .logo .cursor{
    display:inline-block;
    width:8px; height:14px;
    background:var(--red);
    margin-left:4px;
    animation:blink 1s steps(1) infinite;
    vertical-align:middle;
  }
  nav ul{
    list-style:none;
    display:flex;
    gap:26px;
  }
  nav a{
    color:var(--text-dim);
    font-size:12px;
    letter-spacing:1.5px;
    text-transform:uppercase;
  }
  nav a:hover{color:var(--red-bright); text-decoration:none; text-shadow:0 0 8px var(--red-glow);}

  @keyframes blink{50%{opacity:0;}}

  /* ---------- HERO ---------- */
  .hero{
    min-height:100vh;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:flex-start;
    padding:0 32px;
    max-width:980px;
    margin:0 auto;
    position:relative;
    z-index:2;
    border-bottom:none;
  }
  .hero .tag{
    font-family:var(--mono-d);
    font-size:13px;
    letter-spacing:3px;
    color:var(--red-dim);
    margin-bottom:18px;
  }
  .hero h1{
    font-size:clamp(38px, 8vw, 84px);
    line-height:1.02;
    letter-spacing:-1px;
  }
  .hero .role-line{
    font-family:var(--mono-t);
    font-size:clamp(16px,3vw,24px);
    color:var(--text);
    margin-top:18px;
    min-height:32px;
  }
  .hero .role-line .caret{
    display:inline-block;
    width:10px; height:22px;
    background:var(--red);
    margin-left:2px;
    vertical-align:middle;
    animation:blink 0.9s steps(1) infinite;
  }
  .hero p.desc{
    margin-top:26px;
    max-width:600px;
    color:var(--text-dim);
    line-height:1.7;
    font-size:15px;
  }
  .hero .cta-row{
    margin-top:36px;
    display:flex;
    gap:16px;
    flex-wrap:wrap;
  }
  .btn{
    font-family:var(--mono-d);
    font-size:13px;
    letter-spacing:1px;
    padding:13px 24px;
    border:1px solid var(--red);
    color:var(--red-bright);
    background:transparent;
    cursor:pointer;
    transition:all .25s ease;
    text-transform:uppercase;
  }
  .btn:hover{
    background:var(--red);
    color:#0a0505;
    box-shadow:0 0 22px var(--red-glow);
  }
  .btn.solid{
    background:var(--red-dim);
    border-color:var(--red-dim);
    color:var(--text);
  }
  .btn.solid:hover{background:var(--red); border-color:var(--red); color:#0a0505;}

  .scroll-hint{
    position:absolute;
    bottom:34px; left:32px;
    font-size:11px;
    color:var(--text-dim);
    letter-spacing:2px;
    display:flex;
    align-items:center;
    gap:8px;
  }
  .scroll-hint .bar{
    width:1px; height:30px;
    background:linear-gradient(to bottom, var(--red), transparent);
    animation:scrollpulse 1.6s ease-in-out infinite;
  }
  @keyframes scrollpulse{
    0%,100%{opacity:.3; transform:scaleY(0.6);}
    50%{opacity:1; transform:scaleY(1);}
  }

  /* ---------- reveal on scroll ---------- */
  .reveal{
    opacity:0;
    transform:translateY(24px);
    transition:opacity .7s ease, transform .7s ease;
  }
  .reveal.in{
    opacity:1;
    transform:translateY(0);
  }

  /* ---------- ABOUT ---------- */
  .about-grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:40px;
    margin-top:10px;
  }
  .about-grid p{
    color:var(--text);
    line-height:1.85;
    font-size:15px;
  }
  .about-grid .stat-block{
    border-left:2px solid var(--red-dim);
    padding-left:18px;
    display:flex;
    flex-direction:column;
    gap:18px;
  }
  .stat{
    display:flex;
    flex-direction:column;
  }
  .stat .n{
    font-family:var(--mono-d);
    font-size:26px;
    color:var(--red-bright);
    font-weight:800;
  }
  .stat .l{
    font-size:11px;
    color:var(--text-dim);
    letter-spacing:2px;
    text-transform:uppercase;
    margin-top:2px;
  }

  @media (max-width:720px){
    .about-grid{grid-template-columns:1fr;}
  }

  /* ---------- SKILLS ---------- */
  .skill-terminal{
    background:var(--bg-panel);
    border:1px solid var(--line);
    padding:24px;
    font-family:var(--mono-d);
    font-size:13px;
    box-shadow:0 0 30px rgba(255,34,34,0.06) inset;
  }
  .skill-terminal .tbar{
    display:flex; gap:6px;
    margin-bottom:16px;
  }
  .skill-terminal .tbar span{
    width:10px;height:10px;border-radius:50%;
    background:var(--red-dim);
  }
  .skill-groups{
    display:grid;
    grid-template-columns:repeat(auto-fit, minmax(220px,1fr));
    gap:22px;
    margin-top:28px;
  }
  .skill-cat h4{
    color:var(--red-bright);
    font-family:var(--mono-d);
    font-size:13px;
    letter-spacing:1.5px;
    text-transform:uppercase;
    margin-bottom:12px;
    border-bottom:1px solid var(--line);
    padding-bottom:8px;
  }
  .skill-cat .bar-row{
    margin-bottom:12px;
  }
  .skill-cat .bar-row .lbl{
    display:flex;
    justify-content:space-between;
    font-size:12px;
    color:var(--text);
    margin-bottom:5px;
  }
  .bar-track{
    width:100%; height:6px;
    background:#1a0808;
    overflow:hidden;
  }
  .bar-fill{
    height:100%;
    background:linear-gradient(90deg, var(--red-dim), var(--red-bright));
    width:0%;
    transition:width 1.4s cubic-bezier(.2,.8,.2,1);
    box-shadow:0 0 10px var(--red-glow);
  }
  .learning-tag{
    display:inline-block;
    margin:4px 6px 0 0;
    padding:5px 10px;
    font-size:11px;
    border:1px dashed var(--red-dim);
    color:var(--text-dim);
    letter-spacing:1px;
  }

  /* ---------- PROJECTS ---------- */
  .projects-grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(260px,1fr));
    gap:22px;
    margin-top:10px;
  }
  .project-card{
    border:1px solid var(--line);
    background:var(--bg-panel);
    padding:22px;
    position:relative;
    overflow:hidden;
    transition:transform .3s ease, border-color .3s ease;
  }
  .project-card::before{
    content:"";
    position:absolute; top:0; left:0; right:0; height:2px;
    background:linear-gradient(90deg, transparent, var(--red), transparent);
    transform:scaleX(0);
    transition:transform .4s ease;
  }
  .project-card:hover{
    transform:translateY(-6px);
    border-color:var(--red-dim);
  }
  .project-card:hover::before{transform:scaleX(1);}
  .project-card .idx{
    font-family:var(--mono-d);
    color:var(--red-dim);
    font-size:12px;
    letter-spacing:2px;
  }
  .project-card h3{
    font-size:18px;
    margin:10px 0 10px;
  }
  .project-card p{
    color:var(--text-dim);
    font-size:13px;
    line-height:1.6;
    margin-bottom:14px;
  }
  .project-card .stack{
    display:flex;
    flex-wrap:wrap;
    gap:6px;
  }
  .project-card .stack span{
    font-size:10px;
    padding:3px 8px;
    border:1px solid var(--red-dim);
    color:var(--red-bright);
    letter-spacing:1px;
  }
  /* ---------- GOAL / TIMELINE ---------- */
  .goal-line{
    display:flex;
    align-items:center;
    gap:16px;
    margin:22px 0;
    font-family:var(--mono-d);
    font-size:14px;
  }
  .goal-line .dot{
    width:10px;height:10px;
    background:var(--red);
    box-shadow:0 0 10px var(--red-glow);
    flex-shrink:0;
  }
  .goal-line.dim .dot{background:var(--red-dim); box-shadow:none;}
  .goal-line span.txt{color:var(--text);}
  .goal-line.dim span.txt{color:var(--text-dim);}

  /* ---------- CONTACT ---------- */
  .contact-box{
    text-align:center;
    padding:60px 20px;
  }
  .contact-box .big-mail{
    font-family:var(--mono-d);
    font-size:clamp(18px,4vw,32px);
    color:var(--red-bright);
    text-shadow:0 0 18px var(--red-glow);
    display:inline-block;
    padding:18px 26px;
    border:1px solid var(--red-dim);
    margin-top:20px;
    word-break:break-all;
    transition:all .3s ease;
  }
  .contact-box .big-mail:hover{
    background:var(--red);
    color:#0a0505;
    text-shadow:none;
    box-shadow:0 0 28px var(--red-glow);
    text-decoration:none;
  }
  .contact-box p{
    color:var(--text-dim);
    max-width:500px;
    margin:0 auto;
    line-height:1.7;
    font-size:14px;
  }

  footer{
    text-align:center;
    padding:30px;
    font-family:var(--mono-d);
    font-size:11px;
    color:var(--text-dim);
    letter-spacing:1px;
    position:relative;
    z-index:2;
  }
  footer .r{color:var(--red-dim);}
</style>
</head>
<body>

<!-- decorative data-analytics background: grid, line chart, bars, scatter -->
<svg class="chart-bg" viewBox="0 0 1600 1000" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <pattern id="grid" width="80" height="80" patternUnits="userSpaceOnUse">
      <path d="M 80 0 L 0 0 0 80" fill="none" stroke="var(--red-dim)" stroke-width="0.6" opacity="0.35"/>
    </pattern>
  </defs>
  <rect width="1600" height="1000" fill="url(#grid)"/>

  <!-- bar chart cluster, upper left -->
  <g opacity="0.5" stroke="var(--red)" stroke-width="2" fill="none">
    <rect x="90"  y="220" width="26" height="90"  fill="var(--red)" opacity="0.25"/>
    <rect x="130" y="170" width="26" height="140" fill="var(--red)" opacity="0.35"/>
    <rect x="170" y="120" width="26" height="190" fill="var(--red)" opacity="0.5"/>
    <rect x="210" y="200" width="26" height="110" fill="var(--red)" opacity="0.3"/>
    <line x1="80" y1="312" x2="250" y2="312"/>
  </g>

  <!-- line chart, right side -->
  <g opacity="0.55" fill="none" stroke="var(--red)" stroke-width="2.5">
    <polyline points="1180,760 1240,700 1300,730 1360,640 1420,660 1480,560 1540,600"/>
    <circle cx="1180" cy="760" r="4" fill="var(--red)"/>
    <circle cx="1300" cy="730" r="4" fill="var(--red)"/>
    <circle cx="1420" cy="660" r="4" fill="var(--red)"/>
    <circle cx="1540" cy="600" r="4" fill="var(--red)"/>
  </g>

  <!-- scatter plot, lower left -->
  <g fill="var(--red)" opacity="0.45">
    <circle cx="140" cy="700" r="4"/>
    <circle cx="190" cy="740" r="3"/>
    <circle cx="220" cy="690" r="5"/>
    <circle cx="260" cy="760" r="3"/>
    <circle cx="300" cy="710" r="4"/>
    <circle cx="170" cy="810" r="3"/>
    <circle cx="330" cy="780" r="4"/>
  </g>

  <!-- donut chart, upper right -->
  <g transform="translate(1400,180)" opacity="0.5">
    <circle r="70" fill="none" stroke="var(--red-dim)" stroke-width="18"/>
    <path d="M 0 -70 A 70 70 0 0 1 60 33" fill="none" stroke="var(--red)" stroke-width="18"/>
  </g>

  <!-- long baseline axis, center -->
  <line x1="0" y1="500" x2="1600" y2="500" stroke="var(--red-dim)" stroke-width="1" opacity="0.4"/>
</svg>
<div class="crt-overlay"></div>
<div class="vignette"></div>

<header>
  <div class="logo">VISHAL_R<span class="cursor"></span></div>
  <nav>
    <ul>
      <li><a href="#about">about</a></li>
      <li><a href="#skills">skills</a></li>
      <li><a href="#projects">projects</a></li>
      <li><a href="#goal">goal</a></li>
      <li><a href="#contact">contact</a></li>
    </ul>
  </nav>
</header>

<!-- HERO -->
<section class="hero">
  <div class="tag">// aspiring data analyst</div>
  <h1>VISHAL&nbsp;R</h1>
  <div class="role-line"><span id="typedText"></span><span class="caret"></span></div>
  <p class="desc">
    I turn raw numbers into decisions. Currently building my analytics stack with
    SQL, Excel, Python, Java and C — now leveling up with Power BI and Tableau.
    Actively seeking a data analyst internship to apply what I know and explore
    everything data has to offer.
  </p>
  <div class="cta-row">
    <a href="#projects" class="btn solid">view_projects()</a>
    <a href="#contact" class="btn">contact_me()</a>
  </div>
  <div class="scroll-hint"><div class="bar"></div>scroll</div>
</section>

<!-- ABOUT -->
<section id="about">
  <div class="prompt-label reveal">cat about.txt</div>
  <div class="about-grid">
    <p class="reveal">
      I'm Vishal R, working toward a career as a data analyst. I like getting into
      the details of a dataset — cleaning it, questioning it, and pulling a clear
      story out of the noise. I'm comfortable across SQL, Excel, Python, Java and C,
      and I'm currently deep in Power BI and Tableau to round out my visualization
      skills. I like building projects, not just following tutorials — every dataset
      is a new problem to explore. Right now I'm focused on becoming internship-ready.
    </p>
    <div class="stat-block reveal">
      <div class="stat">
        <span class="n">05</span>
        <span class="l">languages / tools mastered</span>
      </div>
      <div class="stat">
        <span class="n">02</span>
        <span class="l">tools currently learning</span>
      </div>
      <div class="stat">
        <span class="n">∞</span>
        <span class="l">curiosity about data</span>
      </div>
      <div class="stat">
        <span class="n">01</span>
        <span class="l">goal — land an internship</span>
      </div>
    </div>
  </div>
</section>

<!-- SKILLS -->
<section id="skills">
  <div class="prompt-label reveal">skills --list --verbose</div>
  <h2 class="reveal" style="font-size:28px;margin-bottom:8px;">Technical Arsenal</h2>
  <p class="reveal" style="color:var(--text-dim);font-size:14px;margin-bottom:10px;">
    Core tools I already know, and what I'm actively adding right now.
  </p>

  <div class="skill-terminal reveal">
    <div class="tbar"><span></span><span></span><span></span></div>

    <div class="skill-groups">
      <div class="skill-cat">
        <h4>Known / Core</h4>

        <div class="bar-row">
          <div class="lbl"><span>SQL</span><span>Databases</span></div>
          <div class="bar-track"><div class="bar-fill" data-fill="90"></div></div>
        </div>

        <div class="bar-row">
          <div class="lbl"><span>Excel</span><span>Data Prep</span></div>
          <div class="bar-track"><div class="bar-fill" data-fill="88"></div></div>
        </div>

        <div class="bar-row">
          <div class="lbl"><span>Python</span><span>Analysis</span></div>
          <div class="bar-track"><div class="bar-fill" data-fill="85"></div></div>
        </div>

        <div class="bar-row">
          <div class="lbl"><span>Java</span><span>Programming</span></div>
          <div class="bar-track"><div class="bar-fill" data-fill="75"></div></div>
        </div>

        <div class="bar-row">
          <div class="lbl"><span>C</span><span>Fundamentals</span></div>
          <div class="bar-track"><div class="bar-fill" data-fill="72"></div></div>
        </div>
      </div>

      <div class="skill-cat">
        <h4>Currently Learning</h4>
        <p style="color:var(--text-dim); font-size:13px; line-height:1.7; margin-bottom:12px;">
          Building visualization and dashboarding skills to turn analysis into
          decisions stakeholders can act on.
        </p>
        <span class="learning-tag">Power BI</span>
        <span class="learning-tag">Tableau</span>

        <h4 style="margin-top:24px;">Core Interests</h4>
        <span class="learning-tag">Data Cleaning</span>
        <span class="learning-tag">EDA</span>
        <span class="learning-tag">Dashboards</span>
        <span class="learning-tag">Reporting</span>
        <span class="learning-tag">Automation</span>
      </div>
    </div>
  </div>
</section>

<!-- PROJECTS -->
<section id="projects">
  <div class="prompt-label reveal">ls projects/</div>
  <h2 class="reveal" style="font-size:28px;margin-bottom:8px;">Projects</h2>
  <p class="reveal" style="color:var(--text-dim);font-size:14px;margin-bottom:30px;">
    A running log of things I've built while learning. Edit the cards below with
    your real project details — titles, descriptions, stack and links.
  </p>

  <div class="projects-grid">
    <div class="project-card reveal">
      <div class="idx">PROJECT_01</div>
      <h3>Sales Data Dashboard</h3>
      <p>Replace this with a short description — what data, what question you
        answered, and what insight you found.</p>
      <div class="stack"><span>SQL</span><span>Excel</span><span>Python</span></div>
    </div>

    <div class="project-card reveal">
      <div class="idx">PROJECT_02</div>
      <h3>Customer Segmentation Analysis</h3>
      <p>Replace this with a short description of your project, the tools you
        used, and the outcome or takeaway.</p>
      <div class="stack"><span>Python</span><span>Pandas</span></div>
    </div>

    <div class="project-card reveal">
      <div class="idx">PROJECT_03</div>
      <h3>Interactive Power BI Report</h3>
      <p>Replace this with your Power BI / Tableau project — what business
        question it answers and how it's used.</p>
      <div class="stack"><span>Power BI</span><span>DAX</span></div>
    </div>

  </div>
</section>

<!-- GOAL -->
<section id="goal">
  <div class="prompt-label reveal">cat roadmap.log</div>
  <h2 class="reveal" style="font-size:28px;margin-bottom:24px;">What I'm Working Toward</h2>

  <div class="goal-line reveal"><div class="dot"></div><span class="txt">Built a strong base in SQL, Excel, Python, Java and C.</span></div>
  <div class="goal-line reveal"><div class="dot"></div><span class="txt">Currently learning Power BI and Tableau for visualization and reporting.</span></div>
  <div class="goal-line dim reveal"><div class="dot"></div><span class="txt">Ready and actively looking for a data analyst internship.</span></div>
  <div class="goal-line dim reveal"><div class="dot"></div><span class="txt">Want to keep exploring — new datasets, new domains, new tools.</span></div>
</section>

<!-- CONTACT -->
<section id="contact">
  <div class="contact-box">
    <div class="prompt-label reveal">contact --info</div>
    <h2 class="reveal" style="font-size:30px;">Let's Talk Data.</h2>
    <p class="reveal">Open to data analyst internship opportunities. Reach out — I'd love to
      hear about the problem you're trying to solve.</p>
    <a class="big-mail reveal" href="mailto:ravichandranvishal50@gmail.com">ravichandranvishal50@gmail.com</a>
  </div>
</section>

<footer>
  &gt; built by Vishal R <span class="r">// designed for a data-analyst future</span>
</footer>

<script>
  // ---------------- Typing effect ----------------
  const roles = [
    "Aspiring Data Analyst",
    "SQL . Excel . Python . Java . C",
    "Learning Power BI & Tableau",
    "Internship Ready. Data Curious."
  ];
  const typedEl = document.getElementById('typedText');
  let roleIdx = 0, charIdx = 0, deleting = false;

  function typeLoop(){
    const current = roles[roleIdx];
    if(!deleting){
      charIdx++;
      typedEl.textContent = current.slice(0, charIdx);
      if(charIdx === current.length){
        deleting = true;
        setTimeout(typeLoop, 1400);
        return;
      }
    } else {
      charIdx--;
      typedEl.textContent = current.slice(0, charIdx);
      if(charIdx === 0){
        deleting = false;
        roleIdx = (roleIdx + 1) % roles.length;
      }
    }
    setTimeout(typeLoop, deleting ? 35 : 65);
  }
  typeLoop();

  // ---------------- Scroll reveal ----------------
  const revealEls = document.querySelectorAll('.reveal');
  const io = new IntersectionObserver((entries)=>{
    entries.forEach(e=>{
      if(e.isIntersecting){
        e.target.classList.add('in');
        io.unobserve(e.target);
      }
    });
  }, {threshold:0.15});
  revealEls.forEach(el=>io.observe(el));

  // ---------------- Skill bar fill on view ----------------
  const bars = document.querySelectorAll('.bar-fill');
  const barIO = new IntersectionObserver((entries)=>{
    entries.forEach(e=>{
      if(e.isIntersecting){
        e.target.style.width = e.target.dataset.fill + '%';
        barIO.unobserve(e.target);
      }
    });
  }, {threshold:0.4});
  bars.forEach(b=>barIO.observe(b));

</script>

</body>
</html>
