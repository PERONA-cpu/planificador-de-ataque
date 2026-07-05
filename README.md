<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador Guerras Tribales - Final Fix</title>
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
        .btn{padding:12px 20px;border-radius:8px;border:none;font-weight:700;cursor:pointer;transition:0.2s;display:inline-flex;align-items:center;justify-content:center;gap:8px}
        .bp{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#fff}
        .bp:hover{opacity:0.9;transform:translateY(-1px)}
        .ugrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(80px,1fr));gap:8px;margin-bottom:15px}
        .ub{background:var(--sur2);border:2px solid var(--brd);border-radius:8px;padding:8px;text-align:center;cursor:pointer}
        .ub.sel{border-color:var(--acc);background:rgba(124,106,247,.15)}
        table{width:100%;border-collapse:collapse;margin-top:15px}
        th{text-align:left;padding:12px;background:var(--sur2);color:var(--txt2);font-size:0.75rem}
        td{padding:10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .alert{padding:10px;border-radius:8px;margin-bottom:10px;font-size:0.85rem}
        .as{background:rgba(76,175,125,.1);color:var(--grn);border:1px solid var(--grn)}
    </style>
</head>
<body>

<div class="wrap">
    <h1>⚔️ Planificador de Guerras Tribales</h1>

    <div class="tabs">
        <button class="tab on" id="tab-trp" onclick="switchTab('trp')">💪 Tropas / Atacantes</button>
        <button class="tab" id="tab-fks" onclick="switchTab('fks')">🎭 Generador Fakes</button>
    </div>

    <!-- CONFIGURACIÓN DE MUNDO -->
    <div class="card">
        <div style="display:grid; grid-template-columns: 1fr 1fr 1fr; gap:15px; align-items:end">
            <div>
                <label>⚡ Velocidad del Mundo</label>
                <input type="number" id="world-unit-speed" value="1.0" step="0.1">
            </div>
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

    <!-- PESTAÑA TROPAS (PARA ATAQUE REAL) -->
    <div id="mode-trp">
        <div class="card">
            <label>Cargar Atacantes (Formato: Jugador 500|500)</label>
            <textarea id="ta-troops" rows="6" placeholder="Paco 500|500&#10;Juan 501|501"></textarea>
            <button class="btn bp" style="width:100%; margin-top:10px" onclick="loadAtacantes()">Cargar Jugadores Atacantes</button>
            <div id="m-trp" style="margin-top:10px"></div>
        </div>
    </div>

    <!-- PESTAÑA FAKES -->
    <div id="mode-fks" style="display:none">
        <div class="card">
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:15px">
                <div>
                    <label>🎯 Objetivos (Enemigos)</label>
                    <textarea id="ta-tgt-fk" rows="8" placeholder="Enemigo1 400|400&#10;Enemigo2 401|401"></textarea>
                </div>
                <div>
                    <label>💪 Atacantes (Tus pueblos)</label>
                    <textarea id="ta-atk-fk" rows="8" placeholder="MiPueblo1 500|500&#10;MiPueblo2 501|501"></textarea>
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

            <button class="btn bp" style="width:100%; margin-top:15px" onclick="calcFakes()">🚀 Generar Lista de Horarios</button>
        </div>

        <div id="fk-result-card" class="card" style="display:none">
            <h3 style="margin-bottom:10px">Horarios Calculados</h3>
            <div style="overflow-x:auto">
                <table>
                    <thead>
                        <tr>
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
</div>

<script>
    const UNITS=[
        {id:'spear',nm:'Lanza',spd:18,e:'🗡️'},{id:'sword',nm:'Espada',spd:22,e:'⚔️'},
        {id:'axe',nm:'Hacha',spd:18,e:'🪓'},{id:'spy',nm:'Espía',spd:9,e:'🕵️'},
        {id:'light',nm:'Ligera',spd:10,e:'🐴'},{id:'heavy',nm:'Pesada',spd:11,e:'🏇'},
        {id:'ram',nm:'Ariete',spd:30,e:'🔨'},{id:'snob',nm:'Noble',spd:35,e:'📜'}
    ];

    let S = { unitFk: 'ram', attackers: [], targets: [] };

    // --- CARGA DE MUNDOS (FIX ESPAÑA) ---
    async function twLoadWorlds() {
        const srv = document.getElementById('w-server').value;
        const wl = document.getElementById('w-world');
        const st = document.getElementById('w-status');
        if(!srv) return;
        
        const [prefix, domain, tws] = srv.split('_');
        wl.disabled = true;
        st.innerText = "⏳ Conectando con el servidor...";

        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://' + tws + '.twstats.com/')}`;
            const res = await fetch(url);
            const html = await res.text();
            const worlds = [];
            const regex = new RegExp(`data-world="${prefix}(\\d+|p\\d+|c\\d+|s\\d+)"`, 'g');
            let m;
            while ((m = regex.exec(html)) !== null) { if(!worlds.includes(m[1])) worlds.push(m[1]); }
            renderWorlds(worlds, prefix);
            st.innerText = "✅ Lista actualizada.";
        } catch(e) {
            if(prefix === 'es') {
                const fallback = ['104','103','102','101','100','99','98','p24','p23','c4'];
                renderWorlds(fallback, 'es');
                st.innerText = "⚠️ Servidor ocupado. Usando lista de emergencia.";
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
        } catch(e) { console.log("Error config. Usar manual."); }
    }

    // --- PARSER ---
    function parseInput(text) {
        const data = [];
        const lines = text.split('\n');
        lines.forEach(line => {
            const m = line.match(/^(.+?)\s+(\d{1,3})\|(\d{1,3})$/);
            if(m) data.push({ p: m[1].trim(), x: parseInt(m[2]), y: parseInt(m[3]) });
            else {
                const m2 = line.match(/(\d{1,3})\|(\d{1,3})/);
                if(m2) data.push({ p: 'Manual', x: parseInt(m2[1]), y: parseInt(m2[2]) });
            }
        });
        return data;
    }

    // --- LÓGICA TABS ---
    function switchTab(t) {
        document.getElementById('mode-trp').style.display = t==='trp'?'block':'none';
        document.getElementById('mode-fks').style.display = t==='fks'?'block':'none';
        document.getElementById('tab-trp').classList.toggle('on', t==='trp');
        document.getElementById('tab-fks').classList.toggle('on', t==='fks');
    }

    // --- GENERAR FAKES ---
    function calcFakes() {
        const tgts = parseInput(document.getElementById('ta-tgt-fk').value);
        const atks = parseInput(document.getElementById('ta-atk-fk').value);
        const date = document.getElementById('fk-date').value;
        const time = document.getElementById('fk-time').value;

        if(!date || !time || !atks.length || !tgts.length) return alert("Faltan datos (Atacantes, Objetivos, Fecha o Hora)");

        const arrivalMs = new Date(`${date}T${time}`).getTime();
        const unit = UNITS.find(u => u.id === S.unitFk);
        const wSpd = parseFloat(document.getElementById('world-unit-speed').value) || 1;
        const worldStr = document.getElementById('w-world').value || 'es100';

        let html = '';
        atks.forEach((a, index) => {
            // Asignación simple: Atacante 1 va al Objetivo 1, Atacante 2 al Objetivo 2...
            const t = tgts[index % tgts.length]; 
            const dist = Math.sqrt(Math.pow(a.x - t.x, 2) + Math.pow(a.y - t.y, 2));
            const travelMin = (dist * unit.spd) / wSpd;
            const launchDate = new Date(arrivalMs - travelMin * 60000);

            html += `<tr>
                <td style="color:var(--gold)">${launchDate.toLocaleString()}</td>
                <td><b>${a.p}</b></td>
                <td>${a.x}|${a.y}</td>
                <td><span style="color:var(--txt2)">→</span></td>
                <td>${t.p} (${t.x}|${t.y})</td>
                <td><button class="btn bp" style="padding:5px 10px; font-size:0.7rem" onclick="window.open('https://${worldStr}.guerrastribales.es/game.php?screen=place&target=${t.x}${t.y}')">🚀</button></td>
            </tr>`;
        });

        document.getElementById('fk-body').innerHTML = html;
        document.getElementById('fk-result-card').style.display = 'block';
    }

    function buildUnitGrid() {
        document.getElementById('ugrid-fakes').innerHTML = UNITS.map(u => `
            <div class="ub ${u.id===S.unitFk?'sel':''}" onclick="S.unitFk='${u.id}';buildUnitGrid()">
                <div style="font-size:1.2rem">${u.e}</div><div style="font-size:0.6rem">${u.nm}</div>
            </div>`).join('');
    }

    window.onload = () => {
        buildUnitGrid();
        document.getElementById('fk-date').value = new Date().toISOString().split('T')[0];
    };
</script>
</body>
</html>
