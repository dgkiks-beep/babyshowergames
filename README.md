<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>🍼 Baby Shower Quiz — Joueur</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --rose: #ec4899;
    --lav: #a855f7;
    --mint: #059669;
    --gold: #f59e0b;
    --text: #1e1b4b;
    --muted: #6b6a8a;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body {
    font-family: 'Nunito', sans-serif;
    background: linear-gradient(160deg, #fdf2f8 0%, #f0fdf4 50%, #ede9fe 100%);
    min-height: 100dvh;
    color: var(--text);
    overflow-x: hidden;
  }

  /* SCREENS */
  .screen { display: none; min-height: 100dvh; flex-direction: column; align-items: center; padding: 24px 20px; }
  .screen.active { display: flex; }

  /* HEADER */
  .mobile-header {
    width: 100%;
    background: linear-gradient(90deg, #ec4899, #a855f7);
    color: white;
    text-align: center;
    padding: 16px 20px;
    border-radius: 0 0 24px 24px;
    margin-bottom: 24px;
    box-shadow: 0 4px 20px rgba(236,72,153,0.3);
  }
  .mobile-header h1 { font-family: 'Playfair Display', serif; font-size: 22px; }
  .mobile-header .sub { font-size: 13px; opacity: 0.85; margin-top: 4px; }

  /* NAME ENTRY */
  .name-card {
    background: white;
    border-radius: 24px;
    padding: 28px 24px;
    width: 100%;
    max-width: 380px;
    box-shadow: 0 8px 32px rgba(168,85,247,0.12);
    text-align: center;
    border: 2.5px solid #f3e8ff;
  }
  .name-card .emoji { font-size: 52px; margin-bottom: 14px; }
  .name-card h2 { font-family: 'Playfair Display', serif; font-size: 24px; margin-bottom: 8px; }
  .name-card p { color: var(--muted); font-size: 14px; margin-bottom: 20px; }
  
  .big-input {
    width: 100%;
    padding: 16px 18px;
    border-radius: 16px;
    border: 2.5px solid #f3e8ff;
    font-family: 'Nunito', sans-serif;
    font-size: 18px;
    font-weight: 700;
    text-align: center;
    outline: none;
    color: var(--text);
    transition: border 0.2s;
    margin-bottom: 16px;
  }
  .big-input:focus { border-color: #a855f7; }

  /* BUTTONS */
  .btn {
    width: 100%;
    padding: 16px 20px;
    border-radius: 16px;
    border: none;
    font-family: 'Nunito', sans-serif;
    font-weight: 800;
    font-size: 17px;
    cursor: pointer;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    touch-action: manipulation;
  }
  .btn:active { transform: scale(0.97); }
  .btn-primary { background: linear-gradient(135deg, #ec4899, #a855f7); color: white; box-shadow: 0 4px 16px rgba(236,72,153,0.35); }
  .btn-success { background: linear-gradient(135deg, #059669, #10b981); color: white; box-shadow: 0 4px 16px rgba(5,150,105,0.3); }
  .btn-light { background: white; color: var(--text); border: 2.5px solid #f3e8ff; }
  .btn-answer {
    padding: 18px 16px;
    border-radius: 18px;
    border: 2.5px solid #f3e8ff;
    background: white;
    font-family: 'Nunito', sans-serif;
    font-size: 15px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 12px;
    touch-action: manipulation;
    color: var(--text);
    width: 100%;
    margin-bottom: 12px;
  }
  .btn-answer:active { transform: scale(0.98); }
  .btn-answer .letter {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: #f3e8ff;
    color: #7c3aed;
    display: flex; align-items: center; justify-content: center;
    font-weight: 900; font-size: 16px;
    flex-shrink: 0;
  }
  .btn-answer.selected { border-color: #a855f7; background: #fdf4ff; }
  .btn-answer.selected .letter { background: #a855f7; color: white; }
  .btn-answer.correct { border-color: #059669; background: #f0fdf4; }
  .btn-answer.correct .letter { background: #059669; color: white; }
  .btn-answer.wrong { border-color: #ef4444; background: #fef2f2; }
  .btn-answer.wrong .letter { background: #ef4444; color: white; }

  /* WAITING */
  .waiting-card {
    background: white;
    border-radius: 24px;
    padding: 32px 24px;
    text-align: center;
    border: 2.5px solid #f3e8ff;
    width: 100%;
    max-width: 380px;
    box-shadow: 0 8px 32px rgba(168,85,247,0.1);
  }
  .waiting-card .emoji { font-size: 50px; margin-bottom: 14px; }
  .spin-emoji { animation: spin 2s linear infinite; display: inline-block; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .bounce-emoji { animation: bounce 1s ease-in-out infinite; display: inline-block; }
  @keyframes bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }

  /* QUESTION CARD */
  .q-card {
    background: white;
    border-radius: 24px;
    padding: 24px 20px;
    width: 100%;
    max-width: 420px;
    box-shadow: 0 8px 32px rgba(168,85,247,0.12);
    border: 2.5px solid #f9a8d4;
    animation: slideIn 0.35s ease;
  }
  @keyframes slideIn { from{opacity:0;transform:translateY(16px)} to{opacity:1;transform:translateY(0)} }

  .q-badge {
    display: inline-block;
    background: linear-gradient(90deg, #ec4899, #a855f7);
    color: white;
    border-radius: 20px;
    padding: 4px 14px;
    font-size: 12px;
    font-weight: 700;
    margin-bottom: 12px;
  }
  .q-text {
    font-family: 'Playfair Display', serif;
    font-size: 19px;
    line-height: 1.45;
    color: var(--text);
    margin-bottom: 20px;
  }

  .timer-bar-wrap { height: 6px; background: #f3e8ff; border-radius: 6px; margin-bottom: 20px; overflow: hidden; }
  .timer-bar { height: 100%; background: linear-gradient(90deg,#ec4899,#a855f7); border-radius: 6px; transition: width 1s linear; }

  /* SCORE DISPLAY */
  .score-chip {
    background: linear-gradient(135deg,#fbbf24,#f59e0b);
    color: #78350f;
    border-radius: 20px;
    padding: 6px 18px;
    font-size: 15px;
    font-weight: 900;
    display: inline-block;
  }

  /* TEXT ANSWER */
  .text-answer-area {
    width: 100%;
    padding: 14px 16px;
    border-radius: 16px;
    border: 2.5px solid #f3e8ff;
    font-family: 'Nunito', sans-serif;
    font-size: 16px;
    font-weight: 700;
    outline: none;
    color: var(--text);
    margin-bottom: 12px;
    transition: border 0.2s;
    resize: none;
  }
  .text-answer-area:focus { border-color: #a855f7; }

  /* RESULT SCREEN */
  .result-card {
    background: white;
    border-radius: 24px;
    padding: 28px 24px;
    text-align: center;
    border: 2.5px solid #f3e8ff;
    width: 100%;
    max-width: 380px;
    animation: slideIn 0.4s ease;
  }
  .result-correct { border-color: #059669; background: #f0fdf4; }
  .result-wrong { border-color: #ef4444; background: #fef2f2; }

  /* PRICE GAME */
  .price-slider-wrap { padding: 10px 0; }
  .price-item-name {
    font-family: 'Playfair Display', serif;
    font-size: 20px;
    color: var(--text);
    text-align: center;
    margin-bottom: 16px;
    line-height: 1.4;
  }
  .price-estimate-input {
    width: 100%;
    padding: 18px;
    border-radius: 18px;
    border: 2.5px solid #f3e8ff;
    font-size: 28px;
    font-weight: 900;
    text-align: center;
    font-family: 'Nunito', sans-serif;
    outline: none;
    color: var(--mint);
    transition: border 0.2s;
    margin-bottom: 8px;
  }
  .price-estimate-input:focus { border-color: #059669; }

  /* LEADERBOARD */
  .lb-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    border-bottom: 1.5px solid #f3e8ff;
    font-size: 15px;
    font-weight: 700;
  }
  .lb-row.me { background: #fdf4ff; border-radius: 12px; border-color: #a855f7; }
  .lb-pts { color: #7c3aed; font-size: 17px; }

  /* STATUS */
  .status-pill {
    background: rgba(255,255,255,0.8);
    border: 1.5px solid #f3e8ff;
    border-radius: 20px;
    padding: 5px 14px;
    font-size: 12px;
    font-weight: 700;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 6px;
    margin-bottom: 16px;
  }
  .dot-live { width: 7px; height: 7px; border-radius: 50%; background: #059669; animation: blink 1.5s infinite; }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0.3} }

  .confetti-container { position: fixed; inset: 0; pointer-events: none; z-index: 200; overflow: hidden; }
  .confetto { position: absolute; width: 8px; height: 8px; opacity: 0; animation: fall 3s ease-out forwards; border-radius: 2px; }
  @keyframes fall { 0%{opacity:1;transform:translateY(-20px) rotate(0)} 100%{opacity:0;transform:translateY(100vh) rotate(720deg)} }

  .connection-form { width: 100%; max-width: 380px; }
</style>
</head>
<body>

<div class="confetti-container" id="confetti-container"></div>

<!-- SCREEN: JOIN -->
<div class="screen active" id="screen-join">
  <div class="mobile-header">
    <h1>🍼 Baby Shower Quiz</h1>
    <div class="sub">Rejoignez le jeu !</div>
  </div>
  <div class="connection-form">
    <div class="name-card">
      <div class="emoji">👋</div>
      <h2>Votre prénom</h2>
      <p>Entrez votre prénom pour rejoindre la partie</p>
      <input class="big-input" id="player-name-input" type="text" placeholder="Marie…" maxlength="20" autocomplete="off">
      <div class="form-group" style="margin-bottom:14px;">
        <label style="font-size:12px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:0.5px;display:block;margin-bottom:6px;">Code de session</label>
        <input class="big-input" id="session-code-input" type="text" placeholder="XXXXX" maxlength="8" style="font-size:22px;letter-spacing:4px;text-transform:uppercase;" autocomplete="off">
      </div>
      <button class="btn btn-primary" onclick="joinGame()">🎉 Rejoindre !</button>
    </div>
  </div>
</div>

<!-- SCREEN: WAITING -->
<div class="screen" id="screen-waiting">
  <div class="mobile-header">
    <h1>🍼 Baby Shower Quiz</h1>
    <div class="sub" id="waiting-name-display">Bienvenue !</div>
  </div>
  <div class="waiting-card">
    <div class="emoji bounce-emoji">🍼</div>
    <h2 style="font-family:'Playfair Display',serif;margin-bottom:8px;">Vous êtes connecté·e !</h2>
    <p style="color:var(--muted);font-size:14px;margin-bottom:20px;">En attente du démarrage par l'animateur…</p>
    <div class="score-chip" id="my-score-wait">⭐ 0 point</div>
    <div style="margin-top:20px;font-size:13px;color:var(--muted);">
      <span class="spin-emoji">⏳</span> L'animateur lancera le jeu depuis la TV
    </div>
  </div>
</div>

<!-- SCREEN: QUESTION (QUIZ) -->
<div class="screen" id="screen-question">
  <div class="mobile-header" id="q-screen-header">
    <h1 id="q-screen-title">Question</h1>
    <div class="sub" id="q-screen-sub">En jeu</div>
  </div>
  <div class="status-pill"><div class="dot-live"></div><span id="q-progress-text">Q1 / 4</span></div>
  <div class="q-card" style="max-width:420px;width:100%;">
    <div class="q-badge" id="q-badge-label">Quiz</div>
    <div class="timer-bar-wrap"><div class="timer-bar" id="mobile-timer-bar" style="width:100%"></div></div>
    <div class="q-text" id="q-text-display">Chargement…</div>
    <div id="choices-container"></div>
    <div id="answer-status" style="display:none;text-align:center;padding:12px 0;font-weight:700;font-size:15px;"></div>
  </div>
  <div style="margin-top:16px;" id="score-display-q">
    <div class="score-chip" id="q-score-chip">⭐ 0 point</div>
  </div>
</div>

<!-- SCREEN: BLIND TEST -->
<div class="screen" id="screen-blind">
  <div class="mobile-header">
    <h1>🎵 Blind Test Disney</h1>
    <div class="sub" id="blind-sub">Musique en cours…</div>
  </div>
  <div class="q-card" style="max-width:420px;width:100%;">
    <div style="text-align:center;font-size:44px;margin:10px 0;animation:bounce 1s infinite ease-in-out;">🎵</div>
    <div class="q-text" style="text-align:center;">Quel est ce titre Disney ?</div>
    <textarea class="text-answer-area" id="blind-answer-input" rows="3" placeholder="Entrez votre réponse…"></textarea>
    <button class="btn btn-primary" id="blind-submit-btn" onclick="submitBlindAnswer()">✅ Valider ma réponse</button>
    <div id="blind-reveal-area" style="display:none;margin-top:16px;background:#f0fdf4;border-radius:16px;padding:16px;text-align:center;">
      <div style="font-size:13px;color:#059669;font-weight:700;margin-bottom:4px;">✅ La réponse était :</div>
      <div style="font-family:'Playfair Display',serif;font-size:20px;color:#1e1b4b;" id="blind-correct-answer"></div>
    </div>
  </div>
  <div style="margin-top:16px;"><div class="score-chip" id="blind-score-chip">⭐ 0 point</div></div>
</div>

<!-- SCREEN: PRIX -->
<div class="screen" id="screen-prix">
  <div class="mobile-header">
    <h1>💰 Le Juste Prix</h1>
    <div class="sub">Estimez le prix de l'objet !</div>
  </div>
  <div class="q-card" style="max-width:420px;width:100%;">
    <div style="font-size:44px;text-align:center;margin:10px 0;">🛒</div>
    <div class="price-item-name" id="prix-item-name">Chargement…</div>
    <p id="prix-hint" style="text-align:center;color:#7c3aed;font-style:italic;font-size:14px;margin-bottom:16px;"></p>
    <input type="number" class="price-estimate-input" id="prix-estimate" placeholder="0.00" min="0" step="0.01">
    <div style="text-align:center;color:var(--muted);font-size:12px;margin-bottom:16px;">Entrez votre estimation en euros (€)</div>
    <button class="btn btn-success" id="prix-submit-btn" onclick="submitPriceAnswer()">✅ Envoyer mon estimation</button>
    <div id="prix-reveal-area" style="display:none;margin-top:16px;border-radius:16px;padding:20px;text-align:center;background:#f0fdf4;">
      <div style="font-size:13px;font-weight:700;color:#059669;margin-bottom:6px;">💰 Le vrai prix :</div>
      <div style="font-size:34px;font-weight:900;color:#059669;font-family:'Playfair Display',serif;" id="prix-real-price"></div>
      <div style="font-size:13px;color:var(--muted);margin-top:8px;" id="prix-winner-msg"></div>
    </div>
  </div>
  <div style="margin-top:16px;"><div class="score-chip" id="prix-score-chip">⭐ 0 point</div></div>
</div>

<!-- SCREEN: LEADERBOARD -->
<div class="screen" id="screen-leaderboard">
  <div class="mobile-header">
    <h1>🏆 Classement</h1>
    <div class="sub">Résultats finaux</div>
  </div>
  <div style="width:100%;max-width:380px;">
    <div style="background:white;border-radius:24px;padding:20px;border:2.5px solid #f3e8ff;margin-bottom:20px;">
      <div id="leaderboard-list"></div>
    </div>
    <button class="btn btn-light" onclick="showScreen('screen-waiting')">🔄 Nouvelle partie</button>
  </div>
</div>

<!-- SCREEN: ANSWERED -->
<div class="screen" id="screen-answered">
  <div class="mobile-header">
    <h1>🍼 Baby Shower Quiz</h1>
    <div class="sub">Réponse envoyée !</div>
  </div>
  <div class="result-card" id="result-card-display">
    <div style="font-size:50px;margin-bottom:14px;" id="result-emoji">⏳</div>
    <h2 style="font-family:'Playfair Display',serif;font-size:22px;margin-bottom:8px;" id="result-title">En attente…</h2>
    <p style="color:var(--muted);font-size:14px;margin-bottom:20px;" id="result-sub">L'animateur va révéler la réponse</p>
    <div class="score-chip" id="result-score-chip">⭐ 0 point</div>
  </div>
</div>

<script>
// =====================
// STATE
// =====================
let playerName = '';
let sessionCode = '';
let myScore = 0;
let bc = null;
let currentAnswered = false;
let currentGameType = '';
let currentQuestionIndex = -1;

// Get session code from URL
const urlParams = new URLSearchParams(window.location.search);
if (urlParams.get('code')) {
  document.getElementById('session-code-input').value = urlParams.get('code');
}

// =====================
// SCREENS
// =====================
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

// =====================
// JOIN
// =====================
function joinGame() {
  const name = document.getElementById('player-name-input').value.trim();
  const code = document.getElementById('session-code-input').value.trim().toUpperCase();
  if (!name) { alert('Entrez votre prénom !'); return; }
  if (!code) { alert('Entrez le code de session affiché sur la TV !'); return; }
  playerName = name;
  sessionCode = code;

  bc = new BroadcastChannel('baby_shower_' + sessionCode);
  bc.onmessage = handleMessage;
  bc.postMessage({ type: 'join', name: playerName });
  bc.postMessage({ type: 'ping' });

  document.getElementById('waiting-name-display').textContent = `Bienvenue ${playerName} ! 🎉`;
  showScreen('screen-waiting');

  // Retry ping
  setTimeout(() => { if (bc) bc.postMessage({ type: 'ping' }); }, 1500);
}

// =====================
// MESSAGE HANDLER
// =====================
function handleMessage(e) {
  const msg = e.data;

  if (msg.type === 'state') {
    handleState(msg);
  } else if (msg.type === 'reveal') {
    handleReveal(msg);
  } else if (msg.type === 'blind_reveal') {
    showBlindReveal(msg.answer);
  } else if (msg.type === 'price_reveal') {
    showPriceReveal(msg.price);
  } else if (msg.type === 'price_winner') {
    showPriceWinner(msg.name, msg.price);
  } else if (msg.type === 'score_update') {
    if (msg.name === playerName) {
      myScore = msg.score;
      updateAllScores();
    }
  } else if (msg.type === 'leaderboard') {
    showLeaderboard(msg.scores);
  }
}

function handleState(state) {
  updateAllScores();
  const gt = state.gameType;
  currentGameType = gt;
  const qIdx = state.questionIndex;

  if (gt === 'quiz') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showQuizQuestion(state);
    }
  } else if (gt === 'celebrites') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showCelebQuestion(state);
    }
  } else if (gt === 'blindtest') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showBlindScreen(state);
    }
  } else if (gt === 'prix') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showPrixScreen(state);
    }
  }
}

// =====================
// QUIZ / CELEB
// =====================
function showQuizQuestion(state) {
  const titles = { maternite: '🤰 Maternité', parents: '👶 Parents' };
  document.getElementById('q-screen-title').textContent = titles[state.gameKey] || 'Quiz';
  document.getElementById('q-screen-sub').textContent = `${playerName} — Q${state.questionIndex+1}/${state.total}`;
  document.getElementById('q-badge-label').textContent = titles[state.gameKey] || 'Quiz';
  document.getElementById('q-progress-text').textContent = `Q${state.questionIndex+1} / ${state.total}`;
  document.getElementById('q-text-display').textContent = state.questionText;
  document.getElementById('answer-status').style.display = 'none';
  renderChoices(state.choices, 'quiz', state.questionIndex, state.gameKey);
  startMobileTimer(30);
  showScreen('screen-question');
}

function showCelebQuestion(state) {
  document.getElementById('q-screen-title').textContent = '⭐ Célébrités';
  document.getElementById('q-screen-sub').textContent = `${playerName}`;
  document.getElementById('q-badge-label').textContent = '⭐ Célébrités Bébés';
  document.getElementById('q-text-display').textContent = state.hint || 'Qui est ce bébé célèbre ?';
  document.getElementById('answer-status').style.display = 'none';
  renderChoices(state.choices, 'celebrites', state.questionIndex, null);
  startMobileTimer(30);
  showScreen('screen-question');
}

function renderChoices(choices, gameType, qIdx, gameKey) {
  const letters = ['A','B','C','D'];
  const container = document.getElementById('choices-container');
  container.innerHTML = (choices||[]).map((c,i) => `
    <button class="btn-answer" id="mobile-choice-${i}" onclick="selectAnswer(${i},'${gameType}','${gameKey}',${qIdx})">
      <div class="letter">${letters[i]}</div>
      <span>${c}</span>
    </button>`).join('');
}

let mobileTimerInterval = null;
let mobileTimerVal = 30;
function startMobileTimer(secs) {
  clearInterval(mobileTimerInterval);
  mobileTimerVal = secs;
  const bar = document.getElementById('mobile-timer-bar');
  mobileTimerInterval = setInterval(() => {
    mobileTimerVal--;
    if (bar) bar.style.width = (mobileTimerVal/secs*100)+'%';
    if (mobileTimerVal <= 0) clearInterval(mobileTimerInterval);
  }, 1000);
}

function selectAnswer(idx, gameType, gameKey, qIdx) {
  if (currentAnswered) return;
  currentAnswered = true;
  clearInterval(mobileTimerInterval);

  document.querySelectorAll('.btn-answer').forEach(b => b.style.opacity = '0.5');
  const selected = document.getElementById('mobile-choice-'+idx);
  if (selected) { selected.classList.add('selected'); selected.style.opacity = '1'; }

  if (bc) bc.postMessage({ type: 'answer', name: playerName, answer: idx, questionId: qIdx, gameKey });

  const statusEl = document.getElementById('answer-status');
  statusEl.style.display = 'block';
  statusEl.innerHTML = '⏳ Réponse envoyée ! En attente de la révélation…';
  statusEl.style.color = '#7c3aed';
}

function handleReveal(msg) {
  const correct = msg.correctIndex;
  const currentScreen = document.querySelector('.screen.active');
  if (!currentScreen) return;

  document.querySelectorAll('.btn-answer').forEach((btn, i) => {
    btn.style.opacity = '1';
    if (i === correct) btn.classList.add('correct');
    else btn.classList.remove('selected');
  });

  // Check if player was correct
  const selectedBtn = document.querySelector('.btn-answer.selected');
  const selectedIdx = selectedBtn ? parseInt(selectedBtn.id.replace('mobile-choice-','')) : -1;

  const statusEl = document.getElementById('answer-status');
  if (statusEl) {
    if (selectedIdx === correct) {
      statusEl.innerHTML = '🎉 Bonne réponse ! +points !';
      statusEl.style.color = '#059669';
      launchConfetti();
    } else if (selectedIdx >= 0) {
      statusEl.innerHTML = `❌ Raté ! La réponse était : <strong>${msg.correctText}</strong>`;
      statusEl.style.color = '#ef4444';
      document.getElementById('mobile-choice-'+selectedIdx)?.classList.add('wrong');
    } else {
      statusEl.innerHTML = `⏱️ Temps écoulé ! Réponse : <strong>${msg.correctText}</strong>`;
      statusEl.style.color = '#f59e0b';
    }
    statusEl.style.display = 'block';
  }
}

// =====================
// BLIND TEST
// =====================
function showBlindScreen(state) {
  currentAnswered = false;
  document.getElementById('blind-answer-input').value = '';
  document.getElementById('blind-submit-btn').disabled = false;
  document.getElementById('blind-reveal-area').style.display = 'none';
  document.getElementById('blind-sub').textContent = `Musique ${state.questionIndex+1} / ${state.total}`;
  updateAllScores();
  showScreen('screen-blind');
}

function submitBlindAnswer() {
  if (currentAnswered) return;
  const answer = document.getElementById('blind-answer-input').value.trim();
  if (!answer) { alert('Entrez votre réponse !'); return; }
  currentAnswered = true;
  document.getElementById('blind-submit-btn').disabled = true;
  document.getElementById('blind-submit-btn').textContent = '✅ Réponse envoyée !';
  if (bc) bc.postMessage({ type: 'blind_answer', name: playerName, answer });
}

function showBlindReveal(correctAnswer) {
  document.getElementById('blind-correct-answer').textContent = correctAnswer;
  document.getElementById('blind-reveal-area').style.display = 'block';
  if (currentAnswered) {
    const typed = document.getElementById('blind-answer-input').value.trim().toLowerCase();
    const correct = correctAnswer.toLowerCase();
    if (correct.includes(typed) || typed.includes(correct.split(' ')[0])) {
      addLocalScore(200);
      document.getElementById('blind-submit-btn').textContent = '🎉 Bonne réponse ! +200 pts';
      document.getElementById('blind-submit-btn').style.background = 'linear-gradient(135deg,#059669,#10b981)';
    }
  }
}

// =====================
// PRIX
// =====================
function showPrixScreen(state) {
  currentAnswered = false;
  document.getElementById('prix-item-name').textContent = state.itemName || 'Chargement…';
  document.getElementById('prix-hint').textContent = state.hint ? `💡 ${state.hint}` : '';
  document.getElementById('prix-estimate').value = '';
  document.getElementById('prix-submit-btn').disabled = false;
  document.getElementById('prix-submit-btn').textContent = '✅ Envoyer mon estimation';
  document.getElementById('prix-submit-btn').style.background = '';
  document.getElementById('prix-reveal-area').style.display = 'none';
  updateAllScores();
  showScreen('screen-prix');
}

function submitPriceAnswer() {
  if (currentAnswered) return;
  const val = document.getElementById('prix-estimate').value;
  if (!val || isNaN(parseFloat(val))) { alert('Entrez un montant valide !'); return; }
  currentAnswered = true;
  document.getElementById('prix-submit-btn').disabled = true;
  document.getElementById('prix-submit-btn').textContent = '⏳ Estimation envoyée !';
  if (bc) bc.postMessage({ type: 'price_answer', name: playerName, value: parseFloat(val) });
}

function showPriceReveal(price) {
  document.getElementById('prix-real-price').textContent = price;
  document.getElementById('prix-reveal-area').style.display = 'block';
  document.getElementById('prix-winner-msg').textContent = '';
  updateAllScores();
}

function showPriceWinner(name, price) {
  const msgEl = document.getElementById('prix-winner-msg');
  if (msgEl) {
    if (name === playerName) {
      msgEl.textContent = '🏆 Vous avez deviné le plus proche ! +300 pts !';
      msgEl.style.color = '#059669';
      addLocalScore(300);
      launchConfetti();
    } else {
      msgEl.textContent = `🥈 ${name} était le·la plus proche !`;
      msgEl.style.color = '#7c3aed';
    }
  }
}

// =====================
// LEADERBOARD
// =====================
function showLeaderboard(sorted) {
  const medals = ['🥇','🥈','🥉'];
  const list = document.getElementById('leaderboard-list');
  list.innerHTML = sorted.map(([n,pts],i) => `
    <div class="lb-row ${n===playerName?'me':''}">
      <span>${medals[i]||`${i+1}.`} ${n} ${n===playerName?'<span style="font-size:11px;color:#a855f7;">(vous)</span>':''}</span>
      <span class="lb-pts">${pts} pts</span>
    </div>`).join('') || '<div style="text-align:center;color:var(--muted);padding:16px;">Aucun score.</div>';
  showScreen('screen-leaderboard');
  launchConfetti();
}

// =====================
// SCORE
// =====================
function addLocalScore(pts) {
  myScore += pts;
  updateAllScores();
}

function updateAllScores() {
  const txt = `⭐ ${myScore} point${myScore>1?'s':''}`;
  ['my-score-wait','q-score-chip','blind-score-chip','prix-score-chip','result-score-chip'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.textContent = txt;
  });
}

// =====================
// CONFETTI
// =====================
function launchConfetti() {
  const container = document.getElementById('confetti-container');
  const colors = ['#ec4899','#a855f7','#fbbf24','#6ee7b7','#f9a8d4','#3b82f6'];
  for (let i = 0; i < 35; i++) {
    const el = document.createElement('div');
    el.className = 'confetto';
    el.style.left = Math.random()*100 + 'vw';
    el.style.background = colors[Math.floor(Math.random()*colors.length)];
    el.style.animationDelay = Math.random()*1.5 + 's';
    el.style.animationDuration = (2+Math.random()*2) + 's';
    container.appendChild(el);
    setTimeout(() => el.remove(), 4000);
  }
}
</script>
</body>
</html><!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>🍼 Baby Shower Quiz — Joueur</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --rose: #ec4899;
    --lav: #a855f7;
    --mint: #059669;
    --gold: #f59e0b;
    --text: #1e1b4b;
    --muted: #6b6a8a;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body {
    font-family: 'Nunito', sans-serif;
    background: linear-gradient(160deg, #fdf2f8 0%, #f0fdf4 50%, #ede9fe 100%);
    min-height: 100dvh;
    color: var(--text);
    overflow-x: hidden;
  }

  /* SCREENS */
  .screen { display: none; min-height: 100dvh; flex-direction: column; align-items: center; padding: 24px 20px; }
  .screen.active { display: flex; }

  /* HEADER */
  .mobile-header {
    width: 100%;
    background: linear-gradient(90deg, #ec4899, #a855f7);
    color: white;
    text-align: center;
    padding: 16px 20px;
    border-radius: 0 0 24px 24px;
    margin-bottom: 24px;
    box-shadow: 0 4px 20px rgba(236,72,153,0.3);
  }
  .mobile-header h1 { font-family: 'Playfair Display', serif; font-size: 22px; }
  .mobile-header .sub { font-size: 13px; opacity: 0.85; margin-top: 4px; }

  /* NAME ENTRY */
  .name-card {
    background: white;
    border-radius: 24px;
    padding: 28px 24px;
    width: 100%;
    max-width: 380px;
    box-shadow: 0 8px 32px rgba(168,85,247,0.12);
    text-align: center;
    border: 2.5px solid #f3e8ff;
  }
  .name-card .emoji { font-size: 52px; margin-bottom: 14px; }
  .name-card h2 { font-family: 'Playfair Display', serif; font-size: 24px; margin-bottom: 8px; }
  .name-card p { color: var(--muted); font-size: 14px; margin-bottom: 20px; }
  
  .big-input {
    width: 100%;
    padding: 16px 18px;
    border-radius: 16px;
    border: 2.5px solid #f3e8ff;
    font-family: 'Nunito', sans-serif;
    font-size: 18px;
    font-weight: 700;
    text-align: center;
    outline: none;
    color: var(--text);
    transition: border 0.2s;
    margin-bottom: 16px;
  }
  .big-input:focus { border-color: #a855f7; }

  /* BUTTONS */
  .btn {
    width: 100%;
    padding: 16px 20px;
    border-radius: 16px;
    border: none;
    font-family: 'Nunito', sans-serif;
    font-weight: 800;
    font-size: 17px;
    cursor: pointer;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    touch-action: manipulation;
  }
  .btn:active { transform: scale(0.97); }
  .btn-primary { background: linear-gradient(135deg, #ec4899, #a855f7); color: white; box-shadow: 0 4px 16px rgba(236,72,153,0.35); }
  .btn-success { background: linear-gradient(135deg, #059669, #10b981); color: white; box-shadow: 0 4px 16px rgba(5,150,105,0.3); }
  .btn-light { background: white; color: var(--text); border: 2.5px solid #f3e8ff; }
  .btn-answer {
    padding: 18px 16px;
    border-radius: 18px;
    border: 2.5px solid #f3e8ff;
    background: white;
    font-family: 'Nunito', sans-serif;
    font-size: 15px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.2s;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 12px;
    touch-action: manipulation;
    color: var(--text);
    width: 100%;
    margin-bottom: 12px;
  }
  .btn-answer:active { transform: scale(0.98); }
  .btn-answer .letter {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: #f3e8ff;
    color: #7c3aed;
    display: flex; align-items: center; justify-content: center;
    font-weight: 900; font-size: 16px;
    flex-shrink: 0;
  }
  .btn-answer.selected { border-color: #a855f7; background: #fdf4ff; }
  .btn-answer.selected .letter { background: #a855f7; color: white; }
  .btn-answer.correct { border-color: #059669; background: #f0fdf4; }
  .btn-answer.correct .letter { background: #059669; color: white; }
  .btn-answer.wrong { border-color: #ef4444; background: #fef2f2; }
  .btn-answer.wrong .letter { background: #ef4444; color: white; }

  /* WAITING */
  .waiting-card {
    background: white;
    border-radius: 24px;
    padding: 32px 24px;
    text-align: center;
    border: 2.5px solid #f3e8ff;
    width: 100%;
    max-width: 380px;
    box-shadow: 0 8px 32px rgba(168,85,247,0.1);
  }
  .waiting-card .emoji { font-size: 50px; margin-bottom: 14px; }
  .spin-emoji { animation: spin 2s linear infinite; display: inline-block; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .bounce-emoji { animation: bounce 1s ease-in-out infinite; display: inline-block; }
  @keyframes bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }

  /* QUESTION CARD */
  .q-card {
    background: white;
    border-radius: 24px;
    padding: 24px 20px;
    width: 100%;
    max-width: 420px;
    box-shadow: 0 8px 32px rgba(168,85,247,0.12);
    border: 2.5px solid #f9a8d4;
    animation: slideIn 0.35s ease;
  }
  @keyframes slideIn { from{opacity:0;transform:translateY(16px)} to{opacity:1;transform:translateY(0)} }

  .q-badge {
    display: inline-block;
    background: linear-gradient(90deg, #ec4899, #a855f7);
    color: white;
    border-radius: 20px;
    padding: 4px 14px;
    font-size: 12px;
    font-weight: 700;
    margin-bottom: 12px;
  }
  .q-text {
    font-family: 'Playfair Display', serif;
    font-size: 19px;
    line-height: 1.45;
    color: var(--text);
    margin-bottom: 20px;
  }

  .timer-bar-wrap { height: 6px; background: #f3e8ff; border-radius: 6px; margin-bottom: 20px; overflow: hidden; }
  .timer-bar { height: 100%; background: linear-gradient(90deg,#ec4899,#a855f7); border-radius: 6px; transition: width 1s linear; }

  /* SCORE DISPLAY */
  .score-chip {
    background: linear-gradient(135deg,#fbbf24,#f59e0b);
    color: #78350f;
    border-radius: 20px;
    padding: 6px 18px;
    font-size: 15px;
    font-weight: 900;
    display: inline-block;
  }

  /* TEXT ANSWER */
  .text-answer-area {
    width: 100%;
    padding: 14px 16px;
    border-radius: 16px;
    border: 2.5px solid #f3e8ff;
    font-family: 'Nunito', sans-serif;
    font-size: 16px;
    font-weight: 700;
    outline: none;
    color: var(--text);
    margin-bottom: 12px;
    transition: border 0.2s;
    resize: none;
  }
  .text-answer-area:focus { border-color: #a855f7; }

  /* RESULT SCREEN */
  .result-card {
    background: white;
    border-radius: 24px;
    padding: 28px 24px;
    text-align: center;
    border: 2.5px solid #f3e8ff;
    width: 100%;
    max-width: 380px;
    animation: slideIn 0.4s ease;
  }
  .result-correct { border-color: #059669; background: #f0fdf4; }
  .result-wrong { border-color: #ef4444; background: #fef2f2; }

  /* PRICE GAME */
  .price-slider-wrap { padding: 10px 0; }
  .price-item-name {
    font-family: 'Playfair Display', serif;
    font-size: 20px;
    color: var(--text);
    text-align: center;
    margin-bottom: 16px;
    line-height: 1.4;
  }
  .price-estimate-input {
    width: 100%;
    padding: 18px;
    border-radius: 18px;
    border: 2.5px solid #f3e8ff;
    font-size: 28px;
    font-weight: 900;
    text-align: center;
    font-family: 'Nunito', sans-serif;
    outline: none;
    color: var(--mint);
    transition: border 0.2s;
    margin-bottom: 8px;
  }
  .price-estimate-input:focus { border-color: #059669; }

  /* LEADERBOARD */
  .lb-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    border-bottom: 1.5px solid #f3e8ff;
    font-size: 15px;
    font-weight: 700;
  }
  .lb-row.me { background: #fdf4ff; border-radius: 12px; border-color: #a855f7; }
  .lb-pts { color: #7c3aed; font-size: 17px; }

  /* STATUS */
  .status-pill {
    background: rgba(255,255,255,0.8);
    border: 1.5px solid #f3e8ff;
    border-radius: 20px;
    padding: 5px 14px;
    font-size: 12px;
    font-weight: 700;
    color: var(--muted);
    display: flex;
    align-items: center;
    gap: 6px;
    margin-bottom: 16px;
  }
  .dot-live { width: 7px; height: 7px; border-radius: 50%; background: #059669; animation: blink 1.5s infinite; }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0.3} }

  .confetti-container { position: fixed; inset: 0; pointer-events: none; z-index: 200; overflow: hidden; }
  .confetto { position: absolute; width: 8px; height: 8px; opacity: 0; animation: fall 3s ease-out forwards; border-radius: 2px; }
  @keyframes fall { 0%{opacity:1;transform:translateY(-20px) rotate(0)} 100%{opacity:0;transform:translateY(100vh) rotate(720deg)} }

  .connection-form { width: 100%; max-width: 380px; }
</style>
</head>
<body>

<div class="confetti-container" id="confetti-container"></div>

<!-- SCREEN: JOIN -->
<div class="screen active" id="screen-join">
  <div class="mobile-header">
    <h1>🍼 Baby Shower Quiz</h1>
    <div class="sub">Rejoignez le jeu !</div>
  </div>
  <div class="connection-form">
    <div class="name-card">
      <div class="emoji">👋</div>
      <h2>Votre prénom</h2>
      <p>Entrez votre prénom pour rejoindre la partie</p>
      <input class="big-input" id="player-name-input" type="text" placeholder="Marie…" maxlength="20" autocomplete="off">
      <div class="form-group" style="margin-bottom:14px;">
        <label style="font-size:12px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:0.5px;display:block;margin-bottom:6px;">Code de session</label>
        <input class="big-input" id="session-code-input" type="text" placeholder="XXXXX" maxlength="8" style="font-size:22px;letter-spacing:4px;text-transform:uppercase;" autocomplete="off">
      </div>
      <button class="btn btn-primary" onclick="joinGame()">🎉 Rejoindre !</button>
    </div>
  </div>
</div>

<!-- SCREEN: WAITING -->
<div class="screen" id="screen-waiting">
  <div class="mobile-header">
    <h1>🍼 Baby Shower Quiz</h1>
    <div class="sub" id="waiting-name-display">Bienvenue !</div>
  </div>
  <div class="waiting-card">
    <div class="emoji bounce-emoji">🍼</div>
    <h2 style="font-family:'Playfair Display',serif;margin-bottom:8px;">Vous êtes connecté·e !</h2>
    <p style="color:var(--muted);font-size:14px;margin-bottom:20px;">En attente du démarrage par l'animateur…</p>
    <div class="score-chip" id="my-score-wait">⭐ 0 point</div>
    <div style="margin-top:20px;font-size:13px;color:var(--muted);">
      <span class="spin-emoji">⏳</span> L'animateur lancera le jeu depuis la TV
    </div>
  </div>
</div>

<!-- SCREEN: QUESTION (QUIZ) -->
<div class="screen" id="screen-question">
  <div class="mobile-header" id="q-screen-header">
    <h1 id="q-screen-title">Question</h1>
    <div class="sub" id="q-screen-sub">En jeu</div>
  </div>
  <div class="status-pill"><div class="dot-live"></div><span id="q-progress-text">Q1 / 4</span></div>
  <div class="q-card" style="max-width:420px;width:100%;">
    <div class="q-badge" id="q-badge-label">Quiz</div>
    <div class="timer-bar-wrap"><div class="timer-bar" id="mobile-timer-bar" style="width:100%"></div></div>
    <div class="q-text" id="q-text-display">Chargement…</div>
    <div id="choices-container"></div>
    <div id="answer-status" style="display:none;text-align:center;padding:12px 0;font-weight:700;font-size:15px;"></div>
  </div>
  <div style="margin-top:16px;" id="score-display-q">
    <div class="score-chip" id="q-score-chip">⭐ 0 point</div>
  </div>
</div>

<!-- SCREEN: BLIND TEST -->
<div class="screen" id="screen-blind">
  <div class="mobile-header">
    <h1>🎵 Blind Test Disney</h1>
    <div class="sub" id="blind-sub">Musique en cours…</div>
  </div>
  <div class="q-card" style="max-width:420px;width:100%;">
    <div style="text-align:center;font-size:44px;margin:10px 0;animation:bounce 1s infinite ease-in-out;">🎵</div>
    <div class="q-text" style="text-align:center;">Quel est ce titre Disney ?</div>
    <textarea class="text-answer-area" id="blind-answer-input" rows="3" placeholder="Entrez votre réponse…"></textarea>
    <button class="btn btn-primary" id="blind-submit-btn" onclick="submitBlindAnswer()">✅ Valider ma réponse</button>
    <div id="blind-reveal-area" style="display:none;margin-top:16px;background:#f0fdf4;border-radius:16px;padding:16px;text-align:center;">
      <div style="font-size:13px;color:#059669;font-weight:700;margin-bottom:4px;">✅ La réponse était :</div>
      <div style="font-family:'Playfair Display',serif;font-size:20px;color:#1e1b4b;" id="blind-correct-answer"></div>
    </div>
  </div>
  <div style="margin-top:16px;"><div class="score-chip" id="blind-score-chip">⭐ 0 point</div></div>
</div>

<!-- SCREEN: PRIX -->
<div class="screen" id="screen-prix">
  <div class="mobile-header">
    <h1>💰 Le Juste Prix</h1>
    <div class="sub">Estimez le prix de l'objet !</div>
  </div>
  <div class="q-card" style="max-width:420px;width:100%;">
    <div style="font-size:44px;text-align:center;margin:10px 0;">🛒</div>
    <div class="price-item-name" id="prix-item-name">Chargement…</div>
    <p id="prix-hint" style="text-align:center;color:#7c3aed;font-style:italic;font-size:14px;margin-bottom:16px;"></p>
    <input type="number" class="price-estimate-input" id="prix-estimate" placeholder="0.00" min="0" step="0.01">
    <div style="text-align:center;color:var(--muted);font-size:12px;margin-bottom:16px;">Entrez votre estimation en euros (€)</div>
    <button class="btn btn-success" id="prix-submit-btn" onclick="submitPriceAnswer()">✅ Envoyer mon estimation</button>
    <div id="prix-reveal-area" style="display:none;margin-top:16px;border-radius:16px;padding:20px;text-align:center;background:#f0fdf4;">
      <div style="font-size:13px;font-weight:700;color:#059669;margin-bottom:6px;">💰 Le vrai prix :</div>
      <div style="font-size:34px;font-weight:900;color:#059669;font-family:'Playfair Display',serif;" id="prix-real-price"></div>
      <div style="font-size:13px;color:var(--muted);margin-top:8px;" id="prix-winner-msg"></div>
    </div>
  </div>
  <div style="margin-top:16px;"><div class="score-chip" id="prix-score-chip">⭐ 0 point</div></div>
</div>

<!-- SCREEN: LEADERBOARD -->
<div class="screen" id="screen-leaderboard">
  <div class="mobile-header">
    <h1>🏆 Classement</h1>
    <div class="sub">Résultats finaux</div>
  </div>
  <div style="width:100%;max-width:380px;">
    <div style="background:white;border-radius:24px;padding:20px;border:2.5px solid #f3e8ff;margin-bottom:20px;">
      <div id="leaderboard-list"></div>
    </div>
    <button class="btn btn-light" onclick="showScreen('screen-waiting')">🔄 Nouvelle partie</button>
  </div>
</div>

<!-- SCREEN: ANSWERED -->
<div class="screen" id="screen-answered">
  <div class="mobile-header">
    <h1>🍼 Baby Shower Quiz</h1>
    <div class="sub">Réponse envoyée !</div>
  </div>
  <div class="result-card" id="result-card-display">
    <div style="font-size:50px;margin-bottom:14px;" id="result-emoji">⏳</div>
    <h2 style="font-family:'Playfair Display',serif;font-size:22px;margin-bottom:8px;" id="result-title">En attente…</h2>
    <p style="color:var(--muted);font-size:14px;margin-bottom:20px;" id="result-sub">L'animateur va révéler la réponse</p>
    <div class="score-chip" id="result-score-chip">⭐ 0 point</div>
  </div>
</div>

<script>
// =====================
// STATE
// =====================
let playerName = '';
let sessionCode = '';
let myScore = 0;
let bc = null;
let currentAnswered = false;
let currentGameType = '';
let currentQuestionIndex = -1;

// Get session code from URL
const urlParams = new URLSearchParams(window.location.search);
if (urlParams.get('code')) {
  document.getElementById('session-code-input').value = urlParams.get('code');
}

// =====================
// SCREENS
// =====================
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

// =====================
// JOIN
// =====================
function joinGame() {
  const name = document.getElementById('player-name-input').value.trim();
  const code = document.getElementById('session-code-input').value.trim().toUpperCase();
  if (!name) { alert('Entrez votre prénom !'); return; }
  if (!code) { alert('Entrez le code de session affiché sur la TV !'); return; }
  playerName = name;
  sessionCode = code;

  bc = new BroadcastChannel('baby_shower_' + sessionCode);
  bc.onmessage = handleMessage;
  bc.postMessage({ type: 'join', name: playerName });
  bc.postMessage({ type: 'ping' });

  document.getElementById('waiting-name-display').textContent = `Bienvenue ${playerName} ! 🎉`;
  showScreen('screen-waiting');

  // Retry ping
  setTimeout(() => { if (bc) bc.postMessage({ type: 'ping' }); }, 1500);
}

// =====================
// MESSAGE HANDLER
// =====================
function handleMessage(e) {
  const msg = e.data;

  if (msg.type === 'state') {
    handleState(msg);
  } else if (msg.type === 'reveal') {
    handleReveal(msg);
  } else if (msg.type === 'blind_reveal') {
    showBlindReveal(msg.answer);
  } else if (msg.type === 'price_reveal') {
    showPriceReveal(msg.price);
  } else if (msg.type === 'price_winner') {
    showPriceWinner(msg.name, msg.price);
  } else if (msg.type === 'score_update') {
    if (msg.name === playerName) {
      myScore = msg.score;
      updateAllScores();
    }
  } else if (msg.type === 'leaderboard') {
    showLeaderboard(msg.scores);
  }
}

function handleState(state) {
  updateAllScores();
  const gt = state.gameType;
  currentGameType = gt;
  const qIdx = state.questionIndex;

  if (gt === 'quiz') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showQuizQuestion(state);
    }
  } else if (gt === 'celebrites') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showCelebQuestion(state);
    }
  } else if (gt === 'blindtest') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showBlindScreen(state);
    }
  } else if (gt === 'prix') {
    if (qIdx !== currentQuestionIndex) {
      currentQuestionIndex = qIdx;
      currentAnswered = false;
      showPrixScreen(state);
    }
  }
}

// =====================
// QUIZ / CELEB
// =====================
function showQuizQuestion(state) {
  const titles = { maternite: '🤰 Maternité', parents: '👶 Parents' };
  document.getElementById('q-screen-title').textContent = titles[state.gameKey] || 'Quiz';
  document.getElementById('q-screen-sub').textContent = `${playerName} — Q${state.questionIndex+1}/${state.total}`;
  document.getElementById('q-badge-label').textContent = titles[state.gameKey] || 'Quiz';
  document.getElementById('q-progress-text').textContent = `Q${state.questionIndex+1} / ${state.total}`;
  document.getElementById('q-text-display').textContent = state.questionText;
  document.getElementById('answer-status').style.display = 'none';
  renderChoices(state.choices, 'quiz', state.questionIndex, state.gameKey);
  startMobileTimer(30);
  showScreen('screen-question');
}

function showCelebQuestion(state) {
  document.getElementById('q-screen-title').textContent = '⭐ Célébrités';
  document.getElementById('q-screen-sub').textContent = `${playerName}`;
  document.getElementById('q-badge-label').textContent = '⭐ Célébrités Bébés';
  document.getElementById('q-text-display').textContent = state.hint || 'Qui est ce bébé célèbre ?';
  document.getElementById('answer-status').style.display = 'none';
  renderChoices(state.choices, 'celebrites', state.questionIndex, null);
  startMobileTimer(30);
  showScreen('screen-question');
}

function renderChoices(choices, gameType, qIdx, gameKey) {
  const letters = ['A','B','C','D'];
  const container = document.getElementById('choices-container');
  container.innerHTML = (choices||[]).map((c,i) => `
    <button class="btn-answer" id="mobile-choice-${i}" onclick="selectAnswer(${i},'${gameType}','${gameKey}',${qIdx})">
      <div class="letter">${letters[i]}</div>
      <span>${c}</span>
    </button>`).join('');
}

let mobileTimerInterval = null;
let mobileTimerVal = 30;
function startMobileTimer(secs) {
  clearInterval(mobileTimerInterval);
  mobileTimerVal = secs;
  const bar = document.getElementById('mobile-timer-bar');
  mobileTimerInterval = setInterval(() => {
    mobileTimerVal--;
    if (bar) bar.style.width = (mobileTimerVal/secs*100)+'%';
    if (mobileTimerVal <= 0) clearInterval(mobileTimerInterval);
  }, 1000);
}

function selectAnswer(idx, gameType, gameKey, qIdx) {
  if (currentAnswered) return;
  currentAnswered = true;
  clearInterval(mobileTimerInterval);

  document.querySelectorAll('.btn-answer').forEach(b => b.style.opacity = '0.5');
  const selected = document.getElementById('mobile-choice-'+idx);
  if (selected) { selected.classList.add('selected'); selected.style.opacity = '1'; }

  if (bc) bc.postMessage({ type: 'answer', name: playerName, answer: idx, questionId: qIdx, gameKey });

  const statusEl = document.getElementById('answer-status');
  statusEl.style.display = 'block';
  statusEl.innerHTML = '⏳ Réponse envoyée ! En attente de la révélation…';
  statusEl.style.color = '#7c3aed';
}

function handleReveal(msg) {
  const correct = msg.correctIndex;
  const currentScreen = document.querySelector('.screen.active');
  if (!currentScreen) return;

  document.querySelectorAll('.btn-answer').forEach((btn, i) => {
    btn.style.opacity = '1';
    if (i === correct) btn.classList.add('correct');
    else btn.classList.remove('selected');
  });

  // Check if player was correct
  const selectedBtn = document.querySelector('.btn-answer.selected');
  const selectedIdx = selectedBtn ? parseInt(selectedBtn.id.replace('mobile-choice-','')) : -1;

  const statusEl = document.getElementById('answer-status');
  if (statusEl) {
    if (selectedIdx === correct) {
      statusEl.innerHTML = '🎉 Bonne réponse ! +points !';
      statusEl.style.color = '#059669';
      launchConfetti();
    } else if (selectedIdx >= 0) {
      statusEl.innerHTML = `❌ Raté ! La réponse était : <strong>${msg.correctText}</strong>`;
      statusEl.style.color = '#ef4444';
      document.getElementById('mobile-choice-'+selectedIdx)?.classList.add('wrong');
    } else {
      statusEl.innerHTML = `⏱️ Temps écoulé ! Réponse : <strong>${msg.correctText}</strong>`;
      statusEl.style.color = '#f59e0b';
    }
    statusEl.style.display = 'block';
  }
}

// =====================
// BLIND TEST
// =====================
function showBlindScreen(state) {
  currentAnswered = false;
  document.getElementById('blind-answer-input').value = '';
  document.getElementById('blind-submit-btn').disabled = false;
  document.getElementById('blind-reveal-area').style.display = 'none';
  document.getElementById('blind-sub').textContent = `Musique ${state.questionIndex+1} / ${state.total}`;
  updateAllScores();
  showScreen('screen-blind');
}

function submitBlindAnswer() {
  if (currentAnswered) return;
  const answer = document.getElementById('blind-answer-input').value.trim();
  if (!answer) { alert('Entrez votre réponse !'); return; }
  currentAnswered = true;
  document.getElementById('blind-submit-btn').disabled = true;
  document.getElementById('blind-submit-btn').textContent = '✅ Réponse envoyée !';
  if (bc) bc.postMessage({ type: 'blind_answer', name: playerName, answer });
}

function showBlindReveal(correctAnswer) {
  document.getElementById('blind-correct-answer').textContent = correctAnswer;
  document.getElementById('blind-reveal-area').style.display = 'block';
  if (currentAnswered) {
    const typed = document.getElementById('blind-answer-input').value.trim().toLowerCase();
    const correct = correctAnswer.toLowerCase();
    if (correct.includes(typed) || typed.includes(correct.split(' ')[0])) {
      addLocalScore(200);
      document.getElementById('blind-submit-btn').textContent = '🎉 Bonne réponse ! +200 pts';
      document.getElementById('blind-submit-btn').style.background = 'linear-gradient(135deg,#059669,#10b981)';
    }
  }
}

// =====================
// PRIX
// =====================
function showPrixScreen(state) {
  currentAnswered = false;
  document.getElementById('prix-item-name').textContent = state.itemName || 'Chargement…';
  document.getElementById('prix-hint').textContent = state.hint ? `💡 ${state.hint}` : '';
  document.getElementById('prix-estimate').value = '';
  document.getElementById('prix-submit-btn').disabled = false;
  document.getElementById('prix-submit-btn').textContent = '✅ Envoyer mon estimation';
  document.getElementById('prix-submit-btn').style.background = '';
  document.getElementById('prix-reveal-area').style.display = 'none';
  updateAllScores();
  showScreen('screen-prix');
}

function submitPriceAnswer() {
  if (currentAnswered) return;
  const val = document.getElementById('prix-estimate').value;
  if (!val || isNaN(parseFloat(val))) { alert('Entrez un montant valide !'); return; }
  currentAnswered = true;
  document.getElementById('prix-submit-btn').disabled = true;
  document.getElementById('prix-submit-btn').textContent = '⏳ Estimation envoyée !';
  if (bc) bc.postMessage({ type: 'price_answer', name: playerName, value: parseFloat(val) });
}

function showPriceReveal(price) {
  document.getElementById('prix-real-price').textContent = price;
  document.getElementById('prix-reveal-area').style.display = 'block';
  document.getElementById('prix-winner-msg').textContent = '';
  updateAllScores();
}

function showPriceWinner(name, price) {
  const msgEl = document.getElementById('prix-winner-msg');
  if (msgEl) {
    if (name === playerName) {
      msgEl.textContent = '🏆 Vous avez deviné le plus proche ! +300 pts !';
      msgEl.style.color = '#059669';
      addLocalScore(300);
      launchConfetti();
    } else {
      msgEl.textContent = `🥈 ${name} était le·la plus proche !`;
      msgEl.style.color = '#7c3aed';
    }
  }
}

// =====================
// LEADERBOARD
// =====================
function showLeaderboard(sorted) {
  const medals = ['🥇','🥈','🥉'];
  const list = document.getElementById('leaderboard-list');
  list.innerHTML = sorted.map(([n,pts],i) => `
    <div class="lb-row ${n===playerName?'me':''}">
      <span>${medals[i]||`${i+1}.`} ${n} ${n===playerName?'<span style="font-size:11px;color:#a855f7;">(vous)</span>':''}</span>
      <span class="lb-pts">${pts} pts</span>
    </div>`).join('') || '<div style="text-align:center;color:var(--muted);padding:16px;">Aucun score.</div>';
  showScreen('screen-leaderboard');
  launchConfetti();
}

// =====================
// SCORE
// =====================
function addLocalScore(pts) {
  myScore += pts;
  updateAllScores();
}

function updateAllScores() {
  const txt = `⭐ ${myScore} point${myScore>1?'s':''}`;
  ['my-score-wait','q-score-chip','blind-score-chip','prix-score-chip','result-score-chip'].forEach(id => {
    const el = document.getElementById(id);
    if (el) el.textContent = txt;
  });
}

// =====================
// CONFETTI
// =====================
function launchConfetti() {
  const container = document.getElementById('confetti-container');
  const colors = ['#ec4899','#a855f7','#fbbf24','#6ee7b7','#f9a8d4','#3b82f6'];
  for (let i = 0; i < 35; i++) {
    const el = document.createElement('div');
    el.className = 'confetto';
    el.style.left = Math.random()*100 + 'vw';
    el.style.background = colors[Math.floor(Math.random()*colors.length)];
    el.style.animationDelay = Math.random()*1.5 + 's';
    el.style.animationDuration = (2+Math.random()*2) + 's';
    container.appendChild(el);
    setTimeout(() => el.remove(), 4000);
  }
}
</script>
</body>
</html>
