<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>Sopa de letras – Toxicología Ambiental</title>
  <style>
    :root{
      --bg:#0b1220;
      --card:#0f1a33;
      --muted:#9fb0d0;
      --text:#e9f0ff;
      --line:#1f2f59;
      --ok:#20c997;

      /* Responsive sizing for grid cells */
      --cell: clamp(26px, 6vw, 38px);   /* cell size adjusts with screen */
      --gap: clamp(4px, 1.2vw, 7px);    /* gap adjusts with screen */
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
    h1{margin:0;font-size:18px;letter-spacing:.2px;line-height:1.2;}
    .sub{margin:0;color:var(--muted);font-size:13px;line-height:1.35;}

    .bar{
      display:flex;
      gap:10px;
      align-items:center;
      flex-wrap:wrap;
      justify-content:flex-end;
      flex:1;
      min-width:260px;
    }

    button{
      border:1px solid rgba(255,255,255,.16);
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

    .wrap{
      max-width:1200px;
      margin:0 auto;
      padding:0 14px 20px;
      display:grid;
      grid-template-columns: 1.15fr .85fr;
      gap:14px;
    }

    /* Mobile: single column, big touch areas */
    @media (max-width: 980px){
      .wrap{grid-template-columns:1fr;}
      header{padding-bottom:10px;}
      h1{font-size:17px;}
      button{width:100%;justify-content:center;}
      .bar{justify-content:stretch;}
    }

    .card{
      background:linear-gradient(180deg,rgba(255,255,255,.06),rgba(255,255,255,.03));
      border:1px solid rgba(255,255,255,.10);
      border-radius:var(--radius);
      box-shadow:var(--shadow);
      overflow:hidden;
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

    /* Grid area */
    .gridWrap{
      padding:14px;
      overflow:auto;               /* allows scroll on tiny devices */
      -webkit-overflow-scrolling: touch;
    }
    .grid{
      display:grid;
      grid-template-columns: repeat(var(--n), var(--cell));
      gap: var(--gap);
      user-select:none;
      width:max-content;
      margin:0 auto;
    }
    .cell{
      width:var(--cell);
      height:var(--cell);
      display:flex;
      align-items:center;
      justify-content:center;
      border-radius:12px;
      border:1px solid rgba(255,255,255,.12);
      background:linear-gradient(180deg,rgba(255,255,255,.06),rgba(255,255,255,.02));
      box-shadow:0 2px 10px rgba(0,0,0,.25);
      cursor:pointer;
      font-weight:800;
      letter-spacing:.4px;
      font-size: clamp(12px, 3.5vw, 16px);
      touch-action: none; /* helps drag selection */
    }
    .cell.alt{background:linear-gradient(180deg,rgba(255,255,255,.05),rgba(255,255,255,.015));}
    .cell.sel{outline:2px solid rgba(255,255,255,.35); background:rgba(255,255,255,.12);}
    .cell.found{background:rgba(32,201,151,.22); border-color:rgba(32,201,151,.55);}

    .cell.wrongFlash{animation:shake .25s ease;}
    @keyframes shake{
      0%{transform:translateX(0)}
      25%{transform:translateX(-2px)}
      50%{transform:translateX(2px)}
      75%{transform:translateX(-1px)}
      100%{transform:translateX(0)}
    }

    .status{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      align-items:center;
      justify-content:space-between;
      padding:12px 14px;
      border-top:1px solid rgba(255,255,255,.10);
      color:var(--muted);
      font-size:13px;
    }
    .status b{color:var(--text)}
    .kbd{
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      border:1px solid rgba(255,255,255,.18);
      padding:2px 6px;border-radius:8px;background:rgba(0,0,0,.18);color:#dfe8ff;
      font-size:12px;
    }

    /* Right panel */
    .panel{
      padding:12px 14px 14px;
      display:flex;
      flex-direction:column;
      gap:12px;
    }
    .hint{
      color:var(--muted);
      font-size:13px;
      line-height:1.4;
    }
    .qa{
      display:flex;
      flex-direction:column;
      gap:10px;
    }
    .row{
      padding:12px;
      border-radius:16px;
      border:1px solid rgba(255,255,255,.10);
      background:rgba(0,0,0,.12);
      display:grid;
      grid-template-columns: 1fr;
      gap:8px;
    }
    .q{
      font-size:13px;
      color:#dbe6ff;
      line-height:1.35;
    }
    .a{
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      font-size:13px;
      letter-spacing:.6px;
      padding:10px 10px;
      border-radius:14px;
      border:1px dashed rgba(255,255,255,.20);
      background:rgba(255,255,255,.04);
      color:#ffffff;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
      overflow:hidden;
    }
    .tag{
      font-size:11px;
      color:var(--muted);
      border:1px solid rgba(255,255,255,.14);
      padding:4px 8px;
      border-radius:999px;
      white-space:nowrap;
      flex:0 0 auto;
    }
    .a span:first-child{
      overflow:hidden;
      text-overflow:ellipsis;
      white-space:nowrap;
      max-width: 100%;
    }
    .a.ok{
      border-style:solid;
      border-color:rgba(32,201,151,.55);
      background:rgba(32,201,151,.12);
    }

    .footerNote{
      max-width:1200px;
      margin:0 auto;
      padding:0 14px 18px;
      color:var(--muted);
      font-size:12px;
      line-height:1.4;
    }
  </style>
</head>
<body>

  <header>
    <div class="title">
      <h1>Sopa de letras – Introducción a la Toxicología Ambiental</h1>
      <p class="sub">Selecciona letras en línea recta (horizontal, vertical o diagonal). Al acertar, se marca en verde y se completa la respuesta.</p>
    </div>
    <div class="bar">
      <button id="btnNew">Nuevo juego (cambia posiciones)</button>
      <button id="btnClear">Limpiar selección</button>
    </div>
  </header>

  <div class="wrap">
    <section class="card">
      <h2>
        Sopa de letras
        <span class="chip" id="chipSize"></span>
      </h2>

      <div class="gridWrap" id="gridWrap">
        <div class="grid" id="grid"></div>
      </div>

      <div class="status">
        <div>Encontradas: <b id="foundCount">0</b>/<b id="totalCount">0</b></div>
        <div>Tip: arrastra o toca letras. <span class="kbd">Esc</span> limpia.</div>
      </div>
    </section>

    <aside class="card">
      <h2>
        Preguntas y respuestas
        <span class="chip" id="chipHelp">30 reactivos</span>
      </h2>
      <div class="panel">
        <div class="hint">
          ✅ Las <b>respuestas</b> se completan cuando encuentras la palabra.<br>
          🔁 “Nuevo juego” reubica las palabras.<br>
          📱 Diseñado para verse bien en celular.
        </div>
        <div class="qa" id="qa"></div>
      </div>
    </aside>
  </div>

  <div class="footerNote">
    Si alguna palabra no cabe, aumenta <b>GRID_SIZE</b> (por ejemplo 20) o acorta respuestas. Recomendación: respuestas sin acentos y sin espacios.
  </div>

<script>
/* =========================
   PREGUNTAS Y RESPUESTAS (unidad)
   ========================= */
const QA = [
  { q: "Sustancia capaz de producir un efecto nocivo en un organismo.", answer: "AGENTE_TOXICO" },
  { q: "Capacidad inherente de una sustancia para causar daño.", answer: "TOXICIDAD" },
  { q: "Cantidad de sustancia que ingresa al organismo.", answer: "DOSIS" },
  { q: "Contacto de un organismo con un agente toxico.", answer: "EXPOSICION" },
  { q: "Tipo de exposicion que ocurre en corto tiempo.", answer: "AGUDA" },
  { q: "Tipo de exposicion que ocurre durante largo tiempo.", answer: "CRONICA" },
  { q: "Exposicion que ocurre repetidamente en intervalos.", answer: "INTERMITENTE" },
  { q: "Origen natural o artificial de sustancias contaminantes.", answer: "FUENTES_DE_CONTAMINACION" },
  { q: "Contaminacion proveniente de procesos industriales.", answer: "INDUSTRIAL" },
  { q: "Contaminacion generada por actividades agricolas.", answer: "AGRICOLA" },
  { q: "Contaminacion producida por residuos domesticos.", answer: "DOMESTICA" },
  { q: "Contaminacion causada por transporte vehicular.", answer: "VEHICULAR" },
  { q: "Clasificacion de toxicos por energia fisica.", answer: "FISICOS" },
  { q: "Clasificacion de toxicos por sustancias quimicas.", answer: "QUIMICOS" },
  { q: "Clasificacion de toxicos por organismos vivos.", answer: "BIOLOGICOS" },
  { q: "Radiacion y ruido son ejemplos de toxicos.", answer: "FISICOS" },
  { q: "Metales pesados y pesticidas son ejemplos de toxicos.", answer: "QUIMICOS" },
  { q: "Virus y bacterias son ejemplos de toxicos.", answer: "BIOLOGICOS" },
  { q: "Proceso mediante el cual el toxico entra al organismo.", answer: "INGRESO" },
  { q: "Cantidad minima que produce efecto observable.", answer: "UMBRAL" },
  { q: "Relacion entre dosis administrada y efecto biologico.", answer: "DOSIS_RESPUESTA" },
  { q: "Contacto directo con la piel.", answer: "DERMICA" },
  { q: "Ingreso por el sistema respiratorio.", answer: "INHALACION" },
  { q: "Ingreso por el sistema digestivo.", answer: "INGESTION" },
  { q: "Sustancia presente en aire, agua o suelo que altera el ambiente.", answer: "CONTAMINANTE" },
  { q: "Efecto danino provocado por sustancia toxica.", answer: "DANO" },
  { q: "Nivel de sustancia presente en un medio.", answer: "CONCENTRACION" },
  { q: "Tiempo durante el cual ocurre la exposicion.", answer: "DURACION" },
  { q: "Frecuencia con la que ocurre la exposicion.", answer: "FRECUENCIA" },
  { q: "Evaluacion integral del peligro y la exposicion.", answer: "RIESGO" }
];

/* =========================
   CONFIGURACIÓN
   ========================= */
let GRID_SIZE = 18; // Si una palabra no cabe, sube a 20

const ALPHABET = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
const DIRECTIONS = [
  {dr:0, dc:1},{dr:1, dc:0},{dr:0, dc:-1},{dr:-1, dc:0},
  {dr:1, dc:1},{dr:1, dc:-1},{dr:-1, dc:1},{dr:-1, dc:-1},
];

const norm = (s) => s
  .toUpperCase()
  .replaceAll("Á","A").replaceAll("É","E").replaceAll("Í","I").replaceAll("Ó","O").replaceAll("Ú","U").replaceAll("Ü","U")
  .replace(/[^A-Z_]/g,"")
  .replaceAll("_",""); // en la sopa no metemos guion bajo

// Estado
let grid = [];
let placed = [];
let selecting = false;
let selected = [];

const $grid = document.getElementById("grid");
const $qa = document.getElementById("qa");
const $foundCount = document.getElementById("foundCount");
const $totalCount = document.getElementById("totalCount");
const $chipSize = document.getElementById("chipSize");
const $chipHelp = document.getElementById("chipHelp");

document.getElementById("btnNew").addEventListener("click", newGame);
document.getElementById("btnClear").addEventListener("click", clearSelection);
window.addEventListener("keydown",(e)=>{ if(e.key==="Escape") clearSelection(); });

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
  for(let t=0;t<1400;t++){
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
  $grid.innerHTML = "";
  const flat = grid.flat();

  flat.forEach((ch, idx)=>{
    const d = document.createElement("div");
    d.className = "cell" + (((Math.floor(idx/GRID_SIZE)+idx)%2===0) ? " alt" : "");
    d.textContent = ch;
    d.dataset.idx = idx;

    // Touch + mouse selection
    d.addEventListener("pointerdown",(e)=>{
      e.preventDefault();
      selecting = true;
      clearSelection();
      addToSelection(idx);
      d.setPointerCapture?.(e.pointerId);
    });

    d.addEventListener("pointerenter",()=>{
      if(!selecting) return;
      if(selected.length===1){
        addToSelection(idx);
      } else if(selected.length>=2){
        const ok = isCollinearSelection(selected[0], selected[1], idx);
        if(ok) addToSelection(idx);
      }
    });

    d.addEventListener("pointerup",(e)=>{
      if(!selecting) return;
      selecting = false;
      checkSelection();
      d.releasePointerCapture?.(e.pointerId);
    });

    // Tap to extend selection (optional)
    d.addEventListener("click",()=>{
      if(selecting) return;
      if(selected.length===0){
        addToSelection(idx);
      } else if(selected.length===1){
        addToSelection(idx);
      } else {
        const ok = isCollinearSelection(selected[0], selected[1], idx);
        if(ok) addToSelection(idx);
      }
    });

    $grid.appendChild(d);
  });

  window.addEventListener("pointerup",()=>{
    if(selecting){
      selecting=false;
      checkSelection();
    }
  }, { once:true });
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
    a.innerHTML = `<span>${mask(p.answer)}</span><span class="tag">NO ENCONTRADA</span>`;

    row.appendChild(q);
    row.appendChild(a);
    $qa.appendChild(row);
  });

  updateCounts();
}

function mask(ans){
  const w = norm(ans);
  return "—".repeat(Math.max(3, w.length));
}

function setAnswerFound(i){
  const box = document.getElementById(`ans_${i}`);
  if(!box) return;
  box.classList.add("ok");
  box.innerHTML = `<span>${norm(placed[i].answer)}</span><span class="tag">OK</span>`;
}

function updateCounts(){
  const found = placed.filter(p=>p.found).length;
  $foundCount.textContent = found;
}

function clearSelection(){
  selected = [];
  document.querySelectorAll(".cell.sel").forEach(el=>el.classList.remove("sel"));
}

function addToSelection(idx){
  if(selected.includes(idx)) return;
  selected.push(idx);
  const el = document.querySelector(`.cell[data-idx="${idx}"]`);
  if(el) el.classList.add("sel");
}

function idxToRC(idx){
  return { r: Math.floor(idx/GRID_SIZE), c: idx % GRID_SIZE };
}

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
      clearSelection();
      updateCounts();
      return;
    }
  }

  selected.forEach(idx=>{
    const el = document.querySelector(`.cell[data-idx="${idx}"]`);
    if(el){
      el.classList.add("wrongFlash");
      setTimeout(()=>el.classList.remove("wrongFlash"), 220);
    }
  });
}

function autoFitGridSize(){
  // If longest word doesn't fit, increase GRID_SIZE a bit automatically
  const maxLen = Math.max(...QA.map(x=>norm(x.answer).length));
  if(maxLen > GRID_SIZE) GRID_SIZE = Math.min(24, Math.max(GRID_SIZE, maxLen + 2));
}

function newGame(){
  autoFitGridSize();

  grid = makeEmptyGrid(GRID_SIZE);
  placed = [];
  clearSelection();

  const items = QA.map(x=>({ q:x.q, answer:x.answer }));
  for(let i=items.length-1;i>0;i--){
    const j = randInt(i+1);
    [items[i],items[j]] = [items[j],items[i]];
  }

  const sorted = items
    .map(x=>({...x, w:norm(x.answer)}))
    .sort((a,b)=>b.w.length-a.w.length);

  for(const it of sorted){
    const idxs = placeWord(it.w);
    if(idxs){
      placed.push({ q: it.q, answer: it.answer, w: it.w, cells: idxs, found:false });
    } else {
      console.warn("No cupo:", it.w, "-> Sube GRID_SIZE");
    }
  }

  fillRandom();
  renderGrid();
  renderQA();
  $chipSize.textContent = `${GRID_SIZE}×${GRID_SIZE}`;
}

newGame();
</script>
</body>
</html>
