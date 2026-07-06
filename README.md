<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Planificador Definitivo de Sarius</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800;900&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0b0e14;--sur:#161923;--brd:#2d3241;--acc:#7c6af7;--gold:#f5c542;--red:#ff4d4d;--blue:#4d94ff;--txt:#e8eaf6;--txt2:#a0a6c0}
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Inter',sans-serif;background:var(--bg);color:var(--txt);padding:20px}
        .header{text-align:center;margin-bottom:30px}
        .header h1{font-size:2.2rem;font-weight:900;color:var(--gold);text-transform:uppercase;letter-spacing:3px}
        .container{max-width:1300px;margin:0 auto;display:grid;grid-template-columns:1fr 1fr;gap:20px}
        .full{grid-column:1/-1}
        .card{background:var(--sur);border:1px solid var(--brd);border-radius:12px;padding:20px;height:100%}
        h2{font-size:0.9rem;text-transform:uppercase;color:var(--acc);margin-bottom:15px;display:flex;align-items:center;gap:8px}
        textarea{width:100%;background:#0b0e14;border:1px solid var(--brd);border-radius:8px;color:#00ff00;padding:12px;font-family:monospace;font-size:0.8rem;resize:vertical}
        
        /* CONTROLES */
        .config-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:15px;margin-bottom:15px}
        .input-group{display:flex;flex-direction:column;gap:5px}
        label{font-size:0.7rem;font-weight:bold;color:var(--txt2)}
        input, select{background:#0b0e14;border:1px solid var(--brd);padding:8px;color:#fff;border-radius:6px}

        /* COMPOSICIÓN DE TROPAS */
        .comp-box{background:rgba(0,0,0,0.3);padding:15px;border-radius:10px;border:1px dashed var(--brd);margin-top:10px}
        .unit-inputs{display:grid;grid-template-columns:repeat(auto-fill,minmax(100px,1fr));gap:10px}
        .u-in{display:flex;align-items:center;gap:5px;background:var(--sur);padding:5px;border-radius:5px;border:1px solid var(--brd)}
        .u-in input{width:100%;border:none;background:transparent;text-align:center}

        .btn-main{width:100%;padding:15px;background:linear-gradient(135deg,var(--acc),var(--blue));border:none;border-radius:8px;color:#fff;font-weight:900;cursor:pointer;font-size:1rem;margin-top:10px;transition:0.3s}
        .btn-main:hover{transform:translateY(-2px);box-shadow:0 5px 15px rgba(124,106,247,0.4)}

        /* RESULTADOS */
        .res-table{width:100%;border-collapse:collapse;margin-top:20px}
        .res-table th{background:var(--brd);padding:12px;font-size:0.7rem;text-align:left}
        .res-table td{padding:10px;border-bottom:1px solid var(--brd);font-size:0.85rem}
        .tr-tag{display:inline-block;padding:2px 6px;border-radius:4px;font-size:0.7rem;font-weight:800;margin-right:5px}
        .tag-real{background:var(--red);color:#fff}
        .tag-fake{background:var(--blue);color:#fff}
        .player-header{background:rgba(124,106,247,0.1);font-weight:900;color:var(--gold)}
        
        .pill{background:#000;border:1px solid #333;padding:2px 6px;border-radius:4px;font-size:0.75rem;margin:1px;display:inline-block}
        
        .msg-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:10px;margin-top:20px}
        .msg-card{background:var(--sur);border:1px solid var(--brd);padding:15px;border-radius:10px;display:flex;justify-content:space-between;align-items:center}
    </style>
</head>
<body>

<div class="wrap">
    <div class="header">
        <h1>Planificador Definitivo de Sarius</h1>
    </div>

    <div class="container">
        <!-- BLOQUE ATACANTES -->
        <div class="card">
            <h2>💪 Datos de la Tribu (JSON)</h2>
            <textarea id="in-alliance" rows="12" placeholder="Pega aquí el JSON del extractor de miembros..."></textarea>
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:10px">
                <div class="input-group">
                    <label>🪓 Mínimo Hachas para OFF</label>
                    <input type="number" id="min-axe" value="5000">
                </div>
                <div class="input-group">
                    <label>🔨 Mínimo Arietes para OFF</label>
                    <input type="number" id="min-ram" value="250">
                </div>
            </div>
        </div>

        <!-- BLOQUE OBJETIVOS -->
        <div class="card">
            <h2>🎯 Objetivos de la Operación</h2>
            <label>⚔️ Objetivos REALES (1 OFF por pueblo)</label>
            <textarea id="in-real" rows="5" placeholder="Enemigo 500|500"></textarea>
            <div style="margin-top:10px"></div>
            <label>🎭 Objetivos FAKES (Ataques de distracción)</label>
            <textarea id="in-fake" rows="5" placeholder="Enemigo 501|501"></textarea>
        </div>

        <!-- CONFIGURACIÓN DE TROPAS -->
        <div class="card full">
            <h2>🛡️ Configuración de Envío</h2>
            <div class="config-grid">
                <div class="input-group">
                    <label>⚡ Vel. Mundo</label>
                    <input type="number" id="world-speed" value="1.0" step="0.1">
                </div>
                <div class="input-group">
                    <label>📅 Fecha Llegada</label>
                    <input type="date" id="date-arr">
                </div>
                <div class="input-group">
                    <label>🕒 Hora Llegada</label>
                    <input type="time" id="time-arr" value="08:00:00" step="1">
                </div>
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
                    <label style="color:var(--red)">Tropas para Ataque REAL:</label>
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
                    <label style="color:var(--blue)">Tropas para FAKES:</label>
                    <div class="unit-inputs" style="margin-top:10px">
                        <div class="u-in">🪓<input type="number" id="fc-axe" value="0"></div>
                        <div class="u-in">🕵️<input type="number" id="fc-spy" value="1"></div>
                        <div class="u-in">🔨<input type="number" id="fc-ram" value="1"></div>
                    </div>
                </div>
            </div>
            
            <button class="btn-main" onclick="processPlan()">🚀 CALCULAR PLANIFICACIÓN ESTRATÉGICA</button>
        </div>

        <!-- MENSAJERÍA -->
        <div id="section-messages" class="card full" style="display:none">
            <h2>📩 Centro de Mensajería Individual</h2>
            <div class="msg-grid" id="msg-grid"></div>
        </div>

        <!-- TABLA FINAL -->
        <div id="section-results" class="card full" style="display:none">
            <div style="display:flex;justify-content:space-between;margin-bottom:15px">
                <h2>📋 Lista de Órdenes</h2>
                <button class="btn-main" style="width:auto;margin:0" onclick="copyForo()">📋 COPIAR PARA EL FORO</button>
            </div>
            <table class="res-table">
                <thead>
                    <tr>
                        <th>Lanzamiento</th>
                        <th>Origen</th>
                        <th>Objetivo</th>
                        <th>Tropas a enviar</th>
                        <th>Tipo</th>
                        <th>Acción</th>
                    </tr>
                </thead>
                <tbody id="res-body"></tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const UNITS_DATA = {
        spear:18, sword:22, axe:18, archer:18, spy:9, light:10, marcher:10, heavy:11, ram:30, catapult:30, snob:35
    };

    function toggleRealCustom(){
        document.getElementById('real-custom').style.display = document.getElementById('real-mode').value === 'custom' ? 'grid' : 'none';
    }

    function parseCoords(text) {
        const list = [];
        text.split('\n').forEach(line => {
            const m = line.match(/(\d{1,3})\|(\d{1,3})/);
            if(m) list.push({x: parseInt(m[1]), y: parseInt(m[2]), ck: m[0]});
        });
        return list;
    }

    function processPlan() {
        const rawAlliance = document.getElementById('in-alliance').value.trim();
        if(!rawAlliance) return alert("Pega el JSON de la tribu.");
        
        let allVillages = JSON.parse(rawAlliance);
        const realTargets = parseCoords(document.getElementById('in-real').value);
        const fakeTargets = parseCoords(document.getElementById('in-fake').value);
        
        const mAxe = parseInt(document.getElementById('min-axe').value);
        const mRam = parseInt(document.getElementById('min-ram').value);
        const vMundo = parseFloat(document.getElementById('world-speed').value) || 1;
        const arrivalMs = new Date(document.getElementById('date-arr').value + 'T' + document.getElementById('time-arr').value).getTime();

        if(isNaN(arrivalMs)) return alert("Fecha o hora inválida.");

        // SEPARAR OFFs DE FAKES
        let offPool = allVillages.filter(v => (v.axe || 0) >= mAxe && (v.ram || 0) >= mRam);
        let fakePool = allVillages.filter(v => !offPool.find(o => o.x === v.x && o.y === v.y));

        let results = [];
        let usedCks = new Set();

        // 1. ASIGNAR REALES
        realTargets.forEach(t => {
            if(offPool.length === 0) return;
            
            // Buscar el más cercano
            offPool.sort((a,b) => {
                const d1 = Math.sqrt(Math.pow(a.x-t.x,2)+Math.pow(a.y-t.y,2));
                const d2 = Math.sqrt(Math.pow(b.x-t.x,2)+Math.pow(b.y-t.y,2));
                return d1 - d2;
            });

            const winner = offPool.shift();
            usedCks.add(`${winner.x}|${winner.y}`);
            results.push(buildOrder(winner, t, 'REAL', arrivalMs, vMundo));
        });

        // 2. ASIGNAR FAKES
        fakeTargets.forEach((t, i) => {
            // Los fakes usan lo que NO sea OFF, o si no hay más, repiten (lógica de fakes)
            const availableForFake = allVillages; 
            const winner = availableForFake[i % availableForFake.length];
            results.push(buildOrder(winner, t, 'FAKE', arrivalMs, vMundo));
        });

        renderResults(results);
    }

    function buildOrder(v, t, type, arrMs, vMundo) {
        const dist = Math.sqrt(Math.pow(v.x-t.x,2)+Math.pow(v.y-t.y,2));
        const unit = document.getElementById('unit-guide').value;
        const travelMs = (dist * UNITS_DATA[unit] / vMundo) * 60000;
        const launch = new Date(arrMs - travelMs);

        // Lógica de Tropas Específicas
        let send = {};
        if(type === 'REAL') {
            if(document.getElementById('real-mode').value === 'all') {
                send = { axe: v.axe, light: v.light, ram: v.ram, catapult: v.catapult, snob: v.snob };
            } else {
                send = { axe: document.getElementById('rc-axe').value, light: document.getElementById('rc-light').value, ram: document.getElementById('rc-ram').value };
            }
        } else {
            send = { axe: document.getElementById('fc-axe').value, spy: document.getElementById('fc-spy').value, ram: document.getElementById('fc-ram').value };
        }

        return {
            player: v.player || v.p,
            origin: `${v.x}|${v.y}`,
            target: t.ck,
            launch: launch.toLocaleString(),
            type: type,
            troops: send,
            url: buildUrl(v, t, send)
        };
    }

    function buildUrl(v, t, tr) {
        let base = `https://es100.guerrastribales.es/game.php?village=${v.vid}&screen=place&target=${t.x}${t.y}`;
        for(let u in tr) if(tr[u]>0) base += `&${u}=${tr[u]}`;
        return base;
    }

    function renderResults(res) {
        const body = document.getElementById('res-body');
        const msgGrid = document.getElementById('msg-grid');
        body.innerHTML = ""; msgGrid.innerHTML = "";
        
        let grouped = {};
        res.forEach(r => {
            if(!grouped[r.player]) grouped[r.player] = [];
            grouped[r.player].push(r);
        });

        for(let p in grouped) {
            // Header jugador
            let hRow = document.createElement('tr');
            hRow.className = "player-header";
            hRow.innerHTML = `<td colspan="6">👤 JUGADOR: ${p}</td>`;
            body.appendChild(hRow);

            grouped[p].forEach(r => {
                let row = document.createElement('tr');
                let trHtml = "";
                for(let u in r.troops) if(r.troops[u]>0) trHtml += `<span class="pill">${r.troops[u]} ${u}</span>`;
                
                row.innerHTML = `
                    <td><b style="color:var(--gold)">${r.launch}</b></td>
                    <td>${r.origin}</td>
                    <td><b>${r.target}</b></td>
                    <td>${trHtml}</td>
                    <td><span class="tr-tag ${r.type==='REAL'?'tag-real':'tag-fake'}">${r.type}</span></td>
                    <td><button class="btn-main" style="padding:5px 10px" onclick="window.open('${r.url}')">🚀</button></td>
                `;
                body.appendChild(row);
            });

            // Card Mensaje
            let mCard = document.createElement('div');
            mCard.className = "msg-card";
            mCard.innerHTML = `
                <span><b>${p}</b> (${grouped[p].length} órden/es)</span>
                <button class="btn-main" style="background:var(--acc2)" onclick="copyMP('${p}')">📩 COPIAR MP</button>
            `;
            msgGrid.appendChild(mCard);
        }

        document.getElementById('section-messages').style.display = "block";
        document.getElementById('section-results').style.display = "block";
        window.scrollTo({top: document.getElementById('section-messages').offsetTop, behavior:'smooth'});
        
        // Guardar para uso global
        window.currentResults = grouped;
    }

    function copyMP(p) {
        const orders = window.currentResults[p];
        let bb = `Hola [player]${p}[/player],\n\nTienes órdenes de ataque coordinado:\n\n[table]\n[**]Tipo[||]Lanzar[||]Origen[||]Objetivo[||]Tropas[/**]\n`;
        orders.forEach(o => {
            let trText = "";
            for(let u in o.troops) if(o.troops[u]>0) trText += `${o.troops[u]} [unit]${u}[/unit] `;
            const type = o.type === 'REAL' ? '[color=#ff0000][b]REAL[/b][/color]' : 'FAKE';
            bb += `[*]${type}[|]${o.launch}[|][coord]${o.origin}[/coord][|][coord]${o.target}[/coord][|]${trText}\n`;
        });
        bb += `[/table]\nConfirma al terminar. ¡Gracias!`;
        navigator.clipboard.writeText(bb);
        alert("Órdenes de " + p + " copiadas al portapapeles.");
    }

    function copyForo() {
        let bb = "[b]📅 PLANIFICACIÓN ESTRATÉGICA[/b]\n\n";
        for(let p in window.currentResults) {
            bb += `[player]${p}[/player]\n[spoiler=Ver órdenes][table]\n[**]Tipo[||]Lanzar[||]Origen[||]Objetivo[/**]\n`;
            window.currentResults[p].forEach(o => {
                const type = o.type === 'REAL' ? '[color=#ff0000]REAL[/color]' : 'FAKE';
                bb += `[*]${type}[|]${o.launch}[|][coord]${o.origin}[/coord][|][coord]${o.target}[/coord]\n`;
            });
            bb += `[/table][/spoiler]\n\n`;
        }
        navigator.clipboard.writeText(bb);
        alert("BBCode completo para el foro copiado.");
    }

    window.onload = () => {
        document.getElementById('date-arr').value = new Date().toISOString().split('T')[0];
    };
</script>
</body>
</html>
