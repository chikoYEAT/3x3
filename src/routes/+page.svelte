<script>
    // Script section if needed for interactivity
</script>

<header>
    <div class="logo">
        <h1>✡️ 3x3 Generator</h1>
    </div>
</header>

<section class="hero">
    <h1>Create Your 3x3</h1>
    <p>If it's anime or movie, you can make it in a few clicks</p>
    <div class="button-container">
        <button><a href="/anime">Anime</a></button>
        <button><a href="/movie">Movie</a></button>
    </div>
</section>

<section class="features">
    <p>Anime 3x3 Generator lets you create stunning grids in just a few clicks!</p>
    <ul>
        <li style="text-align: inherit;">
            <a href="https://github.com/chikoYEAT/3x3">
                <p style="color: lightblue;">GitHub</p>
                (make sure to star the repo 🙏)
            </a>
        </li>
    </ul>
</section>

<style>
    header {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 20px;
        background-color: #333;
        color: white;
        height: 10vh;
    }

    .logo h1 {
		font-family: 'Arial', sans-serif;
		font-size: 2rem;
		margin: 0;
	}

    .hero {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-align: center;
        padding: 20px;
        background: url('https://w0.peakpx.com/wallpaper/739/144/HD-wallpaper-cillian-murphy-in-oppenheimer-movie.jpg') center center/cover no-repeat;
        color: white;
        height: 70vh;
    }

    
    .hero h1 {
        font-size: 3rem;
        margin-bottom: 20px;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.7);
    }

    .hero p {
        font-size: 1.5rem;
        margin-bottom: 40px;
        text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.7);
    }

    .hero button {
        padding: 15px 30px;
        font-size: 1.2rem;
        background-color: #ff4757;
        color: white;
        border: none;
        cursor: pointer;
        border-radius: 5px;
        transition: background-color 0.3s ease;
        text-decoration: none;
    }

    .hero button:hover {
        background-color: #ff6b81;
    }

    .hero button a {
        color: white;
        text-decoration: none;
    }

    .features {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        text-align: center;
        background-color: #222;
        color: white;
        height: 20vh;
        padding: 20px;
    }

    ul {
        list-style-type: none;
        padding: 0;
        margin: 0;
    }

    li {
        font-size: 1.2rem;
        margin-bottom: 10px;
    }

    li a {
        color: #61dafb;
        text-decoration: none;
    }

    li a:hover {
        text-decoration: underline;
    }
</style>
