<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { width: 100vw; height: 100vh; background: #000; overflow: hidden; font-family: system-ui, -apple-system, sans-serif; }

        /* CLOAK */
        #cloak { position: fixed; inset: 0; background: #fff; z-index: 10000; display: flex; flex-direction: column; cursor: pointer; }
        .gc-nav { height: 65px; border-bottom: 1px solid #ddd; display: flex; align-items: center; padding: 0 20px; font-size: 22px; color: #5f6368; }
        .gc-body { padding: 30px; display: flex; gap: 20px; }
        .gc-card { width: 300px; height: 200px; border: 1px solid #ddd; border-radius: 8px; background: #1a73e8; }

        /* PANIC */
        #panic-screen { display: none; position: fixed; inset: 0; background: #fff; z-index: 20000; padding: 50px; color: #333; }

        /* PORTAL */
        #portal { display: none; position: fixed; inset: 0; width: 100vw; height: 100vh; }
        #sidebar { position: absolute; left: 0; top: 0; bottom: 0; width: 260px; background: #111; border-right: 1px solid #333; display: flex; flex-direction: column; z-index: 100; }
        .side-scroll { padding: 15px; flex: 1; overflow-y: auto; color: white; }
        .label { font-size: 10px; color: #666; text-transform: uppercase; margin: 15px 0 8px; }
        .btn { width: 100%; padding: 12px; margin-bottom: 8px; background: #222; border: 1px solid #444; color: #eee; text-align: left; cursor: pointer; border-radius: 6px; font-size: 12px; }
        .btn:hover { border-color: #00d2ff; background: #282828; }
        .panic-btn { background: #d93025; color: white; border: none; font-weight: bold; margin-top: 20px; }

        /* MAIN */
        #main { position: absolute; left: 260px; right: 0; top: 0; bottom: 0; display: flex; flex-direction: column; }
        #toolbar { height: 60px; background: #1a1a1a; padding: 0 20px; display: flex; gap: 15px; align-items: center; border-bottom: 1px solid #333; }
        #url-bar { flex: 1; padding: 10px 15px; border-radius: 20px; border: 1px solid #444; background: #050505; color: #00ff00; outline: none; }
        #viewport { flex: 1; background: #fff; overflow-y: auto !important; position: relative; }
        #frame { width: 100%; height: 8000px; border: none; display: block; }
        .go-btn { background: #00d2ff; color: #000; padding: 10px 25px; border: none; border-radius: 20px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body onclick="launch()">

    <div id="cloak">
        <div class="gc-nav">Google Classroom</div>
        <div class="gc-body"><div class="gc-card"></div><div class="gc-card" style="background:#137333"></div></div>
    </div>

    <div id="panic-screen" onclick="this.style.display='none'">
        <h1>Algebra II: Linear Equations</h1>
        <p style="margin-top:20px;">Solve for x: 4x - 12 = 28</p>
    </div>

    <div id="portal">
        <div id="sidebar">
            <div class="side-scroll">
                <div class="label">GitHub-Friendly Proxies</div>
                <button class="btn" onclick="load('https://api.allorigins.win/get?url=https://www.google.com')">🌐 AllOrigins (Bypass)</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 Croxy (Stable)</button>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
                <button class="btn" onclick="load('https://proxyium.com')">🛡️ Proxyium</button>

                <div class="label">Direct Tools</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing (Educational)</button>
                <button class="btn" onclick="load('https://poki.com')">Poki Games</button>

                <div class="label">History</div>
                <div id="hist-list"></div>
                <button class="btn panic-btn" onclick="panic()">🚨 PANIC (P)</button>
            </div>
        </div>
        <div id="main">
            <div id="toolbar">
                <input type="text" id="url-bar" placeholder="Enter URL...">
                <button class="go-btn" onclick="load()">GO</button>
            </div>
            <div id="viewport"><iframe id="frame" src="about:blank"></iframe></div>
        </div>
    </div>

<script>
    function launch() { 
        if(document.getElementById('portal').style.display !== 'block') {
            document.getElementById('cloak').style.display = 'none';
            document.getElementById('portal').style.display = 'block';
            window.history.replaceState({}, '', ' ');
        }
    }
    function panic() { document.getElementById('panic-screen').style.display = 'block'; }
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
        if (e.key.toLowerCase() === 'p') panic();
    });

    const frame = document.getElementById('frame');
    const input = document.getElementById('url-bar');

    function load(urlManual) {
        let url = urlManual || input.value;
        if(!url) return;
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }
        input.value = url;
        frame.src = url;
        document.getElementById('viewport').scrollTop = 0;

        const h = document.getElementById('hist-list');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "🕒 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        if (h.children.length > 3) h.removeChild(h.lastChild);
        h.insertBefore(b, h.firstChild);
    }
</script>
</body>
</html>
