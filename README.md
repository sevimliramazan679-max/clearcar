<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,viewport-fit=cover">
<title>ClearCar Control</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    user-select:none;
    -webkit-user-select:none;
}

body{
    font-family:Arial,Helvetica,sans-serif;
    background:radial-gradient(circle at top,#1d2735,#0d1015 70%);
    color:#fff;
    height:100vh;
    overflow:hidden;
}

/* ÜST BAR */
.topbar{
    height:70px;
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:0 18px;
}

.brand{
    font-size:28px;
    font-weight:700;
    color:#31ffb1;
}

.brand span{
    color:#fff;
    font-weight:300;
}

#status{
    background:#1f2a36;
    padding:10px 16px;
    border-radius:14px;
    font-size:16px;
}

/* ANA ALAN */
.main{
    height:calc(100vh - 70px);
    display:grid;
    grid-template-columns:180px 1fr 360px;
    gap:18px;
    padding:0 18px 18px;
}

/* KART */
.card{
    background:rgba(255,255,255,.05);
    border:1px solid rgba(255,255,255,.06);
    border-radius:24px;
    backdrop-filter:blur(8px);
    box-shadow:0 15px 30px rgba(0,0,0,.25);
}

/* SOL GAZ */
.gasWrap{
    padding:18px;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
}

.title{
    font-size:22px;
    margin-bottom:18px;
}

#gas{
    -webkit-appearance:none;
    appearance:none;
    width:280px;
    height:10px;
    transform:rotate(-90deg);
    border-radius:10px;
    background:#3a4654;
}

#gas::-webkit-slider-thumb{
    -webkit-appearance:none;
    width:34px;
    height:34px;
    border-radius:50%;
    background:#31ffb1;
    box-shadow:0 0 20px rgba(49,255,177,.6);
}

#gasVal{
    margin-top:45px;
    font-size:34px;
    color:#31ffb1;
}

/* ORTA */
.center{
    padding:25px;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:center;
    text-align:center;
}

.robot{
    font-size:90px;
    margin-bottom:20px;
}

.name{
    font-size:40px;
    font-weight:700;
}

.sub{
    color:#9ba8b8;
    margin-top:8px;
    font-size:18px;
}

#send{
    margin-top:30px;
    font-size:28px;
    color:#31ffb1;
}

/* SAĞ DİREKSİYON */
.steerWrap{
    padding:24px;
    display:flex;
    flex-direction:column;
    justify-content:center;
}

#steer{
    width:100%;
    height:10px;
    appearance:none;
    background:#3a4654;
    border-radius:10px;
}

#steer::-webkit-slider-thumb{
    -webkit-appearance:none;
    width:34px;
    height:34px;
    border-radius:50%;
    background:#3bb8ff;
    box-shadow:0 0 20px rgba(59,184,255,.6);
}

#steerVal{
    text-align:center;
    margin-top:20px;
    font-size:34px;
    color:#3bb8ff;
}

/* MOBİL */
@media(max-width:900px){

.main{
    grid-template-columns:1fr;
    grid-template-rows:260px auto 220px;
}

.gasWrap{
    order:2;
}

.center{
    order:1;
}

.steerWrap{
    order:3;
}

#gas{
    transform:rotate(0deg);
    width:100%;
}

#gasVal{
    margin-top:18px;
}

.robot{
    font-size:70px;
}

.name{
    font-size:32px;
}
}
</style>
</head>

<body>

<div class="topbar">
<div class="brand">Clear<span>Car</span></div>
<div id="status">Bağlanıyor...</div>
</div>

<div class="main">

<!-- SOL -->
<div class="card gasWrap">
<div class="title">Hız</div>
<input id="gas" type="range" min="-255" max="255" value="0">
<div id="gasVal">0</div>
</div>

<!-- ORTA -->
<div class="card center">
<div class="robot">🤖</div>
<div class="name">ClearCar</div>
<div class="sub">Akıllı Temizlik Robotu</div>
<div id="send">0 , 0</div>
</div>

<!-- SAĞ -->
<div class="card steerWrap">
<div class="title" style="text-align:center;margin-bottom:25px;">Yön</div>
<input id="steer" type="range" min="-100" max="100" value="0">
<div id="steerVal">0</div>
</div>

</div>

<script>
let ws;

let gas   = document.getElementById("gas");
let steer = document.getElementById("steer");

let gasVal   = document.getElementById("gasVal");
let steerVal = document.getElementById("steerVal");

let status = document.getElementById("status");
let send   = document.getElementById("send");

// BAĞLANTI
function connect(){

    ws = new WebSocket("wss://ws-server-1-bnzs.onrender.com");

    ws.onopen = ()=>{
        status.innerText = "Bağlandı ✅";
    };

    ws.onclose = ()=>{
        status.innerText = "Bağlantı Koptu ❌";
        setTimeout(connect,1500);
    };

    ws.onerror = ()=>{
        status.innerText = "Hata ⚠️";
    };
}

connect();

// GÖNDER
function sendData(){

    if(!ws || ws.readyState !== 1) return;

    let g = parseInt(gas.value);
    let s = parseInt(steer.value);

    gasVal.innerText   = g;
    steerVal.innerText = s;

    ws.send(g + "," + s);

    send.innerText = g + " , " + s;
}

// canlı tut
setInterval(sendData,180);

// event
gas.oninput = sendData;
steer.oninput = sendData;

// bırakınca merkez
function resetGas(){
    gas.value = 0;
    sendData();
}

function resetSteer(){
    steer.value = 0;
    sendData();
}

gas.onmouseup = resetGas;
gas.ontouchend = resetGas;

steer.onmouseup = resetSteer;
steer.ontouchend = resetSteer;

</script>

</body>
</html>
