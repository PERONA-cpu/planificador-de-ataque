<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Planificador Guerras Tribales - Fix España</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#0f1117;--sur:#1a1d27;--sur2:#23263a;--brd:#2e3145;--acc:#7c6af7;--acc2:#5b8cf5;--gold:#f5c542;--grn:#4caf7d;--red:#e05252;--txt:#e8eaf6;--txt2:#8b90b0;--r:12px}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;padding:20px 14px}
.wrap{max-width:1200px;margin:0 auto;padding:0 4px}
h1{font-size:1.8rem;font-weight:700;background:linear-gradient(135deg,var(--acc),var(--gold));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;text-align:center;margin-bottom:4px}
.sub{text-align:center;color:var(--txt2);font-size:.87rem;margin-bottom:22px}
.tabs{display:flex;gap:8px;justify-content:center;margin-bottom:22px;flex-wrap:wrap}
.tab{padding:9px 22px;border-radius:8px;border:2px solid var(--brd);background:var(--sur);color:var(--txt2);font-family:'Inter',sans-serif;font-size:.88rem;font-weight:600;cursor:pointer;transition:all .2s}
.tab.on{border-color:var(--acc);color:var(--acc);background:rgba(124,106,247,.1)}
.tab:hover{border-color:var(--acc)}
.card{background:var(--sur);border:1px solid var(--brd);border-radius:var(--r);padding:18px;margin-bottom:14px}
.ct{font-size:.95rem;font-weight:600;margin-bottom:13px;display:flex;align-items:center;gap:8px}
.ico{width:24px;height:24px;border-radius:6px;background:linear-gradient(135deg,var(--acc),var(--acc2));display:flex;align-items:center;justify-content:center;font-size:12px;flex-shrink:0}
label{display:block;font-size:.73rem;font-weight:500;color:var(--txt2);margin-bottom:5px;text-transform:uppercase;letter-spacing:.05em}
input[type=text],input[type=number],input[type=date],input[type=time],textarea,select{width:100%;padding:8px 12px;background:var(--sur2);border:1px solid var(--brd);border-radius:8px;color:var(--txt);font-size:.87rem;font-family:'Inter',sans-serif;outline:none}
textarea{resize:vertical;font-family:'Courier New',monospace;font-size:.8rem}
.btn{display:inline-flex;align-items:center;justify-content:center;gap:6px;padding:8px 16px;border-radius:8px;border:none;font-weight:600;cursor:pointer;transition:all .2s}
.bp{background:linear-gradient(135deg,var(--acc),var(--acc2));color:#fff}
.bg{background:var(--sur2);color:var(--txt);border:1px solid var(--brd)}
.alert{padding:9px 13px;border-radius:8px;font-size:.82rem;margin-bottom:12px}
.as{background:rgba(76,175,125,.12);border:1px solid rgba(76,175,125,.3);color:#6de0a8}
.ae{background:rgba(224,82,82,.12);border:1px solid rgba(224,82,82,.3);color:#ff8080}
.ugrid{display:grid;grid-template-columns:repeat(auto-fill,minmax(90px,1fr));gap:6px;margin-bottom:12px}
.ub{background:var(--sur2);border:2px solid var(--brd);border-radius:9px;padding:7px 4px;cursor:pointer;text-align:center}
.ub.sel{border-color:var(--acc);background:rgba(124,106,247,.12)}
table{width:100%;border-collapse:collapse;font-size:.83rem}
th{background:var(--sur2);padding:9px;text-align:left;color:var(--txt2)}
td{padding:8px;border-bottom:1px solid rgba(46,49,69,.4)}
</style>
</head>
<body>

<div class="wrap">
    <h1>⚔️ Planificador Guerras Tribales</h1>
    <p class="sub">Gestión Estratégica · Versión Corregida (.es)</p>

    <div class="tabs">
        <button class="tab on" id="tab-trp" onclick="switchTab('trp')">💪 Tropas</button>
        <button class="tab" id="tab-atk" onclick="switchTab('atk')">⚔️ Ataques</button>
        <button class="tab" id="tab-fks" onclick="switchTab('fks')">🎭 Fakes</button>
    </div>

    <!-- CONFIG VELOCIDAD -->
    <div class="card">
        <div style="display:flex;align-items:center;gap:15px;flex-wrap:wrap">
            <span style="font-weight:600">⚡ Velocidad Mundo:</span>
            <input type="number" id="world-unit-speed" value="1" step="0.1" style="width:80px">
            
            <div style="display:flex; gap:10px; align-items:center">
                <span style="font-size:0.8rem; color:var(--txt2)">🌐 Servidor:</span>
                <select id="w-server" onchange="twLoadWorlds()" style="width:auto">
                    <option value="">Selecciona...</option>
                    <option value="es_guerrastribales.es_es">España (.es)</option>
                    <option value="en_tribalwars.net_www">Internacional (.net)</option>
                    <option value="pt_tribos.com.pt_pt">Portugal (.pt)</option>
                </select>
                <select id="w-world" onchange="twApplyConfig()" disabled style="width:auto">
                    <option value="">Mundos...</option>
                </select>
                <span id="w-status" style="font-size:0.75rem; color:var(--gold)"></span>
            </div>
        </div>
    </div>

    <!-- SECCIÓN TROPAS -->
    <div id="mode-trp">
        <div class="card">
            <div class="ct"><div class="ico">💪</div> Cargar Tropas (JSON o Manual)</div>
            <textarea id="ta-troops" rows="5" placeholder="Pega aquí el JSON o lista: Jugador 500|500"></textarea>
            <button class="btn bp" style="width:100%; margin-top:10px" onclick="analyzeTroops()">Cargar Atacantes</button>
            <div id="m-trp"></div>
        </div>
    </div>

    <!-- SECCIÓN ATAQUES -->
    <div id="mode-atk" style="display:none">
        <div class="card">
            <div class="ct"><div class="ico">🎯</div> Configurar Ofensiva</div>
            <div class="ugrid" id="ugrid-atk"></div>
            <div style="display:grid; grid-template-columns: 2fr 1fr 1fr; gap:12px">
                <div class="fg">
                    <label>Pueblos Objetivo (Ej: Paco 450|450)</label>
                    <textarea id="atk-tgt" rows="5" placeholder="Pega los objetivos aquí..."></textarea>
                </div>
                <div class="fg">
                    <label>Fecha Llegada</label>
                    <input type="date" id="atk-date">
                </div>
                <div class="fg">
                    <label>Hora Llegada</label>
                    <input type="time" id="atk-time" value="08:00:00" step="1">
                </div>
            </div>
            <button class="btn bp" style="width:100%; margin-top:10px" onclick="calcAtk()">Calcular Horarios</button>
            <div id="m-atk"></div>
        </div>
        <div id="atk-result" class="card" style="display:none">
            <table>
                <thead>
                    <tr><th>Lanzar el</th><th>Atacante</th><th>Desde</th><th>Hacia</th><th>🚀</th></tr>
                </thead>
                <tbody id="atk-body"></tbody>
            </table>
        </div>
    </div>

    <!-- SECCIÓN FAKES -->
    <div id="mode-fks" style="display:none">
        <div class="card">
            <div class="ct"><div class="ico">🎭</div> Generador de Fakes</div>
            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:12px">
                <div><label>Objetivos (Enemigos)</label><textarea id="ta-tgt-fk" rows="6" placeholder="Paco 400|400"></textarea></div>
                <div><label>Atacantes (Tus pueblos)</label><textarea id="ta-atk-fk" rows="6" placeholder="MiPueblo 500|500"></textarea></div>
            </div>
            <button class="btn bp" style="width:100%; margin-top:10px" onclick="calcFakes()">Generar Fakes</button>
            <div id="fk-result"></div>
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

    let S = { unitAtk: 'axe', troopData: [] };

    // --- FIX ESPAÑA: CARGA DE MUNDOS CON LISTA DE EMERGENCIA ---
    async function twLoadWorlds() {
        const srvValue = document.getElementById('w-server').value;
        const wl = document.getElementById('w-world');
        const st = document.getElementById('w-status');
        wl.innerHTML = '<option value="">Cargando...</option>';
        wl.disabled = true;

        if(!srvValue) return;
        const [prefix, domain, tws] = srvValue.split('_');

        // Controlador de tiempo para evitar que se quede colgado
        const controller = new AbortController();
        const timeoutId = setTimeout(() => controller.abort(), 3500);

        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://' + tws + '.twstats.com/')}`;
            const res = await fetch(url, { signal: controller.signal });
            const html = await res.text();
            
            const worlds = [];
            const regex = new RegExp(`data-world="${prefix}(\\d+|p\\d+|c\\d+|s\\d+)"`, 'g');
            let m;
            while ((m = regex.exec(html)) !== null) { if(!worlds.includes(m[1])) worlds.push(m[1]); }
            
            if(worlds.length === 0) throw new Error();
            renderWorldOptions(worlds, prefix);
            st.innerHTML = "✅ Mundos cargados.";
        } catch(e) {
            // LISTA DE EMERGENCIA SI FALLA LA WEB
            if(prefix === 'es') {
                const fallback = ['104','103','102','101','100','99','98','97','96','95','p24','p23','c4'];
                renderWorldOptions(fallback, 'es');
                st.innerHTML = "⚠️ Servidor lento. Cargando lista guardada.";
            } else {
                st.innerHTML = "❌ Error de conexión.";
            }
        }
    }

    function renderWorldOptions(worlds, prefix) {
        const wl = document.getElementById('w-world');
        wl.innerHTML = '<option value="">Mundos...</option>';
        worlds.sort((a,b) => b.localeCompare(a, undefined, {numeric: true})).forEach(w => {
            const opt = document.createElement('option');
            opt.value = prefix + w;
            opt.textContent = (prefix + w).toUpperCase();
            wl.appendChild(opt);
        });
        wl.disabled = false;
    }

    async function twApplyConfig() {
        const w = document.getElementById('w-world').value;
        const domain = document.getElementById('w-server').value.split('_')[1];
        try {
            const url = `https://api.allorigins.win/raw?url=${encodeURIComponent('https://' + w + '.' + domain + '/interface.php?func=get_config')}`;
            const res = await fetch(url);
            const xml = await res.text();
            const parser = new DOMParser();
            const doc = parser.parseFromString(xml, "text/xml");
            const speed = parseFloat(doc.querySelector('speed').textContent);
            const uSpeed = parseFloat(doc.querySelector('unit_speed').textContent);
            document.getElementById('world-unit-speed').value = (speed * uSpeed).toFixed(3);
        } catch(e) { console.log("Usando velocidad manual."); }
    }

    // --- PARSER MANUAL MEJORADO (Nombre + Coordenada) ---
    function parseManualInput(text) {
        const data = [];
        const lines = text.split('\n');
        for (let line of lines) {
            line = line.trim();
            if (!line) continue;
            // Busca cualquier nombre seguido de coordenadas
            const m = line.match(/^(.+?)\s+(\d{1,3})\|(\d{1,3})$/);
            if (m) {
                data.push({ player: m[1].trim(), x: parseInt(m[2]), y: parseInt(m[3]) });
            } else {
                const mOnly = line.match(/^(\d{1,3})\|(\d{1,3})$/);
                if (mOnly) data.push({ player: "Manual", x: parseInt(mOnly[1]), y: parseInt(mOnly[2]) });
            }
        }
        return data;
    }

    // --- LÓGICA GENERAL ---
    function switchTab(t) {
        ['trp','atk','fks'].forEach(id => {
            document.getElementById('mode-'+id).style.display = (t===id) ? 'block' : 'none';
            document.getElementById('tab-'+id).classList.toggle('on', t===id);
        });
    }

    function buildUnits() {
        document.getElementById('ugrid-atk').innerHTML = UNITS.map(u => `
            <div class="ub ${u.id===S.unitAtk?'sel':''}" onclick="S.unitAtk='${u.id}';buildUnits()">
                <div style="font-size:1.2rem">${u.e}</div><div style="font-size:0.7rem">${u.nm}</div>
            </div>`).join('');
    }

    function analyzeTroops() {
        const raw = document.getElementById('ta-troops').value.trim();
        try {
            S.troopData = JSON.parse(raw);
            document.getElementById('m-trp').innerHTML = `<div class="alert as">✅ ${S.troopData.length} pueblos cargados desde JSON.</div>`;
        } catch(e) {
            const manual = parseManualInput(raw);
            if(manual.length > 0) {
                S.troopData = manual;
                document.getElementById('m-trp').innerHTML = `<div class="alert as">✅ ${manual.length} pueblos manuales cargados con sus nombres.</div>`;
            } else {
                document.getElementById('m-trp').innerHTML = `<div class="alert ae">❌ Formato no reconocido.</div>`;
            }
        }
    }

    function calcAtk() {
        const date = document.getElementById('atk-date').value;
        const time = document.getElementById('atk-time').value;
        if(!date || !time) return alert("Pon fecha y hora");

        const tgts = parseManualInput(document.getElementById('atk-tgt').value);
        if(!tgts.length || !S.troopData.length) return alert("Faltan atacantes u objetivos.");

        const arrivalMs = new Date(`${date}T${time}`).getTime();
        const unit = UNITS.find(u => u.id === S.unitAtk);
        const wSpd = parseFloat(document.getElementById('world-unit-speed').value) || 1;

        let html = '';
        tgts.forEach(t => {
            S.troopData.forEach(orig => {
                const dist = Math.sqrt(Math.pow(orig.x - t.x, 2) + Math.pow(orig.y - t.y, 2));
                const launchMs = arrivalMs - ((dist * unit.spd) / wSpd) * 60000;
                const launch = new Date(launchMs);
                html += `<tr>
                    <td>${launch.toLocaleString()}</td>
                    <td>${orig.player}</td>
                    <td>${orig.x}|${orig.y}</td>
                    <td>${t.player} (${t.x}|${t.y})</td>
                    <td><button class="btn bg" onclick="window.open('https://${document.getElementById('w-server').value.split('_')[1]}/game.php?screen=place&target=${t.x}${t.y}')">🚀</button></td>
                </tr>`;
            });
        });
        document.getElementById('atk-body').innerHTML = html;
        document.getElementById('atk-result').style.display = 'block';
    }

    window.onload = () => { 
        buildUnits();
        const now = new Date();
        document.getElementById('atk-date').value = now.toISOString().split('T')[0];
    };
</script>
</body>
</html>
