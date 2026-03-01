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
