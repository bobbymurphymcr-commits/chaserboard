<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>THE CHASE — Interactive Board</title>

<style>
:root{
    --blue:#00aaff;
    --blue2:#006eff;
    --cyan:#4ee8ff;
    --red:#ff2020;
    --red2:#a90000;
    --bg:#020914;
    --panel:#061a31;
}

*{
    box-sizing:border-box;
    user-select:none;
}

html,body{
    margin:0;
    width:100%;
    height:100%;
    overflow:hidden;
    font-family:Arial,Helvetica,sans-serif;
    background:#01050d;
    color:#fff;
}

/* =========================================================
   BACKGROUND
   ========================================================= */

body{
    background:
        radial-gradient(circle at 50% 35%,#123c69 0%,#071d39 30%,#020b19 68%,#01050b 100%);
}

body::before{
    content:"";
    position:fixed;
    inset:-30%;
    pointer-events:none;

    background:
        radial-gradient(circle at 20% 50%,rgba(0,150,255,.16),transparent 22%),
        radial-gradient(circle at 80% 50%,rgba(0,90,255,.14),transparent 22%),
        radial-gradient(circle at 50% 5%,rgba(0,200,255,.13),transparent 25%);

    animation:ambient 8s ease-in-out infinite alternate;
}

@keyframes ambient{
    from{transform:scale(1);opacity:.65}
    to{transform:scale(1.12);opacity:1}
}

/* Moving scanlines */

.scanlines{
    position:fixed;
    inset:0;
    pointer-events:none;
    z-index:2;

    background:
        repeating-linear-gradient(
            0deg,
            rgba(255,255,255,.015) 0px,
            rgba(255,255,255,.015) 1px,
            transparent 1px,
            transparent 5px
        );

    opacity:.4;
}

/* =========================================================
   HEADER
   ========================================================= */

.header{
    position:relative;
    z-index:20;
    height:90px;
    text-align:center;
    padding-top:13px;
}

.title{
    font-size:clamp(30px,4.8vw,62px);
    font-weight:1000;
    letter-spacing:9px;

    color:#fff;

    text-shadow:
        0 0 4px #fff,
        0 0 10px var(--cyan),
        0 0 25px var(--blue),
        0 0 50px var(--blue2),
        0 0 90px rgba(0,100,255,.8);
}

.subtitle{
    margin-top:0;
    color:#72d8ff;
    font-size:12px;
    font-weight:bold;
    letter-spacing:5px;
    text-shadow:0 0 10px #008cff;
}

/* LIVE badge */

.live{
    position:absolute;
    left:22px;
    top:18px;

    display:flex;
    align-items:center;
    gap:8px;

    padding:7px 12px;
    border:1px solid rgba(255,255,255,.3);
    border-radius:20px;

    background:rgba(0,10,25,.75);

    font-size:11px;
    font-weight:900;
    letter-spacing:2px;

    box-shadow:0 0 15px rgba(0,140,255,.3);
}

.live-dot{
    width:8px;
    height:8px;
    border-radius:50%;
    background:#ff2222;

    box-shadow:
        0 0 6px red,
        0 0 15px red;

    animation:livePulse 1s infinite alternate;
}

@keyframes livePulse{
    from{opacity:.5;transform:scale(.8)}
    to{opacity:1;transform:scale(1.2)}
}

/* =========================================================
   MAIN BOARD
   ========================================================= */

.main{
    position:relative;
    z-index:5;

    height:calc(100vh - 195px);

    display:flex;
    align-items:center;
    justify-content:center;
}

.board-wrap{
    position:relative;

    width:min(680px,67vw);
    height:min(620px,68vh);

    min-height:400px;
}

/* Outer glow */

.board-wrap::before{
    content:"";
    position:absolute;
    inset:-25px;

    background:radial-gradient(
        ellipse,
        rgba(0,130,255,.25),
        transparent 65%
    );

    filter:blur(18px);
    pointer-events:none;
}

/* =========================================================
   BOARD
   ========================================================= */

.board{
    position:absolute;
    inset:0;

    padding:22px 55px 26px;

    display:flex;
    flex-direction:column;
    justify-content:space-between;

    transform:perspective(1100px) rotateX(2deg);

    clip-path:polygon(
        11% 0,
        89% 0,
        100% 100%,
        0 100%
    );

    background:
        linear-gradient(
            90deg,
            rgba(1,21,47,.95),
            rgba(4,49,91,.95) 50%,
            rgba(1,21,47,.95)
        );

    box-shadow:
        inset 0 0 50px rgba(0,120,255,.25),
        inset 0 0 120px rgba(0,80,200,.15),
        0 0 20px #008cff,
        0 0 60px rgba(0,110,255,.55);
}

/* Bright rails */

.board::before{
    content:"";
    position:absolute;
    inset:0;

    clip-path:polygon(
        11% 0,
        89% 0,
        100% 100%,
        0 100%
    );

    border-left:4px solid #159cff;
    border-right:4px solid #159cff;

    box-shadow:
        inset 0 0 20px rgba(0,180,255,.5),
        0 0 15px #008cff,
        0 0 50px rgba(0,130,255,.7);

    pointer-events:none;
}

/* Moving energy */

.board::after{
    content:"";

    position:absolute;
    left:8%;
    right:8%;
    top:-20%;
    height:140%;

    background:
        linear-gradient(
            180deg,
            transparent,
            rgba(60,210,255,.18),
            transparent 45%
        );

    animation:energy 3.5s linear infinite;
    pointer-events:none;
}

@keyframes energy{
    from{transform:translateY(-60%)}
    to{transform:translateY(60%)}
}

/* =========================================================
   STEPS
   ========================================================= */

.step{
    position:relative;
    z-index:8;

    height:10.7%;
    min-height:43px;

    display:flex;
    align-items:center;
    justify-content:center;

    border:2px solid #126fb9;

    background:
        linear-gradient(
            90deg,
            rgba(1,22,49,.98),
            rgba(8,62,111,.98) 50%,
            rgba(1,22,49,.98)
        );

    box-shadow:
        inset 0 0 15px rgba(0,130,255,.2),
        0 0 5px rgba(0,130,255,.35);

    transform:skewX(-3deg);

    transition:
        background .3s,
        border .3s,
        box-shadow .3s,
        transform .3s;
}

.step::after{
    content:"";

    position:absolute;
    left:0;
    right:0;
    bottom:-2px;
    height:1px;

    background:linear-gradient(
        90deg,
        transparent,
        rgba(70,210,255,.8),
        transparent
    );

    box-shadow:0 0 8px #00baff;
}

.number{
    font-size:clamp(18px,2.4vw,31px);
    font-weight:1000;
    color:#55b9ef;

    text-shadow:
        0 0 7px #008cff,
        0 0 18px #006eff;
}

/* Blue */

.step.contestant{
    border-color:#31d2ff;

    background:
        linear-gradient(
            90deg,
            #004d99,
            #008cff 30%,
            #00cfff 50%,
            #008cff 70%,
            #004d99
        );

    box-shadow:
        inset 0 0 25px rgba(255,255,255,.22),
        inset 0 0 40px rgba(0,200,255,.55),
        0 0 10px #00cfff,
        0 0 30px #008cff,
        0 0 65px rgba(0,120,255,.8);

    animation:blueStep 1.2s infinite alternate;
}

.step.contestant .number{
    color:#fff;
    text-shadow:
        0 0 5px white,
        0 0 15px #00d9ff,
        0 0 30px #008cff;
}

/* Red */

.step.chaser{
    border-color:#ff3d3d;

    background:
        linear-gradient(
            90deg,
            #610000,
            #c40000 30%,
            #ff2525 50%,
            #c40000 70%,
            #610000
        );

    box-shadow:
        inset 0 0 25px rgba(255,100,100,.18),
        inset 0 0 40px rgba(255,0,0,.45),
        0 0 10px #ff1515,
        0 0 30px #e00000,
        0 0 65px rgba(255,0,0,.7);

    animation:redStep 1s infinite alternate;
}

.step.chaser .number{
    color:#fff;
    text-shadow:
        0 0 5px white,
        0 0 15px #ff6060,
        0 0 30px red;
}

/* Caught square */

.step.caught{
    background:
        linear-gradient(
            90deg,
            #5a0000,
            #ff0000,
            #fff,
            #ff0000,
            #5a0000
        );

    border-color:#fff;

    box-shadow:
        0 0 12px white,
        0 0 30px red,
        0 0 70px red,
        inset 0 0 35px white;

    animation:caughtFlash .22s infinite alternate;
}

@keyframes blueStep{
    from{filter:brightness(.95)}
    to{filter:brightness(1.25)}
}

@keyframes redStep{
    from{filter:brightness(.95)}
    to{filter:brightness(1.3)}
}

@keyframes caughtFlash{
    from{transform:skewX(-3deg) scale(1)}
    to{transform:skewX(-3deg) scale(1.025)}
}

/* =========================================================
   HOME
   ========================================================= */

.home{
    position:relative;
    z-index:9;

    height:12%;
    min-height:50px;

    display:flex;
    align-items:center;
    justify-content:center;

    border:3px solid #55eaff;

    background:
        linear-gradient(
            90deg,
            #003c5d,
            #008fc4 35%,
            #00d8ff 50%,
            #008fc4 65%,
            #003c5d
        );

    box-shadow:
        inset 0 0 30px rgba(255,255,255,.25),
        inset 0 0 45px rgba(0,220,255,.55),
        0 0 12px #00d8ff,
        0 0 35px #009dff,
        0 0 70px rgba(0,180,255,.65);

    font-size:clamp(23px,3vw,38px);
    font-weight:1000;
    letter-spacing:8px;

    text-shadow:
        0 0 6px white,
        0 0 16px #00e5ff,
        0 0 32px #008cff;

    animation:homePulse 2s infinite alternate;
}

@keyframes homePulse{
    from{filter:brightness(.95)}
    to{filter:brightness(1.18)}
}

/* =========================================================
   PIECES
   ========================================================= */

.piece{
    position:absolute;
    z-index:25;

    width:78px;
    height:78px;

    border-radius:50%;

    display:flex;
    align-items:center;
    justify-content:center;

    font-size:9px;
    font-weight:1000;
    text-align:center;

    transition:
        top .5s cubic-bezier(.2,.9,.25,1),
        left .5s,
        right .5s,
        transform .18s;

    pointer-events:none;
}

.piece::before{
    content:"";

    position:absolute;
    inset:-10px;

    border-radius:50%;
    opacity:.7;
}

.piece::after{
    content:"";

    position:absolute;
    inset:7px;

    border-radius:50%;

    border:1px solid rgba(255,255,255,.65);
}

.piece span{
    position:relative;
    z-index:3;

    text-shadow:
        0 2px 5px #000,
        0 0 8px white;
}

/* Contestant */

.contestant-piece{
    left:-112px;

    background:
        radial-gradient(
            circle at 30% 23%,
            #fff,
            #79e1ff 12%,
            #008fff 48%,
            #003c99 100%
        );

    border:4px solid #c6f6ff;

    box-shadow:
        0 0 8px white,
        0 0 20px #00d8ff,
        0 0 45px #008cff,
        0 0 80px rgba(0,120,255,.8);

    animation:pieceBlue 1.1s infinite alternate;
}

.contestant-piece::before{
    background:#00baff;
    box-shadow:
        0 0 25px #00d8ff,
        0 0 55px #008cff,
        0 0 90px #0066ff;
}

/* Chaser */

.chaser-piece{
    right:-112px;

    background:
        radial-gradient(
            circle at 30% 23%,
            #fff,
            #ff7777 12%,
            #e40000 48%,
            #690000 100%
        );

    border:4px solid #ffcaca;

    box-shadow:
        0 0 8px white,
        0 0 20px #ff4444,
        0 0 45px red,
        0 0 80px rgba(255,0,0,.8);

    animation:pieceRed 1s infinite alternate;
}

.chaser-piece::before{
    background:red;
    box-shadow:
        0 0 25px #ff3333,
        0 0 55px red,
        0 0 90px #a00000;
}

@keyframes pieceBlue{
    from{transform:scale(1)}
    to{transform:scale(1.055)}
}

@keyframes pieceRed{
    from{transform:scale(1)}
    to{transform:scale(1.06)}
}

/* =========================================================
   OPERATOR PANEL
   ========================================================= */

.controls{
    position:fixed;
    z-index:50;

    left:0;
    right:0;
    bottom:0;

    min-height:105px;

    padding:8px 15px 14px;

    display:flex;
    align-items:center;
    justify-content:center;
    gap:9px;
    flex-wrap:wrap;

    background:
        linear-gradient(
            180deg,
            rgba(2,13,29,.35),
            rgba(1,8,18,.96)
        );

    border-top:1px solid rgba(0,150,255,.3);

    box-shadow:
        0 -15px 45px rgba(0,0,0,.35),
        inset 0 1px 0 rgba(80,190,255,.1);
}

button{
    border:1px solid rgba(255,255,255,.3);
    border-radius:8px;

    padding:12px 18px;

    color:#fff;

    font-weight:1000;
    font-size:12px;

    cursor:pointer;

    transition:.15s;

    min-width:120px;
}

button:hover{
    transform:translateY(-2px);
    filter:brightness(1.25);
}

button:active{
    transform:scale(.97);
}

.blueBtn{
    background:linear-gradient(#00aaff,#0054b8);
    box-shadow:0 0 10px rgba(0,150,255,.55);
}

.redBtn{
    background:linear-gradient(#ff4141,#a40000);
    box-shadow:0 0 10px rgba(255,0,0,.5);
}

.greenBtn{
    background:linear-gradient(#00a66a,#00623f);
    box-shadow:0 0 10px rgba(0,220,140,.35);
}

.darkBtn{
    background:linear-gradient(#50657b,#202b3a);
}

.goldBtn{
    background:linear-gradient(#e2a900,#885c00);
    box-shadow:0 0 10px rgba(255,190,0,.4);
}

/* Start controls */

.start-controls{
    width:100%;
    display:flex;
    justify-content:center;
    gap:7px;
}

.start-controls button{
    min-width:105px;
    padding:7px 10px;
    font-size:10px;
    background:linear-gradient(#173e67,#071a31);
    border-color:#3b98d5;
}

.start-controls button.selected{
    background:linear-gradient(#009fff,#0053a7);
    border-color:#a8efff;
    box-shadow:0 0 12px #00baff;
}

/* =========================================================
   SIDE INFORMATION
   ========================================================= */

.info{
    position:fixed;
    z-index:30;

    left:20px;
    top:100px;

    width:165px;

    padding:13px;

    background:rgba(2,17,36,.78);
    border:1px solid rgba(0,150,255,.4);
    border-radius:10px;

    box-shadow:0 0 25px rgba(0,100,255,.2);

    font-size:11px;
}

.info-title{
    color:#62d8ff;
    font-weight:1000;
    letter-spacing:2px;
    margin-bottom:8px;
}

.info-row{
    display:flex;
    justify-content:space-between;
    padding:4px 0;
    border-bottom:1px solid rgba(255,255,255,.06);
}

.blueText{color:#4edbff}
.redText{color:#ff6666}

@media(max-width:900px){
    .info{display:none}
}

/* =========================================================
   FULLSCREEN BUTTON
   ========================================================= */

.fullscreen{
    position:fixed;
    z-index:60;

    right:20px;
    bottom:120px;

    min-width:auto;
    width:42px;
    height:42px;

    padding:0;

    border-radius:50%;

    background:#09213b;
    border:1px solid #328bc7;

    box-shadow:0 0 12px rgba(0,130,255,.4);

    font-size:18px;
}

/* =========================================================
   RESULT OVERLAY
   ========================================================= */

.overlay{
    position:fixed;
    z-index:200;

    inset:0;

    display:none;
    align-items:center;
    justify-content:center;

    background:
        radial-gradient(
            circle at center,
            rgba(0,70,120,.55),
            rgba(0,0,0,.9)
        );

    backdrop-filter:blur(8px);
}

.overlay.show{
    display:flex;
    animation:overlayIn .3s ease;
}

@keyframes overlayIn{
    from{opacity:0}
    to{opacity:1}
}

.result{
    text-align:center;

    padding:55px 85px;

    border-radius:25px;

    background:
        radial-gradient(
            circle at 50% 20%,
            #116aa3,
            #06274a 55%,
            #020c18
        );

    border:3px solid #37d5ff;

    box-shadow:
        0 0 10px white,
        0 0 35px #00baff,
        0 0 90px rgba(0,130,255,.8),
        inset 0 0 50px rgba(0,150,255,.25);

    animation:resultPop .45s cubic-bezier(.2,.9,.2,1);
}

.result.caught{
    background:
        radial-gradient(
            circle at 50% 20%,
            #a50000,
            #450000 55%,
            #120000
        );

    border-color:#ff5555;

    box-shadow:
        0 0 10px white,
        0 0 40px red,
        0 0 100px rgba(255,0,0,.9),
        inset 0 0 50px rgba(255,0,0,.25);
}

@keyframes resultPop{
    from{
        transform:scale(.7) rotate(-2deg);
        opacity:0;
    }
    to{
        transform:scale(1) rotate(0);
        opacity:1;
    }
}

.result-title{
    font-size:clamp(50px,9vw,110px);
    font-weight:1000;
    letter-spacing:8px;

    text-shadow:
        0 0 5px white,
        0 0 15px #00d8ff,
        0 0 35px #008cff,
        0 0 75px #006cff;
}

.result.caught .result-title{
    text-shadow:
        0 0 5px white,
        0 0 15px #ff9999,
        0 0 35px red,
        0 0 75px red;
}

.result-sub{
    margin-top:10px;

    color:#c9ecff;
    font-size:18px;
    font-weight:bold;
}

.result button{
    margin-top:22px;
}

/* =========================================================
   PARTICLES
   ========================================================= */

.particle{
    position:fixed;
    z-index:190;

    width:6px;
    height:6px;

    border-radius:50%;

    pointer-events:none;

    animation:particleFly 1.7s ease-out forwards;
}

@keyframes particleFly{
    from{
        transform:translate(0,0) scale(1);
        opacity:1;
    }
    to{
        transform:translate(var(--x),var(--y)) scale(0);
        opacity:0;
    }
}

/* =========================================================
   FLASH
   ========================================================= */

.flash{
    position:fixed;
    z-index:180;
    inset:0;

    pointer-events:none;

    opacity:0;
}

.flash.catch{
    animation:catchFlash .6s ease-out;
    background:rgba(255,0,0,.45);
}

.flash.home{
    animation:homeFlash .8s ease-out;
    background:rgba(0,200,255,.3);
}

@keyframes catchFlash{
    0%{opacity:0}
    15%{opacity:1}
    35%{opacity:.15}
    55%{opacity:.8}
    100%{opacity:0}
}

@keyframes homeFlash{
    0%{opacity:0}
    20%{opacity:.9}
    100%{opacity:0}
}

/* =========================================================
   MOBILE
   ========================================================= */

@media(max-width:700px){

    .header{
        height:70px;
    }

    .title{
        letter-spacing:5px;
    }

    .subtitle{
        font-size:9px;
    }

    .live{
        display:none;
    }

    .main{
        height:calc(100vh - 165px);
    }

    .board-wrap{
        width:62vw;
        height:58vh;
        min-height:350px;
    }

    .board{
        padding-left:28px;
        padding-right:28px;
    }

    .piece{
        width:57px;
        height:57px;
        font-size:7px;
    }

    .contestant-piece{
        left:-76px;
    }

    .chaser-piece{
        right:-76px;
    }

    .controls{
        min-height:95px;
    }

    button{
        min-width:105px;
        padding:10px 12px;
        font-size:10px;
    }

    .start-controls button{
        min-width:85px;
        font-size:8px;
    }

    .result{
        width:88vw;
        padding:40px 20px;
    }
}
</style>
</head>

<body>

<div class="scanlines"></div>

<header class="header">

    <div class="live">
        <span class="live-dot"></span>
        LIVE
    </div>

    <div class="title">THE CHASE</div>
    <div class="subtitle">THE HEAD-TO-HEAD</div>

</header>

<div class="info">

    <div class="info-title">CHASE STATUS</div>

    <div class="info-row">
        <span>CONTESTANT</span>
        <span class="blueText" id="contestantStatus">4</span>
    </div>

    <div class="info-row">
        <span>CHASER</span>
        <span class="redText" id="chaserStatus">1</span>
    </div>

    <div class="info-row">
        <span>GAP</span>
        <span id="gapStatus">3</span>
    </div>

</div>

<button class="fullscreen" onclick="toggleFullscreen()">⛶</button>

<main class="main">

    <div class="board-wrap">

        <div class="board" id="board">

            <div
                class="piece contestant-piece"
                id="contestantPiece">

                <span>CONTESTANT</span>

            </div>

            <div
                class="piece chaser-piece"
                id="chaserPiece">

                <span>CHASER</span>

            </div>

            <div class="step" data-pos="1">
                <span class="number">1</span>
            </div>

            <div class="step" data-pos="2">
                <span class="number">2</span>
            </div>

            <div class="step" data-pos="3">
                <span class="number">3</span>
            </div>

            <div class="step" data-pos="4">
                <span class="number">4</span>
            </div>

            <div class="step" data-pos="5">
                <span class="number">5</span>
            </div>

            <div class="step" data-pos="6">
                <span class="number">6</span>
            </div>

            <div class="step" data-pos="7">
                <span class="number">7</span>
            </div>

            <div class="home">
                HOME
            </div>

        </div>

    </div>

</main>

<!-- Controls -->

<div class="controls">

    <div class="start-controls">

        <button id="start2"
            onclick="setStart(2)">
            HIGH — 2 AHEAD
        </button>

        <button id="start3"
            onclick="setStart(3)">
            STANDARD — 3 AHEAD
        </button>

        <button id="start4"
            onclick="setStart(4)">
            LOW — 4 AHEAD
        </button>

    </div>

    <button
        class="blueBtn"
        onclick="moveContestant(1)">
        🔵 CONTESTANT +1
    </button>

    <button
        class="redBtn"
        onclick="moveChaser(1)">
        🔴 CHASER +1
    </button>

    <button
        class="darkBtn"
        onclick="undo()">
        ↶ UNDO
    </button>

    <button
        class="greenBtn"
        onclick="resetGame()">
        ↻ RESET
    </button>

</div>

<!-- Flash -->

<div class="flash" id="flash"></div>

<!-- Result -->

<div class="overlay" id="overlay">

    <div class="result" id="result">

        <div
            class="result-title"
            id="resultTitle">
            CAUGHT!
        </div>

        <div
            class="result-sub"
            id="resultSub">
            The Chaser has caught the contestant.
        </div>

        <button
            class="darkBtn"
            onclick="closeResult()">
            CONTINUE
        </button>

    </div>

</div>

<script>

/* =========================================================
   GAME STATE
   ========================================================= */

let contestant = 4;
let chaser = 1;

let history = [];

let gameOver = false;


/* =========================================================
   AUDIO ENGINE
   ========================================================= */

let audioContext = null;

function audio(){

    if(!audioContext){
        audioContext =
            new (window.AudioContext ||
                 window.webkitAudioContext)();
    }

    if(audioContext.state === "suspended"){
        audioContext.resume();
    }

    return audioContext;
}

function tone(
    frequency,
    duration=.12,
    type="sine",
    volume=.05
){

    try{

        const ctx = audio();

        const osc =
            ctx.createOscillator();

        const gain =
            ctx.createGain();

        osc.type = type;
        osc.frequency.value = frequency;

        gain.gain.setValueAtTime(
            volume,
            ctx.currentTime
        );

        gain.gain.exponentialRampToValueAtTime(
            .001,
            ctx.currentTime + duration
        );

        osc.connect(gain);
        gain.connect(ctx.destination);

        osc.start();

        osc.stop(
            ctx.currentTime + duration
        );

    }catch(e){}
}

function moveSound(player){

    if(player==="contestant"){

        tone(440,.08,"sine",.035);

        setTimeout(
            ()=>tone(660,.13,"sine",.04),
            55
        );

    }else{

        tone(180,.1,"sawtooth",.035);

        setTimeout(
            ()=>tone(130,.15,"sawtooth",.045),
            70
        );
    }
}

function caughtSound(){

    [160,120,90,65].forEach(
        (f,i)=>{
            setTimeout(
                ()=>tone(f,.2,"sawtooth",.07),
                i*100
            );
        }
    );
}

function homeSound(){

    [523,659,784,1046].forEach(
        (f,i)=>{
            setTimeout(
                ()=>tone(f,.22,"sine",.055),
                i*100
            );
        }
    );
}


/* =========================================================
   BOARD
   ========================================================= */

function render(){

    const steps =
        document.querySelectorAll(".step");

    steps.forEach(step=>{

        step.classList.remove(
            "contestant",
            "chaser",
            "caught"
        );

        const p =
            Number(step.dataset.pos);

        if(
            p===contestant &&
            p===chaser
        ){

            step.classList.add("caught");

        }else{

            if(p===contestant)
                step.classList.add("contestant");

            if(p===chaser)
                step.classList.add("chaser");

        }

    });

    positionPiece(
        document.getElementById("contestantPiece"),
        contestant
    );

    positionPiece(
        document.getElementById("chaserPiece"),
        chaser
    );

    document.getElementById(
        "contestantStatus"
    ).textContent=contestant;

    document.getElementById(
        "chaserStatus"
    ).textContent=chaser;

    document.getElementById(
        "gapStatus"
    ).textContent=
        Math.max(0,contestant-chaser);

}


/* =========================================================
   PIECES
   ========================================================= */

function positionPiece(piece,pos){

    const actual =
        Math.max(1,Math.min(7,pos));

    const step =
        document.querySelector(
            `.step[data-pos="${actual}"]`
        );

    if(!step)return;

    piece.style.top =
        step.offsetTop +
        step.offsetHeight/2 -
        piece.offsetHeight/2 +
        "px";
}


/* =========================================================
   SAVE HISTORY
   ========================================================= */

function saveHistory(){

    history.push({
        contestant,
        chaser
    });

    if(history.length>30)
        history.shift();
}


/* =========================================================
   MOVE CONTESTANT
   ========================================================= */

function moveContestant(amount){

    if(gameOver)return;

    saveHistory();

    contestant += amount;

    contestant =
        Math.max(1,Math.min(8,contestant));

    moveSound("contestant");

    render();

    checkEnd();
}


/* =========================================================
   MOVE CHASER
   ========================================================= */

function moveChaser(amount){

    if(gameOver)return;

    saveHistory();

    chaser += amount;

    chaser =
        Math.max(1,Math.min(8,chaser));

    moveSound("chaser");

    render();

    checkEnd();
}


/* =========================================================
   STARTING POSITION
   ========================================================= */

function setStart(ahead){

    contestant = 1 + ahead;
    chaser = 1;

    history=[];

    gameOver=false;

    document
        .querySelectorAll(".start-controls button")
        .forEach(
            b=>b.classList.remove("selected")
        );

    const id =
        ahead===2
        ?"start2"
        :ahead===3
        ?"start3"
        :"start4";

    document
        .getElementById(id)
        .classList.add("selected");

    closeResult();

    render();

    tone(520,.1,"sine",.04);
}


/* =========================================================
   UNDO
   ========================================================= */

function undo(){

    if(!history.length)return;

    const previous =
        history.pop();

    contestant =
        previous.contestant;

    chaser =
        previous.chaser;

    gameOver=false;

    closeResult();

    render();

    tone(300,.1,"triangle",.035);
}


/* =========================================================
   RESET
   ========================================================= */

function resetGame(){

    contestant=4;
    chaser=1;

    history=[];
    gameOver=false;

    document
        .querySelectorAll(".start-controls button")
        .forEach(
            b=>b.classList.remove("selected")
        );

    document
        .getElementById("start3")
        .classList.add("selected");

    closeResult();

    render();

    tone(220,.08,"triangle",.035);

    setTimeout(
        ()=>tone(330,.12,"triangle",.035),
        70
    );
}


/* =========================================================
   END CONDITIONS
   ========================================================= */

function checkEnd(){

    if(
        contestant===chaser &&
        contestant<8
    ){

        gameOver=true;

        flash("catch");

        caughtSound();

        setTimeout(
            ()=>showResult(
                true,
                "CAUGHT!",
                "The Chaser has caught the contestant."
            ),
            350
        );

        return;
    }

    if(contestant>=8){

        gameOver=true;

        flash("home");

        homeSound();

        createParticles();

        setTimeout(
            ()=>showResult(
                false,
                "HOME!",
                "The contestant has made it safely home."
            ),
            350
        );
    }
}


/* =========================================================
   RESULT SCREEN
   ========================================================= */

function showResult(caught,title,subtitle){

    document.getElementById(
        "resultTitle"
    ).textContent=title;

    document.getElementById(
        "resultSub"
    ).textContent=subtitle;

    const result =
        document.getElementById("result");

    result.classList.toggle(
        "caught",
        caught
    );

    document
        .getElementById("overlay")
        .classList.add("show");
}

function closeResult(){

    document
        .getElementById("overlay")
        .classList.remove("show");
}


/* =========================================================
   FLASH
   ========================================================= */

function flash(type){

    const f =
        document.getElementById("flash");

    f.className="flash "+type;

    void f.offsetWidth;

    f.className="flash "+type;
}


/* =========================================================
   CONFETTI / PARTICLES
   ========================================================= */

function createParticles(){

    const colours=[
        "#00d9ff",
        "#ffffff",
        "#008cff",
        "#55eeff",
        "#00ffae"
    ];

    for(let i=0;i<100;i++){

        const p =
            document.createElement("div");

        p.className="particle";

        p.style.background =
            colours[
                Math.floor(
                    Math.random()*colours.length
                )
            ];

        p.style.boxShadow=
            `0 0 10px ${p.style.background}`;

        p.style.left=
            (40+Math.random()*20)+"vw";

        p.style.top=
            (35+Math.random()*20)+"vh";

        p.style.setProperty(
            "--x",
            ((Math.random()-.5)*900)+"px"
        );

        p.style.setProperty(
            "--y",
            ((Math.random()-.8)*700)+"px"
        );

        document.body.appendChild(p);

        setTimeout(
            ()=>p.remove(),
            1800
        );
    }
}


/* =========================================================
   FULLSCREEN
   ========================================================= */

function toggleFullscreen(){

    if(!document.fullscreenElement){

        document.documentElement
            .requestFullscreen()
            .catch(()=>{});

    }else{

        document.exitFullscreen();
    }
}


/* =========================================================
   KEYBOARD CONTROLS
   =========================================================

   → contestant forward
   ← contestant backward
   ↓ chaser forward
   ↑ chaser backward
   R reset
   Z undo
   F fullscreen
   ESC close result
*/

document.addEventListener(
    "keydown",
    e=>{

        if(e.key==="ArrowRight"){
            e.preventDefault();
            moveContestant(1);
        }

        else if(e.key==="ArrowLeft"){
            e.preventDefault();
            moveContestant(-1);
        }

        else if(e.key==="ArrowDown"){
            e.preventDefault();
            moveChaser(1);
        }

        else if(e.key==="ArrowUp"){
            e.preventDefault();
            moveChaser(-1);
        }

        else if(e.key.toLowerCase()==="r"){
            resetGame();
        }

        else if(e.key.toLowerCase()==="z"){
            undo();
        }

        else if(e.key.toLowerCase()==="f"){
            toggleFullscreen();
        }

        else if(e.key==="Escape"){
            closeResult();
        }

    }
);


/* =========================================================
   INITIALISE
   ========================================================= */

document
    .getElementById("start3")
    .classList.add("selected");

window.addEventListener(
    "resize",
    render
);

setTimeout(
    render,
    150
);

</script>

</body>
</html>
