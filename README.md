<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Power Portal</title>
    <style>
        * { box-sizing: border-box; }
        body, html { margin: 0; height: 100%; font-family: sans-serif; background: #121212; color: white; overflow: hidden; }
        
        .container { display: flex; height: 100vh; width: 100vw; }

        /* Sidebar */
        .sidebar { width: 250px; background: #1e1e1e; border-right: 1px solid #333; display: flex; flex-direction: column; flex-shrink: 0; }
        .sidebar h2 { padding: 20px; color: #00d2ff; margin: 0; font-size: 1.2rem; }
        .nav-group { padding: 10px; flex-grow: 1; overflow-y: auto; }
        .btn { width: 100%; padding: 12px; margin-bottom: 5px; background: #2a2a2a; border: none; color: white; text-align: left; cursor: pointer; border-radius: 5px; }
        .btn:hover { background: #3d3d3d; }

        /* Main Window */
        .viewer { flex-grow: 1; display: flex; flex-direction: column; background: #000; position: relative; }
        
        /* The Address Bar */
        .address-bar { background: #222; padding: 10px; display: flex; gap: 10px; align-items: center; }
        .address-bar input { flex-grow: 1; padding: 8px; border-radius: 4px; border: 1px solid #444; background: #111; color: #44ff44; font-family: monospace; }

        /* THE FRAME */
        #portal-frame {
            width: 100%;
            flex-grow: 1;
            border: none;
            background: white;
            /* This ensures the frame is "on top" and clickable */
            position: relative;
            z-index: 1;
        }

        .error-msg { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #666; z-index: 0; }
    </style>
</head>
<body>

<div class="container">
    <div class="sidebar">
        <h2>PORTAL OS</h2>
        <div class="nav-group">
            <p style="font-size: 10px; color: #666;">GAMING</p>
            <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
            <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
            
            <p style="font-size: 10px; color: #666;">TOOLS</p>
            <button class="btn" onclick="load('https://www.wikipedia.org')">📖 Wikipedia</button>
            <button class="btn" onclick="load('https://duckduckgo.com/search?q=news')">🔍 DuckDuckGo</button>
        </div>
    </div>

    <div class="viewer">
        <div class="address-bar">
            <input type="text" id="urlField" placeholder="Enter URL here..." onkeydown="if(event.key==='Enter') load()">
            <button onclick="load()" style="padding: 8px 15px; cursor: pointer;">GO</button>
        </div>
        
        <div class="error-msg">If the site is blank, it blocked the portal.</div>
        
        <iframe 
            id="portal-frame" 
            src="about:blank"
            sandbox="allow-forms allow-modals allow-orientation-lock allow-pointer-lock allow-popups allow-popups-to-escape-sandbox allow-presentation allow-same-origin allow-scripts"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
            allowfullscreen>
        </iframe>
    </div>
</div>

<script>
    function load(manualUrl) {
        const frame = document.getElementById('portal-frame');
        const input = document.getElementById('urlField');
        let url = manualUrl || input.value;

        if (!url) return;

        // Auto-fix URL
        if (!url.startsWith('http')) {
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        
        // Focus the frame so scrolling works immediately
        frame.focus();
    }
</script>

</body>
</html>
