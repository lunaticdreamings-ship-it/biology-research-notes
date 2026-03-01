<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Workspace</title>
    <style>
        :root {
            --bg-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            --card-bg: rgba(255, 255, 255, 0.1);
            --text-color: #ffffff;
        }

        body {
            margin: 0;
            font-family: 'Segoe UI', sans-serif;
            background: var(--bg-gradient);
            height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            color: var(--text-color);
        }

        .search-section {
            margin-top: 10vh;
            text-align: center;
            width: 100%;
        }

        .search-bar {
            width: 40%;
            min-width: 300px;
            padding: 15px 25px;
            border-radius: 30px;
            border: none;
            backdrop-filter: blur(10px);
            background: rgba(255, 255, 255, 0.2);
            color: white;
            font-size: 1.1rem;
            outline: none;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        .search-bar::placeholder { color: rgba(255, 255, 255, 0.7); }

        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 25px;
            width: 80%;
            max-width: 800px;
            margin-top: 50px;
        }

        .dial-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-decoration: none;
            color: white;
            transition: transform 0.3s ease;
        }

        .dial-item:hover { transform: scale(1.1); }

        .icon-box {
            width: 80px;
            height: 80px;
            background: var(--card-bg);
            backdrop-filter: blur(5px);
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            margin-bottom: 10px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .label { font-size: 0.9rem; font-weight: 500; }
    </style>
</head>
<body>

    <div class="search-section">
        <h1>Welcome Back</h1>
        <form action="https://www.google.com/search" method="GET">
            <input type="text" name="q" class="search-bar" placeholder="Search the web..." autofocus>
        </form>
    </div>

    <div class="grid-container">
        <a href="https://www.wikipedia.org" class="dial-item">
