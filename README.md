<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Power Portal</title>
    <style>
        * { box-sizing: border-box; }
        body, html { margin: 0; padding: 0; height: 100%; font-family: sans-serif; background: #121212; color: white; overflow: hidden; }
        .container { display: flex; height: 100vh; width: 100vw; }

        /* Sidebar */
        .sidebar { width: 250px; background: #1e1e1e; border-right: 1px solid #333; display: flex; flex-direction: column; flex-shrink: 0; }
        .sidebar h2 { padding: 20px; color: #00d2ff; margin: 0; font-size: 1.2rem; border-bottom: 1px solid #333; }
        .nav-group { padding: 10px; flex-grow: 1; overflow-y: auto; }
        .nav-label { font-size: 10px; color: #888; margin: 15px 0 5px 5px; text-transform: uppercase; }
        .btn { width: 100%; padding: 12px; margin-bottom: 8px; background: #2a2a2a; border: 1px solid #444; color: white; text-align: left; cursor: pointer; border-radius: 6px; }
        .btn:hover { background: #3d3d3d; border-color: #00d2ff; }

        /* Main Viewer */
        .viewer { flex-grow: 1; display: flex; flex-direction: column; background: #000; position: relative; }
        .address-bar { background: #222; padding: 12px; display: flex; gap: 10px; align-items: center; border-bottom: 1px solid #333; }
        .address-bar input { flex-grow: 1; padding: 10px; border-radius: 4px; border: 1px solid #444; background: #000; color: #44ff44; font-family: monospace; }
        
        .go-btn { padding: 10px 20px; cursor: pointer; background: #00d2ff; color: black; border: none; border-radius: 4px; font-weight: bold; }
        .pop-btn { padding: 10px 15px; cursor: pointer; background: #ff8c00; color: black; border: none; border-radius: 4px; font-weight: bold; font-size: 18px; }

        iframe { width: 100%; height: 100%; border: none; background: white; flex-grow: 1; }
        .error-msg { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #555; z-index: 0; text-align: center; pointer-events: none; }
    </style>
</head>
<body>

<div class="container">
    <div class="sidebar">
        <h2>PORTAL OS</h2>
        <div class="nav-group">
            <div class="nav-label">Gaming</div>
            <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
            <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
            <div class="nav-label">Tools</div>
            <button class="btn" onclick="load('https://www.wikipedia.org')">📖 Wikipedia</button>
            <button class="btn" onclick="load('https://duckduckgo.com')">🔍 DuckDuckGo</button>
        </div>
    </div>

    <div class="viewer">
        <div class="address-bar">
            <input type="text" id="urlField" placeholder="Enter URL..." onkeydown="if(event.key==='Enter') load()">
            <button class="go-btn" onclick="load()">GO</button>
            <button class="pop-btn" onclick="popOut()" title="Cloaked Pop-out">↗</button>
        </div>
        <div class="error-msg">
            <p>If the site refuses to connect, use the <b>Orange Button</b>.</p>
            <p style="font-size: 11px;">Press <b>ESC</b> to Panic (Redirect to Google).</p>
        </div>
        <iframe id="portal-frame" src="about:blank"></iframe>
    </div>
</div>

<script>
    // PANIC BUTTON: Press ESC to hide everything
    window.addEventListener('keydown', function(e) {
        if (e.key === 'Escape') {
            window.location.replace('https://www.google.com');
        }
    });

    function load(manualUrl) {
        const frame = document.getElementById('portal-frame');
        const input = document.getElementById('urlField');
        let url = manualUrl || input.value;
        if (!url) return;
        if (!url.startsWith('http')) url = 'https://' + url;
        input.value = url;
        frame.src = url;
    }

    function popOut() {
        const url = document.getElementById('urlField').value;
        if (!url || url === "about:blank") return;
        
        // The Cloaking Trick
        const win = window.open('about:blank', '_blank');
        if (win) {
            win.document.title = "Portal Page";
            win.document.body.style.margin = '0';
            win.document.body.style.padding = '0';
            win.document.body.style.height = '100vh';
            win.document.body.style.overflow = 'hidden';

            const iframe = win.document.createElement('iframe');
            iframe.style.border = 'none';
            iframe.style.width = '100%';
            iframe.style.height = '100%';
            iframe.src = url;
            
            win.document.body.appendChild(iframe);
        }
    }
</script>
</body>
</html>
