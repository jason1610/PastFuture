<script lang="ts">
	import Header from '../components/home/Header.svelte';
	import Footer from '../components/home/Footer.svelte';
	// import Globals from '../globals.css';
</script>

<Header />
<main>
	<slot />
</main>
<Footer />

<style>
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');

	main {
		width: 100%;
		min-height: 100lvh;
		padding: 0;
		margin: 0;
	}
</style>
