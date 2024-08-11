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

		const canvas = await html2canvas(previewGrid, {
			useCORS: true, // Ensure images from other origins are handled
			width: previewGrid.offsetWidth,
			height: previewGrid.offsetHeight,
			scale: 1
		});

		// Reset the gap after capturing
		previewGrid.style.gap = '10px';

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

	{#if showPreview}
		<div class="preview-overlay" on:click={togglePreview}>
			<div class="preview-content" on:click|stopPropagation>
				<div class="preview-grid" id="preview-grid">
					{#each selectedImages as item}
						<img src={item.images.jpg.large_image_url} alt={item.title} />
					{/each}
				</div>
				<button class="download-btn" on:click={downloadImage}>Download</button>
			</div>
		</div>
	{/if}
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
		background-color: rgb(40, 42, 54);
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 1000;
	}

	.preview-content {
		background-color: rgb(0, 0, 0);
		padding: 20px;
		border-radius: 10px;
		max-width: 90%;
		max-height: 90%;
		overflow: auto;
	}

	.preview-grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		grid-template-rows: repeat(3, 1fr);
		gap: 10px; /* Adjusted to ensure proper spacing between images */
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
		height: 100vh;
		display: flex;
		flex-direction: column;
		position: fixed;
		align-items: center;
		z-index: 1000;
		width: 100%;
		height: 100%;
		overflow: auto;
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
	/* Hide the scrollbar */
	.scroll-container::-webkit-scrollbar {
		display: none;
	}
	.scroll-container {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}

	/* Media Query for Mobile Devices */
	@media (max-width: 600px) {
		.preview-content {
			padding: 10px;
			max-width: 90vw;
			max-height: 100vh;
		}

		.preview-grid {
			width: 300px;
			height: 300px;
		}

		.selected-container {
			max-height: auto;
			width: 80%;
			padding: 5px;
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
			font-size: 14px;
			align-self: center;
			justify-self: unset;
			justify-content: center;
		}

		.card__image {
			height: 200px;
		}

		.card {
			max-width: 200px;
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
				font-size: 16px;
			}
		}
	}
</style>
