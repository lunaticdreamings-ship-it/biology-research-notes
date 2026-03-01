<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        :root { --accent: #00d2ff; --bg: #0a0a0a; --panel: #151515; --text: #eee; }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { width: 100vw; height: 100vh; background: var(--bg); overflow: hidden; font-family: 'Inter', sans-serif; }

        /* --- STAGE 1: THE CLOAK --- */
        #cloak {
            position: fixed; inset: 0; background: #fff; z-index: 10000;
            display: flex; flex-direction: column; cursor: pointer;
        }
        .gc-nav { height: 65px; border-bottom: 1px solid #ddd; display: flex; align-items: center; padding: 0 20px; font-size: 22px; color: #5f6368; }
        .gc-body { padding: 30px; display: flex; gap: 20px; flex-wrap: wrap; }
        .gc-card { width: 300px; height: 200px; border: 1px solid #ddd; border-radius: 8px; background: #1a73e8; }

        /* --- STAGE 3: THE PANIC ASSIGNMENT --- */
        #panic-screen {
            display: none; position: fixed; inset: 0; background: #fff; z-index: 20000; 
            padding: 60px; color: #333; overflow-y: auto; font-family: serif;
        }

        /* --- STAGE 2: THE MASTER PORTAL --- */
        #portal {
            display: none; position: fixed; inset: 0; width: 100vw; height: 100vh;
            display: grid; grid-template-columns: 280px 1fr; grid-template-rows: 65px 1fr;
        }

        #sidebar {
            grid-row: 1 / 3; background: var(--panel); border-right: 1px solid #222;
            display: flex; flex-direction: column; z-index: 100; color: var(--text);
        }
        .side-head { padding: 20px; font-weight: 800; color: var(--accent); border-bottom: 1px solid #222; font-size: 1.1rem; }
        .side-scroll { padding: 15px; flex: 1; overflow-y: auto; }
        .label { font-size: 10px; color: #555; text-transform: uppercase; margin: 15px 0 8px; letter-spacing: 2px; font-weight: bold; }
        
        .btn { 
            width: 100%; padding: 12px; margin-bottom: 6px; background: #1a1a1a; border: 1px solid #252525;
            color: #aaa; text-align: left; cursor: pointer; border-radius: 8px; font-size: 13px; transition: 0.3s;
        }
        .btn:hover { background: #222; border-color: var(--accent); color: #fff; transform: translateX(4px); }
        .panic-btn { background: #441111; color: #ff5555; border: 1px solid #662222; margin-top: 20px; font-weight: bold; }

        #toolbar {
            grid-column: 2; background: #0f0f0f; padding: 0 25px;
            display: flex; gap: 15px; align-items: center; border-bottom: 1px solid #222;
        }
        #url-bar { 
            flex: 1; padding: 12px 20px; border-radius: 30px; border: 1px solid #222; 
            background: #000; color: #00ff00; font-family: 'Fira Code', monospace; outline: none;
        }
        .go-btn { background: var(--accent); color: #000; padding: 10px 25px; border: none; border-radius: 30px; font-weight: bold; cursor: pointer; }

        #viewport { grid-column: 2; background: #fff; overflow-y: auto !important; position: relative; }
        #frame { width: 100%; height: 10000px; border: none; display: block; }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-thumb { background: #333; border-radius: 10px; }
    </style>
</head>
<body onclick="launch()">

    <div id="cloak">
        <div class="gc-nav">Google Classroom</div>
        <div class="gc-body">
            <div class="gc-card"></div>
            <div class="gc-card" style="background:#137333"></div>
            <div class="gc-card" style="background:#e37400"></div>
        </div>
        <div style="position:fixed; bottom:20px; right:20px; color:#ddd">Click to join class...</div>
    </div>

    <div id="panic-screen" onclick="this.style.display='none'">
        <h2 style="border-bottom: 2px solid #333; padding-bottom: 10px;">Linear Algebra Quiz: Module 4</h2>
        <p style="margin: 20px 0;"><strong>Student Name:</strong> ________________________</p>
        <p>1. Solve for $x$: $7(x - 4) + 3 = 5x - 11$</p>
        <p style="margin-top: 20px;">2. Find the slope of the line passing through $(-2, 4)$ and $(5, -10)$.</p>
        <p style="margin-top: 20px;">3. Graph the inequality: $y < -2x + 5$</p>
    </div>

    <div id="portal" style="display: none;">
        <div id="sidebar">
            <div class="side-head">PORTAL V9.1</div>
            <div class="side-scroll">
                <div class="label">Primary Proxy (New Node)</div>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway 1</button>
                <button class="btn" onclick="load('https://proxyium.com')">🛡️ Proxyium Node</button>
                
                <div class="label">Alternative Mirrors</div>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://www.4everproxy.com')">🔗 4EverProxy</button>

                <div class="label">Games & Apps</div>
                <button class="btn" onclick="load('https://poki.com')">🕹️ Poki Games</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
                <button class="btn" onclick="load('https://discord.com/login')">💬 Discord (Use Proxy!)</button>

                <div class="label">History</div>
                <div id="hist-list"></div>

                <div class="label">Saved Bookmarks</div>
                <button class="btn" style="border: 1px dashed #444" onclick="addBookmark()">⭐ Save Current</button>
                <div id="bookmarks"></div>

                <button class="btn panic-btn" onclick="panic()">🚨 PANIC MODE (P)</button>
            </div>
        </div>

        <div id="toolbar">
            <input type="text" id="url-bar" placeholder="Enter URL or Search Query..." onkeydown="if(event.key==='Enter') load()">
            <button class="go-btn" onclick="load()">GO</button>
        </div>

        <div id="viewport">
            <iframe id="frame" src="about:blank"></iframe>
        </div>
    </div>

<script>
    function launch() {
        const p = document.getElementById('portal');
        if(p.style.display === 'none') {
            document.getElementById('cloak').style.display = 'none';
            p.style.display = 'grid';
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
    const viewport = document.getElementById('viewport');

    function load(manual) {
        let url = manual || input.value;
        if (!url) return;
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }
        input.value = url;
        frame.src = url;
        viewport.scrollTop = 0;
        updateList('hist-list', url, "🕒 ");
    }

    function updateList(id, url, emoji) {
        const container = document.getElementById(id);
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = emoji + url.split('//')[1].split('/')[0].substring(0, 18);
        b.onclick = () => load(url);
        if (container.children.length > 5) container.removeChild(container.lastChild);
        container.insertBefore(b, container.firstChild);
    }

    function addBookmark() {
        const url = input.value;
        if(url && url !== "about:blank") updateList('bookmarks', url, "📍 ");
    }
</script>
</body>
</html>
