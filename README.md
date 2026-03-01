<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultimate Media Portal</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            font-family: 'Segoe UI', sans-serif;
            background: #0f3460;
            color: white;
            display: flex;
            overflow: hidden;
        }

        /* Sidebar Taskbar */
        .sidebar {
            width: 260px;
            background: #1a1a2e;
            display: flex;
            flex-direction: column;
            border-right: 2px solid #16213e;
            z-index: 10;
        }

        .brand {
            padding: 20px;
            font-size: 1.5rem;
            font-weight: bold;
            color: #e94560;
            text-align: center;
        }

        .category {
            padding: 15px 20px;
            font-size: 0.9rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .nav-link {
            padding: 12px 20px;
            text-decoration: none;
            color: #cfd8dc;
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            transition: 0.2s;
        }

        .nav-link:hover {
            background: #16213e;
            color: #00d2ff;
        }

        /* The Internal Browser Window */
        .main-viewer {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            background: #fff;
        }

        #browser-frame {
            width: 100%;
            height: 100%;
            border: none;
        }

        .status-bar {
            background: #16213e;
            padding: 5px 15px;
            font-size: 12px;
            color: #4ecca3;
            border-bottom: 1px solid #0f3460;
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <div class="brand">PORTAL OS</div>
        
        <div class="category">🎮 Gaming</div>
        <a class="nav-link" onclick="openSite('https://poki.com')"><span>🕹️</span> Poki Games</a>
        <a class="nav-link" onclick="openSite('https://www.crazygames.com')"><span>🔥</span> CrazyGames</a>
        <a class="nav-link" onclick="openSite('https://classic.minecraft.net/')"><span>⛏️</span> Minecraft Classic</a>

        <div class="category">🎬 Media</div>
        <a class="nav-link" onclick="openSite('https://www.wikipedia.org')"><span>📚</span> Wikipedia</a>
        <a class="nav-link" onclick="openSite('https://archive.org')"><span>🏛️</span> Internet Archive</a>

        <div class="category">🛠️ Tools</div>
        <a class="nav-link" onclick="openSite('https://www.calculator.net/')"><span>🧮</span> Calculator</a>
    </div>

    <div class="main-viewer">
        <div class="status-bar" id="status-text">Status: Home</div>
        <iframe id="browser-frame" src="about:blank"></iframe>
    </div>

    <script>
        function openSite(url) {
            const frame = document.getElementById('browser-frame');
            const status = document.getElementById('status-text');
            
            // Update the iframe source
            frame.src = url;
            
            // Update status text
            status.innerText = "Loading: " + url;
        }
    </script>

</body>
</html>
