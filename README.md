<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daybook</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Big+Shoulders+Display:wght@600;700;800&family=IBM+Plex+Mono:wght@400;500;600&family=Inter:wght@400;500;600;700&display=swap">
<style>
  :root{
    --ink:#14181D;
    --panel:#1C222A;
    --panel-2:#242B34;
    --line:#2C3440;
    --paper:#EDEAE2;
    --muted:#8B93A0;
    --amber:#D89B4A;
    --sage:#8FAE82;
    --rose:#D97C82;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--ink);
    color:var(--paper);
    font-family:'Inter',sans-serif;
    -webkit-font-smoothing:antialiased;
    min-height:100vh;
  }
  .wrap{max-width:880px;margin:0 auto;padding:28px 20px 80px;}
  .head{display:flex;justify-content:space-between;align-items:flex-end;gap:16px;margin-bottom:22px;flex-wrap:wrap;}
  .word{
    font-family:'Big Shoulders Display',sans-serif;
    font-weight:800;
    font-size:42px;
    letter-spacing:0.5px;
    line-height:1;
    color:var(--paper);
    text-transform:uppercase;
  }
  .word span{color:var(--amber);}
  .sub{color:var(--muted);font-size:13px;margin-top:4px;letter-spacing:0.2px;}
  .datebar{display:flex;align-items:center;gap:8px;}
  .datebar input[type=date]{
    background:var(--panel-2);border:1px solid var(--line);color:var(--paper);
    font-family:'IBM Plex Mono',monospace;font-size:13px;border-radius:6px;padding:7px 9px;
    color-scheme:dark;
  }
  button{font-family:'Inter',sans-serif;cursor:pointer;}
  .iconbtn{
    background:var(--panel-2);border:1px solid var(--line);color:var(--paper);
    width:32px;height:32px;border-radius:6px;font-size:15px;display:flex;align-items:center;justify-content:center;
  }
  .iconbtn:hover{border-color:var(--amber);}
  .todaybtn{
    background:transparent;border:1px solid var(--line);color:var(--muted);
    font-size:12px;padding:7px 10px;border-radius:6px;
  }
  .todaybtn:hover{color:var(--paper);border-color:var(--amber);}

  /* Instrument cluster */
  .cluster{
    display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-bottom:22px;
  }
  .gauge-card{
    background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:16px 14px 14px;
    position:relative;overflow:hidden;
  }
  .gauge-card::before{
    content:'';position:absolute;top:0;left:0;right:0;height:2px;
  }
  .gauge-card.habits::before{background:var(--sage);}
  .gauge-card.mood::before{background:var(--rose);}
  .gauge-card.spend::before{background:var(--amber);}
  .gauge-label{
    font-family:'IBM Plex Mono',monospace;font-size:10.5px;letter-spacing:1.5px;color:var(--muted);
    text-transform:uppercase;margin-bottom:8px;
  }
  .gauge-body{display:flex;align-items:center;justify-content:center;height:96px;}
  .ring-num{font-family:'Big Shoulders Display',sans-serif;font-weight:700;font-size:26px;}
  .ring-sub{font-family:'IBM Plex Mono',monospace;font-size:10px;color:var(--muted);text-align:center;margin-top:2px;}
  .gauge-readout{text-align:center;}
  .mood-readout{font-family:'Big Shoulders Display',sans-serif;font-weight:700;font-size:18px;text-align:center;margin-top:-6px;}
  .odo{display:flex;justify-content:center;gap:2px;flex-wrap:wrap;}
  .odo span{
    background:var(--panel-2);border:1px solid var(--line);border-radius:4px;
    font-family:'IBM Plex Mono',monospace;font-weight:600;font-size:20px;color:var(--amber);
    padding:6px 5px;min-width:14px;text-align:center;
    box-shadow:inset 0 1px 3px rgba(0,0,0,0.5);
  }
  .odo span.sym{color:var(--muted);font-weight:400;background:transparent;border:none;box-shadow:none;padding:6px 1px;}

  /* Panels */
  .panels{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:14px;}
  .panel{
    background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:16px;
  }
  .panel.full{grid-column:1 / -1;}
  .panel h2{
    font-family:'Big Shoulders Display',sans-serif;font-weight:700;font-size:17px;
    letter-spacing:0.3px;text-transform:uppercase;margin:0 0 12px;color:var(--paper);
    display:flex;align-items:center;gap:8px;
  }
  .dot{width:8px;height:8px;border-radius:50%;display:inline-block;}
  .dot.sage{background:var(--sage);} .dot.rose{background:var(--rose);} .dot.amber{background:var(--amber);}

  /* Habits */
  .habit-row{
    display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--line);
  }
  .habit-row:last-of-type{border-bottom:none;}
  .check{
    width:20px;height:20px;border-radius:5px;border:1.5px solid var(--line);background:var(--panel-2);
    flex-shrink:0;display:flex;align-items:center;justify-content:center;
  }
  .check.on{background:var(--sage);border-color:var(--sage);}
  .check.on::after{content:'✓';color:var(--ink);font-size:13px;font-weight:700;}
  .habit-name{flex:1;font-size:14px;}
  .habit-del{
    background:none;border:none;color:var(--muted);font-size:15px;opacity:0;transition:opacity .15s;
  }
  .habit-row:hover .habit-del{opacity:1;}
  .habit-del:hover{color:var(--rose);}
  .addrow{display:flex;gap:8px;margin-top:10px;}
  .addrow input, select, textarea{
    background:var(--panel-2);border:1px solid var(--line);color:var(--paper);
    border-radius:6px;padding:8px 10px;font-size:13px;font-family:'Inter',sans-serif;
  }
  .addrow input{flex:1;}
  .smallbtn{
    background:var(--amber);border:none;color:var(--ink);font-weight:600;font-size:13px;
    border-radius:6px;padding:8px 14px;white-space:nowrap;
  }
  .smallbtn:hover{filter:brightness(1.08);}
  .smallbtn.secondary{background:var(--panel-2);color:var(--paper);border:1px solid var(--line);}
  .empty{color:var(--muted);font-size:13px;padding:6px 0;}

  /* Mood */
  .mood-scale{display:flex;gap:6px;margin-bottom:10px;}
  .mood-opt{
    flex:1;background:var(--panel-2);border:1px solid var(--line);border-radius:8px;
    padding:9px 4px;text-align:center;color:var(--muted);font-size:11px;
  }
  .mood-opt b{display:block;font-family:'Big Shoulders Display',sans-serif;font-size:18px;color:var(--paper);font-weight:700;}
  .mood-opt.selected{border-color:var(--rose);background:rgba(217,124,130,0.12);color:var(--paper);}
  .mood-opt.selected b{color:var(--rose);}
  textarea{width:100%;resize:vertical;min-height:54px;font-size:13px;margin-bottom:8px;}

  /* Expenses */
  .exp-form{display:flex;gap:8px;margin-bottom:12px;flex-wrap:wrap;}
  .exp-form input[type=number]{width:90px;}
  .exp-list{display:flex;flex-direction:column;gap:0;}
  .exp-row{
    display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px solid var(--line);font-size:13px;
  }
  .exp-row:last-child{border-bottom:none;}
  .exp-amt{font-family:'IBM Plex Mono',monospace;color:var(--amber);font-weight:600;min-width:64px;}
  .exp-cat{color:var(--muted);font-size:11px;background:var(--panel-2);padding:3px 7px;border-radius:5px;}
  .exp-note{flex:1;color:var(--muted);}
  .exp-del{background:none;border:none;color:var(--muted);font-size:14px;opacity:0;transition:opacity .15s;}
  .exp-row:hover .exp-del{opacity:1;}
  .exp-del:hover{color:var(--rose);}
  .exp-total{margin-top:10px;text-align:right;font-family:'IBM Plex Mono',monospace;font-size:13px;color:var(--muted);}
  .exp-total b{color:var(--amber);}

  /* History */
  .hist-row{margin-bottom:14px;}
  .hist-row:last-child{margin-bottom:0;}
  .hist-label{font-family:'IBM Plex Mono',monospace;font-size:10.5px;letter-spacing:1px;color:var(--muted);text-transform:uppercase;margin-bottom:6px;}
  svg.histsvg{width:100%;height:auto;display:block;}

  .reset-row{text-align:center;margin-top:24px;}
  .reset-link{background:none;border:none;color:var(--muted);font-size:12px;text-decoration:underline;}
  .reset-link:hover{color:var(--rose);}

  .loading{text-align:center;color:var(--muted);font-size:13px;padding:60px 0;font-family:'IBM Plex Mono',monospace;}

  button:focus-visible, input:focus-visible, select:focus-visible, textarea:focus-visible{
    outline:2px solid var(--amber);outline-offset:1px;
  }

  @media(max-width:680px){
    .cluster{grid-template-columns:1fr;}
    .panels{grid-template-columns:1fr;}
    .word{font-size:32px;}
  }
  @media (prefers-reduced-motion: reduce){
    *{transition:none !important;animation:none !important;}
  }
</style>
</head>
<body>
<div class="wrap" id="app">
  <div class="loading" id="loading">Loading your daybook…</div>
</div>

<script>
const CATS = ['Food','Transport','Shopping','Bills','Fun','Other'];
const MOOD_LABELS = ['Rough','Low','Okay','Good','Great'];
const MOOD_COLORS = ['#D97C82','#D9907A','#D9A85A','#B3AE6C','#8FAE82'];

let state = { habits: [], logs: {} };
let selectedDate = todayStr();

function todayStr(d=new Date()){
  const off = d.getTimezoneOffset();
  const local = new Date(d.getTime() - off*60000);
  return local.toISOString().slice(0,10);
}
function fmtDateLabel(ds){
  const [y,m,d] = ds.split('-').map(Number);
  const dt = new Date(y, m-1, d);
  return dt.toLocaleDateString(undefined,{weekday:'short', month:'short', day:'numeric'});
}
function fmtMoney(n){ return '$' + n.toFixed(2); }
function uid(p){ return p + Date.now().toString(36) + Math.random().toString(36).slice(2,6); }

function getLog(ds, create){
  if(!state.logs[ds]){
    if(!create) return { habitsDone:[], mood:null, expenses:[] };
    state.logs[ds] = { habitsDone:[], mood:null, expenses:[] };
  }
  return state.logs[ds];
}

async function save(){
  try{
    const res = await window.storage.set('daybook-data', JSON.stringify(state), false);
    if(!res) console.error('Save returned no result');
  }catch(e){
    console.error('Save failed', e);
  }
}

async function load(){
  try{
    const res = await window.storage.get('daybook-data', false);
    if(res && res.value){
      state = JSON.parse(res.value);
      if(!state.habits) state.habits = [];
      if(!state.logs) state.logs = {};
    }
  }catch(e){
    state = { habits: [], logs: {} };
  }
  render();
}

function shiftDate(days){
  const [y,m,d] = selectedDate.split('-').map(Number);
  const dt = new Date(y, m-1, d);
  dt.setDate(dt.getDate()+days);
  selectedDate = todayStr(dt);
  render();
}

function render(){
  const app = document.getElementById('app');
  const log = getLog(selectedDate, false);

  const habitsTotal = state.habits.length;
  const habitsDone = log.habitsDone.length;
  const habitFrac = habitsTotal ? habitsDone/habitsTotal : 0;

  const moodScore = log.mood ? log.mood.score : null;
  const spendTotal = log.expenses.reduce((s,e)=>s+e.amount,0);

  app.innerHTML = `
    <div class="head">
      <div>
        <div class="word">DAY<span>BOOK</span></div>
        <div class="sub">Your habits, mood, and spending — one page a day.</div>
      </div>
      <div class="datebar">
        <button class="iconbtn" id="prevDay">‹</button>
        <input type="date" id="dateInput" value="${selectedDate}">
        <button class="iconbtn" id="nextDay">›</button>
        <button class="todaybtn" id="todayBtn">Today</button>
      </div>
    </div>

    <div class="cluster">
      <div class="gauge-card habits">
        <div class="gauge-label">Habits — ${fmtDateLabel(selectedDate)}</div>
        <div class="gauge-body">${ringSvg(habitFrac, habitsDone, habitsTotal)}</div>
      </div>
      <div class="gauge-card mood">
        <div class="gauge-label">Mood</div>
        <div class="gauge-body" style="flex-direction:column;">${moodSvg(moodScore)}</div>
      </div>
      <div class="gauge-card spend">
        <div class="gauge-label">Spent today</div>
        <div class="gauge-body">${odometerHtml(spendTotal)}</div>
      </div>
    </div>

    <div class="panels">
      <div class="panel">
        <h2><span class="dot sage"></span>Habits</h2>
        ${renderHabits(log)}
      </div>
      <div class="panel">
        <h2><span class="dot rose"></span>Mood check-in</h2>
        ${renderMood(log)}
      </div>
      <div class="panel full">
        <h2><span class="dot amber"></span>Expenses</h2>
        ${renderExpenses(log, spendTotal)}
      </div>
      <div class="panel full">
        <h2>Last 14 days</h2>
        ${renderHistory()}
      </div>
    </div>

    <div class="reset-row"><button class="reset-link" id="resetBtn">Reset all data</button></div>
  `;

  // wire events
  document.getElementById('prevDay').onclick = ()=>shiftDate(-1);
  document.getElementById('nextDay').onclick = ()=>shiftDate(1);
  document.getElementById('todayBtn').onclick = ()=>{ selectedDate = todayStr(); render(); };
  document.getElementById('dateInput').onchange = (e)=>{ if(e.target.value){ selectedDate = e.target.value; render(); } };
  document.getElementById('resetBtn').onclick = async ()=>{
    if(confirm('This clears all habits, mood entries, and expenses. This cannot be undone. Continue?')){
      state = { habits: [], logs: {} };
      await save();
      render();
    }
  };

  wireHabitEvents(log);
  wireMoodEvents(log);
  wireExpenseEvents(log);
}

/* ---------- Gauges ---------- */
function ringSvg(frac, done, total){
  const r = 36, c = 2*Math.PI*r;
  const offset = c * (1-frac);
  return `
    <svg width="96" height="96" viewBox="0 0 96 96">
      <circle cx="48" cy="48" r="${r}" fill="none" stroke="var(--line)" stroke-width="8"/>
      <circle cx="48" cy="48" r="${r}" fill="none" stroke="var(--sage)" stroke-width="8"
        stroke-linecap="round" stroke-dasharray="${c}" stroke-dashoffset="${offset}"
        transform="rotate(-90 48 48)"/>
      <text x="48" y="44" text-anchor="middle" font-family="Big Shoulders Display" font-weight="700"
        font-size="22" fill="var(--paper)">${done}/${total}</text>
      <text x="48" y="60" text-anchor="middle" font-family="IBM Plex Mono" font-size="9"
        fill="var(--muted)">DONE</text>
    </svg>`;
}

function moodSvg(score){
  const cx=80, cy=72, r=62;
  const segs = 5;
  let arcs = '';
  for(let i=0;i<segs;i++){
    const a1 = 180 - i*36, a2 = 180 - (i+1)*36;
    arcs += arcPath(cx,cy,r,a1,a2,MOOD_COLORS[i]);
  }
  let needle = '';
  if(score){
    const theta = (180 - (score-1)*45) * Math.PI/180;
    const x2 = cx + (r-14)*Math.cos(theta);
    const y2 = cy - (r-14)*Math.sin(theta);
    needle = `<line x1="${cx}" y1="${cy}" x2="${x2.toFixed(1)}" y2="${y2.toFixed(1)}"
      stroke="var(--paper)" stroke-width="3" stroke-linecap="round"/>
      <circle cx="${cx}" cy="${cy}" r="5" fill="var(--paper)"/>`;
  }
  const label = score ? MOOD_LABELS[score-1] : '—';
  const labelColor = score ? MOOD_COLORS[score-1] : 'var(--muted)';
  return `
    <svg width="160" height="92" viewBox="0 0 160 92">
      ${arcs}
      ${needle}
    </svg>
    <div class="mood-readout" style="color:${labelColor}">${label}</div>
  `;
}
function arcPath(cx,cy,r,a1,a2,color){
  const t1 = a1*Math.PI/180, t2 = a2*Math.PI/180;
  const x1 = cx + r*Math.cos(t1), y1 = cy - r*Math.sin(t1);
  const x2 = cx + r*Math.cos(t2), y2 = cy - r*Math.sin(t2);
  return `<path d="M ${x1.toFixed(1)} ${y1.toFixed(1)} A ${r} ${r} 0 0 1 ${x2.toFixed(1)} ${y2.toFixed(1)}"
    fill="none" stroke="${color}" stroke-width="11" stroke-linecap="butt"/>`;
}

function odometerHtml(total){
  const str = fmtMoney(total);
  return `<div class="odo">` + [...str].map(ch=>{
    if(ch === '$' || ch === '.') return `<span class="sym">${ch}</span>`;
    return `<span>${ch}</span>`;
  }).join('') + `</div>`;
}

/* ---------- Habits ---------- */
function renderHabits(log){
  if(state.habits.length === 0){
    return `<div class="empty">No habits yet. Add one to start tracking your days.</div>
      <div class="addrow">
        <input type="text" id="newHabit" placeholder="e.g. Drink water" maxlength="40">
        <button class="smallbtn" id="addHabitBtn">Add</button>
      </div>`;
  }
  const rows = state.habits.map(h=>{
    const on = log.habitsDone.includes(h.id);
    return `<div class="habit-row" data-id="${h.id}">
      <div class="check ${on?'on':''}" data-toggle="${h.id}"></div>
      <div class="habit-name">${escapeHtml(h.name)}</div>
      <button class="habit-del" data-del="${h.id}">✕</button>
    </div>`;
  }).join('');
  return rows + `
    <div class="addrow">
      <input type="text" id="newHabit" placeholder="Add another habit" maxlength="40">
      <button class="smallbtn" id="addHabitBtn">Add</button>
    </div>`;
}
function wireHabitEvents(log){
  const addBtn = document.getElementById('addHabitBtn');
  if(addBtn){
    addBtn.onclick = async ()=>{
      const input = document.getElementById('newHabit');
      const name = input.value.trim();
      if(!name) return;
      state.habits.push({ id: uid('h'), name });
      await save();
      render();
    };
    document.getElementById('newHabit').onkeydown = (e)=>{ if(e.key==='Enter') addBtn.click(); };
  }
  document.querySelectorAll('[data-toggle]').forEach(el=>{
    el.onclick = async ()=>{
      const id = el.getAttribute('data-toggle');
      const l = getLog(selectedDate, true);
      const idx = l.habitsDone.indexOf(id);
      if(idx === -1) l.habitsDone.push(id); else l.habitsDone.splice(idx,1);
      await save();
      render();
    };
  });
  document.querySelectorAll('[data-del]').forEach(el=>{
    el.onclick = async ()=>{
      const id = el.getAttribute('data-del');
      state.habits = state.habits.filter(h=>h.id!==id);
      Object.values(state.logs).forEach(l=>{ l.habitsDone = l.habitsDone.filter(x=>x!==id); });
      await save();
      render();
    };
  });
}

/* ---------- Mood ---------- */
function renderMood(log){
  const opts = MOOD_LABELS.map((lab,i)=>{
    const score = i+1;
    const sel = log.mood && log.mood.score === score ? 'selected' : '';
    return `<div class="mood-opt ${sel}" data-score="${score}"><b>${score}</b>${lab}</div>`;
  }).join('');
  return `
    <div class="mood-scale">${opts}</div>
    <textarea id="moodNote" placeholder="Anything notable about today? (optional)">${log.mood ? escapeHtml(log.mood.note||'') : ''}</textarea>
    <button class="smallbtn" id="saveMoodBtn">Save mood</button>
  `;
}
function wireMoodEvents(log){
  let picked = log.mood ? log.mood.score : null;
  document.querySelectorAll('.mood-opt').forEach(el=>{
    el.onclick = ()=>{
      picked = Number(el.getAttribute('data-score'));
      document.querySelectorAll('.mood-opt').forEach(o=>o.classList.remove('selected'));
      el.classList.add('selected');
    };
  });
  document.getElementById('saveMoodBtn').onclick = async ()=>{
    if(!picked){ alert('Pick a mood first.'); return; }
    const l = getLog(selectedDate, true);
    const note = document.getElementById('moodNote').value.trim();
    l.mood = { score: picked, note };
    await save();
    render();
  };
}

/* ---------- Expenses ---------- */
function renderExpenses(log, total){
  const catOpts = CATS.map(c=>`<option value="${c}">${c}</option>`).join('');
  const rows = log.expenses.length ? log.expenses.map(e=>`
    <div class="exp-row" data-id="${e.id}">
      <span class="exp-amt">${fmtMoney(e.amount)}</span>
      <span class="exp-cat">${e.category}</span>
      <span class="exp-note">${escapeHtml(e.note||'')}</span>
      <button class="exp-del" data-edel="${e.id}">✕</button>
    </div>`).join('') : `<div class="empty">No expenses logged for this day.</div>`;
  return `
    <div class="exp-form">
      <input type="number" id="expAmt" placeholder="0.00" min="0" step="0.01">
      <select id="expCat">${catOpts}</select>
      <input type="text" id="expNote" placeholder="Note (optional)" style="flex:1;min-width:120px;">
      <button class="smallbtn" id="addExpBtn">Add</button>
    </div>
    <div class="exp-list">${rows}</div>
    <div class="exp-total">Total: <b>${fmtMoney(total)}</b></div>
  `;
}
function wireExpenseEvents(log){
  document.getElementById('addExpBtn').onclick = async ()=>{
    const amtEl = document.getElementById('expAmt');
    const amount = parseFloat(amtEl.value);
    if(!amount || amount <= 0){ alert('Enter an amount greater than 0.'); return; }
    const category = document.getElementById('expCat').value;
    const note = document.getElementById('expNote').value.trim();
    const l = getLog(selectedDate, true);
    l.expenses.push({ id: uid('e'), amount, category, note });
    await save();
    render();
  };
  document.querySelectorAll('[data-edel]').forEach(el=>{
    el.onclick = async ()=>{
      const id = el.getAttribute('data-edel');
      const l = getLog(selectedDate, true);
      l.expenses = l.expenses.filter(e=>e.id!==id);
      await save();
      render();
    };
  });
}

/* ---------- History ---------- */
function lastNDates(n){
  const out = [];
  const t = new Date();
  for(let i=n-1;i>=0;i--){
    const d = new Date(t);
    d.setDate(t.getDate()-i);
    out.push(todayStr(d));
  }
  return out;
}
function renderHistory(){
  const dates = lastNDates(14);
  const spends = dates.map(ds=>{
    const l = state.logs[ds];
    return l ? l.expenses.reduce((s,e)=>s+e.amount,0) : 0;
  });
  const moods = dates.map(ds=>{
    const l = state.logs[ds];
    return l && l.mood ? l.mood.score : null;
  });
  const habitFracs = dates.map(ds=>{
    const l = state.logs[ds];
    if(!l || state.habits.length===0) return 0;
    return l.habitsDone.length / state.habits.length;
  });

  return `
    <div class="hist-row">
      <div class="hist-label">Spending</div>
      ${barChart(spends, '#D89B4A', v=>fmtMoney(v))}
    </div>
    <div class="hist-row">
      <div class="hist-label">Mood</div>
      ${dotChart(moods)}
    </div>
    <div class="hist-row">
      <div class="hist-label">Habits completed</div>
      ${barChart(habitFracs, '#8FAE82', v=>Math.round(v*100)+'%', true)}
    </div>
  `;
}
function barChart(values, color, fmt, isFrac){
  const w = 700, h = 60, n = values.length, gap = 4;
  const bw = (w - gap*(n-1)) / n;
  const max = isFrac ? 1 : Math.max(1, ...values);
  const bars = values.map((v,i)=>{
    const bh = max ? (v/max)*44 : 0;
    const x = i*(bw+gap);
    const y = h - 10 - bh;
    return `<rect x="${x.toFixed(1)}" y="${y.toFixed(1)}" width="${bw.toFixed(1)}" height="${bh.toFixed(1)}" rx="2" fill="${color}" opacity="${v>0?1:0.18}"><title>${fmt(v)}</title></rect>`;
  }).join('');
  return `<svg class="histsvg" viewBox="0 0 ${w} ${h}" preserveAspectRatio="none">${bars}
    <line x1="0" y1="${h-10}" x2="${w}" y2="${h-10}" stroke="var(--line)" stroke-width="1"/>
  </svg>`;
}
function dotChart(values){
  const w = 700, h = 60, n = values.length;
  const stepX = w / (n-1 || 1);
  let path = '', dots = '';
  let first = true;
  values.forEach((v,i)=>{
    if(v===null) { first = true; return; }
    const x = i*stepX;
    const y = h - 10 - ((v-1)/4)*40;
    dots += `<circle cx="${x.toFixed(1)}" cy="${y.toFixed(1)}" r="4" fill="${MOOD_COLORS[v-1]}"><title>${MOOD_LABELS[v-1]}</title></circle>`;
    if(!first){
      path += '';
    }
    first = false;
  });
  // connecting lines between consecutive known points
  let lines = '';
  let prev = null;
  values.forEach((v,i)=>{
    if(v===null){ prev=null; return; }
    const x = i*stepX, y = h - 10 - ((v-1)/4)*40;
    if(prev){
      lines += `<line x1="${prev.x.toFixed(1)}" y1="${prev.y.toFixed(1)}" x2="${x.toFixed(1)}" y2="${y.toFixed(1)}" stroke="var(--line)" stroke-width="1.5"/>`;
    }
    prev = {x,y};
  });
  return `<svg class="histsvg" viewBox="0 0 ${w} ${h}" preserveAspectRatio="none">
    <line x1="0" y1="${h-10}" x2="${w}" y2="${h-10}" stroke="var(--line)" stroke-width="1"/>
    ${lines}${dots}
  </svg>`;
}

function escapeHtml(s){
  const d = document.createElement('div');
  d.textContent = s;
  return d.innerHTML;
}

load();
</script>
</body>
</html>
[daybook (2).html](https://github.com/user-attachments/files/29161340/daybook.2.html)
# proj2012u.github.io
Daybook is a simple, private daily tracker for habits, mood, and expenses. No account, no subscription — just one clean page to organize your day.
