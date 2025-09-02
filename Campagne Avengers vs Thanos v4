<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Avengers Battle — Thanos (Option 2)</title>
<style>
:root{
  --bg:#0f0f12; --panel:#121216; --muted:#9aa0a6; --accent:#6ef08a; --fg:#eef3ea;
  --hit:#ff4d4d; --crit:#ffd24d; --heal:#4dff88;
}
*{box-sizing:border-box}
body{ margin:0; font-family:Inter, Roboto, "Helvetica Neue", Arial; background:var(--bg); color:var(--fg); }
header{ padding:18px 12px; text-align:center; }
main{ max-width:760px; margin:0 auto; padding:12px; position:relative; }
.panel{ background:var(--panel); border-radius:12px; padding:14px; margin:10px 0; box-shadow:0 8px 28px rgba(0,0,0,0.6); position:relative; overflow:hidden;}
h1{ margin:0; font-size:20px; }
.muted{ color:var(--muted); font-size:13px; margin-top:6px; }
.controls{ display:flex; gap:12px; flex-wrap:wrap; margin-bottom:10px; }
.card{ background:#0d0d0f; border:1px solid #222; padding:10px; border-radius:8px; min-width:160px; }
#stats{ display:flex; gap:10px; flex-wrap:wrap; margin-bottom:8px; }
.stat{ background:#0d0d0f; border:1px solid #232323; padding:8px 10px; border-radius:8px; font-weight:700; color:var(--fg); min-width:140px; text-align:center; }
.bar-container{ background:#222; border-radius:8px; width:100%; height:14px; margin-top:6px; overflow:hidden; }
.bar{ height:100%; border-radius:8px; transition: width 0.4s ease; }
#story{ white-space:pre-wrap; font-size:15px; line-height:1.4; margin-bottom:8px; }
#log{ color:var(--muted); font-size:13px; margin-top:8px; min-height:64px; white-space:pre-wrap; }
#choices{ display:grid; gap:8px; margin-top:12px; }
button.choice{ background:#141416; color:var(--fg); border:1px solid #2a2a2a; padding:10px 12px; border-radius:8px; text-align:center; cursor:pointer; font-weight:700; }
button.choice:hover{ border-color:var(--accent); color:var(--accent); transform:translateY(-2px); }
.avatar-row{ display:flex; justify-content:space-between; align-items:center; gap:12px; margin-bottom:12px; }
.avatar{ width:110px; height:110px; border-radius:12px; background:#141416; border:2px solid #333; display:flex; align-items:center; justify-content:center; text-align:center; padding:8px; font-weight:800; }
.small{ font-size:13px; color:var(--muted); }
.projectile{ width:14px; height:14px; border-radius:50%; position:absolute; z-index:120; pointer-events:none; animation:proj 0.45s linear forwards; }
@keyframes proj{ 0%{transform:translate(0,0) scale(1);} 100%{transform:translate(250px,-18px) scale(0.9); opacity:1;} }
.shake{ animation:shake 0.35s; }
@keyframes shake{ 0%{transform:translate(0,0);} 25%{transform:translate(6px,0);} 50%{transform:translate(-6px,0);} 75%{transform:translate(6px,0);} 100%{transform:translate(0,0);} }
.floating-dmg{ position:absolute; font-weight:900; pointer-events:none; transition: all 0.9s ease-out; font-size:15px; text-shadow:0 2px 8px rgba(0,0,0,0.5); }
.footer-note{ text-align:center; color:var(--muted); margin-top:6px; font-size:13px; }

/* responsive */
@media (max-width:560px){
  .avatar{ width:88px; height:88px; font-size:13px; }
  .stat{ min-width:110px; font-size:13px; }
  main{ padding:8px; }
}
</style>
</head>
<body>
<header>
  <h1>Avengers Battle — Le Duel Final</h1>
  <div class="muted">Option 2 — Choisis ton héros, ton pouvoir (usage unique) et ton arme, puis affronte Thanos.</div>
</header>
<main>
  <section class="panel">
    <div class="controls">
      <div class="card" style="flex:1">
        <div style="font-weight:900">1) Choisir un Avenger</div>
        <div id="hero-list" class="small" style="margin-top:8px"></div>
      </div>
      <div class="card" style="flex:1">
        <div style="font-weight:900">2) Choisir le pouvoir (usage unique)</div>
        <div id="power-list" class="small" style="margin-top:8px"></div>
      </div>
      <div class="card" style="flex:1">
        <div style="font-weight:900">3) Choisir une arme</div>
        <div id="weapon-list" class="small" style="margin-top:8px"></div>
      </div>
    </div>

    <div class="avatar-row">
      <div>
        <div class="avatar" id="player-avatar">Héros</div>
        <div class="small" style="text-align:center; margin-top:6px" id="player-info"></div>
      </div>
      <div style="flex:1; margin-left:12px; margin-right:12px">
        <div id="stats" aria-live="polite"></div>
        <div id="story" class="small"></div>
      </div>
      <div>
        <div class="avatar" id="enemy-avatar">Thanos</div>
        <div class="small" style="text-align:center; margin-top:6px">Boss final</div>
      </div>
    </div>

    <div id="choices"></div>
    <div id="log" style="margin-top:10px"></div>
  </section>

  <div style="text-align:center; margin-top:10px">
    <button id="restart" class="choice" style="max-width:260px">🔁 Recommencer</button>
  </div>

  <div class="footer-note">3 attaques par round → Thanos riposte ensuite. Critique 20% (x2). Lourde 20% d'échec.</div>
</main>

<script>
/* ---------- Données & Config ---------- */
const HEROES = ["Iron Man","Thor","Black Widow","Captain America","Hulk","Doctor Strange"];
const POWERS = {
  "Iron Man":["Répulseurs","Surcharge d'arc"],
  "Thor":["Mjolnir","Éclair divin"],
  "Black Widow":["Arts martiaux","Infiltration"],
  "Captain America":["Bouclier ricochet","Charge tactique"],
  "Hulk":["Poing destructeur","Rage inarrêtable"],
  "Doctor Strange":["Portail dimensionnel","Sort mystique"]
};
const UNIQUE_POWERS = ["Éclair divin","Infiltration","Portail dimensionnel"];
const WEAPONS = ["Grenade EMP","Bouclier énergétique","Arc plasma","Lance-filet","Épée asgardienne","Neuro-stun"];

/* enemy config */
const THANO_START_PV = 50;
const PLAYER_START_PV = 30;
const MAX_ROUNDS = 5; // number of Thanos ripostes allowed

/* ---------- State ---------- */
let state = {
  player: { hero:null, power:null, weapon:null, pv:PLAYER_START_PV, maxPv:PLAYER_START_PV, usedUniquePowers:{} },
  enemy:  { name:"Thanos", pv:THANO_START_PV, maxPv:THANO_START_PV, rounds:MAX_ROUNDS },
  round: 1,
  attackCount: 0, // attacks performed in current round (0..2)
  log: []
};

/* ---------- Utils ---------- */
const $ = s => document.querySelector(s);
function el(tag, attrs={}, txt=""){ const e=document.createElement(tag); for(const k in attrs) e.setAttribute(k, attrs[k]); e.textContent = txt; return e; }
function rnd(min,max){ return Math.floor(Math.random()*(max-min+1))+min; }
function clamp(v,a,b){ return Math.max(a, Math.min(b,v)); }

/* ---------- UI builders ---------- */
function buildLists(){
  // heroes
  const heroList = $("#hero-list"); heroList.innerHTML = "";
  HEROES.forEach(h => {
    const b = el("button", {class:"choice"}, h);
    b.onclick = () => chooseHero(h);
    heroList.appendChild(b);
  });
  // weapons (initially empty until hero chosen)
  $("#weapon-list").innerHTML = "<div class='small'>Choisis un héros</div>";
  $("#power-list").innerHTML = "<div class='small'>Choisis un héros</div>";
}

function renderStatus(){
  const p = state.player, e = state.enemy;
  const stats = $("#stats"); stats.innerHTML = "";
  stats.appendChild(el("div",{class:"stat"}, `Tes PV: ${p.pv}/${p.maxPv}`));
  stats.appendChild(bar("player-bar", p.pv, p.maxPv, "#6ef08a"));
  stats.appendChild(el("div",{class:"stat"}, `${e.name} PV: ${e.pv}/${e.maxPv}`));
  stats.appendChild(bar("enemy-bar", e.pv, e.maxPv, "#ff4d4d"));
  $("#log").innerHTML = state.log.join("\n");
  $("#player-avatar").textContent = p.hero || "Héros";
  $("#player-info").textContent = `${p.power ? p.power + " — " : ""}${p.weapon ? p.weapon : ""}`;
}

function bar(id, current, max, color){
  const cont = document.createElement("div"); cont.className = "bar-container";
  const b = document.createElement("div"); b.className = "bar"; b.id = id;
  const pct = max>0 ? (current/max)*100 : 0;
  b.style.width = `${pct}%`; b.style.background = color;
  cont.appendChild(b);
  return cont;
}

function pushLog(line){
  state.log.unshift(line);
  state.log = state.log.slice(0, 8);
  renderStatus();
}

/* ---------- Selection flow ---------- */
function start(){
  state.player = { hero:null, power:null, weapon:null, pv:PLAYER_START_PV, maxPv:PLAYER_START_PV, usedUniquePowers:{} };
  state.enemy = { name:"Thanos", pv:THANO_START_PV, maxPv:THANO_START_PV, rounds:MAX_ROUNDS };
  state.round = 1; state.attackCount = 0; state.log = [];
  $("#story").textContent = "Choisis ton Avenger, puis pouvoir et arme.";
  buildLists();
  setChoices([]);
  renderStatus();
}

/* choose hero -> show powers */
function chooseHero(h){
  state.player.hero = h;
  // show powers for this hero
  const powerList = $("#power-list"); powerList.innerHTML = "";
  POWERS[h].forEach(p => {
    const b = el("button",{class:"choice"}, p);
    b.onclick = () => choosePower(p);
    powerList.appendChild(b);
  });
  // show weapons
  const weaponList = $("#weapon-list"); weaponList.innerHTML = "";
  WEAPONS.forEach(w => {
    const b = el("button",{class:"choice"}, w);
    b.onclick = () => chooseWeapon(w);
    weaponList.appendChild(b);
  });
  pushLog(`Héros choisi : ${h}`);
  renderStatus();
}

/* choose power */
function choosePower(pw){
  state.player.power = pw;
  pushLog(`Pouvoir choisi : ${pw} (usage unique)`);
  renderStatus();
}

/* choose weapon -> ready to start combat */
function chooseWeapon(w){
  state.player.weapon = w;
  pushLog(`Arme choisie : ${w}`);
  renderStatus();
  // show start button
  setChoices([{ label:"⚔️ Commencer le combat", onClick: beginCombat }]);
}

/* set choice buttons area */
function setChoices(list){
  const c = $("#choices"); c.innerHTML = "";
  list.forEach(opt => {
    const b = el("button", {class:"choice"}, opt.label);
    b.onclick = opt.onClick;
    c.appendChild(b);
  });
}

/* ---------- Combat flow (3 attacks per round) ---------- */
function beginCombat(){
  if(!state.player.hero){ pushLog("Choisis d'abord un héros."); return; }
  if(!state.player.power){ pushLog("Choisis d'abord un pouvoir."); return; }
  if(!state.player.weapon){ pushLog("Choisis d'abord une arme."); return; }
  state.round = 1; state.attackCount = 0; state.log = [];
  pushLog("⚔️ Le combat commence ! Arrête Thanos avant qu'il efface 50% de l'humanité !");
  renderStatus();
  playerAttackPrompt();
}

/* Present attack options for player's next attack */
function playerAttackPrompt(){
  if(checkEarlyEnd()) return;
  $("#story").textContent = `Round ${state.round}/${state.enemy.rounds} — Attaque ${state.attackCount+1}/3`;
  setChoices([
    { label:"💨 Attaque rapide (3-5 PV)", onClick: ()=>playerAttack("rapide") },
    { label:"💥 Attaque lourde (5-9 PV, 20% d'échec)", onClick: ()=>playerAttack("lourde") },
    { label:`✨ Pouvoir (${state.player.power}) (usage unique)`, onClick: ()=>playerAttack("pouvoir") },
    { label:`⚔️ Utiliser arme (${state.player.weapon}) (4-7 PV)`, onClick: ()=>playerAttack("arme") }
  ]);
}

/* Player attack with animation projectile -> apply damage after animation */
function playerAttack(type){
  // protect if ended
  if(checkEarlyEnd()) return;

  // create projectile element from player avatar towards enemy avatar
  const panel = document.querySelector(".panel");
  const proj = document.createElement("div");
  proj.className = "projectile";
  // color by attack type
  if(type==="pouvoir") proj.style.background = "#9b7dff";
  else if(type==="lourde") proj.style.background = "#ff9a66";
  else if(type==="arme") proj.style.background = "#a6e1ff";
  else proj.style.background = "var(--accent)";
  // place near player avatar
  const playerAvatar = $("#player-avatar");
  const startRect = playerAvatar.getBoundingClientRect();
  const panelRect = panel.getBoundingClientRect();
  // position projectile at player's avatar center relative to panel
  proj.style.left = (startRect.left - panelRect.left + startRect.width/2) + "px";
  proj.style.top  = (startRect.top - panelRect.top + startRect.height/2) + "px";
  panel.appendChild(proj);

  proj.addEventListener("animationend", ()=> {
    // small delay for impact feel
    setTimeout(()=> {
      proj.remove();
      applyPlayerDamage(type);
    }, 60);
  });
}

/* Apply player's damage, show floating numbers, update attackCount and decide next step */
function applyPlayerDamage(type){
  const p = state.player;
  let dmg = 0;
  let miss = false;
  let crit = false;

  // base damage by type
  if(type === "rapide"){ dmg = rnd(3,5); }
  else if(type === "lourde"){ dmg = rnd(5,9); if(Math.random() < 0.20){ dmg = 0; miss = true; } }
  else if(type === "pouvoir"){
    if(UNIQUE_POWERS.includes(p.power)){
      if(p.usedUniquePowers[p.power]){ pushLog("⚠️ Pouvoir déjà utilisé ce combat ! Attaque rapide à la place."); dmg = rnd(3,5); }
      else { dmg = rnd(8,12); p.usedUniquePowers[p.power] = true; pushLog(`💥 ${p.power} activé ! (usage unique)`); }
    } else { dmg = rnd(6,10); }
  }
  else if(type === "arme"){ 
    // slight weapon flavor (some weapons could be stronger)
    dmg = rnd(4,7);
    if(p.weapon === "Épée asgardienne"){ dmg += 1; } // small bonus
    if(p.weapon === "Grenade EMP" && state.enemy.name === "Thanos") { /* no special vs Thanos */ }
  }

  // critical chance (20%) on non-miss and non-zero
  if(!miss && dmg > 0 && Math.random() < 0.20){ crit = true; dmg = dmg * 2; }

  // apply damage
  state.enemy.pv = Math.max(0, state.enemy.pv - dmg);

  // flash bar & shake enemy
  flashBar("enemy-bar");
  const enemyAvatar = $("#enemy-avatar");
  enemyAvatar.classList.add("shake");
  setTimeout(()=>enemyAvatar.classList.remove("shake"), 300);

  // floating damage (colored: crit yellow, normal red)
  if(!miss && dmg > 0){
    floatingDamage($("#enemy-avatar"), `-${dmg}`, crit ? "yellow" : "red");
  } else if(miss){
    floatingDamage($("#enemy-avatar"), "Miss", "gray");
  }

  // log
  if(miss) pushLog(`❌ Ta ${type} a raté !`);
  else pushLog(`${crit ? "✨ Critique ! " : ""}Tu infliges ${dmg} PV à Thanos (${type}).`);

  // increment attack count
  state.attackCount = (state.attackCount || 0) + 1;

  renderStatus();

  // if Thanos dead: immediate end
  if(state.enemy.pv <= 0){
    setTimeout(()=> { pushLog("🎉 Thanos est vaincu ! Tu as sauvé l'humanité."); endGame(true); }, 300);
    return;
  }

  // if reached 3 attacks -> Thanos riposte
  if(state.attackCount >= 3){
    state.attackCount = 0;
    setTimeout(enemyTurn, 600);
  } else {
    // allow next player attack after short delay
    setTimeout(playerAttackPrompt, 350);
  }
}

/* Floating damage helper: element, text, colorName ("red","yellow","green","gray") */
function floatingDamage(targetEl, txt, colorName){
  const panel = document.querySelector("main");
  const rect = targetEl.getBoundingClientRect();
  const bodyRect = document.body.getBoundingClientRect();
  const f = document.createElement("div");
  f.className = "floating-dmg";
  f.textContent = txt;
  // position relative to viewport then animate
  f.style.left = (rect.left + rect.width/2) + "px";
  f.style.top  = (rect.top - 18) + "px";
  if(colorName === "yellow") f.style.color = "var(--crit)";
  else if(colorName === "green") f.style.color = "var(--heal)";
  else if(colorName === "gray") f.style.color = "var(--muted)";
  else f.style.color = "var(--hit)";

  document.body.appendChild(f);
  // trigger move up and fade
  setTimeout(()=> {
    f.style.transform = "translateY(-42px)";
    f.style.opacity = 0;
  }, 30);
  setTimeout(()=> f.remove(), 900);
}

/* Enemy (Thanos) turn: single riposte after player's 3 attacks */
function enemyTurn(){
  if(state.enemy.pv <= 0){ endGame(true); return; }

  // Thanos special behavior: can increase damage slightly as rounds progress
  const baseMin = 4, baseMax = 8;
  // ramp: +0 to +round/2
  const ramp = Math.floor(state.round / 2);
  const dmg = rnd(baseMin + ramp, baseMax + ramp);

  // apply to player
  state.player.pv = Math.max(0, state.player.pv - dmg);
  flashBar("player-bar");

  // floating damage on player
  floatingDamage($("#player-avatar"), `-${dmg}`, "red");

  // shake player
  const playerAvatar = $("#player-avatar");
  playerAvatar.classList.add("shake");
  setTimeout(()=>playerAvatar.classList.remove("shake"), 300);

  pushLog(`⚔️ Thanos riposte et inflige ${dmg} PV à ${state.player.hero || "toi"}.`);

  // advance a round only after Thanos's riposte
  state.round++;

  renderStatus();

  // check end conditions
  if(state.player.pv <= 0){
    setTimeout(()=> { pushLog("💀 Tu as été vaincu... Thanos efface 50% de l'humanité."); endGame(false); }, 200);
    return;
  }
  if(state.round > state.enemy.rounds){
    // Thanos had his max number of ripostes → mission fail (or escape)
    setTimeout(()=> { pushLog("⏳ Thanos a atteint une étape critique... l'humanité est en danger."); endGame(false); }, 200);
    return;
  }

  // otherwise continue next player's attacks
  setTimeout(playerAttackPrompt, 400);
}

/* check if game already ended */
function checkEarlyEnd(){
  if(state.enemy.pv <= 0 || state.player.pv <= 0 || state.round > state.enemy.rounds){
    // disable choices
    setChoices([]);
    return true;
  }
  return false;
}

/* end game: success boolean */
function endGame(success){
  if(success){
    $("#story").textContent = "🏁 Victoire ! Thanos est vaincu — l'humanité est sauvée.";
    pushLog("🏆 Tu as gagné !");
  } else {
    $("#story").textContent = "☠️ Défaite... Thanos a presque atteint son objectif.";
    pushLog("❌ Partie terminée.");
  }
  setChoices([{ label:"🔁 Rejouer", onClick: start }]);
}

/* helper to flash bars */
function flashBar(id){
  const b = $("#"+id);
  if(!b) return;
  b.classList.add("hit");
  setTimeout(()=>b.classList.remove("hit"), 300);
}

/* bind restart */
$("#restart").addEventListener("click", start);

/* boot */
start();

</script>
</body>
</html>
