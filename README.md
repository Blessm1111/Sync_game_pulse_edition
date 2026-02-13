# Sync_game_pulse_edition
Attune.Connect.Heal.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>SYNC: Integrated Pulse Edition</title>
    <style>
        /* --- VISUAL CORE --- */
        body { 
            background: radial-gradient(circle at center, #1a1f35 0%, #0b1026 100%);
            color: #e0f7fa; 
            font-family: 'Segoe UI', sans-serif; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            min-height: 100vh; 
            margin: 0; 
            overflow: hidden; 
            perspective: 2000px; 
        }

        /* --- LAYOUT --- */
        #main-container {
            display: none; 
            flex-direction: row;
            width: 100%;
            height: 100vh;
        }

        #game-area {
            flex: 3;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        #sidebar {
            flex: 1;
            background: rgba(0, 0, 0, 0.4);
            border-left: 1px solid #40E0D0;
            padding: 20px;
            overflow-y: auto;
            backdrop-filter: blur(5px);
            max-width: 350px;
            box-shadow: -5px 0 20px rgba(0,0,0,0.5);
            z-index: 2000;
        }

        /* --- LEFT CLOCK & PILLAR TRACKER --- */
        #left-panel {
            position: absolute;
            top: 20px;
            left: 30px;
            z-index: 100;
            text-align: left;
            display: flex;
            flex-direction: column;
            gap: 15px; /* Space between clock and pillar tracker */
        }

        #pst-clock {
            font-size: 48px; 
            color: #40E0D0;
            font-weight: 300;
            letter-spacing: 2px;
            text-shadow: 0 0 20px rgba(64, 224, 208, 0.4);
            transition: all 0.5s ease;
        }

        #clock-label {
            font-size: 12px;
            color: #666;
            letter-spacing: 3px;
            margin-left: 5px;
            text-transform: uppercase;
        }

        /* The Bless Animation on the Clock */
        .clock-blessed {
            color: #FFD700 !important;
            text-shadow: 0 0 40px #FFD700, 0 0 80px #FFD700 !important;
            transform: scale(1.1);
        }

        #bless-float-text {
            position: absolute;
            top: -30px;
            left: 0;
            width: 100%;
            text-align: center;
            font-size: 24px;
            font-weight: bold;
            color: #FFD700;
            opacity: 0;
            pointer-events: none;
            text-shadow: 0 0 10px #FFD700;
        }

        .bless-float-anim {
            animation: clockBlessAnim 2s ease-out forwards;
        }

        @keyframes clockBlessAnim {
            0% { opacity: 0; transform: translateY(20px) scale(0.8); }
            20% { opacity: 1; transform: translateY(-10px) scale(1.2); }
            100% { opacity: 0; transform: translateY(-50px) scale(1); }
        }

        /* --- STAT BOXES (Modified for Left & Right) --- */
        .stat-box {
            background: rgba(0, 0, 0, 0.6);
            border: 1px solid #40E0D0;
            padding: 10px 15px;
            border-radius: 8px;
            text-align: left; /* Changed to left align for left panel */
            backdrop-filter: blur(4px);
            transition: 0.3s;
            max-width: 200px;
        }
        
        .stat-label { font-size: 10px; color: #aaa; text-transform: uppercase; letter-spacing: 1px; }
        .stat-val { font-size: 20px; font-weight: bold; color: #fff; }

        .stat-pop {
            border-color: #FFD700;
            box-shadow: 0 0 20px #FFD700;
            transform: scale(1.05);
        }

        /* Right Panel just for Games Initiated now */
        #stats-right {
            position: absolute;
            top: 20px;
            right: 20px;
            display: flex;
            z-index: 100;
        }

        /* --- TYPOGRAPHY --- */
        h1.logo { 
            margin-top: 50px; 
            font-weight: 300; 
            letter-spacing: 12px; 
            color: #40E0D0; 
            text-shadow: 0 0 20px rgba(64, 224, 208, 0.8); 
            font-size: 36px;
            z-index: 10;
        }

        h2.rule-title {
            color: #FFC107;
            font-size: 16px;
            border-bottom: 1px solid #444;
            padding-bottom: 5px;
            margin-top: 15px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        p.rule-text, li { font-size: 13px; line-height: 1.5; color: #ccc; margin-bottom: 8px; }

        /* --- INTRO SCREEN --- */
        #intro-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #000;
            z-index: 5000;
            display: flex; justify-content: center; align-items: center; flex-direction: column;
            transition: opacity 1.5s ease-out;
        }

        .iam-btn {
            background: transparent;
            border: 2px solid #40E0D0;
            color: #40E0D0;
            font-size: 40px;
            padding: 20px 60px;
            border-radius: 50px;
            cursor: pointer;
            letter-spacing: 5px;
            font-weight: 300;
            transition: 0.3s;
            box-shadow: 0 0 20px rgba(64, 224, 208, 0.2);
            animation: breathe 4s infinite ease-in-out;
        }
        .iam-btn:hover { background: #40E0D0; color: #000; box-shadow: 0 0 50px rgba(64, 224, 208, 0.8); }

        @keyframes breathe {
            0%, 100% { transform: scale(1); box-shadow: 0 0 20px rgba(64, 224, 208, 0.2); }
            50% { transform: scale(1.1); box-shadow: 0 0 40px rgba(64, 224, 208, 0.5); }
        }

        /* --- 3D PILLARS --- */
        #pillars-scene {
            display: flex; justify-content: center; gap: 30px; 
            height: 450px; width: 100%; padding-top: 350px; 
            transform-style: preserve-3d; transform: rotateX(15deg); 
        }

        .pillar-base { position: relative; width: 55px; height: 1px; transform-style: preserve-3d; }
        .pillar-ring {
            position: absolute; top: 50px; left: 50%; transform: translateX(-50%) rotateX(90deg);
            width: 65px; height: 65px; border: 2px solid rgba(64, 224, 208, 0.3);
            border-radius: 50%; box-shadow: 0 0 20px rgba(64, 224, 208, 0.1); transition: 0.5s;
        }
        .pillar-base.complete .pillar-ring {
            border-color: #FFD700; box-shadow: 0 0 50px #FFD700; background: rgba(255, 215, 0, 0.2);
        }
        .pillar-beam {
            position: absolute; bottom: -50px; left: 10%; width: 80%; height: 0px; 
            background: linear-gradient(to top, rgba(255, 215, 0, 0.8), transparent);
            transform-style: preserve-3d; transition: height 2s ease-out; pointer-events: none; z-index: -1;
        }

        .popup-text {
            position: absolute; top: -450px; left: 50%; transform: translateX(-50%);
            font-size: 20px; font-weight: bold; color: #FFD700; text-shadow: 0 0 10px #FFD700;
            opacity: 0; pointer-events: none; white-space: nowrap;
        }
        .float-anim { animation: floatUp 2s ease-out forwards; }
        .sync-anim { animation: floatUpSync 2s ease-out forwards; color: #00E676; text-shadow: 0 0 10px #00E676; }
        @keyframes floatUp {
            0% { opacity: 0; top: -100px; transform: translateX(-50%) scale(0.5); }
            20% { opacity: 1; transform: translateX(-50%) scale(1.2); }
            100% { opacity: 0; top: -500px; transform: translateX(-50%) scale(1); }
        }
        @keyframes floatUpSync {
            0% { opacity: 0; top: -100px; transform: translateX(-50%) scale(0.5); }
            20% { opacity: 1; transform: translateX(-50%) scale(1.5); }
            100% { opacity: 0; top: -400px; transform: translateX(-50%) scale(1); }
        }

        /* CARD STYLES */
        .pillar-card {
            position: absolute; left: 0; top: 0; width: 55px; height: 85px;
            background: #fff; border-radius: 6px; border: 1px solid #999;
            display: flex; justify-content: center; align-items: flex-start; padding-top: 5px;
            font-weight: 900; font-size: 18px; color: #333;
            box-shadow: 0 -2px 5px rgba(0,0,0,0.2); transition: all 0.3s ease-out; backface-visibility: hidden;
        }
        .pillar-card.ace {
            background: rgba(64, 224, 208, 0.9); color: #004D40; border: 2px solid #fff;
            box-shadow: 0 0 20px #40E0D0; align-items: center; padding-top: 0;
        }

        /* --- 2D ZONES --- */
        .flat-zone { display: flex; flex-direction: column; align-items: center; width: 100%; z-index: 100; }
        .zone-label { font-size: 10px; letter-spacing: 3px; color: #888; margin-bottom: 5px; text-transform: uppercase; }
        .card-row { display: flex; justify-content: center; gap: 8px; min-height: 100px; flex-wrap: wrap; max-width: 800px; }
        .card-2d {
            width: 50px; height: 80px; background: #fff; border-radius: 5px;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            cursor: pointer; box-shadow: 0 4px 10px rgba(0,0,0,0.5); transition: transform 0.2s; position: relative;
        }
        .card-2d:hover { transform: translateY(-10px) scale(1.1); z-index: 50; box-shadow: 0 0 15px #fff; }
        
        .Water { border-bottom: 5px solid #00BCD4; color: #006064; }
        .Earth { border-bottom: 5px solid #FFC107; color: #3E2723; }
        .Air   { border-bottom: 5px solid #00E676; color: #1B5E20; }
        .Fire  { border-bottom: 5px solid #F50057; color: #880E4F; }
        .Joker { 
            background: linear-gradient(135deg, #fff, #E0F7FA); border: 2px solid #FFD700; 
            color: #FFD700; box-shadow: 0 0 15px rgba(255, 215, 0, 0.4);
        }
        
        #controls { margin-top: 10px; display: flex; gap: 15px; }
        .btn {
            background: rgba(0,0,0,0.6); border: 1px solid #40E0D0; color: #40E0D0;
            padding: 8px 16px; border-radius: 30px; cursor: pointer; font-weight: bold;
            font-size: 10px; letter-spacing: 1px; transition: 0.2s; text-transform: uppercase;
        }
        .btn:hover { background: #40E0D0; color: #000; box-shadow: 0 0 20px #40E0D0; }
        .btn-danger { border-color: #F50057; color: #F50057; }
        .btn-danger:hover { background: #F50057; color: #fff; box-shadow: 0 0 20px #F50057; }
        .btn-action { border-color: #FFC107; color: #FFC107; }
        .btn-action:hover { background: #FFC107; color: #000; box-shadow: 0 0 20px #FFC107; }

        #log {
            position: fixed; bottom: 10px; left: 10px; width: 250px; height: 100px;
            background: rgba(0,0,0,0.9); border-left: 3px solid #40E0D0; color: #ccc;
            font-size: 12px; padding: 10px; overflow-y: auto; pointer-events: none;
            font-family: monospace; z-index: 1000;
        }

        #die-result {
            position: fixed; top: 40%; left: 40%; transform: translate(-50%, -50%);
            font-size: 80px; color: #fff; text-shadow: 0 0 20px #40E0D0; opacity: 0;
            transition: opacity 0.5s; pointer-events: none; z-index: 2000; font-weight: bold;
        }

        #healing-popup {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0, 50, 50, 0.85); z-index: 4000;
            justify-content: center; align-items: center; flex-direction: column; animation: fadeIn 0.5s;
        }
        @keyframes fadeIn { from { opacity:0; } to { opacity:1; } }
        .healing-text { font-size: 40px; color: #fff; margin-bottom: 20px; text-shadow: 0 0 20px #40E0D0; text-align: center; }

        #victory {
            display: none; position: fixed; top:0; left:0; width:100%; height:100%;
            background: rgba(0,0,0,0.0); z-index: 3000;
            justify-content: center; align-items: center; flex-direction: column; transition: background 2s;
        }
        .peace-text {
            font-size: 60px; font-weight: 300; color: #fff; text-shadow: 0 0 30px #40E0D0;
            letter-spacing: 5px; opacity: 0; transform: scale(0.8); animation: peaceFadeIn 3s ease-out forwards 1s;
        }
        .score-text {
            font-size: 24px; color: #FFD700; margin-top: 20px; opacity: 0;
            animation: peaceFadeIn 3s ease-out forwards 2s; text-transform: uppercase; letter-spacing: 2px;
        }
        @keyframes peaceFadeIn { to { opacity: 1; transform: scale(1); } }
    </style>
</head>
<body>

    <div id="intro-screen">
        <button class="iam-btn" onclick="startGame()">I AM</button>
    </div>

    <div id="main-container">
        
        <div id="left-panel">
            <div id="bless-float-text">BLESS ✨</div>
            <div id="clock-container">
                <div id="pst-clock">00:00:00</div>
                <div id="clock-label">PST / PACIFIC</div>
            </div>
            
            <div class="stat-box" id="stat-box-pillars">
                <div class="stat-label">Pillars Healed Today</div>
                <div class="stat-val" id="daily-pillars">0</div>
            </div>
        </div>

        <div id="stats-right">
            <div class="stat-box">
                <div class="stat-label">Games Initiated</div>
                <div class="stat-val" id="daily-games">0</div>
            </div>
        </div>

        <div id="game-area">
            <h1 class="logo">SYNC</h1>

            <div id="pillars-scene"></div>

            <div class="flat-zone">
                <div class="zone-label">THE HORIZON</div>
                <div class="card-row" id="horizon-container"></div>
            </div>

            <div class="flat-zone" style="margin-top: 15px;">
                <div class="zone-label">YOUR HAND</div>
                <div class="card-row" id="hand-container"></div>
            </div>

            <div id="controls">
                <div style="color:#aaa; font-size:12px; align-self:center; margin-right:5px">
                    VOID: <span style="color:#E040FB; font-weight:bold; font-size:16px">∞</span>
                </div>
                <button class="btn btn-danger" onclick="rollVoidDie()">ROLL VOID 🎲</button>
                <button class="btn btn-action" onclick="shuffleHorizon()">SHUFFLE HORIZON ↻</button>
                <button class="btn" onclick="refillHand()">REFILL HAND</button>
            </div>
        </div>

        <div id="sidebar">
            <h2 class="rule-title" style="margin-top:0; color:#40E0D0">HOW TO PLAY</h2>
            
            <h2 class="rule-title">THE GOAL</h2>
            <p class="rule-text">Build all 7 Pillars from <b>2</b> to <b>King</b>.</p>

            <h2 class="rule-title">SCORING</h2>
            <p class="rule-text">Your "Light Preserved" score is based on remaining cards. High score = High efficiency.</p>

            <h2 class="rule-title">ACTIONS</h2>
            <ul>
                <li><b>Play:</b> Stack cards +1 in ascending order.</li>
                <li><b>Shuffle Horizon:</b> Recycle Horizon cards.</li>
                <li><b>Harmonic Sync:</b> 3 same-suit cards in a row = Draw 1 Free.</li>
                <li><b>Jokers (★):</b> Wild. Play anywhere.</li>
            </ul>

            <h2 class="rule-title" style="color:#F50057">THE VOID</h2>
            <p class="rule-text">Rolling the die adds Void Cards. Manage them carefully.</p>
        </div>

    </div>

    <div id="log">Protocol v2.3<br>Integrated Layout Active.</div>
    <div id="die-result"></div>
    
    <div id="healing-popup">
        <div class="healing-text">HEALING IS FOR EVERYONE</div>
        <button class="btn" style="font-size: 20px; padding: 15px 40px;" onclick="acceptHealing()">RECEIVE GIFT (DRAW 1)</button>
    </div>

    <div id="victory">
        <h1 class="peace-text">BE AT PEACE, ALWAYS</h1>
        <div class="score-text" id="final-score">LIGHT PRESERVED: 0</div>
        <button class="btn" onclick="location.reload()" style="margin-top:40px; opacity:0; animation: peaceFadeIn 3s ease-out forwards 3s">BREATHE AGAIN</button>
    </div>

<script>
    // --- CONFIG ---
    const SUITS = ['Water', 'Earth', 'Air', 'Fire'];
    const PILLAR_COUNT = 7;
    const HORIZON_SIZE = 8;
    const DIE_FACES = [1, 1, 2, 2, 3, 'EYE'];
    const VOID_RULES = [
        {s:'Water', r:[1,2,3,4]},
        {s:'Earth', r:[5,6,7]},
        {s:'Air',   r:[8,9,10]},
        {s:'Fire',  r:[11,12,13]}
    ];

    // --- STATE ---
    let pillars = Array(PILLAR_COUNT).fill(1);
    let deck = [];     
    let voidTemplates = []; 
    let hand = [];
    let horizon = [];
    let gameActive = false;
    let lastSuitPlayed = null;
    let suitStreak = 0;

    // --- STATS SYSTEM ---
    function checkDailyReset() {
        let lastDate = localStorage.getItem('sync_last_date');
        let today = new Date().toLocaleDateString('en-US', { timeZone: 'America/Los_Angeles' });
        
        if (lastDate !== today) {
            localStorage.setItem('sync_pillars_today', 0);
            localStorage.setItem('sync_games_today', 0);
            localStorage.setItem('sync_last_date', today);
        }
        updateStatsDisplay();
    }

    function updateStatsDisplay() {
        document.getElementById('daily-pillars').innerText = localStorage.getItem('sync_pillars_today') || 0;
        document.getElementById('daily-games').innerText = localStorage.getItem('sync_games_today') || 0;
    }

    function incrementStat(key) {
        checkDailyReset(); 
        let current = parseInt(localStorage.getItem(key) || 0);
        localStorage.setItem(key, current + 1);
        updateStatsDisplay();
    }

    // --- CLOCK ---
    setInterval(() => {
        let now = new Date();
        let options = { timeZone: 'America/Los_Angeles', hour12: false, hour: '2-digit', minute: '2-digit', second: '2-digit' };
        document.getElementById('pst-clock').innerText = now.toLocaleTimeString('en-US', options);
        if(now.getSeconds() === 0) checkDailyReset(); 
    }, 1000);

    // --- START ---
    function startGame() {
        incrementStat('sync_games_today');
        let screen = document.getElementById('intro-screen');
        screen.style.opacity = 0;
        setTimeout(() => { screen.style.display = 'none'; }, 1500);
        document.getElementById('main-container').style.display = 'flex';
        gameActive = true;
        init();
        log("System: I AM Protocol Initiated.");
    }

    function init() {
        checkDailyReset();
        let tempDeck = [];
        for(let d=0; d<2; d++) {
            for(let s of SUITS) {
                for(let i=2; i<=13; i++) {
                    tempDeck.push({ val: i, suit: s, type: 'standard' });
                }
            }
        }
        for(let j=0; j<4; j++) {
            tempDeck.push({ val: 99, suit: 'Joker', type: 'wild' });
        }

        VOID_RULES.forEach(rule => {
            rule.r.forEach(rank => {
                let firstFound = false;
                let i = tempDeck.length;
                while (i--) {
                    if (tempDeck[i].suit === rule.s && tempDeck[i].val === rank) {
                        if (!firstFound) {
                            let template = {...tempDeck[i]};
                            template.type = 'void-card';
                            voidTemplates.push(template);
                            firstFound = true;
                        }
                        tempDeck.splice(i, 1);
                    }
                }
            });
        });

        deck = tempDeck;
        shuffle(deck);

        for(let i=0; i<HORIZON_SIZE; i++) horizon.push(deck.pop());
        for(let i=0; i<7; i++) hand.push(deck.pop());

        init3DPillars();
        render2D();
    }

    function tryPlay(cardIndex, source) {
        if(!gameActive) return;

        let list = source === 'hand' ? hand : horizon;
        let card = list[cardIndex];
        let targetPillar = -1;

        if(card.type === 'wild') {
            for(let i=0; i<PILLAR_COUNT; i++) {
                if(pillars[i] < 13) {
                    targetPillar = i;
                    break;
                }
            }
        } else {
            for(let i=0; i<PILLAR_COUNT; i++) {
                if(card.val === pillars[i] + 1) {
                    targetPillar = i;
                    break;
                }
            }
        }

        if(targetPillar !== -1) {
            let playVal = card.type === 'wild' ? pillars[targetPillar] + 1 : card.val;
            
            pillars[targetPillar]++;
            list.splice(cardIndex, 1);
            
            if(source === 'horizon' && deck.length > 0) horizon.push(deck.pop());

            log(`Built <b>${valToName(playVal)}</b> on Pillar ${targetPillar+1}`);
            addCardToLadder(targetPillar, playVal, card.suit);
            
            // Sync Check
            if(card.suit !== 'Joker' && card.suit === lastSuitPlayed) {
                suitStreak++;
            } else {
                suitStreak = 1;
                lastSuitPlayed = card.suit;
            }

            if(suitStreak === 3) {
                log(`<span style="color:#00E676">HARMONIC SYNC!</span>`);
                triggerSyncVisual(targetPillar);
                setTimeout(showHealingPopup, 500);
                suitStreak = 0;
            }

            if(playVal === 13) triggerBless(targetPillar);
            if(card.type === 'wild') {
                setTimeout(showHealingPopup, 500); 
                suitStreak = 0;
            }

            render2D();
            checkWin();
        } else {
            log(`Cannot play ${valToName(card.val)}. No match.`);
        }
    }

    function refillHand() {
        let count = 0;
        while(hand.length < 7 && deck.length > 0) {
            hand.push(deck.pop());
            count++;
        }
        log(`Refilled ${count} cards.`);
        render2D();
    }

    function shuffleHorizon() {
        if(!gameActive || horizon.length === 0) return;
        let count = horizon.length;
        while(horizon.length > 0) deck.push(horizon.pop());
        shuffle(deck);
        for(let i=0; i<HORIZON_SIZE; i++) if(deck.length > 0) horizon.push(deck.pop());
        log(`<span style="color:#FFC107">HORIZON SHUFFLED</span>`);
        render2D();
    }

    function showHealingPopup() {
        document.getElementById('healing-popup').style.display = 'flex';
    }

    function acceptHealing() {
        if(deck.length > 0) {
            hand.push(deck.pop());
            log(`<span style="color:#00E676">HEALING RECEIVED:</span> +1 Card`);
        } else {
            log("Light Deck Empty.");
        }
        document.getElementById('healing-popup').style.display = 'none';
        render2D();
    }

    function rollVoidDie() {
        if(!gameActive) return;
        let roll = DIE_FACES[Math.floor(Math.random() * DIE_FACES.length)];
        showDieOverlay(roll === 'EYE' ? '👁️' : roll);

        if(roll === 'EYE') {
            if(deck.length > 0) {
                hand.push(deck.pop());
                log(`<span style="color:#00E676">ROLLED EYE:</span> +1 Light Card.`);
            } else { log("Deck Empty!"); }
        } else {
            let cardsDrawn = 0;
            for(let i=0; i<roll; i++) {
                let randomIndex = Math.floor(Math.random() * voidTemplates.length);
                let template = voidTemplates[randomIndex];
                let newCard = { ...template };
                hand.push(newCard);
                cardsDrawn++;
            }
            log(`<span style="color:#E040FB">ROLLED ${roll}:</span> The Void leaks ${cardsDrawn} cards.`);
        }
        render2D();
    }

    function showDieOverlay(val) {
        let el = document.getElementById('die-result');
        el.innerText = val;
        el.style.opacity = 1;
        setTimeout(() => { el.style.opacity = 0; }, 1000);
    }

    function init3DPillars() {
        const scene = document.getElementById('pillars-scene');
        scene.innerHTML = ''; 

        for(let i=0; i<PILLAR_COUNT; i++) {
            let base = document.createElement('div');
            base.className = 'pillar-base';
            base.id = `pillar-${i}`;
            
            let beam = document.createElement('div');
            beam.className = 'pillar-beam';
            base.appendChild(beam);

            let ring = document.createElement('div');
            ring.className = 'pillar-ring';
            base.appendChild(ring);

            let bText = document.createElement('div');
            bText.className = 'popup-text';
            bText.innerText = "BLESS";
            base.appendChild(bText);

            let ace = document.createElement('div');
            ace.className = 'pillar-card ace';
            ace.innerText = "A";
            ace.style.transform = `translateY(0px)`;
            base.appendChild(ace);

            scene.appendChild(base);
        }
    }

    function addCardToLadder(pillarIndex, val, suit) {
        let base = document.getElementById(`pillar-${pillarIndex}`);
        let card = document.createElement('div');
        card.className = 'pillar-card';
        card.innerText = valToName(val);
        
        if(suit === 'Water') card.style.color = '#006064';
        if(suit === 'Fire') card.style.color = '#B71C1C';
        if(suit === 'Earth') card.style.color = '#33691E';
        if(suit === 'Air') card.style.color = '#E65100';
        if(suit === 'Joker') { 
            card.style.background = 'linear-gradient(135deg, #fff, #E0F7FA)'; 
            card.style.borderColor = '#FFD700';
            card.style.color = '#FFD700'; 
        }
        if(suit === 'Void') { card.style.background = '#000'; card.style.color = '#E040FB'; }

        let height = (val - 1) * -35; 
        card.style.transform = `translateY(${height}px)`;
        base.appendChild(card);
    }

    function triggerBless(index) {
        incrementStat('sync_pillars_today'); 
        
        // Pillar Effects
        let base = document.getElementById(`pillar-${index}`);
        base.classList.add('complete');
        let beam = base.querySelector('.pillar-beam');
        beam.style.height = "500px";
        let text = base.querySelector('.popup-text');
        text.innerText = "BLESS";
        text.classList.remove('float-anim', 'sync-anim');
        void text.offsetWidth; 
        text.classList.add('float-anim');

        // CLOCK FLASH EFFECT
        let clock = document.getElementById('pst-clock');
        let blessText = document.getElementById('bless-float-text');
        let statBox = document.getElementById('stat-box-pillars');

        clock.classList.add('clock-blessed');
        blessText.classList.remove('bless-float-anim');
        void blessText.offsetWidth;
        blessText.classList.add('bless-float-anim');
        
        statBox.classList.add('stat-pop');

        setTimeout(() => {
            clock.classList.remove('clock-blessed');
            statBox.classList.remove('stat-pop');
        }, 2000);
    }

    function triggerSyncVisual(index) {
        let base = document.getElementById(`pillar-${index}`);
        let text = base.querySelector('.popup-text');
        text.innerText = "SYNC";
        text.classList.remove('float-anim', 'sync-anim');
        void text.offsetWidth; 
        text.classList.add('sync-anim');
    }

    function triggerGrandFinale() {
        gameActive = false;
        
        let score = deck.length + hand.length + horizon.length;
        document.getElementById('final-score').innerText = `LIGHT PRESERVED: ${score} CARDS`;

        let beams = document.querySelectorAll('.pillar-beam');
        beams.forEach(b => {
            b.style.height = "1000px"; 
            b.style.background = "linear-gradient(to top, rgba(64, 224, 208, 0.8), transparent)";
        });

        let v = document.getElementById('victory');
        v.style.display = 'flex';
        void v.offsetWidth;
        v.style.backgroundColor = "rgba(11, 16, 38, 0.85)";
    }

    function render2D() {
        let hDiv = document.getElementById('horizon-container');
        hDiv.innerHTML = '';
        horizon.forEach((c, i) => {
            let el = createCard2D(c);
            el.onclick = () => tryPlay(i, 'horizon');
            hDiv.appendChild(el);
        });

        let handDiv = document.getElementById('hand-container');
        handDiv.innerHTML = '';
        hand.sort((a,b)=>a.val - b.val).forEach((c, i) => {
            let el = createCard2D(c);
            el.onclick = () => tryPlay(i, 'hand');
            handDiv.appendChild(el);
        });
    }

    function createCard2D(c) {
        let el = document.createElement('div');
        el.className = `card-2d ${c.suit}`;
        let label = c.suit === 'Joker' ? '★' : valToName(c.val);
        el.innerHTML = `<span style="font-size:22px; font-weight:900">${label}</span><span style="font-size:10px">${c.suit}</span>`;
        return el;
    }

    function valToName(v) {
        if(v===11) return 'J';
        if(v===12) return 'Q';
        if(v===13) return 'K';
        if(v===99) return '★';
        return v;
    }

    function shuffle(a) {
        for(let i=a.length-1; i>0; i--){
            const j=Math.floor(Math.random()*(i+1));
            [a[i],a[j]]=[a[j],a[i]];
        }
    }

    function log(m) {
        let l = document.getElementById('log');
        l.innerHTML = `> ${m}<br>` + l.innerHTML;
    }

    function checkWin() {
        if(pillars.every(p => p === 13)) {
            triggerGrandFinale();
        }
    }
</script>
</body>
</html>
