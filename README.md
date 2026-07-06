<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador definitivo de Sarius</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0b0e14;--sur:#161923;--sur2:#1e2230;--brd:#2d3241;--acc:#7c6af7;--gold:#f5c542;--red:#ff4d4d;--blue:#4d94ff;--magenta:#ff00ff;--green:#4caf7d;--txt:#e8eaf6;--txt2:#a0a6c0;--r:12px}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);padding:20px; line-height: 1.4}
        .wrap{max-width:1400px;margin:0 auto}
        
        .header{text-align:center;margin-bottom:25px}
        .header h1{font-size:2.2rem;font-weight:900;color:var(--gold);text-transform:uppercase;letter-spacing:3px;margin:0}
        .header p{color:var(--txt2);font-size:0.8rem;font-weight:600}

        .container{display:grid;grid-template-columns: 1fr 1fr;gap:20px}
        .full{grid-column:1/-1}
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:12px;padding:20px;box-shadow:0 8px 32px rgba(0,0,0,0.5)}
        
        h2{font-size:0.85rem;text-transform:uppercase;color:var(--acc);margin-bottom:15px;display:flex;align-items:center;gap:10px;font-weight:800}
        label{display:block;font-size:0.65rem;color:var(--txt2);margin-bottom:5px;text-transform:uppercase;font-weight:700}
        
        textarea, input, select{width:100%;background:#080a0f;border:1px solid var(--brd);border-radius:8px;color:#00ff88;padding:12px;font-family:'Courier New',monospace;font-size:0.8rem;outline:none}
        input, select{color:#fff; font-family:'Inter', sans-serif}

        /* GESTIÓN DE PUEBLOS */
        #v-manager-cont{display:none; margin-top:20px}
        #village-manager{margin-top:10px; max-height: 450px; overflow-y: auto; border: 1px solid var(--brd); border-radius: 8px; background: #080a0f; padding: 10px}
        .player-group{border: 1px solid var(--brd); border-radius: 8px; margin-bottom: 8px; overflow: hidden}
        .player-header-toggle{background: #1e2230; padding: 12px; display: flex; justify-content: space-between; cursor: pointer; align-items: center}
        .player-header-toggle:hover{background: #2d3241}
        .player-name{color:var(--gold); font-weight: 800; font-size: 0.95rem}
        
        .v-grid{display: none; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 10px; padding: 10px; background: #0b0e14}
        .v-grid.open{display: grid}
        .v-card{background: var(--sur); padding: 10px; border-radius: 6px; border: 1px solid var(--brd); display: flex; flex-direction: column; gap: 8px}
        .v-info{font-family: monospace; font-size: 0.75rem; color: #00ff88}
        .v-selector{display: flex; gap: 4px}
        .v-btn{flex:1; padding: 5px; border-radius: 4px; border: 1px solid var(--brd); background: var(--sur2); color: var(--txt2); font-size: 0.6rem; font-weight: 800; cursor: pointer; transition: 0.1s}
        .v-btn.active-off{background: var(--red); color: #fff; border-color: #ff0000}
        .v-btn.active-noble{background: var(--magenta); color: #fff; border-color: #ff00ff}
        .v-btn.active-front{background: #444; color: #fff; border-color: #aaa}

        /* OBJETIVOS */
        .obj-grid{display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px}
        .btn-main{width:100%;padding:18px;background:linear-gradient(135deg,var(--acc),var(--blue));border:none;border-radius:10px;color:#fff;font-weight:900;cursor:pointer;font-size:1.1rem;margin-top:15px;text-transform:uppercase;transition:0.3s}
        .btn-main:hover{transform:translateY(-2px);box-shadow:0 10px 30px rgba(124,106,247,0.4)}

        /* RESULTADOS */
        .res-table{width:100%;border-collapse:collapse;margin-top:20px}
        .res-table th{text-align:left;padding:12px;background:#11141d;color:var(--txt2);font-size:0.7rem;text-transform:uppercase}
        .res-table td{padding:12px 10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .player-head{background:rgba(124,106,247,0.15); color:var(--gold); font-weight:900}
        
        .pill{background:#000; border:1px solid #333; padding:4px 8px; border-radius:5px; font-size:0.75rem; font-weight:800; margin:1px; display:inline-flex; align-items:center; gap:3px; color: #fff}
        .tag{padding:2px 8px; border-radius:4px; font-size:0.65rem; font-weight:900; color: #fff}
        .tag-real{background:var(--red)}
        .tag-fake{background:var(--blue)}
        .tag-conq{background:var(--magenta)}

        .msg-grid{display:grid;grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:15px; margin-top:20px}
        .msg-card{background:var(--sur2); padding:15px; border-radius:10px; border:1px solid var(--brd); display:flex; justify-content:space-between; align-items:center; border-left:5px solid var(--gold)}
    </style>
</head>
<body>

<div class="wrap">
    <div class="header">
        <h1>PLANIFICADOR DEFINITIVO DE SARIUS</h1>
        <p>INTELIGENCIA TÁCTICA DE ALIANZA • v7.0</p>
    </div>

    <div class="container">
        <!-- 1. CARGA DE DATOS -->
        <div class="card full">
            <h2><div class="ico">💪</div> 1. CARGAR INTELIGENCIA (JSON DE TROPAS)</h2>
            <div style="display:grid; grid-template-columns: 2.5fr 1fr; gap:20px">
                <textarea id="in-troops" rows="5" placeholder="Pega aquí los JSON. El sistema limpiará pueblos vacíos y fusionará todos los jugadores."></textarea>
                <div style="display:flex; flex-direction:column; gap:8px">
                    <button class="btn-main" style="margin:0; padding:12px; background:var(--green)" onclick="loadTribeData()">Sincronizar Alianza</button>
                    <div id="load-status" style="font-size:0.75rem; color:var(--gold); font-weight:bold; text-align:center"></div>
                </div>
            </div>
            
            <div id="v-manager-cont">
                <div style="display:flex; justify-content:space-between; align-items:center; margin-top:20px">
                    <label>2. MANDO CENTRAL: BUSCA JUGADORES Y ASIGNA ROLES</label>
                    <input type="text" id="p-search" placeholder="🔎 Buscar compañero..." oninput="renderManager()" style="width:300px; padding:8px; background:#000; border-color:var(--acc2)">
                </div>
                <div id="village-manager"></div>
            </div>
        </div>

        <!-- 2. OBJETIVOS -->
        <div class="card full">
            <h2><div class="ico">🎯</div> 3. DEFINIR OBJETIVOS</h2>
            <div class="obj-grid">
                <div><label style="color:var(--magenta)">🏰 CONQUISTAS (Nobles)</label><textarea id="obj-conq" rows="6" placeholder="400|400"></textarea></div>
                <div><label style="color:var(--red)">🔥 REALES (Limpieza)</label><textarea id="obj-real" rows="6" placeholder="450|450"></textarea></div>
                <div><label style="color:var(--blue)">🎭 FAKES (Engaño)</label><textarea id="obj-fake" rows="6" placeholder="460|460"></textarea></div>
            </div>
        </div>

        <!-- 3. CONFIGURACIÓN -->
        <div class="card full">
            <h2><div class="ico">⚙️</div> 4. PARÁMETROS DE ENVÍO</h2>
            <div style="display:grid; grid-template-columns: 1fr 1.5fr 1.5fr 1fr; gap:15px; align-items:end">
                <div><label>⚡ Vel. Mundo</label><input type="number" id="w-speed" value="1.0" step="0.1"></div>
                <div><label>📅 Fecha Llegada</label><input type="date" id="date-arr"></div>
                <div><label>🕒 Hora Llegada</label><input type="time" id="time-arr" value="08:00:00" step="1"></div>
                <div>
                    <label>📏 Unidad Guía</label>
                    <select id="u-guide">
                        <option value="ram">Ariete (30m)</option>
                        <option value="snob">Noble (35m)</option>
                        <option value="axe">Hacha (18m)</option>
                    </select>
                </div>
            </div>
            <button class="btn-main" onclick="generatePlan()">🚀 GENERAR PLAN Y LIMPIAR OBJETIVOS</button>
        </div>

        <!-- RESULTADOS -->
        <div id="sec-msg" class="card full" style="display:none">
            <h2>📩 MENSAJERÍA INDIVIDUAL</h2>
            <div class="msg-grid" id="msg-grid"></div>
        </div>

        <div id="sec-res" class="card full" style="display:none">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px">
                <h2>📋 ÓRDENES DE ATAQUE</h2>
                <button class="btn bs" style="width:auto; margin:0; padding:10px 20px" onclick="copyForo()">📋 COPIAR TODO PARA EL FORO</button>
            </div>
            <div style="overflow-x:auto">
                <table class="res-table">
                    <thead>
                        <tr><th>Lanzamiento</th><th>Origen</th><th>Destino</th><th>Unidades</th><th>Tipo</th><th>🚀</th></tr>
                    </thead>
                    <tbody id="res-body"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<script>
    const SPEEDS = { spear:18, sword:22, axe:18, spy:9, light:10, marcher:10, heavy:11, ram:30, snob:35 };
    let db = [];

    function loadTribeData() {
        const raw = document.getElementById('in-troops').value.trim();
        if(!raw) return alert("Pega los datos.");
        
        try {
            // LECTOR DE FUERZA BRUTA: Extraemos cada bloque {...} individualmente
            const regex = /\{"player":.*?\}/g;
            const matches = raw.match(regex);
            
            if(!matches) {
                // Intento con formato corto "p"
                const regexShort = /\{"p":.*?\}/g;
                const matchesShort = raw.match(regexShort);
                if(!matchesShort) throw new Error("Formato no válido");
                var rawData = matchesShort.map(m => JSON.parse(m));
            } else {
                var rawData = matches.map(m => JSON.parse(m));
            }

            // Fusión y filtrado de pueblos ofensivos
            db = rawData.filter(v => {
                const off = (v.axe||0) + (v.light||0) + (v.ram||0) + (v.snob||0) + (v.marcher||0);
                return off > 0;
            }).map(v => ({
                player: v.player || v.p,
                x: parseInt(v.x), y: parseInt(v.y),
                ck: `${v.x}|${v.y}`,
                vid: v.vid,
                axe: v.axe || 0, ram: v.ram || 0, snob: v.snob || 0, light: v.light || 0, marcher: v.marcher||0,
                role: (v.ram > 150) ? 'off' : (v.snob > 0 ? 'noble' : 'none')
            }));

            // Eliminar duplicados
            db = Array.from(new Map(db.map(item => [item.ck, item])).values());

            renderManager();
            document.getElementById('v-manager-cont').style.display = "block";
            document.getElementById('load-status').innerText = `✅ SINCRONIZADOS: ${db.length} PUEBLOS.`;
        } catch(e) { 
            alert("Error al procesar las tablas. Asegúrate de copiar los datos completos."); 
        }
    }

    function renderManager() {
        const container = document.getElementById('village-manager');
        const search = document.getElementById('p-search').value.toLowerCase();
        container.innerHTML = "";
        
        const players = [...new Set(db.map(x => x.player))].sort();

        players.forEach(pnm => {
            if(search && !pnm.toLowerCase().includes(search)) return;

            let pDiv = document.createElement('div');
            pDiv.className = "player-group";
            const pVills = db.filter(v => v.player === pnm);
            
            pDiv.innerHTML = `
                <div class="player-header-toggle" onclick="this.nextElementSibling.classList.toggle('open')">
                    <span class="player-name">${pnm}</span>
                    <span style="font-size:0.7rem; color:var(--acc2)">${pVills.length} pueblos ofensivos ▾</span>
                </div>
                <div class="v-grid"></div>
            `;
            container.appendChild(pDiv);
            
            const grid = pDiv.querySelector('.v-grid');
            if(search) grid.classList.add('open');

            pVills.forEach(v => {
                let vCard = document.createElement('div');
                vCard.className = "v-card";
                vCard.innerHTML = `
                    <div class="v-info"><b>(${v.x}|${v.y})</b> 🪓${v.axe} 🐴${v.light} 🔨${v.ram} 📜${v.snob}</div>
                    <div class="v-selector">
                        <div class="v-btn ${v.role==='off'?'active-off':''}" onclick="setRole('${v.ck}','off')">⚔️ OFF</div>
                        <div class="v-btn ${v.role==='noble'?'active-noble':''}" onclick="setRole('${v.ck}','noble')">📜 NOBLE</div>
                        <div class="v-btn ${v.role==='front'?'active-front':''}" onclick="setRole('${v.ck}','front')">🛡️ FRONT</div>
                    </div>
                `;
                grid.appendChild(vCard);
            });
        });
    }

    function setRole(ck, role) {
        const v = db.find(x => x.ck === ck);
        v.role = (v.role === role) ? 'none' : role;
        renderManager();
    }

    function parseText(id) {
        const txt = document.getElementById(id).value;
        const list = [];
        txt.split('\n').forEach(l => {
            const m = l.match(/(\d{1,3})\|(\d{1,3})/);
            if(m) list.push({x:parseInt(m[1]), y:parseInt(m[2]), ck:m[0]});
        });
        return list;
    }

    function generatePlan() {
        const cT = parseText('obj-conq');
        const rT = parseText('obj-real');
        const fT = parseText('obj-fake');
        const vM = parseFloat(document.getElementById('w-speed').value) || 1;
        const arrMs = new Date(document.getElementById('date-arr').value + 'T' + document.getElementById('time-arr').value).getTime();
        const uG = document.getElementById('u-guide').value;

        if(isNaN(arrMs)) return alert("Fecha u hora incorrecta.");

        let results = [];
        let usedAtks = new Set();
        let success = { conq: [], real: [], fake: [] };

        // 1. CONQUISTAS
        let noblePool = db.filter(v => v.role === 'noble');
        cT.forEach(t => {
            if(!noblePool.length) return;
            noblePool.sort((a,b) => dist(a,t) - dist(b,t));
            const w = noblePool.shift();
            usedAtks.add(w.ck);
            results.push(createOrder(w, t, 'CONQUISTA', arrMs, vM, 'snob'));
            success.conq.push(t.ck);
        });

        // 2. REALES
        let offPool = db.filter(v => v.role === 'off' && !usedAtks.has(v.ck));
        rT.forEach(t => {
            if(!offPool.length) return;
            offPool.sort((a,b) => dist(a,t) - dist(b,t));
            const w = offPool.shift();
            usedAtks.add(w.ck);
            results.push(createOrder(w, t, 'REAL', arrMs, vM, uG));
            success.real.push(t.ck);
        });

        // 3. FAKES
        let fakePool = db.filter(v => v.role !== 'front' && !usedAtks.has(v.ck));
        if(!fakePool.length) fakePool = db;
        fT.forEach((t, i) => {
            const w = fakePool[i % fakePool.length];
            results.push(createOrder(w, t, 'FAKE', arrMs, vM, uG));
            success.fake.push(t.ck);
        });

        // LIMPIEZA DE LISTAS
        updateBoxes(success);
        renderFinal(results);
    }

    function updateBoxes(sc) {
        const boxes = { conq: 'obj-conq', real: 'obj-real', fake: 'obj-fake' };
        for(let k in boxes) {
            let ta = document.getElementById(boxes[k]);
            let lines = ta.value.split('\n');
            sc[k].forEach(c => lines = lines.filter(l => !l.includes(c)));
            ta.value = lines.join('\n').trim();
        }
    }

    function dist(a,b) { return Math.sqrt(Math.pow(a.x-b.x,2)+Math.pow(a.y-b.y,2)); }

    function createOrder(v, t, type, arrMs, vM, uG) {
        const d = dist(v, t);
        const travelMs = (d * SPEEDS[uG] / vM) * 60000;
        const launch = new Date(arrMs - travelMs);
        let trps = (type === 'REAL') ? `&axe=${v.axe}&light=${v.light}&ram=${v.ram}` : (type==='CONQUISTA'?'&snob=1&axe=2000&light=1000':'&spy=1&ram=1');
        return {
            p: v.player, orig: v.ck, dest: t.ck, type,
            launch: launch.toLocaleString(),
            url: `https://es100.guerrastribales.es/game.php?village=${v.vid}&screen=place&target=${t.x}${t.y}${trps}`,
            visual: type === 'REAL' ? `🪓${v.axe} 🐴${v.light} 🔨${v.ram}` : (type==='CONQUISTA'?'📜 NOBLE':'1🕵️1🔨')
        };
    }

    function renderFinal(res) {
        const body = document.getElementById('res-body');
        const grid = document.getElementById('msg-grid');
        body.innerHTML = ""; grid.innerHTML = "";
        let grouped = {};
        res.forEach(r => { if(!grouped[r.p]) grouped[r.p] = []; grouped[r.p].push(r); });

        for(let p in grouped) {
            body.innerHTML += `<tr class="player-head"><td colspan="6">👤 JUGADOR: ${p}</td></tr>`;
            grouped[p].forEach(r => {
                const cls = r.type==='CONQUISTA'?'tag-conq':(r.type==='REAL'?'tag-real':'tag-fake');
                body.innerHTML += `<tr><td>${r.launch}</td><td>${r.orig}</td><td>→</td><td><b>${r.dest}</b></td><td><span class="pill">${r.visual}</span></td><td><span class="tag ${cls}">${r.type}</span></td><td><button class="v-btn" style="background:var(--acc);color:#fff" onclick="window.open('${r.url}')">🚀</button></td></tr>`;
            });
            grid.innerHTML += `<div class="msg-card"><span><b>${p}</b> (${grouped[p].length})</span><button class="btn bm" onclick="copyMP('${p}')">📩 COPIAR MP</button></div>`;
        }
        document.getElementById('sec-msg').style.display = "block";
        document.getElementById('sec-res').style.display = "block";
        window.currentPlan = grouped;
    }

    function copyMP(p) {
        const orders = window.currentPlan[p];
        let bb = `Hola [player]${p}[/player],\n\n[table]\n[**]Tipo[||]Lanzar[||]Origen[||]Destino[||]Tropas[/**]\n`;
        orders.forEach(o => bb += `[*]${o.type}[|]${o.launch}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${o.visual}\n`);
        bb += `[/table]`;
        navigator.clipboard.writeText(bb);
        alert("Copiado.");
    }

    function copyForo() {
        let bb = "[b]📅 PLANIFICACIÓN ESTRATÉGICA[/b]\n\n";
        for(let p in window.currentPlan) {
            bb += `[player]${p}[/player]\n[spoiler=Órdenes][table]\n[**]Tipo[||]Lanzar[||]Origen[||]Destino[/**]\n`;
            window.currentPlan[p].forEach(o => bb += `[*]${o.type}[|]${o.launch}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord]\n`);
            bb += `[/table][/spoiler]\n\n`;
        }
        navigator.clipboard.writeText(bb);
        alert("Copiado Foro.");
    }

    window.onload = () => { document.getElementById('date-arr').value = new Date().toISOString().split('T')[0]; };
</script>
</body>
</html>
