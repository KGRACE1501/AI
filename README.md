<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>🌸 Cute Schedule Planner</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
<script src="https://accounts.google.com/gsi/client" async defer></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{
  font-family:'Noto Sans KR',sans-serif;
  background:linear-gradient(135deg,#fce7f3 0%,#e9d5ff 45%,#dbeafe 100%);
  min-height:100vh;
  padding:20px;
  color:#374151;
}
.container{max-width:1400px;margin:0 auto;position:relative}
header{text-align:center;margin-bottom:24px}
header h1{
  font-size:3rem;
  background:linear-gradient(135deg,#ec4899,#8b5cf6,#3b82f6);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  font-weight:800;letter-spacing:-0.03em
}
header p{color:#6b7280;margin-top:8px;font-size:1.05rem}
.badges{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;margin:18px 0 26px}
.badge{
  background:rgba(255,255,255,.8);
  border:1px solid rgba(255,255,255,.8);
  backdrop-filter:blur(10px);
  padding:10px 16px;border-radius:999px;font-weight:700;
  box-shadow:0 8px 24px rgba(0,0,0,.06)
}
.setup-box{
  background:#fff7ed;border:1px dashed #f59e0b;color:#92400e;
  padding:16px 18px;border-radius:18px;margin-bottom:18px
}
.setup-box code{background:#fff;padding:2px 8px;border-radius:8px}
.sync-row{display:flex;gap:12px;justify-content:center;align-items:center;flex-wrap:wrap;margin-bottom:22px}
.btn{
  border:none;border-radius:16px;padding:13px 20px;font-weight:700;
  cursor:pointer;transition:.2s;box-shadow:0 8px 20px rgba(0,0,0,.08);
  font-family:'Noto Sans KR',sans-serif
}
.btn:hover{transform:translateY(-2px)}
.btn.primary{background:linear-gradient(135deg,#ec4899,#8b5cf6);color:#fff}
.btn.secondary{background:#fff;color:#6b7280}
.btn.success{background:linear-gradient(135deg,#22c55e,#16a34a);color:#fff}
.btn.danger{background:linear-gradient(135deg,#ef4444,#dc2626);color:#fff}
.sync-status{
  display:none;align-items:center;gap:10px;background:#fff;border-radius:999px;
  padding:11px 16px;font-weight:700;color:#059669
}
.sync-status.active{display:flex}
.dot{width:10px;height:10px;border-radius:50%;background:#22c55e;animation:pulse 1.8s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.35}}
.stats{
  display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
  gap:14px;margin-bottom:22px
}
.card{
  background:rgba(255,255,255,.88);
  border:1px solid rgba(255,255,255,.9);
  backdrop-filter:blur(10px);
  border-radius:24px;padding:24px;
  box-shadow:0 12px 36px rgba(17,24,39,.08)
}
.stat{ text-align:center }
.stat-number{
  font-size:2.2rem;font-weight:800;
  background:linear-gradient(135deg,#ec4899,#8b5cf6);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent
}
.stat-label{margin-top:4px;color:#6b7280;font-size:.92rem}
.grid{display:grid;grid-template-columns:1fr 1fr;gap:20px}
@media(max-width:1024px){.grid{grid-template-columns:1fr}header h1{font-size:2.2rem}}
.card h2{
  display:flex;align-items:center;gap:10px;
  margin-bottom:18px;font-size:1.45rem;color:#111827
}
.card h2::after{content:"";flex:1;height:2px;border-radius:999px;background:linear-gradient(90deg,#8b5cf6,transparent)}
.row{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:12px}
input[type=text],input[type=datetime-local],select,textarea{
  flex:1;min-width:160px;border:1px solid #e5e7eb;background:#fff;border-radius:14px;
  padding:13px 15px;font-family:'Noto Sans KR',sans-serif;font-size:1rem;
  transition:.2s;outline:none
}
input:focus,select:focus,textarea:focus{border-color:#8b5cf6;box-shadow:0 0 0 4px rgba(139,92,246,.12)}
textarea{min-height:74px;resize:vertical}
.filters{display:flex;gap:8px;flex-wrap:wrap;margin:10px 0 14px}
.filter-btn{
  border:none;background:#f3f4f6;color:#6b7280;border-radius:999px;padding:9px 14px;
  font-weight:700;cursor:pointer
}
.filter-btn.active{background:linear-gradient(135deg,#ec4899,#8b5cf6);color:#fff}
.list{max-height:560px;overflow:auto;padding-right:6px}
.item{
  background:linear-gradient(135deg,#fff,#f8fbff);
  border:1px solid #eef2ff;border-left:5px solid #8b5cf6;
  border-radius:18px;padding:16px;margin-bottom:12px;
  display:flex;justify-content:space-between;gap:14px;align-items:flex-start
}
.item.completed{opacity:.6;border-left-color:#22c55e}
.item.overdue{border-left-color:#ef4444}
.item.today{border-left-color:#f59e0b}
.title{font-weight:800;color:#111827;font-size:1.03rem}
.meta{margin-top:6px;color:#6b7280;font-size:.92rem;line-height:1.45}
.tags{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
.tag{
  display:inline-flex;align-items:center;gap:6px;
  padding:5px 10px;border-radius:999px;font-size:.78rem;font-weight:800
}
.tag.low{background:#dcfce7;color:#166534}
.tag.medium{background:#fef3c7;color:#92400e}
.tag.high{background:#fee2e2;color:#991b1b}
.tag.ai{background:#fce7f3;color:#be185d}
.tag.gcal{background:#dbeafe;color:#1d4ed8}
.actions{display:flex;gap:8px;flex-wrap:wrap;justify-content:flex-end}
.actions .btn{padding:9px 13px;border-radius:12px;font-size:.9rem;box-shadow:none}
.empty{
  text-align:center;color:#9ca3af;padding:48px 20px;font-weight:600
}
.toast{
  position:fixed;left:50%;bottom:26px;transform:translateX(-50%) translateY(120px);
  background:#111827;color:#fff;padding:13px 18px;border-radius:999px;
  opacity:0;transition:.25s;z-index:9999;box-shadow:0 16px 30px rgba(0,0,0,.18)
}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
.small-note{color:#9ca3af;font-size:.85rem;margin-top:8px}
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>🌸 Cute Schedule Planner</h1>
    <p>AI가 할 일을 예쁘게 정리하고, 구글 캘린더에도 동기화해요</p>
  </header>

  <div class="setup-box" id="setupBox">
    ⚙️ <b>구글 캘린더 연동 설정</b> — 코드 상단의 <code>GOOGLE_CLIENT_ID</code>를 본인의 OAuth 클라이언트 ID로 바꾸고,
    승인된 JavaScript 원본에 이 페이지 주소를 등록하세요.
  </div>

  <div class="sync-row">
    <button class="btn primary" id="googleSigninBtn" onclick="googleSignIn()">🔗 구글 캘린더 연동</button>
    <div class="sync-status" id="syncStatus"><div class="dot"></div><span>구글 캘린더 연결됨</span></div>
    <div class="badge">🤖 AI 자동 스케줄링 ON</div>
  </div>

  <div class="stats">
    <div class="card stat"><div class="stat-number" id="totalTodos">0</div><div class="stat-label">총 할일</div></div>
    <div class="card stat"><div class="stat-number" id="completedTodos">0</div><div class="stat-label">완료</div></div>
    <div class="card stat"><div class="stat-number" id="totalSchedules">0</div><div class="stat-label">스케줄</div></div>
    <div class="card stat"><div class="stat-number" id="upcomingSchedules">0</div><div class="stat-label">예정</div></div>
  </div>

  <div class="grid">
    <div class="card">
      <h2>✅ 할일 목록</h2>
      <div class="row">
        <input type="text" id="todoInput" placeholder="예: 내일 오전에 보고서 작성, 30분 운동..." />
      </div>
      <div class="row">
        <button class="btn primary" style="flex:1" onclick="addTodo()">🤖 AI로 추가 (자동 스케줄링)</button>
        <button class="btn secondary" onclick="addTodo(true)">할일만 추가</button>
      </div>
      <div class="filters">
        <button class="filter-btn active" onclick="filterTodos('all', this)">모두</button>
        <button class="filter-btn" onclick="filterTodos('active', this)">진행중</button>
        <button class="filter-btn" onclick="filterTodos('completed', this)">완료</button>
      </div>
      <div class="list" id="todoList"><div class="empty">할일을 추가해보세요 💭</div></div>
    </div>

    <div class="card">
      <h2>🗓️ 스케줄</h2>
      <div class="row">
        <input type="text" id="scheduleTitle" placeholder="스케줄 제목" />
        <select id="schedulePriority">
          <option value="low">🟢 낮음</option>
          <option value="medium" selected>🟡 중간</option>
          <option value="high">🔴 높음</option>
        </select>
      </div>
      <div class="row">
        <input type="datetime-local" id="scheduleTime" />
        <button class="btn success" onclick="addSchedule()">추가</button>
      </div>
      <div class="filters">
        <button class="filter-btn active" onclick="filterSchedules('all', this)">모두</button>
        <button class="filter-btn" onclick="filterSchedules('today', this)">오늘</button>
        <button class="filter-btn" onclick="filterSchedules('upcoming', this)">예정</button>
        <button class="filter-btn" onclick="filterSchedules('overdue', this)">지난</button>
      </div>
      <div class="list" id="scheduleList"><div class="empty">스케줄이 여기에 나타나요 💫</div></div>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const GOOGLE_CLIENT_ID = "YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com";
const GOOGLE_SCOPE = "https://www.googleapis.com/auth/calendar.events";

let tokenClient = null;
let googleAccessToken = null;
let todos = JSON.parse(localStorage.getItem('cute_todos_v1') || '[]');
let schedules = JSON.parse(localStorage.getItem('cute_schedules_v1') || '[]');
let todoFilter = 'all';
let scheduleFilter = 'all';
let toastTimer = null;

function showToast(msg){
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(()=>t.classList.remove('show'), 3200);
}

function saveTodos(){ localStorage.setItem('cute_todos_v1', JSON.stringify(todos)); }
function saveSchedules(){ localStorage.setItem('cute_schedules_v1', JSON.stringify(schedules)); }

function escapeHtml(t){
  const d = document.createElement('div');
  d.textContent = t;
  return d.innerHTML;
}

function toLocalInput(date){
  const off = date.getTimezoneOffset();
  return new Date(date.getTime() - off*60000).toISOString().slice(0,16);
}

function formatDateTime(ds){
  const d = new Date(ds);
  const now = new Date();
  const tomorrow = new Date(now); tomorrow.setDate(now.getDate()+1);
  let label = d.toLocaleDateString('ko-KR', {month:'long', day:'numeric', weekday:'short'});
  if (d.toDateString() === now.toDateString()) label = '오늘';
  else if (d.toDateString() === tomorrow.toDateString()) label = '내일';
  return `${label} ${d.toLocaleTimeString('ko-KR', {hour:'2-digit', minute:'2-digit'})}`;
}

function getPriorityLabel(p){
  return {high:'🔴 높음', medium:'🟡 중간', low:'🟢 낮음'}[p] || '🟡 중간';
}

function analyzeTodo(text){
  const t = text.toLowerCase();
  let priority = 'medium';
  let durationMin = 60;
  let preferred = 'any';
  let dayOffset = 1;

  if (/긴급|급|asap|중요|당장|오늘까지|마감|deadline/.test(t)) priority = 'high';
  else if (/언젠가|나중에|여유|천천히|틈나면/.test(t)) priority = 'low';

  const hMatch = t.match(/(\d+)\s*시간/);
  const mMatch = t.match(/(\d+)\s*분/);
  if (hMatch) durationMin = parseInt(hMatch[1],10) * 60 + (mMatch ? parseInt(mMatch[1],10) : 0);
  else if (mMatch) durationMin = parseInt(mMatch[1],10);
  else if (/운동|산책|전화|메일|이메일|확인|점검/.test(t)) durationMin = 30;
  else if (/회의|미팅|공부|작업|보고서|프로젝트|문서|코딩|개발/.test(t)) durationMin = 90;
  else if (/청소|정리|장보기|쇼핑/.test(t)) durationMin = 45;

  if (/아침|오전|모닝|기상/.test(t)) preferred = 'morning';
  else if (/점심|런치/.test(t)) preferred = 'noon';
  else if (/오후/.test(t)) preferred = 'afternoon';
  else if (/저녁|밤|퇴근후|야간|운동/.test(t)) preferred = 'evening';
  else if (/공부|집중|보고서|작업|코딩|개발/.test(t)) preferred = 'morning';

  if (/오늘/.test(t)) dayOffset = 0;
  else if (/내일/.test(t)) dayOffset = 1;
  else if (/모레/.test(t)) dayOffset = 2;
  else if (/이번\s*주|금주/.test(t)) dayOffset = 1;
  else if (/다음\s*주|담주/.test(t)) dayOffset = 7;
  else dayOffset = priority === 'high' ? 0 : (priority === 'low' ? 3 : 1);

  return {priority, durationMin, preferred, dayOffset};
}

function preferredStartHour(pref){
  switch(pref){
    case 'morning': return 9;
    case 'noon': return 12;
    case 'afternoon': return 14;
    case 'evening': return 19;
    default: return 10;
  }
}

function findFreeSlot(analysis){
  const dur = analysis.durationMin;
  const startHourPref = preferredStartHour(analysis.preferred);
  const now = new Date();

  for (let d = analysis.dayOffset; d < analysis.dayOffset + 14; d++){
    const day = new Date(now);
    day.setDate(now.getDate() + d);
    day.setHours(0,0,0,0);

    const hours = [];
    for (let h = startHourPref; h <= 22; h += 0.5) hours.push(h);
    for (let h = 8; h < startHourPref; h += 0.5) hours.push(h);

    for (const h of hours){
      const slotStart = new Date(day);
      slotStart.setHours(Math.floor(h), (h % 1) * 60, 0, 0);
      const slotEnd = new Date(slotStart.getTime() + dur * 60000);
      if (slotStart < now) continue;
      if (slotEnd.getHours() > 22 || (slotEnd.getHours() === 22 && slotEnd.getMinutes() > 0)) continue;

      const conflict = schedules.some(s => {
        const es = new Date(s.time);
        const ee = new Date(es.getTime() + (s.durationMin || 60) * 60000);
        return slotStart < ee && slotEnd > es;
      });
      if (!conflict) return slotStart;
    }
  }

  const fb = new Date(now);
  fb.setDate(now.getDate() + 1);
  fb.setHours(startHourPref,0,0,0);
  return fb;
}

function initTokenClient(){
  if (!window.google || !google.accounts || !google.accounts.oauth2) return false;
  tokenClient = google.accounts.oauth2.initTokenClient({
    client_id: GOOGLE_CLIENT_ID,
    scope: GOOGLE_SCOPE,
    callback: async (resp) => {
      if (resp && resp.access_token){
        googleAccessToken = resp.access_token;
        document.getElementById('syncStatus').classList.add('active');
        document.getElementById('googleSigninBtn').textContent = '✅ 연동됨 (다시 연결)';
        showToast('✅ 구글 캘린더 연동 완료!');
        await exportAllToGoogle();
      }
    }
  });
  return true;
}

function googleSignIn(){
  if (GOOGLE_CLIENT_ID.startsWith('YOUR_')){
    alert('먼저 GOOGLE_CLIENT_ID를 본인의 OAuth 클라이언트 ID로 바꿔주세요.');
    return;
  }
  if (!tokenClient){
    if (!initTokenClient()) {
      alert('구글 인증 라이브러리 로딩 중입니다. 잠시 후 다시 시도해주세요.');
      return;
    }
  }
  tokenClient.requestAccessToken({prompt: googleAccessToken ? '' : 'consent'});
}

async function syncScheduleToGoogle(schedule){
  if (!googleAccessToken) return;
  const start = new Date(schedule.time);
  const end = new Date(start.getTime() + (schedule.durationMin || 60) * 60000);
  const event = {
    summary: schedule.title,
    description: (schedule.description || '') + '\n\n[Cute Schedule Planner]',
    start: {dateTime: start.toISOString(), timeZone:'Asia/Seoul'},
    end: {dateTime: end.toISOString(), timeZone:'Asia/Seoul'}
  };

  try{
    const url = schedule.googleEventId
      ? `https://www.googleapis.com/calendar/v3/calendars/primary/events/${schedule.googleEventId}`
      : `https://www.googleapis.com/calendar/v3/calendars/primary/events`;

    const res = await fetch(url, {
      method: schedule.googleEventId ? 'PUT' : 'POST',
      headers: {'Authorization':'Bearer ' + googleAccessToken, 'Content-Type':'application/json'},
      body: JSON.stringify(event)
    });

    if (res.ok){
      const data = await res.json();
      schedule.googleEventId = data.id;
      saveSchedules();
      renderSchedules();
    } else if (res.status === 401){
      googleAccessToken = null;
      document.getElementById('syncStatus').classList.remove('active');
      showToast('⚠️ 구글 인증이 만료됐어요. 다시 연동해주세요');
    }
  }catch(e){
    console.error('구글 동기화 실패', e);
  }
}

async function deleteFromGoogle(eventId){
  if (!googleAccessToken || !eventId) return;
  try{
    await fetch(`https://www.googleapis.com/calendar/v3/calendars/primary/events/${eventId}`,{
      method:'DELETE',
      headers:{'Authorization':'Bearer ' + googleAccessToken}
    });
  }catch(e){ console.error(e); }
}

async function exportAllToGoogle(){
  for (const s of schedules){
    if (!s.googleEventId) await syncScheduleToGoogle(s);
  }
}

function addTodo(skipAI){
  const input = document.getElementById('todoInput');
  const text = input.value.trim();
  if (!text){ alert('할일을 입력해주세요 😊'); return; }

  const todo = { id: Date.now(), text, completed:false, createdAt:new Date().toISOString(), scheduleId:null, suggestion:null };

  if (!skipAI){
    const a = analyzeTodo(text);
    const slot = findFreeSlot(a);
    const sched = {
      id: Date.now() + 1,
      title: text,
      time: toLocalInput(slot),
      priority: a.priority,
      durationMin: a.durationMin,
      description: `AI 자동 배치 (예상 소요 ${a.durationMin}분)`,
      completed:false,
      aiGenerated:true,
      googleEventId:null
    };
    schedules.push(sched);
    todo.scheduleId = sched.id;
    todo.suggestion = `🤖 ${formatDateTime(sched.time)} · ${a.durationMin}분 · ${getPriorityLabel(a.priority)}`;
    saveSchedules();
    if (googleAccessToken) syncScheduleToGoogle(sched);
    showToast(`🤖 "${text}" → ${formatDateTime(sched.time)}에 자동 배치했어요!`);
  }

  todos.push(todo);
  saveTodos();
  input.value = '';
  renderTodos();
  renderSchedules();
  updateStats();
}

function deleteTodo(id){
  const todo = todos.find(t => t.id === id);
  todos = todos.filter(t => t.id !== id);
  if (todo && todo.scheduleId){
    const s = schedules.find(x => x.id === todo.scheduleId);
    if (s && s.googleEventId) deleteFromGoogle(s.googleEventId);
    schedules = schedules.filter(x => x.id !== todo.scheduleId);
    saveSchedules();
    renderSchedules();
  }
  saveTodos();
  renderTodos();
  updateStats();
}

function toggleTodo(id){
  const t = todos.find(x => x.id === id);
  if (t){
    t.completed = !t.completed;
    if (t.scheduleId){
      const s = schedules.find(x => x.id === t.scheduleId);
      if (s){ s.completed = t.completed; saveSchedules(); renderSchedules(); }
    }
    saveTodos();
    renderTodos();
    updateStats();
  }
}

function addSchedule(){
  const title = document.getElementById('scheduleTitle');
  const time = document.getElementById('scheduleTime');
  const priority = document.getElementById('schedulePriority');
  if (!title.value.trim() || !time.value){ alert('제목과 시간을 입력해주세요 ⏰'); return; }

  const s = {
    id: Date.now(),
    title: title.value.trim(),
    time: time.value,
    priority: priority.value,
    durationMin: 60,
    description: '',
    completed:false,
    aiGenerated:false,
    googleEventId:null
  };
  schedules.push(s);
  saveSchedules();
  if (googleAccessToken) syncScheduleToGoogle(s);
  title.value = '';
  time.value = '';
  priority.value = 'medium';
  renderSchedules();
  updateStats();
}

function deleteSchedule(id){
  const s = schedules.find(x => x.id === id);
  if (s && s.googleEventId) deleteFromGoogle(s.googleEventId);
  schedules = schedules.filter(x => x.id !== id);
  todos.forEach(t => { if (t.scheduleId === id){ t.scheduleId = null; t.suggestion = null; }});
  saveSchedules();
  saveTodos();
  renderSchedules();
  renderTodos();
  updateStats();
}

function toggleSchedule(id){
  const s = schedules.find(x => x.id === id);
  if (s){
    s.completed = !s.completed;
    saveSchedules();
    renderSchedules();
    updateStats();
  }
}

function filterTodos(f, btnElement){
  todoFilter = f;
  document.querySelectorAll('.filters')[0].querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  if(btnElement) btnElement.classList.add('active');
  renderTodos();
}

function filterSchedules(f, btnElement){
  scheduleFilter = f;
  document.querySelectorAll('.filters')[1].querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  if(btnElement) btnElement.classList.add('active');
  renderSchedules();
}

function renderTodos(){
  const el = document.getElementById('todoList');
  let list = todos;
  if (todoFilter === 'active') list = todos.filter(t => !t.completed);
  else if (todoFilter === 'completed') list = todos.filter(t => t.completed);

  if (!list.length){ el.innerHTML = '<div class="empty">할일이 없습니다 🎉</div>'; return; }

  el.innerHTML = list.map(t => `
    <div class="item ${t.completed ? 'completed' : ''}">
      <div style="flex:1">
        <div class="title">${escapeHtml(t.text)}</div>
        ${t.suggestion && !t.completed ? `<div class="meta">${escapeHtml(t.suggestion)}</div>` : ''}
      </div>
      <div class="actions">
        <button class="btn ${t.completed ? 'secondary' : 'success'}" onclick="toggleTodo(${t.id})">${t.completed ? '↩️' : '✓'}</button>
        <button class="btn danger" onclick="deleteTodo(${t.id})">🗑️</button>
      </div>
    </div>
  `).join('');
}

function renderSchedules(){
  const el = document.getElementById('scheduleList');
  let list = schedules;
  const now = new Date();

  if (scheduleFilter === 'upcoming') list = schedules.filter(s => !s.completed && new Date(s.time) > now);
  else if (scheduleFilter === 'today') list = schedules.filter(s => new Date(s.time).toDateString() === now.toDateString());
  else if (scheduleFilter === 'overdue') list = schedules.filter(s => !s.completed && new Date(s.time) < now);

  if (!list.length){ el.innerHTML = '<div class="empty">스케줄이 없습니다 💫</div>'; return; }

  list = [...list].sort((a,b) => new Date(a.time) - new Date(b.time));
  el.innerHTML = list.map(s => {
    const d = new Date(s.time);
    const overdue = !s.completed && d < now;
    const today = d.toDateString() === now.toDateString();
    return `
      <div class="item ${s.completed ? 'completed' : ''} ${overdue ? 'overdue' : (today ? 'today' : '')}">
        <div style="flex:1">
          <div class="tags">
            <span class="tag ${s.priority}">${getPriorityLabel(s.priority)}</span>
            ${s.aiGenerated ? '<span class="tag ai">🤖 AI</span>' : ''}
            ${s.googleEventId ? '<span class="tag gcal">📅 구글동기화</span>' : ''}
          </div>
          <div class="title" style="margin-top:8px">${escapeHtml(s.title)}</div>
          ${s.description ? `<div class="meta">${escapeHtml(s.description)}</div>` : ''}
          <div class="meta">⏰ ${formatDateTime(s.time)} (${s.durationMin || 60}분)</div>
        </div>
        <div class="actions">
          <button class="btn ${s.completed ? 'secondary' : 'success'}" onclick="toggleSchedule(${s.id})">${s.completed ? '↩️' : '✓'}</button>
          <button class="btn danger" onclick="deleteSchedule(${s.id})">🗑️</button>
        </div>
      </div>
    `;
  }).join('');
}

function updateStats(){
  document.getElementById('totalTodos').textContent = todos.length;
  document.getElementById('completedTodos').textContent = todos.filter(t => t.completed).length;
  document.getElementById('totalSchedules').textContent = schedules.length;
  document.getElementById('upcomingSchedules').textContent = schedules.filter(s => !s.completed && new Date(s.time) > new Date()).length;
}

window.addEventListener('load', () => {
  renderTodos();
  renderSchedules();
  updateStats();
  
  const tryInit = setInterval(() => {
    if (initTokenClient()) clearInterval(tryInit);
  }, 300);
});

document.getElementById('todoInput').addEventListener('keypress', e => { if (e.key === 'Enter') addTodo(); });
document.getElementById('scheduleTitle').addEventListener('keypress', e => { if (e.key === 'Enter' && document.getElementById('scheduleTime').value) addSchedule(); });
</script>
</body>
</html>
