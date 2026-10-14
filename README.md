# AETHER-AI
**AETHER AI** is a single-file, serverless web chatbot powered by Google's Gemini API. Styled in a purple-and-white glassmorphic interface, it features Markdown rendering, local memory, voice interaction, dark mode, prompt chips, and Hinglish support—running instantly inside any browser.
<!DOCTYPE html>
<html lang="en">
<head>
<!-- ===== STEP 1: PASTE YOUR API KEY BETWEEN THE QUOTES ON LINE 6 ===== -->
<script>
const API_KEY = "AQ.Ab8RN6I6Afl5grgPYfSbcxNth4Yh-DgHPKusvoXa1hROrY-oLQ";
const MODEL = "gemini-3.5-flash-lite";
</script>
<script>
const SYSTEM_PROMPT = `
You are NEXUS Ai, a helpful, thoughtful and transparent AI assistant.
Your personality is friendly, intelligent, precise and future-focused.
Help with conversation, programming, mathematics, studying, planning,
summarization and creative work. Explain complex topics clearly.
Never invent tool results, browsing, model routing or capabilities.
Arithmetic is handled by the app's local calculator when appropriate.
Browser-stored chats and notes are not cloud memory.
Do not claim access to the internet, files or external systems unless
that capability is actually available.
If the user writes in Hindi or Hinglish, reply in the same language.
Use readable Markdown, code blocks and structured lists when useful.
`;
</script>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#17102c">
<title>NEXUS Ai — Adaptive Intelligence</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<style>
/* =========================================================
   1. DESIGN SYSTEM
   ========================================================= */
:root {
  color-scheme: dark;
  --bg: #100b1e;
  --sidebar: #171027;
  --panel: rgba(32, 23, 53, .88);
  --panel2: #211737;
  --text: #f8f5ff;
  --muted: #aaa0c4;
  --line: rgba(192, 164, 255, .16);
  --accent: #b99aff;
  --accent2: #8b5cf6;
  --glow: rgba(167, 119, 255, .23);
  --user: #7046cb;
  --bot: #211936;
  --danger: #ff788d;
  --success: #72e6bd;
  --shadow: 0 18px 55px rgba(0, 0, 0, .2);
}
body.light {
  color-scheme: light;
  --bg: #f5f2fc;
  --sidebar: #ffffff;
  --panel: rgba(255,255,255,.92);
  --panel2: #eee8fb;
  --text: #24163e;
  --muted: #786c90;
  --line: rgba(101, 67, 155, .17);
  --accent: #7542cf;
  --accent2: #8b5cf6;
  --glow: rgba(139, 92, 246, .12);
  --user: #8051dc;
  --bot: #ffffff;
  --danger: #d93052;
  --success: #158568;
  --shadow: 0 18px 55px rgba(51, 34, 86, .08);
}
* { box-sizing: border-box; }
html, body { margin: 0; min-height: 100%; }
body {
  background:
    radial-gradient(ellipse at 55% 0%, var(--glow), transparent 42%),
    var(--bg);
  color: var(--text);
  font-family: "Poppins", sans-serif;
  font-size: 13px;
  transition: background .25s, color .25s;
}
button, textarea, input { font: inherit; }
button { cursor: pointer; }
button:focus-visible, textarea:focus-visible, input:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 3px;
}
button:disabled { cursor: not-allowed; opacity: .5; }
.app { min-height: 100vh; display: flex; }
.sidebar {
  width: 255px; flex-shrink: 0; padding: 22px 16px;
  background: var(--sidebar); border-right: 1px solid var(--line);
  display: flex; flex-direction: column; gap: 18px;
}
.brand { display: flex; align-items: center; gap: 11px; padding: 2px 6px 12px; }
.logo {
  width: 42px; height: 42px; display: grid; place-items: center;
  border-radius: 14px; color: white; font-size: 23px;
  background: linear-gradient(140deg, #c4a5ff, #7c3aed 70%, #5026a5);
  box-shadow: 0 0 27px var(--glow);
}
.brand-name { font-size: 18px; font-weight: 800; letter-spacing: -.6px; }
.brand-name span { color: var(--accent); }
.brand-sub { font-size: 9px; letter-spacing: 1.5px; color: var(--muted); }
.eyebrow { font-size: 10px; letter-spacing: 1.6px; color: var(--muted); font-weight: 600; }
.btn {
  border: 1px solid var(--line); color: var(--text);
  background: var(--panel); border-radius: 11px; padding: 10px 12px;
  transition: transform .15s, border-color .15s, background .15s;
}
.btn:hover { border-color: var(--accent); transform: translateY(-1px); }
.btn.primary {
  color: white; border: 0;
  background: linear-gradient(115deg, #9b6df5, #7241d0);
  box-shadow: 0 6px 22px var(--glow);
}
.sidebar .btn { text-align: left; width: 100%; }
.nav-item { display: flex; align-items: center; gap: 10px; }
.nav-item span { font-size: 16px; width: 21px; text-align: center; }
.side-card {
  border: 1px solid var(--line); border-radius: 14px; padding: 14px;
  background: var(--panel);
}
.side-card p { color: var(--muted); font-size: 11px; line-height: 1.8; margin: 7px 0 0; }
.status-dot {
  width: 7px; height: 7px; display: inline-block; border-radius: 50%;
  background: var(--success); margin-right: 6px;
}
.sidebar-bottom { margin-top: auto; display: grid; gap: 12px; }
.signature { font-size: 10px; color: var(--muted); line-height: 1.9; padding: 0 6px; }
.signature strong { color: var(--accent); letter-spacing: .8px; }
.main { min-width: 0; flex: 1; display: flex; flex-direction: column; min-height: 100vh; }
.topbar {
  min-height: 74px; padding: 13px 24px; border-bottom: 1px solid var(--line);
  display: flex; align-items: center; justify-content: space-between; gap: 12px;
  background: var(--panel); backdrop-filter: blur(18px);
}
.top-title { font-size: 15px; font-weight: 700; }
.top-sub { color: var(--muted); font-size: 10px; margin-top: 3px; }
.top-actions { display: flex; gap: 7px; align-items: center; }
.icon-btn {
  min-width: 37px; height: 37px; padding: 0 10px;
  border: 1px solid var(--line); border-radius: 11px;
  color: var(--text); background: var(--panel);
}
.icon-btn:hover { border-color: var(--accent); }
.content {
  width: min(100%, 1030px); margin: 0 auto; padding: 25px 25px 15px;
  display: flex; flex-direction: column; flex: 1; min-height: 0;
}
.hero {
  position: relative; overflow: hidden; text-align: center; padding: 14px 12px 20px;
}
.core-wrap { height: 112px; display: grid; place-items: center; }
.core {
  position: relative; width: 76px; height: 76px; border-radius: 50%;
  background: radial-gradient(circle at 35% 28%, #fff, #d9bdff 9%, #9b62ff 34%, #5323a6 64%, #1b1038 77%);
  box-shadow: 0 0 22px #965aff88, 0 0 55px #8144f655, inset -7px -7px 17px #1b1038;
  animation: pulse 3.5s ease-in-out infinite;
}
.core::before, .core::after {
  content: ""; position: absolute; inset: -12px; border: 1px solid #a980ff88;
  border-radius: 50%; transform: rotateX(65deg); animation: orbit 9s linear infinite;
}
.core::after { inset: -20px; transform: rotateX(65deg) rotate(60deg); animation-duration: 13s; }
.core-ring {
  position: absolute; inset: -5px; border: 1px dashed #cbb0ff88;
  border-radius: 50%; animation: orbit 18s linear infinite reverse;
}
.hero h1 { font-size: clamp(23px, 3vw, 34px); letter-spacing: -1.1px; margin: 4px 0 8px; }
.gradient-text {
  background: linear-gradient(100deg, #fff, #d1b6ff 42%, #a17bff);
  -webkit-background-clip: text; background-clip: text; color: transparent;
}
body.light .gradient-text { background-image: linear-gradient(100deg, #4d267e, #8a4ee4); }
.hero p { color: var(--muted); max-width: 500px; line-height: 1.8; margin: 0 auto; font-size: 12px; }
.chips { display: flex; justify-content: center; flex-wrap: wrap; gap: 8px; margin-top: 18px; }
.chip {
  border: 1px solid var(--line); border-radius: 22px; background: var(--panel);
  color: var(--text); padding: 9px 13px; font-size: 11px; max-width: 100%;
}
.chip:hover { border-color: var(--accent); background: var(--glow); }
.chat { flex: 1; display: flex; flex-direction: column; gap: 19px; padding: 8px 0 22px; }
.message { display: flex; align-items: flex-start; gap: 10px; max-width: 100%; }
.message.user { flex-direction: row-reverse; }
.avatar {
  flex: 0 0 32px; width: 32px; height: 32px; display: grid; place-items: center;
  border: 1px solid var(--line); border-radius: 12px; background: var(--panel2); font-size: 16px;
}
.message.user .avatar { background: linear-gradient(140deg, #a889ff, #6e3fc5); }
.bubble-wrap { max-width: min(80%, 740px); min-width: 0; }
.message.user .bubble-wrap { display: flex; flex-direction: column; align-items: flex-end; }
.msg-meta { color: var(--muted); font-size: 10px; margin: 1px 4px 6px; }
.bubble {
  overflow-wrap: anywhere; line-height: 1.85; font-size: 12px;
  border: 1px solid var(--line); border-radius: 4px 16px 16px 16px;
  padding: 13px 15px; background: var(--bot); box-shadow: var(--shadow);
}
.message.user .bubble { background: var(--user); color: white; border-color: transparent; border-radius: 16px 4px 16px 16px; }
.bubble p { margin: 0 0 10px; }
.bubble p:last-child { margin-bottom: 0; }
.bubble ul, .bubble ol { padding-left: 22px; margin: 7px 0; }
.bubble li { margin: 4px 0; }
.bubble pre {
  background: #100b1e; color: #f1e9ff; padding: 13px; overflow-x: auto;
  border-radius: 10px; border: 1px solid #ffffff18; margin: 10px 0;
}
.bubble code { font-family: Consolas, monospace; font-size: 11px; }
.bubble :not(pre) > code { padding: 2px 5px; border-radius: 5px; background: var(--glow); }
.bubble pre code { white-space: pre; }
.bubble blockquote { border-left: 3px solid var(--accent); margin: 9px 0; padding-left: 10px; color: var(--muted); }
.msg-tools { display: flex; gap: 5px; margin: 6px 0 0 3px; }
.msg-tools button {
  border: 1px solid var(--line); border-radius: 7px; background: var(--panel);
  color: var(--muted); font-size: 10px; padding: 4px 8px;
}
.msg-tools button:hover { color: var(--accent); }
.typing { display: flex; gap: 5px; padding: 4px 0; }
.typing span { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); animation: dot 1s infinite; }
.typing span:nth-child(2) { animation-delay: .15s; }
.typing span:nth-child(3) { animation-delay: .3s; }
.composer-area { position: sticky; bottom: 0; padding: 10px 0 5px; background: var(--bg); }
.composer {
  border: 1px solid var(--line); background: var(--panel); border-radius: 18px;
  box-shadow: var(--shadow); padding: 12px; transition: border-color .2s;
}
.composer:focus-within { border-color: var(--accent); }
#prompt {
  display: block; width: 100%; min-height: 48px; max-height: 180px; resize: vertical;
  border: 0; outline: 0; background: transparent; color: var(--text);
  line-height: 1.7; padding: 3px 3px 9px;
}
#prompt::placeholder { color: var(--muted); }
.composer-row { display: flex; justify-content: space-between; align-items: center; gap: 10px; }
.composer-hint { color: var(--muted); font-size: 9px; }
.composer-buttons { display: flex; align-items: center; gap: 7px; }
.send-btn {
  min-width: 75px; border: 0; border-radius: 10px; color: white; padding: 10px 14px;
  background: linear-gradient(115deg, #9b6df5, #7241d0); font-weight: 600;
}
.disclaimer { text-align: center; color: var(--muted); font-size: 9px; margin: 10px 0 0; line-height: 1.7; }
.dashboard { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 10px; margin: 7px 0 19px; }
.metric { padding: 11px 13px; background: var(--panel); border: 1px solid var(--line); border-radius: 12px; min-width: 0; }
.metric-label { font-size: 9px; color: var(--muted); letter-spacing: .6px; }
.metric-value { font-size: 12px; font-weight: 600; margin-top: 5px; overflow-wrap: anywhere; }
.activity { font-size: 10px; color: var(--muted); margin: 0 0 12px; min-height: 15px; }
.overlay {
  display: none; position: fixed; inset: 0; z-index: 20; background: #090612a8;
  align-items: center; justify-content: center; padding: 16px;
}
.overlay.open { display: flex; }
.modal {
  width: min(100%, 520px); max-height: 85vh; overflow-y: auto;
  background: var(--sidebar); border: 1px solid var(--line); border-radius: 18px;
  padding: 20px; box-shadow: var(--shadow);
}
.modal-head { display: flex; justify-content: space-between; align-items: center; gap: 10px; margin-bottom: 14px; }
.modal h2 { font-size: 17px; margin: 0; }
.modal p, .modal label { color: var(--muted); font-size: 11px; line-height: 1.8; }
.modal input, .modal textarea, .modal select {
  width: 100%; padding: 10px; border-radius: 9px; border: 1px solid var(--line);
  background: var(--bg); color: var(--text); margin: 6px 0 12px;
}
.modal textarea { min-height: 90px; resize: vertical; }
.note-item { border: 1px solid var(--line); border-radius: 10px; padding: 10px; margin: 8px 0; overflow-wrap: anywhere; }
.note-actions { display: flex; gap: 6px; margin-top: 8px; }
.note-actions button { font-size: 10px; }
.toast {
  position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
  background: var(--panel2); color: var(--text); border: 1px solid var(--line);
  border-radius: 10px; padding: 11px 16px; z-index: 50; box-shadow: var(--shadow);
  max-width: calc(100% - 30px); text-align: center; font-size: 11px;
}
.hidden { display: none !important; }
@keyframes pulse { 0%,100% { transform: scale(.96); filter: brightness(.95); } 50% { transform: scale(1.05); filter: brightness(1.18); } }
@keyframes orbit { to { transform: rotate(360deg) rotateX(65deg); } }
@keyframes dot { 0%,60%,100% { transform: translateY(0); opacity: .45; } 30% { transform: translateY(-5px); opacity: 1; } }
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: .01ms !important; animation-iteration-count: 1 !important; scroll-behavior: auto !important; transition-duration: .01ms !important; }
}
@media (max-width: 800px) {
  .sidebar { width: 70px; padding: 18px 10px; align-items: center; }
  .brand { padding: 0; }
  .brand-copy, .side-label, .side-card, .signature, .sidebar .btn .nav-text { display: none; }
  .sidebar .btn { text-align: center; padding: 11px 5px; }
  .nav-item { justify-content: center; }
  .sidebar-bottom { width: 100%; }
  .topbar { padding: 12px 14px; }
  .content { padding: 16px 14px 10px; }
  .dashboard { grid-template-columns: repeat(3, minmax(0, 1fr)); }
}
@media (max-width: 520px) {
  .sidebar { width: 54px; padding: 13px 6px; }
  .logo { width: 35px; height: 35px; font-size: 19px; border-radius: 11px; }
  .brand { margin-bottom: 5px; }
  .topbar { min-height: 63px; gap: 6px; }
  .top-title { font-size: 12px; }
  .top-sub { font-size: 9px; }
  .top-actions { gap: 4px; }
  .icon-btn { min-width: 32px; height: 33px; padding: 0 7px; }
  .hero { padding: 4px 3px 14px; }
  .core-wrap { height: 93px; }
  .core { width: 61px; height: 61px; }
  .hero h1 { font-size: 23px; }
  .chips { gap: 6px; }
  .chip { padding: 8px 10px; font-size: 10px; }
  .bubble-wrap { max-width: 86%; }
  .bubble { padding: 10px 12px; }
  .dashboard { gap: 5px; }
  .metric { padding: 8px 7px; }
  .metric-label { font-size: 8px; }
  .metric-value { font-size: 10px; }
  .composer-hint { display: none; }
}
</style>
</head>
<body>
<div class="app">
  <aside class="sidebar" aria-label="Main navigation">
    <div class="brand">
      <div class="logo" aria-label="NEXUS logo">✧</div>
      <div class="brand-copy">
        <div class="brand-name">NEXUS <span>Ai</span></div>
        <div class="brand-sub">ADAPTIVE INTELLIGENCE</div>
      </div>
    </div>

    <button class="btn primary nav-item" id="newChatBtn" title="Start a new chat">
      <span>＋</span><span class="nav-text">New conversation</span>
    </button>

    <div class="side-label eyebrow">WORKSPACE</div>
    <button class="btn nav-item" id="calculatorBtn"><span>⌗</span><span class="nav-text">Calculator</span></button>
    <button class="btn nav-item" id="memoryBtn"><span>◇</span><span class="nav-text">Saved notes</span></button>
    <button class="btn nav-item" id="exportBtn"><span>⇩</span><span class="nav-text">Export conversation</span></button>

    <div class="side-card">
      <div class="eyebrow">SYSTEM STATUS</div>
      <p><span class="status-dot"></span><span id="connectionLabel">Ready to connect</span></p>
      <p>Provider: Google Gemini</p>
      <p>Processing: Remote API</p>
      <p>Local tools: Calculator</p>
    </div>

    <div class="sidebar-bottom">
      <div class="signature">A PERSONAL INTELLIGENCE SPACE<br><strong>DIVYANSH AGGARWAL</strong></div>
      <button class="btn nav-item" id="settingsBtn"><span>⚙</span><span class="nav-text">Setup & privacy</span></button>
    </div>
  </aside>

  <main class="main">
    <header class="topbar">
      <div>
        <div class="top-title">NEXUS Ai <span style="color:var(--accent)">/</span> Command Center</div>
        <div class="top-sub">One interface. Adaptive intelligence. Unlimited possibilities.</div>
      </div>
      <div class="top-actions">
        <button class="icon-btn" id="themeBtn" aria-label="Toggle light or dark mode" title="Toggle theme">☼</button>
        <button class="icon-btn" id="clearBtn" aria-label="Clear chat" title="Clear chat">⌫</button>
      </div>
    </header>

    <section class="content">
      <div class="hero" id="welcome">
        <div class="core-wrap"><div class="core" aria-label="Animated AI core"><div class="core-ring"></div></div></div>
        <div class="eyebrow">YOUR PERSONAL AI COMPANION</div>
        <h1>Think beyond. <span class="gradient-text">Build what's next.</span></h1>
        <p>Welcome to NEXUS Ai — a space to explore ideas, solve problems, learn faster, and create with intelligence.</p>
        <div class="chips" id="starterChips">
          <button class="chip">Help me build a futuristic AI project</button>
          <button class="chip">Explain a difficult topic simply</button>
          <button class="chip">Create a practical study plan</button>
        </div>
      </div>

      <div class="dashboard" aria-label="Performance dashboard">
        <div class="metric"><div class="metric-label">SELECTED MODEL</div><div class="metric-value" id="metricModel">Gemini · configured</div></div>
        <div class="metric"><div class="metric-label">LAST RESPONSE</div><div class="metric-value" id="metricLatency">Not measured</div></div>
        <div class="metric"><div class="metric-label">TASK ROUTE</div><div class="metric-value" id="metricRoute">Awaiting request</div></div>
      </div>
      <div class="activity" id="activity" role="status" aria-live="polite">Ready. Your messages are sent to the configured Gemini API when you chat.</div>

      <section class="chat" id="chat" aria-label="Conversation" aria-live="polite"></section>

      <div class="composer-area">
        <div class="composer">
          <textarea id="prompt" rows="2" maxlength="12000" aria-label="Your message"
            placeholder="Ask NEXUS anything… (Enter to send, Shift + Enter for a new line)"></textarea>
          <div class="composer-row">
            <div class="composer-hint">✧ Adaptive routing · Browser-local history</div>
            <div class="composer-buttons">
              <button class="icon-btn hidden" id="micBtn" title="Speak your message" aria-label="Voice input">🎙</button>
              <button class="icon-btn" id="stopBtn" title="Stop waiting" aria-label="Stop generation" disabled>■</button>
              <button class="send-btn" id="sendBtn">Send ↗</button>
            </div>
          </div>
        </div>
        <div class="disclaimer">AI can make mistakes. Verify important information. Do not enter passwords or confidential information.</div>
      </div>
    </section>
  </main>
</div>

<div class="overlay" id="modalOverlay" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
  <div class="modal">
    <div class="modal-head">
      <h2 id="modalTitle">NEXUS workspace</h2>
      <button class="icon-btn" id="closeModalBtn" aria-label="Close dialog">✕</button>
    </div>
    <div id="modalContent"></div>
  </div>
</div>
<div id="toast" class="toast hidden" role="status"></div>

<script>
"use strict";

/* =========================================================
   2. STATE AND STORAGE
   ========================================================= */
const $ = (id) => document.getElementById(id);
const STORAGE_KEY = "nexus-ai-chat-v1";
const NOTES_KEY = "nexus-ai-notes-v1";
const THEME_KEY = "nexus-ai-theme-v1";
let history = [];
let busy = false;
let activeController = null;
let toastTimer = null;
let recognition = null;
let editingNoteId = null;

function safeLoad(key, fallback) {
  try {
    const value = localStorage.getItem(key);
    return value === null ? fallback : JSON.parse(value);
  } catch {
    return fallback;
  }
}
function saveHistory() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(history.slice(-100)));
  } catch {
    showToast("Browser storage is unavailable. This chat may not persist.");
  }
}
function getNotes() {
  const notes = safeLoad(NOTES_KEY, []);
  return Array.isArray(notes) ? notes : [];
}
function saveNotes(notes) {
  try {
    localStorage.setItem(NOTES_KEY, JSON.stringify(notes));
    return true;
  } catch {
    showToast("Could not save notes in browser storage.");
    return false;
  }
}
function showToast(message) {
  const toast = $("toast");
  toast.textContent = message;
  toast.classList.remove("hidden");
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => toast.classList.add("hidden"), 2800);
}
function setActivity(message) {
  $("activity").textContent = message;
}
function setBusy(value) {
  busy = value;
  $("sendBtn").disabled = value;
  $("stopBtn").disabled = !value;
  $("prompt").disabled = value;
  $("sendBtn").textContent = value ? "Working…" : "Send ↗";
}

/* =========================================================
   3. SAFE BASIC MARKDOWN RENDERER
   Escape HTML BEFORE applying a small Markdown subset.
   Never inject raw model-generated HTML into the page.
   ========================================================= */
function escapeHTML(text) {
  return String(text).replace(/[&<>"']/g, (ch) => ({
    "&": "&amp;", "<": "&lt;", ">": "&gt;",
    '"': "&quot;", "'": "&#39;"
  })[ch]);
}
function renderInline(text) {
  let s = escapeHTML(text);
  const code = [];
  s = s.replace(/`([^`\n]+)`/g, (_, value) => {
    const marker = "\u0000CODE" + code.length + "\u0000";
    code.push("<code>" + value + "</code>");
    return marker;
  });
  s = s.replace(/\*\*(.+?)\*\*/g, "<strong>$1</strong>");
  s = s.replace(/(^|[^*])\*([^*\n]+)\*/g, "$1<em>$2</em>");
  s = s.replace(/~~(.+?)~~/g, "<del>$1</del>");
  s = s.replace(/\u0000CODE(\d+)\u0000/g, (_, i) => code[Number(i)] || "");
  return s;
}
function renderMarkdown(text) {
  const source = String(text).replace(/\r\n?/g, "\n");
  const lines = source.split("\n");
  let out = "";
  let paragraph = [];
  let listType = "";
  let inCode = false;
  let codeLines = [];

  function flushParagraph() {
    if (paragraph.length) {
      out += "<p>" + paragraph.map(renderInline).join("<br>") + "</p>";
      paragraph = [];
    }
  }
  function closeList() {
    if (listType) {
      out += "</" + listType + ">";
      listType = "";
    }
  }
  function flushCode() {
    out += "<pre><code>" + escapeHTML(codeLines.join("\n")) + "</code></pre>";
    codeLines = [];
  }

  for (const line of lines) {
    if (line.trim().startsWith("```")) {
      flushParagraph();
      closeList();
      if (!inCode) {
        inCode = true;
        codeLines = [];
      } else {
        inCode = false;
        flushCode();
      }
      continue;
    }
    if (inCode) {
      codeLines.push(line);
      continue;
    }
    const trimmed = line.trim();
    if (!trimmed) {
      flushParagraph();
      closeList();
      continue;
    }
    if (/^[-*_]{3,}$/.test(trimmed)) {
      flushParagraph(); closeList(); out += "<hr>"; continue;
    }
    if (/^>\s?/.test(trimmed)) {
      flushParagraph(); closeList();
      out += "<blockquote>" + renderInline(trimmed.replace(/^>\s?/, "")) + "</blockquote>";
      continue;
    }
    const heading = trimmed.match(/^(#{1,4})\s+(.+)$/);
    if (heading) {
      flushParagraph(); closeList();
      const level = heading[1].length + 1;
      out += "<h" + level + ">" + renderInline(heading[2]) + "</h" + level + ">";
      continue;
    }
    const bullet = trimmed.match(/^[-*+]\s+(.+)$/);
    const numbered = trimmed.match(/^\d+[.)]\s+(.+)$/);
    if (bullet || numbered) {
      flushParagraph();
      const desired = bullet ? "ul" : "ol";
      if (listType !== desired) { closeList(); out += "<" + desired + ">"; listType = desired; }
      out += "<li>" + renderInline((bullet || numbered)[1]) + "</li>";
      continue;
    }
    closeList();
    paragraph.push(line);
  }
  if (inCode) flushCode();
  flushParagraph();
  closeList();
  return out;
}

/* =========================================================
   4. MESSAGE RENDERING AND CONVERSATION HISTORY
   ========================================================= */
function addMessage(role, text, options = {}) {
  const chat = $("chat");
  const row = document.createElement("article");
  row.className = "message " + (role === "user" ? "user" : "assistant");

  const avatar = document.createElement("div");
  avatar.className = "avatar";
  avatar.textContent = role === "user" ? "🙂" : "✧";
  avatar.setAttribute("aria-hidden", "true");

  const wrap = document.createElement("div");
  wrap.className = "bubble-wrap";

  const meta = document.createElement("div");
  meta.className = "msg-meta";
  meta.textContent = role === "user" ? "YOU" : "NEXUS AI";

  const bubble = document.createElement("div");
  bubble.className = "bubble";
  if (options.error) {
    bubble.style.borderColor = "var(--danger)";
    bubble.style.color = "var(--danger)";
  }
  bubble.innerHTML = renderMarkdown(text);

  wrap.append(meta, bubble);
  if (role !== "user" && !options.typing) {
    const tools = document.createElement("div");
    tools.className = "msg-tools";

    const copy = document.createElement("button");
    copy.type = "button";
    copy.textContent = "▣ Copy";
    copy.setAttribute("aria-label", "Copy AI response");
    copy.addEventListener("click", () => copyText(text));

    const speak = document.createElement("button");
    speak.type = "button";
    speak.textContent = "🔊 Read aloud";
    speak.setAttribute("aria-label", "Read AI response aloud");
    speak.addEventListener("click", () => speakText(text));

    tools.append(copy, speak);
    wrap.append(tools);
  }
  row.append(avatar, wrap);
  chat.append(row);
  chat.scrollIntoView({ block: "end", behavior: "smooth" });
  window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
  return row;
}
function addTyping() {
  const row = document.createElement("article");
  row.className = "message";
  row.id = "typingRow";
  const avatar = document.createElement("div");
  avatar.className = "avatar";
  avatar.textContent = "✧";
  const wrap = document.createElement("div");
  wrap.className = "bubble-wrap";
  const meta = document.createElement("div");
  meta.className = "msg-meta";
  meta.textContent = "NEXUS AI IS THINKING";
  const bubble = document.createElement("div");
  bubble.className = "bubble";
  bubble.innerHTML = '<div class="typing" aria-label="Waiting for response"><span></span><span></span><span></span></div>';
  wrap.append(meta, bubble);
  row.append(avatar, wrap);
  $("chat").append(row);
  row.scrollIntoView({ block: "end", behavior: "smooth" });
}
function removeTyping() {
  const row = $("typingRow");
  if (row) row.remove();
}
function renderSavedHistory() {
  $("chat").replaceChildren();
  if (!history.length) {
    $("welcome").classList.remove("hidden");
    addMessage("assistant", "Hi! I'm NEXUS Ai ✧ What would you like to explore today?");
    return;
  }
  $("welcome").classList.add("hidden");
  for (const item of history) {
    if (item && (item.role === "user" || item.role === "assistant") && typeof item.text === "string") {
      addMessage(item.role, item.text, { error: item.error === true });
    }
  }
}
function clearChat() {
  if (busy) {
    showToast("Stop the current request before clearing the chat.");
    return;
  }
  history = [];
  try { localStorage.removeItem(STORAGE_KEY); } catch {}
  $("chat").replaceChildren();
  $("welcome").classList.remove("hidden");
  addMessage("assistant", "Hi! I'm NEXUS Ai ✧ A fresh space for your next idea. What shall we work on?");
  $("metricLatency").textContent = "Not measured";
  $("metricRoute").textContent = "Awaiting request";
  setActivity("New conversation ready.");
}
function newChat() { clearChat(); $("prompt").focus(); }

/* =========================================================
   5. ADAPTIVE ROUTER AND DETERMINISTIC CALCULATOR
   This router selects a local calculator only for basic
   arithmetic. All other supported tasks use the configured
   Gemini API. It does not claim to switch between models.
   ========================================================= */
function classifyTask(text) {
  const s = text.toLowerCase();
  if (/^(what is |calculate |compute |solve )?[\d\s()+\-*/%.]+[=?]?$/.test(s.trim()) &&
      /\d/.test(s) && /[+\-*/%()]/.test(s)) return "Mathematics";
  if (/\b(code|javascript|python|html|css|debug|program|function|error in)\b/.test(s)) return "Coding";
  if (/\b(summarize|summary|make shorter|key points)\b/.test(s)) return "Summarization";
  if (/\b(study plan|schedule|plan my|checklist|roadmap)\b/.test(s)) return "Planning";
  if (/\b(document|uploaded file|attached file|pdf)\b/.test(s)) return "Document help";
  if (/\b(research|latest|current news|search the web|sources)\b/.test(s)) return "Research request";
  return "Conversation";
}
function safeCalculate(input) {
  let expr = input.trim().replace(/^(what is|calculate|compute|solve)\s+/i, "").replace(/[=?]\s*$/, "").trim();
  if (!expr || expr.length > 100 || !/^[\d\s()+\-*/%.]+$/.test(expr)) return null;
  if (!/\d/.test(expr) || !/[+\-*/%()]/.test(expr)) return null;

  let i = 0;
  function skip() { while (/\s/.test(expr[i] || " ") && i < expr.length) i++; }
  function number() {
    skip();
    if (expr[i] === "+") { i++; return number(); }
    if (expr[i] === "-") { i++; return -number(); }
    if (expr[i] === "(") {
      i++; const value = addSub(); skip();
      if (expr[i] !== ")") throw Error("Missing closing parenthesis.");
      i++; return value;
    }
    const match = expr.slice(i).match(/^(?:\d+\.?\d*|\.\d+)/);
    if (!match) throw Error("Invalid arithmetic expression.");
    i += match[0].length;
    const value = Number(match[0]);
    if (!Number.isFinite(value)) throw Error("Number is out of range.");
    return value;
  }
  function mulDiv() {
    let v = number();
    while (true) {
      skip(); const op = expr[i];
      if (!["*", "/", "%"].includes(op)) break;
      i++; const b = number();
      if ((op === "/" || op === "%") && b === 0) throw Error("Division by zero is not allowed.");
      v = op === "*" ? v * b : op === "/" ? v / b : v % b;
      if (!Number.isFinite(v)) throw Error("Result is out of range.");
    }
    return v;
  }
  function addSub() {
    let v = mulDiv();
    while (true) {
      skip(); const op = expr[i];
      if (op !== "+" && op !== "-") break;
      i++; const b = mulDiv(); v = op === "+" ? v + b : v - b;
      if (!Number.isFinite(v)) throw Error("Result is out of range.");
    }
    return v;
  }
  try {
    const result = addSub(); skip();
    if (i !== expr.length) return null;
    return String(Number(result.toPrecision(12)));
  } catch (err) {
    if (err.message === "Division by zero is not allowed.") return "ERROR:" + err.message;
    return null;
  }
}

/* =========================================================
   6. GEMINI API
   The function below follows the requested API URL and
   request/response format. A direct browser key is exposed.
   ========================================================= */
async function callAI(history) {
  // history = [{ role: "user" | "assistant", text: "..." }, ...]
  const url = "https://generativelanguage.googleapis.com/v1beta/models/" + MODEL + ":generateContent";
  const res = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json", "x-goog-api-key": API_KEY.trim() },
    body: JSON.stringify({
      system_instruction: { parts: [{ text: SYSTEM_PROMPT }] },
      contents: history.map(m => ({
        role: m.role === "assistant" ? "model" : "user",
        parts: [{ text: m.text }]
      }))
    })
  });
  const data = await res.json().catch(() => ({}));
  if (!res.ok) throw new Error("Error " + res.status + ": " + ((data.error && data.error.message) || "Request failed"));
  const parts = (data.candidates && data.candidates[0] && data.candidates[0].content && data.candidates[0].content.parts) || [];
  const text = parts.filter(p => p.text && !p.thought).map(p => p.text).join("");
  if (!text) throw new Error("The AI sent an empty reply. Try asking again.");
  return text;
}

/* =========================================================
   7. SEND, ROUTING, ERRORS AND STOP CONTROL
   ========================================================= */
async function sendMessage(prefilledText) {
  if (busy) return;
  const text = String(prefilledText ?? $("prompt").value).trim();
  if (!text) { $("prompt").focus(); return; }
  if (text.length > 12000) { showToast("Please keep your message under 12,000 characters."); return; }

  $("prompt").value = "";
  $("prompt").style.height = "";
  $("welcome").classList.add("hidden");
  addMessage("user", text);
  history.push({ role: "user", text });
  saveHistory();

  const category = classifyTask(text);
  $("metricRoute").textContent = category;
  $("metricModel").textContent = category === "Mathematics" && safeCalculate(text) !== null
    ? "Local calculator" : MODEL;

  if (category === "Mathematics") {
    const result = safeCalculate(text);
    if (result !== null) {
      if (result.startsWith("ERROR:")) {
        const errorText = result.slice(6);
        addMessage("assistant", errorText, { error: true });
        history.push({ role: "assistant", text: errorText, error: true });
      } else {
        const reply = "**Result:** `" + result + "`\n\n*Calculated locally in your browser; no AI API call was needed.*";
        addMessage("assistant", reply);
        history.push({ role: "assistant", text: reply });
      }
      saveHistory();
      setActivity("Completed with the local calculator. No API request was made.");
      $("metricLatency").textContent = "Local";
      return;
    }
  }

  if (!API_KEY.trim() || API_KEY.trim() === "PASTE_YOUR_API_KEY_HERE") {
    const msg = "Paste your API key on line 6 of index.html, save, and refresh.";
    addMessage("assistant", msg, { error: true });
    history.push({ role: "assistant", text: msg, error: true });
    saveHistory();
    setActivity("Setup needed: no API key configured.");
    return;
  }

  if (!navigator.onLine) {
    const msg = "You appear to be offline. Connect to the internet and try again.";
    addMessage("assistant", msg, { error: true });
    history.push({ role: "assistant", text: msg, error: true });
    saveHistory();
    setActivity("Request not sent: browser reports no internet connection.");
    return;
  }

  const requestHistory = history
    .filter(m => (m.role === "user" || m.role === "assistant") && !m.error)
    .slice(-30)
    .map(m => ({ role: m.role, text: m.text }));

  setBusy(true);
  activeController = new AbortController();
  // The specified generateContent request does not support an AbortSignal
  // in the exact callAI signature. Stop therefore stops the UI wait, not
  // necessarily the already-sent network request.
  addTyping();
  setActivity("Understanding request → selecting configured model → generating response.");
  const started = performance.now();

  try {
    const reply = await callAI(requestHistory);
    if (!busy) return;
    const elapsed = Math.round(performance.now() - started);
    addMessage("assistant", reply);
    history.push({ role: "assistant", text: reply });
    saveHistory();
    $("metricLatency").textContent = elapsed + " ms";
    $("metricModel").textContent = MODEL;
    setActivity("Completed. Remote response received from the configured Gemini API.");
    $("connectionLabel").textContent = "API ready";
  } catch (err) {
    if (!busy) return;
    const message = err && err.message ? err.message : String(err);
    addMessage("assistant", "**Request failed**\n\n" + message, { error: true });
    history.push({ role: "assistant", text: "Request failed: " + message, error: true });
    saveHistory();
    setActivity("Request failed. Read the error in the red message bubble.");
    $("connectionLabel").textContent = "Request error";
  } finally {
    removeTyping();
    setBusy(false);
    activeController = null;
    $("prompt").focus();
  }
}
function stopGeneration() {
  if (!busy) return;
  busy = false;
  removeTyping();
  setBusy(false);
  setActivity("Stopped waiting. The browser request may still finish in the background.");
  showToast("Stopped waiting. This API call may not be cancellable.");
}

/* =========================================================
   8. COPY, SPEECH AND EXPORT
   ========================================================= */
async function copyText(text) {
  try {
    if (navigator.clipboard && window.isSecureContext) {
      await navigator.clipboard.writeText(text);
    } else {
      const area = document.createElement("textarea");
      area.value = text;
      area.style.position = "fixed";
      area.style.opacity = "0";
      document.body.append(area);
      area.select();
      const ok = document.execCommand("copy");
      area.remove();
      if (!ok) throw new Error("Copy unavailable");
    }
    showToast("Copied to clipboard.");
  } catch {
    showToast("Copy was blocked by the browser. Select the text and copy it manually.");
  }
}
function speakText(text) {
  if (!("speechSynthesis" in window)) {
    showToast("Text-to-speech is not supported by this browser.");
    return;
  }
  window.speechSynthesis.cancel();
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.lang = "en-IN";
  window.speechSynthesis.speak(utterance);
}
function exportConversation() {
  const transcript = history.map(m => (m.role === "user" ? "YOU" : "NEXUS AI") + ":\n" + m.text).join("\n\n");
  if (!transcript.trim()) { showToast("There is no conversation to export."); return; }
  const blob = new Blob([transcript], { type: "text/plain;charset=utf-8" });
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = "nexus-conversation.txt";
  document.body.append(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
  showToast("Conversation export requested.");
}

/* =========================================================
   9. VOICE INPUT
   ========================================================= */
function setupVoice() {
  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  if (!SpeechRecognition) return;
  $("micBtn").classList.remove("hidden");
  recognition = new SpeechRecognition();
  recognition.lang = "en-IN";
  recognition.interimResults = false;
  recognition.maxAlternatives = 1;
  recognition.onresult = (event) => {
    const transcript = event.results[0][0].transcript;
    $("prompt").value = ($("prompt").value ? $("prompt").value + " " : "") + transcript;
    $("prompt").focus();
  };
  recognition.onerror = (event) => showToast("Voice input error: " + event.error);
  recognition.onend = () => {
    $("micBtn").textContent = "🎙";
    $("micBtn").disabled = false;
  };
  $("micBtn").addEventListener("click", () => {
    try {
      $("micBtn").textContent = "●";
      $("micBtn").disabled = true;
      recognition.start();
      showToast("Listening… speak clearly.");
    } catch {
      $("micBtn").disabled = false;
      $("micBtn").textContent = "🎙";
      showToast("Voice input could not start. Try again.");
    }
  });
}

/* =========================================================
   10. THEME AND MODAL WORKSPACE
   ========================================================= */
function setTheme(theme) {
  document.body.classList.toggle("light", theme === "light");
  $("themeBtn").textContent = theme === "light" ? "☾" : "☼";
  try { localStorage.setItem(THEME_KEY, theme); } catch {}
}
function openModal(title, html) {
  $("modalTitle").textContent = title;
  $("modalContent").replaceChildren();
  // Modal content is built only from fixed application templates.
  const template = document.createElement("template");
  template.innerHTML = html;
  $("modalContent").append(template.content.cloneNode(true));
  $("modalOverlay").classList.add("open");
  $("closeModalBtn").focus();
}
function closeModal() { $("modalOverlay").classList.remove("open"); }

function openSettings() {
  openModal("Setup & privacy", `
    <p><strong>Provider:</strong> Google Gemini API</p>
    <p><strong>Configured model:</strong> <code>${escapeHTML(MODEL)}</code></p>
    <p>The API key is read from line 6 in this file. This direct-browser prototype exposes the key to anyone who can inspect the source. Do not publish or share a file containing a real key.</p>
    <p>Choose a currently supported model in Google's Gemini API documentation if the configured model returns a model-not-found error. This prototype intentionally retains your requested model ID.</p>
    <p><strong>Storage:</strong> chat history, theme and notes are saved in this browser's local storage. Clearing browser data can remove them. Notes are not sent automatically to the AI.</p>
    <p><strong>Network:</strong> AI replies require internet access. The calculator works locally. No offline language model is included.</p>
    <button class="btn" id="clearStorageBtn">Clear chat and saved notes</button>
  `);
  $("clearStorageBtn").addEventListener("click", () => {
    if (!confirm("Delete this browser's saved chat history and notes?")) return;
    history = [];
    try {
      localStorage.removeItem(STORAGE_KEY);
      localStorage.removeItem(NOTES_KEY);
    } catch {}
    closeModal();
    clearChat();
    showToast("Chat history and notes cleared.");
  });
}

function openCalculator() {
  openModal("Local calculator", `
    <p>Evaluates basic arithmetic locally. Use numbers, parentheses, +, −, ×, ÷ and %. No AI request is needed.</p>
    <label for="calcInput">Expression</label>
    <input id="calcInput" placeholder="Example: (125 + 75) * 3">
    <button class="btn primary" id="calcRun">Calculate</button>
    <p id="calcResult" role="status">Your result will appear here.</p>
  `);
  $("calcRun").addEventListener("click", () => {
    const value = $("calcInput").value.trim();
    const result = safeCalculate(value);
    $("calcResult").textContent = result === null ? "Enter a valid arithmetic expression." :
      result.startsWith("ERROR:") ? result.slice(6) : "Result: " + result;
  });
  $("calcInput").addEventListener("keydown", (e) => {
    if (e.key === "Enter") $("calcRun").click();
  });
}

function openMemory() {
  renderNotesModal();
}
function renderNotesModal() {
  const notes = getNotes();
  openModal("Saved notes", `
    <p>These notes are stored locally in this browser. They are not automatically added to AI prompts.</p>
    <label for="noteTitle">Note title</label>
    <input id="noteTitle" maxlength="100" placeholder="Project idea">
    <label for="noteText">Note</label>
    <textarea id="noteText" maxlength="5000" placeholder="Write a useful preference or project note…"></textarea>
    <button class="btn primary" id="saveNoteBtn">Save note</button>
    <div id="notesList"></div>
  `);
  const list = $("notesList");
  if (!notes.length) {
    const empty = document.createElement("p");
    empty.textContent = "No saved notes yet.";
    list.append(empty);
  }
  notes.forEach(note => {
    const card = document.createElement("div");
    card.className = "note-item";
    const title = document.createElement("strong");
    title.textContent = note.title || "Untitled note";
    const body = document.createElement("p");
    body.textContent = note.text || "";
    const actions = document.createElement("div");
    actions.className = "note-actions";
    const edit = document.createElement("button");
    edit.className = "btn";
    edit.textContent = "Edit";
    edit.addEventListener("click", () => {
      $("noteTitle").value = note.title || "";
      $("noteText").value = note.text || "";
      editingNoteId = note.id;
      $("saveNoteBtn").textContent = "Update note";
      $("noteTitle").focus();
    });
    const del = document.createElement("button");
    del.className = "btn";
    del.textContent = "Delete";
    del.addEventListener("click", () => {
      if (!confirm("Delete this note?")) return;
      saveNotes(getNotes().filter(n => n.id !== note.id));
      editingNoteId = null;
      renderNotesModal();
    });
    actions.append(edit, del);
    card.append(title, body, actions);
    list.append(card);
  });
  $("saveNoteBtn").addEventListener("click", () => {
    const title = $("noteTitle").value.trim();
    const text = $("noteText").value.trim();
    if (!title || !text) { showToast("Enter both a title and a note."); return; }
    const current = getNotes();
    if (editingNoteId) {
      const idx = current.findIndex(n => n.id === editingNoteId);
      if (idx >= 0) current[idx] = { ...current[idx], title, text };
      else current.push({ id: editingNoteId, title, text });
    } else {
      current.push({ id: Date.now().toString(36) + Math.random().toString(36).slice(2, 7), title, text });
    }
    if (saveNotes(current)) {
      editingNoteId = null;
      renderNotesModal();
      showToast("Note saved in this browser.");
    }
  });
}

/* =========================================================
   11. EVENT HANDLERS AND STARTUP
   ========================================================= */
$("sendBtn").addEventListener("click", () => sendMessage());
$("stopBtn").addEventListener("click", stopGeneration);
$("newChatBtn").addEventListener("click", newChat);
$("clearBtn").addEventListener("click", () => {
  if (confirm("Clear the current conversation and saved chat history?")) clearChat();
});
$("exportBtn").addEventListener("click", exportConversation);
$("calculatorBtn").addEventListener("click", openCalculator);
$("memoryBtn").addEventListener("click", openMemory);
$("settingsBtn").addEventListener("click", openSettings);
$("themeBtn").addEventListener("click", () => {
  setTheme(document.body.classList.contains("light") ? "dark" : "light");
});
$("closeModalBtn").addEventListener("click", closeModal);
$("modalOverlay").addEventListener("click", (event) => {
  if (event.target === $("modalOverlay")) closeModal();
});
document.addEventListener("keydown", (event) => {
  if (event.key === "Escape") closeModal();
});
$("prompt").addEventListener("keydown", (event) => {
  if (event.key === "Enter" && !event.shiftKey && !event.isComposing) {
    event.preventDefault();
    sendMessage();
  }
});
$("prompt").addEventListener("input", () => {
  $("prompt").style.height = "auto";
  $("prompt").style.height = Math.min($("prompt").scrollHeight, 180) + "px";
});
$("starterChips").addEventListener("click", (event) => {
  const button = event.target.closest("button");
  if (button) sendMessage(button.textContent.trim());
});
window.addEventListener("online", () => {
  $("connectionLabel").textContent = "Network available";
  setActivity("Internet connection detected. Gemini requests can be attempted.");
});
window.addEventListener("offline", () => {
  $("connectionLabel").textContent = "Offline";
  setActivity("Offline. Local calculator and saved notes remain available.");
});

(function init() {
  try {
    history = safeLoad(STORAGE_KEY, []);
    if (!Array.isArray(history)) history = [];
    history = history.filter(m =>
      m && (m.role === "user" || m.role === "assistant") &&
      typeof m.text === "string"
    ).slice(-100);
  } catch {
    history = [];
  }
  const theme = safeLoad(THEME_KEY, "dark");
  setTheme(theme === "light" ? "light" : "dark");
  renderSavedHistory();
  setupVoice();
  $("connectionLabel").textContent = navigator.onLine ? "Ready to connect" : "Offline";
  $("metricModel").textContent = MODEL;
  $("prompt").focus();
})();
</script>
</body>
</html>
