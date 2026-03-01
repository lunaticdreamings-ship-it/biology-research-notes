<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Media Hub</title>
    <style>
        :root {
            --sidebar-width: 240px;
            --accent: #00d2ff;
            --bg: #1a1a2e;
            --card-bg: #16213e;
        }

        body {
            margin: 0;
            font-family: 'Segoe UI', sans-serif;
            background: var(--bg);
            color: white;
            display: flex;
            height: 100vh;
        }

        /* Sidebar Navigation */
        nav {
            width: var(--sidebar-width);
            background: #0f3460;
            display: flex;
            flex-direction: column;
            padding: 20px;
            border-right: 1px solid rgba(255,255,255,0.1);
        }

        nav h2 { font-size: 1.2rem; margin-bottom: 30px; color: var(--accent); }

        .nav-item {
            padding: 12px;
            margin: 5px 0;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .nav-item:hover { background: rgba(255,255,255,0.1); }
        .nav-item.active { background: var(--accent); color: #1a1a2e; font-weight: bold; }

        /* Main Content Area */
        main {
            flex-grow: 1;
            padding: 40px;
            overflow-y: auto;
        }

        .category-content { display: none; }
        .category-content.active { display: block; }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .site-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            text-decoration: none;
            color: white;
            border: 1px solid rgba(255,255,255,0.05);
            transition: transform 0.2s;
        }

        .site-card:hover { transform: translateY(-5px); border-color: var(--accent); }
        .site-card span { display: block; font-size: 2rem; margin-bottom: 10px; }
    </style>
</head>
<body>

    <nav>
        <h2>🚀 My Portal</h2>
        <div class="nav-item active" onclick="showCategory('gaming')">🎮 Gaming</div>
        <div class="nav-item" onclick="showCategory('streaming')">🎬 Streaming</div>
        <div class="nav-item" onclick="showCategory('social')">📱 Social</div>
        <div class="nav-item" onclick="showCategory('tools')">🛠️ Tools</div>
    </nav>

    <main id="main-content">
        <div id="gaming" class="category-content active">
            <h1>Gaming Hub</h1>
            <div class="grid">
                <a href="https://poki.com" target="_blank" class="site-card"><span>🕹️</span>Poki Games</a>
                <a href="https://www.crazygames.com" target="_blank" class="site-card"><span>🔥</span>CrazyGames</a>
                <a href="https://itch.io" target="_blank" class="site-card"><span>👾</span>itch.io</a>
            </div>
        </div>

        <div id="streaming" class="category-content">
            <h1>Movies & Streaming</h1>
            <div class="grid">
                <a href="https://www.netflix.com" target="_blank" class="site-card"><span>📺</span>Netflix</a>
                <a href="https://www.youtube.com" target="_blank" class="site-card"><span>▶️</span>YouTube</a>
                <a href="https://www.twitch.tv" target="_blank" class="site-card"><span>🟣</span>Twitch</a>
            </div>
        </div>

        <div id="social" class="category-content">
            <h1>Social Media</h1>
            <div class="grid">
                <a href="https://discord.com" target="_blank" class="site-card"><span>💬</span>Discord</a>
                <a href="https://reddit.com" target="_blank" class="site-card"><span>🤖</span>Reddit</a>
            </div>
        </div>
    </main>

    <script>
        function showCategory(id) {
            // Remove active class from all categories
            document.querySelectorAll('.category-content').forEach(el => el.classList.remove('active'));
            // Remove active class from all nav items
            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
            
            // Add active class to selected
            document.getElementById(id).classList.add('active');
            event.currentTarget.classList.add('active');
        }
    </script>

</body>
</html>
