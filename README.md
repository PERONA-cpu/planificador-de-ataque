<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador Guerras Tribales - Multiselección</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0f1117;--sur:#1a1d27;--sur2:#23263a;--brd:#2e3145;--acc:#7c6af7;--acc2:#5b8cf5;--gold:#f5c542;--grn:#4caf7d;--red:#e05252;--txt:#e8eaf6;--txt2:#8b90b0;--r:12px}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;padding:20px 14px}
        .wrap{max-width:1100px;margin:0 auto}
        h1{font-size:1.8rem;font-weight:700;background:linear-gradient(135deg,var(--acc),var(--gold));-webkit-background-clip:text;-webkit-text-fill-color:transparent;text-align:center;margin-bottom:20px}
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:var(--r);padding:20px;margin-bottom:15px}
        label{display:block;font-size:.75rem;color:var(--txt2);margin-bottom:5px;text-transform:uppercase;font-weight:700}
        input, textarea, select{width:100%;padding:10px;background:var(--sur2);border:1px solid var(--brd);border-radius:8px;color:var(--txt);font-family:inherit;outline:none}
        .btn{padding:10px 16px;border-radius:8px;border:none;font-weight:700;cursor:pointer;transition:0.2s;display:inline-flex;align-items:center;justify-content:center;gap:8px}
        .bp{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#fff}
        .bs{background:var(--gold);color:#000; font-size: 0.9rem; border: 1px solid #fff}
        .bm{background:var(--acc2); color:#fff; font-size: 0.75rem; padding: 5px 10px}
        .bg{background:var(--brd);color:var(--txt);font-size: 0.7rem; padding: 5px 8px}
        .ugrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(80px,1fr));gap:8px;margin-bottom:15px}
        .ub{background:var(--sur2);border:2px solid var(--brd);border-radius:8px;padding:8px;text-align:center;cursor:pointer}
        .ub.sel{border-color:var(--acc);background:rgba(124,106,247,.15)}
        .troop-selector{display:grid; grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); gap: 10px; background: rgba(0,0,0,0.2); padding: 15px; border-radius: 8px; border: 1px solid var(--brd); margin-bottom: 15px}
        .t-in{display:flex; align-items:center; gap:5px}
        .t-in input{padding: 5px; text-align: center}
        table{width:100%;border-collapse:collapse}
        th{text-align:left;padding:12px;background:var(--sur2);color:var(--txt2);font-size:0.75rem}
        td{padding:10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .msg-grid{display:grid;grid-template-columns:repeat(auto-fill, minmax(240px, 1fr)); gap:10px; margin-top:15px}
        .msg-card{background:var(--sur2); padding:10px; border-radius:8px; border:1px solid var(--brd); display:flex; justify-content:space-between; align-items:center}
        .check-custom{width: 18px !important; height: 18px !important; cursor: pointer; accent-color: var(--gold)}
    </style>
</head>
<body>

<div class="wrap">
    <h1>⚔️ Planificador de Guerras Tribales</h1>

    <!-- CONFIG MUNDO -->
    <div class="card">
        <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:15px; align-items:end">
            <div><label>⚡ Velocidad Mundo</label><input type="number" id="world-unit-speed" value="1.0" step="0.1"></div>
            <div>
                <label>🌐 Servidor</label>
                <select id="w-server" onchange="twLoadWorlds()">
                    <option value="">Selecciona...</option>
                    <option value="es_guerrastribales.es_es">España (.es)</option>
                    <option value="en_tribalwars.net_www">Internacional (.net)</option>
                </select>
            </div>
            <div><label>🌍 Mundo</label><select id="w-world" onchange="twApplyConfig()" disabled><option value="">Mundos...</option></select></div>
        </div>
    </div>

    <!-- CONFIG FAKES -->
    <div class="card">
        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px">
            <div><label>🎯 Objetivos (Enemigos)</label><textarea id="ta-tgt-fk" rows="6" placeholder="Enemigo 400|400"></textarea></div>
            <div><label>💪 Atacantes (Aliados)</label><textarea id="ta-atk-fk" rows="6" placeholder="Aliado 500|500"></textarea></div>
        </div>

        <label style="margin-top:15px">🛡️ Composición del envío:</label>
        <div class="troop-selector" id="troop-inputs">
            <div class="t-in"><span>🗡️</span><input type="number" id="tr-spear" value="0"></div>
            <div class="t-in"><span>⚔️</span><input type="number" id="tr-sword" value="0"></div>
            <div class="t-in"><span>🪓</span><input type="number" id="tr-axe" value="0"></div>
            <div class="t-in"><span>🕵️</span><input type="number" id="tr-spy" value="1"></div>
            <div class="t-in"><span>🐴</span><input type="number" id="tr-light" value="0"></div>
            <div class="t-in"><span>🏇</span><input type="number" id="tr-heavy" value="0"></div>
            <div class="t-in"><span>🔨</span><input type="number" id="tr-ram" value="1"></div>
            <div class="t-in"><span>🪨</span><input type="number" id="tr-catapult" value="0"></div>
        </div>
        
        <label>Unidad para calcular distancia:</label>
        <div class="ugrid" id="ugrid-fakes"></div>

        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px">
            <div><label>Fecha Llegada</label><input type="date" id="fk-date"></div>
            <div><label>Hora Llegada</label><input type="time" id="fk-time" value="08:00:00" step="1"></div>
        </div>

        <button class="btn bp" style="width:100%; margin-top:15px" onclick="calcFakes()">🚀 Generar Planificación</button>
    </div>

    <!-- CENTRO DE MENSAJES CON SELECCIÓN MÚLTIPLE -->
    <div id="player-messages-card" class="card" style="display:none; border-color: var(--acc2)">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:10px">
            <label style="color: var(--acc2); margin:0">📩 Centro de Mensajes</label>
            <div style="display:flex; gap:10px">
                <button class="btn bg" onclick="toggleAllChecks('msg-check', true)">Marcar Todos</button>
                <button class="btn bg" onclick="toggleAllChecks('msg-check', false)">Desmarcar</button>
                <button class="btn bs" onclick="copySelectedMPs()">📋 Copiar Seleccionados</button>
            </div>
        </div>
        <div class="msg-grid" id="msg-grid"></div>
    </div>

    <!-- TABLA GENERAL -->
    <div id="fk-result-card" class="card" style="display:none">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px">
            <label>Horarios y Salidas</label>
            <button class="btn bs" onclick="copySelectedRows()">📋 Copiar Filas Seleccionadas</button>
        </div>
        <div style="overflow-x:auto">
            <table>
                <thead>
                    <tr>
                        <th style="width:30px"><input type="checkbox" onchange="toggleAllChecks('row-check', this.checked)"></th>
                        <th>Lanzar el</th>
                        <th>Atacante</th>
                        <th>Origen</th>
                        <th>Hacia</th>
                        <th>Objetivo</th>
                        <th>🚀</th>
                    </tr>
                </thead>
                <tbody id="fk-body"></tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const UNITS=[
        {id:'spear',nm:'Lanza',spd:18,e:'🗡️'},{id:'sword',nm:'Espada',spd:22,e:'⚔️'},
        {id:'axe',nm:'Hacha',spd:18,e:'🪓'},{id:'spy',nm:'Espía',spd:9,e:'🕵️'},
        {id:'light',nm:'Ligera',spd:10,e:'🐴'},{id:'heavy',nm:'Pesada',spd:11,e:'🏇'},
        {id:'ram',nm:'Ariete',spd:30,e:'🔨'},{id:'snob',nm:'Noble',spd:35,e:'📜'}
    ];

    let S = { unitFk: 'ram', results: [], grouped: {}, troopConfig: {} };

    // --- MUNDOS ---
    async function twLoadWorlds() {
        const srv = document.getElementById('w-server').value;
        const wl = document.getElementById('w-world');
        if(!srv) return;
        const [prefix, domain, tws] = srv.split('_');
        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://' + tws + '.twstats.com/')}`;
            const res = await fetch(url);
            const html = await res.text();
            const worlds = [];
            const regex = new RegExp(`data-world="${prefix}(\\d+|p\\d+|c\\d+|s\\d+)"`, 'g');
            let m;
            while ((m = regex.exec(html)) !== null) { if(!worlds.includes(m[1])) worlds.push(m[1]); }
            wl.innerHTML = '<option value="">Mundos...</option>';
            worlds.sort((a,b)=>b.localeCompare(a, undefined, {numeric:true})).forEach(w=>{
                let opt = document.createElement('option');
                opt.value = prefix+w; opt.innerText = (prefix+w).toUpperCase();
                wl.appendChild(opt);
            });
            wl.disabled = false;
        } catch(e) {
            wl.innerHTML = '<option value="es104">ES104</option><option value="es100">ES100</option>';
            wl.disabled = false;
        }
    }

    async function twApplyConfig() {
        const w = document.getElementById('w-world').value;
        const domain = document.getElementById('w-server').value.split('_')[1];
        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://'+w+'.'+domain+'/interface.php?func=get_config')}`;
            const res = await fetch(url);
            const xml = await res.text();
            const doc = new DOMParser().parseFromString(xml, "text/xml");
            const s = parseFloat(doc.querySelector('speed').textContent);
            const us = parseFloat(doc.querySelector('unit_speed').textContent);
            document.getElementById('world-unit-speed').value = (s * us).toFixed(3);
        } catch(e) {}
    }

    // --- LÓGICA ---
    function parseInput(text) {
        const data = [];
        text.split('\n').forEach(line => {
            const m = line.match(/^(.+?)\s+(\d{1,3})\|(\d{1,3})$/);
            if(m) data.push({ p: m[1].trim(), x: m[2], y: m[3] });
        });
        return data;
    }

    function calcFakes() {
        const tgts = parseInput(document.getElementById('ta-tgt-fk').value);
        const atks = parseInput(document.getElementById('ta-atk-fk').value);
        const date = document.getElementById('fk-date').value;
        const time = document.getElementById('fk-time').value;
        if(!atks.length || !tgts.length) return alert("Faltan datos");

        S.troopConfig = {
            spear: el('tr-spear').value, sword: el('tr-sword').value, axe: el('tr-axe').value,
            spy: el('tr-spy').value, light: el('tr-light').value, heavy: el('tr-heavy').value,
            ram: el('tr-ram').value, catapult: el('tr-catapult').value
        };

        const arrivalMs = new Date(`${date}T${time}`).getTime();
        const unit = UNITS.find(u => u.id === S.unitFk);
        const wSpd = parseFloat(document.getElementById('world-unit-speed').value) || 1;
        const worldStr = document.getElementById('w-world').value || 'es100';

        S.results = []; S.grouped = {};

        atks.forEach((a, i) => {
            const t = tgts[i % tgts.length]; 
            const dist = Math.sqrt(Math.pow(a.x - t.x, 2) + Math.pow(a.y - t.y, 2));
            const launchDate = new Date(arrivalMs - ((dist * unit.spd) / wSpd) * 60000);
            let trUrl = '';
            for(let key in S.troopConfig) if(S.troopConfig[key] > 0) trUrl += `&${key}=${S.troopConfig[key]}`;

            const resObj = {
                launch: launchDate.toLocaleString(),
                attacker: a.p,
                origin: `${a.x}|${a.y}`,
                target: `${t.x}|${t.y}`,
                unit: unit.e,
                url: `https://${worldStr}.guerrastribales.es/game.php?screen=place&target=${t.x}${t.y}${trUrl}`
            };

            S.results.push(resObj);
            if(!S.grouped[a.p]) S.grouped[a.p] = [];
            S.grouped[a.p].push(resObj);
        });
        renderAll();
    }

    function renderAll() {
        let h = '';
        S.results.forEach((r, i) => {
            h += `<tr>
                <td><input type="checkbox" class="row-check check-custom" data-index="${i}"></td>
                <td>${r.launch}</td><td>${r.attacker}</td><td>${r.origin}</td><td>→</td><td>${r.target}</td>
                <td><button class="btn bm" onclick="window.open('${r.url}')">🚀</button></td>
            </tr>`;
        });
        document.getElementById('fk-body').innerHTML = h;
        document.getElementById('fk-result-card').style.display = 'block';

        let m = '';
        Object.keys(S.grouped).sort().forEach(p => {
            m += `<div class="msg-card">
                <input type="checkbox" class="msg-check check-custom" data-player="${p}">
                <span style="flex-grow:1; margin-left:10px"><b>${p}</b></span>
                <button class="btn bm" onclick="copyMP('${p}')">📩</button>
            </div>`;
        });
        document.getElementById('msg-grid').innerHTML = m;
        document.getElementById('player-messages-card').style.display = 'block';
    }

    // --- COPIADO MÚLTIPLE ---

    function toggleAllChecks(className, status) {
        document.querySelectorAll('.' + className).forEach(c => c.checked = status);
    }

    function getTroopBB() {
        let trStr = '';
        for(let k in S.troopConfig) if(S.troopConfig[k]>0) trStr += `${S.troopConfig[k]} [unit]${k}[/unit] `;
        return trStr;
    }

    function copySelectedMPs() {
        let finalBB = `[b]📅 ÓRDENES DE ATAQUE POR JUGADOR[/b]\n\n`;
        let count = 0;
        document.querySelectorAll('.msg-check:checked').forEach(cb => {
            const player = cb.getAttribute('data-player');
            const orders = S.grouped[player];
            finalBB += `[player]${player}[/player]\n[spoiler=Ver órdenes][table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
            orders.forEach(o => {
                finalBB += `[*]${o.launch}[|][coord]${o.origin}[/coord][|][coord]${o.target}[/coord][|]${getTroopBB()}\n`;
            });
            finalBB += `[/table][/spoiler]\n\n`;
            count++;
        });

        if(count === 0) return alert("Selecciona al menos un jugador");
        navigator.clipboard.writeText(finalBB);
        alert(`Copiadas las órdenes de ${count} jugadores.`);
    }

    function copySelectedRows() {
        let bb = `[table]\n[**]Lanzar[||]Atacante[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
        let count = 0;
        document.querySelectorAll('.row-check:checked').forEach(cb => {
            const r = S.results[cb.getAttribute('data-index')];
            bb += `[*]${r.launch}[|][player]${r.attacker}[/player][|][coord]${r.origin}[/coord][|][coord]${r.target}[/coord][|]${getTroopBB()}\n`;
            count++;
        });

        if(count === 0) return alert("Selecciona algunas filas primero");
        bb += `[/table]`;
        navigator.clipboard.writeText(bb);
        alert(`Copiadas ${count} órdenes sueltas.`);
    }

    function copyMP(player) {
        const orders = S.grouped[player];
        let bb = `Hola [player]${player}[/player],\n\n[table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
        orders.forEach(o => bb += `[*]${o.launch}[|][coord]${o.origin}[/coord][|][coord]${o.target}[/coord][|]${getTroopBB()}\n`);
        bb += `[/table]`;
        navigator.clipboard.writeText(bb);
        alert("Mensaje copiado para " + player);
    }

    function copyAllBB() {
        toggleAllChecks('msg-check', true);
        copySelectedMPs();
    }

    function buildUnitGrid() {
        document.getElementById('ugrid-fakes').innerHTML = UNITS.map(u => `
            <div class="ub ${u.id===S.unitFk?'sel':''}" onclick="S.unitFk='${u.id}';buildUnitGrid()">
                <div style="font-size:1.2rem">${u.e}</div><div style="font-size:0.6rem">${u.nm}</div>
            </div>`).join('');
    }

    function el(id){return document.getElementById(id)}
    window.onload = () => { buildUnitGrid(); document.getElementById('fk-date').value = new Date().toISOString().split('T')[0]; };
</script>
</body>
</html>
