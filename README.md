<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>BureauOS — Navigateur bureaucratique (prototype)</title>
  <style>
    :root{
      --bg:#0b0f14;
      --panel:#111827;
      --panel2:#0f172a;
      --text:#e5e7eb;
      --muted:#9ca3af;
      --line:rgba(255,255,255,.08);
      --accent:#7c3aed;
      --good:#22c55e;
      --warn:#f59e0b;
      --bad:#ef4444;
      --shadow: 0 14px 40px rgba(0,0,0,.55);
      --radius:16px;
      --radius2:22px;
      --mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      --sans: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji","Segoe UI Emoji";
    }
    *{ box-sizing:border-box; }
    html,body{ height:100%; }
    body{
      margin:0;
      font-family:var(--sans);
      background: radial-gradient(1200px 500px at 20% -10%, rgba(124,58,237,.25), transparent 60%),
                  radial-gradient(800px 600px at 100% 10%, rgba(34,197,94,.12), transparent 55%),
                  var(--bg);
      color:var(--text);
    }
    a{ color:inherit; text-decoration:none; }
    button, input, select, textarea{
      font-family:inherit;
      color:inherit;
    }
    .container{ max-width:1100px; margin:0 auto; padding:0 18px; }
    .topbar{
      position:sticky; top:0; z-index:50;
      backdrop-filter: blur(10px);
      background: rgba(11,15,20,.65);
      border-bottom:1px solid var(--line);
    }
    .topbar-inner{
      display:flex; align-items:center; justify-content:space-between;
      height:64px;
    }
    .brand{
      display:flex; gap:10px; align-items:center; font-weight:700; letter-spacing:.2px;
    }
    .logo{
      width:34px; height:34px; border-radius:12px;
      background: linear-gradient(135deg, rgba(124,58,237,.95), rgba(34,197,94,.85));
      box-shadow: 0 10px 30px rgba(124,58,237,.25);
      position:relative;
    }
    .logo:after{
      content:"";
      position:absolute; inset:8px;
      border-radius:10px;
      background: rgba(11,15,20,.45);
      border:1px solid rgba(255,255,255,.12);
    }
    .nav{
      display:flex; gap:14px; align-items:center; flex-wrap:wrap;
    }
    .nav a{
      padding:8px 10px; border-radius:12px;
      color:var(--muted);
      border:1px solid transparent;
    }
    .nav a:hover{ color:var(--text); border-color:var(--line); background:rgba(255,255,255,.03); }
    .pill{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      padding:8px 12px; border-radius:999px;
      color:var(--muted);
      display:flex; gap:8px; align-items:center;
    }
    .dot{ width:8px; height:8px; border-radius:99px; background:var(--good); box-shadow:0 0 0 4px rgba(34,197,94,.12); }
    .actions{ display:flex; gap:10px; align-items:center; }
    .btn{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      padding:10px 12px; border-radius:14px;
      cursor:pointer;
      transition: transform .06s ease, background .15s ease, border-color .15s ease;
    }
    .btn:hover{ background:rgba(255,255,255,.06); border-color:rgba(255,255,255,.14); }
    .btn:active{ transform: translateY(1px); }
    .btn-primary{
      border-color:rgba(124,58,237,.55);
      background: rgba(124,58,237,.18);
    }
    .btn-primary:hover{ background: rgba(124,58,237,.26); }
    .btn-danger{
      border-color:rgba(239,68,68,.45);
      background: rgba(239,68,68,.12);
    }
    .btn-mini{ padding:8px 10px; border-radius:12px; font-size:13px; }
    .hero{
      padding:42px 0 28px;
    }
    .grid{
      display:grid; grid-template-columns: 1.25fr .75fr; gap:18px;
    }
    @media (max-width: 900px){
      .grid{ grid-template-columns:1fr; }
      .nav{ display:none; }
    }
    .card{
      background: linear-gradient(180deg, rgba(17,24,39,.82), rgba(15,23,42,.82));
      border:1px solid var(--line);
      border-radius: var(--radius2);
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .card-inner{ padding:18px; }
    .h1{
      font-size: 40px; line-height:1.08; margin:0 0 10px;
      letter-spacing:-.5px;
    }
    .sub{
      color:var(--muted);
      font-size:16px;
      line-height:1.5;
      margin:0 0 16px;
    }
    .row{ display:flex; gap:10px; flex-wrap:wrap; align-items:center; }
    .kpis{ display:flex; gap:10px; flex-wrap:wrap; margin-top:14px; }
    .kpi{
      flex:1;
      min-width: 160px;
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      border-radius: var(--radius);
      padding:12px 12px;
    }
    .kpi .label{ color:var(--muted); font-size:12px; }
    .kpi .value{ font-weight:700; font-size:18px; margin-top:6px; }
    .chip{
      display:inline-flex; align-items:center; gap:8px;
      padding:8px 10px; border-radius: 999px;
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      color:var(--muted);
      font-size:13px;
    }
    .chip strong{ color:var(--text); font-weight:700; }
    .divider{ height:1px; background:var(--line); margin:14px 0; }
    .small{ color:var(--muted); font-size:13px; line-height:1.45; }
    .section{
      padding: 18px 0 40px;
    }
    .section h2{ margin:0 0 10px; font-size:20px; }
    .section p{ margin:0 0 10px; color:var(--muted); }
    .two{
      display:grid; grid-template-columns: 1fr 1fr; gap:18px;
    }
    @media (max-width: 900px){ .two{ grid-template-columns:1fr; } }

    /* Feature list */
    .list{ display:grid; gap:10px; }
    .item{
      display:flex; gap:12px; align-items:flex-start;
      padding:12px; border-radius: var(--radius);
      border:1px solid var(--line); background:rgba(255,255,255,.03);
    }
    .badge{
      font-family:var(--mono);
      font-size:12px;
      padding:4px 8px;
      border-radius: 999px;
      border:1px solid var(--line);
      background: rgba(255,255,255,.03);
      color: var(--muted);
      white-space:nowrap;
      margin-top:2px;
    }
    .item .title{ font-weight:700; margin:0; }
    .item .desc{ margin:6px 0 0; color:var(--muted); font-size:13px; line-height:1.45; }

    /* Pricing */
    .pricing{
      display:grid; grid-template-columns: 1fr 1fr 1fr; gap:14px;
    }
    @media (max-width: 900px){ .pricing{ grid-template-columns:1fr; } }
    .price-card{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      border-radius: var(--radius2);
      padding:16px;
      position:relative;
      overflow:hidden;
    }
    .price-card.featured{
      border-color: rgba(124,58,237,.55);
      background: radial-gradient(900px 200px at 20% 0%, rgba(124,58,237,.25), transparent 55%),
                  rgba(255,255,255,.03);
    }
    .tag{
      position:absolute; top:12px; right:12px;
      padding:6px 10px;
      border-radius:999px;
      border:1px solid var(--line);
      background:rgba(11,15,20,.45);
      color: var(--muted);
      font-size:12px;
    }
    .price-name{ font-weight:800; font-size:16px; }
    .price{ font-size:34px; font-weight:900; margin:8px 0 4px; }
    .per{ color:var(--muted); font-size:12px; }
    .ul{ margin:12px 0 0; padding:0; list-style:none; display:grid; gap:8px; }
    .ul li{
      display:flex; gap:10px; align-items:flex-start;
      color:var(--muted); font-size:13px; line-height:1.35;
    }
    .check{ width:18px; height:18px; border-radius:6px; border:1px solid var(--line); background:rgba(34,197,94,.12); display:inline-flex; align-items:center; justify-content:center; font-size:12px; color:var(--good); }
    .x{ background:rgba(239,68,68,.10); color:var(--bad); }
    .foot{
      padding:24px 0 38px;
      color:var(--muted);
      font-size:13px;
      border-top:1px solid var(--line);
    }

    /* Pages (SPA) */
    .page{ display:none; }
    .page.active{ display:block; }

    /* Forms */
    .field{ display:grid; gap:6px; margin-bottom:10px; }
    label{ font-size:12px; color:var(--muted); }
    input, select, textarea{
      width:100%;
      padding:10px 12px;
      border-radius: 14px;
      border:1px solid var(--line);
      background: rgba(255,255,255,.03);
      outline:none;
    }
    textarea{ min-height:92px; resize:vertical; }
    input:focus, select:focus, textarea:focus{
      border-color: rgba(124,58,237,.55);
      box-shadow: 0 0 0 4px rgba(124,58,237,.12);
    }
    .form-row{ display:grid; grid-template-columns: 1fr 1fr; gap:10px; }
    @media (max-width: 900px){ .form-row{ grid-template-columns:1fr; } }

    /* Dashboard */
    .dash{
      display:grid; grid-template-columns: 260px 1fr; gap:16px;
      padding: 18px 0 40px;
    }
    @media (max-width: 900px){
      .dash{ grid-template-columns:1fr; }
    }
    .side{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      border-radius: var(--radius2);
      padding:14px;
    }
    .side h3{ margin:0 0 10px; font-size:14px; color:var(--muted); font-weight:700; letter-spacing:.2px; }
    .side .navbtn{
      width:100%;
      text-align:left;
      padding:10px 10px;
      border-radius:14px;
      border:1px solid transparent;
      background:transparent;
      color:var(--muted);
      cursor:pointer;
      display:flex; justify-content:space-between; align-items:center;
    }
    .side .navbtn:hover{ background:rgba(255,255,255,.04); border-color:var(--line); color:var(--text); }
    .side .navbtn.active{ background:rgba(124,58,237,.16); border-color:rgba(124,58,237,.45); color:var(--text); }
    .pill2{
      font-family:var(--mono);
      font-size:11px;
      padding:3px 8px;
      border-radius:999px;
      border:1px solid var(--line);
      background:rgba(11,15,20,.35);
      color:var(--muted);
    }
    .main{
      border:1px solid var(--line);
      background: linear-gradient(180deg, rgba(17,24,39,.75), rgba(15,23,42,.75));
      border-radius: var(--radius2);
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .main-head{
      padding:14px 16px;
      display:flex; justify-content:space-between; align-items:center; gap:10px; flex-wrap:wrap;
      border-bottom:1px solid var(--line);
      background: rgba(0,0,0,.12);
    }
    .main-head .title{
      font-size:16px; font-weight:900;
    }
    .tabs{
      display:flex; gap:8px; flex-wrap:wrap;
    }
    .tab{
      padding:8px 10px;
      border-radius: 14px;
      border:1px solid transparent;
      background:rgba(255,255,255,.02);
      color:var(--muted);
      cursor:pointer;
    }
    .tab:hover{ border-color:var(--line); color:var(--text); }
    .tab.active{ background:rgba(124,58,237,.16); border-color:rgba(124,58,237,.45); color:var(--text); }
    .content{ padding:16px; }
    .alert{
      border:1px solid var(--line);
      border-left:4px solid rgba(245,158,11,.9);
      background:rgba(245,158,11,.08);
      border-radius: 16px;
      padding:12px;
      display:flex; gap:12px; align-items:flex-start;
      margin-bottom:12px;
    }
    .alert strong{ display:block; margin-bottom:4px; }
    .alert .meta{ color:var(--muted); font-size:13px; line-height:1.35; }
    .timeline{ display:grid; gap:10px; }
    .t-item{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      border-radius: var(--radius2);
      padding:12px;
      display:flex; gap:12px; align-items:flex-start;
    }
    .status{
      width:12px; height:12px; border-radius:99px;
      margin-top:4px;
      background: var(--muted);
      box-shadow: 0 0 0 4px rgba(156,163,175,.12);
      flex:0 0 auto;
    }
    .status.good{ background:var(--good); box-shadow:0 0 0 4px rgba(34,197,94,.12); }
    .status.warn{ background:var(--warn); box-shadow:0 0 0 4px rgba(245,158,11,.12); }
    .status.bad{ background:var(--bad); box-shadow:0 0 0 4px rgba(239,68,68,.12); }
    .t-item .head{ display:flex; gap:8px; align-items:center; flex-wrap:wrap; }
    .t-item .head .when{ font-family:var(--mono); font-size:12px; color:var(--muted); }
    .t-item .head .prio{
      font-size:12px;
      padding:4px 8px;
      border-radius:999px;
      border:1px solid var(--line);
      background:rgba(11,15,20,.35);
      color:var(--muted);
    }
    .prio.high{ border-color:rgba(239,68,68,.45); color:#fecaca; background:rgba(239,68,68,.08); }
    .prio.med{ border-color:rgba(245,158,11,.45); color:#fde68a; background:rgba(245,158,11,.08); }
    .prio.low{ border-color:rgba(34,197,94,.45); color:#bbf7d0; background:rgba(34,197,94,.08); }
    .t-item .desc{ margin:6px 0 0; color:var(--muted); font-size:13px; line-height:1.45; }
    .t-actions{ margin-top:10px; display:flex; gap:8px; flex-wrap:wrap; }

    .docgrid{ display:grid; grid-template-columns: 1fr 1fr; gap:10px; }
    @media (max-width: 900px){ .docgrid{ grid-template-columns:1fr; } }
    .doc{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      border-radius: var(--radius2);
      padding:12px;
    }
    .doc .name{ font-weight:800; }
    .doc .meta{ color:var(--muted); font-size:12px; margin-top:4px; }
    .doc .btns{ margin-top:10px; display:flex; gap:8px; flex-wrap:wrap; }

    .paywall{
      border:1px solid rgba(124,58,237,.45);
      background: radial-gradient(900px 220px at 20% 0%, rgba(124,58,237,.22), transparent 55%),
                  rgba(124,58,237,.08);
      border-radius: var(--radius2);
      padding:14px;
    }
    .paywall h4{ margin:0 0 6px; }
    .paywall p{ margin:0 0 10px; color:var(--muted); font-size:13px; line-height:1.45; }

    /* Modal */
    .modal-backdrop{
      position:fixed; inset:0; display:none; place-items:center;
      background: rgba(0,0,0,.55);
      z-index:100;
      padding:18px;
    }
    .modal-backdrop.active{ display:grid; }
    .modal{
      width:min(520px, 100%);
      border-radius: 22px;
      border:1px solid var(--line);
      background: linear-gradient(180deg, rgba(17,24,39,.92), rgba(15,23,42,.92));
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .modal-head{
      padding:12px 14px;
      display:flex; justify-content:space-between; align-items:center;
      border-bottom:1px solid var(--line);
      background: rgba(0,0,0,.12);
    }
    .modal-head strong{ font-weight:900; }
    .modal-body{ padding:14px; }
    .modal-foot{
      padding:12px 14px;
      border-top:1px solid var(--line);
      display:flex; justify-content:flex-end; gap:10px; flex-wrap:wrap;
      background: rgba(0,0,0,.10);
    }
    .mutelink{ color:#c4b5fd; }
    .mutelink:hover{ text-decoration: underline; }

    /* Notice */
    .notice{
      border:1px solid var(--line);
      background:rgba(255,255,255,.03);
      border-radius: var(--radius2);
      padding:12px;
      color:var(--muted);
      font-size:13px;
      line-height:1.45;
    }
    .notice strong{ color:var(--text); }

    /* Header anchor-ish */
    .anchor{ scroll-margin-top:90px; }
  </style>
</head>

<body>
  <!-- Topbar -->
  <div class="topbar">
    <div class="container topbar-inner">
      <div class="brand">
        <div class="logo" aria-hidden="true"></div>
        <div>
          <div style="line-height:1.1">BureauOS</div>
          <div style="color:var(--muted); font-size:12px; font-weight:600;">Navigateur bureaucratique</div>
        </div>
      </div>

      <div class="nav" aria-label="Navigation">
        <a href="#/" data-route="/">Accueil</a>
        <a href="#/guides" data-route="/guides">Guides</a>
        <a href="#/pricing" data-route="/pricing">Tarifs</a>
        <a href="#/how" data-route="/how">Comment ça marche</a>
        <a href="#/trust" data-route="/trust">Fiabilité</a>
      </div>

      <div class="actions">
        <div class="pill" title="Prototype UI">
          <span class="dot"></span>
          <span style="font-size:13px;">prototype</span>
        </div>
        <button class="btn btn-mini" id="btnLogin">Se connecter</button>
        <button class="btn btn-primary btn-mini" id="btnSignup">Créer un compte</button>
        <button class="btn btn-mini" id="btnDashboard" style="display:none;">Dashboard</button>
      </div>
    </div>
  </div>

  <!-- Pages -->
  <main class="container">

    <!-- HOME -->
    <section class="page active" id="page-home">
      <div class="hero grid">
        <div class="card">
          <div class="card-inner">
            <div class="chip">
              <span>🎯</span>
              <span><strong>Réduis</strong> l’incertitude, pas ton niveau de stress.</span>
            </div>
            <h1 class="h1">Le système qui transforme la bureaucratie en <span style="color:#c4b5fd;">timeline</span>.</h1>
            <p class="sub">
              BureauOS te donne un plan d’action personnalisé basé sur ton statut (pays, visa, emploi, études, fiscalité),
              avec deadlines, documents, et alertes. Pas un chatbot gadget. Un dossier vivant.
            </p>

            <div class="row">
              <button class="btn btn-primary" data-goto="/pricing">Voir les tarifs</button>
              <button class="btn" data-goto="/guides">Explorer les guides</button>
              <button class="btn" id="btnTryDemo">Essayer la démo (dashboard)</button>
            </div>

            <div class="kpis">
              <div class="kpi">
                <div class="label">Valeur</div>
                <div class="value">“Quoi faire, quand, pourquoi”</div>
              </div>
              <div class="kpi">
                <div class="label">Promesse</div>
                <div class="value">Rien oublier, rien improviser</div>
              </div>
              <div class="kpi">
                <div class="label">Approche</div>
                <div class="value">Cas + checklists + preuves</div>
              </div>
            </div>

            <div class="divider"></div>
            <p class="small">
              ⚖️ <strong>Note importante :</strong> BureauOS n’est pas un cabinet d’avocats et ne fournit pas de conseil juridique.
              C’est un outil d’organisation, d’explication, de génération documentaire et de suivi de démarches, avec indication de confiance.
            </p>
          </div>
        </div>

        <div class="card">
          <div class="card-inner">
            <h2 style="margin:0 0 10px;">Démarrage express</h2>
            <p class="small">Choisis un profil (exemples). Le but : montrer la logique du produit.</p>
            <div class="divider"></div>

            <div class="field">
              <label>Pays</label>
              <select id="quickCountry">
                <option value="FR">France</option>
                <option value="US">États-Unis</option>
                <option value="UK">Royaume-Uni</option>
                <option value="CA">Canada</option>
              </select>
            </div>

            <div class="field">
              <label>Profil</label>
              <select id="quickProfile">
                <option value="student">Étudiant international</option>
                <option value="expat">Expatrié (salarié)</option>
                <option value="freelance">Freelance / indépendant</option>
                <option value="family">Famille (enfants, école, santé)</option>
              </select>
            </div>

            <button class="btn btn-primary" id="btnBuildPlan">Générer ma timeline</button>

            <div class="divider"></div>

            <div class="notice" id="quickOutput">
              <strong>Ce que tu obtiens :</strong><br>
              Une timeline personnalisée + documents à préparer + alertes. (Démo UI)
            </div>
          </div>
        </div>
      </div>

      <section class="section">
        <div class="two">
          <div class="card">
            <div class="card-inner">
              <h2>Pourquoi ce n’est pas “juste Google/ChatGPT”</h2>
              <div class="list">
                <div class="item">
                  <div class="badge">LONG</div>
                  <div>
                    <p class="title">Suivi longitudinal</p>
                    <p class="desc">Ton dossier évolue : statut, dates, documents, historiques. Pas une réponse ponctuelle.</p>
                  </div>
                </div>
                <div class="item">
                  <div class="badge">RISK</div>
                  <div>
                    <p class="title">Gestion du risque</p>
                    <p class="desc">Obligatoire vs recommandé, niveaux de confiance, points critiques, trace des versions.</p>
                  </div>
                </div>
                <div class="item">
                  <div class="badge">TIME</div>
                  <div>
                    <p class="title">Deadlines et relances</p>
                    <p class="desc">C’est la date qui te tue, pas l’info. BureauOS te pousse au bon moment.</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="card">
            <div class="card-inner">
              <h2>Ce que les gens paient vraiment</h2>
              <p class="small">
                Pas “l’info”. Ils paient pour <strong>ne pas se tromper</strong>, <strong>ne rien oublier</strong>, et
                <strong>réduire l’angoisse</strong> avec un plan clair.
              </p>
              <div class="divider"></div>
              <div class="list">
                <div class="item">
                  <div class="badge">DOCS</div>
                  <div>
                    <p class="title">Génération de documents</p>
                    <p class="desc">PDF/Word, lettres, checklists, dossiers prêts à envoyer. Moins de friction.</p>
                  </div>
                </div>
                <div class="item">
                  <div class="badge">PROOF</div>
                  <div>
                    <p class="title">Traçabilité</p>
                    <p class="desc">Historique, preuves, sources et versions pour limiter les “on ne m’a pas dit”.</p>
                  </div>
                </div>
              </div>

              <div class="divider"></div>
              <button class="btn btn-primary" data-goto="/pricing">Je veux voir l’offre</button>
            </div>
          </div>
        </div>
      </section>
    </section>

    <!-- GUIDES (SEO pages) -->
    <section class="page" id="page-guides">
      <div class="section">
        <div class="card">
          <div class="card-inner">
            <h2>Guides (pages SEO)</h2>
            <p class="small">
              Ici, tu crées des pages ultra ciblées qui amènent du trafic. Mais la valeur est dans l’espace personnel.
              Ces pages servent de porte d’entrée et de preuve de sérieux.
            </p>
            <div class="divider"></div>

            <div class="docgrid">
              <div class="doc">
                <div class="name">Étudiant international en France</div>
                <div class="meta">Checklist visa/titre de séjour, logement, banque, assurance, CAF, etc.</div>
                <div class="btns">
                  <button class="btn btn-mini" data-open-guide="fr-student">Ouvrir</button>
                  <button class="btn btn-mini btn-primary" data-cta="signup">Débloquer en suivi perso</button>
                </div>
              </div>

              <div class="doc">
                <div class="name">S’installer aux États-Unis (résident/expat)</div>
                <div class="meta">SSN/ITIN, santé, impôts, logement, permis, école, etc.</div>
                <div class="btns">
                  <button class="btn btn-mini" data-open-guide="us-expat">Ouvrir</button>
                  <button class="btn btn-mini btn-primary" data-cta="signup">Débloquer en suivi perso</button>
                </div>
              </div>

              <div class="doc">
                <div class="name">Freelance transfrontalier</div>
                <div class="meta">Statuts, facturation, TVA, obligations, déclarations, risques.</div>
                <div class="btns">
                  <button class="btn btn-mini" data-open-guide="x-freelance">Ouvrir</button>
                  <button class="btn btn-mini btn-primary" data-cta="signup">Débloquer en suivi perso</button>
                </div>
              </div>

              <div class="doc">
                <div class="name">Famille : santé, école, démarches récurrentes</div>
                <div class="meta">Couverture santé, inscriptions, justificatifs, déménagements.</div>
                <div class="btns">
                  <button class="btn btn-mini" data-open-guide="family">Ouvrir</button>
                  <button class="btn btn-mini btn-primary" data-cta="signup">Débloquer en suivi perso</button>
                </div>
              </div>
            </div>

            <div class="divider"></div>
            <div class="notice" id="guideViewer">
              <strong>Ouvre un guide</strong> pour voir une page SEO type. (Démo simplifiée)
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- PRICING -->
    <section class="page" id="page-pricing">
      <div class="section">
        <div class="card">
          <div class="card-inner">
            <h2>Tarifs</h2>
            <p class="small">
              L’offre est pensée “lifetime problem” : suivi dans le temps + documents + alertes. Le gratuit sert à attirer.
            </p>
            <div class="divider"></div>

            <div class="pricing">
              <div class="price-card">
                <div class="price-name">Gratuit</div>
                <div class="price">0€</div>
                <div class="per">Pour tester la logique</div>
                <ul class="ul">
                  <li><span class="check">✓</span> 1 profil simple (démo)</li>
                  <li><span class="check">✓</span> 3 actions dans la timeline</li>
                  <li><span class="check x">✕</span> Alertes &amp; relances</li>
                  <li><span class="check x">✕</span> Documents exportables</li>
                  <li><span class="check x">✕</span> Historique &amp; versions</li>
                </ul>
                <div class="divider"></div>
                <button class="btn btn-primary" data-cta="signup">Créer un compte</button>
              </div>

              <div class="price-card featured">
                <div class="tag">Le plus vendu</div>
                <div class="price-name">Plus</div>
                <div class="price">9,90€</div>
                <div class="per">par mois</div>
                <ul class="ul">
                  <li><span class="check">✓</span> Timeline complète personnalisée</li>
                  <li><span class="check">✓</span> Alertes e-mail (deadlines)</li>
                  <li><span class="check">✓</span> Génération de documents (PDF/Word)</li>
                  <li><span class="check">✓</span> Scores de confiance &amp; risques</li>
                  <li><span class="check x">✕</span> Multi-pays / multi-statuts</li>
                </ul>
                <div class="divider"></div>
                <button class="btn btn-primary" data-cta="checkout" data-plan="plus">Démarrer Plus</button>
              </div>

              <div class="price-card">
                <div class="price-name">Pro</div>
                <div class="price">24,90€</div>
                <div class="per">par mois</div>
                <ul class="ul">
                  <li><span class="check">✓</span> Multi-statuts (évolutions)</li>
                  <li><span class="check">✓</span> Multi-pays (déménagement)</li>
                  <li><span class="check">✓</span> Historique &amp; versions</li>
                  <li><span class="check">✓</span> Exports illimités</li>
                  <li><span class="check">✓</span> “Pré-audit” (contrôle interne)</li>
                </ul>
                <div class="divider"></div>
                <button class="btn btn-primary" data-cta="checkout" data-plan="pro">Passer Pro</button>
              </div>
            </div>

            <div class="divider"></div>
            <div class="notice">
              <strong>Important :</strong> paiement simulé dans ce prototype. En prod, tu branches Stripe Checkout + webhooks.
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- HOW IT WORKS -->
    <section class="page" id="page-how">
      <div class="section">
        <div class="card">
          <div class="card-inner">
            <h2>Comment ça marche</h2>
            <div class="divider"></div>
            <div class="list">
              <div class="item">
                <div class="badge">1</div>
                <div>
                  <p class="title">On te classe dans un cas</p>
                  <p class="desc">Pays, statut, dates, objectifs. Le produit fonctionne par scénarios (pas par blabla).</p>
                </div>
              </div>
              <div class="item">
                <div class="badge">2</div>
                <div>
                  <p class="title">On génère la timeline</p>
                  <p class="desc">Chaque action a une priorité, une deadline, des documents, et une justification.</p>
                </div>
              </div>
              <div class="item">
                <div class="badge">3</div>
                <div>
                  <p class="title">On te relance avant le mur</p>
                  <p class="desc">Emails (puis SMS/app plus tard). La mémoire et le temps créent la rétention.</p>
                </div>
              </div>
              <div class="item">
                <div class="badge">4</div>
                <div>
                  <p class="title">Tu exportes un dossier propre</p>
                  <p class="desc">PDF/Word, checklists, preuves. La bureaucratie aime les dossiers carrés.</p>
                </div>
              </div>
            </div>

            <div class="divider"></div>
            <button class="btn btn-primary" id="btnGoDash2">Voir le dashboard</button>
          </div>
        </div>
      </div>
    </section>

    <!-- TRUST -->
    <section class="page" id="page-trust">
      <div class="section">
        <div class="card">
          <div class="card-inner">
            <h2>Fiabilité &amp; limites</h2>
            <p class="small">
              C’est ici que tu gagnes la confiance. Si tu joues au chatbot qui “sait tout”, tu te tires une balle.
            </p>
            <div class="divider"></div>
            <div class="list">
              <div class="item">
                <div class="badge">CONF</div>
                <div>
                  <p class="title">Niveaux de confiance</p>
                  <p class="desc">Chaque recommandation affiche un niveau (élevé/moyen/faible) et le type de source attendue.</p>
                </div>
              </div>
              <div class="item">
                <div class="badge">SCOPE</div>
                <div>
                  <p class="title">Périmètre clair</p>
                  <p class="desc">Organisation, explication, génération de documents, checklists. Pas de conseil juridique personnalisé.</p>
                </div>
              </div>
              <div class="item">
                <div class="badge">TRACE</div>
                <div>
                  <p class="title">Historique &amp; preuves</p>
                  <p class="desc">Versioning des docs, audit trail des actions, et export du dossier complet.</p>
                </div>
              </div>
            </div>

            <div class="divider"></div>
            <button class="btn" data-goto="/pricing">Voir les offres</button>
          </div>
        </div>
      </div>
    </section>

    <!-- DASHBOARD -->
    <section class="page" id="page-dashboard">
      <div class="dash">
        <aside class="side">
          <h3>Navigation</h3>
          <button class="navbtn active" data-dash="overview">
            <span>Vue d’ensemble</span> <span class="pill2">HOME</span>
          </button>
          <button class="navbtn" data-dash="timeline">
            <span>Timeline</span> <span class="pill2">TIME</span>
          </button>
          <button class="navbtn" data-dash="documents">
            <span>Documents</span> <span class="pill2">DOCS</span>
          </button>
          <button class="navbtn" data-dash="checklist">
            <span>Checklist</span> <span class="pill2">TODO</span>
          </button>
          <button class="navbtn" data-dash="profile">
            <span>Profil</span> <span class="pill2">ME</span>
          </button>
          <div class="divider"></div>
          <h3>Mon abonnement</h3>
          <div class="notice" id="planBox">
            <strong>Plan :</strong> <span id="planName">Gratuit</span><br>
            <span id="planHint">Certaines fonctions sont verrouillées.</span>
            <div style="margin-top:10px; display:flex; gap:8px; flex-wrap:wrap;">
              <button class="btn btn-mini btn-primary" data-goto="/pricing">Upgrader</button>
              <button class="btn btn-mini" id="btnLogout">Se déconnecter</button>
            </div>
          </div>
        </aside>

        <section class="main">
          <div class="main-head">
            <div>
              <div class="title">Dashboard</div>
              <div class="small">Ton dossier : <span id="dashLabel" style="color:#c4b5fd;">France • Étudiant international</span></div>
            </div>
            <div class="tabs">
              <button class="tab active" data-tab="actions">Actions</button>
              <button class="tab" data-tab="alerts">Alertes</button>
              <button class="tab" data-tab="risk">Risques</button>
            </div>
          </div>

          <div class="content">
            <div class="alert" id="topAlert">
              <div style="font-size:18px;">⏳</div>
              <div>
                <strong>Deadline dans 12 jours</strong>
                <div class="meta">Renouvellement / justificatifs. BureauOS te donne la liste exacte, et un modèle de lettre.</div>
              </div>
              <div style="margin-left:auto;">
                <button class="btn btn-mini btn-primary" id="btnUnlockFromAlert">Débloquer (Plus)</button>
              </div>
            </div>

            <!-- Tab panels -->
            <div id="tab-actions" class="tabpanel">
              <div class="timeline" id="timelineBox"></div>
            </div>

            <div id="tab-alerts" class="tabpanel" style="display:none;">
              <div class="notice">
                <strong>Alertes</strong><br>
                Emails/SMS se déclenchent selon tes dates. Dans la version payante : tu choisis la fréquence + canaux.
              </div>
              <div class="divider"></div>
              <div class="paywall" id="alertsPaywall">
                <h4>Fonction verrouillée</h4>
                <p>Les alertes automatiques sont disponibles en <strong>Plus</strong> et <strong>Pro</strong>.</p>
                <button class="btn btn-primary" data-goto="/pricing">Voir les plans</button>
              </div>
            </div>

            <div id="tab-risk" class="tabpanel" style="display:none;">
              <div class="notice">
                <strong>Score risque (démo)</strong><br>
                On te montre ce qui est critique, ce qui est flou, et ce qui dépend d’une confirmation (preuve/source).
              </div>
              <div class="divider"></div>
              <div class="docgrid">
                <div class="doc">
                  <div class="name">Risque visa / statut</div>
                  <div class="meta">Niveau : <span style="color:#fde68a;">moyen</span> • Confiance : <span style="color:#bbf7d0;">élevée</span></div>
                  <div class="small" style="margin-top:8px;">
                    À confirmer : justificatif domicile + assurance santé complète.
                  </div>
                </div>
                <div class="doc">
                  <div class="name">Risque fiscal</div>
                  <div class="meta">Niveau : <span style="color:#fecaca;">élevé</span> • Confiance : <span style="color:#fde68a;">moyenne</span></div>
                  <div class="small" style="margin-top:8px;">
                    Dépend de ton statut exact (résidence fiscale). BureauOS te pose 3 questions pour trancher.
                  </div>
                </div>
              </div>
            </div>

            <!-- Dashboard panes -->
            <div id="dash-overview" class="dashpanel" style="display:none;"></div>

            <div id="dash-timeline" class="dashpanel" style="display:none;">
              <div class="notice">
                <strong>Timeline</strong><br>
                Chaque action a : deadline, priorité, docs, et justification. C’est ça le produit.
              </div>
              <div class="divider"></div>
              <div class="timeline" id="timelineBox2"></div>
            </div>

            <div id="dash-documents" class="dashpanel" style="display:none;">
              <div class="notice">
                <strong>Documents</strong><br>
                En gratuit : aperçu. En payant : export Word/PDF + versioning.
              </div>
              <div class="divider"></div>

              <div class="docgrid">
                <div class="doc">
                  <div class="name">Modèle de lettre</div>
                  <div class="meta">Renouvellement / justification • Dernière maj : aujourd’hui</div>
                  <div class="btns">
                    <button class="btn btn-mini" onclick="toast('Aperçu ouvert (démo).')">Aperçu</button>
                    <button class="btn btn-mini btn-primary" id="btnExport1">Exporter (payant)</button>
                  </div>
                </div>
                <div class="doc">
                  <div class="name">Checklist dossier</div>
                  <div class="meta">Pièces • preuves • ordre de dépôt</div>
                  <div class="btns">
                    <button class="btn btn-mini" onclick="toast('Aperçu ouvert (démo).')">Aperçu</button>
                    <button class="btn btn-mini btn-primary" id="btnExport2">Exporter (payant)</button>
                  </div>
                </div>
              </div>

              <div class="divider"></div>
              <div class="paywall" id="docsPaywall">
                <h4>Exports verrouillés</h4>
                <p>Les exports Word/PDF et l’historique des versions sont disponibles en <strong>Plus</strong> et <strong>Pro</strong>.</p>
                <button class="btn btn-primary" data-goto="/pricing">Upgrader</button>
              </div>
            </div>

            <div id="dash-checklist" class="dashpanel" style="display:none;">
              <div class="notice">
                <strong>Checklist</strong><br>
                Décompose les démarches en tâches bêtes, parce que les “grands objectifs” ne se font jamais.
              </div>
              <div class="divider"></div>
              <div class="list" id="checklistBox"></div>
            </div>

            <div id="dash-profile" class="dashpanel" style="display:none;">
              <div class="notice">
                <strong>Profil</strong><br>
                Plus ton profil est précis, plus la timeline est fiable. Les gens ne payent pas pour du flou.
              </div>
              <div class="divider"></div>

              <div class="form-row">
                <div class="field">
                  <label>Pays</label>
                  <select id="profileCountry">
                    <option value="FR">France</option>
                    <option value="US">États-Unis</option>
                    <option value="UK">Royaume-Uni</option>
                    <option value="CA">Canada</option>
                  </select>
                </div>
                <div class="field">
                  <label>Profil</label>
                  <select id="profileType">
                    <option value="student">Étudiant international</option>
                    <option value="expat">Expatrié (salarié)</option>
                    <option value="freelance">Freelance / indépendant</option>
                    <option value="family">Famille</option>
                  </select>
                </div>
              </div>

              <div class="form-row">
                <div class="field">
                  <label>Date d’arrivée</label>
                  <input type="date" id="profileArrive" />
                </div>
                <div class="field">
                  <label>Objectif (texte)</label>
                  <input type="text" id="profileGoal" placeholder="Ex: renouveler titre de séjour + trouver logement" />
                </div>
              </div>

              <div class="field">
                <label>Notes</label>
                <textarea id="profileNotes" placeholder="Ce que tu veux que le système sache (démo)."></textarea>
              </div>

              <div class="row">
                <button class="btn btn-primary" id="btnSaveProfile">Enregistrer</button>
                <button class="btn" id="btnRebuild">Recalculer la timeline</button>
              </div>

              <div class="divider"></div>
              <div class="notice">
                <strong>En prod :</strong> tu stockes ça en base (utilisateur) et tu génères une timeline via règles + LLM, avec citations.
              </div>
            </div>

          </div>
        </section>
      </div>
    </section>

    <!-- CHECKOUT (mock) -->
    <section class="page" id="page-checkout">
      <div class="section">
        <div class="card">
          <div class="card-inner">
            <h2>Paiement (démo)</h2>
            <p class="small">Ceci simule Stripe Checkout. Clique “Payer” pour activer le plan dans l’UI.</p>
            <div class="divider"></div>

            <div class="two">
              <div class="notice">
                <strong>Plan sélectionné :</strong> <span id="checkoutPlan">Plus</span><br>
                <span id="checkoutPrice">9,90€ / mois</span><br><br>
                <span class="small">
                  En prod, tu fais : Stripe Checkout → webhook → activation plan en DB → accès fonctionnalités.
                </span>
              </div>

              <div class="card" style="box-shadow:none;">
                <div class="card-inner">
                  <div class="field">
                    <label>Email</label>
                    <input id="payEmail" type="email" placeholder="toi@exemple.com" />
                  </div>
                  <div class="field">
                    <label>Carte</label>
                    <input value="4242 4242 4242 4242" readonly />
                  </div>
                  <div class="form-row">
                    <div class="field">
                      <label>Expiration</label>
                      <input value="12/34" readonly />
                    </div>
                    <div class="field">
                      <label>CVC</label>
                      <input value="123" readonly />
                    </div>
                  </div>
                  <div class="row">
                    <button class="btn btn-primary" id="btnPay">Payer</button>
                    <button class="btn" data-goto="/pricing">Annuler</button>
                  </div>
                  <div class="divider"></div>
                  <div class="small">
                    En vrai, tu ne stockes jamais la carte. Stripe gère.
                  </div>
                </div>
              </div>
            </div>

          </div>
        </div>
      </div>
    </section>

    <footer class="foot">
      <div class="container">
        <div style="display:flex; justify-content:space-between; gap:10px; flex-wrap:wrap;">
          <div>© BureauOS (prototype UI) • <span style="font-family:var(--mono);">v0.1</span></div>
          <div>
            <a class="mutelink" href="#/trust" data-route="/trust">Fiabilité</a>
            <span style="opacity:.4;"> • </span>
            <a class="mutelink" href="#/how" data-route="/how">Comment ça marche</a>
          </div>
        </div>
      </div>
    </footer>

  </main>

  <!-- MODALS -->
  <div class="modal-backdrop" id="modalBackdrop" role="dialog" aria-modal="true" aria-hidden="true">
    <div class="modal" id="modal">
      <div class="modal-head">
        <strong id="modalTitle">Connexion</strong>
        <button class="btn btn-mini" id="btnCloseModal">Fermer</button>
      </div>
      <div class="modal-body" id="modalBody"></div>
      <div class="modal-foot" id="modalFoot"></div>
    </div>
  </div>

  <script>
    // ---------- State (prototype) ----------
    const state = {
      authed: false,
      user: { email: "", country: "FR", profile: "student", arrive: "", goal: "", notes: "" },
      plan: "free", // free | plus | pro
      route: "/",
      dashView: "overview",
      tab: "actions"
    };

    const routes = {
      "/": "page-home",
      "/guides": "page-guides",
      "/pricing": "page-pricing",
      "/how": "page-how",
      "/trust": "page-trust",
      "/dashboard": "page-dashboard",
      "/checkout": "page-checkout",
    };

    // ---------- Demo data builders ----------
    function labelCountry(code){
      return ({FR:"France",US:"États-Unis",UK:"Royaume-Uni",CA:"Canada"})[code] || code;
    }
    function labelProfile(p){
      return ({student:"Étudiant international",expat:"Expatrié (salarié)",freelance:"Freelance / indépendant",family:"Famille"})[p] || p;
    }
    function sampleTimeline(country, profile){
      // Intentionally generic. In prod: rules + sources + LLM with citations.
      const base = [
        { when:"J+0 → J+7", prio:"high", status:"warn", title:"Assembler tes pièces d’identité et preuves", desc:"Passeport/ID, justificatifs d’adresse, attestations nécessaires. Sans ça, tout bloque." },
        { when:"J+7 → J+21", prio:"high", status:"warn", title:"Dossier principal (démarche n°1)", desc:"Déclenche la démarche critique liée à ton statut. Si tu rates la fenêtre, tu payes en stress et en pénalités." },
        { when:"J+14 → J+45", prio:"med", status:"good", title:"Santé / couverture", desc:"Assurance, inscription, justificatifs. À valider selon ton statut exact." },
        { when:"M+1", prio:"med", status:"good", title:"Banque / preuves financières", desc:"Prépare relevés, contrats, attestations. C’est souvent demandé au mauvais moment." },
        { when:"M+2", prio:"low", status:"good", title:"Mise à jour administrative récurrente", desc:"Rappels et relances. La bureaucratie adore les routines." },
      ];
      // Slight flavor
      if(country==="FR" && profile==="student"){
        base[1].title = "Demande / renouvellement titre de séjour (étudiant)";
        base[2].title = "Assurance + attestation de scolarité";
      }
      if(country==="US" && profile==="expat"){
        base[1].title = "SSN/ITIN + setup administratif initial";
        base[2].title = "Couverture santé (choix + preuves)";
      }
      if(profile==="freelance"){
        base[1].title = "Choisir statut + obligations (déclarations)";
        base[3].title = "Facturation + modèle de contrat + preuves";
      }
      if(profile==="family"){
        base[1].title = "Démarches école / santé / justificatifs famille";
        base[2].title = "Couverture santé + attestations";
      }
      return base;
    }

    function checklistItems(country, profile){
      const common = [
        { code:"ID", title:"Regrouper ID + scans propres", desc:"Version PDF lisible, date, taille correcte." },
        { code:"ADDR", title:"Justificatif de domicile", desc:"Facture/attestation, cohérente avec ton nom." },
        { code:"CAL", title:"Mettre les deadlines au calendrier", desc:"Rappels 30/14/7 jours." },
        { code:"DOCS", title:"Créer dossier cloud", desc:"Sous-dossiers : preuves, formulaires, reçus." }
      ];
      if(country==="FR") common.push({ code:"FR", title:"Compte FranceConnect / identifiants", desc:"Tu vas en avoir besoin. Sans ça, tu perds du temps." });
      if(country==="US") common.push({ code:"US", title:"Adresse + preuves + setup administratif", desc:"Beaucoup de démarches demandent la même preuve." });
      if(profile==="freelance") common.push({ code:"BIZ", title:"Modèle facture + contrat", desc:"Tu veux standardiser, pas improviser." });
      return common;
    }

    // ---------- Router ----------
    function go(path){
      state.route = path;
      location.hash = "#" + path;
      renderRoute();
    }
    function renderRoute(){
      // hide all pages
      document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
      const id = routes[state.route] || routes["/"];
      document.getElementById(id).classList.add("active");

      // update topbar dashboard button
      document.getElementById("btnDashboard").style.display = state.authed ? "inline-flex" : "none";

      // if not authed and trying dashboard, open login
      if(state.route==="/dashboard" && !state.authed){
        openAuthModal("login");
        go("/");
        toast("Connecte-toi pour accéder au dashboard.");
      }

      // nav link highlight (simple)
      document.querySelectorAll("[data-route]").forEach(a=>{
        const r = a.getAttribute("data-route");
        a.style.color = (r===state.route) ? "var(--text)" : "var(--muted)";
      });
    }

    window.addEventListener("hashchange", ()=>{
      const p = (location.hash || "#/").slice(1);
      state.route = routes[p] ? p : "/";
      renderRoute();
    });

    // ---------- UI helpers ----------
    function toast(msg){
      const el = document.createElement("div");
      el.textContent = msg;
      el.style.position="fixed";
      el.style.left="18px";
      el.style.bottom="18px";
      el.style.padding="10px 12px";
      el.style.border="1px solid var(--line)";
      el.style.borderRadius="14px";
      el.style.background="rgba(17,24,39,.92)";
      el.style.boxShadow="var(--shadow)";
      el.style.color="var(--text)";
      el.style.zIndex="200";
      document.body.appendChild(el);
      setTimeout(()=>{ el.style.opacity="0"; el.style.transition="opacity .25s"; }, 1800);
      setTimeout(()=>{ el.remove(); }, 2200);
    }

    function updateDashLabel(){
      const lbl = `${labelCountry(state.user.country)} • ${labelProfile(state.user.profile)}`;
      document.getElementById("dashLabel").textContent = lbl;
      document.getElementById("profileCountry").value = state.user.country;
      document.getElementById("profileType").value = state.user.profile;
      document.getElementById("profileArrive").value = state.user.arrive || "";
      document.getElementById("profileGoal").value = state.user.goal || "";
      document.getElementById("profileNotes").value = state.user.notes || "";
    }

    function updatePlanUI(){
      const planName = ({free:"Gratuit",plus:"Plus",pro:"Pro"})[state.plan] || state.plan;
      document.getElementById("planName").textContent = planName;

      const hint = document.getElementById("planHint");
      if(state.plan==="free") hint.textContent = "Certaines fonctions sont verrouillées.";
      if(state.plan==="plus") hint.textContent = "Alertes + exports activés.";
      if(state.plan==="pro") hint.textContent = "Multi-pays + historique + exports illimités.";

      // Paywalls toggles
      const locked = (state.plan==="free");
      document.getElementById("alertsPaywall").style.display = locked ? "block":"none";
      document.getElementById("docsPaywall").style.display = locked ? "block":"none";
      document.getElementById("btnUnlockFromAlert").style.display = locked ? "inline-flex":"none";

      // Export buttons
      document.getElementById("btnExport1").onclick = ()=> {
        if(state.plan==="free") return openPaywall("Exporter Word/PDF");
        toast("Export lancé (démo).");
      };
      document.getElementById("btnExport2").onclick = ()=> {
        if(state.plan==="free") return openPaywall("Exporter Word/PDF");
        toast("Export lancé (démo).");
      };
    }

    function openPaywall(feature){
      openModal("Fonction payante",
        `<div class="paywall">
          <h4>${escapeHtml(feature)}</h4>
          <p>Cette fonctionnalité est disponible en <strong>Plus</strong> ou <strong>Pro</strong>. Si tu veux scaler un produit, c’est ce que tu monétises.</p>
          <button class="btn btn-primary" data-modal-action="goPricing">Voir les plans</button>
        </div>`,
        `<button class="btn" data-modal-action="close">Fermer</button>`
      );
      document.querySelector('[data-modal-action="goPricing"]').onclick = ()=>{ closeModal(); go("/pricing"); };
    }

    function escapeHtml(s){
      return (s||"").replaceAll("&","&amp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll('"',"&quot;").replaceAll("'","&#039;");
    }

    // ---------- Dashboard rendering ----------
    function renderTimeline(){
      const items = sampleTimeline(state.user.country, state.user.profile);

      const make = (boxId) => {
        const box = document.getElementById(boxId);
        box.innerHTML = "";
        items.forEach(it=>{
          const el = document.createElement("div");
          el.className = "t-item";
          el.innerHTML = `
            <div class="status ${it.status}"></div>
            <div style="flex:1;">
              <div class="head">
                <div class="when">${it.when}</div>
                <div class="prio ${it.prio}">${it.prio==="high" ? "critique" : it.prio==="med" ? "important" : "confort"}</div>
              </div>
              <div style="font-weight:900; margin-top:6px;">${escapeHtml(it.title)}</div>
              <div class="desc">${escapeHtml(it.desc)}</div>
              <div class="t-actions">
                <button class="btn btn-mini" onclick="toast('Détails (démo).')">Détails</button>
                <button class="btn btn-mini btn-primary" onclick="${state.plan==='free' ? "openPaywall('Générer documents')" : "toast('Document généré (démo).')"}">Générer doc</button>
              </div>
            </div>
          `;
          box.appendChild(el);
        });
      };

      make("timelineBox");
      make("timelineBox2");
    }

    function renderChecklist(){
      const box = document.getElementById("checklistBox");
      box.innerHTML = "";
      checklistItems(state.user.country, state.user.profile).forEach(ci=>{
        const el = document.createElement("div");
        el.className = "item";
        el.innerHTML = `
          <div class="badge">${escapeHtml(ci.code)}</div>
          <div>
            <p class="title">${escapeHtml(ci.title)}</p>
            <p class="desc">${escapeHtml(ci.desc)}</p>
          </div>
        `;
        box.appendChild(el);
      });
    }

    function setDashView(view){
      state.dashView = view;
      document.querySelectorAll(".side .navbtn").forEach(b=> b.classList.toggle("active", b.getAttribute("data-dash")===view));
      document.querySelectorAll(".dashpanel").forEach(p=> p.style.display="none");

      // Instead of separate page, we use dash panels + shared tab content
      // We'll show relevant panel and keep tabs visible
      if(view==="overview"){
        // show actions tab panel by default
        document.getElementById("tab-actions").style.display = "block";
        document.getElementById("tab-alerts").style.display = "none";
        document.getElementById("tab-risk").style.display = "none";
        setTab("actions");
      } else {
        document.getElementById("tab-actions").style.display = "none";
        document.getElementById("tab-alerts").style.display = "none";
        document.getElementById("tab-risk").style.display = "none";
        document.getElementById("dash-"+view).style.display = "block";
      }
    }

    function setTab(tab){
      state.tab = tab;
      document.querySelectorAll(".tab").forEach(t=> t.classList.toggle("active", t.getAttribute("data-tab")===tab));
      document.querySelectorAll(".tabpanel").forEach(p=> p.style.display="none");
      document.getElementById("tab-"+tab).style.display="block";
    }

    // ---------- Modals (auth) ----------
    function openModal(title, bodyHtml, footHtml){
      document.getElementById("modalTitle").textContent = title;
      document.getElementById("modalBody").innerHTML = bodyHtml;
      document.getElementById("modalFoot").innerHTML = footHtml || "";
      document.getElementById("modalBackdrop").classList.add("active");
      document.getElementById("modalBackdrop").setAttribute("aria-hidden","false");

      // Close handler
      document.querySelectorAll('[data-modal-action="close"]').forEach(b=> b.onclick = closeModal);
    }
    function closeModal(){
      document.getElementById("modalBackdrop").classList.remove("active");
      document.getElementById("modalBackdrop").setAttribute("aria-hidden","true");
    }

    function openAuthModal(mode){
      const isLogin = (mode==="login");
      const title = isLogin ? "Connexion" : "Créer un compte";
      const body = `
        <div class="field">
          <label>Email</label>
          <input id="authEmail" type="email" placeholder="toi@exemple.com" value="${escapeHtml(state.user.email || "")}"/>
        </div>
        <div class="field">
          <label>Mot de passe</label>
          <input id="authPass" type="password" placeholder="••••••••"/>
        </div>
        <div class="small">
          ${isLogin ? `Pas de compte ? <a class="mutelink" href="#" id="switchAuth">Créer un compte</a>` :
                      `Déjà un compte ? <a class="mutelink" href="#" id="switchAuth">Se connecter</a>`}
        </div>
      `;
      const foot = `
        <button class="btn" data-modal-action="close">Annuler</button>
        <button class="btn btn-primary" id="authSubmit">${isLogin ? "Se connecter" : "Créer"}</button>
      `;
      openModal(title, body, foot);

      document.getElementById("switchAuth").onclick = (e)=>{ e.preventDefault(); openAuthModal(isLogin ? "signup":"login"); };
      document.getElementById("authSubmit").onclick = ()=>{
        const email = document.getElementById("authEmail").value.trim();
        if(!email){ toast("Entre un email."); return; }
        state.authed = true;
        state.user.email = email;
        closeModal();
        toast(isLogin ? "Connecté (démo)." : "Compte créé (démo).");
        document.getElementById("btnDashboard").style.display = "inline-flex";
        go("/dashboard");
        hydrateDashboard();
      };
    }

    // ---------- Guides viewer ----------
    const guideContent = {
      "fr-student": {
        title: "Étudiant international en France",
        bullets: [
          "Timeline : arrivée, inscription, titre de séjour, assurance, banque.",
          "Pièces : passeport, attestation scolarité, domicile, ressources.",
          "Risques : délais, preuve domicile, assurance incomplète.",
          "Pourquoi BureauOS : transforme ça en checklist + relances + docs."
        ]
      },
      "us-expat": {
        title: "S’installer aux États-Unis (expat)",
        bullets: [
          "Setup : adresse, SSN/ITIN, banque, assurance.",
          "Délais : certains documents doivent être demandés tôt.",
          "Risques : fiscalité, couverture santé, preuves d’identité.",
          "Pourquoi BureauOS : plan + preuves + suivi dans le temps."
        ]
      },
      "x-freelance": {
        title: "Freelance transfrontalier",
        bullets: [
          "Statut : obligations déclaratives selon pays.",
          "Docs : contrat, facture, preuve de service.",
          "Risques : TVA/impôts, résidence fiscale.",
          "Pourquoi BureauOS : radar à risques + dossier exportable."
        ]
      },
      "family": {
        title: "Famille : démarches récurrentes",
        bullets: [
          "Santé : couverture + attestations.",
          "École : inscriptions, justificatifs, calendriers.",
          "Routines : mises à jour et renouvellements.",
          "Pourquoi BureauOS : mémoire + relances + documents."
        ]
      }
    };

    function showGuide(key){
      const g = guideContent[key];
      const box = document.getElementById("guideViewer");
      if(!g) return;
      box.innerHTML = `
        <strong>${escapeHtml(g.title)}</strong><br>
        <ul style="margin:10px 0 0; padding-left:18px; color:var(--muted);">
          ${g.bullets.map(b=>`<li style="margin:6px 0;">${escapeHtml(b)}</li>`).join("")}
        </ul>
        <div class="divider"></div>
        <button class="btn btn-primary" data-cta="signup">Activer le suivi personnalisé</button>
      `;
      box.querySelector('[data-cta="signup"]').onclick = ()=> openAuthModal("signup");
    }

    // ---------- Checkout (mock) ----------
    function gotoCheckout(plan){
      state.checkoutPlan = plan;
      const planLabel = plan==="pro" ? "Pro" : "Plus";
      const price = plan==="pro" ? "24,90€ / mois" : "9,90€ / mois";
      document.getElementById("checkoutPlan").textContent = planLabel;
      document.getElementById("checkoutPrice").textContent = price;
      go("/checkout");
    }

    function doPayment(){
      const email = (document.getElementById("payEmail").value || state.user.email || "").trim();
      if(!email){ toast("Entre un email (démo)."); return; }
      state.user.email = email;
      state.authed = true;
      state.plan = state.checkoutPlan || "plus";
      toast("Paiement simulé : plan activé.");
      go("/dashboard");
      hydrateDashboard();
    }

    // ---------- Dashboard hydrate ----------
    function hydrateDashboard(){
      updateDashLabel();
      renderTimeline();
      renderChecklist();
      updatePlanUI();
      setDashView("overview");
      setTab("actions");
    }

    // ---------- Bindings ----------
    document.getElementById("btnLogin").onclick = ()=> openAuthModal("login");
    document.getElementById("btnSignup").onclick = ()=> openAuthModal("signup");
    document.getElementById("btnCloseModal").onclick = closeModal;
    document.getElementById("modalBackdrop").addEventListener("click", (e)=>{
      if(e.target.id==="modalBackdrop") closeModal();
    });

    document.getElementById("btnDashboard").onclick = ()=> { go("/dashboard"); hydrateDashboard(); };

    document.querySelectorAll("[data-goto]").forEach(b=>{
      b.onclick = ()=> go(b.getAttribute("data-goto"));
    });

    document.querySelectorAll("[data-cta='signup']").forEach(b=>{
      b.onclick = ()=> openAuthModal("signup");
    });

    document.querySelectorAll("[data-cta='checkout']").forEach(b=>{
      b.onclick = ()=> {
        const plan = b.getAttribute("data-plan") || "plus";
        gotoCheckout(plan);
      };
    });

    document.getElementById("btnPay").onclick = doPayment;

    document.getElementById("btnTryDemo").onclick = ()=>{
      // demo access without login? no, push signup/login
      openAuthModal("signup");
    };

    document.getElementById("btnGoDash2").onclick = ()=>{
      if(!state.authed) return openAuthModal("signup");
      go("/dashboard"); hydrateDashboard();
    };

    document.getElementById("btnBuildPlan").onclick = ()=>{
      const c = document.getElementById("quickCountry").value;
      const p = document.getElementById("quickProfile").value;
      state.user.country = c;
      state.user.profile = p;
      document.getElementById("quickOutput").innerHTML =
        `<strong>Timeline générée (démo) :</strong><br>
         ${labelCountry(c)} • ${labelProfile(p)}<br>
         <span class="small">Prochaine étape logique : accéder au dashboard pour voir la timeline + les docs.</span>`;
      toast("Timeline (démo) prête.");
    };

    document.querySelectorAll("[data-open-guide]").forEach(b=>{
      b.onclick = ()=> showGuide(b.getAttribute("data-open-guide"));
    });

    // Dashboard side nav
    document.querySelectorAll(".side .navbtn").forEach(b=>{
      b.onclick = ()=> setDashView(b.getAttribute("data-dash"));
    });

    // Dashboard tabs
    document.querySelectorAll(".tab").forEach(b=>{
      b.onclick = ()=> setTab(b.getAttribute("data-tab"));
    });

    // Paywall from alert
    document.getElementById("btnUnlockFromAlert").onclick = ()=> go("/pricing");

    // Logout
    document.getElementById("btnLogout").onclick = ()=>{
      state.authed = false;
      state.plan = "free";
      toast("Déconnecté.");
      go("/");
      renderRoute();
    };

    // Profile save
    document.getElementById("btnSaveProfile").onclick = ()=>{
      state.user.country = document.getElementById("profileCountry").value;
      state.user.profile = document.getElementById("profileType").value;
      state.user.arrive = document.getElementById("profileArrive").value;
      state.user.goal = document.getElementById("profileGoal").value.trim();
      state.user.notes = document.getElementById("profileNotes").value.trim();
      toast("Profil enregistré (démo).");
      updateDashLabel();
    };
    document.getElementById("btnRebuild").onclick = ()=>{
      updateDashLabel();
      renderTimeline();
      renderChecklist();
      toast("Timeline recalculée (démo).");
    };

    // Topbar route links
    document.querySelectorAll("[data-route]").forEach(a=>{
      a.addEventListener("click", (e)=>{
        e.preventDefault();
        go(a.getAttribute("data-route"));
      });
    });

    // Initialize route from hash
    (function init(){
      const p = (location.hash || "#/").slice(1);
      state.route = routes[p] ? p : "/";
      renderRoute();
      // default profile in dashboard selects
      hydrateDashboard(); // ok even if hidden
      // But keep user not authed and route home
      if(!state.authed) go("/");
    })();
  </script>
</body>
</html>
