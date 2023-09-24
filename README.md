<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Portfolio</title>
    <style>
        /* Reset some default styles */
        body, h1, h2, p {
            margin: 0;
            padding: 0;
        }

        /* Set a background color and text color */
        body {
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
            color: #333;
        }

        /* Create a header with a background image */
        header {
            background-image: url('your-header-image.jpg');
            background-size: cover;
            text-align: center;
            padding: 100px 0;
        }

        /* Style the header text */
        header h1 {
            font-size: 36px;
            color: #fff;
        }

        /* Create a navigation menu */
        nav {
            background-color: #333;
            text-align: center;
        }

        nav ul {
            list-style: none;
            padding: 20px 0;
        }

        nav li {
            display: inline;
            margin: 0 20px;
        }

        nav a {
            text-decoration: none;
            color: #fff;
            font-weight: bold;
        }

        /* Create a container for your portfolio projects */
        .portfolio-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        /* Style individual portfolio items */
        .portfolio-item {
            margin-bottom: 30px;
        }

        /* Style your footer */
        footer {
            background-color: #333;
            color: #fff;
            text-align: center;
            padding: 20px 0;
        }
    </style>
</head>
<body>
    <header>
        <h1>Your Name</h1>
        <p>Web Developer | Graphic Designer | Photographer</p>
    </header>

    <nav>
        <ul>
            <li><a href="#about">About</a></li>
            <li><a href="#portfolio">Portfolio</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>

    <div class="portfolio-container" id="portfolio">
        <h2>Portfolio</h2>
        <!-- Replace the following with your portfolio project items -->
        <div class="portfolio-item">
            <img src="project1.jpg" alt="Project 1">
            <h3>Project 1</h3>
            <p>Description of Project 1.</p>
        </div>
        <div class="portfolio-item">
            <img src="project2.jpg" alt="Project 2">
            <h3>Project 2</h3>
            <p>Description of Project 2.</p>
        </div>
        <!-- Add more portfolio items as needed -->
    </div>

    <footer>
        &copy; 2023 Your Name. All rights reserved.
    </footer>
</body>
</html>
