# Skill-Up-Tracker
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>SkillUp — Skill Progress Tracker</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --bg-primary: #ffffff;
    --bg-secondary: #f5f5f3;
    --bg-tertiary: #eeede8;
    --text-primary: #1a1a19;
    --text-secondary: #6b6b68;
    --text-tertiary: #9b9b98;
    --border-t: rgba(0,0,0,0.12);
    --border-s: rgba(0,0,0,0.22);
    --border-p: rgba(0,0,0,0.32);
    --radius-md: 8px;
    --radius-lg: 12px;
    --purple: #534AB7;
    --purple-dark: #3C3489;
    --purple-bg: #EEEDFE;
    --purple-text: #3C3489;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg-primary: #1c1c1a;
      --bg-secondary: #252523;
      --bg-tertiary: #2e2e2b;
      --text-primary: #f0efe8;
      --text-secondary: #a0a09a;
      --text-tertiary: #6a6a65;
      --border-t: rgba(255,255,255,0.10);
      --border-s: rgba(255,255,255,0.18);
      --border-p: rgba(255,255,255,0.28);
      --purple-bg: #2a2560;
      --purple-text: #AFA9EC;
    }
  }
  body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: var(--bg-tertiary); color: var(--text-primary); min-height: 100vh; }
  input, select, button, textarea { font-family: inherit; }
  .hidden { display: none !important; }

  /* ── Layout ── */
  .container { max-width: 720px; margin: 0 auto; padding: 2rem 1rem; }

  /* ── Auth ── */
  .auth-wrap { max-width: 380px; margin: 4rem auto; background: var(--bg-primary); border: 0.5px solid var(--border-t); border-radius: var(--radius-lg); padding: 2rem; }
  .auth-logo { text-align: center; margin-bottom: 1.5rem; }
  .auth-logo .logo-mark { width: 48px; height: 48px; background: var(--purple-bg); border-radius: 12px; display: inline-flex; align-items: center; justify-content: center; font-size: 24px; margin-bottom: 10px; }
  .auth-logo h1 { font-size: 22px; font-weight: 500; color: var(--text-primary); }
  .auth-logo p { font-size: 14px; color: var(--text-secondary); margin-top: 4px; }
  .auth-tabs { display: flex; border: 0.5px solid var(--border-s); border-radius: var(--radius-md); overflow: hidden; margin-bottom: 1.5rem; }
  .auth-tab { flex: 1; padding: 9px; font-size: 14px; border: none; background: transparent; cursor: pointer; color: var(--text-secondary); transition: all .15s; }
  .auth-tab.active { background: var(--bg-secondary); color: var(--text-primary); font-weight: 500; }
  .auth-field { margin-bottom: 14px; }
  .auth-field label { font-size: 12px; color: var(--text-secondary); display: block; margin-bottom: 5px; }
  .auth-field input { width: 100%; padding: 9px 12px; border: 0.5px solid var(--border-s); border-radius: var(--radius-md); font-size: 14px; background: var(--bg-primary); color: var(--text-primary); outline: none; transition: border-color .15s; }
  .auth-field input:focus { border-color: var(--purple); }
  .auth-btn { width: 100%; padding: 10px; background: var(--purple); color: #fff; border: none; border-radius: var(--radius-md); font-size: 14px; font-weight: 500; cursor: pointer; margin-top: 4px; transition: background .15s; }
  .auth-btn:hover { background: var(--purple-dark); }
  .auth-err { font-size: 12px; color: #d9534f; margin-top: 10px; text-align: center; min-height: 16px; }
  .auth-demo { font-size: 12px; color: var(--text-tertiary); text-align: center; margin-top: 14px; background: var(--bg-secondary); border-radius: var(--radius-md); padding: 10px; }
  .auth-demo strong { color: var(--text-secondary); }

  /* ── App Header ── */
  .app-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.5rem; flex-wrap: wrap; gap: 10px; background: var(--bg-primary); border: 0.5px solid var(--border-t); border-radius: var(--radius-lg); padding: 12px 16px; }
  .header-left { display: flex; align-items: center; gap: 12px; }
  .avatar { width: 38px; height: 38px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 13px; font-weight: 500; flex-shrink: 0; }
  .header-info p { font-size: 14px; font-weight: 500; color: var(--text-primary); }
  .header-info span { font-size: 12px; color: var(--text-secondary); }
  .header-right { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }
  .nav-btn { padding: 7px 16px; font-size: 13px; border: 0.5px solid var(--border-s); border-radius: var(--radius-md); background: transparent; color: var(--text-secondary); cursor: pointer; transition: all .15s; }
  .nav-btn.active, .nav-btn:hover { background: var(--bg-secondary); color: var(--text-primary); }
  .logout-btn { padding: 7px 12px; font-size: 12px; border: 0.5px solid var(--border-t); border-radius: var(--radius-md); background: transparent; color: var(--text-tertiary); cursor: pointer; transition: all .15s; }
  .logout-btn:hover { color: #d9534f; border-color: #d9534f; }

  /* ── Metric Cards ── */
  .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 10px; margin-bottom: 1.5rem; }
  .mc { background: var(--bg-secondary); border-radius: var(--radius-md); padding: 14px 16px; }
  .mc .ml { font-size: 11px; color: var(--text-secondary); margin-bottom: 4px; }
  .mc .mv { font-size: 22px; font-weight: 500; color: var(--text-primary); }
  .mc .ms { font-size: 11px; color: var(--text-tertiary); margin-top: 2px; }

  /* ── Section Title ── */
  .sec-title { font-size: 11px; font-weight: 500; color: var(--text-secondary); letter-spacing: .06em; text-transform: uppercase; margin-bottom: 10px; }

  /* ── Skill Cards ── */
  .skill-list { display: flex; flex-direction: column; gap: 8px; margin-bottom: 1.25rem; }
  .sk-card { background: var(--bg-primary); border: 0.5px solid var(--border-t); border-radius: var(--radius-lg); padding: 14px 16px; cursor: pointer; transition: border-color .15s; }
  .sk-card:hover { border-color: var(--border-s); }
  .sk-card.exp { border-color: var(--border-p); }
  .sk-hdr { display: flex; align-items: center; gap: 12px; }
  .sk-ico { width: 36px; height: 36px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 16px; flex-shrink: 0; }
  .sk-meta { flex: 1; min-width: 0; }
  .sk-nr { display: flex; align-items: center; justify-content: space-between; margin-bottom: 6px; }
  .sk-name { font-size: 14px; font-weight: 500; color: var(--text-primary); }
  .sk-lv { font-size: 12px; color: var(--text-secondary); }
  .prog-track { height: 5px; background: var(--border-t); border-radius: 3px; overflow: hidden; }
  .prog-fill { height: 100%; border-radius: 3px; transition: width .5s ease; }
  .sk-bot { display: flex; align-items: center; justify-content: space-between; margin-top: 6px; }
  .sk-xp { font-size: 11px; color: var(--text-secondary); }
  .streak-pill { font-size: 11px; padding: 2px 9px; border-radius: 10px; font-weight: 500; }
  .chev { font-size: 11px; color: var(--text-tertiary); flex-shrink: 0; transition: transform .2s; margin-left: 6px; }
  .exp .chev { transform: rotate(180deg); }
  .sk-detail { display: none; margin-top: 14px; padding-top: 14px; border-top: 0.5px solid var(--border-t); }
  .exp .sk-detail { display: block; }
  .d-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 12px; }
  .d-stat { background: var(--bg-secondary); border-radius: var(--radius-md); padding: 10px 12px; }
  .d-stat .dl { font-size: 11px; color: var(--text-secondary); margin-bottom: 3px; }
  .d-stat .dv { font-size: 16px; font-weight: 500; color: var(--text-primary); }
  .ms-row { display: flex; align-items: center; gap: 10px; padding: 6px 0; border-bottom: 0.5px solid var(--border-t); }
  .ms-row:last-child { border-bottom: none; }
  .ms-dot { width: 9px; height: 9px; border-radius: 50%; flex-shrink: 0; }
  .ms-lbl { font-size: 13px; color: var(--text-primary); flex: 1; }
  .ms-done .ms-lbl { color: var(--text-secondary); text-decoration: line-through; }
  .ms-st { font-size: 11px; color: var(--text-secondary); }
  .log-btn { width: 100%; margin-top: 12px; padding: 9px; font-size: 13px; border-radius: var(--radius-md); border: 0.5px solid var(--border-s); background: transparent; color: var(--text-primary); cursor: pointer; transition: background .15s; }
  .log-btn:hover:not(:disabled) { background: var(--bg-secondary); }
  .log-btn:disabled { opacity: .5; cursor: not-allowed; }
  .log-btn.done-today { background: #E1F5EE; color: #085041; border-color: #9FE1CB; font-size: 12px; }

  /* ── Add Skill Button ── */
  .add-skill-btn { width: 100%; padding: 10px; font-size: 13px; border-radius: var(--radius-lg); border: 0.5px dashed var(--border-s); background: transparent; color: var(--text-secondary); cursor: pointer; transition: all .15s; margin-bottom: 1.5rem; }
  .add-skill-btn:hover { border-color: var(--border-p); color: var(--text-primary); background: var(--bg-secondary); }

  /* ── Activity Heatmap ── */
  .act-section { margin-top: .5rem; }
  .act-grid { display: grid; grid-template-columns: repeat(26, 1fr); gap: 3px; margin-top: 8px; }
  .act-cell { height: 12px; border-radius: 2px; }
  .leg-row { display: flex; align-items: center; gap: 5px; margin-top: 6px; justify-content: flex-end; }
  .leg-row span { font-size: 11px; color: var(--text-secondary); }
  .leg-cell { width: 10px; height: 10px; border-radius: 2px; }

  /* ── Leaderboard ── */
  .lb-card { background: var(--bg-primary); border: 0.5px solid var(--border-t); border-radius: var(--radius-lg); overflow: hidden; margin-bottom: 1.5rem; }
  .lb-row { display: flex; align-items: center; gap: 12px; padding: 12px 16px; border-bottom: 0.5px solid var(--border-t); transition: background .15s; }
  .lb-row:last-child { border-bottom: none; }
  .lb-row:hover { background: var(--bg-secondary); }
  .lb-row.me { background: var(--purple-bg); }
  .lb-rank { font-size: 15px; font-weight: 500; color: var(--text-secondary); width: 28px; text-align: center; flex-shrink: 0; }
  .lb-av { width: 34px; height: 34px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: 500; flex-shrink: 0; }
  .lb-info { flex: 1; min-width: 0; }
  .lb-info p { font-size: 14px; font-weight: 500; color: var(--text-primary); display: flex; align-items: center; gap: 6px; flex-wrap: wrap; }
  .lb-info span { font-size: 12px; color: var(--text-secondary); }
  .lb-xp { font-size: 14px; font-weight: 500; color: var(--text-primary); }
  .lb-badge { font-size: 10px; padding: 2px 8px; border-radius: 8px; background: #E1F5EE; color: #085041; }
  .you-badge { font-size: 10px; padding: 2px 8px; border-radius: 8px; background: var(--purple-bg); color: var(--purple-text); }

  /* ── Modal ── */
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.4); z-index: 200; align-items: center; justify-content: center; }
  .modal-overlay.open { display: flex; }
  .modal { background: var(--bg-primary); border-radius: var(--radius-lg); border: 0.5px solid var(--border-s); padding: 1.5rem; width: min(380px, 92vw); }
  .modal h3 { font-size: 16px; font-weight: 500; margin-bottom: 1.25rem; color: var(--text-primary); }
  .modal label { font-size: 12px; color: var(--text-secondary); display: block; margin-bottom: 5px; margin-top: 12px; }
  .modal input, .modal select { width: 100%; padding: 9px 12px; border: 0.5px solid var(--border-s); border-radius: var(--radius-md); font-size: 14px; background: var(--bg-primary); color: var(--text-primary); outline: none; transition: border-color .15s; }
  .modal input:focus, .modal select:focus { border-color: var(--purple); }
  .m-btns { display: flex; gap: 8px; margin-top: 1.25rem; }
  .m-btns button { flex: 1; padding: 9px; border-radius: var(--radius-md); font-size: 14px; cursor: pointer; transition: all .15s; }
  .m-cancel { border: 0.5px solid var(--border-s); background: transparent; color: var(--text-secondary); }
  .m-cancel:hover { background: var(--bg-secondary); }
  .m-confirm { border: none; background: var(--purple); color: #fff; font-weight: 500; }
  .m-confirm:hover { background: var(--purple-dark); }

  /* ── Toast ── */
  .toast { position: fixed; bottom: 1.5rem; right: 1.5rem; background: var(--purple); color: #fff; padding: 10px 16px; border-radius: var(--radius-md); font-size: 14px; opacity: 0; transition: opacity .3s; pointer-events: none; z-index: 300; display: flex; align-items: center; gap: 10px; }
  .toast.show { opacity: 1; pointer-events: auto; }
  .toast-undo { background: rgba(255,255,255,0.2); border: 1px solid rgba(255,255,255,0.4); color: #fff; font-size: 12px; font-weight: 600; padding: 3px 10px; border-radius: 6px; cursor: pointer; white-space: nowrap; transition: background .15s; }
  .toast-undo:hover { background: rgba(255,255,255,0.35); }

  /* ── Delete skill ── */
  .del-btn { font-size: 11px; color: var(--text-tertiary); border: none; background: none; cursor: pointer; padding: 4px 8px; border-radius: 6px; margin-top: 10px; float: right; }
  .del-btn:hover { color: #d9534f; background: #FCEBEB; }
</style>
</head>
<body>
<div id="auth-screen">
  <div class="auth-wrap">
    <div class="auth-logo">
      <div class="logo-mark">⚡</div>
      <h1>SkillUp</h1>
      <p>Track skills. Compete. Level up.</p>
    </div>
    <div class="auth-tabs">
      <button class="auth-tab active" onclick="switchTab('login')">Sign in</button>
      <button class="auth-tab" onclick="switchTab('register')">Register</button>
    </div>

    <div id="login-form">
      <div class="auth-field"><label>Username</label><input type="text" id="li-user" placeholder="your username" /></div>
      <div class="auth-field"><label>Password</label><input type="password" id="li-pass" placeholder="••••••" /></div>
      <button class="auth-btn" onclick="doLogin()">Sign in</button>
      <div class="auth-err" id="li-err"></div>
    </div>

    <div id="reg-form" class="hidden">
      <div class="auth-field"><label>Username</label><input type="text" id="rg-user" placeholder="choose a username" /></div>
      <div class="auth-field"><label>Display name</label><input type="text" id="rg-name" placeholder="your full name" /></div>
      <div class="auth-field"><label>Password</label><input type="password" id="rg-pass" placeholder="min 4 characters" /></div>
      <button class="auth-btn" onclick="doRegister()">Create account</button>
      <div class="auth-err" id="rg-err"></div>
    </div>

    <div class="auth-demo">
      <strong>Demo accounts</strong> — username: <strong>arjun</strong>, <strong>priya</strong>, or <strong>rahul</strong> · password: <strong>demo</strong>
    </div>
  </div>
</div>

<!-- ══ APP SCREEN ══ -->
<div id="app-screen" class="hidden">
  <div class="container">

    <!-- Header -->
    <div class="app-header">
      <div class="header-left">
        <div class="avatar" id="hdr-avatar"></div>
        <div class="header-info">
          <p id="hdr-name"></p>
          <span id="hdr-level"></span>
        </div>
      </div>
      <div class="header-right">
        <button class="nav-btn active" id="tab-my" onclick="switchAppTab('my')">My skills</button>
        <button class="nav-btn" id="tab-lb" onclick="switchAppTab('lb')">Leaderboard</button>
        <button class="logout-btn" onclick="doLogout()">Sign out</button>
      </div>
    </div>

    <!-- MY SKILLS VIEW -->
    <div id="view-my">
      <div class="summary-grid">
        <div class="mc"><div class="ml">Total XP</div><div class="mv" id="s-xp">0</div><div class="ms">all skills</div></div>
        <div class="mc"><div class="ml">Overall level</div><div class="mv" id="s-lvl">1</div></div>
        <div class="mc"><div class="ml">Skills tracked</div><div class="mv" id="s-skills">0</div></div>
        <div class="mc"><div class="ml">Milestones hit</div><div class="mv" id="s-ms">0</div></div>
      </div>

      <div class="sec-title">Your skills</div>
      <div class="skill-list" id="skill-list"></div>
      <button class="add-skill-btn" onclick="openAddModal()">+ Add new skill</button>

      <div class="act-section">
        <div class="sec-title">Practice activity — last 6 months</div>
        <div class="act-grid" id="act-grid"></div>
        <div class="leg-row">
          <span>Less</span>
          <div class="leg-cell" style="background:#e0dff8"></div>
          <div class="leg-cell" style="background:#AFA9EC"></div>
          <div class="leg-cell" style="background:#7F77DD"></div>
          <div class="leg-cell" style="background:#534AB7"></div>
          <div class="leg-cell" style="background:#3C3489"></div>
          <span>More</span>
        </div>
      </div>
    </div>

    <!-- LEADERBOARD VIEW -->
    <div id="view-lb" class="hidden">
      <div class="summary-grid">
        <div class="mc"><div class="ml">Users registered</div><div class="mv" id="lb-users">0</div></div>
        <div class="mc"><div class="ml">Your rank</div><div class="mv" id="lb-rank">#—</div></div>
        <div class="mc"><div class="ml">Top XP today</div><div class="mv" id="lb-topxp">0</div></div>
      </div>
      <div class="sec-title">All-time leaderboard</div>
      <div class="lb-card" id="lb-list"></div>
    </div>

  </div>
</div>

<!-- ══ ADD SKILL MODAL ══ -->
<div class="modal-overlay" id="add-modal" onclick="handleModalBg(event)">
  <div class="modal">
    <h3>Add a new skill</h3>
    <label>Skill name</label>
    <input type="text" id="new-name" placeholder="e.g. Python, Piano, Chess" />
    <label>Category</label>
    <select id="new-cat">
      <option value="code">Coding</option>
      <option value="music">Music</option>
      <option value="language">Language</option>
      <option value="sport">Sport / Fitness</option>
      <option value="art">Art / Design</option>
      <option value="other">Other</option>
    </select>
    <label>Starting XP (0–500)</label>
    <input type="number" id="new-xp" min="0" max="500" value="0" />
    <div class="m-btns">
      <button class="m-cancel" onclick="closeAddModal()">Cancel</button>
      <button class="m-confirm" onclick="confirmAddSkill()">Add skill</button>
    </div>
  </div>
</div>

<!-- ══ DELETE CONFIRM MODAL ══ -->
<div class="modal-overlay" id="del-modal" onclick="handleDelModalBg(event)">
  <div class="modal">
    <h3>Remove skill</h3>
    <p style="font-size:14px;color:var(--text-secondary);margin-bottom:12px">Are you sure you want to remove <strong id="del-skill-name"></strong>? All XP and progress will be lost.</p>
    <label style="font-size:12px;color:var(--text-secondary);display:block;margin-bottom:5px">Enter your password to confirm</label>
    <input type="password" id="del-pass" placeholder="••••••" style="width:100%;padding:9px 12px;border:0.5px solid var(--border-s);border-radius:var(--radius-md);font-size:14px;background:var(--bg-primary);color:var(--text-primary);outline:none;transition:border-color .15s;" onfocus="this.style.borderColor='var(--purple)'" onblur="this.style.borderColor='var(--border-s)'" />
    <div id="del-pass-err" style="font-size:12px;color:#d9534f;min-height:16px;margin-top:6px;"></div>
    <div class="m-btns" style="margin-top:1rem">
      <button class="m-cancel" onclick="closeDelModal()">Cancel</button>
      <button class="m-confirm" style="background:#d9534f" onclick="confirmDelete()">Remove</button>
    </div>
  </div>
</div>

<!-- ══ TOAST ══ -->
<div class="toast" id="toast"><span id="toast-msg"></span></div>

<script>
const CAT = {
  code:     { bg: '#E6F1FB', fill: '#378ADD', icon: '💻' },
  music:    { bg: '#EEEDFE', fill: '#7F77DD', icon: '🎵' },
  language: { bg: '#E1F5EE', fill: '#1D9E75', icon: '🌐' },
  sport:    { bg: '#FAEEDA', fill: '#BA7517', icon: '🏃' },
  art:      { bg: '#FBEAF0', fill: '#D4537E', icon: '🎨' },
  other:    { bg: '#F1EFE8', fill: '#888780', icon: '⚡' }
};

const AV_COLORS = [
  { bg: '#EEEDFE', cl: '#3C3489' },
  { bg: '#E1F5EE', cl: '#085041' },
  { bg: '#FAEEDA', cl: '#633806' },
  { bg: '#E6F1FB', cl: '#0C447C' },
  { bg: '#FBEAF0', cl: '#72243E' },
  { bg: '#EAF3DE', cl: '#27500A' }
];
const xpToLvl = xp => Math.floor(1 + Math.sqrt(xp / 50));
const lvlXP   = l  => Math.pow(l - 1, 2) * 50;
const todayKey = () => new Date().toISOString().slice(0, 10);
const initials = name => name.split(' ').map(w => w[0]).join('').toUpperCase().slice(0, 2);
const avColor  = username => AV_COLORS[username.charCodeAt(0) % AV_COLORS.length];

function lvlProg(xp) {
  const l = xpToLvl(xp);
  const cur = lvlXP(l), next = lvlXP(l + 1);
  return { l, pct: Math.round((xp - cur) / (next - cur) * 100), cur: xp - cur, need: next - cur };
}

function genActivity(sessions) {
  const a = [];
  for (let i = 0; i < 182; i++)
    a.push(Math.random() < sessions / 182 ? Math.ceil(Math.random() * 3) : 0);
  return a;
}

function streakStyle(n) {
  if (n >= 14) return { bg: '#EEEDFE', cl: '#3C3489' };
  if (n >= 7)  return { bg: '#E1F5EE', cl: '#085041' };
  return { bg: '#F1EFE8', cl: '#444441' };
}

function skillMilestones(sk) {
  return [
    { label: 'First 50 XP',       done: sk.xp >= 50 },
    { label: 'Reach level 2',     done: xpToLvl(sk.xp) >= 2 },
    { label: '7-day streak',      done: sk.streak >= 7 },
    { label: 'Level 5 achieved',  done: xpToLvl(sk.xp) >= 5 },
    { label: '500 XP milestone',  done: sk.xp >= 500 },
    { label: 'Level 10 master',   done: xpToLvl(sk.xp) >= 10 },
  ];
}

function userTotalXP(u) {
  return u.skills.reduce((s, sk) => s + sk.xp, 0);
}
let DB = { users: {} };

function saveDB() {
  try { localStorage.setItem('skillup_db', JSON.stringify(DB)); } catch(e) {}
}
function loadDB() {
  try {
    const raw = localStorage.getItem('skillup_db');
    if (raw) DB = JSON.parse(raw);
  } catch(e) {}
}
function seedDemo() {
  if (Object.keys(DB.users).length > 0) return;
  const demos = [
    { u: 'arjun', n: 'Arjun Sharma', p: 'demo',
      skills: [
        { id: 1, name: 'Python',  cat: 'code',     xp: 520, streak: 14, activity: genActivity(52), loggedToday: null },
        { id: 2, name: 'Spanish', cat: 'language',  xp: 310, streak: 8,  activity: genActivity(31), loggedToday: null }
      ]},
    { u: 'priya', n: 'Priya Patel', p: 'demo',
      skills: [
        { id: 1, name: 'Guitar', cat: 'music', xp: 680, streak: 21, activity: genActivity(68), loggedToday: null },
        { id: 2, name: 'Yoga',   cat: 'sport', xp: 240, streak: 6,  activity: genActivity(24), loggedToday: null }
      ]},
    { u: 'rahul', n: 'Rahul Desai', p: 'demo',
      skills: [
        { id: 1, name: 'Chess', cat: 'other', xp: 195, streak: 3, activity: genActivity(19), loggedToday: null }
      ]},
  ];
  demos.forEach(d => {
    DB.users[d.u] = { username: d.u, displayName: d.n, password: d.p, nextId: 10, skills: d.skills };
  });
  saveDB();
}
loadDB();
seedDemo();
let currentUser = null;
let expandedId  = null;
let deleteTarget = null;
let appTab = 'my';
let undoData = null;       // { skill, index } for undo
let undoTimer = null;      // timeout id
let toastTimer = null;     // toast hide timer
function switchTab(t) {
  document.getElementById('login-form').classList.toggle('hidden', t !== 'login');
  document.getElementById('reg-form').classList.toggle('hidden',  t !== 'register');
  document.querySelectorAll('.auth-tab').forEach((b, i) =>
    b.classList.toggle('active', (i === 0 && t === 'login') || (i === 1 && t === 'register')));
}

function doLogin() {
  const u   = document.getElementById('li-user').value.trim().toLowerCase();
  const p   = document.getElementById('li-pass').value;
  const err = document.getElementById('li-err');
  if (!DB.users[u] || DB.users[u].password !== p) { err.textContent = 'Invalid username or password.'; return; }
  err.textContent = '';
  currentUser = u;
  showApp();
}

function doRegister() {
  const u   = document.getElementById('rg-user').value.trim().toLowerCase();
  const n   = document.getElementById('rg-name').value.trim();
  const p   = document.getElementById('rg-pass').value;
  const err = document.getElementById('rg-err');
  if (!u || !n || !p)  { err.textContent = 'All fields are required.'; return; }
  if (p.length < 4)    { err.textContent = 'Password must be at least 4 characters.'; return; }
  if (DB.users[u])     { err.textContent = 'Username already taken.'; return; }
  if (!/^[a-z0-9_]+$/.test(u)) { err.textContent = 'Username: letters, numbers, underscores only.'; return; }
  DB.users[u] = { username: u, displayName: n, password: p, nextId: 1, skills: [] };
  saveDB();
  err.textContent = '';
  currentUser = u;
  showApp();
}

function doLogout() {
  currentUser = null;
  expandedId  = null;
  document.getElementById('auth-screen').classList.remove('hidden');
  document.getElementById('app-screen').classList.add('hidden');
  document.getElementById('li-user').value = '';
  document.getElementById('li-pass').value = '';
  document.getElementById('li-err').textContent = '';
  switchTab('login');
}
function showApp() {
  document.getElementById('auth-screen').classList.add('hidden');
  document.getElementById('app-screen').classList.remove('hidden');
  const u  = DB.users[currentUser];
  const ac = avColor(currentUser);
  const av = document.getElementById('hdr-avatar');
  av.textContent  = initials(u.displayName);
  av.style.background = ac.bg;
  av.style.color      = ac.cl;
  document.getElementById('hdr-name').textContent = u.displayName;
  switchAppTab('my');
}

function switchAppTab(t) {
  appTab = t;
  document.getElementById('view-my').classList.toggle('hidden', t !== 'my');
  document.getElementById('view-lb').classList.toggle('hidden', t !== 'lb');
  document.getElementById('tab-my').classList.toggle('active', t === 'my');
  document.getElementById('tab-lb').classList.toggle('active', t === 'lb');
  if (t === 'lb') renderLB();
  else renderMy();
}
function renderMy() {
  const u = DB.users[currentUser];
  const totalXP = userTotalXP(u);
  const overallLvl = Math.max(1, xpToLvl(Math.round(totalXP / (u.skills.length || 1))));
  const msDone = u.skills.reduce((s, sk) => s + skillMilestones(sk).filter(m => m.done).length, 0);

  document.getElementById('hdr-level').textContent = 'Level ' + overallLvl + ' · ' + totalXP.toLocaleString() + ' XP';
  document.getElementById('s-xp').textContent    = totalXP.toLocaleString();
  document.getElementById('s-lvl').textContent   = overallLvl;
  document.getElementById('s-skills').textContent = u.skills.length;
  document.getElementById('s-ms').textContent    = msDone;

  const list = document.getElementById('skill-list');
  list.innerHTML = '';

  if (u.skills.length === 0) {
    list.innerHTML = '<p style="font-size:14px;color:var(--text-secondary);text-align:center;padding:2rem 0">No skills yet — add your first one below!</p>';
  }

  u.skills.forEach(sk => {
    const p   = lvlProg(sk.xp);
    const c   = CAT[sk.cat];
    const sc  = streakStyle(sk.streak);
    const ms  = skillMilestones(sk);
    const isExp = expandedId === sk.id;
    const loggedToday = sk.loggedToday === todayKey();

    const card = document.createElement('div');
    card.className = 'sk-card' + (isExp ? ' exp' : '');
    card.innerHTML = `
      <div class="sk-hdr" onclick="toggleExpand(${sk.id})">
        <div class="sk-ico" style="background:${c.bg}">${c.icon}</div>
        <div class="sk-meta">
          <div class="sk-nr">
            <span class="sk-name">${escHtml(sk.name)}</span>
            <span class="sk-lv">Lv ${p.l}</span>
          </div>
          <div class="prog-track">
            <div class="prog-fill" style="width:${p.pct}%;background:${c.fill}"></div>
          </div>
          <div class="sk-bot">
            <span class="sk-xp">${sk.xp} XP &nbsp;·&nbsp; ${p.cur}/${p.need} to next level</span>
            <span class="streak-pill" style="background:${sc.bg};color:${sc.cl}">${sk.streak}d streak</span>
          </div>
        </div>
        <span class="chev">▾</span>
      </div>
      <div class="sk-detail">
        <div class="d-grid">
          <div class="d-stat"><div class="dl">Level</div><div class="dv">${p.l}</div></div>
          <div class="d-stat"><div class="dl">Progress</div><div class="dv">${p.pct}%</div></div>
          <div class="d-stat"><div class="dl">Current streak</div><div class="dv">${sk.streak}d</div></div>
          <div class="d-stat"><div class="dl">Total XP</div><div class="dv">${sk.xp}</div></div>
        </div>
        <div class="sec-title" style="margin-bottom:8px">Milestones</div>
        ${ms.map(m => `
          <div class="ms-row${m.done ? ' ms-done' : ''}">
            <div class="ms-dot" style="background:${m.done ? c.fill : 'var(--border-s)'}"></div>
            <span class="ms-lbl">${m.label}</span>
            <span class="ms-st">${m.done ? 'Done' : 'Pending'}</span>
          </div>`).join('')}
        <button
          class="log-btn${loggedToday ? ' done-today' : ''}"
          onclick="logSession(${sk.id})"
          ${loggedToday ? 'disabled' : ''}>
          ${loggedToday
            ? '✓ Practice logged today — come back tomorrow'
            : '+10 XP — Log practice session'}
        </button>
        <button class="del-btn" onclick="openDelModal(${sk.id}, '${escHtml(sk.name)}')">Remove skill</button>
      </div>`;
    list.appendChild(card);
  });

  renderActivity(u);
}

function toggleExpand(id) {
  expandedId = expandedId === id ? null : id;
  renderMy();
}

function logSession(id) {
  const u  = DB.users[currentUser];
  const sk = u.skills.find(s => s.id === id);
  if (!sk) return;
  if (sk.loggedToday === todayKey()) { showToast('Already logged today! Come back tomorrow.'); return; }
  sk.xp += 10;
  sk.streak += 1;
  sk.loggedToday = todayKey();
  sk.activity[sk.activity.length - 1] = Math.min(4, (sk.activity[sk.activity.length - 1] || 0) + 1);
  saveDB();
  showToast('+10 XP logged for ' + sk.name + '!');
  renderMy();
}

function renderActivity(u) {
  const grid   = document.getElementById('act-grid');
  grid.innerHTML = '';
  const merged = new Array(182).fill(0);
  u.skills.forEach(sk => { sk.activity.forEach((v, i) => { merged[i] += v; }); });
  const mx  = Math.max(...merged, 1);
  const pal = ['#F1EFE8', '#EEEDFE', '#AFA9EC', '#7F77DD', '#534AB7', '#3C3489'];
  merged.forEach(v => {
    const idx  = v === 0 ? 0 : Math.min(5, Math.ceil(v / mx * 5));
    const cell = document.createElement('div');
    cell.className = 'act-cell';
    cell.style.background = pal[idx];
    cell.title = v + ' session' + (v !== 1 ? 's' : '');
    grid.appendChild(cell);
  });
}
function renderLB() {
  const allUsers = Object.values(DB.users).map(u => ({
    username:    u.username,
    displayName: u.displayName,
    totalXP:     userTotalXP(u),
    skills:      u.skills.length,
    bestStreak:  u.skills.reduce((m, sk) => Math.max(m, sk.streak), 0),
    todayXP:     u.skills.filter(sk => sk.loggedToday === todayKey()).length * 10
  })).sort((a, b) => b.totalXP - a.totalXP);

  const myRank = allUsers.findIndex(u => u.username === currentUser) + 1;
  const topToday = Math.max(...allUsers.map(u => u.todayXP), 0);

  document.getElementById('lb-users').textContent  = allUsers.length;
  document.getElementById('lb-rank').textContent   = '#' + myRank;
  document.getElementById('lb-topxp').textContent  = topToday;

  const lbList = document.getElementById('lb-list');
  lbList.innerHTML = '';

  allUsers.forEach((u, i) => {
    const rank  = i + 1;
    const ac    = avColor(u.username);
    const isMe  = u.username === currentUser;
    const medal = rank === 1 ? '🥇' : rank === 2 ? '🥈' : rank === 3 ? '🥉' : rank;
    const row   = document.createElement('div');
    row.className = 'lb-row' + (isMe ? ' me' : '');
    row.innerHTML = `
      <span class="lb-rank">${medal}</span>
      <div class="lb-av" style="background:${ac.bg};color:${ac.cl}">${initials(u.displayName)}</div>
      <div class="lb-info">
        <p>${escHtml(u.displayName)}${isMe ? '<span class="you-badge">You</span>' : ''}</p>
        <span>${u.skills} skill${u.skills !== 1 ? 's' : ''} · best streak ${u.bestStreak}d</span>
      </div>
      <div style="text-align:right">
        <div class="lb-xp">${u.totalXP.toLocaleString()} XP</div>
        ${u.todayXP > 0 ? `<span class="lb-badge">+${u.todayXP} today</span>` : ''}
      </div>`;
    lbList.appendChild(row);
  });
}
function openAddModal() {
  document.getElementById('add-modal').classList.add('open');
  document.getElementById('new-name').focus();
}
function closeAddModal() {
  document.getElementById('add-modal').classList.remove('open');
}
function handleModalBg(e) {
  if (e.target === document.getElementById('add-modal')) closeAddModal();
}
function confirmAddSkill() {
  const u    = DB.users[currentUser];
  const name = document.getElementById('new-name').value.trim();
  const cat  = document.getElementById('new-cat').value;
  const xp   = Math.min(500, Math.max(0, parseInt(document.getElementById('new-xp').value) || 0));
  if (!name) { document.getElementById('new-name').focus(); return; }
  u.skills.push({ id: u.nextId++, name, cat, xp, streak: 0, activity: genActivity(Math.ceil(xp / 20)), loggedToday: null });
  saveDB();
  closeAddModal();
  document.getElementById('new-name').value = '';
  document.getElementById('new-xp').value   = '0';
  showToast(name + ' added!');
  renderMy();
}
function openDelModal(id, name) {
  deleteTarget = id;
  document.getElementById('del-skill-name').textContent = name;
  document.getElementById('del-pass').value = '';
  document.getElementById('del-pass-err').textContent = '';
  document.getElementById('del-modal').classList.add('open');
  setTimeout(() => document.getElementById('del-pass').focus(), 100);
}
function closeDelModal() {
  deleteTarget = null;
  document.getElementById('del-pass').value = '';
  document.getElementById('del-pass-err').textContent = '';
  document.getElementById('del-modal').classList.remove('open');
}
function handleDelModalBg(e) {
  if (e.target === document.getElementById('del-modal')) closeDelModal();
}
function confirmDelete() {
  const u        = DB.users[currentUser];
  const entered  = document.getElementById('del-pass').value;
  const errEl    = document.getElementById('del-pass-err');

  if (!entered) { errEl.textContent = 'Please enter your password.'; document.getElementById('del-pass').focus(); return; }
  if (entered !== u.password) { errEl.textContent = 'Incorrect password. Try again.'; document.getElementById('del-pass').value = ''; document.getElementById('del-pass').focus(); return; }

  // Password OK — stash skill for undo
  const idx  = u.skills.findIndex(sk => sk.id === deleteTarget);
  const snap = JSON.parse(JSON.stringify(u.skills[idx]));
  undoData   = { skill: snap, index: idx };

  u.skills = u.skills.filter(sk => sk.id !== deleteTarget);
  if (expandedId === deleteTarget) expandedId = null;
  saveDB();
  closeDelModal();
  showUndoToast('Skill removed.', 5000);
  renderMy();
}
function showToast(msg) {
  clearTimeout(toastTimer);
  const t   = document.getElementById('toast');
  const tm  = document.getElementById('toast-msg');
  // Remove any existing undo button
  const old = t.querySelector('.toast-undo');
  if (old) old.remove();
  tm.textContent = msg;
  t.classList.add('show');
  toastTimer = setTimeout(() => t.classList.remove('show'), 2500);
}
function showUndoToast(msg, duration) {
  clearTimeout(toastTimer);
  clearTimeout(undoTimer);
  const t   = document.getElementById('toast');
  const tm  = document.getElementById('toast-msg');
  const old = t.querySelector('.toast-undo');
  if (old) old.remove();
  tm.textContent = msg;
  const btn = document.createElement('button');
  btn.className   = 'toast-undo';
  btn.textContent = 'Undo';
  btn.onclick     = undoDelete;
  t.appendChild(btn);
  t.classList.add('show');
  // Auto-dismiss & clear undo after duration
  undoTimer  = setTimeout(() => { undoData = null; }, duration);
  toastTimer = setTimeout(() => {
    t.classList.remove('show');
    const b = t.querySelector('.toast-undo');
    if (b) b.remove();
  }, duration);
}
function undoDelete() {
  if (!undoData || !currentUser) return;
  clearTimeout(undoTimer);
  clearTimeout(toastTimer);
  const u = DB.users[currentUser];
  u.skills.splice(undoData.index, 0, undoData.skill);
  undoData = null;
  saveDB();
  const t = document.getElementById('toast');
  t.classList.remove('show');
  const b = t.querySelector('.toast-undo');
  if (b) b.remove();
  showToast('Skill restored!');
  renderMy();
}
function escHtml(str) {
  return str.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}
document.getElementById('li-user').addEventListener('keydown', e => { if (e.key === 'Enter') document.getElementById('li-pass').focus(); });
document.getElementById('li-pass').addEventListener('keydown', e => { if (e.key === 'Enter') doLogin(); });
document.getElementById('rg-user').addEventListener('keydown', e => { if (e.key === 'Enter') document.getElementById('rg-name').focus(); });
document.getElementById('rg-name').addEventListener('keydown', e => { if (e.key === 'Enter') document.getElementById('rg-pass').focus(); });
document.getElementById('rg-pass').addEventListener('keydown', e => { if (e.key === 'Enter') doRegister(); });
document.getElementById('new-name').addEventListener('keydown', e => { if (e.key === 'Enter') confirmAddSkill(); });
document.getElementById('del-pass').addEventListener('keydown', e => { if (e.key === 'Enter') confirmDelete(); });
</script>
</body>
</html>
