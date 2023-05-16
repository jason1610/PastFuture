<script lang="ts">
	import Header from '../components/Header.svelte';
	import Intro from '../components/Intro.svelte';
	import Nav from '../components/Nav.svelte';
</script>

<Header />
<main>
	<Intro />
	<Nav />
	<slot />
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

	:global(body, header) {
		margin: 0;
		padding: 0;
		font-family: 'Roboto', sans-serif;
	}

	:global(html) {
		scroll-behavior: smooth;
		color-scheme: dark;
	}

	main {
		width: 100%;
		min-height: 100vh;
		min-height: 100lvh;
		padding: 0;
		margin: 0;
	}
</style>
