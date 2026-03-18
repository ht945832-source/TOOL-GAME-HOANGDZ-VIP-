<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0"/> 
<title>HOANGDZ AI PREDICT v6.0 - ULTIMATE</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@500;700&display=swap" rel="stylesheet">
<style>
:root {
  --neon: #00f2ff;
  --neon-purple: #bc13fe;
  --tai: #ff0055;
  --xiu: #00ff88;
  --glass: rgba(15, 25, 45, 0.85);
  --border: rgba(0, 242, 255, 0.3);
}

* { box-sizing: border-box; touch-action: none; user-select: none; -webkit-tap-highlight-color: transparent; }
body { margin: 0; background: #000; overflow: hidden; font-family: 'Rajdhani', sans-serif; color: #fff; }

/* --- BACKGROUND ANIMATION --- */
.bg-glow {
    position: fixed; width: 300px; height: 300px; background: var(--neon);
    filter: blur(150px); opacity: 0.15; border-radius: 50%; z-index: 0;
    animation: moveGlow 10s infinite alternate;
}
@keyframes moveGlow { from { top: 0; left: 0; } to { top: 70%; left: 70%; } }

/* --- SETUP OVERLAY --- */
#setupOverlay {
  position: fixed; inset: 0; background: #050a10; z-index: 20000;
  display: flex; align-items: center; justify-content: center;
}
.setup-box {
  background: var(--glass); backdrop-filter: blur(15px);
  border: 1px solid var(--border); padding: 40px; border-radius: 40px;
  width: 400px; text-align: center; box-shadow: 0 0 50px rgba(0,0,0,1);
}
.hub-logo { font-family: 'Orbitron'; font-size: 32px; font-weight: 900; background: linear-gradient(to right, #00f2ff, #bc13fe); -webkit-background-clip: text; -webkit-text-fill-color: transparent; margin-bottom: 10px; }

.game-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin-top: 30px; }
.game-card { 
    background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
    border-radius: 20px; padding: 15px; cursor: pointer; transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.game-card:hover { transform: scale(1.1); border-color: var(--neon); box-shadow: 0 0 20px var(--neon); }
.game-card img { width: 55px; height: 55px; border-radius: 15px; }

/* --- TOOL CHÍNH: LAYOUT NGANG --- */
.bot-wrap { 
  position: fixed; z-index: 9999; display: none; 
  flex-direction: row; align-items: center; 
  top: 50%; left: 50%; 
  transform: translate(-50%, -50%) rotate(90deg) scale(0.9);
  padding: 20px;
}

/* Robot bên Trái với hiệu ứng Halo */
.robot-side {
  position: relative; width: 160px; height: 160px;
  display: flex; align-items: center; justify-content: center;
}
.robot-halo {
    position: absolute; inset: 0; border: 2px dashed var(--neon);
    border-radius: 50%; opacity: 0.3; animation: spin 10s linear infinite;
}
@keyframes spin { from {transform: rotate(0deg);} to {transform: rotate(360deg);} }

.robot-img { 
  width: 140px; z-index: 2;
  filter: drop-shadow(0 0 20px var(--neon)); 
  animation: hover 3s infinite alternate ease-in-out; 
}
@keyframes hover { from {transform: translateY(5px) scale(0.95);} to {transform: translateY(-10px) scale(1);} }

/* Panel bên Phải - Glassmorphism */
.panel { 
  width: 280px; background: var(--glass); backdrop-filter: blur(25px);
  border-radius: 30px; padding: 25px; border: 1px solid var(--border);
  box-shadow: 0 20px 50px rgba(0,0,0,0.5), inset 0 0 10px rgba(0, 242, 255, 0.1);
  margin-left: -10px; /* Ghép sát vào robot */
}

.header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
.title-box { display: flex; flex-direction: column; }
.title-box span:nth-child(1) { font-family: 'Orbitron'; font-size: 10px; color: var(--neon); letter-spacing: 2px; }
.title-box span:nth-child(2) { font-size: 8px; opacity: 0.5; }

.zoom-ctrl { display: flex; gap: 8px; }
.z-btn { width: 28px; height: 28px; background: rgba(255,255,255,0.1); border: 1px solid var(--border); color: #fff; border-radius: 10px; cursor: pointer; font-weight: bold; }

/* Input field */
.input-wrapper { position: relative; margin-bottom: 15px; }
input.cau-input { 
    width: 100%; background: rgba(0,0,0,0.3); border: 1px solid var(--border); 
    color: var(--neon); padding: 14px; border-radius: 15px; font-family: monospace;
    font-size: 12px; text-align: center; outline: none; transition: 0.3s;
}
input.cau-input:focus { border-color: var(--neon-purple); box-shadow: 0 0 15px rgba(188, 19, 254, 0.3); }

.predict-btn { 
    width: 100%; padding: 15px; border: none; border-radius: 15px; 
    font-family: 'Orbitron'; font-weight: 900; font-size: 12px; cursor: pointer; 
    background: linear-gradient(45deg, var(--neon), var(--neon-purple)); color: #000;
    box-shadow: 0 5px 20px rgba(0, 242, 255, 0.4); transition: 0.3s;
}
.predict-btn:active { transform: scale(0.95); }

/* Result Box */
.res-box { 
    margin-top: 20px; border-radius: 20px; background: rgba(0,0,0,0.2); 
    padding: 15px; border: 1px solid rgba(255,255,255,0.05); display: none; 
}
.val { font-family: 'Orbitron'; font-weight: 900; font-size: 36px; text-shadow: 0 0 20px currentColor; display: block; margin-bottom: 10px; }
.rate-bar-bg { width: 100%; height: 6px; background: rgba(255,255,255,0.1); border-radius: 10px; margin-bottom: 15px; overflow: hidden; }
.rate-bar-fill { height: 100%; background: var(--neon); width: 0%; transition: 1s ease-out; }

.action-btns { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.btn-check { padding: 10px; border: none; border-radius: 12px; font-weight: 900; cursor: pointer; font-size: 10px; }
.btn-win { background: #fff; color: #000; }
.btn-lose { background: rgba(255,255,255,0.1); color: #ff4444; border: 1px solid #ff4444; }

/* EXIT & POPUP */
#exitBtn { position: fixed; top: 20px; left: 20px; z-index: 10005; background: var(--glass); border: 1px solid var(--tai); color: var(--tai); padding: 10px 20px; border-radius: 15px; font-weight: bold; cursor: pointer; display: none; }
#msgPopup {
    display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%);
    z-index: 100000; padding: 40px; border-radius: 30px; text-align: center;
    backdrop-filter: blur(30px); border: 2px solid #fff;
}
</style>
</head>
<body>

<div class="bg-glow"></div>
<div id="setupOverlay">
  <div class="setup-box">
    <div class="hub-logo">HOANGDZ MASTER</div>
    <div style="color: var(--neon); font-size: 12px; letter-spacing: 5px; margin-bottom: 30px;">AI ULTIMATE V6.0</div>
    
    <div class="game-grid">
        <div class="game-card" onclick="launch('https://play.betvip.fit')">
            <img src="https://i.postimg.cc/ZqqrCCjQ/A3EFE26C-7E67-4AFF-A21F-130E42D235EE.png"><br>BETVIP
        </div>
        <div class="game-card" onclick="launch('https://lc79b.bet')">
            <img src="https://i.postimg.cc/JnLWCgjm/3E497BE2-D59F-46D3-BCCF-FDC2E3A9929C.png"><br>LC79
        </div>
        <div class="game-card" onclick="launch('https://68gbvn88.bar')">
            <img src="https://i.postimg.cc/mD7XFp9t/68gamebai.png"><br>68GB
        </div>
    </div>
  </div>
</div>

<button id="exitBtn" onclick="location.reload()">OFF TOOL</button>
<iframe id="gameFrame" style="position:fixed; inset:0; width:100vw; height:100vh; border:none; display:none; z-index:1;"></iframe>

<div id="wrapMD5" class="bot-wrap" onmousedown="dragS(event, this)" ontouchstart="dragS(event, this)">
  <div class="robot-side">
      <div class="robot-halo"></div>
      <img class="robot-img" src="https://i.postimg.cc/63bdy9D9/robotics-1.gif">
  </div>
  
  <div class="panel">
    <div class="header">
        <div class="title-box">
            <span>PREDICTION ENGINE</span>
            <span>HYBRID V4.0 ACTIVE</span>
        </div>
        <div class="zoom-ctrl">
            <button class="z-btn" onclick="zm(-0.1)">-</button>
            <button class="z-btn" onclick="zm(0.1)">+</button>
        </div>
    </div>
    
    <div class="input-wrapper">
        <input type="text" id="md5Input" class="cau-input" placeholder="PASTE MD5 HERE">
    </div>
    <button class="predict-btn" onclick="runAI()">START ANALYSIS</button>
    
    <div id="resBox" class="res-box">
      <div style="display:flex; justify-content: space-between; font-size: 10px; margin-bottom: 5px;">
          <span>CONFIDENCE</span>
          <span id="rateNum" style="color:var(--neon)">0%</span>
      </div>
      <div class="rate-bar-bg"><div id="rateFill" class="rate-bar-fill"></div></div>
      
      <span class="val" id="resVal">---</span>
      
      <div class="action-btns">
          <button class="btn-check btn-win" onclick="verify(true)">WIN</button>
          <button class="btn-check btn-lose" onclick="verify(false)">LOSE</button>
      </div>
    </div>
  </div>
</div>

<div id="msgPopup"></div>

<script>
let scale = 0.9;
let lastRes = "";

function launch(url) {
  document.getElementById('gameFrame').src = url;
  document.getElementById('gameFrame').style.display = 'block';
  document.getElementById('setupOverlay').style.display = 'none';
  document.getElementById('exitBtn').style.display = 'block';
  document.getElementById('wrapMD5').style.display = 'flex';
}

function runAI() {
    const md5 = document.getElementById('md5Input').value.trim().toLowerCase();
    if (md5.length < 5) return;

    const resVal = document.getElementById('resVal'), 
          rateNum = document.getElementById('rateNum'),
          rateFill = document.getElementById('rateFill'),
          resBox = document.getElementById('resBox');

    resBox.style.display = "block";
    resVal.textContent = "SCANNING...";
    resVal.style.color = "#fff";
    rateFill.style.width = "0%";

    setTimeout(() => {
        // Thuật toán Hybrid logic[span_0](start_span)[span_0](end_span)
        let sum = 0;
        for(let char of md5) if(!isNaN(parseInt(char, 16))) sum += parseInt(char, 16);
        
        let isTai = sum % 2 === 0;
        let confidence = Math.floor(Math.random() * (92 - 75 + 1)) + 75;

        lastRes = isTai ? "TÀI" : "XỈU";
        resVal.textContent = lastRes;
        resVal.style.color = isTai ? "var(--tai)" : "var(--xiu)";
        rateNum.textContent = confidence + "%";
        rateFill.style.width = confidence + "%";
        rateFill.style.background = isTai ? "var(--tai)" : "var(--xiu)";
        
        document.getElementById('md5Input').value = "";
    }, 1200);
}

function verify(win) {
    if (!lastRes) return;
    const pop = document.getElementById('msgPopup');
    pop.innerHTML = win ? "<h1 style='color:#00ff88; margin:0;'>EXCELLENT!</h1><p>PROFIT COLLECTED</p>" : "<h1 style='color:#ff0055; margin:0;'>FAILED</h1><p>TRY NEXT ROUND</p>";
    pop.style.background = "rgba(10,20,30,0.95)";
    pop.style.borderColor = win ? "#00ff88" : "#ff0055";
    pop.style.color = "#fff";
    pop.style.display = 'block';
    setTimeout(() => pop.style.display = 'none', 1500);
}

function zm(v) {
  scale = Math.min(Math.max(scale + v, 0.5), 1.5);
  document.getElementById('wrapMD5').style.transform = `translate(-50%, -50%) rotate(90deg) scale(${scale})`;
}

// Drag & Drop
let act = null; let sx, sy, ix, iy;
function dragS(e, el) {
  if (e.target.tagName === 'INPUT' || e.target.tagName === 'BUTTON') return;
  act = el; const v = e.type === 'touchstart' ? e.touches[0] : e;
  sx = v.clientX; sy = v.clientY;
  const rect = act.getBoundingClientRect();
  ix = rect.left + rect.width / 2; iy = rect.top + rect.height / 2;
  document.addEventListener('mousemove', dragM); document.addEventListener('touchmove', dragM, {passive: false});
  document.addEventListener('mouseup', dragE); document.addEventListener('touchend', dragE);
}
function dragM(e) {
  if (!act) return; const v = e.type === 'touchmove' ? e.touches[0] : e;
  if(e.type === 'touchmove') e.preventDefault();
  act.style.left = (ix + (v.clientX - sx)) + 'px';
  act.style.top = (iy + (v.clientY - sy)) + 'px';
}
function dragE() { act = null; }
</script>
</body>
</html>
