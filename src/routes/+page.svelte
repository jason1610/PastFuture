<script lang="ts">
	import Terminal from '../components/windows/Terminal.svelte';
	import Xp from '../components/windows/Xp.svelte';
</script>

<div class="timeline">
	<Terminal />
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
