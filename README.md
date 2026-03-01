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
        .btn { width: 100%; padding: 12px; margin-bottom: 5px; background: #2a2a2a; border: none; color: white; text-align: left; cursor: pointer; border-radius: 5px; transition: 0.2s; }
        .btn:hover { background: #3d3d3d; color: #00d2ff; }

        /* Main Window */
        .viewer { flex-grow: 1; display: flex; flex-direction: column; background: #000; position: relative; }
        
        /* The Address Bar */
        .address-bar { background: #222; padding: 10px; display: flex; gap: 8px; align-items: center; border-bottom: 1px solid #333; }
        .address-bar input { flex-grow: 1; padding: 10px; border-radius: 4px; border: 1px solid #444; background: #111; color: #44ff44; font-family: monospace; outline: none; }
        .address-bar input:focus { border-color: #00d2ff; }
        
        .control-btn { padding: 10px 15px; cursor: pointer; background: #333; color: white; border: none; border-radius: 4px; font-weight: bold; }
        .control-btn:hover { background: #444; }
        .pop-out-btn { background: #005a70; }
        .pop-out-btn:hover { background: #008cb0; }

        /* THE FRAME */
        #portal-frame {
            width: 100%;
            flex-grow: 1;
            border: none;
            background: white;
            position: relative;
            z-index: 1;
        }

        .error-msg { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #666; z-index: 0; text-align: center; }
    </style>
</head>
<body>

<div class="container">
    <div class="sidebar">
        <h2>PORTAL OS</h2>
        <div class="nav-group">
            <p style="font-size: 10px; color: #666; letter-spacing: 1px;">GAMING</p>
            <button class="btn" onclick="load('https://poki.com')">🕹️ Poki</button>
            <button class="btn" onclick="load('https://www.crazygames.com')">🔥 CrazyGames</button>
            
            <p style="font-size: 10px; color: #666; letter-spacing: 1px; margin-top: 20px;">TOOLS</p>
            <button class="btn" onclick="load('https://www.wikipedia.org')">📖 Wikipedia</button>
            <button class="btn" onclick="load('https://duckduckgo.com')">🔍 DuckDuckGo</button>
        </div>
    </div>

    <div class="viewer">
        <div class="address-bar">
            <input type="text" id="urlField" placeholder="Enter URL or search..." onkeydown="if(event.key==='Enter') load()">
            <button class="control-btn" onclick="load()">GO</button>
            <button class="control-btn pop-out-btn" onclick="popOut()" title="Open in new about:blank tab">↗</button>
        </div>
        
        <div class="error-msg">
            <p>If the site is blank or says "Refused to connect,"<br>it's blocking the portal frame.</p>
            <p style="font-size: 12px; color: #444;">Use the ↗ button to open it in a new tab.</p>
        </div>
        
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

        // Smart URL handling
        if (!url.includes('.') && !url.startsWith('http')) {
            // If it's just a word, search DuckDuckGo
            url = 'https://duckduckgo.com/?q=' + encodeURIComponent(url);
        } else if (!url.startsWith('http')) {
            // If it looks like a domain, add https
            url = 'https://' + url;
        }

        input.value = url;
        frame.src = url;
        frame.focus();
    }

    function popOut() {
        const url = document.getElementById('urlField').value;
        if (!url || url === "about:blank") {
            alert("Please enter or load a URL first!");
            return;
        }
        
        // This opens a new window
        const newWindow = window.open('about:blank', '_blank');
        
        // This injects the URL into that about:blank window
        if (newWindow) {
            newWindow.location.href = url;
        } else {
            alert("Popup blocked! Please allow popups for this portal.");
        }
    }
</script>

</body>
</html>
