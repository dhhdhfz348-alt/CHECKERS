

Create updated index.html with all requested features
bash

cat > /mnt/user-data/outputs/index.html << 'HTMLEOF'
<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Checkers</title>
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#1a1a2e;color:#fff;min-height:100vh}
#app{max-width:400px;margin:0 auto;padding:8px}
.screen{display:none}.screen.active{display:block}
.lobby-header{text-align:center;padding:20px 0 14px}
.owner-name{font-size:22px;font-weight:700;margin-bottom:2px;background:linear-gradient(90deg,#a78bfa,#60a5fa);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.lobby-header p{font-size:13px;color:#aaa}
.music-bar{display:flex;align-items:center;gap:8px;background:#16213e;border-radius:10px;padding:10px 14px;margin-bottom:12px}
.music-icon{font-size:18px;animation:spin 3s linear infinite}
@keyframes spin{0%{transform:rotate(0)}100%{transform:rotate(360deg)}}
.music-text{flex:1;font-size:13px;color:#a78bfa}
.music-toggle{background:none;border:none;color:#a78bfa;font-size:18px;cursor:pointer}
.player-info{background:#16213e;border-radius:12px;padding:12px 16px;margin-bottom:12px;display:flex;align-items:center;gap:12px}
.avatar{width:44px;height:44px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:15px;flex-shrink:0}
.av1{background:linear-gradient(135deg,#6366f1,#8b5cf6);color:#fff}
.player-meta{flex:1}
.player-meta strong{font-size:15px;display:block;margin-bottom:2px}
.player-meta span{font-size:12px;color:#aaa}
.badge{background:#1b5e20;color:#69f0ae;font-size:11px;padding:3px 8px;border-radius:20px}
.stats-row{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;margin-bottom:12px}
.stat-card{background:#16213e;border-radius:10px;padding:10px;text-align:center}
.stat-card .num{font-size:22px;font-weight:700}
.stat-card .lbl{font-size:11px;color:#aaa;margin-top:2px}
.online-list{margin-bottom:12px}
.online-list h3{font-size:13px;color:#aaa;margin-bottom:8px}
.online-item{display:flex;align-items:center;gap:10px;padding:10px 12px;background:#16213e;border-radius:10px;margin-bottom:6px}
.dot{width:8px;height:8px;border-radius:50%;background:#4caf50;flex-shrink:0;animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.4}}
.online-item .iname{flex:1;font-size:14px}
.online-item .lvl{font-size:12px;color:#aaa}
.btn{display:block;width:100%;padding:12px;border-radius:10px;border:none;background:#0f3460;color:#fff;font-size:14px;cursor:pointer;font-family:inherit;transition:all 0.15s;text-align:center}
.btn:hover{background:#1a4a80;transform:scale(1.01)}
.btn.primary{background:linear-gradient(135deg,#e94560,#c73652);color:#fff}
.btn.sm{padding:6px 12px;font-size:12px;width:auto;border-radius:8px}
.btn.green-sm{padding:6px 12px;font-size:12px;width:auto;border-radius:8px;background:#1b5e20;color:#69f0ae;border:none;cursor:pointer;font-family:inherit}
.btn.red-sm{padding:6px 12px;font-size:12px;width:auto;border-radius:8px;background:#b71c1c;color:#ff8a80;border:none;cursor:pointer;font-family:inherit}
.btn-row{display:flex;gap:8px;margin-bottom:8px}
.btn-row .btn{flex:1}
.game-header{display:flex;align-items:center;gap:8px;margin-bottom:10px}
.turn-indicator{flex:1;text-align:center}
.turn-indicator .who{font-size:13px;font-weight:500}
.turn-indicator .timer{font-size:28px;font-weight:700;color:#e94560;font-variant-numeric:tabular-nums}
.scores{display:flex;gap:6px}
.score-box{background:#16213e;border-radius:8px;padding:6px 12px;text-align:center;min-width:52px}
.score-box .sc{font-size:18px;font-weight:700}
.score-box .sl{font-size:10px;color:#aaa}
.board-wrap{display:flex;justify-content:center;margin-bottom:10px}
canvas#board{border:3px solid #5D4037;border-radius:4px;cursor:pointer;touch-action:none}
.game-actions{display:flex;gap:8px;margin-bottom:8px}
.game-actions .btn{flex:1;padding:9px}
.motivate-bar{background:linear-gradient(135deg,#1b3a4b,#0f3460);border-radius:10px;padding:10px 14px;margin-bottom:8px;font-size:13px;color:#60a5fa;text-align:center;display:none;border:1px solid #1e40af}
.motivate-bar.show{display:block;animation:fadeIn 0.5s}
@keyframes fadeIn{from{opacity:0;transform:translateY(-5px)}to{opacity:1;transform:translateY(0)}}
.chat-area{background:#16213e;border-radius:12px;overflow:hidden;margin-bottom:8px}
.chat-msgs{height:90px;overflow-y:auto;padding:8px;font-size:13px}
.chat-msg{margin-bottom:5px}
.chat-msg .cn{font-weight:700;margin-right:4px}
.chat-msg.sys{color:#aaa;font-style:italic}
.stickers{display:flex;gap:6px;padding:6px 8px;border-top:1px solid #0f3460;overflow-x:auto}
.stk{font-size:22px;cursor:pointer;padding:2px;border-radius:4px;transition:transform 0.1s;flex-shrink:0}
.stk:hover{transform:scale(1.3)}
.chat-input-row{display:flex;gap:6px;padding:6px 8px;border-top:1px solid #0f3460}
.chat-input-row input{flex:1;padding:7px 10px;border:1px solid #0f3460;border-radius:8px;font-size:13px;background:#0f3460;color:#fff;font-family:inherit}
.chat-input-row input::placeholder{color:#aaa}
.chat-input-row button{padding:7px 12px;font-size:13px;border-radius:8px;border:none;background:#e94560;color:#fff;cursor:pointer}
.draw-offer{background:#1b3a1b;border:1px solid #2e7d32;border-radius:10px;padding:10px;margin-bottom:8px;font-size:13px;display:none;align-items:center;gap:8px}
.draw-offer.show{display:flex}
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.75);z-index:100;align-items:center;justify-content:center}
.modal-overlay.open{display:flex}
.modal{background:#16213e;border-radius:16px;padding:24px;max-width:280px;width:90%;text-align:center;border:1px solid #0f3460}
.modal h3{font-size:20px;font-weight:700;margin-bottom:8px}
.modal p{font-size:14px;color:#aaa;margin-bottom:16px}
.modal-btns{display:flex;gap:8px}
.back-btn{background:#16213e;color:#aaa;border:1px solid #0f3460;padding:7px 12px;border-radius:8px;cursor:pointer;font-size:13px}
</style>
</head>
<body>
<div id="app">
  <div id="lobby" class="screen active">
    <div class="lobby-header">
      <div class="owner-name">♟ Abdurahmon Tog'ajon</div>
      <p>Shashka o'yini — raqib toping yoki bot bilan o'ynang</p>
    </div>
    <div class="music-bar">
      <span class="music-icon">🎵</span>
      <span class="music-text">Xotirjam muhit...</span>
      <button class="music-toggle" onclick="toggleMusic()" id="music-btn">⏸</button>
    </div>
    <div class="player-info">
      <div class="avatar av1" id="my-avatar">A</div>
      <div class="player-meta">
        <strong id="my-name">Foydalanuvchi</strong>
        <span id="my-stats">Daraja 1 • 0 ball • 0 g'alaba</span>
      </div>
      <span class="badge">Onlayn</span>
    </div>
    <div class="stats-row">
      <div class="stat-card"><div class="num" style="color:#69f0ae" id="stat-win">0</div><div class="lbl">G'alaba</div></div>
      <div class="stat-card"><div class="num" style="color:#e94560" id="stat-lose">0</div><div class="lbl">Mag'lubiyat</div></div>
      <div class="stat-card"><div class="num" style="color:#60a5fa" id="stat-ball">0</div><div class="lbl">Ball</div></div>
    </div>
    <div class="online-list">
      <h3>Onlayn o'yinchilar</h3>
      <div class="online-item" id="fake-player">
        <div class="dot"></div>
        <div class="avatar" style="width:32px;height:32px;font-size:12px;background:#0f3460;color:#60a5fa">IQ</div>
        <span class="iname" id="fake-name">Iqror</span>
        <span class="lvl">Daraja 3</span>
        <button class="btn sm" onclick="startGame('Iqror')">Taklif</button>
      </div>
    </div>
    <div class="btn-row">
      <button class="btn primary" onclick="startGame('Iqror')">Tez o'yin</button>
      <button class="btn" onclick="startGame('Bot')">Bot bilan</button>
    </div>
  </div>

  <div id="game-screen" class="screen">
    <div class="game-header">
      <button class="back-btn" onclick="goLobby()">&#8592;</button>
      <div class="turn-indicator">
        <div class="who" id="turn-who">Sizning navbat</div>
        <div class="timer" id="timer">60</div>
      </div>
      <div class="scores">
        <div class="score-box"><div class="sc" id="sc1">12</div><div class="sl">Siz</div></div>
        <div class="score-box"><div class="sc" id="sc2">12</div><div class="sl" id="opp-name-sc">Raqib</div></div>
      </div>
    </div>
    <div class="motivate-bar" id="motivate-bar">💪 Zo'r yurish!</div>
    <div class="draw-offer" id="draw-offer">
      <span>🤝</span>
      <span style="flex:1">Raqib durang taklif qildi</span>
      <button class="green-sm" onclick="acceptDraw()">Qabul</button>
      <button class="btn sm" onclick="declineDraw()">Rad</button>
    </div>
    <div class="board-wrap">
      <canvas id="board" width="320" height="320"></canvas>
    </div>
    <div class="game-actions">
      <button class="btn" onclick="offerDraw()">🤝 Durang</button>
      <button class="btn red-sm" onclick="resign()">🏳 Taslim</button>
    </div>
    <div class="chat-area">
      <div class="chat-msgs" id="chat-msgs">
        <div class="chat-msg sys">O'yin boshlandi. Omad!</div>
      </div>
      <div class="stickers">
        <span class="stk" onclick="sendSticker('👍')">👍</span>
        <span class="stk" onclick="sendSticker('😎')">😎</span>
        <span class="stk" onclick="sendSticker('😂')">😂</span>
        <span class="stk" onclick="sendSticker('🤝')">🤝</span>
        <span class="stk" onclick="sendSticker('😤')">😤</span>
        <span class="stk" onclick="sendSticker('👑')">👑</span>
        <span class="stk" onclick="sendSticker('🔥')">🔥</span>
        <span class="stk" onclick="sendSticker('❤️')">❤️</span>
      </div>
      <div class="chat-input-row">
        <input type="text" id="chat-input" placeholder="Xabar yozing..." onkeydown="if(event.key==='Enter')sendChat()">
        <button onclick="sendChat()">&#9658;</button>
      </div>
    </div>
  </div>
</div>

<div class="modal-overlay" id="result-modal">
  <div class="modal">
    <h3 id="modal-title">G'alaba!</h3>
    <p id="modal-body">Siz g'olib bo'ldingiz!</p>
    <div class="modal-btns">
      <button class="btn" onclick="closeModal();goLobby()">Lobbyga</button>
      <button class="btn primary" onclick="closeModal();startGame(currentOpp)">Qayta</button>
    </div>
  </div>
</div>

<script>
// Telegram user
let myName='Foydalanuvchi', wins=0, losses=0, balls=0;
try{
  const tg=window.Telegram.WebApp;
  tg.ready(); tg.expand();
  if(tg.initDataUnsafe&&tg.initDataUnsafe.user){
    const u=tg.initDataUnsafe.user;
    myName=(u.first_name||'')+(u.last_name?' '+u.last_name:'');
    document.getElementById('my-name').textContent=myName||'Foydalanuvchi';
    const ini=(myName||'U').split(' ').map(x=>x[0]).join('').toUpperCase().slice(0,2);
    document.getElementById('my-avatar').textContent=ini;
  }
}catch(e){}

// Stats from localStorage
try{
  wins=parseInt(localStorage.getItem('w')||'0');
  losses=parseInt(localStorage.getItem('l')||'0');
  balls=parseInt(localStorage.getItem('b')||'0');
  updateStats();
}catch(e){}

function updateStats(){
  document.getElementById('stat-win').textContent=wins;
  document.getElementById('stat-lose').textContent=losses;
  document.getElementById('stat-ball').textContent=balls;
  const lvl=Math.floor(balls/50)+1;
  document.getElementById('my-stats').textContent=`Daraja ${lvl} • ${balls} ball • ${wins} g'alaba`;
}

function saveStats(){
  try{localStorage.setItem('w',wins);localStorage.setItem('l',losses);localStorage.setItem('b',balls);}catch(e){}
}

// Music (Web Audio API - ambient tones)
let audioCtx=null, musicOn=true, musicNodes=[];
function startMusic(type){
  stopMusic();
  try{
    audioCtx=new(window.AudioContext||window.webkitAudioContext)();
    if(type==='lobby'){
      // Xotirjam - slow sine waves
      [220,330,440].forEach((freq,i)=>{
        const osc=audioCtx.createOscillator();
        const gain=audioCtx.createGain();
        osc.type='sine'; osc.frequency.value=freq;
        gain.gain.value=0.04;
        osc.connect(gain); gain.connect(audioCtx.destination);
        osc.start(); musicNodes.push(osc,gain);
      });
    } else {
      // Motivatsion - slightly faster
      [261,329,392,523].forEach((freq,i)=>{
        const osc=audioCtx.createOscillator();
        const gain=audioCtx.createGain();
        osc.type='triangle'; osc.frequency.value=freq;
        gain.gain.value=0.03;
        osc.connect(gain); gain.connect(audioCtx.destination);
        osc.start(); musicNodes.push(osc,gain);
      });
    }
  }catch(e){}
}
function stopMusic(){
  try{musicNodes.forEach(n=>{try{n.stop?n.stop():n.disconnect();}catch(e){}});musicNodes=[];}catch(e){}
}
function toggleMusic(){
  musicOn=!musicOn;
  document.getElementById('music-btn').textContent=musicOn?'⏸':'▶';
  if(musicOn)startMusic('lobby'); else stopMusic();
}

// Motivate messages
const motivates=[
  "💪 Zo'r yurish! Davom eting!",
  "🔥 Ajoyib! Siz g'alaba sari ketyapsiz!",
  "🧠 Aqlli yurish! Professional!",
  "⚡ Zo'r! Raqib titrayapti!",
  "👑 Bu yurish shoh yurishiga o'xshaydi!",
  "🎯 Maqsadga qarab boring!",
  "💫 Siz bugun yengilmasssiz!"
];
let moveCount=0;
function checkMotivate(){
  moveCount++;
  if(moveCount%3===0){
    const bar=document.getElementById('motivate-bar');
    bar.textContent=motivates[Math.floor(Math.random()*motivates.length)];
    bar.classList.add('show');
    setTimeout(()=>bar.classList.remove('show'),3000);
  }
}

// Game logic
const SZ=40;
let board=[],selected=null,validMoves=[],currentTurn=1,pieces1=12,pieces2=12;
let timerVal=60,timerInterval=null,oppName='Raqib',currentOpp='Raqib';
let isBot=false,gameOver=false;

function initBoard(){
  board=[];
  for(let r=0;r<8;r++){
    board[r]=[];
    for(let c=0;c<8;c++){
      let p=0;
      if((r+c)%2===1){if(r<3)p=2;else if(r>4)p=1;}
      board[r][c]={piece:p,king:false};
    }
  }
  pieces1=12;pieces2=12;selected=null;validMoves=[];currentTurn=1;gameOver=false;moveCount=0;
  updateScores();
}

function drawBoard(){
  const cv=document.getElementById('board');
  const ctx=cv.getContext('2d');
  for(let r=0;r<8;r++){
    for(let c=0;c<8;c++){
      ctx.fillStyle=(r+c)%2===0?'#F5DEB3':'#8B4513';
      ctx.fillRect(c*SZ,r*SZ,SZ,SZ);
      if(selected&&selected[0]===r&&selected[1]===c){
        ctx.fillStyle='rgba(255,220,0,0.4)';
        ctx.fillRect(c*SZ,r*SZ,SZ,SZ);
      }
    }
  }
  for(const [mr,mc] of validMoves){
    ctx.fillStyle='rgba(0,230,100,0.45)';
    ctx.beginPath();ctx.arc(mc*SZ+SZ/2,mr*SZ+SZ/2,SZ/2-6,0,Math.PI*2);ctx.fill();
  }
  for(let r=0;r<8;r++){
    for(let c=0;c<8;c++){
      const {piece,king}=board[r][c];
      if(!piece)continue;
      ctx.beginPath();ctx.arc(c*SZ+SZ/2,r*SZ+SZ/2,SZ/2-5,0,Math.PI*2);
      ctx.fillStyle=piece===1?'#6366f1':'#e94560';ctx.fill();
      ctx.strokeStyle=piece===1?'#a78bfa':'#ff8a80';ctx.lineWidth=2;ctx.stroke();
      if(king){
        ctx.fillStyle='#FFD700';ctx.font='bold 16px sans-serif';
        ctx.textAlign='center';ctx.textBaseline='middle';
        ctx.fillText('♛',c*SZ+SZ/2,r*SZ+SZ/2);
      }
    }
  }
}

function getCaptures(r,c,b){
  const {piece,king}=b[r][c];
  const dirs=king?[[-1,-1],[-1,1],[1,-1],[1,1]]:(piece===1?[[-1,-1],[-1,1]]:[[1,-1],[1,1]]);
  const caps=[];
  for(const [dr,dc] of dirs){
    const mr=r+dr,mc=c+dc,lr=r+2*dr,lc=c+2*dc;
    if(lr>=0&&lr<8&&lc>=0&&lc<8&&b[mr]&&b[mr][mc]&&b[mr][mc].piece&&b[mr][mc].piece!==piece&&!b[lr][lc].piece)
      caps.push([lr,lc,mr,mc]);
  }
  return caps;
}

function getMoves(r,c){
  const {piece,king}=board[r][c];
  const caps=getCaptures(r,c,board);
  if(caps.length)return caps.map(([lr,lc])=>[lr,lc]);
  const dirs=king?[[-1,-1],[-1,1],[1,-1],[1,1]]:(piece===1?[[-1,-1],[-1,1]]:[[1,-1],[1,1]]);
  return dirs.map(([dr,dc])=>[r+dr,c+dc]).filter(([nr,nc])=>nr>=0&&nr<8&&nc>=0&&nc<8&&!board[nr][nc].piece);
}

function canvasClick(e){
  if(gameOver||currentTurn!==1)return;
  const rect=document.getElementById('board').getBoundingClientRect();
  const ex=e.touches?e.touches[0].clientX:e.clientX;
  const ey=e.touches?e.touches[0].clientY:e.clientY;
  const c=Math.floor((ex-rect.left)/SZ),r=Math.floor((ey-rect.top)/SZ);
  if(r<0||r>7||c<0||c>7)return;
  if(board[r][c].piece===1){selected=[r,c];validMoves=getMoves(r,c);drawBoard();return;}
  if(selected&&validMoves.some(([mr,mc])=>mr===r&&mc===c)){doMove(selected[0],selected[1],r,c,1);return;}
  selected=null;validMoves=[];drawBoard();
}

function doMove(fr,fc,tr,tc,player){
  const caps=getCaptures(fr,fc,board);
  const cap=caps.find(([lr,lc])=>lr===tr&&lc===tc);
  board[tr][tc]={...board[fr][fc]};board[fr][fc]={piece:0,king:false};
  if(cap){board[cap[2]][cap[3]]={piece:0,king:false};if(player===1)pieces2--;else pieces1--;updateScores();}
  if(player===1&&tr===0)board[tr][tc].king=true;
  if(player===2&&tr===7)board[tr][tc].king=true;
  if(player===1)checkMotivate();
  selected=null;validMoves=[];
  if(checkWin())return;
  currentTurn=player===1?2:1;updateTurn();drawBoard();
  if(currentTurn===2&&isBot)setTimeout(botMove,700);
}

function checkWin(){
  if(pieces1===0||pieces2===0){
    gameOver=true;clearInterval(timerInterval);
    const won=pieces1>0;
    if(won){wins++;balls+=15;}else{losses++;balls=Math.max(0,balls-5);}
    saveStats();updateStats();
    setTimeout(()=>showResult(won?'win':'lose'),300);return true;
  }
  return false;
}

function showResult(type){
  const t=document.getElementById('modal-title'),b=document.getElementById('modal-body');
  if(type==='win'){t.textContent="G'alaba! 🏆";b.textContent="Siz g'olib bo'ldingiz! +15 ball qo'shildi.";}
  else if(type==='lose'){t.textContent="Mag'lubiyat 😔";b.textContent="Raqib yutdi. -5 ball. Qayta urinib ko'ring!";}
  else{t.textContent="Durang 🤝";b.textContent="Teng natija. +5 ball.";balls+=5;saveStats();updateStats();}
  stopMusic();
  if(musicOn)setTimeout(()=>startMusic('lobby'),500);
  document.getElementById('result-modal').classList.add('open');
}

function botMove(){
  let best=null;
  for(let r=0;r<8;r++)for(let c=0;c<8;c++){
    if(board[r][c].piece===2){
      const caps=getCaptures(r,c,board);
      if(caps.length){best={fr:r,fc:c,tr:caps[0][0],tc:caps[0][1]};break;}
    }
  }
  if(!best){
    const moves=[];
    for(let r=0;r<8;r++)for(let c=0;c<8;c++)if(board[r][c].piece===2)
      getMoves(r,c).forEach(([nr,nc])=>moves.push({fr:r,fc:c,tr:nr,tc:nc}));
    if(!moves.length){gameOver=true;wins++;balls+=15;saveStats();updateStats();showResult('win');return;}
    best=moves[Math.floor(Math.random()*moves.length)];
  }
  doMove(best.fr,best.fc,best.tr,best.tc,2);
}

function updateTurn(){
  document.getElementById('turn-who').textContent=currentTurn===1?"Sizning navbat":"Raqib o'ynamoqda";
  resetTimer();
}
function updateScores(){
  document.getElementById('sc1').textContent=pieces1;
  document.getElementById('sc2').textContent=pieces2;
}
function resetTimer(){
  clearInterval(timerInterval);timerVal=60;
  document.getElementById('timer').textContent='60';
  timerInterval=setInterval(()=>{
    timerVal--;document.getElementById('timer').textContent=timerVal;
    if(timerVal<=0){
      clearInterval(timerInterval);
      addChatMsg('sys',"Vaqt tugadi! Navbat o'tkazildi.");
      currentTurn=currentTurn===1?2:1;updateTurn();drawBoard();
      if(currentTurn===2&&isBot)setTimeout(botMove,400);
    }
  },1000);
}

function startGame(opp){
  oppName=opp;currentOpp=opp;isBot=(opp==='Bot');
  document.getElementById('opp-name-sc').textContent=opp==='Bot'?'Bot':opp.split(' ')[0];
  document.getElementById('lobby').classList.remove('active');
  document.getElementById('game-screen').classList.add('active');
  document.getElementById('chat-msgs').innerHTML='<div class="chat-msg sys">O\'yin boshlandi. Omad!</div>';
  document.getElementById('draw-offer').classList.remove('show');
  document.getElementById('motivate-bar').classList.remove('show');
  initBoard();drawBoard();updateTurn();
  stopMusic();if(musicOn)startMusic('game');
  if(!isBot)setTimeout(()=>addChatMsg(opp,"Salom! Yaxshi o'yin! 😊"),1500);
}

function goLobby(){
  clearInterval(timerInterval);
  document.getElementById('game-screen').classList.remove('active');
  document.getElementById('lobby').classList.add('active');
  document.getElementById('result-modal').classList.remove('open');
  stopMusic();if(musicOn)startMusic('lobby');
}

function offerDraw(){
  if(gameOver)return;
  addChatMsg('sys','Durang taklif qildingiz.');
  if(isBot){setTimeout(()=>{
    if(Math.random()>0.5)addChatMsg('sys','Bot durangni rad etdi.');
    else{balls+=5;saveStats();updateStats();showResult('draw');gameOver=true;clearInterval(timerInterval);}
  },1000);}
  else setTimeout(()=>document.getElementById('draw-offer').classList.add('show'),600);
}
function acceptDraw(){document.getElementById('draw-offer').classList.remove('show');balls+=5;saveStats();updateStats();showResult('draw');gameOver=true;clearInterval(timerInterval);}
function declineDraw(){document.getElementById('draw-offer').classList.remove('show');addChatMsg('sys','Durang rad etildi.');}
function resign(){if(gameOver)return;gameOver=true;clearInterval(timerInterval);losses++;balls=Math.max(0,balls-5);saveStats();updateStats();showResult('lose');}

function addChatMsg(who,text){
  const d=document.getElementById('chat-msgs');
  const m=document.createElement('div');
  m.className='chat-msg'+(who==='sys'?' sys':'');
  if(who!=='sys')m.innerHTML=`<span class="cn">${who==='me'?'Siz':oppName}:</span>${text}`;
  else m.textContent=text;
  d.appendChild(m);d.scrollTop=d.scrollHeight;
}
function sendChat(){
  const i=document.getElementById('chat-input');
  if(!i.value.trim())return;
  addChatMsg('me',i.value.trim());i.value='';
  if(!isBot)setTimeout(()=>{
    const r=["Yaxshi yurish!","Ko'ramiz... 😏","Zo'r!","Hmm 🤔","😎","🔥"];
    addChatMsg(oppName,r[Math.floor(Math.random()*r.length)]);
  },1200);
}
function sendSticker(s){
  addChatMsg('me',s);
  if(!isBot)setTimeout(()=>{
    const r=['👍','😄','🔥','😮','😂','👑'];
    addChatMsg(oppName,r[Math.floor(Math.random()*r.length)]);
  },900);
}
function closeModal(){document.getElementById('result-modal').classList.remove('open');}

const cv=document.getElementById('board');
cv.addEventListener('click',canvasClick);
cv.addEventListener('touchstart',e=>{e.preventDefault();canvasClick(e);},{passive:false});

// Start lobby music on first interaction
document.body.addEventListener('click',()=>{if(musicOn&&!audioCtx)startMusic('lobby');},{once:true});
</script>
</body>
</html>
HTMLEOF
echo "Done"
Output

Done
