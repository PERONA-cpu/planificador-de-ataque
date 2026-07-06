<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificador Supremo de Sarius</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        :root{--bg:#0b0e14;--sur:#161923;--sur2:#1e2230;--brd:#2d3241;--acc:#7c6af7;--gold:#f5c542;--red:#ff4d4d;--blue:#4d94ff;--magenta:#ff00ff;--txt:#e8eaf6;--txt2:#a0a6c0;--r:12px}
        *{margin:0;padding:0;box-sizing:border-box}
        
        /* Reset para evitar el error del texto superior */
        html, body { background: var(--bg); color: var(--txt); font-family: 'Inter', sans-serif; overflow-x: hidden; }

        .wrap { max-width: 1300px; margin: 0 auto; padding: 20px; }
        
        /* HEADER ESTILO FOTO */
        .header { text-align: center; margin-bottom: 30px; border-bottom: 1px solid var(--brd); padding-bottom: 20px; }
        .header h1 { font-size: 2.2rem; font-weight: 900; color: var(--gold); text-transform: uppercase; letter-spacing: 4px; }
        .header p { color: var(--txt2); font-size: 0.8rem; margin-top: 5px; letter-spacing: 1px; }

        .container { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
        .full { grid-column: 1 / -1; }

        .card { background: var(--sur); border: 1px solid var(--brd); border-radius: var(--r); padding: 20px; box-shadow: 0 4px 30px rgba(0,0,0,0.5); }
        h2 { font-size: 0.85rem; text-transform: uppercase; color: var(--acc); margin-bottom: 15px; display: flex; align-items: center; gap: 10px; font-weight: 800; }
        .ico { width: 24px; height: 24px; border-radius: 6px; background: linear-gradient(135deg, var(--acc), var(--blue)); display: flex; align-items: center; justify-content: center; font-size: 12px; color: #fff; }

        label { display: block; font-size: 0.65rem; color: var(--txt2); margin-bottom: 6px; text-transform: uppercase; font-weight: 700; }
        textarea { width: 100%; background: #080a0f; border: 1px solid var(--brd); border-radius: 8px; color: #00ff88; padding: 12px; font-family: 'Courier New', monospace; font-size: 0.8rem; outline: none; resize: vertical; }
        input, select { background: #080a0f; border: 1px solid var(--brd); padding: 10px; color: #fff; border-radius: 6px; font-size: 0.85rem; outline: none; width: 100%; }

        /* FILTROS DE TROPAS */
        .filter-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-top: 10px; }
        .comp-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 15px; }
        .comp-box { background: rgba(0,0,0,0.2); padding: 15px; border-radius: 10px; border: 1px solid var(--brd); }
        .unit-inputs { display: grid; grid-template-columns: repeat(auto-fill, minmax(80px, 1fr)); gap: 8px; margin-top: 10px; }
        .u-in { display: flex; align-items: center; gap: 5px; background: var(--sur2); padding: 5px; border-radius: 4px; border: 1px solid #333; }
        .u-in input { padding: 2px; border: none; text-align: center; background: transparent; font-weight: 800; color: var(--gold); }

        /* BOTÓN GENERAR */
        .btn-main { width: 100%; padding: 20px; background: linear-gradient(135deg, var(--acc), var(--blue)); border: none; border-radius: 10px; color: #fff; font-weight: 900; cursor: pointer; font-size: 1.1rem; margin-top: 10px; text-transform: uppercase; transition: 0.3s; }
        .btn-main:hover { transform: translateY(-2px); box-shadow: 0 10px 30px rgba(124, 106, 247, 0.4); }

        /* TABLAS GRUPO JUGADOR */
        .res-table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        .res-table th { text-align: left; padding: 12px; background: #11141d; color: var(--txt2); font-size: 0.7rem; }
        .res-table td { padding: 12px; border-bottom: 1px solid var(--brd); font-size: 0.85rem; }
        .player-head { background: rgba(124, 106, 247, 0.1); color: var(--gold); font-weight: 900; }
        
        .pill { background: #000; padding: 3px 8px; border-radius: 5px; border: 1px solid #333; font-size: 0.75rem; font-weight: 800; margin: 1px; display: inline-flex; align-items: center; gap: 3px; }
        .tag { padding: 2px 6px; border-radius: 4px; font-size: 0.65rem; font-weight: 900; color: #fff; }
        .tag-real { background: var(--red); }
        .tag-fake { background: var(--blue); }
        .tag-conq { background: var(--magenta); }

        .msg-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 15px; margin-top: 20px; }
        .msg-card { background: var(--sur2); border: 1px solid var(--brd); padding: 15px; border-radius: 10px; display: flex; justify-content: space-between; align-items: center; border-left: 5px solid var(--gold); }
    </style>
</head>
<body>

<div class="wrap">
    <div class="header">
        <h1>PLANIFICADOR DEFINITIVO DE SARIUS</h1>
        <p>COORDINACIÓN ESTRATÉGICA DE ALTO NIVEL</p>
    </div>

    <div class="container">
        <!-- 1. ENTRADA DE TROPAS -->
        <div class="card">
            <h2><div class="ico">💪</div> Datos de la Tribu (OFF + NOBLES)</h2>
            <label>Pega el JSON de tropas aquí:</label>
            <textarea id="in-troops" rows="12" placeholder="Pega el JSON del extractor..."></textarea>
            
            <div class="filter-row">
                <div class="input-group"><label>🪓 Mín. Hachas</label><input type="number" id="f-axe" value="4000"></div>
                <div class="input-group"><label>🔨 Mín. Arietes</label><input type="number" id="f-ram" value="250"></div>
                <div class="input-group"><label>🛡️ Proximidad (Seguridad)</label><input type="number" id="f-prox" value="0" title="Excluir atacantes a menos de X campos del enemigo"></div>
                <div class="input-group"><label>⚡ Vel. Mundo</label><input type="number" id="f-speed" value="1.0" step="0.1"></div>
            </div>
        </div>

        <!-- 2. OBJETIVOS -->
        <div class="card">
            <h2><div class="ico" style="background:var(--red)">🎯</div> Objetivos de la operación</h2>
            <label style="color:var(--magenta)">🏰 Conquistas (Noble):</label>
            <textarea id="obj-conq" rows="4" placeholder="Enemigo 400|400"></textarea>
            <label style="color:var(--red); margin-top:10px">🔥 Reales (OFF):</label>
            <textarea id="obj-real" rows="4" placeholder="Enemigo 450|450"></textarea>
            <label style="color:var(--blue); margin-top:10px">🎭 Fakes:</label>
            <textarea id="obj-fake" rows="4" placeholder="Enemigo 460|460"></textarea>
        </div>

        <!-- 3. CONFIGURACIÓN DE ENVÍO -->
        <div class="card full">
            <h2><div class="ico">⚙️</div> Configuración de Envío</h2>
            <div class="filter-row">
                <div class="input-group"><label>📅 Fecha Llegada</label><input type="date" id="date-arr"></div>
                <div class="input-group"><label>🕒 Hora Llegada</label><input type="time" id="time-arr" value="08:00:00" step="1"></div>
                <div class="input-group">
                    <label>📏 Unidad Guía</label>
                    <select id="unit-guide">
                        <option value="ram">🔨 Ariete (30m)</option>
                        <option value="snob">📜 Noble (35m)</option>
                        <option value="axe">🪓 Hacha (18m)</option>
                        <option value="spy">🕵️ Espía (9m)</option>
                    </select>
                </div>
            </div>

            <div class="comp-grid">
                <div class="comp-box">
                    <label style="color:var(--magenta)">Tropas para Conquista/Real:</label>
                    <select id="real-mode" onchange="toggleC(this.value)">
                        <option value="all">Mandar TODO lo ofensivo (automático)</option>
                        <option value="fix">Mandar cantidad fija ↓</option>
                    </select>
                    <div id="comp-real" class="unit-inputs" style="display:none">
                        <div class="u-in">🪓<input type="number" id="cr-axe" value="6000"></div>
                        <div class="u-in">🐴<input type="number" id="cr-light" value="3000"></div>
                        <div class="u-in">🔨<input type="number" id="cr-ram" value="300"></div>
                    </div>
                </div>
                <div class="comp-box">
                    <label style="color:var(--blue)">Tropas para Fakes:</label>
                    <div class="unit-inputs">
                        <div class="u-in">🪓<input type="number" id="cf-axe" value="0"></div>
                        <div class="u-in">🕵️<input type="number" id="cf-spy" value="1"></div>
                        <div class="u-in">🔨<input type="number" id="cf-ram" value="1"></div>
                    </div>
                </div>
            </div>
            
            <button class="btn-main" onclick="calc()">🚀 Generar Planificación Estratégica</button>
        </div>

        <!-- MENSAJES -->
        <div id="sec-msg" class="card full" style="display:none">
            <h2>📩 Mensajería por Jugador</h2>
            <div class="msg-grid" id="msg-grid"></div>
        </div>

        <!-- TABLA -->
        <div id="sec-res" class="card full" style="display:none">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:15px">
                <h2>📋 Órdenes de Salida</h2>
                <button class="btn-main" style="width:auto; margin:0; padding:10px 20px" onclick="copyForo()">📋 Copiar para el Foro</button>
            </div>
            <div style="overflow-x:auto">
                <table class="res-table">
                    <thead>
                        <tr><th>Lanzamiento</th><th>Origen</th><th>Hacia</th><th>Objetivo</th><th>Tropas</th><th>Tipo</th><th>🚀</th></tr>
                    </thead>
                    <tbody id="res-body"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<script>
    const UNITS_SPEED = { spear:18, sword:22, axe:18, archer:18, spy:9, light:10, marcher:10, heavy:11, ram:30, catapult:30, snob:35 };

    function toggleC(v){ document.getElementById('comp-real').style.display = v==='fix'?'grid':'none'; }

    function parseCoords(txt) {
        const list = [];
        txt.split('\n').forEach(l => {
            const m = l.match(/(\d{1,3})\|(\d{1,3})/);
            if(m) list.push({x:parseInt(m[1]), y:parseInt(m[2]), ck:m[0]});
        });
        return list;
    }

    function dist(a, b) { return Math.sqrt(Math.pow(a.x-b.x,2)+Math.pow(a.y-b.y,2)); }

    function calc() {
        const raw = document.getElementById('in-troops').value.trim();
        if(!raw) return alert("Pega los datos de tropas.");
        
        const allAtks = JSON.parse(raw).map(v => ({
            p: v.player || v.p, x: v.x, y: v.y, ck: `${v.x}|${v.y}`, vid: v.vid,
            troops: { axe:v.axe||0, light:v.light||0, ram:v.ram||0, snob:v.snob||0, spy:v.spy||0, catapult:v.catapult||0 }
        }));

        const cT = parseCoords(document.getElementById('obj-conq').value);
        const rT = parseCoords(document.getElementById('obj-real').value);
        const fT = parseCoords(document.getElementById('obj-fake').value);

        const mAxe = parseInt(document.getElementById('f-axe').value);
        const mRam = parseInt(document.getElementById('f-ram').value);
        const mProx = parseFloat(document.getElementById('f-prox').value);
        const vM = parseFloat(document.getElementById('f-speed').value) || 1;
        const arrivalMs = new Date(document.getElementById('date-arr').value + 'T' + document.getElementById('time-arr').value).getTime();

        let used = new Set();
        let results = [];

        // 1. CONQUISTAS (Solo pueblos con Nobles)
        let noblePool = allAtks.filter(v => v.troops.snob > 0);
        cT.forEach(t => {
            if(!noblePool.length) return;
            noblePool.sort((a,b) => dist(a,t) - dist(b,t));
            const w = noblePool.shift();
            used.add(w.ck);
            results.push(createRow(w, t, 'CONQUISTA', arrivalMs, vM, 'snob'));
        });

        // 2. REALES (Filtro OFF y Proximidad)
        let offPool = allAtks.filter(v => {
            if(used.has(v.ck)) return false;
            const meetsTroops = v.troops.axe >= mAxe && v.troops.ram >= mRam;
            const meetsProx = !rT.some(t => dist(v, t) < mProx);
            return meetsTroops && meetsProx;
        });

        rT.forEach(t => {
            if(!offPool.length) return;
            offPool.sort((a,b) => dist(a,t) - dist(b,t));
            const w = offPool.shift();
            used.add(w.ck);
            results.push(createRow(w, t, 'REAL', arrivalMs, vM, document.getElementById('unit-guide').value));
        });

        // 3. FAKES (Lo que quede)
        const fakePool = allAtks.filter(v => !used.has(v.ck));
        fT.forEach((t, i) => {
            if(!fakePool.length) return;
            const w = fakePool[i % fakePool.length];
            results.push(createRow(w, t, 'FAKE', arrivalMs, vM, document.getElementById('unit-guide').value));
        });

        render(results);
    }

    function createRow(v, t, type, arrMs, vM, uG) {
        const d = dist(v, t);
        const lDate = new Date(arrMs - (d * UNITS_SPEED[uG] / vM) * 60000);
        
        let send = {};
        if(type === 'CONQUISTA') send = { snob: v.troops.snob, axe: 1000, light: 500 };
        else if(type === 'REAL') {
            if(document.getElementById('real-mode').value === 'all') send = v.troops;
            else send = { axe: document.getElementById('cr-axe').value, light: document.getElementById('cr-light').value, ram: document.getElementById('cr-ram').value };
        } else {
            send = { axe: document.getElementById('cf-axe').value, spy: document.getElementById('cf-spy').value, ram: document.getElementById('cf-ram').value };
        }

        let url = `https://es100.guerrastribales.es/game.php?village=${v.vid}&screen=place&target=${t.x}${t.y}`;
        for(let k in send) if(send[k]>0) url += `&${k}=${send[k]}`;

        return { player: v.p, orig: v.ck, dest: t.ck, launch: lDate.toLocaleString(), type, url, troops: send };
    }

    function render(res) {
        const body = document.getElementById('res-body');
        const grid = document.getElementById('msg-grid');
        body.innerHTML = ""; grid.innerHTML = "";
        
        let grouped = {};
        res.forEach(r => { if(!grouped[r.player]) grouped[r.player] = []; grouped[r.player].push(r); });

        for(let p in grouped) {
            body.innerHTML += `<tr class="player-head"><td colspan="7">👤 JUGADOR: ${p}</td></tr>`;
            grouped[p].forEach(r => {
                let trs = "";
                const icons = {axe:'🪓',light:'🐴',ram:'🔨',snob:'📜',spy:'🕵️'};
                for(let k in r.troops) if(r.troops[k]>0) trs += `<span class="pill">${r.troops[k]}${icons[k]||k}</span>`;
                const tagCls = r.type === 'CONQUISTA' ? 'tag-conq' : (r.type === 'REAL' ? 'tag-real' : 'tag-fake');
                body.innerHTML += `<tr><td>${r.launch}</td><td>${r.orig}</td><td>→</td><td><b>${r.dest}</b></td><td>${trs}</td><td><span class="tag ${tagCls}">${r.type}</span></td><td><button class="btn bp" style="padding:4px" onclick="window.open('${r.url}')">🚀</button></td></tr>`;
            });
            grid.innerHTML += `<div class="msg-card"><span><b>${p}</b> (${grouped[p].length})</span><button class="btn bm" onclick="copyMP('${p}')">📩 COPIAR MP</button></div>`;
        }
        document.getElementById('sec-msg').style.display = "block";
        document.getElementById('sec-res').style.display = "block";
        window.currentPlan = grouped;
    }

    function copyMP(p) {
        let bb = `Hola [player]${p}[/player],\n\n[table]\n[**]Tipo[||]Lanzar[||]Origen[||]Destino[/**]\n`;
        window.currentPlan[p].forEach(o => bb += `[*]${o.type}[|]${o.launch}[|][coord]${o.orig}[/coord][|][coord]${o.dest}[/coord]\n`);
        bb += `[/table]`;
        navigator.clipboard.writeText(bb);
        alert("Copiado.");
    }

    function copyForo() {
        let bb = "[b]📅 PLANIFICACIÓN ESTRATÉGICA[/b]\n\n";
        for(let p in window.currentPlan) {
            bb += `[player]${p}[/player]\n[spoiler=Ver órdenes][table]\n[**]Tipo[||]Lanzar[||]Origen[||]Destino[/**]\n`;
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
