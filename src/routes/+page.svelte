<script lang="ts">
	import MSDOS from '../components/windows/MSDOS.svelte';
	import Xp from '../components/windows/Xp.svelte';
</script>

<div class="timeline">
	<MSDOS />
	<Xp />
</div>

<style>
	.timeline {
		display: flex;
		flex-direction: column;
		/* justify-content: center; */
		align-items: center;
	}
</style>
