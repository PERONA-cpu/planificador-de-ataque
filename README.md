<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador Definitivo de Sarius</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800;900&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0b0e14;--sur:#161923;--sur2:#23263a;--brd:#2d3241;--acc:#7c6af7;--gold:#f5c542;--red:#ff4d4d;--blue:#4d94ff;--green:#4caf7d;--txt:#e8eaf6;--txt2:#a0a6c0}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);padding:20px}
        
        /* TÍTULO SARIUS */
        .header{text-align:center;margin-bottom:30px}
        .header h1{font-size:2.5rem;font-weight:900;color:var(--gold);text-transform:uppercase;letter-spacing:4px; text-shadow: 0 0 15px rgba(245,197,66,0.2)}
        
        .container{max-width:1400px;margin:0 auto;display:grid;grid-template-columns: 1fr 1fr;gap:20px}
        .full{grid-column:1/-1}
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:12px;padding:20px;height:100%}
        h2{font-size:0.9rem;text-transform:uppercase;color:var(--acc);margin-bottom:15px;display:flex;align-items:center;gap:8px}
        textarea{width:100%;background:#0b0e14;border:1px solid var(--brd);border-radius:8px;color:#00ff00;padding:12px;font-family:monospace;font-size:0.8rem;resize:vertical;outline:none}
        
        .config-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:15px;margin-bottom:15px}
        .input-group{display:flex;flex-direction:column;gap:5px}
        label{font-size:0.7rem;font-weight:bold;color:var(--txt2)}
        input, select{background:#0b0e14;border:1px solid var(--brd);padding:8px;color:#fff;border-radius:6px;outline:none}

        /* COMPOSICIÓN */
        .comp-box{background:rgba(0,0,0,0.3);padding:15px;border-radius:10px;border:1px dashed var(--brd);margin-top:10px}
        .unit-inputs{display:grid;grid-template-columns:repeat(auto-fill,minmax(90px,1fr));gap:10px}
        .u-in{display:flex;align-items:center;gap:5px;background:var(--sur);padding:5px;border-radius:5px;border:1px solid var(--brd)}
        .u-in input{width:100%;border:none;background:transparent;text-align:center}

        .btn-main{width:100%;padding:18px;background:linear-gradient(135deg,var(--acc),var(--blue));border:none;border-radius:10px;color:#fff;font-weight:900;cursor:pointer;font-size:1.1rem;margin-top:10px;text-transform:uppercase;letter-spacing:1px}
        .btn-main:hover{transform:translateY(-2px);box-shadow:0 5px 20px rgba(124,106,247,0.5)}

        /* TABLA */
        .res-table{width:100%;border-collapse:collapse;margin-top:20px}
        .res-table th{background:var(--brd);padding:12px;font-size:0.7rem;text-align:left;color:var(--txt2)}
        .res-table td{padding:12px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .tr-tag{display:inline-block;padding:2px 8px;border-radius:4px;font-size:0.65rem;font-weight:900;margin-right:5px}
        .tag-conq{background:#ff00ff;color:#fff}
        .tag-real{background:var(--red);color:#fff}
        .tag-fake{background:var(--blue);color:#fff}
        .player-header{background:rgba(124,106,247,0.1);font-weight:900;color:var(--gold)}
        .pill{background:#000;border:1px solid #333;padding:2px 6px;border-radius:4px;font-size:0.75rem;margin:1px;display:inline-block;font-weight:bold}
        
        .msg-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(300px,1fr));gap:12px;margin-top:20px}
        .msg-card{background:var(--sur);border:1px solid var(--brd);padding:15px;border-radius:10px;display:flex;justify-content:space-between;align-items:center;border-left:4px solid var(--gold)}
    </style>
</head>
<body>

<div class="wrap">
    <div class="header">
        <h1>PLANIFICADOR DEFINITIVO DE SARIUS</h1>
    </div>

    <div class="container">
        <!-- BLOQUE ATACANTES -->
        <div class="card">
            <h2>💪 DATOS DE LA TRIBU (OFFs)</h2>
            <textarea id="in-alliance" rows="10" placeholder="Pega el JSON del extractor de tropas aquí..."></textarea>
            <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;margin-top:10px">
                <div class="input-group"><label>🪓 Min Hachas</label><input type="number" id="min-axe" value="4000"></div>
                <div class="input-group"><label>🔨 Min Arietes</label><input type="number" id="min-ram" value="250"></div>
                <div class="input-group"><label>🛡️ Seguridad (Campos)</label><input type="number" id="min-dist" value="0" title="Excluir atacantes a menos de X campos del enemigo"></div>
            </div>
            <p style="font-size:0.65rem; color:var(--txt2); margin-top:5px">Tip: Seguridad evita usar pueblos pegados al frente para la ofensiva.</p>
        </div>

        <!-- BLOQUE NOBLES -->
        <div class="card">
            <h2>📜 PUEBLOS CON NOBLES</h2>
            <textarea id="in-nobles" rows="10" placeholder="Pega los pueblos que tienen nobles (JSON o Coordenadas)..."></textarea>
            <p style="font-size:0.65rem; color:var(--txt2); margin-top:5px">Estos pueblos se usarán solo para los Objetivos de Conquista.</p>
        </div>

        <!-- BLOQUE OBJETIVOS -->
        <div class="card full">
            <h2>🎯 OBJETIVOS DE LA OPERACIÓN</h2>
            <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:15px">
                <div>
                    <label style="color:#ff00ff">🏰 OBJETIVOS DE CONQUISTA</label>
                    <textarea id="in-conq" rows="6" placeholder="Enemigo 450|450"></textarea>
                </div>
                <div>
                    <label style="color:var(--red)">🔥 OBJETIVOS REALES (OFF)</label>
                    <textarea id="in-real" rows="6" placeholder="Enemigo 500|500"></textarea>
                </div>
                <div>
                    <label style="color:var(--blue)">🎭 OBJETIVOS FAKES</label>
                    <textarea id="in-fake" rows="6" placeholder="Enemigo 501|501"></textarea>
                </div>
            </div>
        </div>

        <!-- CONFIGURACIÓN DE ENVÍO -->
        <div class="card full">
            <h2>🛡️ CONFIGURACIÓN DE ENVÍO</h2>
            <div class="config-grid">
                <div class="input-group"><label>⚡ Vel. Mundo</label><input type="number" id="world-speed" value="1.0" step="0.1"></div>
                <div class="input-group"><label>📅 Fecha Llegada</label><input type="date" id="date-arr"></div>
                <div class="input-group"><label>🕒 Hora Llegada</label><input type="time" id="time-arr" value="08:00:00" step="1"></div>
                <div class="input-group">
                    <label>📏 Unidad Guía</label>
                    <select id="unit-guide">
                        <option value="ram">🔨 Ariete (30m)</option>
                        <option value="snob">📜 Noble (35m)</option>
                        <option value="spy">🕵️ Espía (9m)</option>
                        <option value="axe">🪓 Hacha (18m)</option>
                    </select>
                </div>
            </div>

            <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px">
                <div class="comp-box">
                    <label style="color:var(--red)">Composición Ofensiva:</label>
                    <select id="real-mode" onchange="toggleRealCustom()">
                        <option value="all">Mandar TODO lo ofensivo del pueblo</option>
                        <option value="custom">Mandar cantidad específica ↓</option>
                    </select>
                    <div id="real-custom" class="unit-inputs" style="margin-top:10px; display:none">
                        <div class="u-in">🪓<input type="number" id="rc-axe" value="0"></div>
                        <div class="u-in">🐴<input type="number" id="rc-light" value="0"></div>
                        <div class="u-in">🔨<input type="number" id="rc-ram" value="0"></div>
                    </div>
                </div>
                <div class="comp-box">
                    <label style="color:var(--blue)">Composición Fake:</label>
                    <div class="unit-inputs" style="margin-top:10px">
                        <div class="u-in">🪓<input type="number" id="fc-axe" value="0"></div>
                        <div class="u-in">🕵️<input type="number" id="fc-spy" value="1"></div>
                        <div class="u-in">🔨<input type="number" id="fc-ram" value="1"></div>
                    </div>
                </div>
            </div>
            
            <button class="btn-main" onclick="processPlan()">🚀 CALCULAR PLANIFICACIÓN ESTRATÉGICA</button>
        </div>

        <div id="section-messages" class="card full" style="display:none">
            <h2>📩 MENSAJERÍA POR JUGADOR</h2>
            <div class="msg-grid" id="msg-grid"></div>
        </div>

        <div id="section-results" class="card full" style="display:none">
            <div style="display:flex;justify-content:space-between;margin-bottom:15px">
                <h2>📋 TABLA DE ÓRDENES</h2>
                <button class="btn-main" style="width:auto;margin:0" onclick="copyForo()">📋 COPIAR TODO PARA EL FORO</button>
            </div>
            <table class="res-table">
                <thead>
                    <tr><th>Lanzamiento</th><th>Origen</th><th>Destino</th><th>Tropas</th><th>Tipo</th><th>🚀</th></tr>
                </thead>
                <tbody id="res-body"></tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const UNITS_DATA = { spear:18, sword:22, axe:18, archer:18, spy:9, light:10, marcher:10, heavy:11, ram:30, catapult:30, snob:35 };

    function toggleRealCustom(){ document.getElementById('real-custom').style.display = document.getElementById('real-mode').value === 'custom' ? 'grid' : 'none'; }

    function parseInput(text) {
        const data = [];
        if(!text) return data;
        try { return JSON.parse(text); } catch(e) {
            text.split('\n').forEach(line => {
                const m = line.match(/(\d{1,3})\|(\d{1,3})/);
                if(m) data.push({x: parseInt(m[1]), y: parseInt(m[2]), ck: m[0], p: "Manual", t: {}});
            });
            return data;
        }
    }

    function processPlan() {
        const offList = parseInput(document.getElementById('in-alliance').value);
        const nobleList = parseInput(document.getElementById('in-nobles').value);
        const conqTargets = parseInput(document.getElementById('in-conq').value);
        const realTargets = parseInput(document.getElementById('in-real').value);
        const fakeTargets = parseInput(document.getElementById('in-fake').value);
        
        const mAxe = parseInt(document.getElementById('min-axe').value);
        const mRam = parseInt(document.getElementById('min-ram').value);
        const mDist = parseFloat(document.getElementById('min-dist').value) || 0;
        const vMundo = parseFloat(document.getElementById('world-speed').value) || 1;
        const arrivalMs = new Date(document.getElementById('date-arr').value + 'T' + document.getElementById('time-arr').value).getTime();

        if(isNaN(arrivalMs)) return alert("Fecha u hora inválida.");

        let results = [];
        let usedAtks = new Set();

        // 1. ASIGNAR CONQUISTAS (Nobles)
        let noblePool = nobleList.map(n => ({...n, player: n.player || n.p, t: n.troops || n.t || {}}));
        conqTargets.forEach(t => {
            if(!noblePool.length) return;
            noblePool.sort((a,b) => dist(a,t) - dist(b,t));
            const winner = noblePool.shift();
            usedAtks.add(`${winner.x}|${winner.y}`);
            results.push(buildOrder(winner, t, 'CONQUISTA', arrivalMs, vMundo, 'snob'));
        });

        // 2. ASIGNAR REALES (OFFs que no son nobles ni están cerca)
        let offPool = offList.filter(v => {
            const ck = `${v.x}|${v.y}`;
            const meetsFilter = (v.axe || v.t?.axe || 0) >= mAxe && (v.ram || v.t?.ram || 0) >= mRam;
            const isNotUsed = !usedAtks.has(ck);
            // Filtro de distancia de seguridad
            const tooClose = realTargets.some(t => dist(v, t) < mDist);
            return meetsFilter && isNotUsed && !tooClose;
        }).map(v => ({...v, player: v.player || v.p, t: v.troops || v.t || {axe: v.axe, light: v.light, ram: v.ram, catapult: v.catapult, snob: v.snob}}));

        realTargets.forEach(t => {
            if(!offPool.length) return;
            offPool.sort((a,b) => dist(a,t) - dist(b,t));
            const winner = offPool.shift();
            usedAtks.add(`${winner.x}|${winner.y}`);
            results.push(buildOrder(winner, t, 'REAL', arrivalMs, vMundo, document.getElementById('unit-guide').value));
        });

        // 3. ASIGNAR FAKES (Lo que quede)
        let fakePool = offList.filter(v => !usedAtks.has(`${v.x}|${v.y}`));
        fakeTargets.forEach((t, i) => {
            if(!fakePool.length) return;
            const winner = fakePool[i % fakePool.length];
            results.push(buildOrder(winner, t, 'FAKE', arrivalMs, vMundo, document.getElementById('unit-guide').value));
        });

        renderAll(results);
    }

    function dist(a, b) { return Math.sqrt(Math.pow(a.x-b.x,2)+Math.pow(a.y-b.y,2)); }

    function buildOrder(v, t, type, arrMs, vMundo, uG) {
        const d = dist(v, t);
        const travelMs = (d * UNITS_DATA[uG] / vMundo) * 60000;
        const launch = new Date(arrMs - travelMs);
        
        let send = {};
        if(type === 'CONQUISTA') send = { snob: 4, axe: 1000, light: 500 };
        else if(type === 'REAL') {
            if(document.getElementById('real-mode').value === 'all') send = v.t;
            else send = { axe: el('rc-axe').value, light: el('rc-light').value, ram: el('rc-ram').value };
        } else {
            send = { axe: el('fc-axe').value, spy: el('fc-spy').value, ram: el('fc-ram').value };
        }

        return {
            player: v.p || v.player, origin: `${v.x}|${v.y}`, vid: v.vid,
            target: t.ck, launch: launch.toLocaleString(), type, troops: send,
            url: buildUrl(v.vid, t, send)
        };
    }

    function buildUrl(vid, t, tr) {
        let u = `https://es100.guerrastribales.es/game.php?village=${vid}&screen=place&target=${t.x}${t.y}`;
        for(let k in tr) if(tr[k]>0) u += `&${k}=${tr[k]}`;
        return u;
    }

    function renderAll(res) {
        const body = document.getElementById('res-body');
        const mGrid = document.getElementById('msg-grid');
        body.innerHTML = ""; mGrid.innerHTML = "";
        
        let grouped = {};
        res.forEach(r => { if(!grouped[r.player]) grouped[r.player] = []; grouped[r.player].push(r); });

        for(let p in grouped) {
            let hRow = ` <tr class="player-header"><td colspan="6">👤 JUGADOR: ${p}</td></tr>`;
            body.innerHTML += hRow;
            grouped[p].forEach(r => {
                let trs = "";
                const em = {axe:'🪓',light:'🐴',ram:'🔨',snob:'📜',spy:'🕵️',catapult:'🪨'};
                for(let k in em) if(r.troops[k]>0) trs += `<span class="pill">${r.troops[k]}${em[k]}</span>`;
                
                let tagCls = r.type === 'CONQUISTA' ? 'tag-conq' : (r.type === 'REAL' ? 'tag-real' : 'tag-fake');
                body.innerHTML += `<tr>
                    <td><b style="color:var(--gold)">${r.launch}</b></td>
                    <td>${r.origin}</td><td><b>${r.target}</b></td>
                    <td>${trs}</td><td><span class="tr-tag ${tagCls}">${r.type}</span></td>
                    <td><button class="btn bp" style="padding:5px" onclick="window.open('${r.url}')">🚀</button></td>
                </tr>`;
            });
            mGrid.innerHTML += `<div class="msg-card"><span><b>${p}</b> (${grouped[p].length})</span><button class="btn bm" onclick="copyMP('${p}')">📩 COPIAR MP</button></div>`;
        }
        document.getElementById('section-messages').style.display = "block";
        document.getElementById('section-results').style.display = "block";
        window.currentPlan = grouped;
    }

    function copyMP(p) {
        const orders = window.currentPlan[p];
        let bb = `Hola [player]${p}[/player],\n\n[table]\n[**]Tipo[||]Lanzar[||]Origen[||]Destino[/**]\n`;
        orders.forEach(o => bb += `[*]${o.type}[|]${o.launch}[|][coord]${o.origin}[/coord][|][coord]${o.target}[/coord]\n`);
        bb += `[/table]\nConfirma, gracias.`;
        navigator.clipboard.writeText(bb);
        alert("Copiado.");
    }

    function copyFullForo() {
        let bb = "[b]📅 PLANIFICACIÓN ESTRATÉGICA[/b]\n\n";
        for(let p in window.currentPlan) {
            bb += `[player]${p}[/player]\n[spoiler=Órdenes][table]\n[**]Tipo[||]Lanzar[||]Origen[||]Destino[/**]\n`;
            window.currentPlan[p].forEach(o => bb += `[*]${o.type}[|]${o.launch}[|][coord]${o.origin}[/coord][|][coord]${o.target}[/coord]\n`);
            bb += `[/table][/spoiler]\n\n`;
        }
        navigator.clipboard.writeText(bb);
        alert("Copiado Foro.");
    }

    function el(id){return document.getElementById(id)}
    function buildUnits() {
        document.getElementById('unit-grid').innerHTML = UNITS.map(u => `
            <div class="ub ${u.id===S.uGuia?'sel':''}" onclick="S.uGuia='${u.id}';buildUnits()">
                <div style="font-size:1.2rem">${u.e}</div><div style="font-size:0.6rem">${u.nm}</div>
            </div>`).join('');
    }
    window.onload = () => { buildUnits(); document.getElementById('in-date').value = new Date().toISOString().split('T')[0]; };
</script>
</body>
</html>
