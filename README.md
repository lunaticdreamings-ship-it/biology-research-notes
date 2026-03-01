<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title id="tab-title">Classes</title>
    <link rel="icon" type="image/png" href="https://ssl.gstatic.com/classroom/favicon.png">
    <style>
        * { box-sizing: border-box; }
        body, html { margin: 0; padding: 0; width: 100vw; height: 100vh; background: #000; overflow: hidden; font-family: 'Segoe UI', sans-serif; }

        /* PRE-LOADER CLOAK */
        #cloak { position: fixed; inset: 0; background: #fff; z-index: 10000; }

        /* MAIN APP LAYOUT */
        #app { display: none; width: 100vw; height: 100vh; position: relative; }

        /* SIDEBAR (Locked to Left) */
        #sidebar {
            position: absolute; left: 0; top: 0; bottom: 0; width: 260px;
            background: #121212; border-right: 1px solid #333; z-index: 50;
            display: flex; flex-direction: column; overflow-y: auto; color: #fff;
        }
        .sidebar-header { padding: 20px; border-bottom: 1px solid #333; color: #00d2ff; font-weight: bold; }
        .group { padding: 15px; }
        .label { font-size: 10px; color: #666; text-transform: uppercase; margin-bottom: 8px; letter-spacing: 1px; }
        .btn { 
            width: 100%; padding: 10px; margin-bottom: 5px; background: #1e1e1e; border: 1px solid #333;
            color: #ccc; text-align: left; cursor: pointer; border-radius: 4px; transition: 0.2s;
        }
        .btn:hover { background: #333; border-color: #00d2ff; color: #fff; }

        /* CONTENT AREA (Fill Remaining Space) */
        #content { position: absolute; left: 260px; right: 0; top: 0; bottom: 0; display: flex; flex-direction: column; }

        #toolbar {
            height: 60px; background: #1a1a1a; padding: 10px; display: flex; gap: 10px; 
            align-items: center; border-bottom: 1px solid #333;
        }
        #urlInput { flex: 1; padding: 10px; border-radius: 4px; border: 1px solid #444; background: #000; color: #00ff00; font-family: monospace; }
        
        /* THE VIEWPORT FIX */
        #view-wrapper { 
            flex: 1; width: 100%; position: relative; overflow: auto !important; 
            background: #fff; -webkit-overflow-scrolling: touch;
        }
        #portal-frame { width: 100%; height: 4000px; border: none; display: block; }

        .go { background: #00d2ff; color: #000; padding: 10px 20px; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; }
        .pop { background: #ff8c00; padding: 10px 15px; border: none; border-radius: 4px; cursor: pointer; }
    </style>
</head>
<body>

    <div id="cloak">
        <iframe src="https://classroom.google.com" style="width:100%; height:100%; border:none;"></iframe>
    </div>

    <div id="app">
        <div id="sidebar">
            <div class="sidebar-header">PORTAL OS</div>
            <div class="group">
                <div class="label">Search Engines</div>
                <button class="btn" onclick="load('https://www.bing.com')">Bing (Embedded)</button>
                <button class="btn" onclick="load('https://search.brave.com')">Brave Search</button>
                
                <div class="label">Advanced Proxies</div>
                <button class="btn" onclick="load('https://www.4everproxy.com')">4EverProxy (Stable)</button>
                <button class="btn" onclick="load('https://www.blockaway.net')">BlockAway</button>
                <button class="btn" onclick="load('https://www.croxyproxy.com')">CroxyProxy</button>

                <div class="label">Unblocked Games</div>
                <button class="btn" onclick="load('https://poki.com')">Poki</button>
                <button class="btn" onclick="load('https://www.crazygames.com')">CrazyGames</button>
                <button class="btn" onclick="load('https://www.coolmathgames.com')">CoolMath</button>

                <div class="label">Session</div>
                <div id="hist"></div>
                <button class="btn" style="border:1px dashed #555" onclick="save()">⭐ Bookmark current</button>
                <div id="bookmarks"></div>
            </div>
        </div>

        <div id="content">
            <div id="toolbar">
                <input type="text" id="urlInput" placeholder="Enter URL or Search...">
                <button class="go" onclick="load()">GO</button>
                <button class="pop" onclick="popOut()">↗</button>
            </div>
            <div id="view-wrapper">
                <iframe id="portal-frame" src="about:blank"></iframe>
            </div>
        </div>
    </div>

<script>
    // 1. Double Cloak Redirect
    setTimeout(() => {
        document.getElementById('cloak').style.display = 'none';
        document.getElementById('app').style.display = 'block';
    }, 2500);

    // 2. Panic Button
    window.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') window.location.replace('https://classroom.google.com');
    });

    const frame = document.getElementById('portal-frame');
    const input = document.getElementById('urlInput');
    const wrapper = document.getElementById('view-wrapper');

    function load(manualUrl) {
        let url = manualUrl || input.value;
        if (!url) return;

        // Auto-search logic
        if (!url.includes('.') && !url.startsWith('http')) {
            url = 'https://www.bing.com/search?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        wrapper.scrollTop = 0; // Fixes scroll on new load

        // History
        const h = document.getElementById('hist');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "🕒 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        if (h.children.length > 4) h.removeChild(h.lastChild);
        h.insertBefore(b, h.firstChild);
    }

    function popOut() {
        const url = input.value;
        if (!url) return;
        const win = window.open('about:blank', '_blank');
        win.document.write(`
            <html>
                <head><title>Classes</title><link rel="icon" href="https://ssl.gstatic.com/classroom/favicon.png"></head>
                <body style="margin:0;overflow:hidden">
                    <iframe src="${url}" style="width:100%;height:100vh;border:none"></iframe>
                    <script>setTimeout(()=>{window.history.replaceState(null,null,'about:blank')},100)<\/script>
                </body>
            </html>`);
    }

    function save() {
        const url = input.value;
        const d = document.getElementById('bookmarks');
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerHTML = "📍 " + url.split('//')[1].split('/')[0];
        b.onclick = () => load(url);
        d.appendChild(b);
    }
</script>
</body>
</html>
