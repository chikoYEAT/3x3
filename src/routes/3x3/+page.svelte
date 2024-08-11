<script>
	import { onMount } from 'svelte';
	import html2canvas from 'html2canvas';
	let query = '';
	let searchResults = [];
	let searchQueryUrl = '';
	let selectedImages = [];
	let showPreview = false;

	onMount(() => {
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

	const selectImage = (item) => {
		if (selectedImages.length < 9 && !selectedImages.some((img) => img.mal_id === item.mal_id)) {
			selectedImages = [...selectedImages, item];
		}
	};

	const removeImage = (item) => {
		selectedImages = selectedImages.filter((img) => img.mal_id !== item.mal_id);
	};

	const togglePreview = () => {
		if (selectedImages.length !== 9) {
			alert('Please select exactly 9 images to preview.');
			return;
		}
		showPreview = !showPreview;
	};

	const downloadImage = async () => {
		if (selectedImages.length !== 9) {
			alert('Please select exactly 9 images before downloading.');
			return;
		}

		// Ensure the images are fully loaded before capturing
		const previewGrid = document.querySelector('#preview-grid');
		if (!previewGrid) {
			alert('Preview grid not found.');
			return;
		}

		// Temporarily remove the gap before capturing
		previewGrid.style.gap = '0px';

		const images = previewGrid.querySelectorAll('img');
		await Promise.all(
			Array.from(images).map(
				(img) =>
					new Promise((resolve) => {
						if (img.complete) resolve();
						else img.onload = resolve;
					})
			)
		);

		// Set the scale for upscaling (e.g., 2x for double the resolution)
		const scale = 2;

		// Increase the size of the preview grid
		previewGrid.style.transform = `scale(${scale})`;
		previewGrid.style.transformOrigin = 'top left';

		// Adjust the canvas size
		const canvas = await html2canvas(previewGrid, {
			useCORS: true,
			width: previewGrid.offsetWidth * scale,
			height: previewGrid.offsetHeight * scale,
			scale: 1 // Scale is handled by CSS transform
		});

		// Reset the preview grid after capturing
		previewGrid.style.transform = 'scale(1)';
		previewGrid.style.gap = '10px';

		// Create and download the image
		const link = document.createElement('a');
		link.download = '3x3_anime.png';
		link.href = canvas.toDataURL();
		link.click();
	};
</script>

<header>
	<div class="logo">
		<h1><a href="/">✡️ 3x3 Generator</a></h1>
	</div>
</header>
<div>
	<section class="centered-container">
		<input
			type="text"
			class="text"
			bind:value={query}
			on:keydown={(event) => event.keyCode === 13 && search()}
			placeholder="Search for anime..."
		/>
	</section>

	<div class="selected-container">
		{#each selectedImages as item}
			<div class="selected-card">
				<img src={item.images.jpg.large_image_url} alt={item.title} />
				<button class="remove-btn" on:click={() => removeImage(item)}>X</button>
			</div>
		{/each}
	</div>
	<div id="search_results" class="scroll-container" style="padding-top: 30px; margin-top: 30px ;">
		{#each searchResults as item}
			<div class="card" on:click={() => selectImage(item)}>
				<div class="card__image">
					<img loading="lazy" src={item.images.jpg.large_image_url} alt={item.title} />
				</div>
			</div>
		{/each}
	</div>
	<p style="color: white; padding: 10px; justify-content: center; text-align: center;">
		Selected images: {selectedImages.length}/9
	</p>
	<div class="centered-container">
		<button
			class="preview-btn"
			on:click={togglePreview}
			style="justify-content: center; text-align: center; align-items: center;">Preview</button
		>
	</div>

	{#if showPreview}
		<div class="preview-overlay" on:click={togglePreview}>
			<div class="preview-content" on:click|stopPropagation>
				<div class="preview-grid" id="preview-grid">
					{#each selectedImages as item}
						<img src={item.images.jpg.large_image_url} alt={item.title} />
					{/each}
				</div>
				<button
					class="download-btn"
					on:click={downloadImage}
					style="justify-content: center; align-items: center;">Download</button
				>
			</div>
		</div>
	{/if}
	<div class="footer">
		<p>made with ❤️ by chiko</p>
	</div>
</div>

<style>
	:global(body, html) {
		background-color: #222;
		margin: 0;
		padding: 0;
		width: 100%;
		height: 100%;
		overflow-x: hidden;
		box-sizing: border-box;
	}

	header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 20px;
		background-color: #333;
		color: white;
	}

	.footer {
		bottom: 0;
		right: 0;
		padding: 10px;
		color: white;
		display: flex;
		flex-direction: column;
		justify-content: center;
		text-align: center;
		width: 100%;
	}

	.preview-overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 1000;
	}

	.preview-content {
		background-color: #222;
		padding: 20px;
		border-radius: 10px;
		max-width: 90%;
		max-height: 90%;
		overflow: auto;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
	}

	.preview-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-template-rows: repeat(3, 1fr);
		gap: 10px;
		margin-bottom: 20px;
		width: 900px;
		height: 900px;
	}

	.preview-grid img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		border-radius: 5px;
	}
	.logo h1 {
		font-family: 'Arial', sans-serif;
		font-size: 2rem;
		margin: 0;
	}
	.download-btn {
		appearance: none;
		background-color: #ffffff;
		border-width: 0;
		box-sizing: border-box;
		color: #000000;
		cursor: pointer;
		display: inline-block;
		font-family: Clarkson, Helvetica, sans-serif;
		font-size: 14px;
		font-weight: 500;
		letter-spacing: 0;
		line-height: 1em;
		margin: 0;
		opacity: 1;
		outline: 0;
		padding: 1.5em 2.2em;
		position: relative;
		text-align: center;
		text-decoration: none;
		text-rendering: geometricprecision;
		text-transform: uppercase;
		transition:
			opacity 300ms cubic-bezier(0.694, 0, 0.335, 1),
			background-color 100ms cubic-bezier(0.694, 0, 0.335, 1),
			color 100ms cubic-bezier(0.694, 0, 0.335, 1);
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
		vertical-align: baseline;
		white-space: nowrap;
	}

	.download-btn:before {
		animation: opacityFallbackOut 0.5s step-end forwards;
		backface-visibility: hidden;
		background-color: #ebebeb;
		clip-path: polygon(-1% 0, 0 0, -25% 100%, -1% 100%);
		content: '';
		height: 100%;
		left: 0;
		position: absolute;
		top: 0;
		transform: translateZ(0);
		transition:
			clip-path 0.5s cubic-bezier(0.165, 0.84, 0.44, 1),
			-webkit-clip-path 0.5s cubic-bezier(0.165, 0.84, 0.44, 1);
		width: 100%;
	}

	.download-btn:hover:before {
		animation: opacityFallbackIn 0s step-start forwards;
		clip-path: polygon(0 0, 101% 0, 101% 101%, 0 101%);
	}

	.download-btn:after {
		background-color: #ffffff;
	}

	.selected-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		gap: 10px;
		padding: 20px; /* Padding inside the border */
		height: 368px; /* Fixed height */
		width: 80vw; /* Fixed width */
		overflow-y: auto;
		border: 2px dashed grey; /* Dashed border style */
		border-radius: 12px;
		box-sizing: border-box; /* Ensures padding is included in the element's total width and height */
		margin: 0 auto; /* Centering the container */
	}

	.selected-card {
		position: relative;
		width: 100px;
		height: 150px;
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
	.centered-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin: 20px 0;
	}

	.text {
		background-color: rgb(51, 51, 51);
		color: white;
		width: 90%;
		max-width: 600px;
		padding: 10px;
		border-radius: 30px;
		text-align: center;
		font-size: 1.2em;
		margin-bottom: 10px;
	}

	.preview-btn {
		appearance: none;
		background-color: #ffffff;
		border-width: 0;
		color: #000000;
		cursor: pointer;
		display: inline-block;
		font-family: Clarkson, Helvetica, sans-serif;
		font-size: 14px;
		font-weight: 500;
		letter-spacing: 0;
		line-height: 1em;
		padding: 1.5em 2.2em;
		margin: 10px auto;
		text-align: center;
		text-transform: uppercase;
		transition: background-color 100ms cubic-bezier(0.694, 0, 0.335, 1);
	}

	.scroll-container {
		flex-grow: 1;
		display: flex;
		overflow-x: auto;
		padding-left: 20px;
		align-items: center;
		width: 100%;
		height: 30%;
	}

	.card {
		flex: 0 0 auto;
		margin-right: 20px;
		width: auto;
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

	.scroll-container::-webkit-scrollbar {
		display: none;
	}

	.scroll-container {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}

	/* Media Query for Mobile Devices */
	@media (max-width: 450px) {
		.container {
			width: 100%;
			padding: 0;
			overflow-x: hidden;
		}

		.preview-content {
			padding: 10px;
			max-width: 150vw;
			max-height: 150vh;
			overflow: hidden;
		}

		.preview-grid {
			width: 150vw;
			height: 150vw;
			max-width: 300px;
			max-height: 300px;
		}

		.selected-container {
			max-height: auto;
			width: 100%;
			padding: 5px;
			overflow-x: hidden;
		}

		.selected-card {
			width: 90px;
			height: 120px;
		}

		.text {
			width: 90%;
			font-size: 1em;
		}

		.preview-btn,
		.download-btn {
			font-size: 8px;
		}

		.card__image {
			height: 200px;
		}

		.card {
			max-width: 200px;
		}
	}

	@media (max-width: 1080px) {
		.preview-content {
			padding: 10px;
			max-width: 100vw;
			max-height: 90vh;
		}

		.preview-grid {
			width: 600px;
			height: 600px;
		}

		.preview-btn,
		.download-btn {
			font-size: 10px;
			margin-bottom: 10px;
		}
	}
</style>
