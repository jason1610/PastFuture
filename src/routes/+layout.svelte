<script lang="ts">
	import Header from '../components/home/Header.svelte';
</script>

<Header />
<main>
	<div style="height: 100px;" />
	<slot />
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

	:global(body, header) {
		margin: 0;
		padding: 0;
		font-family: 'Roboto', sans-serif;
	}

	:global(:root) {
		--text-color-primary: white;
		--text-color-secondary: #757575;
		--text-color-accent: #725cd2;

		--title-large-size: 90px;
		--title-large-weight: 700;

		--title-small-size: 30px;
		--title-small-weight: 700;

		--text-size: 18px;
		--text-weight: 400;

		--content-width: 1200px;
	}

	:global(html) {
		scroll-behavior: smooth;
		color-scheme: dark;
	}

	main {
		overflow-x: hidden;
		width: 100%;
		min-height: 100vh;
		min-height: 100lvh;
		padding: 0;
		margin: 0;
	}
</style>
