<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classes</title>
    <link rel="icon" type="image/png" href="https://ssl.gstatic.com/classroom/favicon.png">
    
    <style>
        * { box-sizing: border-box; }
        body, html { margin: 0; padding: 0; height: 100%; width: 100%; font-family: sans-serif; background: #000; overflow: hidden; }

        #sidebar {
            position: fixed;
            left: 0; top: 0; bottom: 0;
            width: 260px;
            background: #151515;
            border-right: 1px solid #333;
            z-index: 9999;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
            color: white;
        }
        #sidebar h2 { padding: 20px; color: #00d2ff; margin: 0; font-size: 1.1rem; border-bottom: 1px solid #333; }
        .nav-group { padding: 15px; }
        .nav-label { font-size: 10px; color: #666; text-transform: uppercase; margin: 15px 0 8px; letter-spacing: 1px; }

        .btn { 
            width: 100%; padding: 10px; margin-bottom: 5px; 
            background: #202020; border: 1px solid #333; color: #ccc; 
            text-align: left; cursor: pointer; border-radius: 4px; font-size: 13px;
        }
        .btn:hover { background: #333; color: #00d2ff; }

        #main-content { margin-left: 260px; height: 100vh; display: flex; flex-direction: column; position: relative; }

        #toolbar {
            height: 60px;
            background: #1a1a1a;
            padding: 10px;
            display: flex;
            gap: 8px;
            align-items: center;
            border-bottom: 1px solid #333;
            z-index: 1000;
        }
        #toolbar input {
            flex-grow: 1;
            padding: 10px;
            border-radius: 4px;
            border: 1px solid #444;
            background: #000;
            color: #00ff00;
            font-family: monospace;
        }

        .ctrl { padding: 8px 12px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; color: white; background: #333; }
        .go { background: #00d2ff; color: #000; }
        .pop { background: #ff8c00; color: #000; }

        /* SCROLL FIX: Interaction Layer */
        #viewport-container {
            flex: 1;
            overflow: auto !important;
            background: #fff;
            position: relative;
            /* This ensures the mouse wheel event isn't swallowed by a dead iframe */
            display: block;
        }

        #portal-frame {
            width: 100%;
            height: 300vh; /* Extra tall to force the parent to scroll */
            border: none;
            display: block;
        }
    </style>
</head>
<body>

<div id="sidebar">
    <h2>PORTAL OS</h2>
    <div class="nav-group">
        <div class="nav-label">Search & Proxies</div>
        <button class="btn" onclick="load('https://duckduckgo.com')">🔍 DuckDuckGo</button>
        <button class="btn" onclick="load('https://www.bing.com')">🅱️ Bing</button>
        <button class="btn" onclick="load('https://www.blockaway.net')">🚀 BlockAway (Stable)</button>
        
        <div class="nav-label">Unblocked Games</div>
        <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
        <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
        <button class="btn" onclick="load('https://www.coolmathgames.com')">➕ CoolMath</button>

        <div class="nav-label">History</div>
        <div id="history-list"></div>
        
        <div class="nav-label">Saved</div>
        <button class="btn" onclick="addBookmark()" style="border: 1px dashed #00d2ff;">⭐ Save Site</button>
        <div id="bookmark-list"></div>
    </div>
</div>

<div id="main-content">
    <div id="toolbar">
        <input type="text" id="urlField" placeholder="Enter URL..." onkeydown="if(event.key==='Enter') load()">
        <button class="ctrl go" onclick="load()">GO</button>
        <button class="ctrl pop" onclick="popOut()">↗</button>
    </div>
    
    <div id="viewport-container">
        <iframe id="portal-frame" src="about:blank"></iframe>
    </div>
</div>

<script>
    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('urlField');
    const container = document.getElementById('viewport-container');
    const historyList = document.getElementById('history-list');

    // ESC Panic Redirect
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

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
        
        // Add to history
        const item = document.createElement('button');
        item.className = 'btn';
        item.innerHTML = "🕒 " + url.split('//')[1].split('/')[0];
        item.onclick = () => load(url);
        if (historyList.children.length >= 5) historyList.removeChild(historyList.lastChild);
        historyList.insertBefore(item, historyList.firstChild);

        container.scrollTop = 0;
    }

    function popOut() {
        const url = input.value;
        if (!url) return;

        // Opens the new window
        const win = window.open('about:blank', '_blank');
        
        // Cloak the Tab Title and Icon immediately
        win.document.write(`
            <html>
                <head>
                    <title>Google Classroom</title>
                    <link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png">
                </head>
                <body style="margin:0;overflow:hidden;">
                    <iframe src="${url}" style="width:100%;height:100vh;border:none"></iframe>
                    <script>
                        // After 3 seconds, clear the URL internally
                        setTimeout(() => {
                            window.history.replaceState({}, '', 'about:blank');
                        }, 3000);
                    <\/script>
                </body>
            </html>
        `);
    }

    function addBookmark() {
        const url = input.value;
        if (!url || url === 'about:blank') return;
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "📍 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        document.getElementById('bookmark-list').appendChild(b);
    }
</script>
</body>
</html>
