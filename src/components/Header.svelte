<script lang="ts">
	import { onMount } from 'svelte';

	let isAtTop: boolean = true;

	const handleScroll = () => {
		if (window.scrollY > 0) {
			isAtTop = false;
		} else {
			isAtTop = true;
		}
	};

	onMount(() => {
		window.addEventListener('scroll', handleScroll);
		return () => {
			window.removeEventListener('scroll', handleScroll);
		};
	});
</script>

<header class={`${isAtTop ? 'top' : 'blur'}`}>
	<div class="content">
		<h1>GUI-timeline</h1>
		<div class="right">
			<p>Github</p>
			<p>Other</p>
		</div>
	</div>
</header>

<style>
	header {
		height: 60px;
		position: fixed;
		display: flex;
		left: 0;
		right: 0;
		top: 0;
		justify-content: center;
		align-items: center;
		box-sizing: border-box;
		transition: all 0.5s ease-in-out;
	}

	header.top {
		height: 100px;
		background-color: transparent;
	}

	header.blur {
		background-color: rgba(0, 0, 0, 0.2);
		backdrop-filter: blur(10px);
		box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.05);
	}

	.content {
		width: 1200px;
		max-width: calc(100% - 40px);
		height: 100%;
		display: flex;
		align-items: center;
		font-weight: 500;
	}

	.right {
		display: flex;
		gap: 20px;
		margin-left: auto;
		display: flex;
		align-items: center;
	}

	h1 {
		font-size: 1.3rem;
		font-weight: 500;
	}
</style>
