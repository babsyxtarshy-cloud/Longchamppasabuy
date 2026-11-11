<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Cakelin's Longchamp Pasabuy</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800&family=Quicksand:wght@500;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#fbeee9;--card:#fffaf7;--ink:#5a3e36;--soft:#8a6d63;--pink:#f3b8c6;--pink2:#e98fa6;--line:#f0d9d3;--ok:#7fb89a}
@media (prefers-color-scheme: dark){:root{--bg:#2b201d;--card:#372a26;--ink:#f6e6df;--soft:#c9ada3;--pink:#a85d74;--pink2:#d98aa1;--line:#4b3a35;--ok:#6fae8f}}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font-family:'Nunito',system-ui,sans-serif;font-size:17px;line-height:1.5}
.wrap{max-width:520px;margin:0 auto;padding:28px 16px 48px}
h1{font-family:'Quicksand',sans-serif;font-size:26px;margin:0 0 4px;text-align:center}
.sub{text-align:center;color:var(--soft);margin:0 0 22px}
.card{background:var(--card);border:1px solid var(--line);border-radius:22px;padding:20px;margin-bottom:16px;box-shadow:0 4px 14px rgba(150,90,80,.07)}
.card h2{font-family:'Quicksand',sans-serif;font-size:19px;margin:0 0 12px}
input{width:100%;padding:14px 16px;border-radius:16px;border:2px solid var(--line);background:var(--bg);color:var(--ink);font:inherit;text-align:center;letter-spacing:2px;text-transform:uppercase}
input:focus{outline:none;border-color:var(--pink2)}
button,.btn{display:inline-block;width:100%;margin-top:12px;padding:14px;border:0;border-radius:999px;background:var(--pink2);color:#fff;font:700 17px 'Nunito',sans-serif;text-align:center;text-decoration:none;cursor:pointer}
.err{color:#c0506a;text-align:center;margin:10px 0 0;min-height:1.2em}
.row{display:flex;justify-content:space-between;gap:12px;padding:8px 0;border-bottom:1px dashed var(--line)}
.row:last-child{border-bottom:0}
.row span:first-child{color:var(--soft)}
.row span:last-child{font-weight:700;text-align:right}
.total{font-size:19px}
.pill{display:inline-block;background:var(--pink);color:var(--ink);border-radius:999px;padding:4px 14px;font-weight:800;font-size:15px}
.steps{list-style:none;margin:0;padding:0}
.steps li{display:flex;align-items:center;gap:12px;padding:7px 0;color:var(--soft)}
.dot{width:22px;height:22px;border-radius:50%;border:2px solid var(--line);flex:none}
.steps li.done{color:var(--ink)}
.steps li.done .dot{background:var(--ok);border-color:var(--ok)}
.steps li.now{color:var(--ink);font-weight:800}
.steps li.now .dot{background:var(--pink2);border-color:var(--pink2)}
.note{background:var(--pink);border-radius:16px;padding:12px 14px;margin-top:10px;font-size:15px}
.small{font-size:14px;color:var(--soft);margin:6px 0 0}
.hide{display:none}
.link{background:none;color:var(--soft);text-decoration:underline;font-size:15px;font-weight:600;width:auto;margin:6px auto 0;display:block;padding:6px}
</style>
</head>
<body>
<div class="wrap">
  <p class="sub" id="eyebrow">Track your order</p>
  <h1>Cakelin's Longchamp Pasabuy</h1>

  <div id="login" class="card" style="margin-top:22px">
    <h2>Enter your access code</h2>
    <input id="code" placeholder="XXXXX-XXXXX" autocomplete="off" autocapitalize="characters">
    <button id="go">View my order</button>
    <p class="err" id="err"></p>
  </div>

  <div id="view" class="hide"></div>
</div>

<script>
var MSG_LINK = "https://www.facebook.com/share/1CCPXzw6fN/?mibextid=wwXIfr";
var BLOBS = [{"s":"Z0uzwjriPpnCjvbUk/J46w==","i":"TrXEqMAZyNviNQg/","d":"vteDop41LltjSmTQ/SEAY8uYkIEwC5H0usbCuGTN7qm7QAszdvou/ds2s3jbajk521X/4UR1lU9xKFHaIIJcKU7XTAU8ltYBWPjzO+TtgQe6pKaf3vYEnoFJguVRv9gqQmKDgkXgoo6Zs7xNu6K9I36AO5I2+73QebRkPfaD21YAf7d+BbQ3W7jxb47YpKvak0Z1+vaEy1cLcFI0lUgXXz3nU6uO4pxGqSE+QXmV2Er0XgsmbtM7FXDHTJi/XLzdObo6fmquGcTvgF3YeKMB6GAx7IdxmyLjb7PGF1BkRxim8BLTJby1IqFKqvDO18+t5wJk2Nw74ysPWdlGsf/FdY+Os2dJvSSAaQrXD1WifJiZQ1ZV9zEmbsi1TO0S0EnZnUfPdguwyLjFimnRBM+8X8P3lj2rWdjeGZhoIkEjoo4ybyIrXdEpRSn+mOVMcklF2//RjbMPcRr3avXQ/63u67ExV4k4lkTwQDdtjVg9Pv9pEoNmfpGYAgoH9Wwbsr/KuNJKk8+TXTmg/1yVZ8C0ZOPpB+UsH0YRHK9Qn/ECzPMnmDxv3W1kIUOo"}];
var STAGES = ["Reserved","Down payment received","Bought abroad","Packed for travel","Arrived in the Philippines","Ready for pickup or delivery","Completed"];

function b64(s){var b=atob(s),a=new Uint8Array(b.length);for(var i=0;i<b.length;i++)a[i]=b.charCodeAt(i);return a}
function esc(s){var d=document.createElement('div');d.textContent=s==null?'':String(s);return d.innerHTML}
function peso(n){return '\u20B1'+Number(n).toLocaleString('en-PH')}

async function tryDecrypt(code, blob){
  try{
    var enc=new TextEncoder();
    var km=await crypto.subtle.importKey('raw',enc.encode(code),'PBKDF2',false,['deriveKey']);
    var key=await crypto.subtle.deriveKey({name:'PBKDF2',salt:b64(blob.s),iterations:100000,hash:'SHA-256'},km,{name:'AES-GCM',length:256},false,['decrypt']);
    var pt=await crypto.subtle.decrypt({name:'AES-GCM',iv:b64(blob.i)},key,b64(blob.d));
    return JSON.parse(new TextDecoder().decode(pt));
  }catch(e){return null}
}

function render(b){
  var total=b.items.reduce(function(a,i){return a+i.price},0)+(b.fee||0);
  var paid=(b.paid||0);
  var h='';
  h+='<div class="card"><h2>Hi, '+esc(b.name)+'!</h2><div class="row"><span>Status</span><span class="pill">'+esc(STAGES[b.stage])+'</span></div></div>';
  b.items.forEach(function(i,n){
    h+='<div class="card"><h2>Item '+(n+1)+': '+esc(i.title)+'</h2>';
    h+='<div class="row"><span>Size</span><span>'+esc(i.size)+'</span></div>';
    h+='<div class="row"><span>Stamping</span><span>'+esc(i.stamp)+'</span></div>';
    h+='<div class="row"><span>Where I\u2019ll buy</span><span>'+esc(i.where)+'</span></div>';
    h+='<div style="margin-top:10px;font-weight:800">Color choices</div>';
    i.colors.forEach(function(c,k){var o=['1st','2nd','3rd'][k]||(k+1)+'th';h+='<div class="row"><span>'+o+' choice</span><span>'+esc(c)+'</span></div>'});
    if(i.note)h+='<div class="note">Note from buyer: '+esc(i.note)+'</div>';
    h+='</div>';
  });
  h+='<div class="card"><h2>Payment plan</h2>';
  b.items.forEach(function(i){h+='<div class="row"><span>'+esc(i.title)+'</span><span>'+peso(i.price)+'</span></div>'});
  if(b.fee)h+='<div class="row"><span>Stamping handling fee</span><span>'+peso(b.fee)+'</span></div>';
  h+='<div class="row total"><span>Total</span><span>'+peso(total)+'</span></div>';
  h+='<div class="row"><span>Payment due</span><span>'+esc(b.due)+'</span></div>';
  h+='<div class="row"><span>60% payment</span><span>'+peso(Math.round(total*0.6))+'</span></div>';
  h+='<div class="row"><span>40% on arrival (due '+esc(b.due2)+')</span><span>'+peso(total-Math.round(total*0.6))+'</span></div>';
  h+='<div class="row"><span>Paid so far</span><span>'+peso(paid)+'</span></div>';
  h+='<div class="row"><span>Perks</span><span>'+esc(b.perks)+'</span></div>';
  h+='<div class="row"><span>Shipping</span><span>'+esc(b.shipping)+'</span></div>';
  h+='<p class="small">QR codes will be sent 1 week before the payment due date, or upon request if you\u2019d like to pay early. No cancellation.</p></div>';
  h+='<div class="card"><h2>Progress</h2><ul class="steps">';
  STAGES.forEach(function(s,n){h+='<li class="'+(n<b.stage?'done':n===b.stage?'now':'')+'"><span class="dot"></span>'+esc(s)+'</li>'});
  h+='</ul></div>';
  h+='<a class="btn" href="'+MSG_LINK+'" target="_blank" rel="noopener">Message me</a>';
  h+='<button class="link" id="out">Log out</button>';
  var v=document.getElementById('view');v.innerHTML=h;v.classList.remove('hide');
  document.getElementById('login').classList.add('hide');
  document.getElementById('eyebrow').classList.add('hide');
  document.getElementById('out').onclick=function(){location.reload()};
}

async function go(){
  var code=document.getElementById('code').value.trim().toUpperCase();
  var err=document.getElementById('err');err.textContent='';
  if(!code){err.textContent='Please enter your code.';return}
  for(var i=0;i<BLOBS.length;i++){
    var r=await tryDecrypt(code,BLOBS[i]);
    if(r){render(r);return}
  }
  err.textContent='That code didn\u2019t work. Please check it and try again.';
}
document.getElementById('go').onclick=go;
document.getElementById('code').addEventListener('keydown',function(e){if(e.key==='Enter')go()});
</script>
</body>
</html>
