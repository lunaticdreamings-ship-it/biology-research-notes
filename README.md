<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        /* BASE RESET */
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body, html { 
            width: 100vw; height: 100vh; 
            background: #000; overflow: hidden; 
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; 
        }

        /* STAGE 1: THE INITIAL CLOAK */
        #cloak-layer {
            position: fixed; inset: 0; background: #fff; z-index: 10000;
        }

        /* STAGE 2: THE FULL SCREEN APP */
        #app-root {
            display: none; /* Triggered by JS */
            width: 100vw; height: 100vh;
            display: grid;
            grid-template-columns: 260px 1fr;
            grid-template-rows: 60px 1fr;
        }

        /* SIDEBAR - Fixed and Scrollable */
        #sidebar {
            grid-row: 1 / 3;
            background: #111;
            border-right: 1px solid #333;
            display: flex;
            flex-direction: column;
            color: white;
            overflow-y: auto;
            z-index: 20;
        }
        .sidebar-brand { padding: 20px; font-weight: bold; color: #00d2ff; border-bottom: 1px solid #333; font-size: 1.2rem; }
        .nav-section { padding: 15px; }
        .nav-label { font-size: 10px; color: #555; text-transform: uppercase; letter-spacing: 1px; margin: 15px 0 8px; }
        
        .btn { 
            width: 100%; padding: 12px; margin-bottom: 6px; 
            background: #1e1e1e; border: 1px solid #333; color: #ccc; 
            text-align: left; cursor: pointer; border-radius: 6px; font-size: 13px;
            transition: all 0.2s;
        }
        .btn:hover { background: #333; border-color: #00d2ff; color: #fff; transform: translateX(3px); }

        /* TOOLBAR - Centered and Functional */
        #toolbar {
            grid-column: 2;
            background: #1a1a1a;
            padding: 10px 20px;
            display: flex;
            gap: 12px;
            align-items: center;
            border-bottom: 1px solid #333;
            z-index: 10;
        }
        #url-input {
            flex: 1;
            padding: 10px 15px;
            border-radius: 20px;
            border: 1px solid #444;
            background: #000;
            color: #44ff44;
            font-family: 'Courier New', monospace;
            outline: none;
        }
        #url-input:focus { border-color: #00d2ff; }

        .ctrl-btn { padding: 10px 18px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; transition: 0.2s; }
        .btn-go { background: #00d2ff; color: #000; }
        .btn-pop { background: #ff8c00; color: #000; }
        .ctrl-btn:hover { opacity: 0.8; transform: translateY(-1px); }

        /* CONTENT AREA - The Scroll Fix */
        #viewport {
            grid-column: 2;
            background: #fff;
            position: relative;
            overflow: auto !important; /* This creates the master scrollbar */
            -webkit-overflow-scrolling: touch;
        }
        #portal-frame {
            width: 100%;
            height: 5000px; /* Forces height so the PARENT container scrolls */
            border: none;
            display: block;
        }

        /* SCROLLBAR STYLING */
        #sidebar::-webkit-scrollbar { width: 5px; }
        #sidebar::-webkit-scrollbar-thumb { background: #333; }
        #viewport::-webkit-scrollbar { width: 10px; }
        #viewport::-webkit-scrollbar-thumb { background: #444; }
    </style>
</head>
<body>

    <div id="cloak-layer">
        <iframe src="https://classroom.google.com" style="width:100%; height:100%; border:none;"></iframe>
    </div>

    <div id="app-root" style="display: none;">
        <div id="sidebar">
            <div class="sidebar-brand">PORTAL OS v4</div>
            
            <div class="nav-section">
                <div class="nav-label">Search Mirrors</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing (Embedded)</button>
                <button class="btn" onclick="load('https://search.brave.com')">Brave Search</button>
                <button class="btn" onclick="load('https://duckduckgo.com')">DuckDuckGo</button>

                <div class="nav-label">Stable Proxies (Use these for YT/Games)</div>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://www.4everproxy.com')">🔗 4EverProxy</button>

                <div class="nav-label">Unblocked Games</div>
                <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
                <button class="btn" onclick="load('https://www.coolmathgames.com')">➕ CoolMath</button>
                <button class="btn" onclick="load('https://armorgames.com')">🛡️ Armor Games</button>

                <div class="nav-label">History</div>
                <div id="history-container"></div>

                <div class="nav-label">Saved Sites</div>
                <button class="btn" style="border: 1px dashed #555;" onclick="saveBookmark()">⭐ Add Bookmark</button>
                <div id="bookmark-container"></div>
            </div>
        </div>

        <div id="toolbar">
            <input type="text" id="url-input" placeholder="Type URL or search query..." onkeydown="if(event.key==='Enter') load()">
            <button class="ctrl-btn btn-go" onclick="load()">GO</button>
            <button class="ctrl-btn btn-pop" onclick="popOut()" title="Open in Hidden Tab">↗</button>
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

    // 1. CLOAK LOGIC: Show real classroom, then swap to portal
    setTimeout(() => {
        cloak.style.display = 'none';
        app.style.display = 'grid';
        // Cleanup the browser history so it looks like it started at /
        window.history.replaceState({}, '', '/');
    }, 2800);

    // 2. PANIC BUTTON (ESC)
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

    // 3. MASTER LOAD FUNCTION
    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url || url === 'about:blank') return;

        // Auto-search logic
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        
        // Reset scroll position to top
        viewport.scrollTop = 0;

        updateHistory(url);
    }

    // 4. HISTORY & BOOKMARKS
    function updateHistory(url) {
        const container = document.getElementById('history-container');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "🕒 " + url.split('//')[1].split('/')[0].substring(0, 20);
        b.onclick = () => load(url);
        if (container.children.length > 4) container.removeChild(container.lastChild);
        container.insertBefore(b, container.firstChild);
    }

    function saveBookmark() {
        const url = input.value;
        if (!url || url === 'about:blank') return;
        const container = document.getElementById('bookmark-container');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "📍 " + url.split('//')[1].split('/')[0].substring(0, 20);
        b.onclick = () => load(url);
        container.appendChild(b);
    }

    // 5. CLOAKED POP-OUT (about:blank injection)
    function popOut() {
        const url = input.value;
        if (!url) return;
        const win = window.open('about:blank', '_blank');
        win.document.write(`
            <html>
                <head>
                    <title>Classes</title>
                    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
                </head>
