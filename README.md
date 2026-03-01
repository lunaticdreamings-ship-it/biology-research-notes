<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Power Portal Elite</title>
    <style>
        /* CORE RESET */
        * { box-sizing: border-box; }
        body, html { 
            margin: 0; padding: 0; height: 100%; width: 100%; 
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; 
            background: #0f0f0f; color: white; overflow: hidden; 
        }

        /* GRID LAYOUT: Sidebar left, Content right */
        .main-structure {
            display: grid;
            grid-template-columns: 260px 1fr;
            height: 100vh;
            width: 100vw;
        }

        /* SIDEBAR */
        .sidebar {
            background: #181818;
            border-right: 1px solid #333;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
            z-index: 100;
        }
        .sidebar h2 { padding: 20px; color: #00d2ff; margin: 0; font-size: 1.1rem; border-bottom: 1px solid #333; }
        .nav-group { padding: 15px; }
        .nav-label { font-size: 10px; color: #666; text-transform: uppercase; letter-spacing: 1px; margin: 15px 0 8px; }

        .btn { 
            width: 100%; padding: 12px; margin-bottom: 6px; 
            background: #252525; border: 1px solid #333; color: #ddd; 
            text-align: left; cursor: pointer; border-radius: 6px; font-size: 14px;
        }
        .btn:hover { background: #333; border-color: #00d2ff; color: white; }

        /* VIEWER AREA */
        .viewer {
            display: flex;
            flex-direction: column;
            height: 100%;
            background: #000;
        }

        /* ADDRESS BAR */
        .address-bar {
            background: #202020;
            padding: 10px 15px;
            display: flex;
            gap: 10px;
            align-items: center;
            border-bottom: 1px solid #333;
            z-index: 100;
        }
        .address-bar input {
            flex-grow: 1;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #444;
            background: #0a0a0a;
            color: #44ff44;
            font-family: 'Courier New', monospace;
            font-size: 14px;
        }
        
        /* BUTTONS */
        .ctrl-btn { padding: 10px 16px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; transition: 0.2s; }
        .go-btn { background: #00d2ff; color: #000; }
        .refresh-btn { background: #333; color: #fff; }
        .pop-btn { background: #ff8c00; color: #000; font-size: 18px; }
        .ctrl-btn:hover { opacity: 0.8; transform: translateY(-1px); }

        /* CONTENT AREA */
        .content-frame-wrapper {
            flex-grow: 1;
            position: relative;
            background: white;
            overflow: auto; /* Enables scrolling for the wrapper */
        }
        iframe {
            width: 100%;
            height: 100%;
            border: none;
            display: block;
        }

        /* OVERLAYS */
        .panic-hint { position: absolute; bottom: 10px; right: 20px; font-size: 11px; color: #444; pointer-events: none; }
    </style>
</head>
<body>

<div class="main-structure">
    <div class="sidebar">
        <h2>PORTAL OS</h2>
        <div class="nav-group">
            <div class="nav-label">Search (Frame-Friendly)</div>
            <button class="btn" onclick="load('https://duckduckgo.com')">🔍 DuckDuckGo</button>
            <button class="btn" onclick="load('https://www.bing.com')">🅱️ Bing</button>
            
            <div class="nav-label">Unblocked Games</div>
            <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
            <button class="btn" onclick="load('https://www.coolmathgames.com')">➕ CoolMath Games</button>
            <button class="btn" onclick="load('https://armorgames.com')">🛡️ Armor Games</button>

            <div class="nav-label">Web Proxies</div>
            <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
            <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>

            <div class="nav-label">Utilities</div>
            <button class="btn" onclick="addBookmark()">⭐ Save Current Site</button>
            <div id="bookmarks"></div>
        </div>
    </div>

    <div class="viewer">
        <div class="address-bar">
            <button class="ctrl-btn refresh-btn" onclick="refreshFrame()">↻</button>
            <input type="text" id="urlField" placeholder="Enter URL or Search Query..." onkeydown="if(event.key==='Enter') load()">
            <button class="ctrl-btn go-btn" onclick="load()">GO</button>
            <button class="ctrl-btn pop-btn" onclick="popOut()" title="Cloaked Pop-out">↗</button>
        </div>
        
        <div class="content-frame-wrapper">
            <iframe id="portal-frame" src="about:blank"></iframe>
            <div class="panic-hint">ESC to Panic</div>
        </div>
    </div>
</div>

<script>
    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('urlField');

    // 1. PANIC BUTTON (ESC)
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') {
            window.location.replace('https://www.google.com');
        }
    });

    // 2. LOAD FUNCTION
    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url) return;

        // Search logic: If no dots and no http, search DuckDuckGo
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://duckduckgo.com/?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
    }

    // 3. REFRESH FUNCTION
    function refreshFrame() {
        const currentSrc = frame.src;
        frame.src = 'about:blank';
        setTimeout(() => { frame.src = currentSrc; }, 10);
    }

    // 4. CLOAKED POP-OUT (about:blank)
    function popOut() {
        const url = input.value;
        if (!url || url === 'about:blank') return;
        
        const win = window.open('about:blank', '_blank');
        if (win) {
            win.document.title = "Search";
            win.document.body.style.margin = '0';
            win.document.body.style.height = '100vh';
            win.document.body.style.overflow = 'hidden';

            const ifr = win.document.createElement('iframe');
            ifr.style.cssText = "width:100%; height:100%; border:none; margin:0; padding:0;";
            ifr.src = url;
            win.document.body.appendChild(ifr);
        }
    }

    // 5. BOOKMARK FEATURE
    function addBookmark() {
        const url = input.value;
        if (!url || url === 'about:blank') return;
        
        const container = document.getElementById('bookmarks');
        const bBtn = document.createElement('button');
        bBtn.className = 'btn';
        bBtn.style.borderColor = '#44ff44';
        bBtn.innerText = "⭐ " + url.replace('https://', '').split('/')[0];
        bBtn.onclick = () => load(url);
        container.appendChild(bBtn);
    }
</script>

</body>
</html>
