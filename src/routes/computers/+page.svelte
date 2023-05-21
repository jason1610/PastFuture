<script lang="ts">
	import MSDOS from '../../components/computers/MSDOS.svelte';
	import Xp from '../../components/computers/Xp.svelte';
	import Intro from '../../components/computers/Intro.svelte';
	import { register } from 'swiper/element/bundle';

	register();
</script>

<!-- <div class="spacer" /> -->
<div class="container">
	<swiper-container mousewheel-force-to-axis="true">
		<swiper-slide>
			<Intro />
		</swiper-slide>

		<swiper-slide>
			<MSDOS />
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
		height: 100%;
	}

	swiper-slide {
		display: flex;
		justify-content: center;
	}

	.container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		width: 100%;
	}
</style>
