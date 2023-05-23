<script lang="ts">
	import { onMount } from 'svelte';

	let iFrame: HTMLDivElement;
	let player: any;
	let isPlaying: boolean = false;

	onMount(() => {
		let tag = document.createElement('script');
		tag.src = 'https://www.youtube.com/iframe_api';
		document.body.appendChild(tag);
		(window as any).onYouTubeIframeAPIReady = () => {
			player = new (window as any).YT.Player(iFrame, {
				videoId: '2B-XwPjn9YY',
				playerVars: {
					playsinline: 1
				},
				enablejsapi: 1,
				allowfullscreen: 1,
				origin: 'https://pastfuture.io',
				events: {
					onStateChange: onPlayerStateChange
				}
			});
		};

		function onPlayerStateChange(event: any) {
			if (event.data == (window as any).YT.PlayerState.PLAYING) {
				isPlaying = true;
			}
			if (
				event.data == (window as any).YT.PlayerState.PAUSED ||
				event.data == (window as any).YT.PlayerState.ENDED
			) {
				isPlaying = false;
			}
		}
	});
</script>

<div class="container">
	<div class="content">
		<div bind:this={iFrame} id="player" />
		<div
			class="article-container"
			style={`${isPlaying ? 'grid-template-rows: 0fr; opacity: 0;' : ''}`}
		>
			<div class="article">
				<h1>Personal Computers</h1>
				<h2>1984</h2>
				<p>
					A game changer in personal computing, featuring a graphical user interface and a mouse,
					both radical innovations at the time. These features enabled more intuitive user
					interactions, moving away from the text-based command line interfaces common in that era.
					The Macintosh was also known for its compact all-in-one design and its memorable "1984"
					Super Bowl ad, marking it as a milestone in both technology and advertising history.
				</p>
			</div>
		</div>
	</div>
</div>

<style>
	.container {
		position: relative;
		width: 100%;
		padding: var(--carousel-padding);
		box-sizing: border-box;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: black;
	}

	.content {
		width: 100%;
		max-width: 700px;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		/* gap: 20px; */
	}

	.article-container {
		display: grid;
		grid-template-rows: 1fr;
		transition: all 1s ease-in-out;
		transition: all 1s ease-in-out;
	}

	.article {
		overflow: hidden;
		pointer-events: none;
	}

	#player {
		/* background-color: red; */
		/* width: 100px; */
		/* height: 100px; */
		width: 100%;
		height: 100%;
		max-width: 700px;
		max-height: 500px;
		border-radius: 10px;
		/* box-shadow: 0 0 50px 0px rgba(255, 255, 255, 0.2); */
		z-index: 1;
	}
</style>
