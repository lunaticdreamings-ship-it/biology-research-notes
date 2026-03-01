<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Media Hub</title>
    <style>
        * { box-sizing: border-box; }
        
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
            font-family: 'Segoe UI', sans-serif;
            background: #0f3460;
            display: flex; /* Sidebar and Viewer sit side-by-side */
            overflow: hidden; /* Prevents the main page from scrolling, only the iframe scrolls */
        }

        /* Sidebar - Fixed width, high z-index to stay on top of the left edge */
        .sidebar {
            width: 260px;
            min-width: 260px;
            background: #1a1a2e;
            display: flex;
            flex-direction: column;
            border-right: 2px solid #e94560;
            overflow-y: auto; /* Scroll if you add too many links */
        }

        .brand {
            padding: 30px 20px;
            font-size: 1.5rem;
            font-weight: bold;
            color: #e94560;
            text-align: center;
            border-bottom: 1px solid #16213e;
        }

        .category {
            padding: 20px 20px 10px 20px;
            font-size: 0.75rem;
            color: #888;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .nav-link {
            padding: 15px 20px;
            text-decoration: none;
            color: #cfd8dc;
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            transition: all 0.2s;
        }

        .nav-link:hover {
            background: #e94560;
            color: white;
        }

        /* Main Viewer - Fills the rest of the screen */
        .main-viewer {
            flex-grow: 1;
            position: relative;
            background: #fff;
        }

        #browser-frame {
            width: 100%;
            height: 100%;
            border: none;
            display: block; /* Ensures it takes up full space */
        }

        /* Home screen overlay (visible until a link is clicked) */
        #home-screen {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: #1a1a2e;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 5;
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <div class="brand">PORTAL OS</div>
        
        <div class="category">🎮 Gaming</div>
        <a class="nav-link" onclick="openSite('https://poki.com')"><span>🕹️</span> Poki Games</a>
        <a class="nav-link" onclick="openSite('https://www.crazygames.com')"><span>🔥</span> Crazy Games</a>
        <a class="nav-link" onclick="openSite('https://www.free-webgame.com/')"><span>👾</span> Web Games</a>

        <div class="category">📚 Resources</div>
        <a class="nav-link" onclick="openSite('https://www.wikipedia.org')"><span>📖</span> Wikipedia</a>
        <a class="nav-link" onclick="openSite('https://archive.org')"><span>🏛️</span> Internet Archive</a>

        <div class="category">⚙️ Options</div>
        <a class="nav-link" onclick="location.reload()"><span>🔄</span> Refresh Portal</a>
    </div>

    <div class="main-viewer">
        <div id="home-screen">
            <h1 style="color: #e94560;">Select a Destination</h1>
            <p style="color: #888;">Use the sidebar to launch a site inside the portal.</p>
        </div>
        
        <iframe id="browser-frame" src="about:blank"></iframe>
    </div>

    <script>
        function openSite(url) {
            const frame = document.getElementById('browser-frame');
            const home = document.getElementById('home-screen');
            
            // Hide the welcome screen
            home.style.display = 'none';
            
            // Load the new URL
            frame.src = url;
        }
    </script>

</body>
</html>
