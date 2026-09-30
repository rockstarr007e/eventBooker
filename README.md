# eventBooker
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>EventBus – Smart Event Bus Booking</title>
<style>
:root{--bg:#f4f6fb;--surf:#fff;--ink:#141b2d;--mut:#64708a;--line:#e2e6f0;--pri:#2f5bea;--pri2:#7c3aed;--ok:#15803d;--okbg:#dcfce7;--bad:#dc2626;--bk:#d5d9e3;--nav:#0f1626;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0d1220;--surf:#171f33;--ink:#e8ecf7;--mut:#96a1bd;--line:#28324d;--okbg:#12351f;--ok:#4ade80;--bk:#2a3350;--nav:#080c17}}
:root[data-theme="dark"]{--bg:#0d1220;--surf:#171f33;--ink:#e8ecf7;--mut:#96a1bd;--line:#28324d;--okbg:#12351f;--ok:#4ade80;--bk:#2a3350;--nav:#080c17}
html{scroll-padding-top:env(safe-area-inset-top,0px);scroll-behavior:smooth}
*{box-sizing:border-box;margin:0;padding:0;font-family:"Segoe UI",system-ui,Arial,sans-serif}
body{background:var(--bg);color:var(--ink)}
button{cursor:pointer;font-weight:600;border:0;border-radius:9px;padding:11px 16px;background:var(--pri);color:#fff}
button:hover{filter:brightness(1.1)}button.ghost{background:transparent;color:var(--ink);border:1px solid var(--line)}
button:disabled{opacity:.5;cursor:not-allowed}
input,select{padding:12px;border:1px solid var(--line);border-radius:9px;background:var(--surf);color:var(--ink);width:100%}
:focus-visible{outline:2px solid var(--pri);outline-offset:2px}
nav{background:var(--nav);color:#fff;padding:14px 5%;display:flex;justify-content:space-between;align-items:center;position:sticky;top:env(safe-area-inset-top,0px);z-index:20;gap:10px;flex-wrap:wrap}
.logo{font-size:23px;font-weight:800;color:#7ea2ff}.nr{display:flex;gap:8px;align-items:center}
nav button{background:#ffffff18;padding:8px 13px}
.hero{padding:50px 5% 60px;text-align:center;color:#fff;background:linear-gradient(135deg,#2f5bea,#7c3aed)}
.hero h1{font-size:clamp(28px,5vw,44px);margin-bottom:10px}.hero p{margin-bottom:24px;opacity:.92}
.search{background:var(--surf);padding:14px;border-radius:14px;max-width:960px;margin:auto;display:grid;grid-template-columns:1fr 1.3fr 1fr auto;gap:10px}
.steps{display:flex;justify-content:center;gap:8px;padding:18px 5%;flex-wrap:wrap}
.step{padding:7px 14px;border-radius:99px;background:var(--surf);border:1px solid var(--line);color:var(--mut);font-size:14px;font-weight:600}
.step.on{background:var(--pri);color:#fff;border-color:var(--pri)}.step.done{color:var(--ok);border-color:var(--ok)}
main{padding:10px 5% 40px;max-width:1200px;margin:auto}
h2.t{font-size:26px;margin:34px 0 16px}
.chips{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:16px;align-items:center}
.chip{padding:8px 15px;border-radius:99px;background:var(--surf);border:1px solid var(--line);color:var(--ink);font-weight:600;font-size:14px}
.chip.on{background:var(--ink);color:var(--bg)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:20px}
.card,.bus,.panel{background:var(--surf);border-radius:14px;border:1px solid var(--line)}
.card{overflow:hidden;transition:transform .15s,box-shadow .15s}.card:hover{transform:translateY(-3px);box-shadow:0 10px 26px #0002}
.card.sel{outline:3px solid var(--pri)}
.pic{height:120px;display:flex;align-items:center;justify-content:center;color:#fff;font-size:44px}
.cb{padding:16px}.cb p{color:var(--mut);margin:5px 0;font-size:14px}.price{color:var(--pri)!important;font-weight:700;font-size:17px!important}
.cb .row{display:flex;justify-content:space-between;align-items:center;margin-top:10px}
.bus{padding:18px;margin:12px 0;display:grid;grid-template-columns:2fr 1.2fr 1fr auto;gap:14px;align-items:center;transition:border-color .15s}
.bus.sel{border-color:var(--pri);box-shadow:0 0 0 2px var(--pri)}
.bus small{color:var(--mut);display:block;margin-top:4px}.tag{display:inline-block;font-size:12px;padding:2px 8px;border-radius:99px;background:var(--line);margin:4px 4px 0 0}
.low{color:var(--bad);font-weight:700}.okc{color:var(--ok);font-weight:700}
.book{display:grid;grid-template-columns:1.1fr 1fr;gap:20px}.panel{padding:22px}
.legend{display:flex;gap:14px;font-size:13px;color:var(--mut);margin:8px 0 16px;flex-wrap:wrap}
.legend i{display:inline-block;width:14px;height:14px;border-radius:4px;margin-right:5px;vertical-align:-2px}
.deck{display:inline-block;padding:16px;border:2px solid var(--line);border-radius:22px 22px 12px 12px;background:var(--bg)}
.wheel{text-align:right;font-size:22px;margin:0 6px 10px}
.srow{display:flex;gap:8px;margin-bottom:8px}.aisle{width:26px}
.seat{width:44px;height:44px;border-radius:8px;background:var(--okbg);color:var(--ok);display:flex;align-items:center;justify-content:center;font-weight:700;font-size:13px;cursor:pointer;border:2px solid transparent;transition:transform .1s}
.seat:hover{transform:scale(1.08)}.seat.sel{background:var(--pri);color:#fff}.seat.bk{background:var(--bk);color:var(--mut);cursor:not-allowed}.seat.bk:hover{transform:none}
.seat.win{border-color:#f59e0b55}
.sum p{display:flex;justify-content:space-between;padding:7px 0;border-bottom:1px dashed var(--line);font-size:15px}
.sum p.tot{font-size:20px;font-weight:800;border:0}.sum label{display:flex;gap:8px;align-items:center;margin:10px 0;font-size:14px}.sum label input{width:auto}
.pax{display:grid;gap:8px;margin:10px 0}.pax div{display:flex;gap:8px;align-items:center}.pax b{min-width:46px;font-size:13px;color:var(--mut)}
.promo{display:flex;gap:8px;margin:10px 0}.timer{font-weight:700;color:var(--bad);font-size:14px;min-height:20px;margin-bottom:8px}
.empty{padding:34px;text-align:center;color:var(--mut);border:2px dashed var(--line);border-radius:14px}
.ov{position:fixed;inset:0;background:#0009;display:none;align-items:center;justify-content:center;z-index:50;padding:16px}
.ov.on{display:flex}.modal{background:var(--surf);border-radius:18px;max-width:440px;width:100%;max-height:88vh;overflow:auto;padding:24px}
.tk{border:2px dashed var(--line);border-radius:14px;padding:16px;margin:12px 0;text-align:center}.tk svg{width:130px;height:130px;margin:8px auto;display:block}
.trip{border:1px solid var(--line);border-radius:12px;padding:12px;margin:10px 0;font-size:14px}.trip button{padding:6px 10px;margin-top:8px;font-size:13px}
#toast{position:fixed;left:50%;transform:translateX(-50%);bottom:calc(20px + env(safe-area-inset-bottom,0px));background:var(--ink);color:var(--bg);padding:12px 20px;border-radius:99px;z-index:60;opacity:0;pointer-events:none;transition:opacity .25s;max-width:90%;text-align:center}
#toast.on{opacity:1}
footer{padding:26px;text-align:center;background:var(--nav);color:#fff;font-size:14px}
@media(max-width:860px){.search,.book{grid-template-columns:1fr}.bus{grid-template-columns:1fr}.bus button{width:100%}}
@media(prefers-reduced-motion:reduce){*{transition:none!important;scroll-behavior:auto!important}}
</style>
</head>
<body>
<nav><div class="logo">🚌 EventBus</div><div class="nr"><button onclick="openTrips()">My Trips <span id="tc"></span></button><button id="th" aria-label="Toggle theme">🌓</button></div></nav>

<section class="hero">
<h1>Book your ride to the event</h1>
<p>Pick an event, compare buses, choose seats and pay — all in one flow.</p>
<div class="search">
<input id="from" list="cities" placeholder="From city" value="Delhi" aria-label="From city">
<input id="q" placeholder="Search event or city" aria-label="Search event">
<input id="date" type="date" aria-label="Earliest date">
<button onclick="renderEvents();go('events')">Search</button>
</div>
<datalist id="cities"><option>Delhi<option>Chandigarh<option>Jaipur<option>Mumbai<option>Pune<option>Bengaluru<option>Hyderabad<option>Kolkata<option>Ludhiana</datalist>
</section>

<div class="steps" id="steps"></div>

<main>
<section id="events"><h2 class="t" style="margin-top:8px">🔥 Events</h2>
<div class="chips" id="cats"></div><div class="grid" id="evGrid"></div></section>

<section id="buses"><h2 class="t">🚌 Buses <span id="busFor" style="font-size:16px;color:var(--mut);font-weight:400"></span></h2>
<div class="chips" id="busTools"></div><div id="busList"></div></section>

<section id="booking"><h2 class="t">💺 Seats &amp; checkout</h2><div id="bookBox"></div></section>
</main>
<footer>© 2026 EventBus • Smart Event Transportation Platform</footer>

<div class="ov" id="ov"><div class="modal" id="modal"></div></div>
<div id="toast" role="status"></div>

<script>
const EVENTS=[
{id:1,n:"Sunburn Music Festival",c:"Music",city:"Delhi",d:"2026-10-15",p:1499,e:"🎵",g:"#f97316,#ec4899"},
{id:2,n:"India vs Australia",c:"Sports",city:"Mumbai",d:"2026-10-22",p:999,e:"🏏",g:"#16a34a,#0ea5e9"},
{id:3,n:"India Tech Expo",c:"Tech",city:"Bengaluru",d:"2026-11-05",p:799,e:"💻",g:"#2563eb,#7c3aed"},
{id:4,n:"Pushkar Camel Fair",c:"Culture",city:"Jaipur",d:"2026-11-12",p:499,e:"🐪",g:"#d97706,#dc2626"},
{id:5,n:"Comedy Night Live",c:"Comedy",city:"Pune",d:"2026-10-30",p:699,e:"🎤",g:"#8b5cf6,#ec4899"},
{id:6,n:"Kolkata Food Carnival",c:"Culture",city:"Kolkata",d:"2026-11-20",p:399,e:"🍛",g:"#eab308,#ef4444"}];
const OPS=[["EventBus Premium","AC Sleeper",599,"07:30",4.6,["WiFi","Charging","Blanket"]],["Smart Travels","AC Seater",499,"09:00",4.2,["Charging","Water"]],["Event Express","Volvo Multi-Axle",699,"10:30",4.5,["WiFi","Charging","Snacks"]],["Night Rider","Non-AC Seater",349,"13:15",3.9,["Water"]]];
const S={ev:null,bus:null,seats:[],promo:null,pass:true,cat:"All",type:"All",sort:"time",hold:null,tick:null};
const $=s=>document.querySelector(s),money=n=>"₹"+Math.round(n).toLocaleString("en-IN");
const esc=s=>String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));
const hash=s=>{let x=7;for(const c of s)x=(x*31+c.charCodeAt(0))>>>0;return x};
const rng=a=>()=>{a=(a+0x6D2B79F5)>>>0;let t=a;t=Math.imul(t^t>>>15,t|1);t^=t+Math.imul(t^t>>>7,t|61);return((t^t>>>14)>>>0)/4294967296};
const store={get(k,d){try{return JSON.parse(localStorage.getItem(k))??d}catch(e){return d}},set(k,v){try{localStorage.setItem(k,JSON.stringify(v))}catch(e){}}};
const go=id=>document.getElementById(id).scrollIntoView({behavior:"smooth"});
let tt;function toast(m){const t=$("#toast");t.textContent=m;t.classList.add("on");clearTimeout(tt);tt=setTimeout(()=>t.classList.remove("on"),2600)}

/* theme */
$("#th").onclick=()=>{const r=document.documentElement,dark=r.dataset.theme?r.dataset.theme==="dark":matchMedia("(prefers-color-scheme:dark)").matches;r.dataset.theme=dark?"light":"dark";store.set("eb-theme",r.dataset.theme)};
{const t=store.get("eb-theme",null);if(t)document.documentElement.dataset.theme=t}

/* steps */
function renderSteps(){const n=["Event","Bus","Seats","Confirm"],cur=!S.ev?0:!S.bus?1:!S.seats.length?2:3;
$("#steps").innerHTML=n.map((x,i)=>`<span class="step ${i<cur?"done":i===cur?"on":""}">${i<cur?"✓":i+1}. ${x}</span>`).join("")}

/* events */
function renderEvents(){
const cats=["All",...new Set(EVENTS.map(e=>e.c))];
$("#cats").innerHTML=cats.map(c=>`<button class="chip ${S.cat===c?"on":""}" data-c="${c}">${c}</button>`).join("");
$("#cats").querySelectorAll("button").forEach(b=>b.onclick=()=>{S.cat=b.dataset.c;renderEvents()});
const q=$("#q").value.trim().toLowerCase(),d=$("#date").value;
const list=EVENTS.filter(e=>(S.cat==="All"||e.c===S.cat)&&(!q||(e.n+e.city).toLowerCase().includes(q))&&(!d||e.d>=d));
$("#evGrid").innerHTML=list.length?list.map(e=>`<div class="card ${S.ev&&S.ev.id===e.id?"sel":""}"><div class="pic" style="background:linear-gradient(135deg,${e.g})">${e.e}</div><div class="cb"><h3>${e.n}</h3><p>📍 ${e.city} • 📅 ${new Date(e.d).toLocaleDateString("en-IN",{day:"numeric",month:"short",year:"numeric"})}</p><div class="row"><span class="price">Pass ${money(e.p)}</span><button data-id="${e.id}">${S.ev&&S.ev.id===e.id?"Selected ✓":"View buses"}</button></div></div></div>`).join(""):`<div class="empty" style="grid-column:1/-1">No events match. <button class="ghost" onclick="$('#q').value='';$('#date').value='';S.cat='All';renderEvents()">Clear filters</button></div>`;
$("#evGrid").querySelectorAll("button[data-id]").forEach(b=>b.onclick=()=>pickEvent(+b.dataset.id));
renderSteps()}
function pickEvent(id){S.ev=EVENTS.find(e=>e.id===id);S.bus=null;clearSeats();renderEvents();renderBuses();renderBook();toast("Showing buses to "+S.ev.n);go("buses")}

/* buses */
function genBuses(){const from=$("#from").value.trim()||"Delhi",m=.6+(hash(from+S.ev.city)%90)/100;
return OPS.map((o,i)=>{const h=hash(o[0]+S.ev.id+from),dur=3+h%6,[hh,mm]=o[3].split(":").map(Number),arr=((hh+dur)%24)*60+mm;
return{id:o[0]+"|"+S.ev.id+"|"+from,name:o[0],type:o[1],base:Math.round(o[2]*m/10)*10,dep:o[3],arr:String(Math.floor(arr/60)).padStart(2,"0")+":"+String(arr%60).padStart(2,"0"),dur,rate:o[4],am:o[5],from}})}
function bookedSet(b){const r=rng(hash(b.id)),s=new Set(),n=6+Math.floor(r()*18);while(s.size<n)s.add(1+Math.floor(r()*40));
store.get("eb-trips",[]).filter(t=>t.busId===b.id&&t.status==="Confirmed").forEach(t=>t.seats.forEach(x=>s.add(x)));return s}
function surge(b){return bookedSet(b).size/40>.5?.12:0}
function seatPrice(b){return Math.round(b.base*(1+surge(b)))}
function renderBuses(){
if(!S.ev){$("#busFor").textContent="";$("#busTools").innerHTML="";$("#busList").innerHTML='<div class="empty">Choose an event above to see buses.</div>';return}
$("#busFor").textContent="to "+S.ev.n+", "+S.ev.city;
const types=["All","AC","Non-AC"];
$("#busTools").innerHTML=types.map(t=>`<button class="chip ${S.type===t?"on":""}" data-t="${t}">${t}</button>`).join("")+`<select id="sort" style="width:auto;margin-left:auto"><option value="time">Earliest</option><option value="price">Cheapest</option><option value="rate">Top rated</option><option value="seats">Most seats</option></select>`;
$("#sort").value=S.sort;$("#sort").onchange=e=>{S.sort=e.target.value;renderBuses()};
$("#busTools").querySelectorAll("button").forEach(b=>b.onclick=()=>{S.type=b.dataset.t;renderBuses()});
let l=genBuses().map(b=>({...b,left:40-bookedSet(b).size,price:seatPrice(b)}));
if(S.type==="AC")l=l.filter(b=>!b.type.startsWith("Non"));if(S.type==="Non-AC")l=l.filter(b=>b.type.startsWith("Non"));
const k={time:(a,b)=>a.dep.localeCompare(b.dep),price:(a,b)=>a.price-b.price,rate:(a,b)=>b.rate-a.rate,seats:(a,b)=>b.left-a.left}[S.sort];l.sort(k);
$("#busList").innerHTML=l.length?l.map(b=>`<div class="bus ${S.bus&&S.bus.id===b.id?"sel":""}"><div><h3>${b.name}</h3><small>${b.type} • ⭐ ${b.rate}</small>${b.am.map(a=>`<span class="tag">${a}</span>`).join("")}</div><div><b>${b.dep} → ${b.arr}</b><small>${b.dur}h journey from ${esc(b.from)}</small></div><div><b style="font-size:19px">${money(b.price)}</b>${b.price>b.base?'<small>🔥 Demand pricing</small>':""}<small class="${b.left<=10?"low":"okc"}">${b.left<=10?"Only "+b.left+" left":b.left+" seats"}</small></div><button data-id="${b.id}">${S.bus&&S.bus.id===b.id?"Selected ✓":"Select"}</button></div>`).join(""):'<div class="empty">No buses match this filter.</div>';
$("#busList").querySelectorAll("button").forEach(x=>x.onclick=()=>{const b=l.find(y=>y.id===x.dataset.id);if(S.bus&&S.bus.id!==b.id)clearSeats();S.bus=b;renderBuses();renderBook();go("booking")})}

/* seats & checkout */
function clearSeats(){S.seats=[];S.promo=null;clearInterval(S.tick);S.hold=null}
function startHold(){if(S.hold)return;S.hold=Date.now()+300000;S.tick=setInterval(()=>{const r=S.hold-Date.now();if(r<=0){clearSeats();renderBook();toast("Seat hold expired. Please pick again.");return}const t=$("#timer");if(t)t.textContent="⏱ Seats held for "+Math.floor(r/60000)+":"+String(Math.floor(r/1000)%60).padStart(2,"0")},1000)}
function pricing(){const sp=S.bus.price,sub=S.seats.length*sp,pass=S.pass?S.seats.length*S.ev.p:0;let disc=0;
if(S.promo==="EVENT10")disc=(sub+pass)*.1;if(S.promo==="FIRST20")disc=Math.min(300,(sub+pass)*.2);return{sub,pass,disc,tot:sub+pass-disc}}
function renderBook(){renderSteps();const box=$("#bookBox");
if(!S.bus){box.innerHTML='<div class="empty">Select an event and a bus to open the seat map.</div>';return}
const bk=bookedSet(S.bus),rows=[];
for(let r=0;r<10;r++){const seat=c=>{const n=r*4+c+1,b=bk.has(n),s=S.seats.includes(n);return`<div class="seat ${b?"bk":s?"sel":""} ${c%3===0?"win":""}" data-n="${n}" role="button" aria-label="Seat ${n}${b?" booked":""}">${n}</div>`};rows.push(`<div class="srow">${seat(0)}${seat(1)}<div class="aisle"></div>${seat(2)}${seat(3)}</div>`)}
const p=pricing();
box.innerHTML=`<div class="book"><div class="panel"><div class="legend"><span><i style="background:var(--okbg)"></i>Free</span><span><i style="background:var(--pri)"></i>Yours</span><span><i style="background:var(--bk)"></i>Booked</span><span><i style="border:2px solid #f59e0b55"></i>Window</span></div><div class="deck"><div class="wheel">🎛️</div>${rows.join("")}</div><p style="color:var(--mut);font-size:13px;margin-top:10px">Pick up to 6 seats.</p></div>
<div class="panel sum"><div class="timer" id="timer"></div><h3 style="margin-bottom:8px">${S.bus.name}</h3><p><span>Event</span><b>${S.ev.n}</b></p><p><span>Departs</span><b>${S.bus.dep} from ${esc(S.bus.from)}</b></p><p><span>Seats</span><b>${S.seats.length?[...S.seats].sort((a,b)=>a-b).join(", "):"None"}</b></p>
${S.seats.length?`<div class="pax">${[...S.seats].sort((a,b)=>a-b).map(n=>`<div><b>Seat ${n}</b><input class="pn" data-n="${n}" placeholder="Passenger name" maxlength="30"><input class="pa" data-n="${n}" type="number" min="1" max="99" placeholder="Age" style="max-width:80px"></div>`).join("")}</div>
<label><input type="checkbox" id="pass" ${S.pass?"checked":""}> Add event pass (${money(S.ev.p)} each)</label>
<div class="promo"><input id="pc" placeholder="Promo: EVENT10 / FIRST20"><button class="ghost" id="pb">Apply</button></div>
<select id="pay" style="margin-bottom:10px"><option>UPI</option><option>Credit / Debit card</option><option>Net banking</option><option>Wallet</option></select>
<p><span>Bus fare</span><span>${money(p.sub)}</span></p>${S.pass?`<p><span>Event pass</span><span>${money(p.pass)}</span></p>`:""}${p.disc?`<p><span>Discount (${S.promo})</span><span style="color:var(--ok)">−${money(p.disc)}</span></p>`:""}<p class="tot"><span>Total</span><span>${money(p.tot)}</span></p><button style="width:100%" id="cf">Pay ${money(p.tot)} &amp; confirm</button>`:'<p style="color:var(--mut);margin-top:12px;border:0">Tap a seat to start.</p>'}</div></div>`;
box.querySelectorAll(".seat:not(.bk)").forEach(s=>s.onclick=()=>toggleSeat(+s.dataset.n));
if(S.seats.length){$("#pass").onchange=e=>{S.pass=e.target.checked;keepInputs()};$("#pb").onclick=applyPromo;$("#cf").onclick=confirmBooking}}
function keepInputs(){const v=[...document.querySelectorAll(".pn,.pa")].map(i=>[i.className+i.dataset.n,i.value]),pc=$("#pc")?.value,py=$("#pay")?.value;renderBook();v.forEach(([k,x])=>{const i=document.querySelector("."+k.slice(0,2)+`[data-n="${k.slice(2)}"]`);if(i)i.value=x});if($("#pay"))$("#pay").value=py}
function toggleSeat(n){const i=S.seats.indexOf(n);if(i>-1)S.seats.splice(i,1);else{if(S.seats.length>=6)return toast("Max 6 seats per booking");S.seats.push(n);startHold()}
if(!S.seats.length){clearInterval(S.tick);S.hold=null}keepInputs()}
function applyPromo(){const c=$("#pc").value.trim().toUpperCase();if(!["EVENT10","FIRST20"].includes(c))return toast("Invalid promo code");S.promo=c;toast("Promo "+c+" applied");keepInputs()}
function confirmBooking(){const pax=[];for(const n of S.seats){const nm=document.querySelector(`.pn[data-n="${n}"]`).value.trim(),ag=+document.querySelector(`.pa[data-n="${n}"]`).value;if(!nm||!ag||ag<1)return toast("Enter name and age for seat "+n);pax.push({seat:n,name:nm,age:ag})}
const p=pricing(),t={id:"EB"+Date.now().toString(36).toUpperCase().slice(-6),busId:S.bus.id,bus:S.bus.name,type:S.bus.type,ev:S.ev.n,city:S.ev.city,date:S.ev.d,from:S.bus.from,dep:S.bus.dep,arr:S.bus.arr,seats:[...S.seats].sort((a,b)=>a-b),pax,total:Math.round(p.tot),pay:$("#pay").value,status:"Confirmed"};
const all=store.get("eb-trips",[]);all.unshift(t);store.set("eb-trips",all);clearSeats();S.bus=null;
renderBuses();renderBook();renderEvents();countTrips();showTicket(t)}

/* ticket + trips */
function qr(id){const r=rng(hash(id)),n=13;let s=`<svg viewBox="0 0 ${n} ${n}" shape-rendering="crispEdges"><rect width="${n}" height="${n}" fill="#fff"/>`;
const fin=(x,y)=>`<rect x="${x}" y="${y}" width="4" height="4" fill="#000"/><rect x="${x+1}" y="${y+1}" width="2" height="2" fill="#fff"/><rect x="${x+1.5}" y="${y+1.5}" width="1" height="1" fill="#000"/>`;
for(let y=0;y<n;y++)for(let x=0;x<n;x++){const corner=(x<5&&y<5)||(x>7&&y<5)||(x<5&&y>7);if(!corner&&r()>.5)s+=`<rect x="${x}" y="${y}" width="1" height="1" fill="#000"/>`}
return s+fin(0,0)+fin(9,0)+fin(0,9)+"</svg>"}
function ticketHTML(t){return`<div class="tk"><h3>${esc(t.ev)}</h3><p style="color:var(--mut);font-size:14px">${esc(t.city)} • ${new Date(t.date).toLocaleDateString("en-IN",{day:"numeric",month:"short",year:"numeric"})}</p>${qr(t.id)}<b>${t.id}</b><p style="font-size:14px;margin-top:8px">${esc(t.bus)} (${esc(t.type)})<br>${esc(t.from)} • ${t.dep} → ${t.arr}</p><p style="font-size:14px;margin-top:8px">${t.pax.map(p=>`Seat ${p.seat}: ${esc(p.name)} (${p.age})`).join("<br>")}</p><p style="margin-top:8px"><b>${money(t.total)}</b> via ${esc(t.pay)}</p></div>`}
function showTicket(t){$("#modal").innerHTML=`<h2>🎉 Booking confirmed</h2>${ticketHTML(t)}<button style="width:100%" onclick="closeM()">Done</button>`;$("#ov").classList.add("on")}
function openTrips(){const a=store.get("eb-trips",[]);$("#modal").innerHTML=`<h2>My trips</h2>${a.length?a.map(t=>`<div class="trip"><b>${esc(t.ev)}</b> — ${t.id}<br>${esc(t.bus)} • Seats ${t.seats.join(", ")} • ${money(t.total)}<br><span class="${t.status==="Confirmed"?"okc":"low"}">${t.status}</span><br><button class="ghost" onclick="viewTrip('${t.id}')">View ticket</button> ${t.status==="Confirmed"?`<button class="ghost" onclick="cancelTrip('${t.id}')">Cancel</button>`:""}</div>`).join(""):'<div class="empty" style="margin:14px 0">No bookings yet. Your tickets will show up here.</div>'}<button style="width:100%" onclick="closeM()">Close</button>`;$("#ov").classList.add("on")}
function viewTrip(id){const t=store.get("eb-trips",[]).find(x=>x.id===id);$("#modal").innerHTML=`<h2>Ticket</h2>${ticketHTML(t)}<button class="ghost" style="width:100%" onclick="openTrips()">Back</button>`}
function cancelTrip(id){const a=store.get("eb-trips",[]),t=a.find(x=>x.id===id);t.status="Cancelled";store.set("eb-trips",a);toast("Cancelled. Refund of "+money(t.total*.9)+" (10% fee) in 3–5 days");openTrips();countTrips();renderBuses();renderBook()}
function closeM(){$("#ov").classList.remove("on")}
$("#ov").onclick=e=>{if(e.target.id==="ov")closeM()};document.addEventListener("keydown",e=>{if(e.key==="Escape")closeM()});
function countTrips(){const n=store.get("eb-trips",[]).filter(t=>t.status==="Confirmed").length;$("#tc").textContent=n?"("+n+")":""}
["q","date","from"].forEach(i=>$("#"+i).addEventListener("input",()=>{renderEvents();if(i==="from"&&S.ev){clearSeats();S.bus=null;renderBuses();renderBook()}}));
renderEvents();renderBuses();renderBook();countTrips();
</script>
</body>
</html>
