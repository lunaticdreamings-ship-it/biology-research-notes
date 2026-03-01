<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Media Portal v3</title>
    <style>
        :root {
            --bg: #0f172a;
            --panel: #1e293b;
            --accent: #38bdf8;
            --text: #f1f5f9;
        }

        body, html {
            margin: 0; padding: 0; height: 100%;
            font-family: 'Inter', sans-serif;
            background: var(--bg);
            color: var(--text);
            display: flex;
            overflow: hidden;
        }

        /* Sidebar - Responsive Collapse */
        .sidebar {
            width: 300px;
            background: var(--panel);
            display: flex;
            flex-direction: column;
            border-right: 1px solid #334155;
            transition: 0.3s;
        }

        .header { padding: 20px; text-align: center; border-bottom: 1px solid #334155; }
        
        .search-box { padding: 15px; }
        .search-box input {
            width: 100%; padding: 12px;
            border-radius: 8px; border: none;
            background: #0f172a; color: white;
            outline: 2px solid #334155;
        }

        .menu-area { flex-grow: 1; overflow-y: auto; padding: 10px; }
        
        .cat-label { 
            padding: 15px 10px 5px; font-size: 11px; 
            text-transform: uppercase; color: #94a3b8; letter-spacing: 1px;
        }

        .nav-item {
            display: flex; align-items: center; gap: 12px;
            padding: 12px; margin: 4px 0;
            border-radius: 8px; cursor: pointer;
            text-decoration: none; color: inherit;
        }
        .nav-item:hover { background: #334155; }

        /* Main Viewer */
        .viewport {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .top-bar {
            height: 50px; background: #1e293b;
            display: flex; align-items: center;
            padding: 0 20px; justify-content: space-between;
        }

        #main-frame {
            width: 100%; height: 100%;
            border: none; background: #f8fafc;
        }

        /* Home Dashboard Overlay */
        #dashboard {
            position: absolute; top: 0; left: 0;
            width: 100%; height: 100%;
            background: var(--bg);
            z-index: 5; padding: 40px;
            overflow-y: auto;
        }

        .card-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 20px;
        }

        .card {
            background: var(--panel); padding: 20px;
            border-radius: 12px; text-align: center;
            cursor: pointer; border: 1px solid #334155;
        }
        .card:hover { border-color: var(--accent); transform: translateY(-2px); }
        .card i { font-size: 2rem; display: block; margin-bottom: 10px; }
    </style>
</head>
<body>

<div class="sidebar">
    <div class="header"><h2>PORTAL 3.0</h2></div>
    <div class="search-box">
        <input type="text" id="urlIn" placeholder="Search or type URL..." onkeydown="if(event.key==='Enter') launch()">
    </div>
    <div class="menu-area">
        <div class="cat-label">Main Menu</div>
        <div class="nav-item" onclick="goHome()">🏠 Home Dashboard</div>
        
        <div class="cat-label">🕹️ Gaming</div>
        <div class="nav-item" onclick="launch('https://poki.com')">🎮 Poki Games</div>
        <div class="nav-item" onclick="launch('https://www.crazygames.com')">🔥 Crazy Games</div>
        
        <div class="cat-label">🎬 Streaming</div>
        <div class="nav-item" onclick="launch('https://www.youtube.com/embed')">📺 YouTube TV</div>
    </div>
</div>

<div class="viewport">
    <div class="top-bar">
        <span id="url-display">Status: Dashboard</span>
        <button onclick="popOut()" style="background:var(--accent); border:none; padding:5px 10px; border-radius:4px; cursor:pointer;">External Open ↗</button>
    </div>

    <div id="dashboard">
        <h1>Welcome</h1>
        <div class="card-grid">
            <div class="card" onclick="launch('https://poki.com')"><i>🕹️</i>Games</div>
            <div class="card" onclick="launch('https://duckduckgo.com')"><i>🔍</i>Search</div>
            <div class="card" onclick="launch('https://en.wikipedia.org')"><i>📚</i>Wiki</div>
        </div>
    </div>

    <iframe id="main-frame" src="about:blank"></iframe>
</div>

<script>
    const frame = document.getElementById('main-frame');
    const dash = document.getElementById('dashboard');
    const display = document.getElementById('url-display');

    function launch(url) {
        let target = url || document.getElementById('urlIn').value;
        if (!target) return;

        // Auto-format URL
        if (!target.startsWith('http')) {
            if (target.includes('.')) target = 'https://' + target;
            else target = 'https://duckduckgo.com/?q=' + encodeURIComponent(target);
        }

        dash.style.display = 'none';
        frame.src = target;
        display.innerText = "Viewing: " + target;
    }

    function goHome() {
        dash.style.display = 'block';
        frame.src = 'about:blank';
        display.innerText = "Status: Dashboard";
    }

    function popOut() {
        if (frame.src !== "about:blank") {
            window.open(frame.src, '_blank');
        }
    }
</script>

</body>
</html>
