<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,viewport-fit=cover">
<title>ClearCar Control</title>
<link rel="stylesheet" href="panelcss.css">
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
