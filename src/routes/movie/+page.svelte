<script>
</script>

<header>
	<div class="logo">
		<h1>✡️ 3x3 Generator</h1>
	</div>
</header>

<section class="hero">
	<h1>Create Your Movie 3x3</h1>
	<p>Showcase your top 9 movie with our easy-to-use generator.</p>
	<button><a href="/movie/3x3">Create Now</a></button>
</section>

<section class="preview-grid">
	<h2 style="color: white;">Example 3x3 Grid</h2>
	<div class="grid-container">
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/mobile/thumbnail/xlarge/deadpool-and-wolverine-et00341295-1718018322.jpg" alt="Movie 1" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/mobile/thumbnail/xlarge/it-ends-with-us-et00398533-1720009408.jpg" alt="Anime 2" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/mobile/thumbnail/xlarge/kalki-2898-ad-et00352941-1718275859.jpg" alt="Anime 3" />
		</div>
		<div class="grid-item">
		  <img src="https://assets-in.bmscdn.com/iedb/movies/images/extra/vertical_logo/mobile/thumbnail/xxlarge/interstellar-et00019066-19-02-2021-02-25-12.jpg" alt="Anime 4" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/extra/vertical_logo/mobile/thumbnail/xxlarge/despicable-me-4-et00386901-1723182543.jpg" alt="Anime 5" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/extra/vertical_logo/mobile/thumbnail/xxlarge/spider-man-series-et00306968-30-03-2021-08-41-52.jpg" alt="Anime 6" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/mobile/thumbnail/xlarge/inside-out-2-et00384010-1705486755.jpg" alt="Anime 7" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/extra/vertical_logo/mobile/thumbnail/xxlarge/the-shawshank-redemption-et00069227-1707458004.jpg" alt="Anime 8" />
		</div>
		<div class="grid-item">
			<img src="https://assets-in.bmscdn.com/iedb/movies/images/extra/vertical_logo/mobile/thumbnail/xxlarge/batman-begins-et00065696-17-03-2021-04-09-19.jpg" alt="Anime 9" />
		</div>
	</div>
</section>
<section class="features">
	<p>Anime 3x3 Generator lets you create stunning grids in just a few clicks!</p>
	<ul>
		<li style="text-align: inherit;">
			<a href="https://github.com/chikoYEAT/3x3"
				><p style="color: lightblue;">github</p>
				(make sure to star repo🙏)</a
			>
		</li>
	</ul>
</section>

<style>
	header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 20px;
		background-color: #333;
		color: white;
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
		padding: 100px 20px;
		background: url('https://w0.peakpx.com/wallpaper/101/565/HD-wallpaper-batman-new-batmobile-robert-pattinson-the-batman-batman-2021-movies-movies.jpg') center center/cover no-repeat;
		color: white;
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

	.features {
		padding: 60px 20px;
		text-align: center;
		background-color: #222;
		color: white;
	}

	.features h2 {
		font-size: 2.5rem;
		margin-bottom: 20px;
	}

	.grid-container {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 10px;
		max-width: 800px;
		margin: 0 auto;
	}

	.grid-item img {
		width: 100%;
		height: auto;
		object-fit: cover;
		border-radius: 5px;
		transition:
			transform 0.3s ease,
			box-shadow 0.3s ease;
	}

	.grid-item img:hover {
		transform: scale(1.05);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
	}

	.preview-grid {
		padding: 40px 20px;
		background-color: #1a1a1a;
	}

	.preview-grid h2 {
		font-size: 2rem;
		margin-bottom: 20px;
		text-align: center;
	}

	ul {
		list-style-type: none;
		padding: 0;
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
