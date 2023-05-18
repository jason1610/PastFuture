<script lang="ts">
	import Background from '../components/home/assets/retro-background.jpg';
	import Splash from '../components/home/Splash.svelte';
	import OS from '../components/home/OS.svelte';
	import Timeline from '../components/home/Timeline.svelte';
</script>

<div class="container">
	<Splash />
	<Timeline />
	<!-- <div style="margin-top:50vh;" /> -->
	<OS />
	<div id="background">
		<img src={Background} alt="" />
	</div>
</div>

<style>
	.container {
		position: relative;
	}

	#background {
		position: absolute;
		top: -100px;
		left: 0;
		bottom: 0;
		overflow: hidden;
		width: 100%;
		height: 100%;
		z-index: -1;
	}

	#background img {
		filter: blur(50px);
		width: calc(100% + 100px);
		height: calc(100% + 100px);
		margin-left: -50px;
		margin-top: -50px;
		object-fit: cover;
		z-index: -1;
	}
</style>
