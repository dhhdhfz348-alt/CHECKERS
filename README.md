<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Checkers — Abdurahmon Tog'ajon</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #1a1a2e; color: #fff; min-height: 100vh; overflow-x: hidden; }
        #app { max-width: 400px; margin: 0 auto; padding: 8px; }
        .screen { display: none; }
        .screen.active { display: block; }
        .lobby-header { text-align: center; padding: 20px 0 14px; }
        .owner-name { font-size: 24px; font-weight: 700; margin-bottom: 4px; background: linear-gradient(90deg, #f59e0b, #e94560); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .lobby-header p { font-size: 13px; color: #aaa; }
        .music-bar { display: flex; align-items: center; gap: 8px; background: #16213e; border-radius: 10px; padding: 10px 14px; margin-bottom: 12px; border: 1px solid #0f3460; }
        .music-icon { font-size: 18px; animation: spin 4s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0); } 100% { transform: rotate(360deg); } }
        .music-text { flex: 1; font-size: 13px; color: #a78bfa; }
        .music-toggle { background: none; border: none; color: #a78bfa; font-size: 18px; cursor: pointer; }
        .player-info { background: #16213e; border-radius: 12px; padding: 12px 16px; margin-bottom: 12px; display: flex; align-items: center; gap: 12px; }
        .avatar { width: 44px; height: 44px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 15px; flex-shrink: 0; }
        .av1 { background: linear-gradient(135deg, #6366f1, #8b5cf6); color: #fff; }
        .player-meta { flex: 1; }
        .player-meta strong { font-size: 15px; display: block; margin-bottom: 2px; }
        .player-meta span { font-size: 12px; color: #aaa; }
        .badge { background: #1b5e20; color: #69f0ae; font-size: 11px; padding: 3px 8px; border-radius: 20px; }
        .stats-row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 12px; }
        .stat-card { background: #16213e; border-radius: 10px; padding: 10px; text-align: center; border: 1px solid #0f3460; }
        .stat-card .num { font-size: 22px; font-weight: 700; }
        .stat-card .lbl { font-size: 11px; color: #aaa; margin-top: 2px; }
        .online-list { margin-bottom: 12px; }
        .online-list h3 { font-size: 13px; color: #aaa; margin-bottom: 8px; }
        .online-item { display: flex; align-items: center; gap: 10px; padding: 10px 12px; background: #16213e; border-radius: 10px; margin-bottom: 6px; }
        .dot { width: 8px; height: 8px; border-radius: 50%; background: #4caf50; flex-shrink: 0; animation: pulse 2s infinite; }
        @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.4; } }
        .online-item .iname { flex: 1; font-size: 14px; }
        .online-item .lvl { font-size: 12px; color: #aaa; margin-right: 8px; }
        .btn { display: block; width: 100%; padding: 12px; border-radius: 10px; border: none; background: #0f3460; color: #fff; font-size: 14px; cursor: pointer; font-family: inherit; transition: all 0.15s; text-align: center; font-weight: 600; }
        .btn:hover { background: #1a4a80; transform: scale(1.01); }
        .btn.primary { background: linear-gradient(135deg, #e94560, #c73652); color: #fff; }
        .btn.sm { padding: 6px 12px; font-size: 12px; width: auto; border-radius: 8px; }
        .btn.green-sm { padding: 6px 12px; font-size: 12px; width: auto; border-radius: 8px; background: #1b5e20; color: #69f0ae; }
        .btn.red-sm { padding: 6px 12px; font-size: 12px; width: auto; border-radius: 8px; background: #b71c1c; color: #ff8a80; }
        .btn-row { display: flex; gap: 8px; margin-top: 15px; }
        .btn-row .btn { flex: 1; }
        .game-header { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
        .turn-indicator { flex: 1; text-align: center; }
        .turn-indicator .who { font-size: 13px; font-weight: 500; color: #60a5fa; }
        .turn-indicator .timer { font-size: 28px; font-weight: 700; color: #e94560; }
        .scores { display: flex; gap: 6px; }
        .score-box { background: #16213e; border-radius: 8px; padding: 6px 12px; text-align: center; min-width: 52px; border: 1px solid #0f3460; }
        .score-box .sc { font-size: 18px; font-weight: 700; }
        .score-box .sl { font-size: 10px; color: #aaa; }
        .board-wrap { display: flex; justify-content: center; margin-bottom: 10px; }
        canvas id="board" { border: 4px solid #5D4037; border-radius: 8px; cursor: pointer; touch-action: none; background: #8B4513; box-shadow: 0 4px 20px rgba(0,0,0,0.5); }
        .game-actions { display: flex; gap: 8px; margin-bottom: 8px; }
        .game-actions .btn { flex: 1; padding: 10px; }
        .motivate-bar { background: linear-gradient(135deg, #1e3a8a, #3b82f6); border-radius: 10px; padding: 12px; margin-bottom: 8px; font-size: 14px; font-weight: bold; color: #fff; text-align: center; display: none; border: 1px solid #60a5fa; box-shadow: 0 0 10px rgba(59,130,246,0.5); }
        .motivate-bar.show { display: block; animation: bounceIn 0.5s; }
        @keyframes bounceIn { 0% { transform: scale(0.3); opacity: 0; } 50% { transform: scale(1.05); opacity: 0.8; } 70% { transform: scale(0.9); opacity: 0.9; } 100% { transform: scale(1); opacity: 1; } }
        .chat-area { background: #16213e; border-radius: 12px; overflow: hidden; margin-bottom: 8px; border: 1px solid #0f3460; }
        .chat-msgs { height: 80px; overflow-y: auto; padding: 8px; font-size: 13px; }
        .chat-msg { margin-bottom: 5px; }
        .chat-msg .cn { font-weight: 700; margin-right: 4px; color: #f59e0b; }
        .chat-msg.sys { color: #aaa; font-style: italic; }
        .stickers { display: flex; gap: 6px; padding: 6px 8px; border-top: 1px solid #0f3460; overflow-x: auto; }
        .stk { font-size: 22px; cursor: pointer; transition: transform 0.1s; flex-shrink: 0; }
        .stk:hover { transform: scale(1.3); }
        .chat-input-row { display: flex; gap: 6px; padding: 6px 8px; border-top: 1px solid #0f3460; }
        .chat-input-row input { flex: 1; padding: 7px 10px; border: 1px solid #0f3460; border-radius: 8px; font-size: 13px; background: #0f3460; color: #fff; }
        .chat-input-row button { padding: 7px 14px; font-size: 13px; border-radius: 8px; border: none; background: #e94560; color: #fff; cursor: pointer; font-weight: bold; }
        .draw-offer { background: #1b3a1b; border: 1px solid #2e7d32; border-radius: 10px; padding: 10px; margin-bottom: 8px; font-size: 13px; display: none; align-items: center; gap: 8px; }
        .draw-offer.show { display: flex; }
        .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.85); z-index: 100; align-items: center; justify-content: center; }
        .modal-overlay.open { display: flex; }
        .modal { background: #16213e; border-radius: 16px; padding: 24px; max-width: 280px; width: 90%; text-align: center; border: 1px solid #e94560; }
        .modal h3 { font-size: 22px; font-weight: 700; margin-bottom: 8px; }
        .modal p { font-size: 14px; color: #aaa; margin-bottom: 16px; }
        .modal-btns { display: flex; gap: 8px; }
        .back-btn { background: #16213e; color: #aaa; border: 1px solid #0f3460; padding: 6px 12px; border-radius: 8px; cursor: pointer; font-size: 14px; }
    </style>
</head>
<body>
<div id="app">
    <div id="lobby" class="screen active">
        <div class="lobby-header">
            <div class="owner-name">👑 Abdurahmon Tog'ajon</div>
            <p>Eksklyuziv Shashka Maydoni</p>
        </div>
        <div class="music-bar">
            <span class="music-icon" id="m-icon">🎵</span>
            <span class="music-text" id="m-text">Sokin miya xotirjamligi ohangi...</span>
            <button class="music-toggle" onclick="toggleMusic()" id="music-btn">⏸</button>
        </div>
        <div class="player-info">
            <div class="avatar av1" id="my-avatar">A</div>
            <div class="player-meta">
                <strong id="my-name">Foydalanuvchi</strong>
                <span id="my-stats">Daraja 1 • 0 ball</span>
            </div>
            <span class="badge">Onlayn</span>
        </div>
        <div class="stats-row">
            <div class="stat-card"><div class="num" style="color:#69f0ae" id="stat-win">0</div><div class="lbl">G'alaba</div></div>
            <div class="stat-card"><div class="num" style="color:#e94560" id="stat-lose">0</div><div class="lbl">Mag'lubiyat</div></div>
            <div class="stat-card"><div class="num" style="color:#60a5fa" id="stat-ball">0</div><div class="lbl">Ball</div></div>
        </div>
        <div class="online-list">
            <h3>Raqiblar</h3>
            <div class="online-item">
                <div class="dot"></div>
                <div class="avatar" style="width:32px;height:32px;font-size:12px;background:#0f3460;color:#60a5fa">🎯</div>
                <span class="iname">Iqror (Real o'yinchi)</span>
                <span class="lvl">Daraja 3</span>
                <button class="btn sm primary" onclick="startGame('Iqror')">O'ynash</button>
            </div>
        </div>
        <div class="btn-row">
            <button class="btn" style="background: #2e7d32;" onclick="startGame('Doniyor')">Jonli taklif yuborish</button>
        </div>
    </div>

    <div id="game-screen" class="screen">
        <div class="game-header">
            <button class="back-btn" onclick="goLobby()">←</button>
            <div class="turn-indicator">
                <div class="who" id="turn-who">Sizning navbatingiz</div>
                <div class="timer" id="timer">60</div>
            </div>
            <div class="scores">
                <div class="score-box"><div class="sc" id="sc1">12</div><div class="sl">Siz</div></div>
                <div class="score-box"><div class="sc" id="sc2">12</div><div class="sl" id="opp-name-sc">Raqib</div></div>
            </div>
        </div>
        <div class="motivate-bar" id="motivate-bar">⚡ DAHSHAT! Ketma-ket 3 ta urish! Yengilmassiz! ⚡</div>
        <div class="draw-offer" id="draw-offer">
            <span>🤝</span>
            <span style="flex:1">Raqib durang taklif qildi</span>
            <button class="green-sm" onclick="acceptDraw()">Qabul</button>
            <button class="btn sm" onclick="declineDraw()">Rad etish</button>
        </div>
        <div class="board-wrap">
            <canvas id="board" width="320" height="320"></canvas>
        </div>
        <div class="game-actions">
            <button class="btn" onclick="offerDraw()">🤝 Durang</button>
            <button class="btn red-sm" onclick="resign()">🏳 Taslim bo'lish</button>
        </div>
        <div class="chat-area">
            <div class="chat-msgs" id="chat-msgs"></div>
            <div class="stickers">
                <span class="stk" onclick="sendSticker('👍')">👍</span>
                <span class="stk" onclick="sendSticker('😎')">😎</span>
                <span class="stk" onclick="sendSticker('🔥')">🔥</span>
                <span class="stk" onclick="sendSticker('💪')">💪</span>
                <span class="stk" onclick="sendSticker('👑')">👑</span>
                <span class="stk" onclick="sendSticker('👏')">👏</span>
            </div>
            <div class="chat-input-row">
                <input type="text" id="chat-input" placeholder="Xabar yozing..." onkeydown="if(event.key==='Enter')sendChat()">
                <button onclick="sendChat()">➤</button>
            </div>
        </div>
    </div>
</div>

<div class="modal-overlay" id="result-modal">
    <div class="modal">
        <h3 id="modal-title">G'alaba!</h3>
        <p id="modal-body">Siz mukammal g'alaba qozondingiz!</p>
        <div class="modal-btns">
            <button class="btn primary" onclick="closeModal();goLobby()">Lobbyga qaytish</button>
        </div>
    </div>
</div>

<script>
    let myName = 'Abdurahmon', wins = 0, losses = 0, balls = 0;
    let currentTurn = 1, pieces1 = 12, pieces2 = 12, gameOver = false;
    let timerVal = 60, timerInterval = null, currentOpp = 'Raqib', comboStrikes = 0;
    let selected = null, validMoves = [];
    let board = [];
    const SZ = 40;

    // Telegram sozlamalari
    try {
        const tg = window.Telegram.WebApp;
        tg.ready(); tg.expand();
        if(tg.initDataUnsafe && tg.initDataUnsafe.user) {
            let u = tg.initDataUnsafe.user;
            myName = u.first_name || 'Abdurahmon';
            document.getElementById('my-name').textContent = myName;
        }
    } catch(e) {}

    // Stats yuklash
    try {
        wins = parseInt(localStorage.getItem('w')||'0');
        losses = parseInt(localStorage.getItem('l')||'0');
        balls = parseInt(localStorage.getItem('b')||'0');
        updateStats();
    } catch(e){}

    function updateStats() {
        document.getElementById('stat-win').textContent = wins;
        document.getElementById('stat-lose').textContent = losses;
        document.getElementById('stat-ball').textContent = balls;
        document.getElementById('my-stats').textContent = `Daraja ${Math.floor(balls/50)+1} • ${balls} ball`;
    }

    // Musiqa generatori (Hech qanday faylsiz ishlaydi!)
    let audioCtx = null, musicOn = true, oscs = [];
    function startMusic(mode) {
        if(!musicOn) return;
        stopMusic();
        try {
            audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            let freqs = mode === 'lobby' ? [196, 261, 329] : [261, 392, 523];
            let text = mode === 'lobby' ? "Sokin miya xotirjamligi ohangi..." : "🔥 JANG! Motivatsiya musiqasi!";
            document.getElementById('m-text').textContent = text;
            freqs.forEach((f, i) => {
                let osc = audioCtx.createOscillator();
                let gain = audioCtx.createGain();
                osc.type = mode === 'lobby' ? 'sine' : 'triangle';
                osc.frequency.value = f;
                gain.gain.value = 0.02;
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start();
                oscs.push(osc);
            });
        } catch(e){}
    }
    function stopMusic() {
        oscs.forEach(o => { try{ o.stop(); }catch(e){} });
        oscs = [];
    }
    function toggleMusic() {
        musicOn = !musicOn;
        document.getElementById('music-btn').textContent = musicOn ? '⏸' : '▶';
        if(musicOn) startMusic('lobby'); else stopMusic();
    }

    // Motivatsiyalar ro'yxati (3 marta urganda portlaydi)
    const motivates = [
        "🔥 DAXSHATLI COMBO! 3 ta shashkani bitta urishda yakson qildingiz!",
        "🧠 GENIAL REJA! Abdurahmon uslubidagi g'ayratli zarba!",
        "💪 RAKIB CHOCDA! Sizni to'xtatib bo'lmaydi, olg'a!",
        "👑 SHOHONA KO'CHISH! Bu yurish darsliklarga kiradi!"
    ];

    function initBoard() {
        board = [];
        for(let r=0; r<8; r++) {
            board[r] = [];
            for(let c=0; c<8; c++) {
                let p = 0;
                if((r+c)%2===1) {
                    if(r < 3) p = 2;
                    else if(r > 4) p = 1;
                }
                board[r][c] = { piece: p, king: false };
            }
        }
        pieces1 = 12; pieces2 = 12; gameOver = false; comboStrikes = 0;
        document.getElementById('sc1').textContent = 12;
        document.getElementById('sc2').textContent = 12;
    }

    function drawBoard() {
        const cv = document.getElementById('board');
        const ctx = cv.getContext('2d');
        ctx.clearRect(0,0,320,320);
        for(let r=0; r<8; r++) {
            for(let c=0; c<8; c++) {
                ctx.fillStyle = (r+c)%2===0 ? '#f0d9b5' : '#b58863';
                ctx.fillRect(c*SZ, r*SZ, SZ, SZ);
                if(selected && selected[0]===r && selected[1]===c) {
                    ctx.fillStyle = 'rgba(255, 255, 0, 0.4)';
                    ctx.fillRect(c*SZ, r*SZ, SZ, SZ);
                }
            }
        }
        validMoves.forEach(([mr, mc]) => {
            ctx.fillStyle = 'rgba(0, 255, 0, 0.5)';
            ctx.beginPath(); ctx.arc(mc*SZ+SZ/2, mr*SZ+SZ/2, 6, 0, Math.PI*2); ctx.fill();
        });
        for(let r=0; r<8; r++) {
            for(let c=0; c<8; c++) {
                let p = board[r][c].piece;
                if(!p) continue;
                ctx.beginPath();
                ctx.arc(c*SZ+SZ/2, r*SZ+SZ/2, SZ/2-4, 0, Math.PI*2);
                ctx.fillStyle = p === 1 ? '#3b82f6' : '#ef4444';
                ctx.fill();
                ctx.strokeStyle = '#fff'; ctx.lineWidth = 1; ctx.stroke();
                if(board[r][c].king) {
                    ctx.fillStyle = '#fff'; ctx.font = '16px serif';
                    ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
                    ctx.fillText('👑', c*SZ+SZ/2, r*SZ+SZ/2);
                }
            }
        }
    }

    function getMoves(r, c) {
        let p = board[r][c].piece;
        let moves = [];
        let dirs = board[r][c].king ? [[1,1],[1,-1],[-1,1],[-1,-1]] : (p===1 ? [[-1,1],[-1,-1]] : [[1,1],[1,-1]]);
        
        // Oddiy yurishlar va urishlar
        dirs.forEach(([dr, dc]) => {
            let nr = r + dr, nc = c + dc;
            if(nr>=0 && nr<8 && nc>=0 && nc<8) {
                if(!board[nr][nc].piece) {
                    moves.push([nr, nc, false]);
                } else if(board[nr][nc].piece !== p) {
                    let nnr = nr + dr, nnc = nc + dc;
                    if(nnr>=0 && nnr<8 && nnc>=0 && nnc<8 && !board[nnr][nnc].piece) {
                        moves.push([nnr, nnc, true, nr, nc]); // Urish bor
                    }
                }
            }
        });
        return moves;
    }

    function handleCanvasClick(e) {
        if(gameOver || currentTurn !== 1) return;
        const rect = document.getElementById('board').getBoundingClientRect();
        const clientX = e.touches ? e.touches[0].clientX : e.clientX;
        const clientY = e.touches ? e.touches[0].clientY : e.clientY;
        let c = Math.floor((clientX - rect.left)/SZ);
        let r = Math.floor((clientY - rect.top)/SZ);
        
        if(board[r] && board[r][c] && board[r][c].piece === 1) {
            selected = [r, c];
            validMoves = getMoves(r, c);
            drawBoard();
        } else if(selected) {
            let hit = validMoves.find(([mr, mc]) => mr === r && mc === c);
            if(hit) {
                // Yurishni bajarish
                let [fr, fc] = selected;
                board[r][c] = { ...board[fr][fc] };
                board[fr][fc] = { piece: 0, king: false };
                
                if(r === 0) board[r][c].king = true;

                if(hit[2]) { // Agar urgan bo'lsa
                    board[hit[3]][hit[4]] = { piece: 0, king: false };
                    pieces2--;
                    comboStrikes++;
                    document.getElementById('sc2').textContent = pieces2;
                    if(comboStrikes >= 3) {
                        let bar = document.getElementById('motivate-bar');
                        bar.textContent = motivates[Math.floor(Math.random()*motivates.length)];
                        bar.classList.add('show');
                        setTimeout(() => bar.classList.remove('show'), 4000);
                    }
                } else {
                    comboStrikes = 0;
                }

                selected = null; validMoves = [];
                if(pieces2 <= 0) { endGame(true); return; }
                
                currentTurn = 2;
                document.getElementById('turn-who').textContent = "Raqib o'ylamoqda...";
                drawBoard();
                setTimeout(rivalAI, 1000);
            }
        }
    }

    function rivalAI() {
        if(gameOver) return;
        let allMoves = [];
        for(let r=0; r<8; r++) {
            for(let c=0; c<8; c++) {
                if(board[r][c].piece === 2) {
                    getMoves(r, c).forEach(m => allMoves.push({fr: r, fc: c, m: m}));
                }
            }
        }
        let hits = allMoves.filter(x => x.m[2]);
        let finalMove = hits.length > 0 ? hits[Math.floor(Math.random()*hits.length)] : allMoves[Math.floor(Math.random()*allMoves.length)];
        
        if(finalMove) {
            let {fr, fc, m} = finalMove;
            let [tr, tc, isHit, hr, hc] = m;
            board[tr][tc] = { ...board[fr][fc] };
            board[fr][fc] = { piece: 0, king: false };
            if(tr === 7) board[tr][tc].king = true;
            if(isHit) {
                board[hr][hc] = { piece: 0, king: false };
                pieces1--;
                document.getElementById('sc1').textContent = pieces1;
            }
            if(pieces1 <= 0) { endGame(false); return; }
        }
        currentTurn = 1;
        document.getElementById('turn-who').textContent = "Sizning navbatingiz";
        resetTimer();
        drawBoard();
    }

    function startGame(opp) {
        currentOpp = opp;
        document.getElementById('opp-name-sc').textContent = opp;
        document.getElementById('lobby').classList.remove('active');
        document.getElementById('game-screen').classList.add('active');
        document.getElementById('chat-msgs').innerHTML = '<div class="chat-msg sys">O\'yin boshlandi. Omad tilaymiz!</div>';
        initBoard();
        drawBoard();
        resetTimer();
        startMusic('game');
        // Haqiqiy odamdek chat yozishi
        setTimeout(() => addChatMsg(opp, "Salom! Abdurahmon akaga qarshi o'yin qiziqarli bo'ladi! 😊"), 2000);
    }

    function resetTimer() {
        clearInterval(timerInterval);
        timerVal = 60;
        document.getElementById('timer').textContent = timerVal;
        timerInterval = setInterval(() => {
            timerVal--;
            document.getElementById('timer').textContent = timerVal;
            if(timerVal <= 0) { clearInterval(timerInterval); rivalAI(); }
        }, 1000);
    }

    function addChatMsg(sender, text) {
        let div = document.createElement('div');
        div.className = 'chat-msg';
        div.innerHTML = `<span class="cn">${sender}:</span> <span>${text}</span>`;
        let area = document.getElementById('chat-msgs');
        area.appendChild(div);
        area.scrollTop = area.scrollHeight;
    }

    function sendChat() {
        let inp = document.getElementById('chat-input');
        if(!inp.value.trim()) return;
        addChatMsg('Siz', inp.value);
        let val = inp.value; inp.value = '';
        setTimeout(() => {
            addChatMsg(currentOpp, "Yaxshi yurish, ammo men taslim bo'lmayman! 😉");
        }, 2000);
    }

    function sendSticker(stk) {
        addChatMsg('Siz', stk);
        setTimeout(() => {
            let replies = ['😎', '🤝', '🔥', '💪'];
            addChatMsg(currentOpp, replies[Math.floor(Math.random()*replies.length)]);
        }, 1500);
    }

    function offerDraw() {
        document.getElementById('draw-offer').classList.add('show');
    }
    function acceptDraw() {
        document.getElementById('draw-offer').classList.remove('show');
        balls += 5; updateStats(); saveStats();
        alert("Durang qabul qilindi! +5 ball!"); goLobby();
    }
    function declineDraw() {
        document.getElementById('draw-offer').classList.remove('show');
    }
    function resign() { endGame(false); }

    function endGame(isWin) {
        gameOver = true; clearInterval(timerInterval);
        let t = document.getElementById('modal-title');
        let b = document.getElementById('modal-body');
        if(isWin) {
            wins++; balls += 15;
            t.textContent = "G'alaba! 🎉"; b.textContent = "Siz daxshatli o'yin ko'rsatib, +15 ball yutdingiz!";
        } else {
            losses++; balls = Math.max(0, balls - 5);
            t.textContent = "Mag'lubiyat 😔"; b.textContent = "Bu safar omad kelmadi. -5 ball.";
        }
        updateStats(); saveStats();
        document.getElementById('result-modal').classList.add('open');
    }

    function saveStats() {
        localStorage.setItem('w', wins); localStorage.setItem('l', losses); localStorage.setItem('b', balls);
    }
    function closeModal() { document.getElementById('result-modal').classList.remove('open'); }
    function goLobby() {
        document.getElementById('game-screen').classList.remove('active');
        document.getElementById('lobby').classList.add('active');
        closeModal();
        startMusic('lobby');
    }

    document.getElementById('board').addEventListener('click', handleCanvasClick);
    document.getElementById('board').addEventListener('touchstart', handleCanvasClick);
    
    // Ilk boshlanishda musiqani faollashtirish
    window.addEventListener('click', () => { if(!audioCtx) startMusic('lobby'); }, {once: true});
</script>
</body>
</html>
