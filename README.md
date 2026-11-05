<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8"><title>Strike Arena 4v4</title>
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover"><meta name="apple-mobile-web-app-capable" content="yes"><meta name="mobile-web-app-capable" content="yes">
<style>
:root{--bg:#0b0f17;--p:#1c2433;--t:#e8eefc;--b:#3b9bff;--r:#ff4d5a;--g:#ffc83d}
*{box-sizing:border-box}html,body{margin:0;height:100%;background:#000;overflow:hidden;font-family:system-ui,'Segoe UI',Arial,sans-serif;color:var(--t)}
canvas{display:block}
.ov{position:fixed;inset:0;display:none;align-items:center;justify-content:center;flex-direction:column;background:rgba(5,8,14,.82);z-index:10;text-align:center;padding:20px}
.ov.on{display:flex}
h1{font-size:44px;margin:0 0 6px;letter-spacing:4px}h1 span{color:var(--g)}
.btn{background:var(--g);color:#111;border:0;padding:14px 38px;font-size:18px;font-weight:800;border-radius:8px;cursor:pointer;margin-top:18px}
.btn:hover{filter:brightness(1.1)}
small{opacity:.7;line-height:1.7}
#hud{position:fixed;inset:0;pointer-events:none;display:none}
#score{position:absolute;top:12px;left:50%;transform:translateX(-50%);display:flex;gap:2px;font-weight:800;font-size:26px}
#score div{padding:6px 22px;background:rgba(10,14,22,.75)}
#sb{color:var(--b);border-radius:8px 0 0 8px}#sr{color:var(--r);border-radius:0 8px 8px 0}
#goal{position:absolute;top:58px;left:50%;transform:translateX(-50%);font-size:12px;opacity:.7;letter-spacing:2px}
#cross{position:absolute;left:50%;top:50%;width:22px;height:22px;transform:translate(-50%,-50%)}
#cross:before,#cross:after{content:"";position:absolute;background:#fff;box-shadow:0 0 3px #000}
#cross:before{left:10px;top:0;width:2px;height:22px}#cross:after{top:10px;left:0;height:2px;width:22px}
#cross i{position:absolute;left:8px;top:8px;width:6px;height:6px;background:#000;border-radius:50%}
#hit{position:absolute;left:50%;top:50%;width:34px;height:34px;transform:translate(-50%,-50%) rotate(45deg);border:2px solid #fff;opacity:0}
#flash{position:absolute;inset:0;box-shadow:inset 0 0 120px 30px rgba(255,0,0,.7);opacity:0;transition:opacity .4s}
.box{position:absolute;bottom:18px;background:rgba(10,14,22,.78);padding:10px 16px;border-radius:10px;border-left:4px solid var(--g)}
#hp{left:18px;min-width:200px}#am{right:18px;text-align:right}
.bar{height:8px;background:#333;border-radius:4px;margin-top:6px;overflow:hidden}.bar i{display:block;height:100%;background:#4ade80;width:100%}
.big{font-size:30px;font-weight:800}.mo{color:var(--g);font-weight:700}
#feed{position:absolute;top:12px;right:14px;text-align:right;font-size:14px}
#feed div{background:rgba(10,14,22,.75);padding:4px 10px;margin-bottom:4px;border-radius:5px}
#msg{position:absolute;top:30%;width:100%;text-align:center;font-size:28px;font-weight:800;text-shadow:0 2px 8px #000}
#buy{background:rgba(5,8,14,.9)}
#buy .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px;width:min(780px,95vw);margin-top:14px}
.it{background:var(--p);border:2px solid #2b364c;border-radius:10px;padding:12px;cursor:pointer;text-align:left}
.it:hover{border-color:var(--g)}.it.no{opacity:.4}.it.own{border-color:#4ade80}
.it b{font-size:17px}.it span{display:block;font-size:12px;opacity:.75;margin-top:4px}.it em{color:var(--g);font-style:normal;font-weight:800;display:block;margin-top:6px}
#tc{position:fixed;inset:0;display:none;z-index:5;touch-action:none;user-select:none;-webkit-user-select:none}body.t #tc{display:block}
#lk{position:absolute;right:0;top:0;width:55%;height:100%}#js{position:absolute;left:0;bottom:0;width:45%;height:60%}
#jb{position:absolute;width:110px;height:110px;border-radius:50%;border:2px solid rgba(255,255,255,.4);background:rgba(255,255,255,.1);left:30px;bottom:40px}
#jk{position:absolute;left:35px;top:35px;width:40px;height:40px;border-radius:50%;background:rgba(255,255,255,.55)}
.tb{position:absolute;width:60px;height:60px;border-radius:50%;background:rgba(255,255,255,.18);border:2px solid rgba(255,255,255,.5);display:flex;align-items:center;justify-content:center;font-weight:800;font-size:12px;z-index:6}.tb:active{background:rgba(255,200,61,.55)}
#bf{width:86px;height:86px;right:26px;bottom:34px;font-size:30px}#bj{right:128px;bottom:30px}#br{right:20px;bottom:136px}#bb{right:92px;bottom:104px}#bs{right:20px;bottom:206px}#bz{right:92px;bottom:176px}
body.t #hp{top:50px;bottom:auto;left:8px;min-width:120px;padding:5px 10px}body.t #hp .big{font-size:20px}body.t #am{top:50px;bottom:auto;right:8px;padding:5px 10px}body.t #am small{display:none}body.t #am .big{font-size:22px}
body.t #feed{top:130px;right:8px;font-size:11px}body.t #score{font-size:20px}body.t #goal{display:none}body.t #msg{font-size:20px}
#buy{overflow:auto;justify-content:flex-start;padding-top:10px}body.t .it{padding:8px}body.t h1{font-size:28px}
#tools{position:fixed;top:8px;left:8px;z-index:8;display:none;gap:6px}body.in #tools{display:flex}
.sq{width:38px;height:38px;border-radius:8px;border:2px solid rgba(255,255,255,.5);background:rgba(10,14,22,.65);color:#fff;font-size:18px;cursor:pointer;padding:0}
#set{z-index:20;overflow:auto;justify-content:flex-start;padding-top:12px}.sp{width:min(420px,90vw);margin-top:14px;text-align:left}.sp label{display:flex;justify-content:space-between;font-weight:700}.sp input{width:100%;margin-top:6px;accent-color:#ffc83d}
.g2{background:#2b364c!important;color:#fff!important}.sm{margin-top:12px!important;padding:11px 26px!important;font-size:15px!important}
body:not(.t) .to{display:none}
#ed{position:fixed;top:10px;left:50%;transform:translateX(-50%);z-index:20;display:none;align-items:center;gap:10px;background:rgba(5,8,14,.92);padding:8px 14px;border-radius:10px;font-weight:700;font-size:14px}#ed .btn{margin:0;padding:8px 16px;font-size:14px}
body.ed #ed{display:flex}body.ed .ov{display:none!important}body.ed #tc{z-index:15}body.ed .tb{outline:2px dashed #ffc83d;background:rgba(255,200,61,.25)}
#lobby{background:radial-gradient(ellipse at 50% 30%,#1b2a45 0,#0a0f1a 70%);justify-content:flex-start;align-items:stretch;padding:0;overflow:hidden}
#lobby:before{content:"";position:absolute;inset:0;background:repeating-linear-gradient(115deg,transparent 0 60px,rgba(255,200,61,.05) 60px 120px);pointer-events:none}
.lt{display:flex;align-items:center;gap:12px;padding:10px 16px;background:linear-gradient(90deg,rgba(10,14,22,.9),transparent);z-index:1;text-align:left}
.av{width:46px;height:46px;border-radius:50%;background:#2b364c;border:2px solid var(--g);display:flex;align-items:center;justify-content:center;font-size:24px}
.pi input{background:transparent;border:0;border-bottom:1px solid #556;color:#fff;font-weight:800;font-size:16px;width:130px;outline:0}
.cur{margin-left:auto;margin-right:50px;background:rgba(10,14,22,.7);padding:6px 12px;border-radius:20px;font-weight:700;font-size:13px}
.hero{position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font-size:min(44vh,220px);filter:drop-shadow(0 10px 30px rgba(0,0,0,.6))}
.sq4{position:absolute;left:14px;top:50%;transform:translateY(-50%);display:flex;flex-direction:column;gap:8px;z-index:1}
.sl{width:60px;height:60px;border-radius:10px;background:rgba(28,36,51,.85);border:2px solid #2b364c;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:22px}.sl small{font-size:9px}
.lb{position:absolute;right:20px;bottom:18px;text-align:right;z-index:1}#ml{font-weight:800;letter-spacing:2px;color:var(--g);font-size:13px}
.bt{font-size:24px;padding:16px 56px;margin:6px 0 0;clip-path:polygon(8% 0,100% 0,92% 100%,0 100%);animation:pl 1.6s infinite}@keyframes pl{50%{filter:brightness(1.25)}}
#lset{position:absolute;top:10px;right:12px;z-index:2}
#mp{z-index:15;overflow:auto}.pg{display:none;width:min(560px,94vw)}.cards{display:flex;gap:12px;margin-top:16px}.card{flex:1;background:var(--p);border:2px solid #2b364c;border-radius:12px;padding:16px;cursor:pointer}.card:hover{border-color:var(--g)}.card b{font-size:20px;display:block}.card span{font-size:12px;opacity:.75}
textarea{width:100%;height:64px;background:#0e1420;color:#9fd;border:1px solid #2b364c;border-radius:8px;font-size:10px;margin-top:6px;padding:6px}
.pg p{margin:12px 0 0;font-size:13px;opacity:.85;text-align:left}#hl{text-align:left;font-size:14px;line-height:1.8;margin-top:10px}
</style></head><body>
<div id="hud">
 <div id="score"><div id="sb">0</div><div id="sr">0</div></div><div id="goal">FIRST TO 40 · 1 KILL = 1 POINT</div>
 <div id="cross"><i></i></div><div id="hit"></div><div id="flash"></div><div id="msg"></div><div id="feed"></div>
 <div class="box" id="hp"><div>HEALTH <span class="big" id="hpn">100</span></div><div class="bar"><i id="hpb"></i></div><div style="margin-top:6px">KILLS <b id="k">0</b> · DEATHS <b id="d">0</b></div></div>
 <div class="box" id="am"><div id="wn">PISTOL</div><div class="big" id="an">12 / 60</div><div class="mo" id="mo">$1000</div><small>B buy · R reload · 1-5 swap</small></div>
</div>
<div id="tc"><div id="lk"></div><div id="js"><div id="jb"><div id="jk"></div></div></div><div class="tb" id="bf">🔫</div><div class="tb" id="bj">JUMP</div><div class="tb" id="br">R</div><div class="tb" id="bb">BUY</div><div class="tb" id="bs">SWAP</div><div class="tb" id="bz">ZOOM</div></div>
<div class="ov on" id="lobby">
<div class="lt"><div class="av">🪖</div><div class="pi"><input id="pn" maxlength="12" value="Player"><br><small>LV 12 · Ace</small></div><div class="cur">⭐ 2,450 &nbsp; 💰 $1000</div></div>
<div class="hero">🥷</div>
<div class="sq4"><div class="sl">🪖<small>You</small></div><div class="sl">➕<small>Invite</small></div><div class="sl">➕<small></small></div><div class="sl">➕<small></small></div></div>
<button class="sq" id="lset">⚙</button>
<div class="lb"><div id="ml">TEAM DEATHMATCH · CUSTOM</div><button class="btn bt" id="battle">⚔ BATTLE</button></div></div>
<div class="ov" id="mp">
<div class="pg" id="p_mode"><h1 style="font-size:30px">SELECT MODE</h1><div class="cards"><div class="card" id="m44"><b>4 v 4</b><span>Team Deathmatch vs bots · first to 40</span></div><div class="card" id="mcu"><b>CUSTOM</b><span>Play with friends on the same Wi-Fi</span></div></div><button class="btn sm g2 bk" data-to="x">CANCEL</button></div>
<div class="pg" id="p_menu"><h1 style="font-size:30px">CUSTOM ROOM</h1><div class="cards"><div class="card" id="mc"><b>CREATE ROOM</b><span>Be the host and invite friends</span></div><div class="card" id="mj"><b>JOIN ROOM</b><span>Use a host’s invite code</span></div></div><button class="btn sm g2 bk" data-to="p_mode">BACK</button></div>
<div class="pg" id="p_host"><h1 style="font-size:28px">YOUR ROOM</h1><div id="hl"></div><div id="rcw" style="display:none"><p>Room code — tell your friends:</p><div id="rcn" style="font-size:44px;font-weight:800;letter-spacing:8px;color:#ffc83d">------</div></div><p id="rmsg"></p>
<div id="ocw" style="display:none"><p>1. Send this invite code to your friend:</p><textarea id="oc" readonly></textarea><button class="btn sm g2" onclick="cp('oc')">COPY</button><p>2. Paste their reply code here:</p><textarea id="ac"></textarea><button class="btn sm g2" id="acb">CONNECT</button></div>
<div><button class="btn sm g2" id="addp">+ MANUAL INVITE</button> <button class="btn sm" id="stm">START MATCH</button></div><button class="btn sm g2 bk" data-to="p_menu">BACK</button></div>
<div class="pg" id="p_join"><h1 style="font-size:28px">JOIN ROOM</h1><p>Enter the 6-digit room code:</p><input id="jn" inputmode="numeric" maxlength="6" placeholder="000000" style="font-size:30px;letter-spacing:8px;width:100%;text-align:center;background:#0e1420;color:#fff;border:2px solid #2b364c;border-radius:10px;padding:8px"><button class="btn sm" id="jjoin">JOIN</button><p>Or use a manual invite code:</p><p>1. Paste the host’s invite code:</p><textarea id="jc"></textarea><button class="btn sm g2" id="jgo">CREATE REPLY CODE</button>
<p>2. Send this reply code back to the host:</p><textarea id="jo" readonly></textarea><button class="btn sm g2" onclick="cp('jo')">COPY</button><p id="jst">Waiting for connection…</p><button class="btn sm g2 bk" data-to="p_menu">BACK</button></div></div>
<div class="ov" id="start"><h1>STRIKE <span>ARENA</span></h1><div>4 v 4 Battle Royale Match · First team to 40 wins</div>
<small><br>WASD move · Shift sprint · Space jump · Mouse aim · Click shoot · Right-click zoom<br>R reload · B buy menu · 1–5 switch weapon<br>📱 Phone (landscape): left stick move · drag right side to aim · 🔫 fire · BUY / SWAP buttons<br>You are on the <b style="color:var(--b)">BLUE</b> team with 3 bot allies. Earn $300 per kill.</small>
<button class="btn" id="go">DEPLOY</button><button class="btn sm g2" id="sbtn">SETTINGS</button></div>
<div class="ov" id="pause"><h1>PAUSED</h1><button class="btn" id="res">RESUME</button><button class="btn sm g2" id="ps">SETTINGS</button></div>
<div class="ov" id="buy"><h1 style="font-size:32px">ARMORY <span id="bm"></span></h1><div class="grid" id="grid"></div><button class="btn" id="cb">CLOSE (B)</button></div>
<div id="tools"><button class="sq" id="bfs">⛶</button><button class="sq" id="bgr">⚙</button></div>
<div id="ed"><span>Drag buttons to move them</span><button class="btn" id="edone">DONE</button><button class="btn g2" id="ereset">RESET</button></div>
<div class="ov" id="set"><h1 style="font-size:32px">SETTINGS</h1>
<div class="sp"><label>Look sensitivity <b id="svl"></b></label><input type="range" id="sv" min=".3" max="3" step=".1"></div>
<div class="sp to"><label>Button size <b id="bvl"></b></label><input type="range" id="bv" min=".7" max="1.6" step=".1"></div>
<button class="btn sm g2 to" id="ebtn">EDIT BUTTON POSITIONS</button><button class="btn sm g2" id="sfs">FULLSCREEN</button><button class="btn sm" id="scl">CLOSE</button></div>
<div class="ov" id="over"><h1 id="ot"></h1><div id="os"></div><button class="btn" onclick="location.reload()">PLAY AGAIN</button></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
const $=id=>document.getElementById(id),R=Math.random,T=THREE;
const W={pistol:{n:'Pistol',d:25,r:.28,m:12,rl:1.1,s:.01,p:1,a:0,c:0,i:'Free sidearm'},
smg:{n:'SMG',d:14,r:.075,m:30,rl:1.6,s:.035,p:1,a:1,c:700,i:'Fast & spraying'},
shotgun:{n:'Shotgun',d:11,r:.8,m:6,rl:2.2,s:.075,p:8,a:0,c:1100,i:'Brutal up close'},
rifle:{n:'Assault Rifle',d:24,r:.1,m:30,rl:2,s:.016,p:1,a:1,c:1700,i:'All-rounder'},
sniper:{n:'Sniper',d:110,r:1.2,m:5,rl:2.5,s:.001,p:1,a:0,c:2500,i:'One-shot power'}};
const ORDER=['pistol','smg','shotgun','rifle','sniper'];
const mob=/Mobi|Android|iPhone|iPad/i.test(navigator.userAgent);let renderer=new T.WebGLRenderer({antialias:false,powerPreference:'high-performance'});renderer.setSize(innerWidth,innerHeight);renderer.setPixelRatio(mob?1:Math.min(devicePixelRatio,1.5));document.body.prepend(renderer.domElement);
const scene=new T.Scene();scene.background=new T.Color(0x8fb8e8);scene.fog=new T.Fog(0x8fb8e8,30,110);
const cam=new T.PerspectiveCamera(75,innerWidth/innerHeight,.1,200);scene.add(cam);
scene.add(new T.HemisphereLight(0xffffff,0x445566,.9));const sun=new T.DirectionalLight(0xffffff,.7);sun.position.set(30,60,20);scene.add(sun);
const floor=new T.Mesh(new T.PlaneGeometry(100,100),new T.MeshLambertMaterial({color:0x4f7a3a}));floor.rotation.x=-Math.PI/2;scene.add(floor);

// map
const obs=[],obsM=[];function wall(x,z,w,d,h,c){const m=new T.Mesh(new T.BoxGeometry(w,h,d),new T.MeshLambertMaterial({color:c}));m.position.set(x,h/2,z);scene.add(m);obs.push({x,z,w,d});obsM.push(m)}
wall(0,-50,100,2,6,0x555d6b);wall(0,50,100,2,6,0x555d6b);wall(-50,0,2,100,6,0x555d6b);wall(50,0,2,100,6,0x555d6b);
const L={t1:[],t2:[],t3:[],hb:[],hr:[],hd:[]},HS=[],hc=[0xd9c7a3,0xb8c4cf,0xc98f78,0xe3d9c3],gc=[0x2f6b2f,0x3a7d34,0x27602e];
for(let i=0;i<300&&HS.length<9;i++){const x=(R()-.5)*80,z=(R()-.5)*62,w=6+R()*3;if(HS.some(o=>Math.hypot(o.x-x,o.z-z)<14)||hitObs(x,z,w/2+1))continue;HS.push({x,z});obs.push({x,z,w,d:w});const dz=z+w/2+.05;
 L.hb.push({x,y:1.6,z,sx:w,sy:3.2,sz:w,c:hc[HS.length%4]});L.hr.push({x,y:4.3,z,sx:w*.78,sy:2.2,sz:w*.78,ry:Math.PI/4,c:0x8b3a2b});
 L.hd.push({x,y:.9,z:dz,sx:1,sy:1.8,sz:.12,c:0x4a2f1b},{x:x-w/4,y:1.9,z:dz,sx:1,sy:.9,sz:.12,c:0x9fd4f0},{x:x+w/4,y:1.9,z:dz,sx:1,sy:.9,sz:.12,c:0x9fd4f0})}
for(let i=0;i<120&&L.t1.length<42;i++){const x=(R()-.5)*92,z=(R()-.5)*92;if(hitObs(x,z,2.5))continue;obs.push({x,z,w:1.2,d:1.2});const s=.8+R()*.6,c=gc[i%3];
 L.t1.push({x,y:1.2*s,z,sx:s,sy:s,sz:s,c:0x6b4423});L.t2.push({x,y:3.1*s,z,sx:s,sy:s,sz:s,c});L.t3.push({x,y:4.3*s,z,sx:.7*s,sy:.7*s,sz:.7*s,c:0x3f8a38})}
function inst(geo,list){const im=new T.InstancedMesh(geo,new T.MeshLambertMaterial(),list.length),m=new T.Matrix4(),q=new T.Quaternion(),Y=new T.Vector3(0,1,0),p=new T.Vector3(),s=new T.Vector3();
 list.forEach((o,i)=>{q.setFromAxisAngle(Y,o.ry||0);m.compose(p.set(o.x,o.y,o.z),q,s.set(o.sx,o.sy,o.sz));im.setMatrixAt(i,m);im.setColorAt(i,new T.Color(o.c))});im.frustumCulled=false;scene.add(im);obsM.push(im)}
const cone=new T.ConeGeometry(1.6,2.6,7),ub=new T.BoxGeometry(1,1,1);
inst(new T.CylinderGeometry(.25,.35,2.4,6),L.t1);inst(cone,L.t2);inst(cone,L.t3);inst(ub,L.hb);inst(new T.ConeGeometry(1,1,4),L.hr);inst(ub,L.hd);
function hitObs(x,z,r){for(const o of obs)if(Math.abs(x-o.x)<o.w/2+r&&Math.abs(z-o.z)<o.d/2+r)return true;return false}
function los(a,b){const dx=b.x-a.x,dz=b.z-a.z,n=Math.ceil(Math.hypot(dx,dz)/.8);for(let i=1;i<n;i++)if(hitObs(a.x+dx*i/n,a.z+dz*i/n,0))return false;return true}
// entities
const ents=[],score={b:0,r:0},meshes=[];let kills=0,deaths=0;
const spawn=t=>({x:(R()-.5)*60,z:t=='b'?38+R()*6:-38-R()*6});
function mk(team,isP){const e={team,isP,hp:100,alive:true,x:0,z:0,t:0,cd:0,tgt:null,ph:R()*9,name:isP?'You':(team=='b'?'Ally ':'Foe ')+(ents.length)};
 if(!isP)buildBot(e);
 e.id=ents.length;respawn(e);ents.push(e);return e}
function respawn(e){const s=spawn(e.team);let n=0;while(hitObs(s.x,s.z,1)&&n++<30){s.x=(R()-.5)*60}e.x=s.x;e.z=s.z;e.hp=100;e.alive=true;if(e.g)e.g.visible=true}
const MC={},mat=c=>MC[c]||(MC[c]=new T.MeshLambertMaterial({color:c}));
const bx=new T.BoxGeometry(1,1,1),gh=new T.SphereGeometry(.2,8,6),gm=new T.SphereGeometry(.23,8,6,0,6.283,0,1.4),SK=[0xf1c9a5,0xc68a5f,0x8d5a3b,0x5c3a24],PT=[0x3b4252,0x4a4036,0x2e3a2e];
function buildBot(e){const g=new T.Group(),tc=e.team=='b'?0x2f7de0:0xe0343f,dk=e.team=='b'?0x1d4f94:0x962029,sk=SK[(R()*4)|0],pt=PT[(R()*3)|0];e.parts=[];e.lg=[];
 const A=(geo,c,x,y,z,sx,sy,sz,par,h)=>{const m=new T.Mesh(geo,mat(c));m.position.set(x,y,z);if(sx)m.scale.set(sx,sy,sz);m.userData={e,h};(par||g).add(m);e.parts.push(m);return m};
 A(bx,tc,0,1.2,0,.56,.65,.3);A(bx,dk,0,.88,0,.5,.12,.28);A(bx,pt,0,.78,0,.5,.2,.28);A(bx,sk,0,1.58,0,.1,.1,.1);A(gh,sk,0,1.68,0,0,0,0,null,1);A(gm,dk,0,1.7,0,0,0,0,null,1);
 for(const s of[-1,1]){const lg=new T.Group();lg.position.set(s*.13,.72,0);g.add(lg);A(bx,pt,0,-.3,0,.2,.62,.22,lg);A(bx,0x1a1a1a,0,-.66,.04,.22,.12,.3,lg);e.lg.push(lg);
  const ar=new T.Group();ar.position.set(s*.37,1.45,0);ar.rotation.x=s>0?-1.3:-1.1;ar.rotation.y=s>0?-.1:.45;g.add(ar);A(bx,tc,0,-.2,0,.15,.4,.15,ar);A(bx,sk,0,-.45,0,.13,.2,.13,ar)}
 A(bx,0x111111,.3,1.25,.4,.08,.1,.7);A(bx,0x333333,.3,1.2,.62,.05,.05,.3);scene.add(g);e.g=g}
const P=mk('b',1);P.vy=0;P.yaw=Math.PI;P.pitch=0;
for(let i=0;i<3;i++)mk('b');for(let i=0;i<4;i++)mk('r');
// weapons state
const inv={pistol:{m:12,res:60}};let cur='pistol',money=1000,fireT=0,relT=0,zoom=false;
function setHUD(){const w=W[cur],s=inv[cur];$('wn').textContent=w.n.toUpperCase();$('an').textContent=relT>0?'RELOADING':s.m+' / '+s.res;$('mo').textContent='$'+money;
 $('hpn').textContent=Math.max(0,Math.ceil(P.hp));$('hpb').style.width=Math.max(0,P.hp)+'%';$('k').textContent=kills;$('d').textContent=deaths;$('sb').textContent=score.b;$('sr').textContent=score.r}
// viewmodel
const vm=new T.Mesh(new T.BoxGeometry(.1,.12,.6),new T.MeshLambertMaterial({color:0x222831}));vm.position.set(.25,-.22,-.5);
