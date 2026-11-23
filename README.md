<!DOCTYPE html>
<html lang="pl"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>GUI Maker – Minecraft 1.21.11</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
<style>
:root{--bg:#1e1e1e;--pn:#2b2b2b;--tx:#f0f0f0;--mu:#a0a0a0;--ac:#55aa55;--bd:#4a4a4a;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media(prefers-color-scheme:light){:root{--bg:#c6c6c6;--pn:#e6e6e6;--tx:#202020;--mu:#555;--bd:#8b8b8b}}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--tx);font:14px system-ui,Segoe UI,sans-serif}
h1{font-size:18px;margin:0;font-variant:small-caps;letter-spacing:.06em}
h3{font-variant:small-caps;letter-spacing:.06em;font-size:13px;color:var(--ac);margin:14px 0 6px}
header{padding:12px 16px;border-bottom:2px solid var(--bd);display:flex;gap:12px;align-items:center;flex-wrap:wrap}
main{display:grid;grid-template-columns:290px minmax(0,1fr) 250px;gap:12px;padding:12px;align-items:start}
@media(max-width:1000px){main{grid-template-columns:1fr}}
.box{background:var(--pn);border:2px solid var(--bd);padding:12px}
label{display:flex;justify-content:space-between;align-items:center;gap:8px;margin:5px 0;font-variant:small-caps;color:var(--mu)}
input[type=text],select{background:transparent;color:var(--tx);border:2px solid var(--bd);padding:3px 6px;width:130px;font:inherit}
input[type=color]{border:0;background:none;width:36px;height:24px;padding:0}
input[type=range]{width:120px;accent-color:var(--ac)}
button{background:#7a7a7a;color:#fff;border:2px solid #000;border-top-color:#bbb;border-left-color:#bbb;padding:5px 9px;font:inherit;cursor:pointer;font-variant:small-caps}
button.p{background:#4a8a3a}
button.on{background:#3a6a2a;outline:2px solid #ffff55}
.row{display:flex;gap:6px;flex-wrap:wrap;margin:6px 0;align-items:center}
.item{display:flex;justify-content:space-between;padding:6px 8px;cursor:pointer;border:2px solid transparent}
.item.sel{border-color:var(--ac)}
.stage{overflow:auto;text-align:center;padding:16px;background:repeating-conic-gradient(#8884 0 25%,#0000 0 50%) 0 0/16px 16px}
canvas#pv{max-width:100%;image-rendering:pixelated;cursor:crosshair;touch-action:none}
textarea{width:100%;height:54px;background:transparent;color:var(--tx);border:2px solid var(--bd);font:12px monospace}
small{color:var(--mu)}
</style></head><body>
<header><h1>GUI Maker</h1><small>1–4 narzędzia · Shift+klik = cały rząd · Alt+klik = cała kolumna · Ctrl+Z cofnij · Ctrl+Y ponów · Delete usuwa zaznaczoną ramkę · projekt zapisuje się sam</small></header>
<main>
<section class="box"><h3 style="margin-top:0">Narzędzia</h3><div class="row"><button id="undo">↶ cofnij</button><button id="redo">↷ ponów</button><label style="margin:0">zoom<input type="range" id="zm" min="2" max="5" step=".5" value="3"></label></div>
<div class="row" id="tools">
<button data-t="fill" class="on">wypełnij slot</button><button data-t="round">zaokrąglij slot</button><button data-t="frame">ramka</button><button data-t="erase">gumka</button></div>
<label>wypełnienie kolorem tła panelu<input type="checkbox" id="fbg" checked></label><label>własny kolor wypełnienia<input type="color" id="pc" value="#ffaa00"></label><div class="row" id="pal"></div>
<label>promień zaokrąglenia<input type="range" id="rr" min="0" max="8" step="0.5" value="3"></label>
<label>napis ramki<input type="text" id="ft" value="Sklep"></label>
<label>szerokość ramki<input type="range" id="fw" min="30" max="176" value="100"></label>
<small id="hint"></small><div class="row"><button class="p" id="addf">+ dodaj ramkę z napisem</button></div>
<div class="row"><button id="fall">wypełnij wszystkie</button><button id="rall">zaokrąglij wszystkie</button><button id="clr">wyczyść</button></div>
<h3>Preset wyglądu</h3><select id="pre" style="width:100%"><option value="">wybierz…</option><option value="vanilla">Minecraft (klasyczny)</option><option value="dark">ciemny</option><option value="soft">zaokrąglony jasny</option><option value="night">nocny niebieski</option></select><div id="form"></div>
<h3>Tło jako tekstura</h3><div class="row"><input type="file" id="img" accept="image/*"><button id="noimg">usuń</button></div>
</section>
<section class="box"><div class="stage"><canvas id="pv"></canvas></div>
<small id="sb"></small><br><small>Wybierz narzędzie po lewej i klikaj lub przeciągaj po slotach. Ramkę możesz przeciągać myszką.</small></section>
<section class="box"><h3 style="margin-top:0">Lista GUI</h3><div id="list"></div>
<div class="row"><button class="p" id="add">+ dodaj do listy</button><button id="vn">+ zwykłe GUI Minecraft</button><button id="dup">kopiuj</button><button id="del">usuń</button></div>
<h3>Znak GUI (glyph)</h3>
<label>typ<select id="gm"><option value="pua">prywatny</option><option value="cjk">chińskie znaczki</option></select></label>
<label>znak<input type="text" id="gl" maxlength="2"></label>
<div class="row"><button id="rg">losuj znak</button></div>
<h3>Tytuł do kodu</h3><textarea id="out" readonly></textarea><div class="row"><button id="cp">kopiuj tytuł</button></div>
<small><b>Wstaw cały tytuł</b> jako nazwę ekwipunku, razem z pierwszym znakiem \uF808. To przesunięcie o 8 pikseli w lewo, bez niego GUI jest przesunięte w prawo. Tytuł musi mieć kolor biały, inaczej tekstura ściemnieje.</small>
<h3>Texture pack 1.21.11</h3>
<label>nazwa<input type="text" id="pn" value="Custom GUI"></label>
<label>rozdzielczość<select id="res"><option value="1">1x</option><option value="2">2x</option><option value="4" selected>4x</option><option value="8">8x</option></select></label>
<label>ukryj domyślne tło skrzyni<input type="checkbox" id="hide"></label><small>Zostaw odznaczone, bo wtedy inne menu skrzyń działają normalnie. Zaznaczenie robi z nich przezroczyste okna.</small>
<div class="row"><button class="p" id="mk">stwórz texture pack</button></div><div class="row"><button id="sp">zapisz projekt</button><input type="file" id="lp" accept=".json" style="width:100%"></div><small id="st"></small>
</section></main>
<script>
const $=id=>document.getElementById(id),W=176,PAD=7,FH=14;
const SH=[[128,'\uF880'],[64,'\uF840'],[32,'\uF820'],[16,'\uF810'],[8,'\uF808'],[4,'\uF804'],[2,'\uF802'],[1,'\uF801']];
let guis=[],cur=0,tool='fill',down=false,drag=null,hov=null;
const H=g=>114+g.rows*18;
const HINTS={fill:'Klik w slot: zamalowuje go kolorem tła panelu i usuwa jego ramkę. Ramka zostaje tylko tam, gdzie slot jest niezamalowany. Odznacz opcję, żeby użyć własnego koloru.',round:'Klik w slot: ustawia mu zaokrąglenie.',frame:'Klik stawia ramkę na górnej krawędzi. Napis wpisz w polu „napis ramki”, zmienia się na bieżąco. Klik w ramkę ją zaznacza.',erase:'Klik w slot lub ramkę: usuwa wypełnienie, zaokrąglenie lub ramkę.'};
function rnd(){const m=$('gm').value;for(let i=0;i<999;i++){const c=String.fromCharCode(m==='cjk'?0x4E00+Math.floor(Math.random()*0x5000):0xE000+Math.floor(Math.random()*0x1000));if(!guis.some(g=>g.glyph===c))return c}return'\uE000'}
function mk(n){return{name:n,rows:3,inv:true,glyph:rnd(),bevel:true,bg:'#c6c6c6',radius:0,slot:'#8b8b8b',slotR:0,tColor:'#404040',paint:{},srad:{},frames:[],img:null}}
const F=[['Układ'],['name','nazwa GUI','text'],['rows','ilość rzędów slotów','range',1,6,1],['inv','sloty ekwipunku gracza','check'],
['Wygląd'],['bevel','klasyczne ramki 3D','check'],['bg','kolor panelu','color'],['radius','zaokrąglenie panelu','range',0,20,1],['slot','kolor slotów','color'],['slotR','zaokrąglenie slotów','range',0,8,.5],['tColor','kolor napisu ramek','color']];
function slots(g){const a=[];for(let r=0;r<g.rows;r++)for(let c=0;c<9;c++)a.push({k:'k'+r+','+c,x:8+c*18,y:18+r*18});
if(g.inv){const y0=18+g.rows*18+13;for(let r=0;r<3;r++)for(let c=0;c<9;c++)a.push({k:'i'+r+','+c,x:8+c*18,y:y0+r*18});for(let c=0;c<9;c++)a.push({k:'h'+c,x:8+c*18,y:y0+58})}return a}
function sc(x,t,cx,cy,sz,col){x.fillStyle=col;x.textBaseline='middle';x.textAlign='left';
const ps=[...t].map(ch=>({c:ch.toUpperCase(),s:ch!==ch.toUpperCase()?sz*.78:sz}));let w=0;
ps.forEach(p=>{x.font='700 '+p.s+'px system-ui,sans-serif';p.w=x.measureText(p.c).width+.4;w+=p.w});
let px=cx-w/2;ps.forEach(p=>{x.font='700 '+p.s+'px system-ui,sans-serif';x.fillText(p.c,px,cy+.5);px+=p.w})}
function box(x,X,Y,w,h,r,fill,bv,tl,br,ol){x.beginPath();x.roundRect(X,Y,w,h,r);x.fillStyle=fill;x.fill();
if(r>0){if(ol){x.strokeStyle=ol;x.lineWidth=1;x.beginPath();x.roundRect(X+.5,Y+.5,w-1,h-1,Math.max(0,r-.5));x.stroke()}return}
let i=0;if(ol){x.fillStyle=ol;x.fillRect(X,Y,w,1);x.fillRect(X,Y+h-1,w,1);x.fillRect(X,Y,1,h);x.fillRect(X+w-1,Y,1,h);i=1}
if(bv){x.fillStyle=tl;x.fillRect(X+i,Y+i,w-2*i-1,1);x.fillRect(X+i,Y+i,1,h-2*i-1);x.fillStyle=br;x.fillRect(X+i+1,Y+h-i-1,w-2*i-1,1);x.fillRect(X+w-i-1,Y+i+1,1,h-2*i-1)}}
function render(g,S,pad){const h=H(g),cv=document.createElement('canvas');cv.width=W*S;cv.height=(h+pad)*S;const x=cv.getContext('2d');x.scale(S,S);x.translate(0,pad);
box(x,0,0,W,h,g.radius,g.bg,g.bevel,'#fff','#555','#000');
if(g.img){x.save();x.beginPath();x.roundRect(1,1,W-2,h-2,Math.max(0,g.radius-1));x.clip();x.drawImage(g.img,0,0,W,h);x.restore()}
for(const s of slots(g)){if(g.paint[s.k]==='bg')continue;const r=g.srad[s.k]!==undefined?g.srad[s.k]:g.slotR;box(x,s.x-1,s.y-1,18,18,r,g.paint[s.k]||g.slot,g.bevel,'#373737','#fff','#373737')}
for(const f of g.frames){box(x,f.x,-FH/2,f.w,FH,0,g.bg,g.bevel,'#fff','#555','#000');sc(x,f.text,f.x+f.w/2,0,8,g.tColor)}
return cv}
function draw(){const g=guis[cur],cv=$('pv'),r=render(g,6,PAD),c=cv.getContext('2d');cv.width=r.width;cv.height=r.height;c.drawImage(r,0,0);cv.style.width=(W*$('zm').value)+'px';
if(hov){const s=slots(g).find(q=>q.k===hov);if(s){c.strokeStyle='#ffff55';c.lineWidth=3;c.strokeRect((s.x-1)*6+1.5,(s.y-1+PAD)*6+1.5,105,105)}}
info();try{localStorage.setItem('guimaker',JSON.stringify({cur,guis:guis.map(x=>({...x,img:null,sel:null}))}))}catch(e){}}
function shift(n){let s='',m=n;for(const[v,c]of SH)while(m>=v){s+=c;m-=v}return s}
const title=g=>shift(8)+g.glyph+shift(W+1-8);
const esc=s=>s.replace(/[\u007f-\uffff]/g,c=>'\\u'+c.charCodeAt(0).toString(16).toUpperCase().padStart(4,'0'));
function info(){const g=guis[cur];$('out').value=esc(title(g));$('gl').value=g.glyph}
function list(){$('list').innerHTML='';guis.forEach((g,i)=>{const d=document.createElement('div');d.className='item'+(i===cur?' sel':'');const a=document.createElement('span');a.textContent=g.name;const b=document.createElement('small');b.textContent=g.rows+' rz.';d.append(a,b);d.onclick=()=>{cur=i;all()};$('list').append(d)})}
function form(){const g=guis[cur],f=$('form');f.innerHTML='';F.forEach(e=>{if(e.length===1){const h=document.createElement('h3');h.textContent=e[0];f.append(h);return}
const[k,l,t,mn,mx,st]=e,lb=document.createElement('label'),sp=document.createElement('span'),i=document.createElement('input');sp.textContent=l;
if(t==='check'){i.type='checkbox';i.checked=g[k]}else if(t==='range'){i.type='range';i.min=mn;i.max=mx;i.step=st;i.value=g[k]}else{i.type=t;i.value=g[k]}
i.oninput=()=>{pt();g[k]=t==='check'?i.checked:t==='range'?+i.value:i.value;if(k==='slotR')g.srad={};if(k==='rows'){const ok=new Set(slots(g).map(s=>s.k));for(const q in g.paint)if(!ok.has(q))delete g.paint[q];for(const q in g.srad)if(!ok.has(q))delete g.srad[q]}if(k==='name'||k==='rows')list();draw()};
lb.append(sp,i);f.append(lb)})}
function all(){hist=[];fut=[];list();form();draw()}
function act(e){const g=guis[cur],b=$('pv').getBoundingClientRect(),gx=(e.clientX-b.left)/b.width*W,gy=(e.clientY-b.top)/b.height*(H(g)+PAD)-PAD,first=e.type==='pointerdown';
const inF=f=>gx>=f.x&&gx<=f.x+f.w&&Math.abs(gy)<=FH/2;if(first)push();
if(tool==='frame'){if(!first){if(drag){drag.f.x=Math.max(0,Math.min(W-drag.f.w,Math.round(gx-drag.o)));draw()}return}let f=g.frames.find(inF);if(f){g.sel=f;drag={f,o:gx-f.x};$('ft').value=f.text;$('fw').value=f.w}else{const w=+$('fw').value;f={w,x:Math.max(0,Math.min(W-w,Math.round(gx-w/2))),text:$('ft').value};g.frames.push(f);g.sel=f}draw();return}
if(tool==='erase'&&first){const i=g.frames.findIndex(inF);if(i>=0){g.frames.splice(i,1);g.sel=null;draw();return}}
for(const s of slots(g))if(gx>=s.x-1&&gx<s.x+17&&gy>=s.y-1&&gy<s.y+17){(e.shiftKey?slots(g).filter(q=>q.y===s.y):e.altKey?slots(g).filter(q=>q.x===s.x):[s]).forEach(q=>{if(tool==='fill')g.paint[q.k]=$('fbg').checked?'bg':$('pc').value;else if(tool==='round')g.srad[q.k]=+$('rr').value;else{delete g.paint[q.k];delete g.srad[q.k]}});draw();return}}
$('pv').onpointerdown=e=>{down=true;act(e)};$('pv').onpointermove=e=>{if(down)act(e);else hover(e)};$('pv').onpointerleave=()=>{if(hov){hov=null;draw()}};addEventListener('pointerup',()=>{down=false;drag=null});
document.querySelectorAll('#tools button').forEach(b=>b.onclick=()=>{tool=b.dataset.t;document.querySelectorAll('#tools button').forEach(x=>x.className=x===b?'on':'');$('hint').textContent=HINTS[tool]});
$('hint').textContent=HINTS.fill;
let hist=[],fut=[],lp=0;
const snap=()=>JSON.stringify({...guis[cur],img:null,sel:null});
function push(){hist.push(snap());if(hist.length>60)hist.shift();fut=[]}
function pt(){const n=Date.now();if(n-lp>700)push();lp=n}
function rest(s){const g=guis[cur],img=g.img;Object.assign(g,JSON.parse(s),{img,sel:null});list();form();draw()}
function undo(){if(!hist.length)return;fut.push(snap());rest(hist.pop())}
function redo(){if(!fut.length)return;hist.push(snap());rest(fut.pop())}
$('undo').onclick=undo;$('redo').onclick=redo;$('zm').oninput=draw;
addEventListener('keydown',e=>{if(/INPUT|TEXTAREA|SELECT/.test(e.target.tagName))return;const k=e.key.toLowerCase(),c=e.ctrlKey||e.metaKey;
if(c&&k==='z'){e.preventDefault();e.shiftKey?redo():undo()}else if(c&&k==='y'){e.preventDefault();redo()}
else if(k.length===1&&'1234'.includes(k)){document.querySelector('#tools button:nth-child('+k+')').click()}
else if(k==='delete'||k==='backspace'){const g=guis[cur];if(g.sel){push();g.frames=g.frames.filter(f=>f!==g.sel);g.sel=null;draw()}}});
function hover(e){const g=guis[cur],b=$('pv').getBoundingClientRect(),gx=(e.clientX-b.left)/b.width*W,gy=(e.clientY-b.top)/b.height*(H(g)+PAD)-PAD;const s=slots(g).find(q=>gx>=q.x-1&&gx<q.x+17&&gy>=q.y-1&&gy<q.y+17),k=s?s.k:null;
if(k!==hov){hov=k;draw()}$('sb').textContent=s?'slot: '+s.k.replace(/^k/,'skrzynia ').replace(/^i/,'ekwipunek ').replace(/^h/,'hotbar '):''}
['#ffffff','#c6c6c6','#8b8b8b','#555555','#000000','#aa0000','#ff5555','#ffaa00','#ffff55','#00aa00','#55ff55','#00aaaa','#55ffff','#0000aa','#5555ff','#aa00aa','#ff55ff'].forEach(c=>{const b=document.createElement('button');b.style.cssText='width:20px;height:20px;padding:0;background:'+c;b.title=c;b.onclick=()=>{$('pc').value=c;$('fbg').checked=false};$('pal').append(b)});
const PR={vanilla:{bg:'#c6c6c6',slot:'#8b8b8b',bevel:true,radius:0,slotR:0,tColor:'#404040'},dark:{bg:'#2b2b2b',slot:'#151515',bevel:true,radius:0,slotR:0,tColor:'#e0e0e0'},soft:{bg:'#e8e8e8',slot:'#c0c0c0',bevel:false,radius:8,slotR:3,tColor:'#303030'},night:{bg:'#1e2a44',slot:'#0f1830',bevel:false,radius:6,slotR:2,tColor:'#cfe0ff'}};
$('pre').onchange=()=>{const v=$('pre').value;if(!v)return;push();const g=guis[cur];Object.assign(g,PR[v]);g.srad={};$('pre').value='';form();draw()};
$('cp').onclick=()=>{$('out').select();let ok=false;try{ok=document.execCommand('copy')}catch(x){}$('st').textContent=ok?'Skopiowano tytuł.':'Zaznaczone, skopiuj przez Ctrl+C.'};
$('sp').onclick=async()=>{try{const d=await claude.use('downloads');await d.save({filename:'projekt-gui.json',data:JSON.stringify({guis:guis.map(g=>({...g,img:null,sel:null}))})})}catch(x){$('st').textContent='Nie zapisano projektu.'}};
$('lp').onchange=e=>{const f=e.target.files[0];if(!f)return;f.text().then(t=>{try{const d=JSON.parse(t);guis=d.guis.map(g=>({...mk(''),...g,img:null,sel:null}));cur=0;all();$('st').textContent='Wczytano projekt.'}catch(x){$('st').textContent='Zły plik projektu.'}})};
$('ft').oninput=()=>{pt();const g=guis[cur];if(g.sel){g.sel.text=$('ft').value;draw()}};
$('fw').oninput=()=>{pt();const f=guis[cur].sel;if(f){f.w=+$('fw').value;f.x=Math.max(0,Math.min(W-f.w,f.x));draw()}};
$('addf').onclick=()=>{const g=guis[cur],w=+$('fw').value,f={w,x:Math.round((W-w)/2),text:$('ft').value};g.frames.push(f);g.sel=f;draw()};
$('fall').onclick=()=>{const g=guis[cur];slots(g).forEach(s=>g.paint[s.k]=$('fbg').checked?'bg':$('pc').value);draw()};
$('rall').onclick=()=>{const g=guis[cur];slots(g).forEach(s=>g.srad[s.k]=+$('rr').value);draw()};
$('clr').onclick=()=>{const g=guis[cur];g.paint={};g.srad={};g.frames=[];g.sel=null;draw()};
$('add').onclick=()=>{guis.push(mk('gui '+(guis.length+1)));cur=guis.length-1;all()};
$('vn').onclick=()=>{const g=mk('skrzynia');g.rows=3;guis.push(g);cur=guis.length-1;all()};
$('dup').onclick=()=>{const o=guis[cur],g=Object.assign({},o,{paint:{...o.paint},srad:{...o.srad},frames:o.frames.map(f=>({...f})),sel:null,name:o.name+' kopia'});g.glyph=rnd();guis.push(g);cur=guis.length-1;all()};
$('del').onclick=()=>{if(guis.length<2)return;guis.splice(cur,1);cur=Math.max(0,cur-1);all()};
$('rg').onclick=()=>{guis[cur].glyph=rnd();info()};
$('gl').oninput=()=>{const v=[...$('gl').value][0];if(v&&!guis.some((g,i)=>i!==cur&&g.glyph===v)){guis[cur].glyph=v;$('out').value=esc(title(guis[cur]))}};
$('img').onchange=e=>{const f=e.target.files[0];if(!f)return;const im=new Image();im.onload=()=>{guis[cur].img=im;draw()};im.src=URL.createObjectURL(f)};
$('noimg').onclick=()=>{guis[cur].img=null;draw()};
const blob=c=>new Promise(r=>c.toBlob(r,'image/png'));
$('mk').onclick=async()=>{const st=$('st');try{
const S=+$('res').value,z=new JSZip(),adv={};SH.forEach(([v,c])=>adv[c]=-v);
z.file('pack.mcmeta',JSON.stringify({pack:{description:$('pn').value,min_format:[75,0],max_format:[75,0]}},null,2));
const prov=[{type:'space',advances:adv}];let readme='Tytuly GUI (Java, format \\uXXXX, kolor bialy):\n\n';
for(let i=0;i<guis.length;i++){const g=guis[i],id='gui_'+i,p=g.frames.length?PAD:0;z.file('assets/minecraft/textures/font/gui/'+id+'.png',await blob(render(g,S,p)));
prov.push({type:'bitmap',file:'minecraft:font/gui/'+id+'.png',ascent:13+p,height:H(g)+p,chars:[g.glyph]});readme+=g.name+' ('+g.rows+' rzedow): '+esc(title(g))+'\n'}
z.file('assets/minecraft/font/default.json',esc(JSON.stringify({providers:prov},null,2)));
if($('hide').checked){const e=document.createElement('canvas');e.width=e.height=256;z.file('assets/minecraft/textures/gui/container/generic_54.png',await blob(e))}
z.file('README.txt',readme);
const out=await z.generateAsync({type:'blob'}),d=await claude.use('downloads');
if(!d){st.textContent='Pobieranie niedostępne w tym widoku.';return}
await d.save({filename:($('pn').value||'gui-pack').replace(/[^\w-]+/g,'_')+'.zip',data:out});st.textContent='Gotowe.'}catch(e){st.textContent='Nie zapisano: '+(e&&e.message||e)}};
try{const d=JSON.parse(localStorage.getItem('guimaker'));if(d&&d.guis&&d.guis.length){guis=d.guis.map(g=>({...mk(''),...g,img:null,sel:null}));cur=Math.min(d.cur||0,guis.length-1)}}catch(e){}
if(!guis.length)guis.push(mk('gui 1'));all();
</script></body></html>
