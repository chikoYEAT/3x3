<script>
	import { onMount } from 'svelte';
	let query = '';
	let searchResults = [];
	let searchQueryUrl = '';
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
</script>

<div class="container">
	<input
		type="text"
		class="text"
		bind:value={query}
		on:keydown={(event) => event.keyCode === 13 && search()}
		placeholder="Search for anime..."
	/>
	<div id="search_results" class="scroll-container">
		{#each searchResults as item}
			<div class="card">
				<a href={item.url} class="card__image">
					<img loading="lazy" src={item.images.jpg.large_image_url} alt={item.title} />
				</a>
			</div>
		{/each}
	</div>
</div>

<style>
	.container {
		width: 100%;
		overflow: hidden;
	}
	.text {
		margin-top: 25px;
		text-align: center;
		width: 100%;
		margin-bottom: 20px;
	}
	.scroll-container {
		display: flex;
		overflow-x: auto;
		padding-bottom: 10px;
		white-space: nowrap;
	}
	.card {
		flex: 0 0 auto;
		margin-right: 10px;
		width: 200px; /* Set a fixed width */
	}
	.card__image {
		display: block;
		height: 300px; /* Set a fixed height */
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
