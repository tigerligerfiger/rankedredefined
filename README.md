 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>AlexRedefined — Chat Ranked</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Rajdhani:wght@500;600;700&display=swap" rel="stylesheet"/>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root {
    --orange: #FF4500;
    --bg: #080810;
    --bg2: #0d0d0d;
    --bg3: #111;
  }
  body { background: var(--bg); color: #fff; font-family: 'Bebas Neue', Impact, sans-serif; min-height: 100vh; }
  ::-webkit-scrollbar { width: 3px; }
  ::-webkit-scrollbar-thumb { background: var(--orange); border-radius: 2px; }
  ::-webkit-scrollbar-track { background: #0a0a0a; }

  /* Header */
  #header { background: linear-gradient(90deg,#0d0500,#110a00,#0d0500); border-bottom: 1px solid #FF450022; padding: 0 20px; }
  #header-inner { max-width: 880px; margin: 0 auto; }
  #top-row { padding: 22px 0 0; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 12px; }
  #logo { display: flex; align-items: center; gap: 14px; }
  #logo-icon { font-size: 2.4rem; }
  #title { font-size: 2.3rem; color: var(--orange); line-height: 1; letter-spacing: .08em; text-shadow: 0 0 28px #FF450077; }
  #subtitle { font-family: 'Rajdhani', sans-serif; font-size: .78rem; color: #555; letter-spacing: .2em; font-weight: 700; text-transform: uppercase; }
  #status-row { display: flex; align-items: center; gap: 10px; }
  #status-dot { width: 8px; height: 8px; border-radius: 50%; background: #555; flex-shrink: 0; transition: background .3s; }
  #status-dot.connected { background: #00ff87; animation: liveGlow 2s ease infinite; }
  #status-dot.connecting { background: #FFD700; animation: pulse 1s ease infinite; }
  #status-dot.error { background: #ff4444; }
  #status-text { font-family: 'Rajdhani', sans-serif; font-size: .82rem; font-weight: 700; letter-spacing: .12em; color: #555; transition: color .3s; }
  #connect-btn { background: var(--orange); border: none; border-radius: 7px; padding: 9px 20px; color: #fff; font-family: 'Bebas Neue', sans-serif; font-size: .95rem; letter-spacing: .1em; cursor: pointer; transition: all .15s; }
  #connect-btn:hover { filter: brightness(1.15); transform: scale(1.02); }
  #connect-btn.disconnect { background: #2a0000; border: 1px solid #550000; color: #ff6666; }

  #stats-row { display: flex; gap: 28px; padding: 14px 0 0; flex-wrap: wrap; }
  .stat-val { font-size: 1.3rem; color: var(--orange); }
  .stat-label { font-family: 'Rajdhani', sans-serif; font-size: .7rem; color: #444; font-weight: 700; letter-spacing: .12em; margin-left: 6px; }

  /* Tabs */
  #tabs { display: flex; gap: 2px; margin-top: 14px; }
  .tab { background: none; border: none; border-bottom: 2px solid transparent; cursor: pointer; font-family: 'Bebas Neue', sans-serif; letter-spacing: .1em; padding: 10px 22px; font-size: 1rem; color: #444; transition: all .2s; }
  .tab.active { color: var(--orange); border-bottom: 2px solid var(--orange); }
  .tab:hover:not(.active) { color: #888; }

  /* Content */
  #content { max-width: 880px; margin: 0 auto; padding: 28px 20px; }

  /* Leaderboard */
  #lb-list { display: flex; flex-direction: column; gap: 6px; }
  .lb-row { background: #0c0c0c; border: 1px solid #181818; border-radius: 10px; padding: 12px 16px; display: flex; align-items: center; gap: 12px; transition: background .15s, border-color .15s; }
  .lb-row:hover { background: rgba(255,69,0,.06); }
  .lb-row.top { background: #0f0a00; }
  .lb-pos { width: 32px; text-align: center; font-size: 1rem; color: #333; flex-shrink: 0; }
  .lb-icon { font-size: 1.4rem; flex-shrink: 0; }
  .lb-info { flex: 1; min-width: 0; }
  .lb-name { font-size: 1.05rem; letter-spacing: .05em; color: #eee; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .bar-wrap { display: flex; align-items: center; gap: 7px; margin-top: 4px; }
  .bar-bg { background: #1a1a1a; border-radius: 20px; height: 3px; width: 100px; overflow: hidden; flex-shrink: 0; }
  .bar-fill { height: 100%; border-radius: 20px; transition: width .8s cubic-bezier(.4,0,.2,1); }
  .bar-rank { font-family: 'Rajdhani', sans-serif; font-size: .7rem; font-weight: 700; letter-spacing: .05em; }
  .lb-elo { text-align: right; flex-shrink: 0; }
  .lb-elo-val { font-size: 1.4rem; line-height: 1; }
  .lb-elo-label { font-family: 'Rajdhani', sans-serif; font-size: .65rem; color: #333; font-weight: 700; letter-spacing: .1em; }

  #lb-empty { text-align: center; padding: 70px 0; font-family: 'Rajdhani', sans-serif; color: #222; font-size: 1rem; font-weight: 700; letter-spacing: .15em; }

  /* Live chat */
  #chat-header { display: flex; align-items: center; gap: 8px; margin-bottom: 14px; }
  #chat-live-dot { width: 7px; height: 7px; border-radius: 50%; background: #222; flex-shrink: 0; }
  #chat-live-dot.live { background: #00ff87; animation: pulse 1.2s ease infinite; }
  #chat-label { font-family: 'Rajdhani', sans-serif; font-size: .8rem; color: #444; font-weight: 700; letter-spacing: .15em; }
  #chat-feed { background: #0a0a0a; border: 1px solid #181818; border-radius: 12px; height: 480px; overflow-y: auto; padding: 12px 14px; display: flex; flex-direction: column; gap: 5px; }
  #chat-empty { margin: auto; font-family: 'Rajdhani', sans-serif; color: #2a2a2a; font-size: .9rem; font-weight: 700; letter-spacing: .1em; text-align: center; }
  .chat-entry { display: flex; gap: 7px; align-items: flex-start; animation: slideIn .25s ease; }
  .chat-rank-icon { flex-shrink: 0; font-size: 13px; margin-top: 2px; }
  .chat-username { font-size: .9rem; letter-spacing: .04em; flex-shrink: 0; }
  .chat-elo { font-family: 'Rajdhani', sans-serif; font-size: .72rem; color: #2e2e2e; font-weight: 700; margin-top: 2px; flex-shrink: 0; }
  .chat-msg { font-family: 'Rajdhani', sans-serif; font-size: .88rem; color: #999; font-weight: 600; word-break: break-word; }

  /* Ranks */
  #ranks-list { display: flex; flex-direction: column; gap: 12px; }
  .rank-card { background: #0c0c0c; border-radius: 10px; border-left: 3px solid; padding: 18px 22px; display: flex; align-items: center; gap: 18px; }
  .rank-card-icon { font-size: 2.2rem; }
  .rank-card-info { flex: 1; }
  .rank-card-name { font-size: 1.7rem; line-height: 1; }
  .rank-card-range { font-family: 'Rajdhani', sans-serif; font-size: .8rem; color: #444; font-weight: 700; letter-spacing: .1em; }
  .rank-card-count { text-align: center; background: #111; border-radius: 8px; padding: 8px 16px; min-width: 64px; }
  .rank-count-val { font-size: 1.6rem; line-height: 1; }
  .rank-count-label { font-family: 'Rajdhani', sans-serif; font-size: .65rem; color: #333; font-weight: 700; letter-spacing: .1em; }

  /* Toast */
  #toasts { position: fixed; top: 16px; right: 16px; display: flex; flex-direction: column; gap: 6px; z-index: 999; pointer-events: none; }
  .toast { background: #111; border-radius: 8px; padding: 8px 14px; display: flex; align-items: center; gap: 8px; animation: gainPop 2.5s ease forwards; }
  .toast-name { font-size: .95rem; letter-spacing: .05em; }
  .toast-info { font-family: 'Rajdhani', sans-serif; font-size: .75rem; color: #555; font-weight: 700; }

  @keyframes liveGlow { 0%,100%{box-shadow:0 0 8px #00ff8755} 50%{box-shadow:0 0 22px #00ff87aa} }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.4} }
  @keyframes slideIn { from{opacity:0;transform:translateX(-8px)} to{opacity:1;transform:translateX(0)} }
  @keyframes gainPop { 0%{opacity:0;transform:translateY(6px) scale(.9)} 15%{opacity:1;transform:translateY(0) scale(1)} 80%{opacity:1} 100%{opacity:0} }

  .hidden { display: none !important; }
</style>
</head>
<body>

<div id="header">
  <div id="header-inner">
    <div id="top-row">
      <div id="logo">
        <div id="logo-icon">🎮</div>
        <div>
          <div id="title">ALEXREDEFINED</div>
          <div id="subtitle">Live Chat Ranked System &nbsp;•&nbsp; +1 ELO per message</div>
        </div>
      </div>
      <div id="status-row">
        <div id="status-dot"></div>
        <span id="status-text">OFFLINE</span>
        <button id="connect-btn" onclick="toggleConnect()">CONNECT TO CHAT</button>
      </div>
    </div>
    <div id="stats-row">
      <div><span class="stat-val" id="stat-chatters">0</span><span class="stat-label">CHATTERS TRACKED</span></div>
      <div><span class="stat-val" id="stat-msgs">0</span><span class="stat-label">TOTAL MESSAGES</span></div>
      <div><span class="stat-val" id="stat-top">—</span><span class="stat-label">TOP CHATTER</span></div>
    </div>
    <div id="tabs">
      <button class="tab active" onclick="switchTab('leaderboard', this)">LEADERBOARD</button>
      <button class="tab" onclick="switchTab('livechat', this)">LIVE CHAT</button>
      <button class="tab" onclick="switchTab('ranks', this)">RANKS</button>
    </div>
  </div>
</div>

<div id="content">
  <!-- Leaderboard -->
  <div id="tab-leaderboard">
    <div id="lb-empty">CONNECT TO START TRACKING</div>
    <div id="lb-list"></div>
  </div>

  <!-- Live Chat -->
  <div id="tab-livechat" class="hidden">
    <div id="chat-header">
      <div id="chat-live-dot"></div>
      <span id="chat-label">NOT CONNECTED</span>
    </div>
    <div id="chat-feed">
      <div id="chat-empty">CONNECT TO SEE LIVE CHAT</div>
    </div>
  </div>

  <!-- Ranks -->
  <div id="tab-ranks" class="hidden">
    <div id="ranks-list"></div>
  </div>
</div>

<div id="toasts"></div>

<script>
const CHANNEL = 'alexredefined';
const RANKS = [
  { name:'Bronze',    min:0,     max:999,   color:'#CD7F32', icon:'🥉' },
  { name:'Silver',    min:1000,  max:4999,  color:'#C0C0C0', icon:'🥈' },
  { name:'Gold',      min:5000,  max:14999, color:'#FFD700', icon:'🥇' },
  { name:'Diamond',   min:15000, max:29999, color:'#7DF9FF', icon:'💎' },
  { name:'Alex',      min:30000, max:49999, color:'#FF4500', icon:'🔥' },
  { name:'True Alex', min:50000, max:100000,color:'#FFFFFF', icon:'👑' },
];

function getRank(elo) {
  for (let i = RANKS.length - 1; i >= 0; i--) if (elo >= RANKS[i].min) return RANKS[i];
  return RANKS[0];
}
function getProgress(elo) {
  const r = getRank(elo);
  return Math.min(100, ((elo - r.min) / (r.max - r.min)) * 100);
}

// State
let leaderboard = JSON.parse(localStorage.getItem('alex-lb') || '{}');
let totalMsgs = parseInt(localStorage.getItem('alex-msgs') || '0');
let ws = null, pingInterval = null;
let currentTab = 'leaderboard';

// Save
function save() {
  localStorage.setItem('alex-lb', JSON.stringify(leaderboard));
  localStorage.setItem('alex-msgs', totalMsgs);
}

// Status UI
function setStatus(state) {
  const dot = document.getElementById('status-dot');
  const text = document.getElementById('status-text');
  const btn = document.getElementById('connect-btn');
  dot.className = state;
  if (state === 'connected') {
    text.style.color = '#00ff87'; text.textContent = 'LIVE — #' + CHANNEL;
    btn.className = 'disconnect'; btn.textContent = 'DISCONNECT';
    document.getElementById('chat-live-dot').className = 'live';
    document.getElementById('chat-label').textContent = 'LIVE FEED — #' + CHANNEL;
  } else if (state === 'connecting') {
    text.style.color = '#FFD700'; text.textContent = 'CONNECTING...';
    btn.className = ''; btn.textContent = 'CONNECTING...'; btn.disabled = true;
  } else if (state === 'error') {
    text.style.color = '#ff4444'; text.textContent = 'CONNECTION ERROR';
    btn.className = ''; btn.textContent = 'RETRY'; btn.disabled = false;
    document.getElementById('chat-live-dot').className = '';
    document.getElementById('chat-label').textContent = 'NOT CONNECTED';
  } else {
    text.style.color = '#555'; text.textContent = 'OFFLINE';
    btn.className = ''; btn.textContent = 'CONNECT TO CHAT'; btn.disabled = false;
    document.getElementById('chat-live-dot').className = '';
    document.getElementById('chat-label').textContent = 'NOT CONNECTED';
  }
}

function toggleConnect() {
  if (ws && (ws.readyState === WebSocket.OPEN || ws.readyState === WebSocket.CONNECTING)) {
    disconnect();
  } else {
    connect();
  }
}

function connect() {
  setStatus('connecting');
  ws = new WebSocket('wss://irc-ws.chat.twitch.tv:443');

  ws.onopen = () => {
    ws.send('CAP REQ :twitch.tv/tags twitch.tv/commands');
    ws.send('PASS SCHMOOPIIE');
    ws.send('NICK justinfan' + Math.floor(Math.random() * 99999));
    ws.send('JOIN #' + CHANNEL);
    pingInterval = setInterval(() => { if (ws.readyState === WebSocket.OPEN) ws.send('PING :tmi.twitch.tv'); }, 60000);
  };

  ws.onmessage = (e) => {
    const lines = e.data.trim().split('\r\n');
    lines.forEach(line => {
      if (line.startsWith('PING')) { ws.send('PONG :tmi.twitch.tv'); return; }
      if (line.includes(':tmi.twitch.tv 001')) setStatus('connected');
      if (!line.includes('PRIVMSG')) return;

      // Parse tags
      let displayName = null, color = null;
      const tagMatch = line.match(/^@([^ ]+)/);
      if (tagMatch) {
        tagMatch[1].split(';').forEach(t => {
          const [k,v] = t.split('=');
          if (k === 'display-name' && v) displayName = v;
          if (k === 'color' && v) color = v;
        });
      }
      const nickMatch = line.match(/:([^!]+)!/);
      const msgMatch = line.match(/PRIVMSG #\w+ :(.+)/);
      if (!nickMatch || !msgMatch) return;

      const key = nickMatch[1].toLowerCase();
      const username = displayName || nickMatch[1];
      const msg = msgMatch[1];

      if (!leaderboard[key]) leaderboard[key] = { username, elo: 0, color };
      leaderboard[key].elo++;
      leaderboard[key].username = username;
      if (!leaderboard[key].color && color) leaderboard[key].color = color;
      totalMsgs++;

      const rank = getRank(leaderboard[key].elo);

      // Debounced save
      clearTimeout(window._saveTimer);
      window._saveTimer = setTimeout(save, 1500);

      updateStats();
      if (currentTab === 'leaderboard') renderLeaderboard();
      if (currentTab === 'livechat') appendChat(username, msg, leaderboard[key].elo, rank, color);
      if (currentTab === 'ranks') renderRanks();
      showToast(username, rank, leaderboard[key].elo);
    });
  };

  ws.onerror = () => setStatus('error');
  ws.onclose = () => { clearInterval(pingInterval); if (document.getElementById('status-text').textContent !== 'OFFLINE') setStatus('disconnected'); };
}

function disconnect() {
  if (ws) ws.close();
  clearInterval(pingInterval);
  setStatus('disconnected');
}

// Render leaderboard
function renderLeaderboard() {
  const sorted = Object.values(leaderboard).sort((a,b) => b.elo - a.elo);
  const empty = document.getElementById('lb-empty');
  const list = document.getElementById('lb-list');

  if (sorted.length === 0) { empty.style.display = ''; list.innerHTML = ''; return; }
  empty.style.display = 'none';

  list.innerHTML = sorted.map((user, i) => {
    const rank = getRank(user.elo);
    const prog = getProgress(user.elo);
    const isTop = i < 3;
    const posIcons = ['🥇','🥈','🥉'];
    return `
      <div class="lb-row ${isTop ? 'top' : ''}" style="border-color:${isTop ? rank.color+'44' : '#181818'}">
        <div class="lb-pos" style="color:${i===0?'#FFD700':i===1?'#C0C0C0':i===2?'#CD7F32':'#333'};font-size:${isTop?'1.3rem':'1rem'}">
          ${isTop ? posIcons[i] : '#'+(i+1)}
        </div>
        <div class="lb-icon">${rank.icon}</div>
        <div class="lb-info">
          <div class="lb-name" style="color:${user.color||'#eee'}">${escHtml(user.username)}</div>
          <div class="bar-wrap">
            <div class="bar-bg"><div class="bar-fill" style="width:${prog}%;background:${rank.color}"></div></div>
            <span class="bar-rank" style="color:${rank.color}">${rank.name}</span>
          </div>
        </div>
        <div class="lb-elo">
          <div class="lb-elo-val" style="color:${rank.color}">${user.elo.toLocaleString()}</div>
          <div class="lb-elo-label">ELO</div>
        </div>
      </div>`;
  }).join('');
}

// Append chat message
function appendChat(username, msg, elo, rank, color) {
  const feed = document.getElementById('chat-feed');
  const empty = document.getElementById('chat-empty');
  if (empty) empty.remove();

  const el = document.createElement('div');
  el.className = 'chat-entry';
  el.innerHTML = `
    <span class="chat-rank-icon">${rank.icon}</span>
    <span class="chat-username" style="color:${color||rank.color}">${escHtml(username)}</span>
    <span class="chat-elo">[${elo}]</span>
    <span class="chat-msg">${escHtml(msg)}</span>`;
  feed.appendChild(el);
  // keep max 100 entries
  while (feed.children.length > 100) feed.removeChild(feed.firstChild);
  feed.scrollTop = feed.scrollHeight;
}

// Render ranks tab
function renderRanks() {
  const list = document.getElementById('ranks-list');
  list.innerHTML = RANKS.map(rank => {
    const count = Object.values(leaderboard).filter(u => getRank(u.elo).name === rank.name).length;
    return `
      <div class="rank-card" style="border-left-color:${rank.color};box-shadow:0 0 18px ${rank.color}0d">
        <div class="rank-card-icon">${rank.icon}</div>
        <div class="rank-card-info">
          <div class="rank-card-name" style="color:${rank.color};text-shadow:0 0 12px ${rank.color}55">${rank.name.toUpperCase()}</div>
          <div class="rank-card-range">${rank.min.toLocaleString()} – ${rank.max.toLocaleString()} ELO</div>
        </div>
        <div class="rank-card-count">
          <div class="rank-count-val" style="color:${rank.color}">${count}</div>
          <div class="rank-count-label">CHATTERS</div>
        </div>
      </div>`;
  }).join('');
}

// Stats
function updateStats() {
  const sorted = Object.values(leaderboard).sort((a,b) => b.elo - a.elo);
  document.getElementById('stat-chatters').textContent = sorted.length.toLocaleString();
  document.getElementById('stat-msgs').textContent = totalMsgs.toLocaleString();
  document.getElementById('stat-top').textContent = sorted[0]?.username || '—';
}

// Toast
function showToast(username, rank, elo) {
  const box = document.getElementById('toasts');
  const el = document.createElement('div');
  el.className = 'toast';
  el.style.border = `1px solid ${rank.color}55`;
  el.innerHTML = `<span>${rank.icon}</span><span class="toast-name" style="color:${rank.color}">${escHtml(username)}</span><span class="toast-info">+1 ELO → ${elo.toLocaleString()}</span>`;
  box.appendChild(el);
  setTimeout(() => el.remove(), 2600);
  // keep max 5
  while (box.children.length > 5) box.removeChild(box.firstChild);
}

// Tab switching
function switchTab(name, btn) {
  currentTab = name;
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('tab-leaderboard').classList.toggle('hidden', name !== 'leaderboard');
  document.getElementById('tab-livechat').classList.toggle('hidden', name !== 'livechat');
  document.getElementById('tab-ranks').classList.toggle('hidden', name !== 'ranks');
  if (name === 'leaderboard') renderLeaderboard();
  if (name === 'ranks') renderRanks();
}

function escHtml(s) {
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

// Init
updateStats();
renderLeaderboard();
renderRanks();
</script>
</body>
</html>
