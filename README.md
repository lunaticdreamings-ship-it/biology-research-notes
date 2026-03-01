<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { width: 100vw; height: 100vh; background: #fff; overflow: hidden; font-family: sans-serif; }

        /* THE CLOAK LAYER */
        #cloak-layer {
            position: fixed; inset: 0; background: #fff; z-index: 10000;
            cursor: default;
        }
        #cloak-iframe { width: 100%; height: 100%; border: none; pointer-events: none; }

        /* THE HIDDEN APP */
        #app-root {
            display: none; /* Changed to none until triggered */
            width: 100vw; height: 100vh;
            display: grid;
            grid-template-columns: 260px 1fr;
            grid-template-rows: 60px 1fr;
        }

        /* SIDEBAR */
        #sidebar {
            grid-row: 1 / 3; background: #111; border-right: 1px solid #333;
            display: flex; flex-direction: column; color: white; overflow-y: auto; z-index: 20;
        }
        .sidebar-brand { padding: 20px; font-weight: bold; color: #00d2ff; border-bottom: 1px solid #333; }
        .nav-section { padding: 15px; }
        .nav-label { font-size: 10px; color: #555; text-transform: uppercase; margin: 15px 0 8px; }
        
        .btn { 
            width: 100%; padding: 12px; margin-bottom: 6px; 
            background: #1e1e1e; border: 1px solid #333; color: #ccc; 
            text-align: left; cursor: pointer; border-radius: 6px; font-size: 13px;
        }
        .btn:hover { background: #333; border-color: #00d2ff; color: #fff; }

        /* TOOLBAR */
        #toolbar {
            grid-column: 2; background: #1a1a1a; padding: 10px 20px;
            display: flex; gap: 12px; align-items: center; border-bottom: 1px solid #333;
        }
        #url-input {
            flex: 1; padding: 10px 15px; border-radius: 20px; border: 1px solid #444;
            background: #000; color: #44ff44; font-family: monospace; outline: none;
        }

        .ctrl-btn { padding: 10px 18px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; }
        .btn-go { background: #00d2ff; color: #000; }
        .btn-pop { background: #ff8c00; color: #000; }

        /* VIEWPORT & SCROLL FIX */
        #viewport { grid-column: 2; background: #fff; position: relative; overflow-y: auto !important; }
        #portal-frame { width: 100%; height: 5000px; border: none; display: block; }

        /* FAKE BUTTON FOR REDIRECT */
        #trigger-zone {
            position: absolute; top: 15px; right: 15px;
            width: 40px; height: 40px; background: transparent;
            z-index: 10001; cursor: pointer;
        }
    </style>
</head>
<body onclick="unlock()">

    <div id="cloak-layer">
        <div id="trigger-zone" onclick="unlock()"></div>
        <iframe id="cloak-iframe" src="https://classroom.google.com"></iframe>
    </div>

    <div id="app-root" style="display: none;">
        <div id="sidebar">
            <div class="sidebar-brand">PORTAL OS v5</div>
            <div class="nav-section">
                <div class="nav-label">Search</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing Search</button>
                <button class="btn" onclick="load('https://search.brave.com')">Brave Search</button>

                <div class="nav-label">Proxies (No Login)</div>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://www.4everproxy.com')">🔗 4EverProxy</button>

                <div class="nav-label">Games</div>
                <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
                <button class="btn" onclick="load('https://www.coolmathgames.com')">➕ CoolMath</button>

                <div class="nav-label">History</div>
                <div id="history-container"></div>
            </div>
        </div>

        <div id="toolbar">
            <input type="text" id="url-input" placeholder="Enter URL...">
            <button class="ctrl-btn btn-go" onclick="load()">GO</button>
            <button class="ctrl-btn btn-pop" onclick="popOut()">↗</button>
        </div>

        <div id="viewport">
            <iframe id="portal-frame" src="about:blank"></iframe>
        </div>
    </div>

<script>
    const cloak = document.getElementById('cloak-layer');
    const app = document.getElementById('app-root');
    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('url-input');
    const viewport = document.getElementById('viewport');

    // FIX: Transition function
    function unlock() {
        cloak.style.display = 'none';
        app.style.display = 'grid';
        // This clears the visible path in the address bar to just "/" or "about:blank"
        window.history.replaceState({}, '', ' '); 
    }

    // Auto-transition after 5 seconds if no click happens
    setTimeout(unlock, 5000);

    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url) return;
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }
        input.value = url;
        frame.src = url;
        viewport.scrollTop = 0;
        updateHistory(url);
    }

    function updateHistory(url) {
        const container = document.getElementById('history-container');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "🕒 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        if (container.children.length > 5) container.removeChild(container.lastChild);
        container.insertBefore(b, container.firstChild);
    }

    function popOut() {
        const url = input.value;
        if (!url) return;
        const win = window.open('about:blank', '_blank');
        win.document.write(`<html><head><title>Classes</title><link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png"></head><body style="margin:0;overflow:hidden"><iframe src="${url}" style="width:100%;height:100vh;border:none"></iframe></body></html>`);
    }
</script>
</body>
</html>
