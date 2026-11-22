<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="color-scheme" content="dark">
<title>Pomo Deck</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Mali:wght@400;600&family=Press+Start+2P&family=VT323&display=swap" rel="stylesheet">
<style>
/* Layout: a pink handheld deck. CRT screen on top, LCD timer below, tape slot at the bottom. */
:root{
  --ink:#2b1233; --night:#241029; --night2:#2f1636; --plum:#582a63;
  --rose:#ff6fae; --candy:#ff9ec7; --bubble:#ffc2dc; --blush:#ffe6f0;
  --mint:#7fe0c3; --butter:#ffe08a; --lav:#b9a3ff;
  --px:'Press Start 2P',ui-monospace,monospace;
  --lcd:'VT323',ui-monospace,monospace;
  --th:'Mali','Sarabun','Noto Sans Thai',system-ui,sans-serif;
  color-scheme:dark;
}
*{box-sizing:border-box}
html,body{margin:0}
body{
  min-height:100vh;
  background-color:var(--night);
  background-image:
    radial-gradient(circle at 50% 20%,rgba(255,111,174,.22),transparent 55%),
    linear-gradient(var(--night2) 2px,transparent 2px),
    linear-gradient(90deg,var(--night2) 2px,transparent 2px);
  background-size:auto,32px 32px,32px 32px;
  color:var(--blush);
  font-family:var(--th);
  font-size:14px;line-height:1.5;
}
.deck{max-width:640px;margin:0 auto;padding:32px 20px 48px;display:grid;gap:24px}

/* pixel outline helper: 4px notched border drawn with hard shadows */
.px{box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink)}

.device{
  position:relative;margin:4px;padding:20px 20px 24px;display:grid;gap:18px;
  background:var(--candy);
  box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink),
    inset 0 -8px 0 0 #e87fb0,inset 0 6px 0 0 #ffd0e6,0 16px 0 0 rgba(0,0,0,.3);
}
.topbar{display:flex;justify-content:space-between;align-items:center;gap:12px}
.brand{display:flex;align-items:center;gap:10px;font-family:var(--px);font-size:12px;color:var(--ink);letter-spacing:1px}
.brand svg{width:21px;height:18px;fill:var(--rose);stroke:none}
.leds{display:flex;gap:10px;align-items:center;font-family:var(--px);font-size:8px;color:var(--ink)}
.leds span{display:flex;align-items:center;gap:6px}
.led{width:10px;height:10px;background:var(--plum);box-shadow:0 0 0 2px var(--ink)}
.led.on-work{background:var(--rose);box-shadow:0 0 0 2px var(--ink),0 0 8px 2px rgba(255,111,174,.9)}
.led.on-break{background:var(--mint);box-shadow:0 0 0 2px var(--ink),0 0 8px 2px rgba(127,224,195,.9)}

.bezel{background:var(--ink);padding:14px;box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink),inset 0 0 0 4px #4a2358}
.screen{position:relative;aspect-ratio:16/9;background:#14091c;overflow:hidden}
.screen canvas,.host{position:absolute;inset:0;width:100%;height:100%}
.screen canvas{image-rendering:pixelated;image-rendering:crisp-edges;image-rendering:pixelated}
.host iframe{width:100%;height:100%;border:0;display:block}
.crt{position:absolute;inset:0;pointer-events:none;z-index:2;
  background:
    repeating-linear-gradient(to bottom,rgba(0,0,0,0) 0 2px,rgba(30,8,40,.16) 2px 3px),
    radial-gradient(ellipse at center,rgba(0,0,0,0) 62%,rgba(30,8,40,.5) 100%)}
.osd{position:absolute;left:10px;right:10px;top:8px;z-index:3;display:flex;justify-content:space-between;gap:8px;
  font-family:var(--px);font-size:9px;color:#fff;text-shadow:2px 2px 0 var(--ink);pointer-events:none}
.osd-state{display:flex;align-items:center;gap:6px}
.osd-ico{display:inline-block;width:0;height:0;border-style:solid;border-width:5px 0 5px 9px;border-color:transparent transparent transparent #fff}
.osd-state[data-s="pause"] .osd-ico{width:9px;height:10px;border:0;background:linear-gradient(90deg,#fff 0 3px,transparent 3px 6px,#fff 6px 9px)}
.osd-state[data-s="none"] .osd-ico{width:9px;height:9px;border:0;background:#fff;opacity:.5}
.screenmsg{position:absolute;left:8px;right:8px;bottom:8px;z-index:4;background:rgba(43,18,51,.92);color:var(--blush);
  padding:6px 10px;font-size:12px;pointer-events:none;box-shadow:0 0 0 2px var(--rose)}

.lcd{display:grid;grid-template-columns:1fr auto;align-items:end;gap:6px 12px;background:var(--ink);padding:14px 16px 16px}
.phase{font-family:var(--px);font-size:10px;color:var(--rose);letter-spacing:1px;margin-bottom:6px}
body[data-phase="break"] .phase{color:var(--mint)}
.time{font-family:var(--lcd);font-size:clamp(76px,19vw,108px);line-height:.8;color:var(--bubble);
  text-shadow:0 0 12px rgba(255,111,174,.55);letter-spacing:2px;font-variant-numeric:tabular-nums}
body[data-phase="break"] .time{color:#d2fff0;text-shadow:0 0 12px rgba(127,224,195,.55)}
.cycles{display:grid;gap:8px;justify-items:end;padding-bottom:6px}
.hearts{display:flex;gap:4px}
.heart{width:21px;height:18px;fill:#4a2358}
.heart.on{fill:var(--rose)}
.cycleN{font-family:var(--px);font-size:9px;color:#c98fb5}
.bar{grid-column:1/-1;display:grid;grid-template-columns:repeat(24,1fr);gap:2px;margin-top:8px}
.bar i{height:10px;background:#4a2358}
.bar i.on{background:var(--rose)}
body[data-phase="break"] .bar i.on{background:var(--mint)}

.row{display:flex;gap:12px;flex-wrap:wrap}
.btn{font-family:var(--px);font-size:10px;letter-spacing:1px;color:var(--ink);border:0;cursor:pointer;
  padding:14px 16px;background:var(--fill,var(--bubble));margin:4px;
  box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink),inset 0 -4px 0 0 rgba(0,0,0,.18),inset 0 4px 0 0 rgba(255,255,255,.35)}
.btn:hover{filter:brightness(1.06)}
.btn:active{transform:translateY(3px);box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink)}
.btn:disabled{opacity:.5;cursor:not-allowed;filter:none}
.btn:focus-visible,input:focus-visible{outline:3px dashed var(--butter);outline-offset:5px}
.btn.big{flex:1 1 150px;--fill:var(--rose);font-size:12px;padding:16px}
.btn.small{flex:0 1 auto}
.lbl{font-family:var(--px);font-size:9px;color:var(--ink);margin:0 0 10px;letter-spacing:1px}
.lbl small{font-family:var(--th);font-size:12px;letter-spacing:0;color:#6b2d5c;margin-left:6px}
.presets{display:grid;grid-template-columns:repeat(4,1fr);gap:8px}
.presets .btn{margin:4px;padding:12px 4px;font-size:10px;text-align:center}
.presets .btn[aria-checked="true"]{--fill:var(--butter)}
.custom{display:flex;flex-wrap:wrap;gap:12px;align-items:end;margin-top:12px}
.custom label{display:grid;gap:6px;font-size:12px;color:var(--ink)}
input[type="number"],input[type="text"],input[type="url"]{
  font-family:var(--th);font-size:14px;color:var(--blush);background:var(--ink);border:0;padding:10px 12px;margin:4px;
  box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink),inset 0 0 0 2px #4a2358}
input[type="number"]{width:96px}
input::placeholder{color:#a97aa5}

.tape{display:grid;grid-template-columns:210px 1fr;gap:16px;align-items:center;background:var(--bubble);padding:14px 16px}
.deckslot{position:relative;height:104px;padding-top:22px}
.cart{position:absolute;left:50%;top:22px;width:172px;height:74px;margin-left:-86px;background:var(--lav);
  box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink),inset 0 4px 0 0 rgba(255,255,255,.35);
  transform:translateY(-16px);transition:transform .45s steps(5)}
.cart[data-state="in"]{transform:none}
.cart[data-src="youtube"]{background:var(--rose)}
.cart[data-src="spotify"]{background:var(--mint)}
.cart[data-src="soundcloud"]{background:var(--butter)}
.cart-label{display:flex;align-items:center;gap:8px;margin:8px 10px;padding:5px 7px;background:var(--blush);
  font-family:var(--px);font-size:8px;color:var(--ink);white-space:nowrap;overflow:hidden}
.cart-label b{font-weight:400;background:var(--ink);color:var(--blush);padding:2px 4px;flex:none}
.cart-label span{overflow:hidden;text-overflow:ellipsis;min-width:0}
.lip{position:absolute;left:0;right:0;top:60px;bottom:0;z-index:2;background:var(--ink);box-shadow:inset 0 4px 0 0 #4a2358;
  display:grid;place-items:end center;padding-bottom:6px;font-family:var(--px);font-size:7px;color:#b27aa3;letter-spacing:1px}
.tapeform{display:grid;gap:10px}
.status{min-height:1.5em;font-size:12px;color:var(--ink);margin:0}
.status.err{color:#8a0f3a;font-weight:600}

.opts{display:flex;flex-wrap:wrap;gap:10px 24px}
.opts label{display:flex;align-items:center;gap:10px;font-size:13px;color:var(--ink);cursor:pointer}
.opts input{appearance:none;-webkit-appearance:none;width:18px;height:18px;margin:4px;background:var(--blush);cursor:pointer;
  box-shadow:0 -4px 0 0 var(--ink),0 4px 0 0 var(--ink),-4px 0 0 0 var(--ink),4px 0 0 0 var(--ink)}
.opts input:checked{background:linear-gradient(var(--rose),var(--rose)) center/8px 8px no-repeat,var(--blush)}

.foot{margin:0;display:grid;gap:6px;font-size:12px;color:#c9a3c7;text-align:center}
.foot p{margin:0}
.filehint{color:var(--butter)}

@media (max-width:520px){
  .device{padding:16px 14px 20px}
  .tape{grid-template-columns:1fr}
  .osd{font-size:8px}
  .presets{grid-template-columns:repeat(2,1fr)}
}
@media (prefers-reduced-motion:reduce){.cart{transition:none}}
</style>
</head>
<body data-phase="work">
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <symbol id="heart" viewBox="0 0 7 6" shape-rendering="crispEdges">
    <path d="M1 0h2v1H1zM4 0h2v1H4zM0 1h7v1H0zM0 2h7v1H0zM1 3h5v1H1zM2 4h3v1H2zM3 5h1v1H3z"/>
  </symbol>
</svg>

<main class="deck">
  <div class="device">
    <div class="topbar">
      <div class="brand"><svg viewBox="0 0 7 6" aria-hidden="true"><use href="#heart"/></svg>POMO DECK</div>
      <div class="leds" aria-hidden="true">
        <span><i class="led" id="ledWork"></i>FOCUS</span>
        <span><i class="led" id="ledBreak"></i>BREAK</span>
      </div>
    </div>

    <div class="bezel">
      <div class="screen" id="screen">
        <canvas id="scene" aria-label="ห้องนั่งทำงานแบบพิกเซล มีแมว หน้าต่างกลางคืน และโคมไฟ"></canvas>
        <div class="host" id="host" hidden></div>
        <div class="crt"></div>
        <div class="osd">
          <div class="osd-state" id="osdState" data-s="none"><i class="osd-ico"></i><span id="osdText">NO TAPE</span></div>
          <div id="osdTime">FOCUS 50:00</div>
        </div>
        <div class="screenmsg" id="screenMsg" hidden></div>
      </div>
    </div>

    <section class="lcd px" aria-label="ตัวจับเวลา">
      <div>
        <div class="phase" id="phase">FOCUS</div>
        <div class="time" id="time" role="timer" aria-live="off">50:00</div>
      </div>
      <div class="cycles">
        <div class="hearts" id="hearts" aria-hidden="true"></div>
        <div class="cycleN" id="cycleN">x0</div>
      </div>
      <div class="bar" id="bar" aria-hidden="true"></div>
    </section>

    <div class="row">
      <button class="btn big" id="btnStart" type="button">START</button>
      <button class="btn small" id="btnReset" type="button">RESET</button>
      <button class="btn small" id="btnSkip" type="button">SKIP</button>
    </div>

    <section>
      <p class="lbl">PRESET<small>โฟกัส · พัก (นาที)</small></p>
      <div class="presets" role="radiogroup" aria-label="พรีเซตเวลา">
        <button class="btn" type="button" role="radio" data-p="25-5" aria-checked="false">25 · 5</button>
        <button class="btn" type="button" role="radio" data-p="30-10" aria-checked="false">30 · 10</button>
        <button class="btn" type="button" role="radio" data-p="50-10" aria-checked="true">50 · 10</button>
        <button class="btn" type="button" role="radio" data-p="custom" aria-checked="false">CUSTOM</button>
      </div>
      <div class="custom" id="custom" hidden>
        <label for="cw">โฟกัส (นาที)<input id="cw" type="number" min="1" max="180" value="45"></label>
        <label for="cb">พัก (นาที)<input id="cb" type="number" min="1" max="60" value="15"></label>
        <button class="btn small" id="btnSet" type="button">SET</button>
      </div>
    </section>

    <section class="tape px" aria-label="ช่องใส่เทปเพลง">
      <div class="deckslot" aria-hidden="true">
        <div class="cart" id="cart" data-state="out" data-src="">
          <div class="cart-label"><b id="cartType">NO TAPE</b><span id="cartId">- - -</span></div>
        </div>
        <div class="lip">INSERT TAPE</div>
      </div>
      <form class="tapeform" id="tapeForm" autocomplete="off">
        <p class="lbl">TAPE<small>วางลิงก์ YouTube / Spotify / SoundCloud</small></p>
        <input id="link" type="text" inputmode="url" spellcheck="false" placeholder="https://www.youtube.com/watch?v=..." aria-label="ลิงก์เพลงหรือ playlist">
        <div class="row">
          <button class="btn small" id="btnInsert" type="submit" style="--fill:var(--rose)">INSERT</button>
          <button class="btn small" id="btnEject" type="button" style="--fill:var(--lav)" disabled>EJECT</button>
        </div>
        <p class="status" id="status" role="status">ยังไม่มีเทปในเครื่อง</p>
      </form>
    </section>

    <div class="opts">
      <label><input type="checkbox" id="optMusic" checked>เปิดเพลงเองตอนเริ่มโฟกัส</label>
      <label><input type="checkbox" id="optNext" checked>เริ่มรอบถัดไปต่อเลย</label>
    </div>
  </div>

  <div class="foot">
    <p>หมดเวลาโฟกัส เพลงจะหยุดเอง และเล่นต่ออีกครั้งตอนเริ่มรอบโฟกัสใหม่</p>
    <p>Spotify ถ้าไม่ได้ล็อกอินในเบราว์เซอร์จะฟังได้แค่ตัวอย่าง 30 วินาที</p>
    <p class="filehint" id="fileHint" hidden>ตอนนี้เปิดจากไฟล์ในเครื่องตรง ๆ YouTube บางคลิปอาจไม่ยอมเล่น (error 153) ให้เปิดผ่าน localhost หรืออัปขึ้นโฮสต์ฟรีอย่าง Netlify Drop หรือ GitHub Pages</p>
  </div>
</main>

<script>
(() => {
'use strict';
const $ = s => document.querySelector(s);
const store = {
  get(k, d){ try{ const v = localStorage.getItem('pomodeck.'+k); return v === null ? d : JSON.parse(v); }catch(e){ return d; } },
  set(k, v){ try{ localStorage.setItem('pomodeck.'+k, JSON.stringify(v)); }catch(e){} }
};

/* ---------- state ---------- */
const S = { phase:'work', running:false, total:0, remaining:0, endAt:0, cycles:0,
  work:50, brk:10, preset:'50-10', autoMusic:true, autoNext:true };
const saved = store.get('prefs', {});
Object.assign(S, ['preset','work','brk','autoMusic','autoNext'].reduce((o,k)=>{ if(saved[k]!==undefined) o[k]=saved[k]; return o; }, {}));
function savePrefs(){ store.set('prefs', { preset:S.preset, work:S.work, brk:S.brk, autoMusic:S.autoMusic, autoNext:S.autoNext }); }
function setDur(){ S.total = (S.phase === 'work' ? S.work : S.brk) * 60000; S.remaining = S.total; }

/* ---------- elements ---------- */
const el = {
  time:$('#time'), phase:$('#phase'), bar:$('#bar'), hearts:$('#hearts'), cycleN:$('#cycleN'),
  start:$('#btnStart'), reset:$('#btnReset'), skip:$('#btnSkip'),
  osdState:$('#osdState'), osdText:$('#osdText'), osdTime:$('#osdTime'), msg:$('#screenMsg'),
  host:$('#host'), cart:$('#cart'), cartType:$('#cartType'), cartId:$('#cartId'),
  link:$('#link'), insert:$('#btnInsert'), eject:$('#btnEject'), status:$('#status'),
  ledWork:$('#ledWork'), ledBreak:$('#ledBreak'),
  custom:$('#custom'), cw:$('#cw'), cb:$('#cb')
};
const barCells = [];
for (let i = 0; i < 24; i++){ const c = document.createElement('i'); el.bar.appendChild(c); barCells.push(c); }
const heartEls = [];
for (let i = 0; i < 4; i++){
  const s = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
  s.setAttribute('viewBox', '0 0 7 6'); s.setAttribute('class', 'heart');
  s.innerHTML = '<use href="#heart"/>';
  el.hearts.appendChild(s); heartEls.push(s);
}

/* ---------- render ---------- */
function render(){
  const secs = Math.ceil(S.remaining / 1000);
  const txt = String(Math.floor(secs / 60)).padStart(2, '0') + ':' + String(secs % 60).padStart(2, '0');
  const name = S.phase === 'work' ? 'FOCUS' : 'BREAK';
  if (el.time.textContent !== txt) el.time.textContent = txt;
  el.phase.textContent = name;
  document.body.dataset.phase = S.phase;
  el.osdTime.textContent = name + ' ' + txt;
  document.title = txt + ' · ' + name + ' · Pomo Deck';
  const filled = Math.round((1 - S.remaining / S.total) * barCells.length);
  barCells.forEach((c, i) => c.classList.toggle('on', i < filled));
  const h = S.cycles === 0 ? 0 : ((S.cycles - 1) % 4) + 1;
  heartEls.forEach((e, i) => e.classList.toggle('on', i < h));
  el.cycleN.textContent = 'x' + S.cycles;
  el.start.textContent = S.running ? 'PAUSE' : 'START';
  el.ledWork.className = 'led' + (S.running && S.phase === 'work' ? ' on-work' : '');
  el.ledBreak.className = 'led' + (S.running && S.phase === 'break' ? ' on-break' : '');
}
function renderOsd(){
  if (!cur){ el.osdState.dataset.s = 'none'; el.osdText.textContent = 'NO TAPE'; return; }
  el.osdState.dataset.s = music.playing ? 'play' : 'pause';
  el.osdText.textContent = music.playing ? 'PLAY' : 'PAUSE';
}
function setStatus(t, err){ el.status.textContent = t; el.status.classList.toggle('err', !!err); }
function setMsg(t){ if (t){ el.msg.textContent = t; el.msg.hidden = false; } else { el.msg.hidden = true; } }

/* ---------- sound ---------- */
let ac = null;
function ensureAudio(){ try{ if (!ac) ac = new (window.AudioContext || window.webkitAudioContext)(); if (ac.state === 'suspended') ac.resume(); }catch(e){} }
function chime(kind){
  if (!ac) return;
  const notes = kind === 'work' ? [523, 659, 784, 1047] : [784, 659];
  notes.forEach((f, i) => {
    const o = ac.createOscillator(), g = ac.createGain(), t0 = ac.currentTime + i * 0.16;
    o.type = 'square'; o.frequency.value = f;
    g.gain.setValueAtTime(0.0001, t0);
    g.gain.exponentialRampToValueAtTime(0.07, t0 + 0.01);
    g.gain.exponentialRampToValueAtTime(0.0001, t0 + 0.15);
    o.connect(g); g.connect(ac.destination); o.start(t0); o.stop(t0 + 0.17);
  });
}

/* ---------- music ---------- */
let cur = null, token = 0;
const music = {
  ad:null, want:false, playing:false,
  play(){ this.want = true; if (this.ad) this.ad.play(); },
  pause(){ this.want = false; if (this.ad) this.ad.pause(); }
};

const scripts = {};
function loadScript(src){
  return scripts[src] || (scripts[src] = new Promise((res, rej) => {
    const s = document.createElement('script');
    s.src = src; s.async = true; s.onload = res;
    s.onerror = () => { delete scripts[src]; rej(new Error('load')); };
    document.head.appendChild(s);
  }));
}
function loadYT(){
  if (window.YT && window.YT.Player) return Promise.resolve();
  return new Promise((res, rej) => {
    const prev = window.onYouTubeIframeAPIReady;
    window.onYouTubeIframeAPIReady = () => { if (prev) prev(); res(); };
    loadScript('https://www.youtube.com/iframe_api').catch(rej);
  });
}
let spP = null;
function loadSpotify(){
  return spP || (spP = new Promise((res, rej) => {
    window.onSpotifyIframeApiReady = api => res(api);
    loadScript('https://open.spotify.com/embed/iframe-api/v1').catch(e => { spP = null; rej(e); });
  }));
}
function loadSC(){ return loadScript('https://w.soundcloud.com/player/api.js').then(() => window.SC); }

function ytErr(c){
  if (c === 101 || c === 150 || c === 153) return 'คลิปนี้ห้ามฝังเล่นนอก YouTube หรือเปิดไฟล์ตรง ๆ ไม่ได้ ลองลิงก์อื่น หรือเปิดผ่านโฮสต์';
  if (c === 100) return 'ไม่พบคลิปนี้ อาจถูกลบหรือตั้งเป็นส่วนตัว';
  if (c === 2) return 'ลิงก์ไม่ถูกต้อง';
  return 'เล่นคลิปนี้ในเครื่องเล่นนี้ไม่ได้';
}

function mkYouTube(src, host, cb){
  const mount = document.createElement('div'); host.appendChild(mount);
  let player = null, ready = false, dead = false;
  loadYT().then(() => {
    if (dead) return;
    const vars = { playsinline:1, rel:0, modestbranding:1, iv_load_policy:3 };
    if (/^https?:$/.test(location.protocol)) vars.origin = location.origin;
    const opt = { width:'100%', height:'100%', playerVars:vars, events:{
      onReady: e => { ready = true; if (src.listId){ try{ e.target.setLoop(true); }catch(_){} } cb.ready(); },
      onError: e => cb.error(ytErr(e.data)),
      onStateChange: e => { cb.state(e.data === 1 || e.data === 3); }
    }};
    if (src.listId){ vars.listType = 'playlist'; vars.list = src.listId; }
    if (src.videoId){ opt.videoId = src.videoId; if (!src.listId){ vars.loop = 1; vars.playlist = src.videoId; } }
    player = new window.YT.Player(mount, opt);
  }).catch(() => cb.error('โหลดตัวเล่น YouTube ไม่ได้ เช็กอินเทอร์เน็ตแล้วลองใหม่นะ'));
  return {
    play(){ if (ready) player.playVideo(); },
    pause(){ if (ready) player.pauseVideo(); },
    destroy(){ dead = true; try{ if (player) player.destroy(); }catch(_){} }
  };
}
function mkSpotify(src, host, cb){
  const mount = document.createElement('div'); host.appendChild(mount);
  let ctrl = null, ready = false, paused = true, dead = false;
  loadSpotify().then(api => {
    if (dead) return;
    api.createController(mount, { uri:src.uri, width:'100%', height:'100%' }, c => {
      ctrl = c;
      if (dead){ try{ c.destroy(); }catch(_){} return; }
      c.addListener('ready', () => { ready = true; cb.ready(); });
      c.addListener('playback_update', e => { paused = !!(e.data && e.data.isPaused); cb.state(!paused); });
    });
  }).catch(() => cb.error('โหลดตัวเล่น Spotify ไม่ได้ เช็กอินเทอร์เน็ตแล้วลองใหม่นะ'));
  return {
    play(){ if (ready && paused) ctrl.togglePlay(); },
    pause(){ if (ready && !paused) ctrl.pause(); },
    destroy(){ dead = true; try{ if (ctrl) ctrl.destroy(); }catch(_){} }
  };
}
function mkSoundCloud(src, host, cb){
  const f = document.createElement('iframe');
  f.setAttribute('allow', 'autoplay');
  f.src = 'https://w.soundcloud.com/player/?url=' + encodeURIComponent(src.url) +
    '&color=%23ff6fae&auto_play=false&hide_related=true&show_comments=false&show_teaser=false&visual=true';
  host.appendChild(f);
  let w = null, ready = false, dead = false;
  loadSC().then(SC => {
    if (dead) return;
    w = SC.Widget(f);
    w.bind(SC.Widget.Events.READY, () => { ready = true; cb.ready(); });
    w.bind(SC.Widget.Events.PLAY, () => cb.state(true));
    w.bind(SC.Widget.Events.PAUSE, () => cb.state(false));
  }).catch(() => cb.error('โหลดตัวเล่น SoundCloud ไม่ได้ เช็กอินเทอร์เน็ตแล้วลองใหม่นะ'));
  return {
    play(){ if (ready) w.play(); },
    pause(){ if (ready) w.pause(); },
    destroy(){ dead = true; }
  };
}

function parseSource(raw){
  raw = (raw || '').trim();
  if (!raw) return null;
  let u;
  try{ u = new URL(/^https?:\/\//i.test(raw) ? raw : 'https://' + raw); }catch(e){ return null; }
  const h = u.hostname.replace(/^(www|m|music)\./, '');
  const parts = u.pathname.split('/').filter(Boolean);
  if (['youtube.com', 'youtu.be', 'youtube-nocookie.com'].includes(h)){
    let v = u.searchParams.get('v'), list = u.searchParams.get('list');
    if (h === 'youtu.be') v = parts[0] || v;
    else if (['shorts', 'embed', 'live', 'v'].includes(parts[0])) v = parts[1] || v;
    let mix = false;
    if (list && /^(RD|LL|WL)/.test(list)){ list = null; mix = true; }
    if (!v && !list) return mix ? { type:'mix' } : null;
    return { type:'youtube', videoId:v || null, listId:list || null, tag:list ? 'YT LIST' : 'YT', id:list || v };
  }
  if (h === 'open.spotify.com'){
    const kinds = ['playlist', 'album', 'track', 'episode', 'show', 'artist'];
    const i = parts.findIndex(p => kinds.includes(p));
    if (i < 0 || !parts[i + 1]) return null;
    return { type:'spotify', uri:'spotify:' + parts[i] + ':' + parts[i + 1], tag:'SPOTIFY', id:parts[i] + ' ' + parts[i + 1].slice(0, 8) };
  }
  if (h === 'soundcloud.com' || h === 'on.soundcloud.com'){
    if (!parts.length) return null;
    return { type:'soundcloud', url:u.href, tag:'SC', id:parts.join('/') };
  }
  return null;
}

function insert(raw, quiet){
  const src = parseSource(raw);
  if (!src){ setStatus('ลิงก์นี้ยังไม่รองรับ ลองลิงก์ YouTube, Spotify หรือ SoundCloud นะ', true); return; }
  if (src.type === 'mix'){ setStatus('ลิงก์ Mix ของ YouTube ฝังเล่นไม่ได้ ลองใช้ playlist ปกติหรือลิงก์คลิปเดี่ยวแทนนะ', true); return; }
  eject(true);
  const my = ++token;
  const guard = f => (...a) => { if (my === token) f(...a); };
  const cb = {
    ready: guard(() => { setMsg(''); if (music.want && music.ad) music.ad.play(); }),
    state: guard(p => { music.playing = p; renderOsd(); }),
    error: guard(m => { setMsg(m); setStatus(m, true); })
  };
  el.host.hidden = false;
  const make = { youtube:mkYouTube, spotify:mkSpotify, soundcloud:mkSoundCloud }[src.type];
  const ad = make(src, el.host, cb);
  cur = { ad, src }; music.ad = ad; music.playing = false;
  el.cart.dataset.src = src.type; el.cart.dataset.state = 'in';
  el.cartType.textContent = src.tag; el.cartId.textContent = src.id;
  el.eject.disabled = false;
  el.link.value = raw.trim();
  store.set('link', raw.trim());
  setStatus(quiet ? 'ใส่เทปเดิมให้แล้ว' : 'ใส่เทปแล้ว กด START เพลงจะเริ่มพร้อมรอบโฟกัส');
  renderOsd();
}
function eject(silent){
  token++;
  if (cur){ cur.ad.destroy(); cur = null; }
  music.ad = null; music.playing = false;
  el.host.innerHTML = ''; el.host.hidden = true;
  el.cart.dataset.state = 'out'; el.cart.dataset.src = '';
  el.cartType.textContent = 'NO TAPE'; el.cartId.textContent = '- - -';
  el.eject.disabled = true;
  setMsg(''); renderOsd();
  if (!silent){ store.set('link', ''); setStatus('ดึงเทปออกแล้ว'); }
}

/* ---------- timer ---------- */
function hardReset(){
  S.running = false; S.phase = 'work'; setDur(); music.pause(); render();
}
function startTimer(){
  ensureAudio();
  S.running = true; S.endAt = Date.now() + S.remaining;
  if (S.phase === 'work' && S.autoMusic) music.play();
  render();
}
function pauseTimer(){
  S.remaining = Math.max(0, S.endAt - Date.now());
  S.running = false; music.pause(); render();
}
function finish(natural){
  const wasWork = S.phase === 'work';
  const keep = natural ? S.autoNext : S.running;
  if (natural){ chime(wasWork ? 'work' : 'break'); if (wasWork) S.cycles++; }
  S.phase = wasWork ? 'break' : 'work';
  setDur();
  if (S.phase === 'break') music.pause();
  if (keep){
    S.running = true; S.endAt = Date.now() + S.remaining;
    if (S.phase === 'work' && S.autoMusic) music.play();
  } else {
    S.running = false; music.pause();
  }
  render();
}
setInterval(() => {
  if (!S.running) return;
  S.remaining = Math.max(0, S.endAt - Date.now());
  if (S.remaining === 0) finish(true); else render();
}, 250);

el.start.addEventListener('click', () => { S.running ? pauseTimer() : startTimer(); });
el.reset.addEventListener('click', () => { S.running = false; setDur(); music.pause(); render(); });
el.skip.addEventListener('click', () => finish(false));
document.addEventListener('keydown', e => {
  if (e.code !== 'Space') return;
  const t = e.target.tagName;
  if (t === 'INPUT' || t === 'BUTTON' || t === 'TEXTAREA') return;
  e.preventDefault(); el.start.click();
});

/* ---------- presets / options ---------- */
const presetBtns = [...document.querySelectorAll('.presets [data-p]')];
function syncPresetUI(){
  presetBtns.forEach(b => b.setAttribute('aria-checked', String(b.dataset.p === S.preset)));
  el.custom.hidden = S.preset !== 'custom';
  if (S.preset === 'custom'){ el.cw.value = S.work; el.cb.value = S.brk; }
}
presetBtns.forEach(b => b.addEventListener('click', () => {
  const p = b.dataset.p;
  if (p === 'custom'){ S.preset = 'custom'; syncPresetUI(); return; }
  const [w, k] = p.split('-').map(Number);
  S.preset = p; S.work = w; S.brk = k;
  syncPresetUI(); hardReset(); savePrefs();
}));
$('#btnSet').addEventListener('click', () => {
  const clamp = (v, lo, hi, d) => { v = Math.round(Number(v)); return Number.isFinite(v) && v >= lo ? Math.min(v, hi) : d; };
  S.work = clamp(el.cw.value, 1, 180, 45); S.brk = clamp(el.cb.value, 1, 60, 15);
  S.preset = 'custom'; syncPresetUI(); hardReset(); savePrefs();
});
$('#optMusic').checked = S.autoMusic;
$('#optNext').checked = S.autoNext;
$('#optMusic').addEventListener('change', e => { S.autoMusic = e.target.checked; savePrefs(); });
$('#optNext').addEventListener('change', e => { S.autoNext = e.target.checked; savePrefs(); });

$('#tapeForm').addEventListener('submit', e => { e.preventDefault(); insert(el.link.value); });
el.eject.addEventListener('click', () => eject(false));

/* ---------- idle scene: pixel room, drawn at 192x108 and scaled up ---------- */
const cv = $('#scene'), cx = cv.getContext('2d');
const W = 192, H = 108;
cv.width = W; cv.height = H;
const R = (x, y, w, h, c) => { cx.fillStyle = c; cx.fillRect(Math.floor(x), Math.floor(y), w, h); };
let seed = 42;
const rand = () => (seed = (seed * 16807) % 2147483647) / 2147483647;
const stars = Array.from({ length:16 }, () => ({ x:62 + Math.floor(rand() * 68), y:16 + Math.floor(rand() * 26), o:Math.floor(rand() * 4) }));
const petals = Array.from({ length:7 }, () => ({ x:rand() * W, y:rand() * H, s:0.3 + rand() * 0.5 }));
const city = [[60,8,14],[68,6,22],[74,9,12],[83,7,18],[90,10,26],[100,6,15],[106,9,20],[115,7,13],[122,10,24]];
const bands = ['#2a1a5e', '#4b2a85', '#8a3f9e', '#d4579f', '#ff8fb0'];
const skyAt = y => bands[Math.min(4, Math.max(0, Math.floor((y - 12) / 11)))];
const HEART = ['.XX.XX.', 'XXXXXXX', 'XXXXXXX', '.XXXXX.', '..XXX..', '...X...'];
function heart(x, y, s, c){ HEART.forEach((row, j) => { for (let i = 0; i < 7; i++) if (row[i] === 'X') R(x + i * s, y + j * s, s, s, c); }); }
const hash = (a, b, c) => (((a * 73856093) ^ (b * 19349663) ^ (c * 83492791)) >>> 0) % 100;

function drawScene(t){
  // wall + dotted wallpaper
  R(0, 0, W, H, '#3a1c47');
  for (let y = 14; y < 84; y += 12) for (let x = ((y / 12) % 2) * 6; x < W; x += 12) R(x, y, 2, 2, '#4a2658');
  // fairy lights
  R(0, 3, W, 1, '#2a1233');
  const lc = ['#ff6fae', '#ffe08a', '#7fe0c3'];
  for (let x = 6; x < W; x += 12) R(x, 4, 2, 3, lc[(Math.floor(x / 12) + (t >> 3)) % 3]);
  // poster
  R(10, 16, 28, 36, '#ffc2dc'); R(12, 18, 24, 32, '#582a63');
  heart(16, 24, 2, '#ff6fae'); R(15, 42, 18, 1, '#ff8fc0'); R(15, 45, 11, 1, '#b9a3ff');
  // window
  R(54, 8, 84, 62, '#ff8fc0'); R(54, 8, 84, 2, '#ffc2dc');
  bands.forEach((c, i) => { const y0 = 12 + i * 11; R(58, y0, 76, Math.min(11, 66 - y0), c); });
  for (let i = 1; i < 5; i++){
    const by = 12 + i * 11;
    for (let x = 58; x < 134; x += 2){ R(x + (i % 2), by - 1, 1, 1, bands[i]); R(x + 1 - (i % 2), by, 1, 1, bands[i - 1]); }
  }
  stars.forEach(s => {
    const ph = ((t >> 2) + s.o) % 4;
    if (ph === 0) return;
    R(s.x, s.y, 1, 1, '#fff1c9');
    if (ph === 1){ R(s.x - 1, s.y, 3, 1, '#fff1c9'); R(s.x, s.y - 1, 1, 3, '#fff1c9'); }
  });
  for (let dy = -6; dy <= 6; dy++) for (let dx = -6; dx <= 6; dx++){
    if (dx * dx + dy * dy <= 36) R(120 + dx, 26 + dy, 1, 1, '#fff1c9');
  }
  for (let dy = -6; dy <= 6; dy++) for (let dx = -6; dx <= 6; dx++){
    if (dx * dx + dy * dy <= 30) R(123 + dx, 24 + dy, 1, 1, skyAt(24 + dy));
  }
  city.forEach(([x, w, h]) => {
    R(x, 66 - h, w, h, '#2d1650');
    for (let yy = 66 - h + 3; yy < 64; yy += 4) for (let xx = x + 2; xx < x + w - 1; xx += 3){
      if (hash(xx, yy, Math.floor(t / 40)) < 35) R(xx, yy, 1, 2, '#ffe08a');
    }
  });
  R(94, 12, 4, 54, '#ff8fc0'); R(58, 37, 76, 3, '#ff8fc0');
  R(50, 70, 92, 4, '#ffc2dc'); R(50, 74, 92, 2, '#d9649f');
  // cat on the sill
  const cat = '#1f0f29', tf = (t >> 3) & 1;
  R(69, 62, 14, 8, cat); R(72, 57, 8, 6, cat); R(72, 55, 2, 2, cat); R(78, 55, 2, 2, cat);
  R(83, 67, 3, 2, cat); R(86, 63 + tf, 2, 5, cat);
  // desk
  R(0, 84, W, 24, '#582a63'); R(0, 84, W, 3, '#7b3a85'); R(0, 87, W, 1, '#3f1c4a');
  // plant
  R(29, 62, 2, 14, '#5ec9a6');
  R(22, 66, 7, 3, '#7fe0c3'); R(31, 60, 8, 3, '#7fe0c3'); R(24, 70, 5, 2, '#5ec9a6');
  R(31, 67, 7, 2, '#5ec9a6'); R(26, 58, 4, 3, '#7fe0c3');
  R(24, 76, 12, 9, '#d9649f'); R(24, 76, 12, 2, '#ff8fc0');
  // laptop
  R(84, 70, 30, 14, '#e8a0c4'); R(86, 72, 26, 10, '#1b0d26');
  R(88, 74, 10, 1, '#ff6fae'); R(88, 77, 16, 1, '#b9a3ff'); R(88, 80, 8, 1, '#7fe0c3');
  if ((t >> 2) & 1) R(98, 79, 2, 2, '#ffffff');
  R(80, 84, 38, 2, '#ffc2dc');
  // mug + steam
  R(140, 77, 9, 7, '#ffc2dc'); R(141, 77, 7, 1, '#8a4b3a');
  R(149, 78, 3, 5, '#ffc2dc'); R(150, 79, 1, 3, '#582a63');
  for (let i = 0; i < 3; i++) R(143 + (((t >> 3) + i) & 1) * 2, 74 - i * 3 - ((t >> 1) % 3), 1, 2, 'rgba(255,230,240,.6)');
  // lamp
  R(168, 66, 2, 18, '#ffc2dc'); R(164, 83, 10, 2, '#ffc2dc');
  R(160, 58, 18, 6, '#ff6fae'); R(160, 58, 18, 1, '#ffb3d1'); R(166, 64, 6, 1, '#ffe08a');
  for (let i = 0; i < 20; i++){ const w = 8 + i * 2; R(169 - w / 2, 65 + i, w, 1, 'rgba(255,224,138,.07)'); }
  // drifting petals
  petals.forEach(p => {
    const x = (p.x + t * p.s * 0.6) % W, y = (p.y + t * p.s * 0.3) % H;
    R(x, y, 2, 1, '#ffb3d1'); R(x + 1, y + 1, 1, 1, '#ff8fc0');
  });
}
let frame = 0;
const still = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;
drawScene(0);
if (!still) setInterval(() => { if (!document.hidden && el.host.hidden) drawScene(++frame); }, 160);

/* ---------- boot ---------- */
if (location.protocol === 'file:') $('#fileHint').hidden = false;
syncPresetUI();
setDur();
render();
renderOsd();
const lastLink = store.get('link', '');
if (lastLink){ el.link.value = lastLink; insert(lastLink, true); }
})();
</script>
</body>
</html>
