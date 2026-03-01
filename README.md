<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Media & Search Portal</title>
    <style>
        * { box-sizing: border-box; }
        
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
            font-family: 'Segoe UI', sans-serif;
            background: #0f3460;
            display: flex;
            overflow: hidden;
        }

        /* Sidebar Navigation */
        .sidebar {
            width: 280px;
            min-width: 280px;
            background: #1a1a2e;
            display: flex;
            flex-direction: column;
            border-right: 2px solid #e94560;
            overflow-y: auto;
            z-index: 10;
        }

        .brand {
            padding: 25px;
            font-size: 1.4rem;
            font-weight: bold;
            color: #e94560;
            text-align: center;
            background: #16213e;
        }

        /* Search Tab Area */
        .search-tab {
            padding: 15px;
            background: #16213e;
            border-bottom: 1px solid #0f3460;
        }

        .search-tab input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: none;
            background: #0f3460;
            color: white;
            outline: none;
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
            transition: 0.2s;
        }

        .nav-link:hover {
            background: #e94560;
            color: white;
        }

        /* Main Viewer */
        .main-viewer {
            flex-grow: 1;
            position: relative;
            background: #000;
        }

        /* This is the key for scrolling - height must be 100% */
        #browser-frame {
            width: 100%;
            height: 100%;
            border: none;
            background: white;
        }

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
            text-align: center;
            padding: 20px;
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <div class="brand">PORTAL OS</div>
        
        <div class="category">🔍 Web Search</div>
        <div class="search-tab">
            <input type="text" id="urlInput" placeholder="Enter URL or Search..." onkeydown="if(event.key==='Enter') loadCustom()">
        </div>

        <div class="category">🎮 Gaming</div>
        <a class="nav-link" onclick="openSite('https://poki.com')"><span>🕹️</span> Poki Games</a>
        <a class="nav-link" onclick="openSite('https://www.crazygames.com')"><span>🔥</span> Crazy Games</a>

        <div class="category">📺 Media</div>
        <a class="nav-link" onclick="openSite('https://www.wikipedia.org')"><span>📖</span> Wikipedia</a>
        <a class="nav-link" onclick="openSite('https://archive.org')"><span>🏛️</span> Internet Archive</a>
        <a class="nav-link" onclick="openSite('https://www.ted.com')"><span>💡</span> TED Talks</a>
    </div>

    <div class="main-viewer">
        <div id="home-screen">
            <h1 style="color: #e94560;">System Ready</h1>
            <p style="color: #888;">Select a category or use the search bar to begin.</p>
        </div>
        
        <iframe id="browser-frame" src="about:blank"></iframe>
    </div>

    <script>
        function openSite(url) {
            const frame = document.getElementById('browser-frame');
            const home = document.getElementById('home-screen');
            home.style.display = 'none';
            frame.src = url;
        }

        function loadCustom() {
            const val = document.getElementById('urlInput').value;
            let targetUrl = "";

            // Check if it's a URL or a search query
            if (val.includes('.') && !val.includes(' ')) {
                targetUrl = val.startsWith('http') ? val : 'https://' + val;
            } else {
                // Use DuckDuckGo's embeddable search
                targetUrl = 'https://duckduckgo.com/search?q=' + encodeURIComponent(val);
            }
            openSite(targetUrl);
        }
    </script>

</body>
</html>
