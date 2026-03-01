<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Embedded Web Portal</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            font-family: sans-serif;
            overflow: hidden; /* Prevents double scrollbars */
        }

        /* Top Navigation Bar */
        .navbar {
            height: 60px;
            background: #2c3e50;
            display: flex;
            align-items: center;
            padding: 0 20px;
            color: white;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        .navbar input {
            flex-grow: 1;
            margin: 0 15px;
            padding: 8px;
            border-radius: 4px;
            border: none;
        }

        .navbar button {
            padding: 8px 15px;
            background: #27ae60;
            border: none;
            color: white;
            border-radius: 4px;
            cursor: pointer;
        }

        /* The Iframe Window */
        #content-frame {
            width: 100%;
            height: calc(100vh - 60px); /* Subtracts navbar height */
            border: none;
            background: white;
        }
    </style>
</head>
<body>

    <div class="navbar">
        <strong>Portal:</strong>
        <input type="text" id="urlInput" placeholder="Enter URL (e.g., https://www.wikipedia.org)">
        <button onclick="loadPage()">Go</button>
    </div>

    <iframe id="content-frame" src="about:blank"></iframe>

    <script>
        function loadPage() {
            const input = document.getElementById('urlInput').value;
            const frame = document.getElementById('content-frame');
            
            // Basic check to ensure the URL starts with http/https
            if (input.startsWith('http://') || input.startsWith('https://')) {
                frame.src = input;
            } else {
                frame.src = 'https://' + input;
            }
        }

        // Allow pressing "Enter" to trigger the button
        document.getElementById("urlInput").addEventListener("keyup", function(event) {
            if (event.key === "Enter") {
                loadPage();
            }
        });
    </script>

</body>
</html>
