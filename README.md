<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Power Portal</title>
    <style>
        * { box-sizing: border-box; }
        
        /* Ensure the base page doesn't bounce or scroll weirdly */
        body, html { 
            margin: 0; 
            padding: 0; 
            height: 100%; 
            width: 100%;
            font-family: sans-serif; 
            background: #121212; 
            color: white; 
            overflow: hidden; 
        }

        .container { 
            display: flex; 
            height: 100vh; 
            width: 100vw; 
        }

        /* Sidebar: Fixed width, scrolls internally if list is long */
        .sidebar { 
            width: 250px; 
            background: #1e1e1e; 
            border-right: 1px solid #333; 
            display: flex; 
            flex-direction: column; 
            flex-shrink: 0;
            z-index: 10; /* Keeps it above the iframe */
        }

        .sidebar h2 { padding: 20px; color: #00d2ff; margin: 0; font-size: 1.2rem; border-bottom: 1px solid #333; }
        .nav-group { padding: 10px; flex-grow: 1; overflow-y: auto; }
        .nav-label { font-size: 10px; color: #888; margin: 15px 0 5px 5px; text-transform: uppercase; }

        .btn { 
            width: 100%; padding: 12px; margin-bottom: 8px; 
            background: #2a2a2a; border: 1px solid #444; color: white; 
            text-align: left; cursor: pointer; border-radius: 6px; 
        }
        .btn:hover { background: #3d3d3d; border-color: #00d2ff; }

        /* Main Viewer Area */
        .viewer { 
            flex-grow: 1; 
            display: flex; 
            flex-direction: column; 
            background: #000; 
            position: relative;
            min-width: 0; /* Important for flex-grow to work right */
        }

        .address-bar { 
            background: #222; 
            padding: 12px; 
            display: flex; 
            gap: 10px; 
            align-items: center; 
            border-bottom: 1px solid #333;
            z-index: 10; 
        }

        .address-bar input { 
            flex-grow: 1; 
            padding: 10px; 
            border-radius: 4px; 
            border: 1px solid #444; 
            background: #000; 
            color: #44ff44; 
            font-family: monospace; 
        }
        
        .go-btn { padding: 10px 20px; cursor: pointer; background: #00d2ff; color: black; border: none; border-radius: 4px; font-weight: bold; }
        .pop-btn { padding: 10px 15px; cursor: pointer; background: #ff8c00; color: black; border: none; border-radius: 4px; font-weight: bold; font-size: 18px; }

        /* The Frame Container - This handles the scrolling */
        .frame-container {
            flex-grow: 1;
            position: relative;
            width: 100%;
            height: 100%;
            overflow: auto; /* Allows the container to scroll if the iframe is huge */
            -webkit-overflow-scrolling: touch;
        }

        iframe { 
            width: 100%; 
            height: 100%; 
            border: none; 
            background: white; 
            display: block;
        }

        .error-msg { 
            position: absolute; 
            top: 50%; left: 50%; 
            transform: translate(-50%, -50%); 
            color: #555; 
            z-index: 0; 
            text-align: center; 
            pointer-events: none; 
        }
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
            <p>If blank, use the <b>Orange Button</b>.</p>
            <p style="font-size: 11px;">Press <b>ESC</b> to Panic.</p>
        </div>

        <div class="frame-container">
            <iframe id="portal-frame" src="about:blank"></iframe>
        </div>
    </div>
</div>

<script>
    // PANIC BUTTON: Instant redirect
    window.addEventListener('keydown', function(e) {
        if (e.key === 'Escape') {
            window.location.replace('https://www.google.com');
        }
    });

    function load(manualUrl) {
        const frame = document.getElementById('
