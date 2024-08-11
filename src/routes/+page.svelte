<script>
	import { onMount } from 'svelte';
	import html2canvas from 'html2canvas';
	let query = '';
	let searchResults = [];
	let searchQueryUrl = '';
	let selectedImages = [];

	onMount(() => {
		// Load html2canvas script
		const script = document.createElement('script');
		script.src = 'https://html2canvas.hertzen.com/dist/html2canvas.min.js';
		script.async = true;
		document.body.appendChild(script);
	});
	const search = async () => {
		const requestUrl = `https://api.jikan.moe/v4/anime?q=${query}&sfw`;
		searchQueryUrl = 'fetching...';
		const startTime = new Date().getTime();
		const response = await fetch(requestUrl);
		const data = await response.json();
		const timeTaken = new Date().getTime() - startTime;
		searchQueryUrl = requestUrl;
		searchResults = data.data.slice(0, 16); // Limit to 16 results
	};

	const handleContentClick = (event) => {
		event.stopPropagation();
	};
	const selectImage = (item) => {
		if (selectedImages.length < 9 && !selectedImages.some((img) => img.mal_id === item.mal_id)) {
			selectedImages = [...selectedImages, item];
		}
	};

	const removeImage = (item) => {
		selectedImages = selectedImages.filter((img) => img.mal_id !== item.mal_id);
	};

	const downloadImage = async () => {
		if (selectedImages.length !== 9) {
			alert('Please select exactly 9 images before downloading.');
			return;
		}

		const canvas = await html2canvas(selectedContainerRef, {
			width: 900,
			height: 900,
			scale: 1
		});

		const link = document.createElement('a');
		link.download = '3x3_anime.png';
		link.href = canvas.toDataURL();
		link.click();
	};
</script>

<div class="container">
	<input
		type="text"
		class="text"
		bind:value={query}
		on:keydown={(event) => event.keyCode === 13 && search()}
		placeholder="Search for anime..."
	/>
	<div class="selected-container">
		{#each selectedImages as item}
			<div class="selected-card">
				<img src={item.images.jpg.large_image_url} alt={item.title} />
				<button class="remove-btn" on:click={() => removeImage(item)}>X</button>
			</div>
		{/each}
	</div>
	<div id="search_results" class="scroll-container">
		{#each searchResults as item}
			<div class="card" on:click={() => selectImage(item)}>
				<div class="card__image">
					<img loading="lazy" src={item.images.jpg.large_image_url} alt={item.title} />
				</div>
			</div>
		{/each}
	</div>
	<button class="preview-btn" on:click={togglePreview}>Preview</button>
</div>

<style>
	:global(body, html) {
		margin: 0;
		padding: 0;
		height: 100%;
		width: 100%;
		overflow: hidden;
		background-color: rgb(40, 42, 54);
	}
	.preview-overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.8);
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 1000;
	}

	.preview-content {
		background-color: white;
		padding: 20px;
		border-radius: 10px;
		max-width: 90%;
		max-height: 90%;
		overflow: auto;
	}

	.preview-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 10px;
		margin-bottom: 20px;
	}

	.preview-grid img {
		width: 100%;
		height: auto;
		object-fit: cover;
		border-radius: 5px;
	}

	.preview-btn,
	.download-btn {
		background-color: #4caf50;
		border: none;
		color: white;
		padding: 15px 32px;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 16px;
		margin: 20px auto;
		cursor: pointer;
		border-radius: 5px;
	}
	.container {
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
	}
	.selected-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		gap: 10px;
		padding: 10px;
		max-height: 40vh;
		overflow-y: auto;
	}
	.selected-card {
		position: relative;
		width: 100px;
		height: 150px;
	}
	.download-btn {
		background-color: #4caf50;
		border: none;
		color: white;
		padding: 15px 32px;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 16px;
		margin: 20px auto;
		cursor: pointer;
		border-radius: 5px;
	}
	.selected-card img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		border-radius: 5px;
	}
	.remove-btn {
		position: absolute;
		top: 5px;
		right: 5px;
		background: rgba(255, 255, 255, 0.7);
		border: none;
		border-radius: 50%;
		width: 20px;
		height: 20px;
		font-size: 12px;
		cursor: pointer;
	}
	.text {
		background-color: #000000;
		color: white;
		margin: 20px auto;
		width: 80%;
		max-width: 600px;
		padding: 10px;
		border-radius: 30px;
		text-align: center;
		font-size: 1.2em;
	}
	.scroll-container {
		flex-grow: 1;
		display: flex;
		overflow-x: auto;
		padding-left: 20px;
		align-items: center;
	}
	.card {
		flex: 0 0 auto;
		margin-right: 20px;
		width: autow;
		max-width: 300px;
		cursor: pointer;
	}
	.card__image {
		display: block;
		height: 300px;
		max-height: 500px;
		overflow: hidden;
	}
	.card__image img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		border-radius: 10px;
	}
	/* Hide the scrollbar */
	.scroll-container::-webkit-scrollbar {
		display: none;
	}
	.scroll-container {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}
</style>
