<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Web Portal</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        header {
            background-color: #2c3e50;
            color: white;
            width: 100%;
            padding: 2rem 0;
            text-align: center;
        }

        .search-container {
            margin: 20px 0;
        }

        input[type="text"] {
            padding: 10px;
            width: 300px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            max-width: 1000px;
            width: 90%;
            padding: 20px;
        }

        .card {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            text-align: center;
            transition: transform 0.2s;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card h3 { margin-top: 0; color: #2980b9; }

        .btn {
            display: inline-block;
            margin-top: 10px;
            padding: 8px 16px;
            background-color: #2980b9;
            color: white;
            text-decoration: none;
            border-radius: 4px;
        }

        .btn:hover { background-color: #3498db; }
    </style>
</head>
<body>

<header>
    <h1>Personal Resource Portal</h1>
    <div class="search-container">
        <form action="https://www.google.com/search" method="GET">
            <input type="text" name="q" placeholder="Search the web...">
        </form>
    </div>
</header>

<div class="grid-container">
    <div class="card">
        <h3>Education</h3>
        <p>Learning and research tools.</p>
        <a href="https://en.wikipedia.org" class="btn">Wikipedia</a>
        <a href="https://www.khanacademy.org" class="btn">Khan Academy</a>
    </div>

    <div class="card">
        <h3>Development</h3>
        <p>Coding and documentation.</p>
        <a href="https://github.com" class="btn">GitHub</a>
        <a href="https://stackoverflow.com" class="btn">Stack Overflow</a>
    </div>

    <div class="card">
        <h3>Tools</h3>
        <p>Utility and productivity.</p>
        <a href="https://drive.google.com" class="btn">Google Drive</a>
        <a href="https://trello.com" class="btn">Trello</a>
    </div>
</div>

</body>
</html>
