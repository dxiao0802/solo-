<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>修煉者計畫 — 單機版</title>
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0a0c12">
<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Rajdhani:wght@500;600;700&display=swap');
:root {
  --bg:#0a0c12;--surface:#12151f;--surface2:#1a1f2e;
  --border:rgba(255,255,255,0.07);
  --accent:#00d4ff;--purple:#7c3aed;--gold:#f59e0b;
  --danger:#ef4444;--success:#10b981;
  --text:#e2e8f0;--muted:#64748b;
  --mono:'Space Mono',monospace;--body:'Rajdhani',sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);color:var(--text);font-family:var(--body);min-height:100vh;max-width:480px;margin:0 auto;}

/* Nav */
.nav{display:flex;background:var(--surface);border-bottom:1px solid var(--border);position:sticky;top:0;z-index:10;}
.nav-btn{flex:1;padding:11px 4px;background:none;border:none;border-bottom:2px solid transparent;color:var(--muted);font-family:var(--mono);font-size:10px;letter-spacing:.08em;cursor:pointer;transition:all .2s;text-transform:uppercase;}
.nav-btn.active{color:var(--accent);border-bottom-color:var(--accent);}

/* Screens */
.screen{display:none;padding:16px;}
.screen.active{display:block;}

/* Header */
.app-header{padding:14px 16px 0;display:flex;justify-content:space-between;align-items:center;}
.app-header h2{font-family:var(--mono);font-size:14px;color:var(--accent);}
.app-header p{font-size:12px;color:var(--muted);}
.tier-pill{background:rgba(124,58,237,.15);border:1px solid rgba(124,58,237,.3);border-radius:8px;padding:5px 10px;font-family:var(--mono);font-size:11px;color:#c4b5fd;}

/* Ring area */
.ring-area{display:flex;align-items:center;justify-content:center;gap:24px;margin:12px 0 20px;}
.ring-wrap{position:relative;flex-shrink:0;}
.ring-svg{filter:drop-shadow(0 0 18px #00d4ff66);transition:filter .5s;}
.ring-center{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);text-align:center;}
.ring-pct{font-family:var(--mono);font-size:24px;font-weight:700;}
.ring-sub{font-size:10px;color:var(--muted);letter-spacing:.1em;margin-top:2px;}

/* Score panel */
.score-panel{display:flex;flex-direction:column;gap:8px;}
.score-big{font-family:var(--mono);font-size:36px;font-weight:700;color:var(--gold);line-height:1;}
.score-label{font-size:11px;color:var(--muted);letter-spacing:.1em;text-transform:uppercase;}
.score-max{font-family:var(--mono);font-size:12px;color:var(--muted);}
.streak-badge{font-family:var(--mono);font-size:12px;color:var(--gold);}

/* Section label */
.section-label{font-family:var(--mono);font-size:10px;letter-spacing:.15em;color:var(--muted);text-transform:uppercase;margin-bottom:10px;}

/* Habit rows */
.habit-row{display:flex;align-items:center;gap:10px;padding:11px 13px;background:var(--surface);border:1px solid var(--border);border-radius:10px;margin-bottom:8px;transition:border-color .2s;}
.habit-row.done{border-color:rgba(16,185,129,.3);background:rgba(16,185,129,.04);}
.habit-row.done .habit-name{text-decoration:line-through;color:var(--muted);}
.check-btn{width:22px;height:22px;border-radius:50%;border:1.5px solid var(--border);background:none;cursor:pointer;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:all .2s;}
.habit-row.done .check-btn{background:var(--success);border-color:var(--success);}
.check-btn svg{opacity:0;transition:opacity .2s;}
.habit-row.done .check-btn svg{opacity:1;}
.habit-name{flex:1;font-size:14px;font-weight:600;}
.habit-pts{font-family:var(--mono);font-size:12px;color:var(--gold);min-width:36px;text-align:right;}
.edit-btn{background:none;border:none;color:var(--muted);cursor:pointer;font-size:14px;padding:2px 4px;border-radius:4px;transition:color .2s;}
.edit-btn:hover{color:var(--accent);}
.del-btn{background:none;border:none;color:var(--muted);cursor:pointer;font-size:14px;padding:2px 4px;border-radius:4px;transition:color .2s;}
.del-btn:hover{color:var(--danger);}

/* Add habit */
.add-btn{width:100%;padding:11px;background:none;border:1px dashed var(--border);border-radius:10px;color:var(--muted);font-family:var(--body);font-size:14px;cursor:pointer;transition:all .2s;margin-top:4px;}
.add-btn:hover{border-color:rgba(0,212,255,.3);color:var(--accent);}

/* Stats */
.stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px;}
.stat-card{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:12px 14px;}
.stat-val{font-family:var(--mono);font-size:22px;font-weight:700;color:var(--text);}
.stat-label{font-size:12px;color:var(--muted);margin-top:2px;}

/* Weekly chart */
.week-row{display:flex;gap:6px;justify-content:center;margin:12px 0;}
.day-col{display:flex;flex-direction:column;align-items:center;gap:4px;}
.day-bar-wrap{width:28px;height:80px;background:var(--surface2);border-radius:4px;display:flex;align-items:flex-end;overflow:hidden;}
.day-bar{width:100%;border-radius:4px;background:linear-gradient(180deg,var(--accent),var(--purple));transition:height .5s ease;}
.day-label{font-family:var(--mono);font-size:9px;color:var(--muted);}
.day-pts{font-family:var(--mono);font-size:9px;color:var(--gold);}

/* Modal */
.modal-bg{display:none;position:fixed;inset:0;background:rgba(10,12,18,.85);z-index:100;align-items:center;justify-content:center;padding:20px;}
.modal-bg.show{display:flex;}
.modal{background:var(--surface);border:1px solid var(--border);border-radius:16px;padding:22px 20px;width:100%;max-width:340px;}
.modal h3{font-size:16px;font-weight:700;margin-bottom:16px;}
.modal label{font-size:13px;color:var(--muted);display:block;margin-bottom:4px;margin-top:12px;}
.modal input,.modal select{width:100%;padding:9px 12px;background:var(--surface2);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:var(--body);font-size:14px;}
.modal input:focus,.modal select:focus{outline:none;border-color:rgba(0,212,255,.4);}
.preset-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:8px;}
.preset-btn{padding:8px;background:var(--surface2);border:1px solid var(--border);border-radius:8px;color:var(--text);font-size:13px;cursor:pointer;text-align:left;transition:border-color .2s;}
.preset-btn:hover{border-color:rgba(0,212,255,.3);}
.preset-pts{font-family:var(--mono);font-size:11px;color:var(--gold);}
.modal-btns{display:flex;gap:8px;margin-top:16px;}
.btn-primary{flex:1;padding:11px;background:linear-gradient(135deg,var(--purple),#4f46e5);border:none;border-radius:8px;color:#fff;font-family:var(--body);font-size:15px;font-weight:700;cursor:pointer;}
.btn-secondary{flex:1;padding:11px;background:none;border:1px solid var(--border);border-radius:8px;color:var(--muted);font-family:var(--body);font-size:15px;cursor:pointer;}

/* Toast */
#toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:var(--surface2);border:1px solid var(--border);border-radius:20px;padding:8px 20px;font-family:var(--mono);font-size:12px;color:var(--accent);opacity:0;transition:opacity .3s;pointer-events:none;z-index:200;white-space:nowrap;}
#toast.show{opacity:1;}
</style>
</head>
<body>

<div class="app-header" id="header">
  <div><h2>修煉者計畫</h2><p>單機版 · Solo Mode</p></div>
  <div class="tier-pill" id="tier-pill">🔥 載入中</div>
</div>

<nav class="nav">
  <button class="nav-btn active" onclick="switchTab('home',this)">主頁</button>
  <button class="nav-btn" onclick="switchTab('stats',this)">統計</button>
  <button class="nav-btn" onclick="switchTab('settings',this)">設定</button>
</nav>

<!-- 主頁 -->
<div id="tab-home" class="screen active">
  <div class="ring-area">
    <div class="ring-wrap">
      <svg class="ring-svg" id="ring-svg" width="140" height="140" viewBox="0 0 140 140">
        <defs>
          <linearGradient id="rg" x1="0%" y1="0%" x2="100%" y2="0%">
            <stop id="rs1" offset="0%" stop-color="#00d4ff"/>
            <stop id="rs2" offset="100%" stop-color="#7c3aed"/>
          </linearGradient>
        </defs>
        <circle cx="70" cy="70" r="56" fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="12"/>
        <circle id="ring-arc" cx="70" cy="70" r="56" fill="none"
          stroke="url(#rg)" stroke-width="12" stroke-linecap="round"
          stroke-dasharray="351.9" stroke-dashoffset="351.9"
          transform="rotate(-90 70 70)" style="transition:stroke-dashoffset .6s ease"/>
      </svg>
      <div class="ring-center">
        <div class="ring-pct" id="ring-pct">0%</div>
        <div class="ring-sub">完成率</div>
      </div>
    </div>
    <div class="score-panel">
      <div>
        <div class="score-big" id="score-today">0</div>
        <div class="score-label">今日得分</div>
        <div class="score-max" id="score-max">/ 0 分</div>
      </div>
      <div class="streak-badge" id="streak-badge">🔥 連勝 0 天</div>
    </div>
  </div>

  <div class="section-label">今日習慣</div>
  <div id="habit-list"></div>
  <button class="add-btn" onclick="openAddModal()">＋ 新增習慣</button>
</div>

<!-- 統計 -->
<div id="tab-stats" class="screen">
  <div class="section-label">本週概覽</div>
  <div class="stats-grid" id="stats-grid"></div>
  <div class="section-label">每日得分（近 7 天）</div>
  <div class="week-row" id="week-chart"></div>
  <div class="section-label" style="margin-top:16px">習慣完成率</div>
  <div id="habit-stats"></div>
</div>

<!-- 設定 -->
<div id="tab-settings" class="screen">
  <div class="section-label">個人資料</div>
  <div style="background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:14px;margin-bottom:16px;">
    <label style="font-size:12px;color:var(--muted);display:block;margin-bottom:4px;">名稱</label>
    <input id="setting-name" type="text" placeholder="你的名稱"
      style="width:100%;padding:9px 12px;background:var(--surface2);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:var(--body);font-size:14px;margin-bottom:10px;">
    <label style="font-size:12px;color:var(--muted);display:block;margin-bottom:4px;">系所</label>
    <input id="setting-dept" type="text" placeholder="例：Electrical Engineering"
      style="width:100%;padding:9px 12px;background:var(--surface2);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:var(--body);font-size:14px;margin-bottom:10px;">
    <button onclick="saveProfile()"
      style="width:100%;padding:10px;background:linear-gradient(135deg,var(--purple),#4f46e5);border:none;border-radius:8px;color:#fff;font-family:var(--body);font-size:15px;font-weight:700;cursor:pointer;">
      儲存
    </button>
  </div>
  <div class="section-label">危險區域</div>
  <button onclick="resetAll()"
    style="width:100%;padding:10px;background:none;border:1px solid rgba(239,68,68,.3);border-radius:8px;color:#fca5a5;font-family:var(--body);font-size:14px;cursor:pointer;">
    重置所有資料
  </button>
</div>

<!-- 新增/編輯習慣 Modal -->
<div class="modal-bg" id="modal">
  <div class="modal">
    <h3 id="modal-title">新增習慣</h3>
    <div class="preset-grid" id="preset-grid"></div>
    <label>習慣名稱</label>
    <input type="text" id="habit-name-input" placeholder="例：冥想 10 分鐘">
    <label>Emoji</label>
    <input type="text" id="habit-emoji-input" placeholder="例：🧘" maxlength="2">
    <label>每日分數</label>
    <input type="number" id="habit-pts-input" min="1" max="50" value="5">
    <div class="modal-btns">
      <button class="btn-secondary" onclick="closeModal()">取消</button>
      <button class="btn-primary" onclick="saveHabit()">儲存</button>
    </div>
  </div>
</div>

<div id="toast"></div>

<script>
// ── 預設習慣模板 ───────────────────────────────────────────
const PRESETS = [
  { name:'睡滿 7 小時', emoji:'😴', pts:10 },
  { name:'規律運動',    emoji:'🏃', pts:8  },
  { name:'閱讀 20 頁',  emoji:'📖', pts:6  },
  { name:'規律飲食',    emoji:'🥗', pts:6  },
  { name:'背單字',      emoji:'📝', pts:4  },
  { name:'喝足水分',    emoji:'💧', pts:4  },
  { name:'冥想',        emoji:'🧘', pts:5  },
  { name:'寫日記',      emoji:'✍️', pts:3  },
];

// ── 資料結構 ───────────────────────────────────────────────
function loadData() {
  const raw = localStorage.getItem('solo_v2');
  if (raw) return JSON.parse(raw);
  return {
    profile: { name:'修煉者', dept:'Electrical Engineering' },
    habits: [],
    logs: {},      // { 'YYYY-MM-DD': { habitId: true/false } }
    streak: 0,
    longestStreak: 0,
    lastDate: null,
  };
}

function saveData(d) {
  localStorage.setItem('solo_v2', JSON.stringify(d));
}

let data = loadData();
let editingId = null;

function today() {
  return new Date().toISOString().split('T')[0];
}

function getTodayLog() {
  const t = today();
  if (!data.logs[t]) data.logs[t] = {};
  return data.logs[t];
}

// ── Streak 計算 ────────────────────────────────────────────
function updateStreak() {
  const t = today();
  const log = data.logs[t] || {};
  const allDone = data.habits.length > 0 &&
    data.habits.every(h => log[h.id]);

  if (allDone) {
    const yesterday = new Date();
    yesterday.setDate(yesterday.getDate() - 1);
    const yd = yesterday.toISOString().split('T')[0];

    if (data.lastDate === yd || data.lastDate === t) {
      if (data.lastDate !== t) {
        data.streak += 1;
        data.lastDate = t;
      }
    } else {
      data.streak = 1;
      data.lastDate = t;
    }
    data.longestStreak = Math.max(data.longestStreak, data.streak);
  }
  saveData(data);
}

// ── 渲染主頁 ───────────────────────────────────────────────
function renderHome() {
  const log = getTodayLog();
  const total = data.habits.length;
  const done  = data.habits.filter(h => log[h.id]).length;
  const pct   = total ? Math.round(done / total * 100) : 0;

  const todayPts = data.habits.reduce((s,h) => s + (log[h.id] ? h.pts : 0), 0);
  const maxPts   = data.habits.reduce((s,h) => s + h.pts, 0);

  // Ring
  const circ = 351.9;
  document.getElementById('ring-arc').style.strokeDashoffset = circ * (1 - pct/100);
  document.getElementById('ring-pct').textContent = pct + '%';
  document.getElementById('score-today').textContent = todayPts;
  document.getElementById('score-max').textContent = '/ ' + maxPts + ' 分';
  document.getElementById('streak-badge').textContent = '🔥 連勝 ' + data.streak + ' 天';

  // Ring colour
  const svg = document.querySelector('.ring-svg');
  const s1 = document.getElementById('rs1');
  const s2 = document.getElementById('rs2');
  if (pct < 34) {
    s1.setAttribute('stop-color','#ef4444'); s2.setAttribute('stop-color','#dc2626');
    svg.style.filter = 'drop-shadow(0 0 16px rgba(239,68,68,.6))';
  } else if (pct < 67) {
    s1.setAttribute('stop-color','#f59e0b'); s2.setAttribute('stop-color','#f97316');
    svg.style.filter = 'drop-shadow(0 0 16px rgba(245,158,11,.6))';
  } else {
    s1.setAttribute('stop-color','#00d4ff'); s2.setAttribute('stop-color','#7c3aed');
    svg.style.filter = 'drop-shadow(0 0 18px rgba(0,212,255,.5))';
  }

  // Header
  document.querySelector('#header h2').textContent = data.profile.name.toUpperCase();
  document.querySelector('#header p').textContent = data.profile.dept + ' · 單機版';
  document.getElementById('tier-pill').textContent = getTierLabel();

  // Habits
  const list = document.getElementById('habit-list');
  if (!data.habits.length) {
    list.innerHTML = `<div style="text-align:center;padding:30px;color:var(--muted);font-size:13px;">還沒有習慣，點下方新增！</div>`;
    return;
  }

  list.innerHTML = data.habits.map(h => `
    <div class="habit-row ${log[h.id] ? 'done' : ''}" id="row-${h.id}">
      <button class="check-btn" onclick="toggleHabit('${h.id}')">
        <svg width="12" height="12" viewBox="0 0 12 12" fill="none">
          <path d="M2 6l3 3 5-5" stroke="#fff" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
      </button>
      <span class="habit-name">${h.emoji} ${h.name}</span>
      <span class="habit-pts">+${h.pts}</span>
      <button class="edit-btn" onclick="openEditModal('${h.id}')">✏️</button>
      <button class="del-btn" onclick="deleteHabit('${h.id}')">✕</button>
    </div>
  `).join('');
}

// ── Toggle 習慣（可反選）──────────────────────────────────
function toggleHabit(id) {
  const log = getTodayLog();
  log[id] = !log[id];   // 已勾 → 取消，未勾 → 勾選
  data.logs[today()] = log;
  updateStreak();
  renderHome();

  const h = data.habits.find(x => x.id === id);
  if (log[id]) {
    toast(`✅ ${h.emoji} ${h.name} +${h.pts} 分`);
  } else {
    toast(`↩️ ${h.emoji} ${h.name} 取消`);
  }
}

// ── 段位 ──────────────────────────────────────────────────
function getTierLabel() {
  const s = data.streak;
  if (s >= 100) return '💎 鑽石';
  if (s >= 60)  return '💜 紫晶';
  if (s >= 30)  return '💚 翡翠';
  if (s >= 14)  return '🥇 黃金';
  if (s >= 7)   return '🥈 白銀';
  return '🥉 青銅';
}

// ── 統計頁 ────────────────────────────────────────────────
function renderStats() {
  const t = today();
  const log = getTodayLog();
  const todayPts = data.habits.reduce((s,h) => s + (log[h.id] ? h.pts : 0), 0);

  // 近 7 天
  const days = [];
  for (let i=6; i>=0; i--) {
    const d = new Date(); d.setDate(d.getDate()-i);
    const key = d.toISOString().split('T')[0];
    const dayLog = data.logs[key] || {};
    const pts = data.habits.reduce((s,h) => s + (dayLog[h.id] ? h.pts : 0), 0);
    const label = ['日','一','二','三','四','五','六'][d.getDay()];
    days.push({ key, pts, label });
  }

  const maxPts = Math.max(...days.map(d=>d.pts), 1);
  const totalPts7 = days.reduce((s,d)=>s+d.pts, 0);
  const activeDays = days.filter(d=>d.pts>0).length;

  // Stats grid
  document.getElementById('stats-grid').innerHTML = `
    <div class="stat-card">
      <div class="stat-val" style="color:var(--gold)">${todayPts}</div>
      <div class="stat-label">今日得分</div>
    </div>
    <div class="stat-card">
      <div class="stat-val" style="color:var(--accent)">${data.streak}</div>
      <div class="stat-label">🔥 連勝天數</div>
    </div>
    <div class="stat-card">
      <div class="stat-val">${totalPts7}</div>
      <div class="stat-label">本週總分</div>
    </div>
    <div class="stat-card">
      <div class="stat-val">${data.longestStreak}</div>
      <div class="stat-label">最長連勝</div>
    </div>
  `;

  // Weekly bar chart
  document.getElementById('week-chart').innerHTML = days.map(d => `
    <div class="day-col">
      <div class="day-pts">${d.pts}</div>
      <div class="day-bar-wrap">
        <div class="day-bar" style="height:${Math.round(d.pts/maxPts*100)}%"></div>
      </div>
      <div class="day-label">${d.label}</div>
    </div>
  `).join('');

  // Habit completion rates
  const all7Logs = days.map(d => data.logs[d.key] || {});
  document.getElementById('habit-stats').innerHTML = data.habits.map(h => {
    const cnt = all7Logs.filter(l => l[h.id]).length;
    const rate = Math.round(cnt/7*100);
    return `
      <div style="margin-bottom:10px;">
        <div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:4px;">
          <span>${h.emoji} ${h.name}</span>
          <span style="font-family:var(--mono);color:var(--gold)">${cnt}/7 天</span>
        </div>
        <div style="height:5px;background:var(--surface2);border-radius:3px;overflow:hidden;">
          <div style="height:100%;width:${rate}%;background:linear-gradient(90deg,var(--accent),var(--purple));border-radius:3px;transition:width .5s"></div>
        </div>
      </div>
    `;
  }).join('') || `<div style="color:var(--muted);font-size:13px;text-align:center;padding:20px;">新增習慣後才會顯示統計</div>`;
}

// ── 設定頁 ────────────────────────────────────────────────
function renderSettings() {
  document.getElementById('setting-name').value = data.profile.name;
  document.getElementById('setting-dept').value = data.profile.dept;
}

function saveProfile() {
  data.profile.name = document.getElementById('setting-name').value || '修煉者';
  data.profile.dept = document.getElementById('setting-dept').value || '';
  saveData(data);
  renderHome();
  toast('✅ 儲存成功');
}

function resetAll() {
  if (confirm('確定要重置所有資料？這無法復原。')) {
    localStorage.removeItem('solo_v2');
    data = loadData();
    renderHome();
    toast('🔄 已重置');
  }
}

// ── 新增/編輯 Modal ───────────────────────────────────────
function openAddModal() {
  editingId = null;
  document.getElementById('modal-title').textContent = '新增習慣';
  document.getElementById('habit-name-input').value = '';
  document.getElementById('habit-emoji-input').value = '';
  document.getElementById('habit-pts-input').value = '5';

  // Preset buttons
  document.getElementById('preset-grid').innerHTML = PRESETS.map(p => `
    <button class="preset-btn" onclick="fillPreset('${p.name}','${p.emoji}',${p.pts})">
      ${p.emoji} ${p.name}<br>
      <span class="preset-pts">+${p.pts} 分</span>
    </button>
  `).join('');

  document.getElementById('modal').classList.add('show');
}

function openEditModal(id) {
  const h = data.habits.find(x => x.id === id);
  if (!h) return;
  editingId = id;
  document.getElementById('modal-title').textContent = '編輯習慣';
  document.getElementById('habit-name-input').value = h.name;
  document.getElementById('habit-emoji-input').value = h.emoji;
  document.getElementById('habit-pts-input').value = h.pts;
  document.getElementById('preset-grid').innerHTML = '';
  document.getElementById('modal').classList.add('show');
}

function fillPreset(name, emoji, pts) {
  document.getElementById('habit-name-input').value = name;
  document.getElementById('habit-emoji-input').value = emoji;
  document.getElementById('habit-pts-input').value = pts;
}

function closeModal() {
  document.getElementById('modal').classList.remove('show');
  editingId = null;
}

function saveHabit() {
  const name  = document.getElementById('habit-name-input').value.trim();
  const emoji = document.getElementById('habit-emoji-input').value.trim() || '⭐';
  const pts   = parseInt(document.getElementById('habit-pts-input').value) || 5;

  if (!name) { toast('❌ 請輸入習慣名稱'); return; }

  if (editingId) {
    const h = data.habits.find(x => x.id === editingId);
    if (h) { h.name = name; h.emoji = emoji; h.pts = pts; }
    toast('✅ 已更新');
  } else {
    data.habits.push({
      id: 'h' + Date.now(),
      name, emoji, pts,
      createdAt: today(),
    });
    toast(`✅ 新增「${name}」`);
  }

  saveData(data);
  closeModal();
  renderHome();
}

function deleteHabit(id) {
  const h = data.habits.find(x => x.id === id);
  if (!confirm(`刪除「${h?.name}」？`)) return;
  data.habits = data.habits.filter(x => x.id !== id);
  saveData(data);
  renderHome();
  toast('🗑️ 已刪除');
}

// ── Tab 切換 ──────────────────────────────────────────────
window.switchTab = function(name, btn) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('tab-' + name).classList.add('active');
  btn.classList.add('active');
  if (name === 'stats')    renderStats();
  if (name === 'settings') renderSettings();
};

// ── Toast ─────────────────────────────────────────────────
function toast(msg) {
  const el = document.getElementById('toast');
  el.textContent = msg;
  el.classList.add('show');
  setTimeout(() => el.classList.remove('show'), 2200);
}

// 啟動
renderHome();
</script>
</body>
</html>
