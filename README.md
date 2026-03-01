<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="tab-title">Classes</title>
    <link rel="icon" type="image/png" href="https://ssl.gstatic.com/classroom/favicon.png">
    
    <style>
        /* RESET & CENTERing */
        * { box-sizing: border-box; }
        body, html { 
            margin: 0; padding: 0; height: 100vh; width: 100vw; 
            background: #121212; display: flex; justify-content: center; align-items: center;
            overflow: auto; /* This allows you to scroll the portal itself if it's too big */
        }

        /* THE GOOGLE CLASSROOM PRE-LOADER */
        #cloak-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #fff; z-index: 100000;
        }

        /* THE MAIN PORTAL BOX */
        #portal-content { 
            display: none; 
            width: 95vw; height: 90vh; /* Keeps it slightly smaller than screen to stay centered */
            background: #1a1a1a; border: 1px solid #333; border-radius: 8px;
            box-shadow: 0 0 30px rgba(0,0,0,0.5); overflow: hidden;
        }

        .app-grid { display: grid; grid-template-columns: 240px 1fr; height: 100%; width: 100%; }

        /* SIDEBAR */
        #sidebar {
            background: #181818; border-right: 1px solid #333;
            display: flex; flex-direction: column; overflow-y: auto; color: white; padding: 10px;
        }
        .nav-label { font-size: 10px; color: #666; text-transform: uppercase; margin: 15px 0 5px; }
        .btn { 
            width: 100%; padding: 10px; margin-bottom: 5px; 
            background: #252525; border: 1px solid #444; color: #ddd; 
            text-align: left; cursor: pointer; border-radius: 4px; font-size: 13px;
        }
        .btn:hover { background: #333; border-color: #00d2ff; }

        /* MAIN VIEW */
        #main-view { display: flex; flex-direction: column; background: #000; }
        
        #toolbar {
            height: 55px; background: #222; padding: 10px;
            display: flex; gap: 8px; align-items: center; border-bottom: 1px solid #333;
        }
        #toolbar input {
            flex-grow: 1; padding: 8px; border-radius: 4px; border: 1px solid #444;
            background: #000; color: #00ff00; font-family: monospace;
        }

        /* SCROLL FIX */
        #scroll-wrapper {
            flex-grow: 1; overflow: auto; background: #fff; position: relative;
        }
        /* We use a huge height to force the wrapper to scroll, not the iframe */
        #portal-frame { width: 100%; height: 3000px; border: none; display: block; }

        .ctrl { padding: 8px 12px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; }
    </style>
</head>
<body>

    <div id="cloak-screen">
        <iframe src="https://classroom.google.com" style="width: 100%; height: 100%; border: none;"></iframe>
    </div>

    <div id="portal-content">
        <div class="app-grid">
            <div id="sidebar">
                <h3 style="color:#00d2ff; margin: 10px 0;">PORTAL</h3>
                
                <div class="nav-label">Direct Mirrors</div>
                <button class="btn" onclick="load('https://www.bing.com')">🅱️ Bing Search</button>
                <button class="btn" onclick="load('https://www.wikipedia.org')">📖 Wikipedia</button>
                
                <div class="nav-label">Proxies</div>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">🚀 CroxyProxy</button>
                <button class="btn" onclick="load('https://www.blockaway.net')">🌐 BlockAway</button>
                
                <div class="nav-label">History</div>
                <div id="history-list"></div>
            </div>

            <div id="main-view">
                <div id="toolbar">
                    <input type="text" id="urlField" placeholder="Paste URL here...">
                    <button class="ctrl" style="background:#00d2ff" onclick="load()">GO</button>
                    <button class="ctrl" style="background:#ff8c00" onclick="popOut()">↗</button>
                </div>
                <div id="scroll-wrapper">
                    <iframe id="portal-frame" src="about:blank"></iframe>
                </div>
            </div>
        </div>
    </div>

<script>
    // 1. AUTOMATIC CLOAK SWITCH (3 Seconds)
    setTimeout(() => {
        document.getElementById('cloak-screen').style.display = 'none';
        document.getElementById('portal-content').style.display = 'block';
        window.history.replaceState({}, '', '/');
    }, 3000);

    // 2. PANIC (ESC)
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('urlField');
    const wrapper = document.getElementById('scroll-wrapper');

    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url) return;

        // Clean URL
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        wrapper.scrollTop = 0; // Reset scroll on load
    }

    function popOut() {
        const url = input.value;
        if (!url) return;
        const win = window.open('about:blank', '_blank');
        win.document.write(`
            <html>
                <head><title>Classes</title><link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png"></head>
                <body style="margin:0;overflow:hidden"><iframe src="${url}" style="width:100%;height:100vh;border:none"></iframe></body>
            </html>
        `);
    }
</script>
</body>
</html>
