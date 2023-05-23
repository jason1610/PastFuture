<script lang="ts">
	import MSDOS from '../../components/computers/MSDOS.svelte';
	import Xp from '../../components/computers/Xp.svelte';
	import Intro from '../../components/computers/ComputerIntro.svelte';
	import IBM from '../../components/computers/IBM.svelte';
	import Macintosh from '../../components/computers/Macintosh.svelte';
	import { register } from 'swiper/element/bundle';
	register();
</script>

<!-- <div class="spacer" /> -->
<div class="container">
	<swiper-container mousewheel-force-to-axis="true" navigation="true">
		<swiper-slide>
			<Intro />
		</swiper-slide>
		<swiper-slide>
			<IBM />
		</swiper-slide>
		<swiper-slide>
			<MSDOS />
		</swiper-slide>
		<swiper-slide>
			<Macintosh />
		</swiper-slide>
		<swiper-slide>
			<Xp />
		</swiper-slide>
	</swiper-container>
</div>

<style>
	.spacer {
		height: 100px;
	}
	swiper-container {
		width: 100%;
	}

	swiper-slide {
		/* height: 1200px; */
		display: flex;
		justify-content: center;
		height: auto !important;
	}

	:global(.swiper-button-next) {
		background-color: red;
		width: 500px;
		height: 500px;
	}

	.container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		width: 100%;
	}
</style>
