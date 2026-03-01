<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { width: 100%; height: 100%; overflow: hidden; font-family: 'Roboto', Arial, sans-serif; background: #fff; }

        /* THE CLOAK (Fake Google Classroom UI to prevent 403) */
        #cloak-layer {
            position: fixed; inset: 0; background: #fff; z-index: 10000;
            display: flex; flex-direction: column; cursor: pointer;
        }
        .gc-header { height: 64px; border-bottom: 1px solid #e0e0e0; display: flex; align-items: center; padding: 0 20px; justify-content: space-between; }
        .gc-logo { display: flex; align-items: center; gap: 10px; color: #5f6368; font-size: 22px; }
        .gc-content { padding: 24px; display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; }
        .gc-card { height: 200px; border: 1px solid #dadce0; border-radius: 8px; background: #fff; position: relative; overflow: hidden; }
        .gc-card-top { height: 100px; background: #1967d2; padding: 15px; color: #fff; }

        /* THE ACTUAL PORTAL (Hidden) */
        #app-root {
            display: none; width: 100vw; height: 100vh;
            grid-template-columns: 260px 1fr; grid-template-rows: 60px 1fr;
        }

        /* SIDEBAR FIX: Absolute positioning to ensure it stays visible */
        #sidebar {
            grid-row: 1 / 3; background: #111; border-right: 1px solid #333;
            color: #fff; display: flex; flex-direction: column; overflow-y: auto; z-index: 50;
        }
        .sidebar-header { padding: 20px; font-weight: bold; color: #00d2ff; border-bottom: 1px solid #333; }
        .nav-section { padding: 15px; }
        .nav-label { font-size: 10px; color: #555; text-transform: uppercase; margin: 15px 0 8px; }
        .btn { 
            width: 100%; padding: 12px; margin-bottom: 6px; background: #1e1e1e; border: 1px solid #333;
            color: #ccc; text-align: left; cursor: pointer; border-radius: 6px; font-size: 13px;
        }
        .btn:hover { background: #333; color: #00d2ff; border-color: #00d2ff; }

        /* MAIN AREA: Full Screen Scaling */
        #main-view { grid-column: 2; display: flex; flex-direction: column; background: #000; height: 100vh; }
        #toolbar {
            height: 60px; background: #1a1a1a; padding: 10px 20px;
            display: flex; gap: 10px; align-items: center; border-bottom: 1px solid #333;
        }
        #url-input { flex: 1; padding: 10px; border-radius: 4px; border: 1px solid #444; background: #000; color: #00ff00; }
        
        /* SCROLL FIX: The forced viewport */
        #viewport { flex: 1; width: 100%; overflow: auto !important; background: #fff; }
        #portal-frame { width: 100%; height: 5000px; border: none; display: block; }

        .ctrl { padding: 10px 15px; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; }
        .go { background: #00d2ff; }
        .pop { background: #ff8c00; }
    </style>
</head>
<body onclick="unlock()">

    <div id="cloak-layer">
        <div class="gc-header">
            <div class="gc-logo">
                <img src="https://ssl.gstatic.com/classroom/favicon.png" width="24">
                Google Classroom
            </div>
            <div style="color:#5f6368">➕ &nbsp; Apps</div>
        </div>
        <div class="gc-content">
            <div class="gc-card"><div class="gc-card-top"><h3>English 101</h3><p>Period 3</p></div></div>
            <div class="gc-card"><div class="gc-card-top" style="background:#137333"><h3>Algebra II</h3><p>Period 1</p></div></div>
            <div class="gc-card"><div class="gc-card-top" style="background:#e37400"><h3>World History</h3><p>Period 5</p></div></div>
        </div>
        <div style="position:fixed; bottom:20px; right:20px; color:#ccc; font-size:12px;">Click anywhere to join class...</div>
    </div>

    <div id="app-root">
        <div id="sidebar">
            <div class="sidebar-header">PORTAL ELITE</div>
            <div class="nav-section">
                <div class="nav-label">Search</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing (Safe)</button>
                <button class="btn" onclick="load('https://search.brave.com')">Brave Search</button>

                <div class="nav-label">Proxies (For Blocked Sites)</div>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://www.4everproxy.com')">🔗 4EverProxy</button>

                <div class="nav-label">Games</div>
                <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
                <button class="btn" onclick="load('https://www.coolmathgames.com')">➕ CoolMath</button>

                <div class="nav-label">History</div>
                <div id="h-container"></div>
            </div>
        </div>

        <div id="main-view">
            <div id="toolbar">
                <input type="text" id="url-input" placeholder="Type a URL or search...">
                <button class="ctrl go" onclick="load()">GO</button>
                <button class="ctrl pop" onclick="popOut()">↗</button>
            </div>
            <div id="viewport">
                <iframe id="portal-frame" src="about:blank"></iframe>
            </div>
        </div>
    </div>

<script>
    const cloak = document.getElementById('cloak-layer');
    const app = document.getElementById('app-root');
    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('url-input');
    const viewport = document.getElementById('viewport');

    function unlock() {
        cloak.style.display = 'none';
        app.style.display = 'grid';
        // Stealth URL clean
        window.history.replaceState({}, '', ' ');
    }

    // Auto-unlock after 4 seconds
    setTimeout(unlock, 4000);

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
        const container = document.getElementById('h-container');
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
