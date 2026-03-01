<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Power Portal Ultimate</title>
    <style>
        * { box-sizing: border-box; }
        body, html { 
            margin: 0; padding: 0; height: 100%; width: 100%; 
            font-family: 'Segoe UI', sans-serif; background: #0a0a0a; color: white; 
            overflow: hidden; 
        }

        .app-grid {
            display: grid;
            grid-template-columns: 260px 1fr;
            height: 100vh;
        }

        /* SIDEBAR */
        .sidebar {
            background: #151515;
            border-right: 1px solid #333;
            display: flex;
            flex-direction: column;
            z-index: 50;
            overflow-y: auto;
        }
        .sidebar h2 { padding: 20px; color: #00d2ff; margin: 0; font-size: 1.1rem; border-bottom: 1px solid #333; }
        .nav-group { padding: 15px; flex-grow: 1; }
        .nav-label { font-size: 10px; color: #666; text-transform: uppercase; margin: 15px 0 8px; letter-spacing: 1px; }

        .btn { 
            width: 100%; padding: 10px; margin-bottom: 5px; 
            background: #202020; border: 1px solid #333; color: #ccc; 
            text-align: left; cursor: pointer; border-radius: 4px; font-size: 13px;
        }
        .btn:hover { background: #333; color: #00d2ff; }

        /* VIEWER AREA */
        .viewer { display: flex; flex-direction: column; background: #000; position: relative; }

        .address-bar {
            background: #1a1a1a;
            padding: 10px 15px;
            display: flex;
            gap: 8px;
            z-index: 100;
            border-bottom: 1px solid #333;
        }
        .address-bar input {
            flex-grow: 1;
            padding: 10px;
            border-radius: 4px;
            border: 1px solid #444;
            background: #000;
            color: #00ff00;
            font-family: monospace;
        }
        
        .ctrl-btn { padding: 8px 12px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; }
        .go-btn { background: #00d2ff; }
        .pop-btn { background: #ff8c00; font-size: 16px; }

        /* THE SCROLL FIX: Forced Viewport */
        .viewport-container {
            flex: 1;
            position: relative;
            overflow: auto !important; /* Force browser scrollbars */
            background: #fff;
            /* This ensures the mouse wheel is captured by the container */
            pointer-events: auto; 
        }

        iframe {
            width: 100%;
            height: 100%;
            min-height: 100.1vh; /* Slightly taller than 100% to force scroll capability */
            border: none;
            display: block;
        }

        /* Panic Hint */
        .panic-overlay { position: absolute; bottom: 5px; right: 10px; font-size: 10px; color: #444; pointer-events: none; z-index: 200; }
    </style>
</head>
<body>

<div class="app-grid">
    <div class="sidebar">
        <h2>PORTAL OS</h2>
        <div class="nav-group">
            <div class="nav-label">Recommended</div>
            <button class="btn" onclick="load('https://duckduckgo.com')">🔍 DuckDuckGo</button>
            <button class="btn" onclick="load('https://www.bing.com')">🅱️ Bing</button>
            <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway Proxy</button>
            
            <div class="nav-label">History</div>
            <div id="history-list"></div>

            <div class="nav-label">Bookmarks</div>
            <button class="btn" onclick="addBookmark()" style="border-style: dashed;">⭐ Save Current</button>
            <div id="bookmark-list"></div>
        </div>
    </div>

    <div class="viewer">
        <div class="address-bar">
            <input type="text" id="urlField" placeholder="Enter URL..." onkeydown="if(event.key==='Enter') load()">
            <button class="ctrl-btn go-btn" onclick="load()">GO</button>
            <button class="ctrl-btn pop-btn" onclick="popOut()">↗</button>
        </div>
        
        <div class="viewport-container" id="scroller">
            <iframe id="portal-frame" src="about:blank"></iframe>
        </div>
        <div class="panic-overlay">ESC to Panic</div>
    </div>
</div>

<script>
    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('urlField');
    const scroller = document.getElementById('scroller');
    const historyList = document.getElementById('history-list');

    // 1. ESC Panic
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://google.com');
    });

    // 2. Load Function with History
    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url || url === 'about:blank') return;

        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://duckduckgo.com/?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        
        addToHistory(url);
        
        // Forced Scroll Reset
        scroller.scrollTop = 0;
        // Focus the frame so keys (arrows/space) work for scrolling
        setTimeout(() => frame.focus(), 500);
    }

    function addToHistory(url) {
        const item = document.createElement('button');
        item.className = 'btn';
        const displayUrl = url.replace('https://','').split('/')[0];
        item.innerHTML = `🕒 ${displayUrl}`;
        item.onclick = () => load(url);
        
        // Keep only top 5 history items
        if (historyList.children.length >= 5) {
            historyList.removeChild(historyList.lastChild);
        }
        historyList.insertBefore(item, historyList.firstChild);
    }

    // 3. Cloaked Pop-Out
    function popOut() {
        const url = input.value;
        if (!url || url === 'about:blank') return;
        const win = window.open('about:blank', '_blank');
        if (win) {
            win.document.write(`
                <title>Portal</title>
                <body style="margin:0; overflow:hidden;">
                    <iframe src="${url}" style="width:100%; height:100vh; border:none;"></iframe>
                </body>
            `);
        }
    }

    function addBookmark() {
        const url = input.value;
        if (!url || url === 'about:blank') return;
        const list = document.getElementById('bookmark-list');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "📍 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        list.appendChild(b);
    }
</script>
</body>
</html>
