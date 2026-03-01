<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        /* KILL ALL GAPS */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { width: 100vw; height: 100vh; overflow: hidden; background: #000; font-family: sans-serif; }

        /* CLOAK LAYER (Fake Google Classroom) */
        #cloak {
            position: fixed; inset: 0; background: #fff; z-index: 1000;
            display: flex; flex-direction: column; cursor: pointer;
        }
        .gc-bar { height: 60px; border-bottom: 1px solid #ddd; display: flex; align-items: center; padding: 0 20px; font-size: 20px; color: #5f6368; }
        .gc-main { padding: 40px; display: flex; gap: 20px; }
        .gc-card { width: 300px; height: 200px; border: 1px solid #ddd; border-radius: 8px; background: #1967d2; }

        /* THE PORTAL (Full Screen Layout) */
        #portal {
            display: none; width: 100vw; height: 100vh;
            position: absolute; top: 0; left: 0;
        }

        /* SIDEBAR - Fixed to Left */
        #sidebar {
            position: absolute; left: 0; top: 0; bottom: 0; width: 260px;
            background: #111; border-right: 1px solid #333; color: white;
            overflow-y: auto; z-index: 100;
        }
        .side-title { padding: 20px; font-weight: bold; color: #00d2ff; border-bottom: 1px solid #333; }
        .side-section { padding: 15px; }
        .label { font-size: 10px; color: #666; text-transform: uppercase; margin: 15px 0 5px; }
        
        .btn { 
            width: 100%; padding: 12px; margin-bottom: 6px; 
            background: #1e1e1e; border: 1px solid #333; color: #ccc; 
            text-align: left; cursor: pointer; border-radius: 6px; font-size: 13px;
        }
        .btn:hover { background: #333; color: #fff; border-color: #00d2ff; }

        /* MAIN AREA - Stretch to fill */
        #main {
            position: absolute; left: 260px; right: 0; top: 0; bottom: 0;
            display: flex; flex-direction: column;
        }

        #toolbar {
            height: 60px; background: #1a1a1a; padding: 10px 20px;
            display: flex; gap: 10px; align-items: center; border-bottom: 1px solid #333;
        }
        #url-bar { flex: 1; padding: 10px; border-radius: 4px; border: 1px solid #444; background: #000; color: #00ff00; }

        /* VIEWPORT & SCROLLING */
        #view-container { flex: 1; background: #fff; overflow-y: auto !important; }
        #frame { width: 100%; height: 5000px; border: none; display: block; }

        .go { background: #00d2ff; padding: 10px 20px; border: none; font-weight: bold; border-radius: 4px; cursor: pointer; }
    </style>
</head>
<body onclick="start()">

    <div id="cloak">
        <div class="gc-bar">Google Classroom</div>
        <div class="gc-main">
            <div class="gc-card"></div>
            <div class="gc-card" style="background:#137333"></div>
        </div>
    </div>

    <div id="portal">
        <div id="sidebar">
            <div class="side-title">PORTAL OS v6</div>
            <div class="side-section">
                <div class="label">Search</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing</button>
                <button class="btn" onclick="load('https://search.brave.com')">Brave</button>
                <button class="btn" onclick="load('https://duckduckgo.com')">DuckDuckGo</button>

                <div class="label">Proxies (Required for most sites)</div>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://www.4everproxy.com')">🔗 4EverProxy</button>

                <div class="label">Games</div>
                <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
                <button class="btn" onclick="load('https://www.coolmathgames.com')">➕ CoolMath</button>

                <div class="label">History</div>
                <div id="hist"></div>
            </div>
        </div>

        <div id="main">
            <div id="toolbar">
                <input type="text" id="url-bar" placeholder="Paste URL or search here...">
                <button class="go" onclick="load()">GO</button>
            </div>
            <div id="view-container">
                <iframe id="frame" src="about:blank"></iframe>
            </div>
        </div>
    </div>

<script>
    const cloak = document.getElementById('cloak');
    const portal = document.getElementById('portal');
    const frame = document.getElementById('frame');
    const input = document.getElementById('url-bar');

    function start() {
        cloak.style.display = 'none';
        portal.style.display = 'block';
        window.history.replaceState({}, '', ' ');
    }

    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

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
        document.getElementById('view-container').scrollTop = 0;

        // History Update
        const h = document.getElementById('hist');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "🕒 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        if (h.children.length > 5) h.removeChild(h.lastChild);
        h.insertBefore(b, h.firstChild);
    }
</script>
</body>
</html>
