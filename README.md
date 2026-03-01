<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        /* FORCE FULL SCREEN */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { 
            width: 100vw; height: 100vh; 
            background: #000; overflow: hidden; 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
        }

        /* CLOAK LAYER */
        #cloak {
            position: fixed; inset: 0; background: #fff; z-index: 10000;
            display: flex; flex-direction: column; cursor: pointer;
        }
        .gc-nav { height: 65px; border-bottom: 1px solid #ddd; display: flex; align-items: center; padding: 0 20px; font-size: 22px; color: #5f6368; }
        .gc-body { padding: 30px; display: flex; gap: 20px; flex-wrap: wrap; }
        .gc-card { width: 300px; height: 200px; border: 1px solid #ddd; border-radius: 8px; background: #1a73e8; }

        /* MASTER LAYOUT */
        #portal {
            display: none;
            position: fixed; inset: 0;
            width: 100vw; height: 100vh;
            background: #000;
        }

        /* SIDEBAR - restored and locked */
        #sidebar {
            position: absolute; left: 0; top: 0; bottom: 0; width: 260px;
            background: #111; border-right: 1px solid #333;
            display: flex; flex-direction: column; overflow-y: auto; color: white;
            z-index: 100;
        }
        .side-head { padding: 20px; font-weight: bold; color: #00d2ff; border-bottom: 1px solid #333; text-transform: uppercase; }
        .side-scroll { padding: 15px; flex: 1; }
        .label { font-size: 10px; color: #666; text-transform: uppercase; margin: 15px 0 8px; letter-spacing: 1px; }
        
        .btn { 
            width: 100%; padding: 12px; margin-bottom: 8px; 
            background: #222; border: 1px solid #444; color: #eee; 
            text-align: left; cursor: pointer; border-radius: 6px; font-size: 13px;
        }
        .btn:hover { background: #333; border-color: #00d2ff; color: #fff; }

        /* MAIN CONTENT AREA */
        #main {
            position: absolute; left: 260px; right: 0; top: 0; bottom: 0;
            display: flex; flex-direction: column; background: #000;
        }

        #toolbar {
            height: 60px; background: #1a1a1a; padding: 0 20px;
            display: flex; gap: 15px; align-items: center; border-bottom: 1px solid #333;
        }
        #url-bar { 
            flex: 1; padding: 10px 15px; border-radius: 20px; 
            border: 1px solid #444; background: #050505; color: #00ff00; 
            font-family: monospace; outline: none;
        }

        /* VIEWPORT & SCROLLING */
        #viewport { flex: 1; background: #fff; overflow-y: auto !important; position: relative; }
        #frame { width: 100%; height: 6000px; border: none; display: block; }

        .go-btn { background: #00d2ff; color: #000; padding: 10px 25px; border: none; border-radius: 20px; font-weight: bold; cursor: pointer; }
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
    </div>

    <div id="portal">
        <div id="sidebar">
            <div class="side-head">Portal Elite v7</div>
            <div class="side-scroll">
                <div class="label">Search Engines</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing Search</button>
                <button class="btn" onclick="load('https://search.brave.com')">Brave Search</button>
                
                <div class="label">Proxies (Try all if one fails)</div>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway (Best)</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://proxyium.com')">🛡️ Proxyium</button>
                <button class="btn" onclick="load('https://www.4everproxy.com')">🔗 4EverProxy</button>

                <div class="label">Games</div>
                <button class="btn" onclick="load('https://poki.com')">Poki</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">CrazyGames</button>
                <button class="btn" onclick="load('https://www.coolmathgames.com')">CoolMath</button>
                <button class="btn" onclick="load('https://armorgames.com')">Armor Games</button>

                <div class="label">History</div>
                <div id="hist-list"></div>
            </div>
        </div>

        <div id="main">
            <div id="toolbar">
                <input type="text" id="url-bar" placeholder="Enter URL or Search Query..." onkeydown="if(event.key==='Enter') load()">
                <button class="go-btn" onclick="load()">GO</button>
            </div>
            <div id="viewport">
                <iframe id="frame" src="about:blank"></iframe>
            </div>
        </div>
    </div>

<script>
    function launch() {
        document.getElementById('cloak').style.display = 'none';
        document.getElementById('portal').style.display = 'block';
        window.history.replaceState({}, '', ' ');
    }

    const frame = document.getElementById('frame');
    const input = document.getElementById('url-bar');
    const viewport = document.getElementById('viewport');

    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

    function load(manual) {
        let url = manual || input.value;
        if (!url) return;

        // Auto-Search logic
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        viewport.scrollTop = 0;

        // Add to history
        const h = document.getElementById('hist-list');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "🕒 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        if (h.children.length > 4) h.removeChild(h.lastChild);
        h.insertBefore(b, h.firstChild);
    }
</script>
</body>
</html>
</html>
