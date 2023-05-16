<script lang="ts">
	import Start from './assets/xp-start.png';

	let startMenuOpen: boolean = false;
	let draggingFile: number = -1;
	let desktopIcons: { x: number; y: number; name: string; img: string };

	const handleStart = () => {
		startMenuOpen = !startMenuOpen;
	};

	function dragStart(e: MouseEvent, file: number) {
		draggingFile = file;
	}

	function drag(event: MouseEvent) {
		if (draggingFile !== -1) {
			const newPosition = { x: event.clientX, y: event.clientY };
				if (position.x === newPosition.x && position.y === newPosition.y) {
					return { x: newPosition.x + 10, y: newPosition.y + 10 };
				} else {
					return position;
				}
			});
		}
	}

	function dragEnd() {
		draggingFile = -1;
	}
</script>

<div class="container">
	<div class="desktop">
		{#each desktopIcons as file, index (file)}
			<div class="file" style={`left: ${positions[index].x}px; top: ${positions[index].y}px;`}>
				<div
					class="icon"
					on:mousedown={(e) => dragStart(e, index)}
					on:mouseup={dragEnd}
					on:mousemove={drag}
				/>
				{file}
			</div>
		{/each}
		{#if startMenuOpen}
			<div class="menu">
				<div class="top">
					<div class="profile">
						<img src="" alt="" />
					</div>
				</div>
				<!-- <div id="start-menu-close" on:click={closeStartMenu}>X</div>
				<div id="start-menu-content">Start Menu Content</div> -->
			</div>
		{/if}
		<div class="taskbar">
			<button class="start" on:click={handleStart}>
				<img src={Start} alt="start" />
			</button>
		</div>
	</div>
	<div class="description">
		<h1>Windows XP</h1>
		<p>
			Windows XP is a graphical operating system. It was released in 2001. It was the first consumer
			version of Windows to be built on the Windows NT kernel. It was also the first version of
			Windows to use product activation to combat software piracy.
		</p>
	</div>
</div>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 50px 0;
		padding: 20px;
	}

	.desktop {
		height: 500px;
		aspect-ratio: 4/3;
		max-height: 500px;
		max-width: calc(100vw - 40px);
		background-color: rgb(37, 33, 33);
		position: relative;
		display: flex;
		flex-direction: column;
		/* border-radius: 20px; */
		justify-content: flex-end;
		background-image: url('./assets/xp-background.jpg');
		background-size: cover;
		background-position: center;
		overflow: hidden;
		box-sizing: border-box;
	}

	.taskbar {
		width: 100%;
		height: 40px;
		background-color: hsl(221, 73%, 50%);
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 73%, 58%) 5%,
			hsl(223, 72%, 49%) 25%,
			hsl(221, 73%, 50%) 90%,
			hsl(222, 74%, 35%) 100%
		);
		display: flex;
		align-items: center;
		overflow: hidden;
		z-index: 1;
	}

	.start {
		height: 100%;
		width: 100px;
		background-color: #4da052;
		background: linear-gradient(
			hsl(117, 48%, 39%) 0%,
			hsl(113, 33%, 51%) 5%,
			hsl(119, 53%, 37%) 25%,
			hsl(118, 39%, 51%) 100%
		);
		border: none;
		border-radius: 0 10px 10px 0 / 0 19px 25px 0;
		box-shadow: inset -3px -1px 5px 2px #0e4e26, 0.5px 0 1px 0px #a0aba475;
		font-weight: 700;
		font-size: 20px;
	}

	.start:hover {
		cursor: pointer;
		filter: brightness(1.1);
	}

	.start img {
		width: 100%;
		height: 100%;
		object-fit: contain;
	}

	.menu {
		position: absolute;
		bottom: 40px;
		left: 0;
		width: 300px;
		height: 350px;
		background-color: white;
		overflow: hidden;
		border-radius: 5px 5px 0 0;
		box-shadow: 2px 5px 5px 2px hsla(0, 0%, 0%, 0.5);
		z-index: 0;
	}

	.menu .top {
		background-color: hsl(221, 73%, 50%);
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 80%, 63%) 5%,
			hsl(223, 72%, 49%) 10%,
			#3d87ea 98%,
			hsl(223, 72%, 49%) 100%
		);
		height: 70px;
		display: flex;
		align-items: center;
		padding: 0 5px;
	}

	.menu .top .profile {
		width: 50px;
		aspect-ratio: 1;
		object-fit: contain;
		background-color: white;
		border-radius: 3px;
		padding: 1px;
		box-shadow: 1px 0px 3px 1px #171c4875;
	}

	.menu .top .profile img {
		width: 100%;
		height: 100%;
		object-fit: contain;
		background-color: blue;
		display: block;
	}

	.file {
		position: absolute;
		display: flex;
		padding: 10px;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.icon {
		width: 70px;
		aspect-ratio: 1;
		background-color: rgb(37, 33, 33);
	}

	.description {
		width: 500px;
		max-width: 90vw;
	}
</style>
