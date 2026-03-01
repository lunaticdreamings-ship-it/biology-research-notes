<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="tab-title">Classes</title>
    <link id="tab-icon" rel="icon" type="image/png" href="https://ssl.gstatic.com/classroom/favicon.png">
    
    <style>
        * { box-sizing: border-box; }
        body, html { margin: 0; padding: 0; height: 100%; width: 100%; font-family: sans-serif; background: #fff; overflow: hidden; }

        /* THE GOOGLE CLASSROOM PRE-LOADER */
        #cloak-screen {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: #fff;
            z-index: 100000;
            display: flex;
            flex-direction: column;
        }

        /* THE PORTAL (Hidden at first) */
        #portal-content { display: none; height: 100vh; width: 100vw; background: #000; }
        .app-grid { display: flex; height: 100vh; width: 100vw; }

        #sidebar {
            width: 260px; background: #151515; border-right: 1px solid #333;
            display: flex; flex-direction: column; overflow-y: auto; color: white;
        }
        .nav-group { padding: 15px; }
        .btn { 
            width: 100%; padding: 10px; margin-bottom: 5px; 
            background: #202020; border: 1px solid #333; color: #ccc; 
            text-align: left; cursor: pointer; border-radius: 4px;
        }

        #main-view { flex-grow: 1; display: flex; flex-direction: column; position: relative; }
        #toolbar {
            height: 60px; background: #1a1a1a; padding: 10px;
            display: flex; gap: 8px; align-items: center; border-bottom: 1px solid #333;
        }
        #toolbar input {
            flex-grow: 1; padding: 10px; border-radius: 4px; border: 1px solid #444;
            background: #000; color: #00ff00; font-family: monospace;
        }

        /* UNIVERSAL SCROLL WRAPPER */
        #scroll-wrapper {
            flex-grow: 1; overflow-y: scroll !important; -webkit-overflow-scrolling: touch;
            background: #fff; position: relative;
        }
        iframe { width: 100%; height: 5000px; border: none; display: block; }

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
                <div class="nav-group">
                    <h3 style="color:#00d2ff">PORTAL OS</h3>
                    <button class="btn" onclick="load('https://duckduckgo.com')">🔍 DuckDuckGo</button>
                    <button class="btn" onclick="load('https://www.bing.com')">🅱️ Bing</button>
                    <button class="btn" onclick="load('https://www.blockaway.net')">🚀 BlockAway Proxy</button>
                    <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
                    <div id="history-list"></div>
                </div>
            </div>
            <div id="main-view">
                <div id="toolbar">
                    <input type="text" id="urlField" placeholder="Enter URL...">
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
    const cloak = document.getElementById('cloak-screen');
    const portal = document.getElementById('portal-content');
    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('urlField');

    // 1. THE AUTOMATIC REDIRECT (3 Seconds)
    setTimeout(() => {
        cloak.style.display = 'none';
        portal.style.display = 'block';
        // Attempting to mask URL
        window.history.replaceState({}, '', '/');
    }, 3000);

    // 2. PANIC BUTTON
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

    // 3. LOAD FUNCTION
    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url) return;
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://duckduckgo.com/?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }
        input.value = url;
        frame.src = url;
        document.getElementById('scroll-wrapper').scrollTop = 0;
    }

    // 4. CLOAKED POP-OUT
    function popOut() {
        const url = input.value;
        if (!url) return;
        const win = window.open('about:blank', '_blank');
        win.document.write(`
            <html>
                <head><title>Classes</title><link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png"></head>
                <body style="margin:0;overflow:hidden">
                    <iframe src="${url}" style="width:100%;height:100vh;border:none"></iframe>
                </body>
            </html>
        `);
    }
</script>
</body>
</html>
