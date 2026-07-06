<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador Guerras Tribales ELITE - Yonkone</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0f1117;--sur:#1a1d27;--sur2:#23263a;--brd:#2e3145;--acc:#7c6af7;--acc2:#5b8cf5;--gold:#f5c542;--grn:#4caf7d;--red:#e05252;--txt:#e8eaf6;--txt2:#8b90b0;--r:12px}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;padding:20px 14px}
        .wrap{max-width:1200px;margin:0 auto}
        h1{font-size:1.8rem;font-weight:700;background:linear-gradient(135deg,var(--acc),var(--gold));-webkit-background-clip:text;-webkit-text-fill-color:transparent;text-align:center;margin-bottom:15px}
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:var(--r);padding:20px;margin-bottom:15px;position:relative}
        .ct{font-size:.95rem;font-weight:600;margin-bottom:13px;display:flex;align-items:center;gap:8px}
        .ico{width:24px;height:24px;border-radius:6px;background:linear-gradient(135deg,var(--acc),var(--acc2));display:flex;align-items:center;justify-content:center;font-size:12px}
        label{display:block;font-size:.7rem;color:var(--txt2);margin-bottom:5px;text-transform:uppercase;font-weight:700;letter-spacing:0.05em}
        input, textarea, select{width:100%;padding:10px;background:var(--sur2);border:1px solid var(--brd);border-radius:8px;color:var(--txt);font-family:inherit;outline:none;font-size:0.85rem}
        .btn{padding:10px 16px;border-radius:8px;border:none;font-weight:700;cursor:pointer;transition:0.2s;display:inline-flex;align-items:center;justify-content:center;gap:8px;font-size:0.85rem}
        .bp{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#fff}
        .bs{background:var(--gold);color:#000}
        .bg{background:var(--brd);color:var(--txt)}
        .ugrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(80px,1fr));gap:8px;margin-bottom:15px}
        .ub{background:var(--sur2);border:2px solid var(--brd);border-radius:8px;padding:8px;text-align:center;cursor:pointer}
        .ub.sel{border-color:var(--acc);background:rgba(124,106,247,.15)}
        .troop-selector{display:grid; grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); gap: 10px; background: rgba(0,0,0,0.2); padding: 12px; border-radius: 8px; border: 1px solid var(--brd); margin-bottom: 15px}
        .t-in{display:flex; align-items:center; gap:5px}
        .t-in input{padding: 4px; text-align: center; font-size: 0.75rem}
        table{width:100%;border-collapse:collapse}
        th{text-align:left;padding:10px;background:var(--sur2);color:var(--txt2);font-size:0.7rem;text-transform:uppercase}
        td{padding:10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .used-tag{background:var(--red); color:#fff; padding:2px 6px; border-radius:4px; font-size:0.65rem; font-weight:bold; margin-left:5px}
        .msg-grid{display:grid;grid-template-columns:repeat(auto-fill, minmax(260px, 1fr)); gap:10px; margin-top:10px}
        .msg-card{background:var(--sur2); padding:10px; border-radius:8px; border:1px solid var(--brd); display:flex; justify-content:space-between; align-items:center}
        .check-c{accent-color:var(--gold); width:18px; height:18px; cursor:pointer}
    </style>
</head>
<body>

<div class="wrap">
    <h1>⚔️ Planificador Estratégico ELITE</h1>

    <!-- 1. CONFIGURACIÓN DEL MUNDO -->
    <div class="card">
        <div class="ct"><div class="ico">🌐</div> Configuración</div>
        <div style="display:grid; grid-template-columns: 1fr 1fr 1fr 1fr; gap:12px; align-items:end">
            <div><label>⚡ Vel. Mundo</label><input type="number" id="v-mundo" value="1.0" step="0.1"></div>
            <div>
                <label>📍 Servidor</label>
                <select id="v-server" onchange="twLoadWorlds()">
                    <option value="">Selecciona...</option>
                    <option value="es_guerrastribales.es_es">España (.es)</option>
                    <option value="en_tribalwars.net_www">Internacional (.net)</option>
                </select>
            </div>
            <div><label>🌍 Mundo</label><select id="v-world" onchange="twApplyConfig()" disabled><option value="">Mundos...</option></select></div>
            <div><label>Tipo Ataque</label><select id="v-mode"><option value="real">⚔️ REAL (Bloquea pueblos)</option><option value="fakes">🎭 FAKES (Pueblos infinitos)</option></select></div>
        </div>
    </div>

    <!-- 2. ENTRADA DE DATOS -->
    <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px">
        <div class="card">
            <div class="ct"><div class="ico" style="background:var(--red)">🎯</div> Objetivos (Enemigos)</div>
            <textarea id="in-targets" rows="8" placeholder="Enemigo 400|400"></textarea>
        </div>
        <div class="card">
            <div class="ct"><div class="ico" style="background:var(--grn)">💪</div> Atacantes (Pueblos de la Tribu)</div>
            <textarea id="in-attackers" rows="8" placeholder='Pega el JSON de tropas aquí...'></textarea>
            <div style="margin-top:10px; display:flex; justify-content:space-between; align-items:center">
                <span id="count-used" style="font-size:0.7rem; color:var(--red); font-weight:bold">Pueblos usados: 0</span>
                <button class="btn bg" style="font-size:0.6rem; padding:4px 8px" onclick="resetUsed()">Resetear Pueblos Usados</button>
            </div>
        </div>
    </div>

    <!-- 3. COMPOSICIÓN DE TROPAS -->
    <div class="card">
        <div class="ct"><div class="ico">🛡️</div> Composición de Tropas por ataque</div>
        <div class="troop-selector">
            <div class="t-in"><span>🗡️</span><input type="number" id="tr-spear" value="0"></div>
            <div class="t-in"><span>⚔️</span><input type="number" id="tr-sword" value="0"></div>
            <div class="t-in"><span>🪓</span><input type="number" id="tr-axe" value="0"></div>
            <div class="t-in"><span>🕵️</span><input type="number" id="tr-spy" value="1"></div>
            <div class="t-in"><span>🐴</span><input type="number" id="tr-light" value="0"></div>
            <div class="t-in"><span>🏇</span><input type="number" id="tr-heavy" value="0"></div>
            <div class="t-in"><span>🔨</span><input type="number" id="tr-ram" value="1"></div>
            <div class="t-in"><span>🪨</span><input type="number" id="tr-catapult" value="0"></div>
        </div>
        <label>Calcular distancia según unidad:</label>
        <div class="ugrid" id="unit-grid"></div>
        
        <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:15px; margin-top:10px">
            <div><label>Fecha Llegada</label><input type="date" id="in-date"></div>
            <div><label>Hora Llegada</label><input type="time" id="in-time" value="08:00:00" step="1"></div>
            <div style="display:flex; align-items:end"><button class="btn bp" style="width:100%" onclick="mainAction()">🚀 GENERAR PLANIFICACIÓN</button></div>
        </div>
    </div>

    <!-- 4. CENTRO DE MENSAJERÍA -->
    <div id="card-messages" class="card" style="display:none; border-color:var(--acc2)">
        <div class="ct" style="color:var(--acc2)"><div class="ico" style="background:var(--acc2)">📩</div> Centro de Mensajería</div>
        <div style="display:flex; gap:10px; margin-bottom:10px">
            <button class="btn bg" onclick="selMsg(true)">Marcar Todos</button>
            <button class="btn bg" onclick="selMsg(false)">Desmarcar</button>
            <button class="btn bs" onclick="copyMultiMP()">📋 Copiar Seleccionados</button>
        </div>
        <div class="msg-grid" id="msg-grid"></div>
    </div>

    <!-- 5. TABLA DE RESULTADOS -->
    <div id="card-results" class="card" style="display:none">
        <div class="ct"><div class="ico">📋</div> Resultados</div>
        <button class="btn bs" style="width:100%; margin-bottom:15px" onclick="copyFullForo()">📋 COPIAR TODO PARA EL FORO (Spoilers)</button>
        <div style="overflow-x:auto">
            <table>
                <thead>
                    <tr><th>Lanzar el</th><th>Atacante</th><th>Origen</th><th>Hacia</th><th>Objetivo</th><th>Tropas</th><th>🚀</th></tr>
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

    let S = { uGuia: 'ram', used: new Set(), results: [], grouped: {}, troopConfig: {} };

    // --- MUNDOS FIX ---
    async function twLoadWorlds() {
        const srv = document.getElementById('v-server').value;
        const wl = document.getElementById('v-world');
        if(!srv) return;
        const [prefix, domain, tws] = srv.split('_');
        try {
            const res = await fetch(`https://api.allorigins.win/raw?url=${encodeURIComponent('https://' + tws + '.twstats.com/')}`);
            const html = await res.text();
            const worlds = [];
            const regex = new RegExp(`data-world="${prefix}(\\d+|p\\d+|c\\d+|s\\d+)"`, 'g');
            let m;
            while ((m = regex.exec(html)) !== null) { if(!worlds.includes(m[1])) worlds.push(m[1]); }
            wl.innerHTML = '<option value="">Elige Mundo...</option>';
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
        const w = document.getElementById('v-world').value;
        const srv = document.getElementById('v-server').value.split('_')[1];
        try {
            const res = await fetch(`https://api.allorigins.win/raw?url=${encodeURIComponent('https://'+w+'.'+srv+'/interface.php?func=get_config')}`);
            const xml = await res.text();
            const doc = new DOMParser().parseFromString(xml, "text/xml");
            const s = parseFloat(doc.querySelector('speed').textContent);
            const us = parseFloat(doc.querySelector('unit_speed').textContent);
            document.getElementById('v-mundo').value = (s * us).toFixed(3);
        } catch(e) {}
    }

    // --- LOGICA CORE ---
    function parseInput(text, isJSON = false) {
        const data = [];
        if(isJSON) {
            try {
                const json = JSON.parse(text);
                return json.map(v => ({ 
                    p: v.player || v.p, 
                    x: v.x, 
                    y: v.y, 
                    ck: `${v.x}|${v.y}`, 
                    vid: v.vid,
                    troops: {axe: v.axe, light: v.light, ram: v.ram, snob: v.snob, spear: v.spear, sword: v.sword, spy: v.spy, heavy: v.heavy}
                }));
            } catch(e) { alert("Error en JSON de atacantes"); return []; }
        } else {
            text.split('\n').forEach(line => {
                const m = line.match(/(\d{1,3})\|(\d{1,3})/);
                if(m) data.push({ p: 'Objetivo', x: parseInt(m[1]), y: parseInt(m[2]), ck: `${m[1]}|${m[2]}` });
            });
            return data;
        }
    }

    function mainAction() {
        const date = document.getElementById('in-date').value;
        const time = document.getElementById('in-time').value;
        if(!date || !time) return alert("Fecha/Hora faltante");

        const tgts = parseInput(document.getElementById('in-targets').value, false);
        const allAtks = parseInput(document.getElementById('in-attackers').value, true);
        const mode = document.getElementById('v-mode').value;
        
        S.troopConfig = {
            spear: el('tr-spear').value, sword: el('tr-sword').value, axe: el('tr-axe').value,
            spy: el('tr-spy').value, light: el('tr-light').value, heavy: el('tr-heavy').value,
            ram: el('tr-ram').value, catapult: el('tr-catapult').value
        };

        // Filtrar atacantes si es modo REAL (No usar pueblos ya marcados como usados)
        const availableAtks = mode === 'real' ? allAtks.filter(a => !S.used.has(a.ck)) : allAtks;
        
        if(!availableAtks.length || !tgts.length) return alert("No hay suficientes pueblos disponibles.");

        const arrMs = new Date(`${date}T${time}`).getTime();
        const u = UNITS.find(u => u.id === S.uGuia);
        const vM = parseFloat(document.getElementById('v-mundo').value) || 1;
        const wStr = document.getElementById('v-world').value || 'es100';

        S.results = []; S.grouped = {};

        // Asignación de pueblos (buscando el más cercano a cada objetivo)
        tgts.forEach((t, i) => {
            if(availableAtks.length === 0) return;
            
            // Buscar el atacante más cercano disponible para este objetivo
            availableAtks.sort((a, b) => {
                const d1 = Math.sqrt(Math.pow(a.x - t.x, 2) + Math.pow(a.y - t.y, 2));
                const d2 = Math.sqrt(Math.pow(b.x - t.x, 2) + Math.pow(b.y - t.y, 2));
                return d1 - d2;
            });

            const a = availableAtks.shift(); // Extraer el más cercano
            const dist = Math.sqrt(Math.pow(a.x - t.x, 2) + Math.pow(a.y - t.y, 2));
            const lDate = new Date(arrMs - ((dist * u.spd) / vM) * 60000);

            // Generar URL con tropas
            let trUrl = '';
            for(let key in S.troopConfig) {
                const qty = S.troopConfig[key] == 0 ? (a.troops[key] || 0) : S.troopConfig[key];
                if(qty > 0) trUrl += `&${key}=${qty}`;
            }

            const res = {
                l: lDate.toLocaleString(),
                p: a.p, orig: a.ck, dest: t.ck,
                trStr: getTroopBB(a.troops),
                url: `https://${wStr}.guerrastribales.es/game.php?screen=place&target=${t.x}${t.y}${trUrl}`
            };

            S.results.push(res);
            if(!S.grouped[a.p]) S.grouped[a.p] = [];
            S.grouped[a.p].push(res);
            
            if(mode === 'real') S.used.add(a.ck); // Marcar como usado permanentemente
        });

        document.getElementById('count-used').innerText = `Pueblos usados: ${S.used.size}`;
        render();
    }

    function render() {
        let h = '';
        S.results.forEach(r => {
            h += `<tr><td><b style="color:var(--gold)">${r.l}</b></td><td>${r.p}</td><td>${r.orig}</td><td>→</td><td>${r.dest}</td><td style="font-size:0.7rem">${r.trStr}</td><td><button class="btn bp" style="padding:4px" onclick="window.open('${r.url}')">🚀</button></td></tr>`;
        });
        document.getElementById('res-body').innerHTML = h;
        document.getElementById('card-results').style.display = 'block';

        let m = '';
        Object.keys(S.grouped).sort().forEach(p => {
            m += `<div class="msg-card"><input type="checkbox" class="check-c" data-p="${p}"><span style="flex-grow:1; margin-left:10px"><b>${p}</b> <small>(${S.grouped[p].length})</small></span><button class="btn bm" onclick="copyMP('${p}', this)">📩</button></div>`;
        });
        document.getElementById('msg-grid').innerHTML = m;
        document.getElementById('card-messages').style.display = 'block';
    }

    function getTroopBB(vTroops) {
        let trStr = '';
        const em = {spear:'🗡️',sword:'⚔️',axe:'🪓',spy:'🕵️',light:'🐴',heavy:'🏇',ram:'🔨',catapult:'🪨'};
        for(let k in S.troopConfig) {
            const qty = S.troopConfig[k] == 0 ? (vTroops[k] || 0) : S.troopConfig[k];
            if(qty > 0) trStr += `${qty}${em[k]} `;
        }
        return trStr;
    }

    function copyMP(p, btn) {
        let bb = `Hola [player]${p}[/player],\n\nTienes órdenes de ataque:\n\n[table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
        S.grouped[p].forEach(o => bb += `[*]${o.l}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${o.trStr}\n`);
        bb += `[/table]`;
        navigator.clipboard.writeText(bb);
        btn.innerText = "✅"; setTimeout(()=> btn.innerText = "📩", 2000);
    }

    function copyMultiMP() {
        let bb = '';
        document.querySelectorAll('.check-c:checked').forEach(c => {
            const p = c.getAttribute('data-p');
            bb += `[player]${p}[/player]\n[spoiler=Tus Órdenes][table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
            S.grouped[p].forEach(o => bb += `[*]${o.l}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${o.trStr}\n`);
            bb += `[/table][/spoiler]\n\n`;
        });
        navigator.clipboard.writeText(bb);
        alert("Mensajes combinados copiados.");
    }

    function copyFullForo() {
        let bb = `[b]📅 PLANIFICACIÓN ESTRATÉGICA[/b]\n\n`;
        Object.keys(S.grouped).sort().forEach(p => {
            bb += `[player]${p}[/player]\n[spoiler=Ver órdenes][table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
            S.grouped[p].forEach(o => bb += `[*]${o.l}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord][|]${o.trStr}\n`);
            bb += `[/table][/spoiler]\n\n`;
        });
        navigator.clipboard.writeText(bb);
        alert("Copiado para el foro.");
    }

    function resetUsed() { S.used.clear(); document.getElementById('count-used').innerText = "Pueblos usados: 0"; alert("Lista de OFFs reseteada."); }
    function selMsg(st) { document.querySelectorAll('.check-c').forEach(c => c.checked = st); }
    function el(id){return document.getElementById(id)}
    function buildGrid() {
        document.getElementById('unit-grid').innerHTML = UNITS.map(u => `
            <div class="ub ${u.id===S.uGuia?'sel':''}" onclick="S.uGuia='${u.id}';buildGrid()">
                <div style="font-size:1.2rem">${u.e}</div><div style="font-size:0.6rem">${u.nm}</div>
            </div>`).join('');
    }
    window.onload = () => { buildGrid(); document.getElementById('in-date').value = new Date().toISOString().split('T')[0]; };
</script>
</body>
</html>
