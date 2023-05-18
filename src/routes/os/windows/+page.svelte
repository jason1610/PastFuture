<script lang="ts">
	import MSDOS from '../../../components/os/windows/MSDOS.svelte';
	import Xp from '../../../components/os/windows/Xp.svelte';
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
