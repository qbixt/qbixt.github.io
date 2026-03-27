<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BGC App</title>

<style>
body {
  margin:0;
  font-family:Arial;
  background: radial-gradient(circle at top,#0b1220,#020617);
  color:white;
}

.container {
  padding:15px;
  padding-bottom:100px;
}

.card {
  background: rgba(255,255,255,0.05);
  padding:15px;
  border-radius:20px;
  margin-bottom:10px;
}

.tap-btn {
  width:160px;
  height:160px;
  border-radius:50%;
  display:block;
  margin:20px auto;
  box-shadow:0 0 40px #9333ea;
}

.btn {
  width:100%;
  padding:10px;
  margin-top:10px;
  border:none;
  border-radius:10px;
  background:#3b82f6;
  color:white;
}

.nav {
  position:fixed;
  bottom:0;
  width:100%;
  background:#020617;
  display:flex;
  justify-content:space-around;
  padding:10px;
}
</style>
</head>

<body>

<div class="container">

<!-- HOME -->
<div id="home">

<div class="card">
<h2>BGC Dashboard</h2>
<p>Balance: <span id="balance">0</span></p>
<p>Energy: <span id="energy">100</span></p>
<p>Mining: <span id="mining">1</span>/5s</p>
</div>

<img src="https://i.imgur.com/8Km9tLL.png" class="tap-btn" onclick="tap(event)">

</div>

<!-- BOOST -->
<div id="boost" class="hidden">
<div class="card">
<h3>Boost 🚀</h3>

<button class="btn" onclick="upgradeTap()">Upgrade Tap (100)</button>
<button class="btn" onclick="upgradeEnergy()">Upgrade Energy (150)</button>

</div>
</div>

<!-- NFT -->
<div id="nft" class="hidden">
<div class="card">
<h3>NFT 💎</h3>

<button class="btn" onclick="upgradeMiner()">Upgrade Miner</button>

</div>
</div>

<!-- WALLET -->
<div id="wallet" class="hidden">
<div class="card">
<h3>💸 Withdraw</h3>
<p>Coming Soon 🚀</p>
</div>
</div>

</div>

<!-- NAV -->
<div class="nav">
<button onclick="showPage('home')">Home</button>
<button onclick="showPage('boost')">Boost</button>
<button onclick="showPage('nft')">NFT</button>
<button onclick="showPage('wallet')">Wallet</button>
</div>

<script>

let balance = Number(localStorage.getItem("bal")) || 0;
let energy = Number(localStorage.getItem("energy")) || 100;
let maxEnergy = Number(localStorage.getItem("maxEnergy")) || 100;
let miningRate = Number(localStorage.getItem("mining")) || 1;
let boost = Number(localStorage.getItem("boost")) || 1;
let minerLvl = Number(localStorage.getItem("minerLvl")) || 1;

function save(){
localStorage.setItem("bal",balance);
localStorage.setItem("energy",energy);
localStorage.setItem("maxEnergy",maxEnergy);
localStorage.setItem("mining",miningRate);
localStorage.setItem("boost",boost);
localStorage.setItem("minerLvl",minerLvl);
}

function updateUI(){
document.getElementById("balance").innerText = Math.floor(balance);
document.getElementById("energy").innerText = energy;
document.getElementById("mining").innerText = miningRate;
}

function tap(e){
if(energy<=0){alert("Energy habis");return;}

balance += boost;
energy--;

spawnCoin(e.clientX,e.clientY);

save();
updateUI();
}

function spawnCoin(x,y){
let c=document.createElement("div");
c.innerText="💰";
c.style.position="fixed";
c.style.left=x+"px";
c.style.top=y+"px";
c.style.animation="fly 1s forwards";
document.body.appendChild(c);
setTimeout(()=>c.remove(),1000);
}

let style=document.createElement("style");
style.innerHTML=`
@keyframes fly{
0%{opacity:1;transform:translateY(0);}
100%{opacity:0;transform:translateY(-50px);}
}`;
document.head.appendChild(style);

function upgradeTap(){
if(balance>=100){
balance-=100;
boost++;
save();updateUI();
}
}

function upgradeEnergy(){
if(balance>=150){
balance-=150;
maxEnergy+=20;
save();updateUI();
}
}

function upgradeMiner(){
let cost=minerLvl*100;
if(balance>=cost){
balance-=cost;
minerLvl++;
miningRate++;
save();updateUI();
}
}

setInterval(()=>{
if(energy<maxEnergy){
energy++;
save();updateUI();
}
},3000);

setInterval(()=>{
balance+=miningRate;
save();updateUI();
},5000);

function showPage(p){
document.querySelectorAll("#home,#boost,#nft,#wallet").forEach(el=>el.classList.add("hidden"));
document.getElementById(p).classList.remove("hidden");
}

updateUI();

</script>

</body>
</html>
