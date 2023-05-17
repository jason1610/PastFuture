<script lang="ts">
	import Start from './assets/xp-start.png';
	import { onMount } from 'svelte';
	import IE from './assets/xp-ie.png';
	import Recycling from './assets/xp-recycling.webp';
	import Profile from './assets/xp-profile.jpg';
	import MSN from './assets/xp-msn.png';
	import Notepad from './assets/xp-notepad.webp';
	import Minesweeper from './assets/xp-minesweeper.png';
	import Volume from './assets/xp-volume.png';
	import Virus from './assets/xp-virus.png';
	import Programs from './assets/xp-programs.png';
	import Documents from './assets/xp-documents.png';
	import Pictures from './assets/xp-pictures.png';
	import Music from './assets/xp-music.png';
	import Favorites from './assets/xp-favorites.png';
	import Help from './assets/xp-help.png';
	import Off from './assets/xp-off.png';
	import Key from './assets/xp-key.png';

	const fileSize: number = 80;

	let desktop: HTMLDivElement;
	let screen: HTMLDivElement;
	let startMenuOpen: boolean = false;
	let draggingFile: number = -1;
	let desktopRect: DOMRect;
	let offset: { x: number; y: number } = { x: 0, y: 0 };
	let openedStartMenu: boolean = false;
	let oldPosition: { x: number; y: number } = { x: 0, y: 0 };
	let dektopFiles: { x: number; y: number; name: string; img: string }[] = [
		// { x: 0, y: 0, name: 'My Computer', img: '' },
		{ x: 50, y: 0, name: 'Recycle Bin', img: `${Recycling}` },
		{ x: 400, y: 0, name: 'Internet', img: `${IE}` }
	];

	const handleStartButton = () => {
		startMenuOpen = !startMenuOpen;
		openedStartMenu = true;
	};

	const handleScreenClick = (e: MouseEvent) => {
		if (startMenuOpen && !openedStartMenu && e.target == desktop) {
			startMenuOpen = false;
		}
		openedStartMenu = false;
	};

	const snapToGrid = (position: { x: number; y: number }) => {
		const cellSize = 80;
		const snappedX = Math.floor(position.x / cellSize) * cellSize;
		const snappedY = Math.floor(position.y / cellSize) * cellSize;
		return {
			x: snappedX,
			y: snappedY
		};
	};

	const dragStart = (e: MouseEvent, file: number) => {
		// if (e.target instanceof HTMLParagraphElement) return;
		desktopRect = desktop.getBoundingClientRect();
		oldPosition = snapToGrid({ x: e.clientX - desktopRect.left, y: e.clientY - desktopRect.top });
		draggingFile = file;
		offset = {
			x: e.clientX - desktopRect.left - oldPosition.x,
			y: e.clientY - desktopRect.top - oldPosition.y
		};
	};

	const drag = (e: MouseEvent) => {
		if (draggingFile !== -1) {
			const newFile: { x: number; y: number; name: string; img: string } = {
				x: e.clientX - desktopRect.left - offset.x,
				y: e.clientY - desktopRect.top - offset.y,
				name: dektopFiles[draggingFile].name,
				img: dektopFiles[draggingFile].img
			};
			//
			dektopFiles = [
				...dektopFiles.slice(0, draggingFile),
				newFile,
				...dektopFiles.slice(draggingFile + 1)
			];
		}
	};

	const dragEnd = (e: MouseEvent) => {
		if (draggingFile === -1) return;

		const snapPosition = snapToGrid({
			x: dektopFiles[draggingFile].x + offset.x,
			y: dektopFiles[draggingFile].y + offset.y
		});

		if (
			dektopFiles[draggingFile].x + offset.x < 0 ||
			dektopFiles[draggingFile].x - 80 > desktopRect.width ||
			dektopFiles[draggingFile].y + offset.y < 0 ||
			dektopFiles[draggingFile].y > desktopRect.height ||
			dektopFiles.some(
				(file, index) =>
					index !== draggingFile && file.x === snapPosition.x && file.y === snapPosition.y
			)
		) {
			dektopFiles = [
				...dektopFiles.slice(0, draggingFile),
				{ ...dektopFiles[draggingFile], x: oldPosition.x, y: oldPosition.y },
				...dektopFiles.slice(draggingFile + 1)
			];
		} else {
			dektopFiles = [
				...dektopFiles.slice(0, draggingFile),
				{ ...dektopFiles[draggingFile], x: snapPosition.x, y: snapPosition.y },
				...dektopFiles.slice(draggingFile + 1)
			];
		}

		draggingFile = -1;
	};

	onMount(() => {
		window.addEventListener('mousemove', drag);
		window.addEventListener('mouseup', dragEnd);
		return () => {
			window.removeEventListener('mousemove', drag);
			window.removeEventListener('mouseup', dragEnd);
		};
	});
</script>

<div class="container">
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div class="screen" bind:this={screen} on:click={handleScreenClick}>
		<div class="desktop" bind:this={desktop}>
			{#each dektopFiles as file, index (file)}
				<div
					class="file"
					style={`left:${dektopFiles[index].x}px; top:${dektopFiles[index].y}px;`}
					on:mousedown={(e) => dragStart(e, index)}
				>
					<img src={file.img} alt={file.name} draggable="false" />
					<p>
						{file.name}
					</p>
				</div>
			{/each}
			{#if startMenuOpen}
				<div class="menu">
					<div class="top">
						<div class="profile">
							<img src={Profile} alt="" draggable="false" />
						</div>
						<h2>Admin</h2>
					</div>
					<div class="orange" />
					<div class="list-container">
						<div class="list-left">
							<div class="item">
								<img src={IE} alt="" draggable="false" />
								<div class="text">
									<p>Internet</p>
									<p>Internet Explorer</p>
								</div>
							</div>
							<div class="seperator" />
							<div class="item">
								<img src={Notepad} alt="" draggable="false" />
								<p>Notepad</p>
							</div>
							<div class="item">
								<img src={Minesweeper} alt="" draggable="false" />
								<p>Minesweeper</p>
							</div>
							<div class="item">
								<img src={MSN} alt="" draggable="false" />
								<p>MSN</p>
							</div>
							<div class="seperator" style="margin-top: auto; " />
							<div class="all-programs">
								<p>All Programs</p>
								<img src={Programs} alt="" />
							</div>
						</div>
						<div class="list-right">
							<div class="small-item">
								<img src={Documents} alt="" />
								<p>My Documents</p>
							</div>
							<div class="small-item">
								<img src={Pictures} alt="" />
								<p>My Pictures</p>
							</div>
							<div class="small-item">
								<img src={Music} alt="" />
								<p>My Music</p>
							</div>
							<div class="small-item">
								<img src={Favorites} alt="" />
								<p>My Favorites</p>
							</div>
							<div class="seperator" />
							<div class="small-item">
								<img src={Help} alt="" />
								<p>Help</p>
							</div>
						</div>
					</div>
					<div class="bottom">
						<button>
							<img src={Key} alt="" />
						</button>
						<p>Log Off</p>
						<button style="margin-left: 5px;">
							<img src={Off} alt="" />
						</button>
						<p>Turn Off Computer</p>
					</div>
					<!-- <div id="start-menu-close" on:click={closeStartMenu}>X</div>
					<div id="start-menu-content">Start Menu Content</div> -->
				</div>
			{/if}
		</div>
		<div class="taskbar">
			<button
				class="start"
				on:click={handleStartButton}
				style={`${startMenuOpen ? 'filter: brightness(0.95)' : ''}`}
			>
				<img src={Start} alt="start" draggable="false" />
			</button>
			<div class="tray">
				<img src={Virus} alt="" draggable="false" />
				<img src={Volume} alt="" draggable="false" />
				<p>4:23 AM</p>
			</div>
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
	@font-face {
		font-family: 'Tahoma';
		src: url('./assets/Tahoma.ttf');
	}
	@font-face {
		font-family: 'Tahoma Pixel';
		src: url('./assets/Tahoma-pixel.ttf');
	}

	@font-face {
		font-family: 'Tahoma Bold';
		src: url('./assets/Tahoma-bold.ttf');
	}

	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 50px 0;
		padding: 20px;
	}

	.screen {
		height: 500px;
		aspect-ratio: 4/3;
		max-height: 500px;
		max-width: calc(100vw - 40px);
		background-color: rgb(37, 33, 33);
		position: relative;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		background-image: url('./assets/xp-background.jpg');
		background-size: cover;
		background-position: center;
		overflow: hidden;
		box-sizing: border-box;
		font-family: Tahoma, sans-serif;
		-webkit-user-select: none;
		-khtml-user-select: none;
		-moz-user-select: none;
		-o-user-select: none;
		user-select: none;
	}

	.desktop {
		width: 100%;
		height: 100%;
		position: relative;
		overflow: hidden;
	}

	.taskbar {
		width: 100%;
		height: 35px;
		background-color: hsl(221, 73%, 50%);
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 73%, 58%) 5%,
			hsl(223, 72%, 49%) 18%,
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
		width: 90px;
		background-color: #4da052;
		background: linear-gradient(
			hsl(117, 48%, 39%) 0%,
			hsl(113, 33%, 51%) 5%,
			hsl(119, 53%, 37%) 25%,
			hsl(118, 39%, 51%) 100%
		);
		border: none;
		border-radius: 0 10px 10px 0 / 0 19px 25px 0;
		box-shadow: inset -3px -1px 5px 2px #0e4e26, 0.5px 0 1px 0px #073d3b9a;
		font-weight: 700;
		font-size: 20px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.start:hover {
		cursor: pointer;
		filter: brightness(1.15);
	}

	.start img {
		width: 100%;
		height: 90%;
		object-fit: contain;
	}

	.tray {
		margin-left: auto;
		background-color: #127bdd;
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 80%, 63%) 5%,
			#107ad9 18%,
			#0e81e5 90%,
			hsl(221, 73%, 50%) 100%
		);
		font-family: 'Tahoma Pixel', sans-serif;
		font-size: 18px;
		height: 100%;
		/* width: 150px; */
		display: flex;
		align-items: center;
		box-sizing: border-box;
		justify-content: flex-end;
	}

	.tray p {
		margin: 0 10px;
	}

	.tray img {
		height: 45%;
		aspect-ratio: 1;
		margin-left: 5px;
		filter: drop-shadow(0px 0px 1px hsla(0, 0%, 0%, 0.8));
	}

	.menu {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 300px;
		height: 350px;
		overflow: hidden;
		border-radius: 5px 5px 0 0;
		box-shadow: 2px 5px 5px 2px hsla(0, 0%, 0%, 0.5);
		z-index: 0;
		display: flex;
		flex-direction: column;
		background-color: hsl(221, 73%, 50%);
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 80%, 63%) 1%,
			hsl(223, 72%, 49%) 1.5%,
			hsl(214, 80%, 58%) 15.5%,
			hsl(223, 72%, 49%) 30%,
			hsl(223, 72%, 49%) 85%,
			hsl(214, 80%, 58%) 87%,
			hsl(223, 72%, 49%) 99%,
			hsl(223, 76%, 29%) 100%
		);
	}

	.menu .top {
		height: 70px;
		display: flex;
		align-items: center;
		padding: 0 5px;
		gap: 10px;
		font-size: 14px;
	}

	.menu .profile {
		width: 45px;
		aspect-ratio: 1;
		object-fit: contain;
		background-color: white;
		border-radius: 3px;
		padding: 1px;
		box-shadow: 1px 0px 3px 1px #171c4875;
	}

	.menu .profile img {
		width: 100%;
		height: 100%;
		object-fit: contain;
		background-color: blue;
		display: block;
	}

	.profile {
		width: 50px;
		aspect-ratio: 1;
		object-fit: contain;
		background-color: white;
		border-radius: 3px;
		padding: 1px;
		box-shadow: 1px 0px 3px 1px #171c4875;
	}

	.orange {
		width: 99.5%;
		height: 3px;
		background-color: #e88d47;
		background: linear-gradient(to right, white, #e88d47, #a4bfdd);
	}

	.seperator {
		width: 100%;
		height: 1px;
		/* margin: 5px 0; */
		background-color: #ffffff;
		background: linear-gradient(to right, transparent, rgba(128, 128, 128, 0.505), transparent);
	}

	.file {
		position: absolute;
		display: flex;
		height: 80px;
		width: 80px;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.file img {
		width: 45px;
		aspect-ratio: 1;
		filter: drop-shadow(0px 0px 3px hsla(0, 0%, 0%, 0.5));
	}

	.file p {
		text-shadow: 1px 1px 3px hsla(0, 0%, 0%, 1);
		width: 100%;
		margin: 0;
		margin-top: 5px;
		font-size: 19px;
		white-space: nowrap;
		overflow: hidden;
		text-align: center;
		text-overflow: ellipsis;
		font-family: 'Tahoma Pixel', sans-serif;
		cursor: default;
	}

	.file p::selection {
		background-color: blue;
	}

	.list-container {
		height: 100%;
		width: 99.5%;
		display: flex;
		flex-direction: row;
	}

	.list-left {
		width: 100%;
		height: 100%;
		background-color: #ffffff;
		overflow: hidden;
		display: flex;
		flex-direction: column;
	}

	.list-right {
		width: 100%;
		height: 100%;
		background-color: #cce1fa;
		box-shadow: -1px 0px 0px 0px #a4bfdd;
		box-sizing: border-box;
	}

	.item {
		font-family: 'Tahoma Pixel', sans-serif;
		font-size: 17px;
		color: black;
		margin-top: 5px;
		margin-bottom: 5px;
		margin-left: 5px;
		width: calc(100% - 10px);
		display: flex;
		height: 35px;
		align-items: center;
	}

	.item:hover {
		background-color: #2c5fbc;
		color: white;
	}

	.item p {
		margin: 0;
		margin-left: 5px;
	}

	.item img {
		height: 100%;
		aspect-ratio: 1;
		object-fit: contain;
	}

	.item .text p {
		line-height: 14px;
	}

	.item .text p:first-child {
		font-family: 'Tahoma Bold', sans-serif;
		font-size: 10px;
	}

	.item .text p:last-child {
		font-size: 15px;
		color: rgb(156, 156, 156);
	}

	.small-item {
		font-family: 'Tahoma Bold', sans-serif;
		font-size: 10.5px;
		color: #10224f;
		margin-top: 5px;
		margin-bottom: 5px;
		margin-left: 5px;
		width: calc(100% - 10px);
		display: flex;
		height: 30px;
		gap: 2px;
		align-items: center;
	}

	.small-item img {
		height: 75%;
		aspect-ratio: 1;
		object-fit: contain;
	}

	.small-item:hover {
		background-color: #2c5fbc;
		color: white;
	}

	.all-programs {
		font-family: 'Tahoma Bold', sans-serif;
		font-size: 10.5px;
		color: black;
		margin-top: 5px;
		margin-bottom: 5px;
		margin-left: 5px;
		width: calc(100% - 10px);
		display: flex;
		height: 35px;
		align-items: center;
		justify-content: center;
	}

	.all-programs img {
		height: 90%;
		aspect-ratio: 1;
		object-fit: contain;
	}

	.all-programs:hover {
		background-color: #2c5fbc;
		color: white;
	}

	.bottom {
		height: 40px;
		width: 100%;
		padding: 0 5px;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: flex-end;
		font-family: 'Tahoma Pixel', sans-serif;
		gap: 5px;
	}

	.bottom button {
		height: 70%;
		aspect-ratio: 1;
		border-radius: 5px;
		border: 1px solid white;
		padding: 0;
		overflow: hidden;
	}

	.bottom button:first-child {
		background-color: #dca718;
		background-image: linear-gradient(140deg, hsl(42, 43%, 91%) 0%, transparent 35%);
		box-shadow: inset 0px 0px 2px 2px #ae7a02;
	}
	.bottom button:nth-child(3) {
		background-color: #d2624b;
		background-image: linear-gradient(160deg, hsl(15, 43%, 91%) 0%, transparent 35%);
		box-shadow: inset 0px 0px 1px 1px #832916;
	}

	.bottom button:hover {
		filter: brightness(1.1);
	}

	.bottom img {
		height: 100%;
		aspect-ratio: 1;
		object-fit: contain;
	}

	.description {
		width: 500px;
		max-width: 90vw;
	}
</style>
