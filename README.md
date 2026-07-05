<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador Guerras Tribales - Export BBCode</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0f1117;--sur:#1a1d27;--sur2:#23263a;--brd:#2e3145;--acc:#7c6af7;--acc2:#5b8cf5;--gold:#f5c542;--grn:#4caf7d;--red:#e05252;--txt:#e8eaf6;--txt2:#8b90b0;--r:12px}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;padding:20px 14px}
        .wrap{max-width:1100px;margin:0 auto}
        h1{font-size:1.8rem;font-weight:700;background:linear-gradient(135deg,var(--acc),var(--gold));-webkit-background-clip:text;-webkit-text-fill-color:transparent;text-align:center;margin-bottom:20px}
        .tabs{display:flex;gap:8px;justify-content:center;margin-bottom:20px}
        .tab{padding:10px 20px;border-radius:8px;border:2px solid var(--brd);background:var(--sur);color:var(--txt2);font-weight:600;cursor:pointer;transition:0.2s}
        .tab.on{border-color:var(--acc);color:var(--acc);background:rgba(124,106,247,.1)}
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:var(--r);padding:20px;margin-bottom:15px}
        label{display:block;font-size:.75rem;color:var(--txt2);margin-bottom:5px;text-transform:uppercase}
        input, textarea, select{width:100%;padding:10px;background:var(--sur2);border:1px solid var(--brd);border-radius:8px;color:var(--txt);font-family:inherit;outline:none}
        textarea{font-family:monospace;font-size:0.85rem}
        .btn{padding:10px 16px;border-radius:8px;border:none;font-weight:700;cursor:pointer;transition:0.2s;display:inline-flex;align-items:center;justify-content:center;gap:8px;font-size: 0.85rem;}
        .bp{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#fff}
        .bs{background:var(--gold);color:#000}
        .bg{background:var(--sur2);color:var(--txt);border:1px solid var(--brd)}
        .btn:hover{opacity:0.9;transform:translateY(-1px)}
        .ugrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(80px,1fr));gap:8px;margin-bottom:15px}
        .ub{background:var(--sur2);border:2px solid var(--brd);border-radius:8px;padding:8px;text-align:center;cursor:pointer}
        .ub.sel{border-color:var(--acc);background:rgba(124,106,247,.15)}
        table{width:100%;border-collapse:collapse;margin-top:15px}
        th{text-align:left;padding:12px;background:var(--sur2);color:var(--txt2);font-size:0.75rem}
        td{padding:10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
    </style>
</head>
<body>

<div class="wrap">
    <h1>⚔️ Planificador de Guerras Tribales</h1>

    <div class="tabs">
        <button class="tab on" id="tab-fks" onclick="switchTab('fks')">🎭 Generador Fakes</button>
        <button class="tab" id="tab-info">ℹ️ Instrucciones</button>
    </div>

    <!-- CONFIGURACIÓN DE MUNDO -->
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
            <div>
                <label>🌍 Mundo</label>
                <select id="w-world" onchange="twApplyConfig()" disabled><option value="">Mundos...</option></select>
            </div>
        </div>
        <div id="w-status" style="font-size:0.7rem; margin-top:5px; color:var(--gold)"></div>
    </div>

    <!-- GENERADOR DE FAKES -->
    <div id="mode-fks">
        <div class="card">
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px">
                <div>
                    <label>🎯 Objetivos (Enemigos)</label>
                    <textarea id="ta-tgt-fk" rows="8" placeholder="Enemigo1 400|400&#10;Enemigo2 401|401"></textarea>
                </div>
                <div>
                    <label>💪 Atacantes (Aliados)</label>
                    <textarea id="ta-atk-fk" rows="8" placeholder="Aliado1 500|500&#10;Aliado2 501|501"></textarea>
                </div>
            </div>
            
            <div style="margin-top:15px">
                <label>Unidad más lenta:</label>
                <div class="ugrid" id="ugrid-fakes"></div>
            </div>

            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px; margin-top:10px">
                <div><label>Fecha Llegada</label><input type="date" id="fk-date"></div>
                <div><label>Hora Llegada</label><input type="time" id="fk-time" value="08:00:00" step="1"></div>
            </div>

            <button class="btn bp" style="width:100%; margin-top:15px" onclick="calcFakes()">🚀 Generar Lista de Fakes</button>
        </div>

        <!-- RESULTADOS -->
        <div id="fk-result-card" class="card" style="display:none">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px">
                <h3 style="margin:0">Horarios Calculados</h3>
                <button class="btn bs" onclick="copyAllBB()">📋 Copiar Todo (Foro)</button>
            </div>
            <div style="overflow-x:auto">
                <table>
                    <thead>
                        <tr>
                            <th>Lanzar el</th>
                            <th>Atacante</th>
                            <th>Origen</th>
                            <th>Hacia</th>
                            <th>Objetivo</th>
                            <th>Órdenes</th>
                            <th>🚀</th>
                        </tr>
                    </thead>
                    <tbody id="fk-body"></tbody>
                </table>
            </div>
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

    let S = { unitFk: 'ram', results: [] };

    // --- SISTEMA DE CARGA DE MUNDOS ---
    async function twLoadWorlds() {
        const srv = document.getElementById('w-server').value;
        const wl = document.getElementById('w-world');
        const st = document.getElementById('w-status');
        if(!srv) return;
        const [prefix, domain, tws] = srv.split('_');
        wl.disabled = true;
        st.innerText = "⏳ Cargando...";
        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://' + tws + '.twstats.com/')}`;
            const res = await fetch(url);
            const html = await res.text();
            const worlds = [];
            const regex = new RegExp(`data-world="${prefix}(\\d+|p\\d+|c\\d+|s\\d+)"`, 'g');
            let m;
            while ((m = regex.exec(html)) !== null) { if(!worlds.includes(m[1])) worlds.push(m[1]); }
            renderWorlds(worlds, prefix);
            st.innerText = "✅ Listo";
        } catch(e) {
            if(prefix === 'es') {
                renderWorlds(['104','103','102','101','100','99','98','p24','c4'], 'es');
                st.innerText = "⚠️ Modo Emergencia Activo";
            }
        }
    }

    function renderWorlds(list, prefix) {
        const wl = document.getElementById('w-world');
        wl.innerHTML = '<option value="">Mundos...</option>';
        list.sort((a,b)=>b.localeCompare(a, undefined, {numeric:true})).forEach(w=>{
            let opt = document.createElement('option');
            opt.value = prefix+w; opt.innerText = (prefix+w).toUpperCase();
            wl.appendChild(opt);
        });
        wl.disabled = false;
    }

    async function twApplyConfig() {
        const w = document.getElementById('w-world').value;
        const domain = document.getElementById('w-server').value.split('_')[1];
        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://'+w+'.'+domain+'/interface.php?func=get_config')}`;
            const res = await fetch(url);
            const xml = await res.text();
            const parser = new DOMParser();
            const doc = parser.parseFromString(xml, "text/xml");
            const s = parseFloat(doc.querySelector('speed').textContent);
            const us = parseFloat(doc.querySelector('unit_speed').textContent);
            document.getElementById('world-unit-speed').value = (s * us).toFixed(3);
        } catch(e) { console.log("Manual"); }
    }

    // --- LOGICA DE CALCULOS ---
    function parseInput(text) {
        const data = [];
        text.split('\n').forEach(line => {
            const m = line.match(/^(.+?)\s+(\d{1,3})\|(\d{1,3})$/);
            if(m) data.push({ p: m[1].trim(), x: parseInt(m[2]), y: parseInt(m[3]) });
            else {
                const m2 = line.match(/(\d{1,3})\|(\d{1,3})/);
                if(m2) data.push({ p: 'Manual', x: parseInt(m2[1]), y: parseInt(m2[2]) });
            }
        });
        return data;
    }

    function calcFakes() {
        const tgts = parseInput(document.getElementById('ta-tgt-fk').value);
        const atks = parseInput(document.getElementById('ta-atk-fk').value);
        const date = document.getElementById('fk-date').value;
        const time = document.getElementById('fk-time').value;
        if(!date || !time || !atks.length || !tgts.length) return alert("Faltan datos");

        const arrivalMs = new Date(`${date}T${time}`).getTime();
        const unit = UNITS.find(u => u.id === S.unitFk);
        const wSpd = parseFloat(document.getElementById('world-unit-speed').value) || 1;
        const worldStr = document.getElementById('w-world').value || 'es100';

        S.results = [];
        atks.forEach((a, i) => {
            const t = tgts[i % tgts.length]; 
            const dist = Math.sqrt(Math.pow(a.x - t.x, 2) + Math.pow(a.y - t.y, 2));
            const travelMin = (dist * unit.spd) / wSpd;
            const launchDate = new Date(arrivalMs - travelMin * 60000);

            S.results.push({
                launch: launchDate,
                attacker: a.p,
                origin: `${a.x}|${a.y}`,
                target: `${t.x}|${t.y}`,
                targetP: t.p,
                unit: unit.e,
                url: `https://${worldStr}.guerrastribales.es/game.php?screen=place&target=${t.x}${t.y}`
            });
        });

        renderTable();
    }

    function renderTable() {
        let html = '';
        S.results.forEach((r, i) => {
            html += `<tr>
                <td style="color:var(--gold)">${r.launch.toLocaleString()}</td>
                <td><b>${r.attacker}</b></td>
                <td>${r.origin}</td>
                <td>→</td>
                <td>${r.targetP} (${r.target})</td>
                <td><button class="btn bg" onclick="copySingleBB(${i})">Copiar</button></td>
                <td><button class="btn bp" style="padding:5px 10px" onclick="window.open('${r.url}')">🚀</button></td>
            </tr>`;
        });
        document.getElementById('fk-body').innerHTML = html;
        document.getElementById('fk-result-card').style.display = 'block';
    }

    // --- SISTEMA DE COPIADO BBCODE ---

    function copySingleBB(index) {
        const r = S.results[index];
        const bb = `[b]Lanzar:[/b] ${r.launch.toLocaleString()} [b]Desde:[/b] [coord]${r.origin}[/coord] [b]Hacia:[/b] [coord]${r.target}[/coord] [b]Unidad:[/b] ${r.unit}`;
        navigator.clipboard.writeText(bb);
        alert("Órden copiada para " + r.attacker);
    }

    function copyAllBB() {
        const grouped = {};
        S.results.forEach(r => {
            if(!grouped[r.attacker]) grouped[r.attacker] = [];
            grouped[r.attacker].push(r);
        });

        let bb = `[b]📅 PLANIFICACIÓN DE FAKES[/b]\n\n`;
        for (const player in grouped) {
            bb += `[player]${player}[/player]\n[spoiler=Tus fakes][table]\n[**]Lanzar[||]Origen[||]Objetivo[||]Unidad[/**]\n`;
            grouped[player].forEach(r => {
                bb += `[*]${r.launch.toLocaleString()}[|][coord]${r.origin}[/coord][|][coord]${r.target}[/coord][|]${r.unit}\n`;
            });
            bb += `[/table][/spoiler]\n\n`;
        }
        
        navigator.clipboard.writeText(bb);
        alert("BBCode total copiado al portapapeles. ¡Listo para pegar en el foro!");
    }

    function buildUnitGrid() {
        document.getElementById('ugrid-fakes').innerHTML = UNITS.map(u => `
            <div class="ub ${u.id===S.unitFk?'sel':''}" onclick="S.unitFk='${u.id}';buildUnitGrid()">
                <div style="font-size:1.2rem">${u.e}</div><div style="font-size:0.6rem">${u.nm}</div>
            </div>`).join('');
    }

    function switchTab(t) {
        document.getElementById('mode-fks').style.display = 'block';
    }

    window.onload = () => {
        buildUnitGrid();
        document.getElementById('fk-date').value = new Date().toISOString().split('T')[0];
    };
</script>
</body>
</html>
