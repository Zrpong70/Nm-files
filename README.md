# Nm-files
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MALI · Chat Intelligence Dashboard</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;600;700&family=Noto+Sans+Thai:wght@300;400;500;600;700&display=swap');

  :root {
    --bg-0: #050508;
    --bg-1: #0a0a10;
    --bg-2: #0f0f18;
    --bg-3: #141420;
    --bg-4: #1a1a2a;
    --border: rgba(100,100,200,0.12);
    --border-bright: rgba(120,120,255,0.25);
    --green: #00ff9d;
    --green-dim: rgba(0,255,157,0.15);
    --green-glow: 0 0 12px rgba(0,255,157,0.4);
    --blue: #4fc3f7;
    --blue-dim: rgba(79,195,247,0.12);
    --purple: #b39ddb;
    --purple-dim: rgba(179,157,219,0.15);
    --pink: #f48fb1;
    --orange: #ffb74d;
    --red: #ef5350;
    --text-1: #e8e8f0;
    --text-2: #9999bb;
    --text-3: #555577;
    --radius: 8px;
    --font-mono: 'JetBrains Mono', monospace;
    --font-thai: 'Noto Sans Thai', sans-serif;
    /* User colors */
    --color-thi: #4fc3f7;
    --color-mali: #f48fb1;
    --color-other: #a5d6a7;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg-0);
    color: var(--text-1);
    font-family: var(--font-thai);
    height: 100vh;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }

  /* ── TOP BAR ── */
  .topbar {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 10px 20px;
    background: var(--bg-1);
    border-bottom: 1px solid var(--border);
    flex-shrink: 0;
    z-index: 100;
  }
  .topbar-logo {
    font-family: var(--font-mono);
    font-size: 14px;
    font-weight: 700;
    color: var(--green);
    text-shadow: var(--green-glow);
    letter-spacing: 3px;
    white-space: nowrap;
  }
  .topbar-logo span { color: var(--text-3); font-weight: 300; }
  .topbar-search {
    flex: 1;
    max-width: 400px;
    position: relative;
  }
  .topbar-search input {
    width: 100%;
    background: var(--bg-3);
    border: 1px solid var(--border);
    color: var(--text-1);
    padding: 6px 12px 6px 32px;
    border-radius: 20px;
    font-family: var(--font-thai);
    font-size: 13px;
    outline: none;
    transition: border-color 0.2s;
  }
  .topbar-search input:focus { border-color: var(--green); }
  .topbar-search::before {
    content: '⌕';
    position: absolute; left: 10px; top: 50%; transform: translateY(-50%);
    color: var(--text-3); font-size: 14px; pointer-events: none;
  }
  .topbar-stats {
    display: flex; gap: 12px; margin-left: auto;
  }
  .stat-pill {
    font-family: var(--font-mono);
    font-size: 11px;
    padding: 3px 10px;
    border-radius: 20px;
    border: 1px solid var(--border);
    color: var(--text-2);
    white-space: nowrap;
  }
  .stat-pill.green { border-color: var(--green); color: var(--green); }

  /* ── MAIN LAYOUT ── */
  .main {
    display: flex;
    flex: 1;
    overflow: hidden;
  }

  /* ── SIDEBAR ── */
  .sidebar {
    width: 240px;
    min-width: 200px;
    background: var(--bg-1);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    overflow: hidden;
    flex-shrink: 0;
  }
  .sidebar-section {
    padding: 10px 14px 6px;
    font-family: var(--font-mono);
    font-size: 9px;
    color: var(--text-3);
    letter-spacing: 2px;
    text-transform: uppercase;
    border-bottom: 1px solid var(--border);
  }
  .sidebar-filter {
    padding: 8px 12px;
    display: flex; gap: 6px; flex-wrap: wrap;
    border-bottom: 1px solid var(--border);
  }
  .filter-btn {
    font-family: var(--font-mono);
    font-size: 10px;
    padding: 3px 8px;
    border-radius: 12px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--text-3);
    cursor: pointer;
    transition: all 0.2s;
  }
  .filter-btn:hover, .filter-btn.active { border-color: var(--green); color: var(--green); background: var(--green-dim); }
  .rooms-list {
    flex: 1;
    overflow-y: auto;
    padding: 4px 0;
  }
  .room-item {
    padding: 10px 14px;
    cursor: pointer;
    border-left: 2px solid transparent;
    transition: all 0.15s;
    position: relative;
  }
  .room-item:hover { background: var(--bg-3); }
  .room-item.active { background: var(--bg-4); border-left-color: var(--green); }
  .room-name {
    font-size: 13px;
    font-weight: 500;
    color: var(--text-1);
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  }
  .room-preview {
    font-size: 11px;
    color: var(--text-3);
    margin-top: 2px;
    white-space: nowrap; overflow: hidden; text-overflow: ellipsis;
  }
  .room-badge {
    position: absolute; right: 10px; top: 10px;
    font-family: var(--font-mono);
    font-size: 9px;
    padding: 1px 5px;
    border-radius: 8px;
    background: var(--bg-3);
    color: var(--text-3);
    border: 1px solid var(--border);
  }
  .room-badge.direct { color: var(--blue); border-color: rgba(79,195,247,0.3); }
  .room-badge.group { color: var(--purple); border-color: rgba(179,157,219,0.3); }

  .sidebar-bottom {
    padding: 10px 12px;
    border-top: 1px solid var(--border);
  }
  .user-legend {
    display: flex; flex-direction: column; gap: 4px;
  }
  .legend-item {
    display: flex; align-items: center; gap: 8px;
    font-size: 11px; color: var(--text-2);
  }
  .legend-dot {
    width: 8px; height: 8px; border-radius: 50%;
    flex-shrink: 0;
  }

  /* ── CHAT PANEL ── */
  .chat-panel {
    flex: 1;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    background: var(--bg-0);
  }
  .chat-header {
    padding: 12px 20px;
    background: var(--bg-1);
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 12px;
    flex-shrink: 0;
  }
  .chat-header-title {
    font-weight: 600; font-size: 15px;
  }
  .chat-header-sub {
    font-size: 11px; color: var(--text-3);
    font-family: var(--font-mono);
  }
  .chat-header-actions {
    margin-left: auto;
    display: flex; gap: 8px;
  }
  .action-btn {
    background: var(--bg-3);
    border: 1px solid var(--border);
    color: var(--text-2);
    padding: 5px 10px;
    border-radius: 6px;
    font-size: 11px;
    cursor: pointer;
    font-family: var(--font-mono);
    transition: all 0.2s;
  }
  .action-btn:hover { border-color: var(--green); color: var(--green); background: var(--green-dim); }

  .chat-messages {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .date-divider {
    display: flex; align-items: center; gap: 12px;
    margin: 16px 0 8px;
  }
  .date-divider-line {
    flex: 1; height: 1px; background: var(--border);
  }
  .date-divider-text {
    font-family: var(--font-mono);
    font-size: 10px; color: var(--text-3);
    white-space: nowrap;
  }

  .msg-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 6px;
  }
  .msg-group.me { align-items: flex-end; }
  .msg-group.other { align-items: flex-start; }

  .msg-sender-label {
    font-size: 10px;
    font-family: var(--font-mono);
    color: var(--text-3);
    margin-bottom: 3px;
    padding: 0 4px;
  }
  .msg-group.me .msg-sender-label { color: var(--color-thi); }

  .msg-row {
    display: flex;
    align-items: flex-end;
    gap: 6px;
    max-width: 70%;
  }
  .msg-group.me .msg-row { flex-direction: row-reverse; }

  .msg-avatar {
    width: 24px; height: 24px;
    border-radius: 50%;
    font-size: 10px;
    display: flex; align-items: center; justify-content: center;
    font-weight: 700;
    flex-shrink: 0;
    font-family: var(--font-mono);
    margin-bottom: 1px;
  }

  .msg-bubble {
    padding: 8px 12px;
    border-radius: 16px;
    font-size: 13.5px;
    line-height: 1.55;
    position: relative;
    cursor: pointer;
    transition: opacity 0.15s;
    word-break: break-word;
    max-width: 100%;
  }
  .msg-bubble:hover { opacity: 0.85; }

  /* ME = ธี */
  .msg-group.me .msg-bubble {
    background: linear-gradient(135deg, rgba(79,195,247,0.25), rgba(79,195,247,0.15));
    border: 1px solid rgba(79,195,247,0.3);
    border-radius: 16px 16px 4px 16px;
    color: var(--text-1);
  }
  /* MALI */
  .msg-group.mali .msg-bubble {
    background: linear-gradient(135deg, rgba(244,143,177,0.2), rgba(244,143,177,0.1));
    border: 1px solid rgba(244,143,177,0.3);
    border-radius: 16px 16px 16px 4px;
    color: var(--text-1);
  }
  /* OTHER */
  .msg-group.other .msg-bubble {
    background: var(--bg-3);
    border: 1px solid var(--border);
    border-radius: 16px 16px 16px 4px;
    color: var(--text-1);
  }

  .msg-bubble.highlight {
    border-color: var(--green) !important;
    box-shadow: var(--green-glow) !important;
  }

  .msg-meta {
    font-family: var(--font-mono);
    font-size: 9px;
    color: var(--text-3);
    margin-top: 2px;
    padding: 0 4px;
    display: flex; gap: 8px; align-items: center;
  }
  .msg-group.me .msg-meta { justify-content: flex-end; }

  .pin-icon { color: var(--orange); }
  .copy-icon { cursor: pointer; }
  .copy-icon:hover { color: var(--green); }

  .replied-to {
    font-size: 11px;
    color: var(--text-3);
    border-left: 2px solid var(--text-3);
    padding: 2px 6px;
    margin-bottom: 4px;
    font-style: italic;
  }

  .empty-state {
    flex: 1;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    gap: 12px; color: var(--text-3);
  }
  .empty-state-icon {
    font-size: 40px; opacity: 0.3;
  }
  .empty-state-text {
    font-family: var(--font-mono); font-size: 12px;
    letter-spacing: 2px;
  }

  /* Search highlight */
  mark {
    background: rgba(0,255,157,0.3);
    color: var(--green);
    border-radius: 2px;
    padding: 0 1px;
  }

  /* ── AI PANEL ── */
  .ai-panel {
    width: 300px;
    min-width: 260px;
    background: var(--bg-1);
    border-left: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    overflow: hidden;
    flex-shrink: 0;
  }
  .ai-header {
    padding: 12px 14px;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 8px;
  }
  .ai-header-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: var(--green-glow);
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%,100% { opacity: 1; }
    50% { opacity: 0.4; }
  }
  .ai-header-title {
    font-family: var(--font-mono);
    font-size: 11px; font-weight: 600;
    color: var(--green);
    letter-spacing: 2px;
  }
  .ai-output {
    flex: 1;
    overflow-y: auto;
    padding: 12px;
    font-family: var(--font-mono);
    font-size: 12px;
    line-height: 1.7;
    color: var(--text-2);
    display: flex; flex-direction: column; gap: 10px;
  }
  .ai-msg {
    display: flex; flex-direction: column; gap: 4px;
  }
  .ai-msg-role {
    font-size: 9px; letter-spacing: 2px;
    color: var(--text-3);
  }
  .ai-msg.user .ai-msg-role { color: var(--blue); }
  .ai-msg.assistant .ai-msg-role { color: var(--green); }
  .ai-msg-content {
    background: var(--bg-3);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 8px 10px;
    color: var(--text-1);
    font-size: 12px;
    white-space: pre-wrap;
    word-break: break-word;
  }
  .ai-msg.user .ai-msg-content {
    background: var(--blue-dim);
    border-color: rgba(79,195,247,0.2);
    color: var(--blue);
  }
  .ai-msg.assistant .ai-msg-content {
    background: var(--green-dim);
    border-color: rgba(0,255,157,0.15);
  }

  .ai-quick-btns {
    padding: 8px 12px;
    display: flex; flex-direction: column; gap: 5px;
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
  }
  .ai-quick-label {
    font-family: var(--font-mono);
    font-size: 9px; color: var(--text-3); letter-spacing: 1px;
    margin-bottom: 2px;
  }
  .quick-btn {
    background: var(--bg-3);
    border: 1px solid var(--border);
    color: var(--text-2);
    padding: 5px 10px;
    border-radius: 5px;
    font-size: 11px;
    cursor: pointer;
    text-align: left;
    font-family: var(--font-thai);
    transition: all 0.15s;
  }
  .quick-btn:hover { border-color: var(--green); color: var(--green); background: var(--green-dim); }

  .ai-input-row {
    padding: 10px 12px;
    display: flex; gap: 8px;
    flex-shrink: 0;
  }
  .ai-input {
    flex: 1;
    background: var(--bg-3);
    border: 1px solid var(--border);
    color: var(--text-1);
    padding: 7px 10px;
    border-radius: 6px;
    font-family: var(--font-thai);
    font-size: 12px;
    outline: none;
    transition: border-color 0.2s;
    resize: none;
  }
  .ai-input:focus { border-color: var(--green); }
  .ai-send-btn {
    background: var(--green-dim);
    border: 1px solid var(--green);
    color: var(--green);
    padding: 7px 12px;
    border-radius: 6px;
    cursor: pointer;
    font-family: var(--font-mono);
    font-size: 12px;
    font-weight: 600;
    transition: all 0.2s;
    flex-shrink: 0;
  }
  .ai-send-btn:hover { background: var(--green); color: var(--bg-0); }

  /* scrollbar */
  ::-webkit-scrollbar { width: 4px; height: 4px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border-bright); border-radius: 4px; }
  ::-webkit-scrollbar-thumb:hover { background: var(--text-3); }

  /* copy toast */
  .toast {
    position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
    background: var(--green);
    color: var(--bg-0);
    padding: 8px 20px;
    border-radius: 20px;
    font-family: var(--font-mono);
    font-size: 12px;
    font-weight: 700;
    z-index: 9999;
    opacity: 0;
    transition: opacity 0.3s;
    pointer-events: none;
  }
  .toast.show { opacity: 1; }

  /* stats bar in sidebar */
  .mini-stats {
    padding: 8px 12px;
    border-bottom: 1px solid var(--border);
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 6px;
  }
  .mini-stat {
    background: var(--bg-3);
    border: 1px solid var(--border);
    border-radius: 5px;
    padding: 5px 8px;
  }
  .mini-stat-val {
    font-family: var(--font-mono);
    font-size: 14px;
    font-weight: 700;
    color: var(--green);
  }
  .mini-stat-lbl {
    font-size: 9px;
    color: var(--text-3);
    font-family: var(--font-mono);
  }

  .search-bar-room {
    padding: 6px 10px;
    border-bottom: 1px solid var(--border);
  }
  .search-bar-room input {
    width: 100%;
    background: var(--bg-3);
    border: 1px solid var(--border);
    color: var(--text-1);
    padding: 5px 10px;
    border-radius: 5px;
    font-size: 12px;
    font-family: var(--font-thai);
    outline: none;
  }
  .search-bar-room input:focus { border-color: var(--blue); }

  /* pinned section */
  .pinned-bar {
    padding: 6px 14px;
    background: rgba(255,183,77,0.05);
    border-bottom: 1px solid rgba(255,183,77,0.15);
    font-size: 11px;
    color: var(--orange);
    font-family: var(--font-mono);
    display: flex; align-items: center; gap: 6px;
    cursor: pointer;
    transition: background 0.2s;
  }
  .pinned-bar:hover { background: rgba(255,183,77,0.1); }
  .pinned-bar.hidden { display: none; }

  /* ai thinking */
  .ai-thinking {
    display: flex; gap: 4px; padding: 8px 10px;
  }
  .ai-thinking span {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--green);
    animation: bounce 1.2s infinite;
  }
  .ai-thinking span:nth-child(2) { animation-delay: 0.2s; }
  .ai-thinking span:nth-child(3) { animation-delay: 0.4s; }
  @keyframes bounce {
    0%,60%,100% { transform: translateY(0); opacity: 0.4; }
    30% { transform: translateY(-6px); opacity: 1; }
  }

  /* msg count badge on room */
  .room-msg-count {
    font-family: var(--font-mono);
    font-size: 9px;
    color: var(--text-3);
    margin-top: 2px;
  }

  /* ── RESPONSIVE ── */
  @media (max-width: 900px) {
    .ai-panel { display: none; }
  }
  @media (max-width: 600px) {
    .sidebar { width: 180px; }
  }
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <div class="topbar-logo">MALI<span>.chat</span></div>
  <div class="topbar-search">
    <input type="text" id="globalSearch" placeholder="ค้นหาข้อความทั้งหมด..." oninput="globalSearchMessages(this.value)">
  </div>
  <div class="topbar-stats">
    <div class="stat-pill green" id="totalMsgStat">— ข้อความ</div>
    <div class="stat-pill" id="totalRoomStat">— rooms</div>
  </div>
</div>

<!-- MAIN -->
<div class="main">

  <!-- SIDEBAR -->
  <div class="sidebar">
    <div class="sidebar-section">ROOMS</div>
    <div class="mini-stats">
      <div class="mini-stat">
        <div class="mini-stat-val" id="sideStatMsg">—</div>
        <div class="mini-stat-lbl">messages</div>
      </div>
      <div class="mini-stat">
        <div class="mini-stat-val" id="sideStatRooms">—</div>
        <div class="mini-stat-lbl">rooms</div>
      </div>
    </div>
    <div class="sidebar-filter">
      <button class="filter-btn active" onclick="filterRooms('all',this)">ALL</button>
      <button class="filter-btn" onclick="filterRooms('direct',this)">DM</button>
      <button class="filter-btn" onclick="filterRooms('group',this)">GROUP</button>
    </div>
    <div class="search-bar-room">
      <input type="text" placeholder="ค้นหาห้อง..." oninput="filterRoomsByName(this.value)">
    </div>
    <div class="rooms-list" id="roomsList"></div>
    <div class="sidebar-bottom">
      <div class="user-legend">
        <div style="font-family:var(--font-mono);font-size:9px;color:var(--text-3);letter-spacing:1px;margin-bottom:4px;">USER COLORS</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--color-thi)"></div>ธี (เรา)</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--color-mali)"></div>หมายมี</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--color-other)"></div>คนอื่น</div>
      </div>
    </div>
  </div>

  <!-- CHAT PANEL -->
  <div class="chat-panel">
    <div class="chat-header">
      <div>
        <div class="chat-header-title" id="chatRoomName">เลือกห้องสนทนา</div>
        <div class="chat-header-sub" id="chatRoomSub">—</div>
      </div>
      <div class="chat-header-actions">
        <button class="action-btn" onclick="sortMessages()">⇅ Sort</button>
        <button class="action-btn" onclick="scrollToBottom()">↓ Latest</button>
        <button class="action-btn" onclick="showPinned()">📌 Pinned</button>
      </div>
    </div>
    <div class="pinned-bar hidden" id="pinnedBar" onclick="hidePinned()">
      📌 <span id="pinnedCount">0</span> ข้อความที่ปิน — คลิกเพื่อซ่อน
    </div>
    <div class="chat-messages" id="chatMessages">
      <div class="empty-state">
        <div class="empty-state-icon">💬</div>
        <div class="empty-state-text">SELECT A ROOM</div>
        <div style="font-size:11px;color:var(--text-3);font-family:var(--font-mono)">เลือกห้องสนทนาจาก sidebar</div>
      </div>
    </div>
  </div>

  <!-- AI PANEL -->
  <div class="ai-panel">
    <div class="ai-header">
      <div class="ai-header-dot"></div>
      <div class="ai-header-title">AI · ANALYST</div>
    </div>
    <div class="ai-output" id="aiOutput">
      <div class="ai-msg assistant">
        <div class="ai-msg-role">▶ SYSTEM</div>
        <div class="ai-msg-content">พร้อมวิเคราะห์ข้อมูลแชท\nถามอะไรก็ได้เกี่ยวกับบทสนทนาในระบบ</div>
      </div>
    </div>
    <div class="ai-quick-btns">
      <div class="ai-quick-label">▸ QUICK QUERIES</div>
      <button class="quick-btn" onclick="aiQuery('สรุปบทสนทนาทั้งหมด')">📋 สรุปบทสนทนาทั้งหมด</button>
      <button class="quick-btn" onclick="aiQuery('ใครพูดเยอะที่สุด')">📊 ใครพูดเยอะที่สุด</button>
      <button class="quick-btn" onclick="aiQuery('หมายมีทำอะไรกับธี')">🔍 พฤติกรรมของหมายมี</button>
      <button class="quick-btn" onclick="aiQuery('keyword ที่พบบ่อย')">🔑 Keywords บ่อยที่สุด</button>
      <button class="quick-btn" onclick="aiQuery('วิเคราะห์ความสัมพันธ์')">💔 วิเคราะ
