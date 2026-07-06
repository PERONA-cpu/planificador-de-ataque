<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador definitivo de Sarius</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0f1117;--sur:#1a1d27;--sur2:#23263a;--brd:#2e3145;--acc:#7c6af7;--acc2:#5b8cf5;--gold:#f5c542;--grn:#4caf7d;--red:#e05252;--txt:#e8eaf6;--txt2:#8b90b0;--r:12px}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;padding:20px 14px}
        .wrap{max-width:1250px;margin:0 auto}
        
        /* TÍTULO PEDIDO */
        .main-title{font-size:2rem;font-weight:900;color:var(--gold);text-align:center;margin-bottom:20px;text-transform:uppercase;letter-spacing:2px;text-shadow: 0 2px 10px rgba(245,197,66,0.3)}
        
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:var(--r);padding:20px;margin-bottom:15px;box-shadow: 0 4px 20px rgba(0,0,0,0.4)}
        .ct{font-size:.95rem;font-weight:600;margin-bottom:13px;display:flex;align-items:center;gap:8px}
        .ico{width:26px;height:26px;border-radius:6px;background:linear-gradient(135deg,var(--acc),var(--acc2));display:flex;align-items:center;justify-content:center;font-size:14px}
        
        label{display:block;font-size:.7rem;color:var(--txt2);margin-bottom:5px;text-transform:uppercase;font-weight:800;letter-spacing:0.05em}
        input, textarea, select{width:100%;padding:10px;background:var(--sur2);border:1px solid var(--brd);border-radius:8px;color:var(--txt);font-family:inherit;outline:none;transition:0.2s}
        input:focus, textarea:focus{border-color:var(--acc); box-shadow: 0 0 0 2px rgba(124,106,247,0.2)}
        
        .grid-3{display:grid; grid-template-columns: 1fr 1.5fr 1fr 1fr; gap:12px; align-items:end}
        .grid-targets{display:grid; grid-template-columns: 1fr 1fr 1.5fr; gap:15px}
        
        .btn{padding:12px 20px;border-radius:8px;border:none;font-weight:700;cursor:pointer;transition:0.2s;display:inline-flex;align-items:center;justify-content:center;gap:8px;font-size:0.85rem}
        .bp{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#fff}
        .bs{background:var(--gold);color:#000;border:2px solid #fff}
        .bm{background:var(--acc2); color:#fff; font-size: 0.75rem; padding: 5px 10px}
        .bg{background:var(--brd);color:var(--txt)}
        
        .ugrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(85px,1fr));gap:8px;margin-bottom:15px}
        .ub{background:var(--sur2);border:2px solid var(--brd);border-radius:8px;padding:8px;text-align:center;cursor:pointer}
        .ub.sel{border-color:var(--acc);background:rgba(124,106,247,.15)}
        
        /* FILTRO OFENSIVO */
        .filter-panel{background:rgba(224,82,82,0.05); border:1px solid rgba(224,82,82,0.2); border-radius:10px; padding:15px; margin-bottom:15px}
        .troop-selector{display:grid; grid-template-columns: repeat(auto-fill, minmax(110px, 1fr)); gap: 10px}
        .t-in{display:flex; align-items:center; gap:5px; background:var(--bg); padding:5px; border-radius:6px; border:1px solid var(--brd)}
        .t-in input{padding: 2px; text-align: center; border:none; background:transparent}

        /* TABLA Y RESULTADOS */
        table{width:100%;border-collapse:collapse}
        th{text-align:left;padding:12px;background:var(--sur2);color:var(--txt2);font-size:0.7rem;text-transform:uppercase}
        td{padding:10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .player-row{background:rgba(124,106,247,0.15); font-weight:900; color:var(--acc2)}
        
        /* NITIDEZ TROPAS */
        .tr-pill{display:inline-flex; align-items:center; gap:4px; background:#000; padding:3px 8px; border-radius:5px; border:1px solid #333; margin:2px; font-weight:800; color:#fff; font-size:0.75rem}
        .tr-pill.empty{opacity:0.3; font-weight:400}
        
        .tag{padding:2px 6px; border-radius:4px; font-size:0.65rem; font-weight:900; margin-left:5px}
        .tag-real{background:var(--red); color:#fff}
        .tag-fake{background:var(--acc2); color:#fff}

        .msg-grid{display:grid;grid-template-columns:repeat(auto-fill, minmax(280px, 1fr)); gap:12px}
        .msg-card{background:var(--sur2); padding:12px; border-radius:10px; border:1px solid var(--brd); display:flex; justify-content:space-between; align-items:center}
        .check-c{accent-color:var(--gold); width:20px; height:20px; cursor:pointer}
    </style>
</head>
<body>

<div class="wrap">
    <div class="main-title">Planificador definitivo de Sarius</div>

    <!-- 1. CONFIGURACIÓN -->
    <div class="card">
        <div class="grid-3">
            <div><label>⚡ Vel. Unidades</label><input type="number" id="v-mundo" value="1.0" step="0.1"></div>
            <div>
                <label>📍 Servidor</label>
                <select id="v-server" onchange="twLoadWorlds()">
                    <option value="es_guerrastribales.es_es">España (.es)</option>
                    <option value="en_tribalwars.net_www">Internacional (.net)</option>
                </select>
            </div>
            <div>
                <label>🌍 Mundo</label>
                <select id="v-world" onchange="twApplyConfig()"><option value="es100">ES100</option></select>
            </div>
            <div><label>Acción</label><button class="btn bp" style="width:100%" onclick="mainAction()">🚀 CALCULAR TODO</button></div>
        </div>
        <div id="v-status" style="font-size:0.7rem; margin-top:5px; color:var(--gold)"></div>
    </div>

    <!-- 2. FILTROS OFENSIVOS -->
    <div class="filter-panel">
        <label style="color:var(--red); margin-bottom:10px; display:block">⚔️ Filtro de pueblos para Ataques REALES (Mínimo de tropas necesarias para usar el pueblo)</label>
        <div class="troop-selector">
            <div class="t-in"><span>🪓</span><input type="number" id="f-axe" value="5000" title="Mínimo hachas"></div>
            <div class="t-in"><span>🐴</span><input type="number" id="f-light" value="2000" title="Mínimo ligeras"></div>
            <div class="t-in"><span>🔨</span><input type="number" id="f-ram" value="250" title="Mínimo arietes"></div>
            <div style="display:flex; align-items:center; padding-left:10px; color:var(--txt2); font-size:0.7rem">Solo pueblos que superen esto serán usados para objetivos REALES.</div>
        </div>
    </div>

    <!-- 3. OBJETIVOS Y ATACANTES -->
    <div class="grid-targets">
        <div class="card">
            <div class="ct"><div class="ico" style="background:var(--red)">🔥</div> Objetivos REALES</div>
            <textarea id="in-real" rows="10" placeholder="Enemigo 400|400"></textarea>
        </div>
        <div class="card">
            <div class="ct"><div class="ico" style="background:var(--acc2)">🎭</div> Objetivos FAKES</div>
            <textarea id="in-fakes" rows="10" placeholder="Enemigo 401|401"></textarea>
        </div>
        <div class="card">
            <div class="ct"><div class="ico" style="background:var(--grn)">💪</div> Atacantes (JSON Tropas)</div>
            <textarea id="in-attackers" rows="10" placeholder="Pega el JSON de tropas aquí..."></textarea>
            <div id="count-info" style="margin-top:5px; font-size:0.7rem; font-weight:bold"></div>
        </div>
    </div>

    <!-- 4. VELOCIDAD -->
    <div class="card">
        <label>Unidad más lenta del envío (Calcula tiempo):</label>
        <div class="ugrid" id="unit-grid"></div>
        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px">
            <div><label>Fecha Llegada</label><input type="date" id="in-date"></div>
            <div><label>Hora Llegada</label><input type="time" id="in-time" value="08:00:00" step="1"></div>
        </div>
    </div>

    <!-- 5. MENSAJERÍA -->
    <div id="card-messages" class="card" style="display:none; border-color:var(--acc2)">
        <div class="ct" style="color:var(--acc2)">📩 Centro de Mensajería Individual</div>
        <div style="display:flex; gap:10px; margin-bottom:15px">
            <button class="btn bs" onclick="copyMultiMP()">📋 Copiar Selección</button>
            <button class="btn bg" onclick="selAll(true)">Todos</button>
            <button class="btn bg" onclick="selAll(false)">Ninguno</button>
        </div>
        <div class="msg-grid" id="msg-grid"></div>
    </div>

    <!-- 6. RESULTADOS -->
    <div id="card-results" class="card" style="display:none">
        <div class="ct">📋 Órdenes de Ataque Agrupadas</div>
        <button class="btn bs" style="width:100%; margin-bottom:15px" onclick="copyFullForo()">📋 COPIAR TODO PARA EL FORO (Spoilers)</button>
        <div style="overflow-x:auto">
            <table>
                <thead>
                    <tr><th>Lanzar el</th><th>Origen</th><th>Hacia</th><th>Objetivo</th><th>Tropas del Pueblo</th><th>Tipo</th><th>🚀</th></tr>
                </thead>
                <tbody id="res-body"></tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const UNITS=[
        {id:'spear',nm:'Lanza',spd:18,e:'🗡️'},{id:'sword',nm:'Espada',spd:22,e:'⚔️'},
        {id:'axe',nm:'Hacha',spd:18,e:'🪓'},{id:'spy',nm:'Espía',spd:9,e:'🕵️'},
        {id:'light',nm:'Ligera',spd:10,e:'🐴'},{id:'heavy',nm:'Pesada',spd:11,e:'🏇'},
        {id:'ram',nm:'Ariete',spd:30,e:'🔨'},{id:'catapult',nm:'Cata',spd:30,e:'🪨'},
        {id:'snob',nm:'Noble',spd:35,e:'📜'}
    ];

    let S = { uGuia: 'ram', results: [], grouped: {}, used: new Set() };

    // --- LOGICA ---
    function parseJSON(text) {
        try {
            return JSON.parse(text).map(v => ({
                p: v.player || v.p, 
                x: v.x, y: v.y, ck: `${v.x}|${v.y}`, 
                vid: v.vid,
                t: {
                    spear: v.spear||0, sword: v.sword||0, axe: v.axe||0,
                    spy: v.spy||0, light: v.light||0, heavy: v.heavy||0,
                    ram: v.ram||0, catapult: v.catapult||0, snob: v.snob||0
                }
            }));
        } catch(e) { return []; }
    }

    function parseText(text) {
        const data = [];
        text.split('\n').forEach(l => {
            const m = l.match(/(\d{1,3})\|(\d{1,3})/);
            if(m) data.push({x:parseInt(m[1]), y:parseInt(m[2]), ck:m[0]});
        });
        return data;
    }

    function mainAction() {
        const atks = parseJSON(document.getElementById('in-attackers').value);
        const realTgts = parseText(document.getElementById('in-real').value);
        const fakeTgts = parseText(document.getElementById('in-fakes').value);
        
        const date = document.getElementById('in-date').value;
        const time = document.getElementById('in-time').value;
        if(!atks.length || (!realTgts.length && !fakeTgts.length) || !date || !time) return alert("Faltan datos.");

        const arrivalMs = new Date(`${date}T${time}`).getTime();
        const u = UNITS.find(x => x.id === S.uGuia);
        const vM = parseFloat(document.getElementById('v-mundo').value) || 1;
        const wStr = document.getElementById('v-world').value || 'es100';

        // Filtros de OFF
        const minAxe = parseInt(el('f-axe').value)||0;
        const minLight = parseInt(el('f-light').value)||0;
        const minRam = parseInt(el('f-ram').value)||0;

        S.results = []; S.grouped = {}; S.used.clear();

        // 1. PROCESAR REALES (Prioridad y Filtro)
        let offPueblos = atks.filter(a => a.t.axe >= minAxe && a.t.light >= minLight && a.t.ram >= minRam);
        
        realTgts.forEach(t => {
            if(!offPueblos.length) return;
            offPueblos.sort((a,b) => dist(a,t) - dist(b,t));
            const a = offPueblos.shift();
            S.used.add(a.ck);
            addRes(a, t, 'REAL', arrivalMs, u, vM, wStr);
        });

        // 2. PROCESAR FAKES (Resto de pueblos)
        fakeTgts.forEach((t, i) => {
            const a = atks[i % atks.length]; // Fakes se pueden repetir o usar cualquier cosa
            addRes(a, t, 'FAKE', arrivalMs, u, vM, wStr);
        });

        render();
    }

    function dist(a, b) { return Math.sqrt(Math.pow(a.x-b.x,2)+Math.pow(a.y-b.y,2)); }

    function addRes(a, t, mode, arrMs, u, vM, wStr) {
        const d = dist(a, t);
        const lDate = new Date(arrMs - ((d * u.spd) / vM) * 60000);
        
        // Rellenar URL según modo (Si es real, mandar todo. Si es fake, lo configurado)
        let trStr = "";
        if(mode === 'REAL') {
            for(let k in a.t) if(a.t[k]>0) trStr += `&${k}=${a.t[k]}`;
        } else {
            trStr = "&spy=1&ram=1"; // Default fake
        }

        const res = {
            l: lDate.toLocaleString(),
            p: a.p, orig: a.ck, dest: t.ck, mode: mode,
            tVis: getTVis(a.t),
            tBB: getTBB(a.t),
            url: `https://${wStr}.guerrastribales.es/game.php?screen=place&target=${t.x}${t.y}${trStr}`
        };
        S.results.push(res);
        if(!S.grouped[a.p]) S.grouped[a.p] = [];
        S.grouped[a.p].push(res);
    }

    function getTVis(t) {
        const em = {axe:'🪓',light:'🐴',ram:'🔨',snob:'📜',spy:'🕵️',heavy:'🏇',spear:'🗡️',sword:'⚔️'};
        let s = "";
        for(let k in em) {
            if(t[k]>0) s += `<span class="tr-pill">${t[k]} ${em[k]}</span>`;
            else s += `<span class="tr-pill empty">0 ${em[k]}</span>`;
        }
        return s;
    }

    function getTBB(t) {
        let s = "";
        if(t.axe>0) s += `${t.axe} [unit]axe[/unit] `;
        if(t.light>0) s += `${t.light} [unit]light[/unit] `;
        if(t.ram>0) s += `${t.ram} [unit]ram[/unit] `;
        return s;
    }

    function render() {
        let h = '';
        const sortedPlayers = Object.keys(S.grouped).sort();
        
        sortedPlayers.forEach(p => {
            h += `<tr class="player-row"><td colspan="7">👤 JUGADOR: ${p}</td></tr>`;
            S.grouped[p].forEach(r => {
                const tag = r.mode === 'REAL' ? 'tag-real' : 'tag-fake';
                h += `<tr>
                    <td><b style="color:var(--gold)">${r.l}</b></td>
                    <td>${r.orig}</td>
                    <td>→</td>
                    <td><b>${r.dest}</b></td>
                    <td>${r.tVis}</td>
                    <td><span class="tag ${tag}">${r.mode}</span></td>
                    <td><button class="btn bp" style="padding:5px" onclick="window.open('${r.url}')">🚀</button></td>
                </tr>`;
            });
        });
        
        document.getElementById('res-body').innerHTML = h;
        document.getElementById('card-results').style.display = 'block';

        let m = '';
        sortedPlayers.forEach(p => {
            m += `<div class="msg-card">
                <input type="checkbox" class="check-c" data-p="${p}">
                <span style="flex-grow:1; margin-left:10px"><b>${p}</b> <small>(${S.grouped[p].length})</small></span>
                <button class="btn bm" onclick="copyMP('${p}')">📩</button>
            </div>`;
        });
        document.getElementById('msg-grid').innerHTML = m;
        document.getElementById('card-messages').style.display = 'block';
    }

    function copyMP(p) {
        let bb = `Hola [player]${p}[/player],\n\n[table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tipo[||]Tropas[/**]\n`;
        S.grouped[p].forEach(o => {
            const type = o.mode === 'REAL' ? '[color=#ff0000][b]REAL[/b][/color]' : 'FAKE';
            bb += `[*]${o.l}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${type}[|]${o.tBB}\n`;
        });
        bb += `[/table]`;
        navigator.clipboard.writeText(bb);
        alert("Copiado MP para " + p);
    }

    function copyMultiMP() {
        let bb = "";
        document.querySelectorAll('.check-c:checked').forEach(c => {
            const p = c.getAttribute('data-p');
            bb += `[player]${p}[/player]\n[spoiler=Tus Órdenes][table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tipo[/**]\n`;
            S.grouped[p].forEach(o => {
                const type = o.mode === 'REAL' ? '[color=#ff0000]REAL[/color]' : 'FAKE';
                bb += `[*]${o.l}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${type}\n`;
            });
            bb += `[/table][/spoiler]\n\n`;
        });
        navigator.clipboard.writeText(bb);
        alert("Mensajes seleccionados copiados.");
    }

    function copyFullForo() {
        let bb = `[b]📅 PLANIFICACIÓN ESTRATÉGICA[/b]\n\n`;
        Object.keys(S.grouped).sort().forEach(p => {
            bb += `[player]${p}[/player]\n[spoiler=Ver órdenes][table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tipo[/**]\n`;
            S.grouped[p].forEach(o => {
                const type = o.mode === 'REAL' ? '[color=#ff0000]REAL[/color]' : 'FAKE';
                bb += `[*]${o.l}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${type}\n`;
            });
            bb += `[/table][/spoiler]\n\n`;
        });
        navigator.clipboard.writeText(bb);
        alert("Copiado para foro.");
    }

    // --- HELPERS ---
    function twLoadWorlds() {
        const s = document.getElementById('v-server').value;
        const wl = document.getElementById('v-world');
        wl.innerHTML = s.includes('es') ? '<option value="es104">ES104</option><option value="es100">ES100</option>' : '<option value="en130">EN130</option>';
    }
    function twApplyConfig() { document.getElementById('v-mundo').value = "1.0"; }
    function selAll(st) { document.querySelectorAll('.check-c').forEach(c => c.checked = st); }
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
