<script lang="ts">
	import { onMount } from 'svelte';
	let currentDirectory: string;

	onMount(() => {	
		currentDirectory = window.location.pathname.split('/')[1];
		window.addEventListener('popstate', () => {
			currentDirectory = window.location.pathname.split('/')[1];
		});
	});
</script>

<div class="container">
	<a href="/" data-sveltekit-noscroll class={`${currentDirectory === '' ? 'selected' : ''}`}
		>Windows</a
	>
	<span>|</span>
	<a href="/mac" data-sveltekit-noscroll class={`${currentDirectory === 'mac' ? 'selected' : ''}`}
		>Mac</a
	>
	<span>|</span>
	<a
		href="/linux"
		data-sveltekit-noscroll
		class={`${currentDirectory === 'linux' ? 'selected' : ''}`}>Linux</a
	>
</div>

<style>
	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 20px;
		margin: 0 0 50px 0;
	}

	a {
		text-decoration: none;
		color: white;
		font-size: 20px;
		font-weight: 500;
	}

	a:hover {
		color: #a0a0a0;
	}

	.selected {
		color: rgb(125, 114, 173);
	}
</style>
