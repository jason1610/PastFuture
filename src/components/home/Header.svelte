<script lang="ts">
	import { onMount } from 'svelte';

	let isAtTop: boolean = true;
	let onMobile: boolean = false;

	const handleScroll = () => {
		if (window.scrollY > 0) {
			isAtTop = false;
		} else {
			isAtTop = true;
		}
	};

	onMount(() => {
		if (window.innerWidth < 800) {
			onMobile = true;
		}

		window.addEventListener('scroll', handleScroll);
		return () => {
			window.removeEventListener('scroll', handleScroll);
		};
	});
</script>

<header class={`${isAtTop ? 'top' : 'scroll'}` + `${onMobile ? ' mobile' : ''}`}>
	<div class="content">
		<div class="logo">
			<a href="/">PastFuture</a>
		</div>
		<div class="links">
			<a href="https://1610.fr" target="_blank">1610</a>
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
		z-index: 1000;
		pointer-events: none;
	}

	header.top {
		height: 90px;
		background-color: transparent;
	}

	header.scroll {
		background-color: rgba(0, 0, 0, 0.2);
		backdrop-filter: blur(10px);
		box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.05);
		-webkit-backdrop-filter: blur(10px);
	}

	header.mobile {
		background-color: transparent;
	}

	header.mobile.top {
		height: 60px;
	}

	header.mobile.scroll {
		opacity: 0;
	}

	.content {
		width: 1200px;
		max-width: calc(100% - 40px);
		height: 100%;
		display: flex;
		align-items: center;
		font-weight: 500;
	}

	.links {
		display: flex;
		gap: 20px;
		margin-left: auto;
		display: flex;
		align-items: center;
	}

	.logo a {
		font-size: 1.3rem;
		font-weight: 500;
		text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
	}

	a {
		pointer-events: all;
		color: var(--text-color-primary);
		text-decoration: none;
	}
</style>
