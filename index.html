<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>Flight Command</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
body,html{width:100%;height:100%;overflow:hidden;background:#010103;font-family:'Segoe UI',Impact,sans-serif;user-select:none}
canvas{display:block;width:100vw;height:100vh;background:radial-gradient(circle at center,#06061c 0%,#000002 100%);cursor:crosshair}
video{display:none}
</style>
</head>
<body>
<canvas id="g"></canvas>
<video id="cv" autoplay playsinline muted></video>
<script>
const c=document.getElementById('g'),ctx=c.getContext('2d');

let p={x:0,y:0,vx:0,vy:0,r:18,hp:100,maxHp:100},
    keys={},bullets=[],targets=[],stars=[],particles=[],boss=null,
    score=0,lvl=1,over=false,overAt=0,paused=false,shake=0,lShot=0,gTime=0,
    lBoss=0,tDest=0,tReq=8,isWarp=false,wTime=0,uMouse=false,
    mouse={x:0,y:0},menu=true,keybindsMenu=false;

/* ---------- WEBCAM CONTROL ---------- */
const CW=64,CH=48;
const cam={on:false,v:document.getElementById('cv'),prev:null,tx:0,ty:0,motion:0,msg:"",stream:null,
    sc:Object.assign(document.createElement('canvas'),{width:CW,height:CH})};
const sx=cam.sc.getContext('2d',{willReadFrequently:true});

async function toggleCam(){
    if(cam.on){
        cam.stream&&cam.stream.getTracks().forEach(t=>t.stop());
        cam.on=false;cam.prev=null;cam.msg="CAMERA OFF";return;
    }
    if(!navigator.mediaDevices||!navigator.mediaDevices.getUserMedia){
        cam.msg="CAMERA NOT SUPPORTED (use https or localhost)";return;
    }
    cam.msg="REQUESTING CAMERA ACCESS...";
    try{
        cam.stream=await navigator.mediaDevices.getUserMedia({video:{width:640,height:480,facingMode:"user"},audio:false});
        cam.v.srcObject=cam.stream;await cam.v.play();
        cam.on=true;cam.prev=null;cam.tx=p.x;cam.ty=p.y;
        cam.msg="CAMERA ONLINE - MOVE YOUR HAND OR BODY";
    }catch(err){
        cam.on=false;cam.msg="CAMERA BLOCKED: "+(err.name||"error");
    }
}

function camStep(){
    if(!cam.on||cam.v.readyState<2)return;
    sx.drawImage(cam.v,0,0,CW,CH);
    const d=sx.getImageData(0,0,CW,CH).data,cur=new Uint8Array(CW*CH);
    let n=0,mx=0,my=0;
    for(let i=0;i<CW*CH;i++){
        const l=d[i*4]*.3+d[i*4+1]*.59+d[i*4+2]*.11;
        cur[i]=l;
        if(cam.prev&&Math.abs(l-cam.prev[i])>30){n++;mx+=i%CW;my+=(i/CW)|0;}
    }
    cam.prev=cur;cam.motion=n;
    if(n>14){
        const tx=(1-(mx/n)/CW)*c.width,ty=(my/n/CH)*c.height; // mirrored so it feels natural
        cam.tx+=(tx-cam.tx)*.35;cam.ty+=(ty-cam.ty)*.35;
    }
}

function drawCamBackground(){
    if(!cam.on||cam.v.readyState<2)return;
    ctx.save();ctx.globalAlpha=.28;
    ctx.translate(c.width,0);ctx.scale(-1,1);
    ctx.drawImage(cam.v,0,0,c.width,c.height);
    ctx.restore();
}

function drawCamPreview(){
    if(!cam.on||cam.v.readyState<2)return;
    const w=160,h=120,x=c.width-w-15,y=c.height-h-15;
    ctx.save();ctx.globalAlpha=.9;
    ctx.translate(x+w,y);ctx.scale(-1,1);
    ctx.drawImage(cam.v,0,0,w,h);ctx.restore();
    ctx.strokeStyle="#00ffcc";ctx.lineWidth=2;ctx.strokeRect(x,y,w,h);
    ctx.fillStyle=cam.motion>14?"#00ffcc":"#ff5555";
    ctx.beginPath();
    ctx.arc(x+(cam.tx/c.width)*w,y+(cam.ty/c.height)*h,5,0,7);ctx.fill();
    ctx.textAlign="left";
    txt(cam.motion>14?"TRACKING":"NO MOTION",x,y-6,`bold 11px monospace`,ctx.fillStyle);
}

/* ---------- MENU ---------- */
const buttons=[
    {text:"START GAME",action:"start"},
    {text:"CAMERA: OFF",action:"camera"},
    {text:"KEYBINDS",action:"keybinds"},
    {text:"CREDITS",action:"credits"}
].map(b=>({...b,x:0,y:0,w:0,h:0}));

function resize(){
    c.width=window.innerWidth;c.height=window.innerHeight;
    if(!p.x){p.x=c.width/2;p.y=c.height*.8}
    stars=Array.from({length:Math.floor((c.width*c.height)/9000)},
        ()=>({x:Math.random()*c.width,y:Math.random()*c.height,s:Math.random()*2+.5,v:Math.random()*3+.5}));
}

function kL(e,v){
    let k=e.key.toLowerCase();
    if(v&&k==="l"){keybindsMenu=!keybindsMenu;return;}
    if(v&&k==="c"){toggleCam();return;}
    if(v&&k==="r"&&!menu&&!over){resetGame();spawn();return;}
    if(v&&!over&&(k==='escape'||k==='p'))paused=!paused;
    keys[k]=v;
    if(v)uMouse=false;
}

function mL(e){
    uMouse=true;
    if(e.touches){mouse.x=e.touches[0].clientX;mouse.y=e.touches[0].clientY;}
    else{mouse.x=e.clientX;mouse.y=e.clientY;}
}

window.addEventListener('keydown',e=>kL(e,true));
window.addEventListener('keyup',e=>kL(e,false));
window.addEventListener('blur',()=>{keys={}});
window.addEventListener('mousemove',mL);

function checkButtonClick(mx,my){
    for(let b of buttons)
        if(mx>b.x&&mx<b.x+b.w&&my>b.y&&my<b.y+b.h)return b.action;
    return null;
}

function menuAction(act){
    if(act==="start"){resetGame();menu=false;spawn();}
    else if(act==="camera")toggleCam();
    else if(act==="keybinds")keybindsMenu=true;
    else if(act==="credits")alert("Flight Command\nMade for a birthday party.\nHave fun!");
}

let lastTouch=0;
window.addEventListener('mousedown',e=>{
    if(Date.now()-lastTouch<600)return;
    mL(e);
    if(keybindsMenu){keybindsMenu=false;return;}
    if(menu){menuAction(checkButtonClick(mouse.x,mouse.y));return;}
    if(e.button===0){
        if(!over&&mouse.x>20&&mouse.x<150&&mouse.y>c.height-45&&mouse.y<c.height-5){paused=!paused;return;}
        keys[' ']=true;
    }
});
window.addEventListener('mouseup',e=>{if(e.button===0)keys[' ']=false;});

window.addEventListener('touchstart',e=>{
    e.preventDefault();lastTouch=Date.now();mL(e);
    if(keybindsMenu){keybindsMenu=false;return;}
    if(menu){menuAction(checkButtonClick(mouse.x,mouse.y));return;}
    if(!over&&mouse.x>20&&mouse.x<150&&mouse.y>c.height-45&&mouse.y<c.height-5){paused=!paused;return;}
    keys[' ']=true;
},{passive:false});
window.addEventListener('touchmove',mL,{passive:true});
window.addEventListener('touchend',()=>{keys[' ']=false;});

/* ---------- GAME ---------- */
function pcle(x,y,col,cnt=5,sp=5){
    for(let i=0;i<cnt;i++)
        particles.push({x,y,color:col,vx:(Math.random()-.5)*sp,vy:(Math.random()-.5)*sp,alpha:1,s:Math.random()*3+1});
}

function spawn(){
    if(isWarp||boss)return;
    for(let i=0;i<3+lvl;i++){
        let r=Math.random()*22+12;
        targets.push({x:Math.random()*(c.width-r*2)+r,y:Math.random()*-600-r,r,
            s:Math.random()*(1.2+lvl*.15)+1.5,hp:Math.ceil(r/6),maxHp:Math.ceil(r/6),
            rot:Math.random()*Math.PI,rotS:(Math.random()-.5)*.04});
    }
}

function spawnBoss(){
    targets=[];
    boss={x:c.width/2,y:-150,tx:c.width/2,ty:120,r:65,hp:50+lvl*40,maxHp:50+lvl*40,vx:3,dir:1};
    pcle(c.width/2,100,'#ff0055',30,8);
}

function resetGame(){
    p={x:c.width/2,y:c.height*.8,vx:0,vy:0,r:18,hp:100,maxHp:100};
    bullets=[];targets=[];particles=[];boss=null;keys={};
    score=0;lvl=1;over=false;tDest=0;tReq=8;
    isWarp=false;gTime=0;lBoss=0;shake=0;
    cam.tx=p.x;cam.ty=p.y;
}

function update(){
    if(menu||keybindsMenu||paused||over)return;
    if(shake>0)shake*=.9;
    if(!isWarp)gTime+=1/60;
    camStep();

    if(gTime-lBoss>=60&&!boss&&!isWarp){spawnBoss();lBoss=gTime;}

    if(isWarp){
        wTime--;shake=3;
        if(Math.random()>.05)pcle(Math.random()*c.width,0,'#00ffff',1,2);
        if(wTime<=0){
            isWarp=false;lvl++;tDest=0;tReq=8+lvl*4;
            bullets=[];targets=[];spawn();
        }
    }

    if(cam.on){
        p.vx=(cam.tx-p.x)*.15;
        p.vy=(cam.ty-p.y)*.15;
    }else if(uMouse){
        p.vx=(mouse.x-p.x)*.18;
        p.vy=(mouse.y-p.y)*.18;
    }else{
        let acc=.9,fric=.86;
        if(keys['a']||keys['arrowleft'])p.vx-=acc;
        if(keys['d']||keys['arrowright'])p.vx+=acc;
        if(keys['w']||keys['arrowup'])p.vy-=acc;
        if(keys['s']||keys['arrowdown'])p.vy+=acc;
        p.vx*=fric;p.vy*=fric;
    }

    p.x=Math.max(p.r,Math.min(c.width-p.r,p.x+p.vx));
    p.y=Math.max(p.r,Math.min(c.height-p.r,p.y+p.vy));

    if(Math.random()>.2){
        pcle(p.x-6,p.y+p.r-2,'#00ffff',1,2);
        pcle(p.x+6,p.y+p.r-2,'#00ffff',1,2);
    }

    let now=Date.now();
    if((keys[' ']||cam.on)&&now-lShot>120&&!isWarp){ // camera mode auto-fires
        bullets.push({x:p.x-7,y:p.y-p.r,vy:-18},{x:p.x+7,y:p.y-p.r,vy:-18});
        lShot=now;shake=Math.max(shake,1.5);
    }

    stars.forEach(s=>{
        s.y+=isWarp?s.v*12:boss?s.v*.5:s.v;
        if(s.y>c.height){s.y=0;s.x=Math.random()*c.width}
    });

    for(let i=bullets.length-1;i>=0;i--){
        bullets[i].y+=bullets[i].vy;
        if(bullets[i].y<0)bullets.splice(i,1);
    }

    if(boss){
        if(boss.y<boss.ty)boss.y+=(boss.ty-boss.y)*.05;
        else{
            boss.x+=boss.vx*boss.dir;
            if(boss.x>c.width-boss.r||boss.x<boss.r)boss.dir*=-1;
            if(Math.random()<.03+lvl*.01)
                targets.push({x:boss.x+(Math.random()-.5)*40,y:boss.y+20,r:14,s:5+lvl,hp:1,maxHp:1,rot:0,rotS:.1});
        }
        if(Math.hypot(p.x-boss.x,p.y-boss.y)<p.r+boss.r){
            p.hp-=2;shake=10;
            if(p.hp<=0){over=true;overAt=Date.now();pcle(p.x,p.y,'#00ffff',50,10);}
        }
    }else if(targets.length===0&&!isWarp)spawn();

    for(let i=targets.length-1;i>=0;i--){
        let t=targets[i];
        t.y+=t.s;t.rot+=t.rotS;
        if(t.y-t.r>c.height){targets.splice(i,1);continue;}

        if(!isWarp&&Math.hypot(p.x-t.x,p.y-t.y)<p.r+t.r){
            pcle(t.x,t.y,'#ff3300',15,6);
            p.hp-=Math.floor(t.r*.8);shake=15;
            targets.splice(i,1);
            if(p.hp<=0){over=true;overAt=Date.now();pcle(p.x,p.y,'#00ffff',50,10);}
            continue;
        }

        for(let j=bullets.length-1;j>=0;j--){
            if(Math.hypot(bullets[j].x-t.x,bullets[j].y-t.y)<t.r+5){
                pcle(bullets[j].x,t.y,'#00ffff',2,3);
                bullets.splice(j,1);t.hp--;
                if(t.hp<=0){
                    pcle(t.x,t.y,'#ff9900',12,5);
                    score+=Math.floor(t.r*10);tDest++;
                    targets.splice(i,1);
                    if(tDest>=tReq&&!isWarp&&!boss){isWarp=true;wTime=90;}
                }
                break;
            }
        }
    }

    if(boss){
        for(let j=bullets.length-1;j>=0;j--){
            if(Math.hypot(bullets[j].x-boss.x,bullets[j].y-boss.y)<boss.r){
                pcle(bullets[j].x,boss.y,'#ffffff',3,4);
                bullets.splice(j,1);boss.hp--;
                if(boss.hp<=0){
                    pcle(boss.x,boss.y,'#ff0055',60,12);
                    score+=5000*lvl;boss=null;
                    isWarp=true;wTime=100;
                }
                break;
            }
        }
    }

    for(let i=particles.length-1;i>=0;i--){
        particles[i].x+=particles[i].vx;
        particles[i].y+=particles[i].vy;
        particles[i].alpha-=.025;
        if(particles[i].alpha<=0)particles.splice(i,1);
    }
}

function txt(t,x,y,f,col,s=0,sc='#000'){
    ctx.font=f;ctx.fillStyle=col;
    if(s>0){ctx.shadowBlur=s;ctx.shadowColor=sc;}
    ctx.fillText(t,x,y);ctx.shadowBlur=0;
}

function menuOverlay(title,tc,s1,s2,sc,sub){
    ctx.fillStyle='rgba(1,1,3,0.75)';
    ctx.fillRect(0,0,c.width,c.height);
    ctx.textAlign="center";
    txt(title,c.width/2,c.height/2-25,`bold 5.5vw sans-serif`,tc,25,sc);
    txt(s1,c.width/2,c.height/2+30,`2.2vw monospace`,"#ffffff");
    if(s2)txt(s2,c.width/2,c.height/2+70,`1.6vw monospace`,"#8899aa");
    txt(sub,c.width/2,c.height/2+140,`bold 1.3vw monospace`,"#00ffcc",10,"#00ffcc");
}

function mainMenu(){
    ctx.fillStyle="rgba(0,0,0,0.85)";
    ctx.fillRect(0,0,c.width,c.height);
    ctx.textAlign="center";
    txt("FLIGHT COMMAND",c.width/2,c.height/2-170,`bold 6vw monospace`,"#00ffcc",25,"#00ffcc");
    txt("SECTOR DEFENSE PROTOCOL ONLINE",c.width/2,c.height/2-115,`2vw monospace`,"#ffffff");

    buttons[1].text="CAMERA: "+(cam.on?"ON":"OFF");
    let bx=c.width/2-150,by=c.height/2-70;
    buttons.forEach((b,i)=>{
        b.x=bx;b.y=by+i*75;b.w=300;b.h=56;
        ctx.fillStyle="rgba(0,20,30,0.8)";ctx.fillRect(b.x,b.y,b.w,b.h);
        ctx.strokeStyle=(b.action==="camera"&&cam.on)?"#ffee00":"#00ffaa";
        ctx.lineWidth=3;ctx.strokeRect(b.x,b.y,b.w,b.h);
        txt(b.text,b.x+b.w/2,b.y+37,`bold 20px monospace`,ctx.strokeStyle);
    });

    if(cam.msg)txt(cam.msg,c.width/2,c.height-95,`bold 15px monospace`,"#ffee00");
    txt("C: CAMERA  |  L: KEYBINDS  |  P/ESC: PAUSE  |  R: RESTART",c.width/2,c.height-60,`1.4vw monospace`,"#88aaee");
}

function drawKeybindsMenu(){
    ctx.fillStyle="rgba(0,0,0,0.85)";
    ctx.fillRect(0,0,c.width,c.height);
    ctx.textAlign="center";
    txt("KEYBINDS",c.width/2,c.height/2-170,`bold 5vw monospace`,"#00ffcc",25,"#00ffcc");
    [
        "Move: WASD / Arrow Keys",
        "Shoot: SPACE (or click)",
        "Webcam: C  (move hand/body, auto-fire)",
        "Pause: P or ESC",
        "Toggle Keybinds Menu: L",
        "Restart: R",
        "Mouse Aim: Move Mouse (until a key is pressed)"
    ].forEach((b,i)=>txt(b,c.width/2,c.height/2-70+i*40,`1.8vw monospace`,"#ffffff"));
    txt("PRESS L OR CLICK TO CLOSE",c.width/2,c.height/2+250,`bold 1.5vw monospace`,"#00ffaa",10,"#00ffaa");
}

function draw(){
    ctx.save();
    if(shake>1&&!paused&&!over&&!menu&&!keybindsMenu)
        ctx.translate((Math.random()-.5)*shake,(Math.random()-.5)*shake);
    ctx.clearRect(0,0,c.width,c.height);

    if(keybindsMenu){drawKeybindsMenu();ctx.restore();return;}
    if(menu){drawCamBackground();mainMenu();ctx.restore();return;}

    drawCamBackground();

    ctx.fillStyle="#ffffff";
    stars.forEach(s=>{
        ctx.globalAlpha=isWarp?s.s:s.s/2.2;
        ctx.fillRect(s.x,s.y,isWarp?2:s.s,isWarp?s.s*8:s.s);
    });

    particles.forEach(pt=>{
        ctx.globalAlpha=pt.alpha;ctx.fillStyle=pt.color;
        ctx.fillRect(pt.x,pt.y,pt.s,pt.s);
    });
    ctx.globalAlpha=1;

    if(isWarp&&!paused&&!over){
        ctx.textAlign="center";
        txt(`WARPING TO SECTOR 0${lvl+1}`,c.width/2,c.height/2,`bold 4vw monospace`,"#00ffcc",20,"#00ffcc");
    }

    targets.forEach(t=>{
        ctx.save();ctx.translate(t.x,t.y);ctx.rotate(t.rot);
        ctx.fillStyle="#262220";ctx.strokeStyle="#ff5500";ctx.lineWidth=1.5;
        ctx.beginPath();
        for(let i=0;i<6;i++){
            let a=(i/6)*Math.PI*2,rd=t.r*(.8+Math.sin(i*3)*.15);
            if(i===0)ctx.moveTo(Math.cos(a)*rd,Math.sin(a)*rd);
            else ctx.lineTo(Math.cos(a)*rd,Math.sin(a)*rd);
        }
        ctx.closePath();ctx.fill();ctx.stroke();ctx.restore();
    });

    if(boss){
        ctx.save();ctx.translate(boss.x,boss.y);
        ctx.shadowBlur=25;ctx.shadowColor="#ff0055";
        ctx.fillStyle="#161218";ctx.strokeStyle="#ff0055";ctx.lineWidth=3;
        ctx.beginPath();
        ctx.moveTo(0,boss.r);ctx.lineTo(-boss.r,-boss.r*.3);
        ctx.lineTo(-boss.r*.4,-boss.r);ctx.lineTo(boss.r*.4,-boss.r);
        ctx.lineTo(boss.r,-boss.r*.3);
        ctx.closePath();ctx.fill();ctx.stroke();
        ctx.fillStyle="#ff0055";
        ctx.fillRect(-boss.r*.6,-boss.r-15,boss.r*1.2*(boss.hp/boss.maxHp),5);
        ctx.restore();ctx.shadowBlur=0;
    }

    ctx.save();
    ctx.translate(p.x,p.y);
    ctx.shadowBlur=15;ctx.shadowColor="#00f0ff";
    ctx.fillStyle="#0a2530";ctx.strokeStyle="#00f0ff";ctx.lineWidth=2;
    ctx.beginPath();
    ctx.moveTo(-16,12);ctx.lineTo(-22,10);ctx.lineTo(-14,-6);
    ctx.lineTo(14,-6);ctx.lineTo(22,10);ctx.lineTo(16,12);
    ctx.closePath();ctx.fill();ctx.stroke();
    ctx.fillStyle="#101520";
    ctx.beginPath();
    ctx.moveTo(0,-18);ctx.lineTo(-10,10);ctx.lineTo(0,4);
    ctx.lineTo(10,10);ctx.closePath();ctx.fill();ctx.stroke();
    ctx.fillStyle="#ffffff";
    ctx.beginPath();
    ctx.moveTo(0,-10);ctx.lineTo(-4,0);ctx.lineTo(0,3);
    ctx.lineTo(4,0);ctx.closePath();ctx.fill();
    ctx.restore();
    ctx.shadowBlur=0;

    ctx.fillStyle="#00ffff";
    ctx.shadowBlur=10;ctx.shadowColor="#00ffff";
    bullets.forEach(b=>ctx.fillRect(b.x-1.5,b.y-10,3,12));
    ctx.shadowBlur=0;
    ctx.restore();

    ctx.textAlign="left";
    txt(`SYSTEM MONITOR S-0${lvl}`,20,35,`13px monospace`,"rgba(0,255,255,0.9)");
    txt(`TOTAL VAL SCORE : ${score}`,20,52,`13px monospace`,"rgba(0,255,255,0.9)");

    if(boss){
        txt(`ALERT: CAPITAL ANOMALY DETECTED`,20,72,`13px monospace`,"#ff0055");
    }else{
        txt(`HOSTILE REMNANTS: ${Math.max(0,tReq-tDest)}`,20,72,`13px monospace`,"rgba(255,160,0,0.9)");
        ctx.fillStyle="#112233";ctx.fillRect(20,85,150,5);
        ctx.fillStyle="#ffa500";ctx.fillRect(20,85,150*Math.min(1,tDest/tReq),5);
    }

    txt(`NEXT BOSS IN: ${Math.max(0,Math.ceil(60-(gTime-lBoss)))}s`,
        20,boss?92:105,`13px monospace`,"#88aaee");

    let hpP=Math.max(0,p.hp/p.maxHp);
    ctx.fillStyle="rgba(255,255,255,0.03)";ctx.fillRect(c.width-210,25,190,22);
    ctx.fillStyle=hpP>.35?"#00ffaa":"#ff2244";ctx.fillRect(c.width-210,25,190*hpP,22);
    ctx.strokeStyle="rgba(0,255,255,0.4)";ctx.lineWidth=1;ctx.strokeRect(c.width-210,25,190,22);
    ctx.textAlign="center";
    txt(`HULL MATRIX: ${Math.max(0,p.hp)}%`,c.width-115,39,`bold 11px monospace`,"#000000");

    ctx.textAlign="left";
    txt(paused?"[ PLAY ]":"[ PAUSE ]",20,c.height-20,`bold 14px monospace`,paused?"#00ffcc":"#ff5555");

    drawCamPreview();

    if(over){
        if((keys['r']||keys[' '])&&Date.now()-overAt>800){resetGame();menu=true;}
        menuOverlay("CRITICAL CORE FAILURE",`#ff2244`,`TOTAL SCORE: ${score}`,"SYSTEM OFFLINE - CORES DESTROYED","#ff2244","PRESS 'R' OR SPACE TO RETURN TO MENU");
    }else if(paused){
        menuOverlay("COMMAND INTERCEPT PAUSED",`#00f0ff`,`SECTOR LEVEL 0${lvl}`,`CURRENT SCORE: ${score}`,"#00f0ff","PRESS 'P', 'ESC' OR CLICK PAUSE TO RESUME");
    }
}

function loop(){update();draw();requestAnimationFrame(loop);}
window.addEventListener('resize',resize);
resize();spawn();loop();
</script>
</body>
</html>
