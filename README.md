<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>Sopa de letras – Toxicología Ambiental</title>
  <style>
    :root{
      --bg:#0b1220;
      --muted:#9fb0d0;
      --text:#e9f0ff;
      --line:rgba(255,255,255,.14);
      --ok:#20c997;
      --warn:#ffcc66;

      --cell: clamp(28px, 6vw, 42px);
      --gap: clamp(4px, 1.2vw, 8px);
      --radius: 18px;
      --shadow: 0 10px 30px rgba(0,0,0,.35);
    }
    *{box-sizing:border-box;font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,"Helvetica Neue",Arial;}
    body{
      margin:0;
      background:linear-gradient(120deg,#060b16,#0b1220 55%,#091633);
      color:var(--text);
      min-height:100vh;
      -webkit-tap-highlight-color: transparent;
    }
    header{
      padding:18px 14px 12px;
      max-width:1200px;
      margin:0 auto;
      display:flex;
      gap:12px;
      align-items:flex-start;
      justify-content:space-between;
      flex-wrap:wrap;
    }
    header .title{display:flex;flex-direction:column;gap:6px;min-width:260px;flex:1;}
    h1{margin:0;font-size:18px;line-height:1.2;}
    .sub{margin:0;color:var(--muted);font-size:13px;line-height:1.35;}

    .bar{
      display:flex;gap:10px;align-items:center;flex-wrap:wrap;
      justify-content:flex-end;flex:1;min-width:260px;
    }
    button{
      border:1px solid var(--line);
      background:rgba(255,255,255,.06);
      color:var(--text);
      padding:12px 14px;
      border-radius:14px;
      cursor:pointer;
      transition:.15s transform ease,.15s background ease;
      box-shadow:0 6px 18px rgba(0,0,0,.25);
      font-size:14px;
      line-height:1;
      touch-action: manipulation;
      white-space:nowrap;
    }
    button:hover{transform:translateY(-1px);background:rgba(255,255,255,.09);}
    button:active{transform:translateY(0px);}
    .btnGhost{background:transparent;}
    .btnDanger{border-color:rgba(255,120,120,.35);background:rgba(255,120,120,.08);}
    .btnDanger:hover{background:rgba(255,120,120,.12);}

    .wrap{
      max-width:1200px;
      margin:0 auto;
      padding:0 14px 20px;
      display:grid;
      grid-template-columns: 1.15fr .85fr;
      gap:14px;
    }
    @media (max-width: 980px){
      .wrap{grid-template-columns:1fr;}
      button{width:100%;}
      .bar{justify-content:stretch;}
    }

    .card{
      background:linear-gradient(180deg,rgba(255,255,255,.06),rgba(255,255,255,.03));
      border:1px solid rgba(255,255,255,.10);
      border-radius:var(--radius);
      box-shadow:var(--shadow);
      overflow:hidden;
      position:relative;
    }
    .card h2{
      margin:0;
      padding:12px 14px;
      font-size:14px;
      border-bottom:1px solid rgba(255,255,255,.10);
      color:#dfe8ff;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
    }
    .chip{
      font-size:12px;
      color:var(--muted);
      border:1px solid rgba(255,255,255,.14);
      padding:6px 10px;
      border-radius:999px;
      background:rgba(0,0,0,.15);
      white-space:nowrap;
    }

    .gridHead{
      padding:10px 14px;
      border-bottom:1px solid rgba(255,255,255,.10);
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      align-items:center;
      justify-content:space-between;
    }
    .liveSel{
      display:flex;
      gap:8px;
      align-items:center;
      color:var(--muted);
      font-size:13px;
      min-width:220px;
    }
    .liveSel b{color:var(--text)}
    .tips{
      color:var(--muted);
      font-size:12px;
      display:flex;
      gap:8px;
      flex-wrap:wrap;
      align-items:center;
      justify-content:flex-end;
    }
    .kbd{
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      border:1px solid rgba(255,255,255,.18);
      padding:2px 6px;border-radius:8px;background:rgba(0,0,0,.18);color:#dfe8ff;
      font-size:12px;
    }

    .gridWrap{
      padding:14px;
      overflow:auto;
      -webkit-overflow-scrolling: touch;
      touch-action: none;
    }
    .grid{
      display:grid;
      grid-template-columns: repeat(var(--n), var(--cell));
      gap: var(--gap);
      user-select:none;
      width:max-content;
      margin:0 auto;
      position:relative;
    }
    .cell{
      width:var(--cell);
      height:var(--cell);
      display:flex;align-items:center;justify-content:center;
      border-radius:12px;
      border:1px solid rgba(255,255,255,.12);
      background:linear-gradient(180deg,rgba(255,255,255,.06),rgba(255,255,255,.02));
      box-shadow:0 2px 10px rgba(0,0,0,.25);
      cursor:pointer;
      font-weight:800;
      letter-spacing:.4px;
      font-size: clamp(12px, 3.5vw, 16px);
      touch-action: none;
    }
    .cell.alt{background:linear-gradient(180deg,rgba(255,255,255,.05),rgba(255,255,255,.015));}
    .cell.sel{outline:2px solid rgba(255,255,255,.35); background:rgba(255,255,255,.12);}
    .cell.found{background:rgba(32,201,151,.22); border-color:rgba(32,201,151,.55);}
    .cell.wrongFlash{animation:shake .25s ease;}
    @keyframes shake{0%{transform:translateX(0)}25%{transform:translateX(-2px)}50%{transform:translateX(2px)}75%{transform:translateX(-1px)}100%{transform:translateX(0)}}

    .selLine{
      position:absolute;
      pointer-events:none;
      left:0;top:0;
      width:100%;height:100%;
      z-index:2;
    }
    .selLine path{
      stroke: rgba(255,255,255,.55);
      stroke-width: 10;
      stroke-linecap: round;
      fill: none;
      filter: drop-shadow(0 6px 12px rgba(0,0,0,.35));
    }
    .selLine.ok path{stroke: rgba(32,201,151,.9);}

    .status{
      display:flex;gap:10px;flex-wrap:wrap;align-items:center;justify-content:space-between;
      padding:12px 14px;
      border-top:1px solid rgba(255,255,255,.10);
      color:var(--muted);
      font-size:13px;
    }
    .status b{color:var(--text)}
    .progress{
      display:flex;
      gap:8px;
      align-items:center;
      flex-wrap:wrap;
    }
    .meter{
      width:160px;height:8px;border-radius:999px;
      background:rgba(255,255,255,.10);
      overflow:hidden;
      border:1px solid rgba(255,255,255,.10);
    }
    .meter > div{
      height:100%;
      width:0%;
      background:rgba(32,201,151,.75);
      transition:.25s width ease;
    }

    .panel{padding:12px 14px 14px;display:flex;flex-direction:column;gap:12px;}
    .hint{color:var(--muted);font-size:13px;line-height:1.4;}
    .qa{display:flex;flex-direction:column;gap:10px;}
    .row{
      padding:12px;border-radius:16px;border:1px solid rgba(255,255,255,.10);
      background:rgba(0,0,0,.12);
      display:grid;grid-template-columns: 1fr;gap:10px;
    }
    .q{font-size:13px;color:#dbe6ff;line-height:1.35;}
    .a{
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      font-size:13px;letter-spacing:.6px;
      padding:10px 10px;border-radius:14px;
      border:1px dashed rgba(255,255,255,.20);
      background:rgba(255,255,255,.04);
      display:flex;align-items:center;justify-content:space-between;gap:10px;
    }
    .tag{font-size:11px;color:var(--muted);border:1px solid rgba(255,255,255,.14);padding:4px 8px;border-radius:999px;white-space:nowrap;}
    .a.ok{border-style:solid;border-color:rgba(32,201,151,.55);background:rgba(32,201,151,.12);}

    .rowActions{display:flex;gap:8px;flex-wrap:wrap;}
    .miniBtn{padding:9px 10px;font-size:13px;border-radius:12px;flex:1;min-width:120px;}
    .miniBtn.warn{border-color:rgba(255,204,102,.35);background:rgba(255,204,102,.10);}
    .miniBtn.warn:hover{background:rgba(255,204,102,.14);}

    /* ===== Modal inicio ===== */
    .modalBack{
      position:fixed; inset:0;
      background:rgba(0,0,0,.55);
      display:flex;align-items:center;justify-content:center;
      padding:14px;
      z-index:9999;
    }
    .modal{
      width:min(520px, 100%);
      background:linear-gradient(180deg,rgba(255,255,255,.08),rgba(255,255,255,.04));
      border:1px solid rgba(255,255,255,.12);
      border-radius:22px;
      box-shadow:0 20px 60px rgba(0,0,0,.5);
      padding:16px;
    }
    .modal h3{margin:0 0 6px 0;font-size:16px;}
    .modal p{margin:0 0 12px 0;color:var(--muted);font-size:13px;line-height:1.4;}
    .form{
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:10px;
    }
    @media (max-width:560px){ .form{grid-template-columns:1fr;} }
    label{font-size:12px;color:var(--muted);}
    input{
      width:100%;
      padding:12px 12px;
      border-radius:14px;
      border:1px solid rgba(255,255,255,.14);
      background:rgba(0,0,0,.18);
      color:var(--text);
      outline:none;
      font-size:14px;
    }
    .modalActions{display:flex;gap:10px;flex-wrap:wrap;margin-top:12px;}
    .note{margin-top:10px;color:var(--muted);font-size:12px;line-height:1.35;}
    .topPlayer{
      display:flex; gap:10px; flex-wrap:wrap; align-items:center;
      margin-top:8px;
      color:var(--muted);
      font-size:13px;
    }
    .badge{
      border:1px solid rgba(255,255,255,.14);
      padding:6px 10px; border-radius:999px;
      background:rgba(0,0,0,.15);
      color:#dfe8ff;
      font-size:12px;
    }

    /* ===== Host panel ===== */
    .host{
      max-width:1200px;
      margin:0 auto;
      padding:0 14px 14px;
      display:none;
    }
    .host.show{display:block;}
    .hostCard{
      background:rgba(0,0,0,.18);
      border:1px solid rgba(255,255,255,.12);
      border-radius:18px;
      overflow:hidden;
      box-shadow:var(--shadow);
    }
    .hostHead{
      padding:12px 14px;
      border-bottom:1px solid rgba(255,255,255,.10);
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:10px;
      flex-wrap:wrap;
    }
    .hostHead b{color:#dfe8ff}
    table{
      width:100%;
      border-collapse:collapse;
      font-size:13px;
    }
    th, td{
      padding:10px 14px;
      border-bottom:1px solid rgba(255,255,255,.08);
      text-align:left;
      color:var(--muted);
    }
    th{color:#dfe8ff; font-weight:700;}
    tr:nth-child(even) td{background:rgba(255,255,255,.02);}
    .win{color:var(--ok); font-weight:800;}
    .small{font-size:12px;color:var(--muted);}
  </style>
</head>
<body>

<!-- Modal de inicio -->
<div class="modalBack" id="modalBack" style="display:none">
  <div class="modal">
    <h3>Antes de iniciar</h3>
    <p>Escribe tu <b>nombre</b> y tu <b>carrera</b>. Esto permite mostrar tu avance (y en modo organizador, el ranking en vivo).</p>

    <div class="form">
      <div>
        <label>Nombre</label>
        <input id="inpName" placeholder="Ej. Kevin Eduardo" maxlength="40" />
      </div>
      <div>
        <label>Carrera</label>
        <input id="inpCareer" placeholder="Ej. Desarrollo Sustentable" maxlength="60" />
      </div>
    </div>

    <div class="modalActions">
      <button id="btnStart">Iniciar</button>
      <button id="btnSkip" class="btnGhost">Solo probar (sin ranking)</button>
    </div>

    <div class="note">
      <b>Nota:</b> Para ver “quién va ganando” en vivo desde tu PC necesitas activar Firebase (gratis). Si no, cada alumno solo verá su propio progreso.
    </div>
  </div>
</div>

<header>
  <div class="title">
    <h1>Sopa de letras – Introducción a la Toxicología Ambiental</h1>
    <p class="sub">Arrastra con mouse o dedo. En celular también puedes: <b>tocar inicio</b> y luego <b>tocar final</b>.</p>

    <div class="topPlayer" id="topPlayer" style="display:none">
      <span class="badge" id="bName"></span>
      <span class="badge" id="bCareer"></span>
      <span class="badge" id="bScore"></span>
    </div>
  </div>
  <div class="bar">
    <button id="btnNew">Nuevo juego (cambia posiciones)</button>
    <button id="btnClear" class="btnGhost">Limpiar selección</button>
    <button id="btnResetUser" class="btnDanger">Cambiar nombre</button>
  </div>
</header>

<!-- Panel organizador -->
<div class="host" id="hostPanel">
  <div class="hostCard">
    <div class="hostHead">
      <div>
        <b>Organizador – Avance en vivo</b>
        <div class="small">Abre con <span class="kbd">?host=1</span> para ver el ranking. (Requiere Firebase activo)</div>
      </div>
      <div class="small" id="hostStatus">Estado: <b style="color:var(--warn)">Sin Firebase</b></div>
    </div>
    <div style="overflow:auto">
      <table>
        <thead>
          <tr>
            <th>Pos.</th>
            <th>Nombre</th>
            <th>Carrera</th>
            <th>Encontradas</th>
            <th>%</th>
            <th>Tiempo</th>
          </tr>
        </thead>
        <tbody id="tbodyRank">
          <tr><td colspan="6" class="small">Activa Firebase para ver el ranking aquí.</td></tr>
        </tbody>
      </table>
    </div>
  </div>
</div>

<div class="wrap">
  <section class="card">
    <h2>
      Sopa de letras
      <span class="chip" id="chipSize"></span>
    </h2>

    <div class="gridHead">
      <div class="liveSel">Seleccionado: <b id="liveWord">—</b></div>
      <div class="tips">
        <span>Tip: <span class="kbd">Esc</span> limpia</span>
        <span>•</span>
        <span>Celular: <span class="kbd">toca inicio</span> + <span class="kbd">toca final</span></span>
      </div>
    </div>

    <div class="gridWrap" id="gridWrap">
      <div class="grid" id="grid">
        <svg class="selLine" id="selLine" viewBox="0 0 100 100" preserveAspectRatio="none">
          <path id="selPath" d=""></path>
        </svg>
      </div>
    </div>

    <div class="status">
      <div class="progress">
        <div>Encontradas: <b id="foundCount">0</b>/<b id="totalCount">0</b></div>
        <div class="meter"><div id="meterBar"></div></div>
        <div><b id="pct">0%</b></div>
      </div>
      <div>Tiempo: <b id="timer">00:00</b></div>
    </div>
  </section>

  <aside class="card">
    <h2>
      Preguntas y respuestas
      <span class="chip" id="chipHelp">15 reactivos</span>
    </h2>

    <div class="panel">
      <div class="hint">
        ✅ 15 reactivos.<br>
        ✅ Selección por arrastre y por 2 toques (celular).<br>
        ✅ “Pista” opcional por pregunta.<br>
        ✅ Nombre y carrera arriba.
      </div>

      <div class="qa" id="qa"></div>
    </div>
  </aside>
</div>

<script type="module">
/* =========================================================
   ✅ Firebase (OPCIONAL, para ranking en vivo)
   ---------------------------------------------------------
   1) Crea proyecto en Firebase
   2) Activa Realtime Database (modo test para tu clase)
   3) Pega tu config en FIREBASE_CONFIG
   ========================================================= */
const FIREBASE_CONFIG = null;
/*
const FIREBASE_CONFIG = {
  apiKey: "TU_APIKEY",
  authDomain: "TU_PROYECTO.firebaseapp.com",
  databaseURL: "https://TU_PROYECTO-default-rtdb.firebaseio.com",
  projectId: "TU_PROYECTO",
  appId: "TU_APPID"
};
*/

let firebaseOn = false;
let db = null;
let rankUnsub = null;

async function initFirebaseIfConfigured(){
  if(!FIREBASE_CONFIG) return;

  const { initializeApp } = await import("https://www.gstatic.com/firebasejs/10.12.4/firebase-app.js");
  const { getDatabase, ref, set, onValue } = await import("https://www.gstatic.com/firebasejs/10.12.4/firebase-database.js");

  const app = initializeApp(FIREBASE_CONFIG);
  db = getDatabase(app);
  firebaseOn = true;

  window.__rtdb = { ref, set, onValue }; // para usar en funciones
}

/* ====== 15 PREGUNTAS (10 + 5 nuevas) ====== */
const QA = [
  { q: "Sustancia capaz de producir daño en un organismo.", answer: "AGENTE_TOXICO" },
  { q: "Capacidad inherente de una sustancia para causar daño.", answer: "TOXICIDAD" },
  { q: "Cantidad de sustancia que ingresa al organismo.", answer: "DOSIS" },
  { q: "Contacto de un organismo con un agente tóxico.", answer: "EXPOSICION" },
  { q: "Tipo de exposición de corta duración.", answer: "AGUDA" },
  { q: "Tipo de exposición de larga duración.", answer: "CRONICA" },
  { q: "Origen natural o artificial de sustancias contaminantes.", answer: "FUENTES" },
  { q: "Clasificación de tóxicos por energía física (ej. radiación, ruido).", answer: "FISICOS" },
  { q: "Clasificación de tóxicos por sustancias (ej. metales, plaguicidas).", answer: "QUIMICOS" },
  { q: "Clasificación de tóxicos por organismos vivos (ej. virus, bacterias).", answer: "BIOLOGICOS" },

  /* +5 nuevas (tipos/vías) */
  { q: "Vía de entrada al organismo por respirar contaminantes.", answer: "INHALACION" },
  { q: "Vía de entrada al organismo por consumir alimentos/agua.", answer: "INGESTION" },
  { q: "Vía de entrada al organismo por contacto con la piel.", answer: "DERMICA" },
  { q: "Exposición típica relacionada con actividades laborales.", answer: "OCUPACIONAL" },
  { q: "Exposición por contaminantes presentes en aire/agua/suelo.", answer: "AMBIENTAL" },
];

/* ====== CONFIG ====== */
let GRID_SIZE = 18;
const ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
const DIRECTIONS = [
  {dr:0, dc:1},{dr:1, dc:0},{dr:0, dc:-1},{dr:-1, dc:0},
  {dr:1, dc:1},{dr:1, dc:-1},{dr:-1, dc:1},{dr:-1, dc:-1},
];

const norm = (s) => s
  .toUpperCase()
  .replaceAll("Á","A").replaceAll("É","E").replaceAll("Í","I").replaceAll("Ó","O").replaceAll("Ú","U").replaceAll("Ü","U")
  .replace(/[^A-Z_]/g,"")
  .replaceAll("_","");

let grid = [];
let placed = [];
let selecting = false;
let selected = [];
let activePointerId = null;
let tapStartIdx = null;

// timer
let startTime = null;
let timerInt = null;

const $grid = document.getElementById("grid");
const $qa = document.getElementById("qa");
const $foundCount = document.getElementById("foundCount");
const $totalCount = document.getElementById("totalCount");
const $chipSize = document.getElementById("chipSize");
const $chipHelp = document.getElementById("chipHelp");
const $liveWord = document.getElementById("liveWord");
const $timer = document.getElementById("timer");
const $meterBar = document.getElementById("meterBar");
const $pct = document.getElementById("pct");
const $selLine = document.getElementById("selLine");
const $selPath = document.getElementById("selPath");

document.getElementById("btnNew").addEventListener("click", newGame);
document.getElementById("btnClear").addEventListener("click", clearSelection);
window.addEventListener("keydown",(e)=>{ if(e.key==="Escape") clearSelection(); });

/* ====== Usuario (Nombre/Carrera) ====== */
const $modalBack = document.getElementById("modalBack");
const $inpName = document.getElementById("inpName");
const $inpCareer = document.getElementById("inpCareer");
const $btnStart = document.getElementById("btnStart");
const $btnSkip = document.getElementById("btnSkip");

const $topPlayer = document.getElementById("topPlayer");
const $bName = document.getElementById("bName");
const $bCareer = document.getElementById("bCareer");
const $bScore = document.getElementById("bScore");
document.getElementById("btnResetUser").addEventListener("click", ()=>{
  localStorage.removeItem("ta_user");
  showUserModal();
});

let user = null; // {id, name, career}

function randomId(){
  return Math.random().toString(16).slice(2) + Date.now().toString(16);
}

function showUserModal(){
  $modalBack.style.display = "flex";
  const saved = localStorage.getItem("ta_user");
  if(saved){
    try{
      const u = JSON.parse(saved);
      $inpName.value = u.name || "";
      $inpCareer.value = u.career || "";
    }catch{}
  }
}

function setUser(u){
  user = u;
  localStorage.setItem("ta_user", JSON.stringify(u));
  $modalBack.style.display = "none";

  $topPlayer.style.display = "flex";
  $bName.textContent = `👤 ${u.name || "Alumno"}`;
  $bCareer.textContent = `🎓 ${u.career || "Carrera"}`;
  $bScore.textContent = `🏁 0/${QA.length}`;
}

$btnStart.addEventListener("click", ()=>{
  const name = ($inpName.value || "").trim();
  const career = ($inpCareer.value || "").trim();
  if(!name || !career){
    alert("Escribe Nombre y Carrera.");
    return;
  }
  const saved = localStorage.getItem("ta_user");
  let id = null;
  if(saved){
    try{ id = JSON.parse(saved).id; }catch{}
  }
  setUser({ id: id || randomId(), name, career });
  pushProgress(); // inicial
});

$btnSkip.addEventListener("click", ()=>{
  setUser({ id: randomId(), name: "Invitado", career: "—" });
});

/* ====== Host mode ====== */
const isHost = new URLSearchParams(location.search).get("host") === "1";
const $hostPanel = document.getElementById("hostPanel");
const $hostStatus = document.getElementById("hostStatus");
const $tbodyRank = document.getElementById("tbodyRank");
if(isHost) $hostPanel.classList.add("show");

/* ====== Helpers grid ====== */
function randInt(n){ return Math.floor(Math.random()*n); }
function makeEmptyGrid(n){ return Array.from({length:n},()=>Array.from({length:n},()=>null)); }
function inBounds(r,c){ return r>=0 && c>=0 && r<GRID_SIZE && c<GRID_SIZE; }

function canPlace(word, r, c, dir){
  const cells = [];
  for(let i=0;i<word.length;i++){
    const rr = r + dir.dr*i;
    const cc = c + dir.dc*i;
    if(!inBounds(rr,cc)) return null;
    const ch = grid[rr][cc];
    if(ch !== null && ch !== word[i]) return null;
    cells.push({r:rr,c:cc});
  }
  return cells;
}

function placeWord(word){
  for(let t=0;t<2200;t++){
    const dir = DIRECTIONS[randInt(DIRECTIONS.length)];
    const r = randInt(GRID_SIZE);
    const c = randInt(GRID_SIZE);
    const cells = canPlace(word,r,c,dir);
    if(!cells) continue;

    for(let i=0;i<word.length;i++){
      const {r:rr,c:cc} = cells[i];
      grid[rr][cc] = word[i];
    }
    return cells.map(p=>p.r*GRID_SIZE+p.c);
  }
  return null;
}

function fillRandom(){
  for(let r=0;r<GRID_SIZE;r++){
    for(let c=0;c<GRID_SIZE;c++){
      if(grid[r][c]===null) grid[r][c] = ALPHABET[randInt(ALPHABET.length)];
    }
  }
}

function renderGrid(){
  $grid.style.setProperty("--n", GRID_SIZE);

  const svg = $selLine;
  $grid.innerHTML = "";
  $grid.appendChild(svg);

  const flat = grid.flat();
  flat.forEach((ch, idx)=>{
    const d = document.createElement("div");
    d.className = "cell" + (((Math.floor(idx/GRID_SIZE)+idx)%2===0) ? " alt" : "");
    d.textContent = ch;
    d.dataset.idx = idx;
    $grid.appendChild(d);
  });

  $grid.addEventListener("pointerdown", onPointerDown);
  window.addEventListener("pointermove", onPointerMove);
  window.addEventListener("pointerup", onPointerUp);
  window.addEventListener("pointercancel", onPointerUp);

  $grid.addEventListener("click", onGridClick);
}

function onGridClick(e){
  const cell = e.target.closest(".cell");
  if(!cell) return;
  const idx = Number(cell.dataset.idx);
  if(selecting) return;

  if(tapStartIdx === null){
    tapStartIdx = idx;
    clearSelection();
    addToSelection(idx);
    updateLive();
    drawLine();
    return;
  }

  const seq = lineBetween(tapStartIdx, idx);
  if(!seq){
    tapStartIdx = idx;
    clearSelection();
    addToSelection(idx);
    updateLive();
    drawLine();
    return;
  }
  clearSelection();
  seq.forEach(addToSelection);
  updateLive();
  drawLine();
  checkSelection();
  tapStartIdx = null;
}

function onPointerDown(e){
  const cell = getCellFromPoint(e.clientX, e.clientY);
  if(!cell) return;
  e.preventDefault();

  tapStartIdx = null;
  selecting = true;
  activePointerId = e.pointerId;
  clearSelection();
  addToSelection(Number(cell.dataset.idx));
  updateLive();
  drawLine();
}

function onPointerMove(e){
  if(!selecting) return;
  if(activePointerId !== null && e.pointerId !== activePointerId) return;

  const cell = getCellFromPoint(e.clientX, e.clientY);
  if(!cell) return;

  const idx = Number(cell.dataset.idx);

  if(selected.length === 0){
    addToSelection(idx);
  }else if(selected.length === 1){
    addToSelection(idx);
  }else{
    if(isCollinearSelection(selected[0], selected[1], idx)){
      addToSelection(idx);
    }
  }
  updateLive();
  drawLine();
}

function onPointerUp(e){
  if(!selecting) return;
  if(activePointerId !== null && e.pointerId !== activePointerId) return;

  selecting = false;
  activePointerId = null;
  checkSelection();
  updateLive();
  drawLine();
}

function getCellFromPoint(x,y){
  const el = document.elementFromPoint(x,y);
  if(!el) return null;
  if(el.classList && el.classList.contains("cell")) return el;
  return el.closest?.(".cell") || null;
}

function renderQA(){
  $qa.innerHTML = "";
  const total = placed.length;
  $totalCount.textContent = total;
  $chipHelp.textContent = `${total} reactivos`;

  placed.forEach((p, i)=>{
    const row = document.createElement("div");
    row.className = "row";

    const q = document.createElement("div");
    q.className = "q";
    q.textContent = `${i+1}. ${p.q}`;

    const a = document.createElement("div");
    a.className = "a";
    a.id = `ans_${i}`;
    a.innerHTML = `<span>${"—".repeat(Math.max(3, norm(p.answer).length))}</span><span class="tag">NO ENCONTRADA</span>`;

    const actions = document.createElement("div");
    actions.className = "rowActions";

    const btnHint = document.createElement("button");
    btnHint.className = "miniBtn warn";
    btnHint.type = "button";
    btnHint.textContent = "Ver pista";
    btnHint.addEventListener("click", ()=>{
      const w = norm(p.answer);
      alert(`Pista: empieza con "${w[0]}", termina con "${w[w.length-1]}" (longitud: ${w.length}).`);
    });

    actions.appendChild(btnHint);
    row.appendChild(q);
    row.appendChild(a);
    row.appendChild(actions);
    $qa.appendChild(row);
  });

  updateCounts();
}

function setAnswerFound(i){
  const box = document.getElementById(`ans_${i}`);
  if(!box) return;
  box.classList.add("ok");
  box.innerHTML = `<span>${norm(placed[i].answer)}</span><span class="tag">OK</span>`;
}

function updateCounts(){
  const found = placed.filter(p=>p.found).length;
  const total = placed.length;
  $foundCount.textContent = found;

  const pct = total ? Math.round((found/total)*100) : 0;
  $meterBar.style.width = `${pct}%`;
  $pct.textContent = `${pct}%`;

  if(user){
    $bScore.textContent = `🏁 ${found}/${total}`;
  }

  // cada cambio de avance -> subir a ranking
  pushProgress();
}

function clearSelection(){
  selected = [];
  document.querySelectorAll(".cell.sel").forEach(el=>el.classList.remove("sel"));
  $selPath.setAttribute("d","");
  $selLine.classList.remove("ok");
  $liveWord.textContent = "—";
}

function addToSelection(idx){
  if(selected.includes(idx)) return;
  selected.push(idx);
  const el = document.querySelector(`.cell[data-idx="${idx}"]`);
  if(el) el.classList.add("sel");
}

function idxToRC(idx){ return { r: Math.floor(idx/GRID_SIZE), c: idx % GRID_SIZE }; }

function isCollinearSelection(a,b,c){
  const A = idxToRC(a), B = idxToRC(b), C = idxToRC(c);
  const dr1 = B.r - A.r, dc1 = B.c - A.c;
  const dr2 = C.r - B.r, dc2 = C.c - B.c;
  const nd1 = {dr: Math.sign(dr1), dc: Math.sign(dc1)};
  const nd2 = {dr: Math.sign(dr2), dc: Math.sign(dc2)};
  return (nd1.dr===nd2.dr && nd1.dc===nd2.dc) && !(nd2.dr===0 && nd2.dc===0);
}

function selectionString(){
  const flat = grid.flat();
  return selected.map(i=>flat[i]).join("");
}

function updateLive(){
  if(selected.length===0){ $liveWord.textContent="—"; return; }
  $liveWord.textContent = selectionString();
}

function flashWrong(){
  selected.forEach(idx=>{
    const el = document.querySelector(`.cell[data-idx="${idx}"]`);
    if(el){
      el.classList.add("wrongFlash");
      setTimeout(()=>el.classList.remove("wrongFlash"), 220);
    }
  });
}

function checkSelection(){
  if(selected.length<2) return;

  const s = selectionString();
  const rev = s.split("").reverse().join("");

  for(let i=0;i<placed.length;i++){
    const p = placed[i];
    if(p.found) continue;
    const w = norm(p.answer);
    if(s===w || rev===w){
      p.found = true;
      p.cells.forEach(idx=>{
        const el = document.querySelector(`.cell[data-idx="${idx}"]`);
        if(el){
          el.classList.remove("sel");
          el.classList.add("found");
        }
      });
      setAnswerFound(i);
      $selLine.classList.add("ok");
      updateCounts();
      setTimeout(()=>{ clearSelection(); }, 160);
      return;
    }
  }
  flashWrong();
}

function cellCenter(idx){
  const el = document.querySelector(`.cell[data-idx="${idx}"]`);
  if(!el) return null;
  const r = el.getBoundingClientRect();
  const g = $grid.getBoundingClientRect();
  const x = (r.left + r.right)/2 - g.left;
  const y = (r.top + r.bottom)/2 - g.top;
  return {x,y};
}

function drawLine(){
  if(selected.length<2){
    $selPath.setAttribute("d","");
    $selLine.classList.remove("ok");
    return;
  }
  const a = cellCenter(selected[0]);
  const b = cellCenter(selected[selected.length-1]);
  if(!a || !b) return;

  const gb = $grid.getBoundingClientRect();
  $selLine.setAttribute("viewBox", `0 0 ${gb.width} ${gb.height}`);
  $selPath.setAttribute("d", `M ${a.x} ${a.y} L ${b.x} ${b.y}`);
}

function lineBetween(aIdx, bIdx){
  if(aIdx===bIdx) return [aIdx];
  const A = idxToRC(aIdx), B = idxToRC(bIdx);
  const dr = B.r - A.r;
  const dc = B.c - A.c;

  const sdr = Math.sign(dr);
  const sdc = Math.sign(dc);

  if(!(dr===0 || dc===0 || Math.abs(dr)===Math.abs(dc))) return null;

  const len = Math.max(Math.abs(dr), Math.abs(dc)) + 1;
  const seq = [];
  for(let i=0;i<len;i++){
    const rr = A.r + sdr*i;
    const cc = A.c + sdc*i;
    if(!inBounds(rr,cc)) return null;
    seq.push(rr*GRID_SIZE+cc);
  }
  return seq;
}

/* ====== TIMER ====== */
function startTimer(){
  stopTimer();
  startTime = Date.now();
  timerInt = setInterval(()=>{
    const s = Math.floor((Date.now()-startTime)/1000);
    const mm = String(Math.floor(s/60)).padStart(2,"0");
    const ss = String(s%60).padStart(2,"0");
    $timer.textContent = `${mm}:${ss}`;
    // actualizar ranking cada 2-3s aprox (sin saturar)
    if(s % 3 === 0) pushProgress();
  }, 300);
}
function stopTimer(){
  if(timerInt) clearInterval(timerInt);
  timerInt = null;
}

function elapsedSeconds(){
  if(!startTime) return 0;
  return Math.floor((Date.now()-startTime)/1000);
}
function fmtTime(sec){
  const mm = String(Math.floor(sec/60)).padStart(2,"0");
  const ss = String(sec%60).padStart(2,"0");
  return `${mm}:${ss}`;
}

/* ====== Ranking en vivo ====== */
async function pushProgress(){
  if(!firebaseOn || !db || !user) return;
  const total = placed.length || QA.length;
  const found = placed.filter(p=>p.found).length;
  const percent = total ? Math.round(found/total*100) : 0;
  const t = elapsedSeconds();

  // “score” para ordenar: primero % mayor, luego tiempo menor
  const scoreKey = percent*100000 - t; // mayor mejor

  const { ref, set } = window.__rtdb;
  await set(ref(db, `ta_live/${location.pathname.replaceAll("/","_")}/players/${user.id}`), {
    name: user.name,
    career: user.career,
    found,
    total,
    percent,
    time: t,
    timeFmt: fmtTime(t),
    scoreKey,
    updatedAt: Date.now()
  });
}

async function subscribeRanking(){
  if(!isHost) return;
  if(!firebaseOn || !db){
    $hostStatus.innerHTML = `Estado: <b style="color:var(--warn)">Sin Firebase</b>`;
    return;
  }
  $hostStatus.innerHTML = `Estado: <b style="color:var(--ok)">En vivo</b>`;

  const { ref, onValue } = window.__rtdb;
  const pathKey = location.pathname.replaceAll("/","_");
  const rankRef = ref(db, `ta_live/${pathKey}/players`);

  onValue(rankRef, (snap)=>{
    const val = snap.val() || {};
    const arr = Object.values(val);

    // ordenar: scoreKey desc
    arr.sort((a,b)=> (b.scoreKey||0) - (a.scoreKey||0));

    if(arr.length===0){
      $tbodyRank.innerHTML = `<tr><td colspan="6" class="small">Sin jugadores todavía.</td></tr>`;
      return;
    }

    const top = arr[0];
    $tbodyRank.innerHTML = arr.map((p, i)=>{
      const isWin = (i===0);
      return `
        <tr>
          <td class="${isWin?'win':''}">${i+1}</td>
          <td class="${isWin?'win':''}">${escapeHtml(p.name||'')}</td>
          <td>${escapeHtml(p.career||'')}</td>
          <td><b style="color:#dfe8ff">${p.found||0}</b> / ${p.total||0}</td>
          <td>${p.percent||0}%</td>
          <td>${p.timeFmt || fmtTime(p.time||0)}</td>
        </tr>
      `;
    }).join("");
  });
}

function escapeHtml(s){
  return String(s).replace(/[&<>"']/g, (m)=>({
    "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"
  }[m]));
}

/* ====== GAME ====== */
function newGame(){
  const maxLen = Math.max(...QA.map(x=>norm(x.answer).length));
  GRID_SIZE = Math.max(18, Math.min(22, maxLen + 4));

  grid = makeEmptyGrid(GRID_SIZE);
  placed = [];
  clearSelection();
  tapStartIdx = null;

  const sorted = QA
    .map(x=>({...x, w:norm(x.answer)}))
    .sort((a,b)=>b.w.length-a.w.length);

  for(const it of sorted){
    const idxs = placeWord(it.w);
    if(idxs){
      placed.push({ q: it.q, answer: it.answer, w: it.w, cells: idxs, found:false });
    }
  }

  fillRandom();
  renderGrid();
  renderQA();
  $chipSize.textContent = `${GRID_SIZE}×${GRID_SIZE}`;

  startTimer();
  updateCounts();
}

(async function boot(){
  await initFirebaseIfConfigured();
  if(isHost) await subscribeRanking();

  // cargar usuario o pedirlo
  const saved = localStorage.getItem("ta_user");
  if(saved){
    try{
      setUser(JSON.parse(saved));
    }catch{
      showUserModal();
    }
  }else{
    showUserModal();
  }

  newGame();
})();
</script>
</body>
</html>
3) Para que el ranking “en vivo” funcione (Firebase)

Si lo quieres ya real:

Entra a Firebase Console

Crea proyecto

Activa Realtime Database (modo test durante tu clase)

Copia tu config y pégala en el código donde dice:
