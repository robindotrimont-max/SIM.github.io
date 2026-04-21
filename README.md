<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title> SIM.IO — Calculez la rentabilité de votre batterie domestique</title>
<style>
  :root {
    --bg: #f7f6f1;
    --surface: #ffffff;
    --ink: #0f2a1d;
    --ink-soft: #4a5c54;
    --accent: #c6f36c;
    --accent-dark: #9ecd3d;
    --line: #e4e2d9;
    --good: #2e7d54;
    --bad: #c0463a;
    --radius: 18px;
    --shadow: 0 10px 30px rgba(15, 42, 29, 0.06);
  }
  * { box-sizing: border-box; }
  html, body { margin: 0; padding: 0; }
  body {
    font-family: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    background: var(--bg);
    color: var(--ink);
    -webkit-font-smoothing: antialiased;
    line-height: 1.5;
  }
  a { color: inherit; }
  h1, h2, h3 { font-family: "Georgia", "Times New Roman", serif; font-weight: 500; letter-spacing: -0.01em; }
  h1 { font-size: clamp(2rem, 4.2vw, 3.4rem); line-height: 1.1; margin: 0 0 1rem; }
  h2 { font-size: clamp(1.5rem, 2.8vw, 2rem); margin: 0 0 0.6rem; }
  h3 { font-size: 1.1rem; margin: 0 0 0.4rem; font-family: "Inter", sans-serif; font-weight: 600; letter-spacing: 0; }

  /* ---------- Header ---------- */
  header {
    position: sticky; top: 0; z-index: 10;
    background: rgba(247, 246, 241, 0.88);
    backdrop-filter: saturate(140%) blur(8px);
    border-bottom: 1px solid var(--line);
  }
  .nav {
    max-width: 1200px; margin: 0 auto; padding: 14px 24px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .brand { display: flex; align-items: center; gap: 10px; font-weight: 700; letter-spacing: -0.02em; font-size: 1.1rem; }
  .brand-dot {
    width: 26px; height: 26px; border-radius: 50%;
    background: var(--ink);
    position: relative;
  }
  .brand-dot::after {
    content: ""; position: absolute; inset: 4px; border-radius: 50%;
    background: var(--accent);
  }
  .nav-links { display: none; gap: 28px; font-size: 0.95rem; color: var(--ink-soft); }
  .nav-links a { text-decoration: none; }
  .nav-links a:hover { color: var(--ink); }
  @media(min-width: 860px) { .nav-links { display: flex; } }
  .btn {
    border: none; cursor: pointer; border-radius: 999px;
    padding: 12px 22px; font-weight: 600; font-size: 0.95rem;
    transition: transform 0.15s ease, background 0.15s ease;
  }
  .btn-primary { background: var(--ink); color: #fff; }
  .btn-primary:hover { background: #183a2b; }
  .btn-ghost { background: transparent; color: var(--ink); }

  /* ---------- Hero ---------- */
  .hero {
    max-width: 1200px; margin: 0 auto; padding: 72px 24px 32px;
    display: grid; grid-template-columns: 1fr; gap: 36px; align-items: center;
  }
  @media(min-width: 980px) {
    .hero { grid-template-columns: 1.1fr 0.9fr; padding-top: 96px; padding-bottom: 56px; }
  }
  .eyebrow {
    display: inline-block; padding: 6px 14px; border-radius: 999px;
    background: var(--accent); color: var(--ink); font-size: 0.82rem; font-weight: 600;
    margin-bottom: 18px;
  }
  .hero p.lead { font-size: 1.1rem; color: var(--ink-soft); max-width: 560px; margin: 0 0 28px; }
  .hero-cta { display: flex; gap: 12px; flex-wrap: wrap; }
  .hero-visual {
    background: linear-gradient(135deg, #11362a 0%, #1a4b3a 60%, #256a52 100%);
    border-radius: 28px; padding: 32px; color: #fff; position: relative;
    min-height: 320px; overflow: hidden;
  }
  .hero-visual h3 { color: #fff; font-size: 1rem; opacity: 0.85; margin-bottom: 8px; }
  .hero-visual .big { font-family: Georgia, serif; font-size: 2.2rem; letter-spacing: -0.02em; }
  .hero-visual .sun {
    position: absolute; right: -40px; top: -40px; width: 200px; height: 200px; border-radius: 50%;
    background: radial-gradient(circle at 30% 30%, var(--accent) 0%, rgba(198, 243, 108, 0) 70%);
  }
  .hero-stats { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-top: 24px; }
  .stat {
    background: rgba(255, 255, 255, 0.08); border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 14px; padding: 14px;
  }
  .stat small { opacity: 0.7; font-size: 0.78rem; }
  .stat b { display: block; font-family: Georgia, serif; font-size: 1.5rem; margin-top: 4px; }

  /* ---------- Simulator ---------- */
  .simulator {
    max-width: 1200px; margin: 0 auto; padding: 32px 24px 80px;
    display: grid; grid-template-columns: 1fr; gap: 28px;
  }
  @media(min-width: 980px) {
    .simulator { grid-template-columns: 1.15fr 0.85fr; align-items: start; }
  }
  .card {
    background: var(--surface); border: 1px solid var(--line);
    border-radius: var(--radius); box-shadow: var(--shadow);
    padding: 28px;
  }
  .card + .card { margin-top: 20px; }
  .section-title { display: flex; align-items: center; gap: 10px; margin-bottom: 18px; }
  .step-num {
    width: 28px; height: 28px; border-radius: 50%; background: var(--accent);
    color: var(--ink); font-weight: 700; font-size: 0.9rem;
    display: inline-flex; align-items: center; justify-content: center;
  }
  .grid {
    display: grid; grid-template-columns: 1fr 1fr; gap: 14px;
  }
  .grid .full { grid-column: 1 / -1; }
  label.field { display: flex; flex-direction: column; gap: 6px; font-size: 0.9rem; color: var(--ink-soft); }
  label.field span.hint { font-size: 0.78rem; color: #8a9892; font-weight: 400; }
  input[type="number"], select, input[type="text"] {
    width: 100%; padding: 12px 14px; font-size: 1rem;
    border: 1px solid var(--line); border-radius: 10px; background: #fafaf6;
    color: var(--ink); transition: border 0.15s ease, background 0.15s ease;
    font-family: inherit;
  }
  input[type="number"]:focus, select:focus, input[type="text"]:focus {
    outline: none; border-color: var(--ink); background: #fff;
  }
  input[type="range"] { width: 100%; accent-color: var(--ink); }
  .toggle-group { display: flex; gap: 8px; flex-wrap: wrap; }
  .chip {
    padding: 10px 16px; border-radius: 999px; border: 1px solid var(--line);
    background: #fafaf6; cursor: pointer; font-size: 0.92rem; font-weight: 500;
    transition: all 0.15s ease;
  }
  .chip.active { background: var(--ink); color: #fff; border-color: var(--ink); }
  .chip:hover:not(.active) { border-color: var(--ink); }

  .collapsible-head {
    display: flex; justify-content: space-between; align-items: center; cursor: pointer;
    padding: 6px 0;
  }
  .collapsible-head .chev { transition: transform 0.2s ease; color: var(--ink-soft); }
  .collapsible.open .chev { transform: rotate(180deg); }
  .collapsible-body { display: none; padding-top: 16px; }
  .collapsible.open .collapsible-body { display: block; }

  /* ---------- Result panel ---------- */
  .result {
    position: sticky; top: 88px;
    background: var(--ink); color: #fff; border-radius: var(--radius);
    padding: 28px; overflow: hidden; position: relative;
  }
  .result .accent-glow {
    position: absolute; top: -80px; right: -80px; width: 280px; height: 280px;
    background: radial-gradient(circle, rgba(198, 243, 108, 0.28), transparent 70%);
    pointer-events: none;
  }
  .result h2 { color: #fff; }
  .result small { color: rgba(255, 255, 255, 0.65); }
  .kpi { margin: 22px 0; }
  .kpi .big {
    font-family: Georgia, serif; font-size: 3rem; line-height: 1; letter-spacing: -0.02em;
    color: var(--accent);
  }
  .kpi .label { font-size: 0.88rem; opacity: 0.8; margin-top: 6px; }
  .kpi-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 6px; }
  .kpi-row .mini {
    background: rgba(255, 255, 255, 0.06); border-radius: 12px; padding: 14px;
  }
  .kpi-row .mini b { display: block; font-family: Georgia, serif; font-size: 1.4rem; margin-top: 4px; }
  .kpi-row .mini small { font-size: 0.75rem; }

  .bar-chart {
    margin-top: 22px; background: rgba(255, 255, 255, 0.06); border-radius: 14px; padding: 18px;
  }
  .bar-row { margin: 14px 0; }
  .bar-row .label-row { display: flex; justify-content: space-between; font-size: 0.82rem; margin-bottom: 6px; opacity: 0.9; }
  .bar-track { height: 12px; background: rgba(255, 255, 255, 0.1); border-radius: 999px; overflow: hidden; }
  .bar-fill { height: 100%; border-radius: 999px; transition: width 0.6s ease; }
  .bar-before { background: #8aa29a; }
  .bar-after { background: linear-gradient(90deg, var(--accent), var(--accent-dark)); }

  .verdict {
    margin-top: 22px; padding: 16px; border-radius: 14px;
    background: rgba(198, 243, 108, 0.12); border: 1px solid rgba(198, 243, 108, 0.25);
    font-size: 0.92rem; line-height: 1.5;
  }
  .verdict.bad { background: rgba(192, 70, 58, 0.15); border-color: rgba(192, 70, 58, 0.35); }
  .verdict b { color: var(--accent); }
  .verdict.bad b { color: #ffb4ad; }

  /* ---------- Footer ---------- */
  footer {
    background: var(--ink); color: rgba(255, 255, 255, 0.7); padding: 40px 24px; margin-top: 40px;
  }
  .foot-wrap {
    max-width: 1200px; margin: 0 auto;
    display: flex; flex-wrap: wrap; gap: 20px; justify-content: space-between; align-items: center;
    font-size: 0.88rem;
  }

  .benefits {
    max-width: 1200px; margin: 0 auto; padding: 40px 24px;
    display: grid; grid-template-columns: 1fr; gap: 20px;
  }
  @media(min-width: 860px) { .benefits { grid-template-columns: repeat(3, 1fr); } }
  .benefit { padding: 24px; border: 1px solid var(--line); border-radius: var(--radius); background: var(--surface); }
  .benefit .ic {
    width: 40px; height: 40px; border-radius: 12px; background: var(--accent);
    display: inline-flex; align-items: center; justify-content: center; margin-bottom: 14px; font-size: 1.1rem;
  }

  .disclaimer { font-size: 0.78rem; color: var(--ink-soft); margin-top: 18px; line-height: 1.5; }
</style>
</head>
<body>

<header>
  <div class="nav">
    <div class="brand">
      <div class="brand-dot"></div>
      <span>SIM.io</span>
    </div>
    <nav class="nav-links">
      <a href="#simulateur">Simulateur</a>
      <a href="#comment">Comment ça marche</a>
      <a href="#avantages">Avantages</a>
    </nav>
    <button class="btn btn-primary" onclick="document.getElementById('simulateur').scrollIntoView({behavior:'smooth'})">Simuler</button>
  </div>
</header>

<!-- HERO -->
<section class="hero">
  <div>
    <span class="eyebrow">Simulateur de rentabilité — Belgique</span>
    <h1>Une batterie domestique, est-ce vraiment rentable pour vous ?</h1>
    <p class="lead">
      Estimez en 2 minutes les économies qu'une batterie peut générer dans votre foyer,
      et en combien d'années elle sera amortie. Résultat instantané, sans e-mail.
    </p>
    <div class="hero-cta">
      <button class="btn btn-primary" onclick="document.getElementById('simulateur').scrollIntoView({behavior:'smooth'})">Lancer la simulation →</button>
      <a class="btn btn-ghost" href="#comment">Comment on calcule ?</a>
    </div>
  </div>
  <div class="hero-visual">
    <div class="sun"></div>
    <h3>Aperçu typique d'un foyer belge</h3>
    <div class="big">30 → 75 %</div>
    <p style="opacity:0.85; margin: 8px 0 0;">Taux d'autoconsommation moyen, avant et après l'installation d'une batterie.</p>
    <div class="hero-stats">
      <div class="stat"><small>Prix moyen du kWh</small><b>0,35 €</b></div>
      <div class="stat"><small>Tarif d'injection</small><b>0,05 €</b></div>
      <div class="stat"><small>Coût moyen / kWh batterie</small><b>~900 €</b></div>
      <div class="stat"><small>Durée de vie</small><b>10–15 ans</b></div>
    </div>
  </div>
</section>

<!-- SIMULATOR -->
<section class="simulator" id="simulateur">
  <!-- FORM -->
  <div>
    <!-- STEP 1: Profile -->
    <div class="card">
      <div class="section-title"><span class="step-num">1</span><h2>Votre foyer</h2></div>
      <div class="grid">
        <label class="field">Type de logement
          <select id="type_logement">
            <option value="maison">Maison individuelle</option>
            <option value="appartement">Appartement</option>
            <option value="autre">Autre</option>
          </select>
        </label>
        <label class="field">Nombre de personnes
          <input type="number" id="personnes" value="4" min="1" max="12" />
        </label>
        <label class="field full">Région
          <div class="toggle-group" id="region_group">
            <div class="chip active" data-v="wallonie">Wallonie</div>
            <div class="chip" data-v="flandre">Flandre</div>
            <div class="chip" data-v="bruxelles">Bruxelles</div>
          </div>
        </label>
      </div>
    </div>

    <!-- STEP 2: Solar panels -->
    <div class="card">
      <div class="section-title"><span class="step-num">2</span><h2>Vos panneaux solaires</h2></div>
      <div class="toggle-group" id="pv_group" style="margin-bottom:14px;">
        <div class="chip active" data-v="oui">J'ai déjà des panneaux</div>
        <div class="chip" data-v="non">Je n'en ai pas</div>
      </div>
      <div id="pv_details">
        <div class="grid">
          <label class="field">Production connue ?
            <select id="prod_mode">
              <option value="annee">Oui — en kWh / an</option>
              <option value="mois">Oui — en kWh / mois (moyenne)</option>
              <option value="kwc">Non — j'ai juste la puissance (kWc)</option>
            </select>
          </label>
          <label class="field" id="prod_val_wrap">Production <span class="hint" id="prod_unit_hint">(kWh/an)</span>
            <input type="number" id="prod_val" value="4500" min="0" />
          </label>
          <label class="field full">Puissance installée <span class="hint">(kWc)</span>
            <input type="number" id="puissance_kwc" value="5" min="0" step="0.1" />
          </label>
        </div>
      </div>
    </div>

    <!-- STEP 3: Consumption -->
    <div class="card">
      <div class="section-title"><span class="step-num">3</span><h2>Votre consommation</h2></div>
      <div class="grid">
        <label class="field">Consommation annuelle <span class="hint">(kWh/an)</span>
          <input type="number" id="conso" value="4000" min="500" />
        </label>
        <label class="field">Pic de consommation
          <select id="pic">
            <option value="matin">Matinée (6h–10h)</option>
            <option value="journee">Journée (10h–17h)</option>
            <option value="soir" selected>Soir (17h–23h)</option>
            <option value="nuit">Nuit (23h–6h)</option>
            <option value="uniforme">Répartie / uniforme</option>
          </select>
        </label>
        <label class="field full">
          Part de la consommation en soirée/nuit : <b id="soir_val">60</b> %
          <input type="range" id="part_soir" min="10" max="90" value="60" />
          <span class="hint">Plus cette part est élevée, plus la batterie est utile (elle stocke le surplus solaire pour le restituer hors soleil).</span>
        </label>
      </div>
    </div>

    <!-- STEP 4: Battery + tariffs (advanced) -->
    <div class="card">
      <div class="section-title"><span class="step-num">4</span><h2>La batterie envisagée</h2></div>
      <div class="grid">
        <label class="field">Capacité batterie <span class="hint">(kWh)</span>
          <input type="number" id="bat_capa" value="10" min="1" max="30" step="0.5" />
        </label>
        <label class="field">Coût total installé <span class="hint">(€, pose comprise)</span>
          <input type="number" id="bat_cout" value="9000" min="1000" step="100" />
        </label>
        <label class="field">Prime / subside <span class="hint">(€, si applicable)</span>
          <input type="number" id="bat_prime" value="0" min="0" step="100" />
        </label>
        <label class="field">Durée de vie estimée <span class="hint">(années)</span>
          <input type="number" id="bat_vie" value="12" min="5" max="25" />
        </label>
      </div>
    </div>

    <!-- ADVANCED: tarifs -->
    <div class="card collapsible" id="col_adv">
      <div class="collapsible-head" onclick="document.getElementById('col_adv').classList.toggle('open')">
        <h3>Paramètres avancés — tarifs d'électricité</h3>
        <span class="chev">▼</span>
      </div>
      <div class="collapsible-body">
        <div class="grid">
          <label class="field">Prix d'achat kWh <span class="hint">(€/kWh, moyen)</span>
            <input type="number" id="prix_achat" value="0.35" step="0.01" min="0.05" />
          </label>
          <label class="field">Tarif d'injection <span class="hint">(€/kWh)</span>
            <input type="number" id="prix_inject" value="0.05" step="0.01" min="0" />
          </label>
          <label class="field">Tarif capacitaire annuel évité <span class="hint">(€/an, Flandre surtout)</span>
            <input type="number" id="capacitaire" value="0" min="0" step="10" />
          </label>
          <label class="field">Inflation annuelle du kWh <span class="hint">(% / an)</span>
            <input type="number" id="inflation" value="3" min="0" max="15" step="0.5" />
          </label>
        </div>
        <p class="disclaimer">
          Valeurs pré-remplies basées sur des moyennes belges 2024–2025. Le tarif d'injection dépend de votre fournisseur.
          En Flandre, l'ajout d'une batterie peut réduire le tarif capacitaire (basé sur le pic de puissance mensuel).
        </p>
      </div>
    </div>
  </div>

  <!-- RESULT -->
  <div>
    <div class="result">
      <div class="accent-glow"></div>
      <small>Résultat de la simulation</small>
      <h2>Votre rentabilité</h2>

      <div class="kpi">
        <div class="big" id="roi_val">—</div>
        <div class="label">années avant rentabilisation</div>
      </div>

      <div class="kpi-row">
        <div class="mini">
          <small>Économies / an</small>
          <b id="eco_val">— €</b>
        </div>
        <div class="mini">
          <small>Gain sur la durée de vie</small>
          <b id="gain_val">— €</b>
        </div>
        <div class="mini">
          <small>Capacité conseillée</small>
          <b id="capa_reco">— kWh</b>
        </div>
        <div class="mini">
          <small>Coût net batterie</small>
          <b id="cout_net">— €</b>
        </div>
      </div>

      <div class="bar-chart">
        <h3 style="font-family:Inter,sans-serif;font-size:0.95rem;color:#fff;margin:0 0 10px;">Autoconsommation solaire</h3>
        <div class="bar-row">
          <div class="label-row"><span>Sans batterie</span><span id="auto_before_lbl">30 %</span></div>
          <div class="bar-track"><div class="bar-fill bar-before" id="bar_before" style="width:30%"></div></div>
        </div>
        <div class="bar-row">
          <div class="label-row"><span>Avec batterie</span><span id="auto_after_lbl">75 %</span></div>
          <div class="bar-track"><div class="bar-fill bar-after" id="bar_after" style="width:75%"></div></div>
        </div>
      </div>

      <div class="verdict" id="verdict">
        Renseignez votre situation pour voir le verdict.
      </div>

      <p class="disclaimer" style="color:rgba(255,255,255,0.55); margin-top:16px;">
        Estimation indicative. Les résultats réels dépendent de votre profil de consommation horaire,
        du rendement réel de la batterie et de l'évolution des tarifs.
      </p>
    </div>
  </div>
</section>

<!-- BENEFITS -->
<section class="benefits" id="avantages">
  <div class="benefit">
    <div class="ic">⚡</div>
    <h3>Plus d'autoconsommation</h3>
    <p style="color:var(--ink-soft); margin:0;">Stockez votre surplus solaire en journée et consommez-le le soir plutôt que de l'injecter à bas prix.</p>
  </div>
  <div class="benefit">
    <div class="ic">💰</div>
    <h3>Facture allégée</h3>
    <p style="color:var(--ink-soft); margin:0;">Réduisez vos achats au réseau (~0,35 €/kWh) en valorisant chaque kWh produit.</p>
  </div>
  <div class="benefit">
    <div class="ic">🛡️</div>
    <h3>Moins exposé aux hausses</h3>
    <p style="color:var(--ink-soft); margin:0;">Plus vous consommez votre propre énergie, moins les augmentations du prix du kWh vous affectent.</p>
  </div>
</section>

<!-- HOW -->
<section class="benefits" id="comment" style="padding-top:0;">
  <div class="benefit" style="grid-column:1/-1;">
    <h3>Comment le calcul est fait</h3>
    <p style="color:var(--ink-soft);">
      On part de votre production solaire annuelle (ou on l'estime via la puissance installée — ~950 kWh/kWc/an en Belgique).
      Sans batterie, on considère un taux d'autoconsommation de 30 %. Avec batterie, on recalcule ce taux en fonction
      de votre consommation du soir/nuit et de la capacité choisie, plafonné à ~85 %.
      <br><br>
      <b>Économies annuelles</b> = kWh supplémentaires autoconsommés × (prix d'achat − tarif d'injection),
      + éventuel gain sur le tarif capacitaire. <b>Retour sur investissement</b> = (coût batterie − prime) / économies annuelles,
      ajusté par l'inflation du kWh.
    </p>
  </div>
</section>

<footer>
  <div class="foot-wrap">
    <div class="brand" style="color:#fff;">
      <div class="brand-dot"></div>
      <span>SIM.io</span>
    </div>
    <div>Simulateur indicatif — ne remplace pas un devis d'installateur agréé.</div>
  </div>
</footer>

<script>
/* -------- State + helpers -------- */
const state = {
  region: 'wallonie',
  pv: 'oui',
};
function $(id){ return document.getElementById(id); }
function fmtEUR(x){ return new Intl.NumberFormat('fr-BE',{style:'currency',currency:'EUR',maximumFractionDigits:0}).format(x); }
function fmtNum(x, d=1){ return new Intl.NumberFormat('fr-BE',{maximumFractionDigits:d}).format(x); }

/* -------- Toggle groups -------- */
document.querySelectorAll('#region_group .chip').forEach(c => {
  c.addEventListener('click', () => {
    document.querySelectorAll('#region_group .chip').forEach(x => x.classList.remove('active'));
    c.classList.add('active'); state.region = c.dataset.v; recompute();
  });
});
document.querySelectorAll('#pv_group .chip').forEach(c => {
  c.addEventListener('click', () => {
    document.querySelectorAll('#pv_group .chip').forEach(x => x.classList.remove('active'));
    c.classList.add('active'); state.pv = c.dataset.v;
    $('pv_details').style.display = state.pv === 'oui' ? 'block' : 'none';
    recompute();
  });
});

/* -------- Prod mode -> unit hint -------- */
$('prod_mode').addEventListener('change', () => {
  const m = $('prod_mode').value;
  const hint = $('prod_unit_hint');
  if (m === 'annee') { hint.textContent = '(kWh/an)'; $('prod_val').value = 4500; }
  else if (m === 'mois') { hint.textContent = '(kWh/mois, moyenne)'; $('prod_val').value = 375; }
  else { hint.textContent = '(non utilisé — basé sur kWc)'; }
  recompute();
});

/* -------- Sync soir slider -------- */
$('part_soir').addEventListener('input', e => {
  $('soir_val').textContent = e.target.value;
  recompute();
});

/* -------- Listen to all inputs -------- */
['type_logement','personnes','prod_mode','prod_val','puissance_kwc',
 'conso','pic','bat_capa','bat_cout','bat_prime','bat_vie',
 'prix_achat','prix_inject','capacitaire','inflation']
 .forEach(id => {
   const el = $(id);
   if (!el) return;
   el.addEventListener('input', recompute);
   el.addEventListener('change', recompute);
 });

/* -------- Core calculation -------- */
function computeProductionAnnuelle(){
  if (state.pv !== 'oui') return 0;
  const mode = $('prod_mode').value;
  if (mode === 'annee') return Math.max(0, +$('prod_val').value || 0);
  if (mode === 'mois')  return Math.max(0, (+$('prod_val').value || 0) * 12);
  // kWc -> Belgique ≈ 950 kWh/kWc/an
  return Math.max(0, (+$('puissance_kwc').value || 0) * 950);
}

function recommendCapacity(conso, prod){
  // Règle simple : stocker ≈ la conso moyenne de la soirée/nuit (5-10 kWh typique)
  // On vise couvrir 1/3 de la conso journalière, borné 3–15 kWh
  const dailyConso = conso / 365;
  const soirShare = (+$('part_soir').value || 60) / 100;
  const need = dailyConso * soirShare; // kWh/jour à couvrir
  // Plafonné aussi par le surplus disponible
  const dailyProd = prod / 365;
  const surplus = Math.max(0, dailyProd - dailyConso * (1 - soirShare));
  const reco = Math.min(need, surplus * 1.2);
  return Math.max(3, Math.min(15, Math.round(reco * 2) / 2));
}

function autoconsoAfter(conso, prod, capaBat){
  // Taux de base 30%, remonte selon capacité relative à la conso soir/nuit
  if (prod <= 0) return { before: 0, after: 0 };
  const soirShare = (+$('part_soir').value || 60) / 100;
  const dailyConso = conso / 365;
  const nightNeed = dailyConso * soirShare;         // kWh/jour hors soleil
  const stored = Math.min(capaBat * 0.9, nightNeed); // cycle utile 90%, limité par le besoin
  // kWh autoconso supplémentaires par an grâce à la batterie :
  const daysEffective = 300; // jours avec production utile (~10 mois)
  const extraKwh = stored * daysEffective;
  const baseAuto = 0.30;
  const baseKwh = prod * baseAuto;
  const maxAuto = 0.85;
  const afterKwh = Math.min(prod * maxAuto, baseKwh + extraKwh);
  return {
    before: baseAuto,
    after: afterKwh / prod,
    extraKwh: afterKwh - baseKwh
  };
}

function recompute(){
  const conso = Math.max(500, +$('conso').value || 0);
  const prod  = computeProductionAnnuelle();
  const capaBat = Math.max(0.5, +$('bat_capa').value || 0);
  const coutBat = Math.max(0, +$('bat_cout').value || 0);
  const prime = Math.max(0, +$('bat_prime').value || 0);
  const vieBat = Math.max(1, +$('bat_vie').value || 12);
  const pAchat = Math.max(0.01, +$('prix_achat').value || 0.35);
  const pInj = Math.max(0, +$('prix_inject').value || 0.05);
  const capac = Math.max(0, +$('capacitaire').value || 0);
  const infl = Math.max(0, +$('inflation').value || 0) / 100;

  const ac = autoconsoAfter(conso, prod, capaBat);
  const extraKwh = ac.extraKwh || 0;

  // Économies annuelles : chaque kWh supplémentaire autoconsommé vaut (pAchat - pInj)
  // car on évite de l'acheter ET on renonce à l'injection.
  const ecoAuto = extraKwh * (pAchat - pInj);
  const ecoAnnuelle = ecoAuto + capac;

  // Coût net
  const coutNet = Math.max(0, coutBat - prime);

  // ROI ajusté de l'inflation (progression géométrique simple)
  let roi = null;
  if (ecoAnnuelle > 0) {
    // Résoudre coutNet = eco*(1+infl)^0 + eco*(1+infl)^1 + ... sur n années
    // Approx: somme géométrique. On itère pour trouver n.
    let cum = 0, y = 0;
    while (cum < coutNet && y < 40) {
      cum += ecoAnnuelle * Math.pow(1 + infl, y);
      y += 1;
    }
    roi = cum >= coutNet ? y : null;
  }

  // Gain net sur durée de vie
  let gainTotal = 0;
  for (let y = 0; y < vieBat; y++){
    gainTotal += ecoAnnuelle * Math.pow(1 + infl, y);
  }
  const gainNet = gainTotal - coutNet;

  // Recommandation capacité
  const capaReco = recommendCapacity(conso, prod);

  /* -------- Update UI -------- */
  $('roi_val').textContent = roi ? fmtNum(roi, 0) : '—';
  $('eco_val').textContent = ecoAnnuelle > 0 ? fmtEUR(ecoAnnuelle) : '—';
  $('gain_val').textContent = fmtEUR(gainNet);
  $('capa_reco').textContent = prod > 0 ? fmtNum(capaReco, 1) + ' kWh' : 'N/A';
  $('cout_net').textContent = fmtEUR(coutNet);

  const beforePct = Math.round(ac.before * 100);
  const afterPct = Math.round(ac.after * 100);
  $('bar_before').style.width = beforePct + '%';
  $('bar_after').style.width = afterPct + '%';
  $('auto_before_lbl').textContent = beforePct + ' %';
  $('auto_after_lbl').textContent = afterPct + ' %';

  /* -------- Verdict -------- */
  const v = $('verdict');
  v.classList.remove('bad');
  if (prod <= 0) {
    v.innerHTML = "Sans panneaux solaires, une batterie seule n'est généralement <b>pas rentable</b> : elle ne fait qu'arbitrer jour/nuit sur le réseau, avec des écarts de prix insuffisants en Belgique.";
    v.classList.add('bad');
  } else if (!roi) {
    v.innerHTML = "Avec ces paramètres, la batterie <b>ne se rentabilise pas</b> sur 40 ans. Essayez une capacité plus adaptée, vérifiez le coût d'installation, ou activez la prime.";
    v.classList.add('bad');
  } else if (roi > vieBat) {
    v.innerHTML = `Rentabilisée en <b>${roi} ans</b>, soit <b>au-delà de la durée de vie estimée</b> (${vieBat} ans). La batterie n'est probablement <b>pas rentable</b> dans cette configuration.`;
    v.classList.add('bad');
  } else if (roi <= vieBat * 0.6) {
    v.innerHTML = `Très bonne opération : batterie rentabilisée en <b>${roi} ans</b>, puis ~<b>${vieBat - roi} ans d'économies pures</b>. Gain net estimé : <b>${fmtEUR(gainNet)}</b>.`;
  } else {
    v.innerHTML = `Rentabilité correcte : <b>${roi} ans</b> sur ${vieBat} ans de durée de vie. Gain net estimé : <b>${fmtEUR(gainNet)}</b>.`;
  }
}

recompute();
</script>

</body>
</html>
