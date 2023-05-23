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
	import Close from './assets/xp-close.png';
	import Minimize from './assets/xp-minimize.png';
	import Window from './assets/xp-window.png';

	const fileSize: number = 80;

	let article: string =
		'VVVVVVVVVVVVVVVVVVV\n>>>WINDOWS XP<<<\n^^^^^^^^^^^^^^\n2001\n\nWindows XP is considered one of Microsofts most successful operating systems. It combined the reliability and security of professional-grade operating systems with the usability and compatibility features that made consumer-grade systems popular. XP introduced features like a revamped GUI, fast user switching, and system restore, all of which contributed to its broad adoption. Its longevity also made it noteworthy; despite subsequent OS releases, XP remained in use in many sectors for well over a decade.';
	let stickyOffset: number = 0;
	let desktop: HTMLDivElement;
	let screen: HTMLDivElement;
	let startMenuOpen: boolean = false;
	let openedStartMenu: boolean = false;
	let oldPosition: { x: number; y: number } = { x: 0, y: 0 };
	let draggingFile: number = -1;
	let desktopRect: DOMRect;
	let offset: { x: number; y: number } = { x: 0, y: 0 };
	let desktopFiles: { x: number; y: number; name: string; img: string }[] = [
		{ x: 0, y: 0, name: 'Recycle Bin', img: `${Recycling}` },
		{ x: fileSize * 2, y: fileSize * 2, name: 'Internet', img: `${IE}` },
		{ x: fileSize * 3, y: fileSize * 3, name: 'Notepad', img: `${Notepad}` }
	];
	let draggingWindow: boolean = false;
	let openedNotepad: boolean = true;
	let minimizedNotepad: boolean = false;
	let lastNotePadData: { x: number; y: number; width: number; height: number } = {
		x: 0,
		y: 0,
		width: 0,
		height: 0
	};
	let maximizedNotepad: boolean = false;
	let notepad: { x: number; y: number; width: number; height: number } = {
		x: fileSize * 4,
		y: 0,
		width: 400,
		height: 400
	};

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

	const dragFileStart = (e: MouseEvent, file: number) => {
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
				name: desktopFiles[draggingFile].name,
				img: desktopFiles[draggingFile].img
			};
			desktopFiles = [
				...desktopFiles.slice(0, draggingFile),
				newFile,
				...desktopFiles.slice(draggingFile + 1)
			];
		} else if (draggingWindow) {
			notepad = {
				x: e.clientX - offset.x,
				y: e.clientY - offset.y,
				width: notepad.width,
				height: notepad.height
			};
		}
	};

	// const touchDrag = (e: TouchEvent, file: number) => {
	// 	e.preventDefault();
	// 	if (draggingFile === -1) {
	// 		desktopRect = desktop.getBoundingClientRect();
	// 		oldPosition = snapToGrid({
	// 			x: e.touches[0].clientX - desktopRect.left,
	// 			y: e.touches[0].clientY - desktopRect.top
	// 		});
	// 		draggingFile = file;
	// 		offset = {
	// 			x: e.touches[0].clientX - desktopRect.left - oldPosition.x,
	// 			y: e.touches[0].clientY - desktopRect.top - oldPosition.y
	// 		};

	// 		const newFile: { x: number; y: number; name: string; img: string } = {
	// 			x: e.touches[0].clientX - desktopRect.left - offset.x,
	// 			y: e.touches[0].clientY - desktopRect.top - offset.y,
	// 			name: desktopFiles[draggingFile].name,
	// 			img: desktopFiles[draggingFile].img
	// 		};
	// 		desktopFiles = [
	// 			...desktopFiles.slice(0, draggingFile),
	// 			newFile,
	// 			...desktopFiles.slice(draggingFile + 1)
	// 		];
	// 	}
	// };

	const dragEnd = () => {
		if (draggingFile !== -1) {
			const snapPosition = snapToGrid({
				x: desktopFiles[draggingFile].x + offset.x,
				y: desktopFiles[draggingFile].y + offset.y
			});

			if (
				desktopFiles[draggingFile].x + offset.x < 0 ||
				desktopFiles[draggingFile].x > desktopRect.width ||
				desktopFiles[draggingFile].y + offset.y < 0 ||
				desktopFiles[draggingFile].y > desktopRect.height ||
				desktopFiles.some(
					(file, index) =>
						index !== draggingFile && file.x === snapPosition.x && file.y === snapPosition.y
				)
			) {
				desktopFiles = [
					...desktopFiles.slice(0, draggingFile),
					{ ...desktopFiles[draggingFile], x: oldPosition.x, y: oldPosition.y },
					...desktopFiles.slice(draggingFile + 1)
				];
			} else {
				desktopFiles = [
					...desktopFiles.slice(0, draggingFile),
					{ ...desktopFiles[draggingFile], x: snapPosition.x, y: snapPosition.y },
					...desktopFiles.slice(draggingFile + 1)
				];
			}
		}

		if (!draggingWindow) {
			//stuff
		}

		draggingWindow = false;
		draggingFile = -1;
	};

	const handleStickyOffset = () => {
		const screenHeight = screen.getBoundingClientRect().height;
		const viewportHeight = window.innerHeight;
		stickyOffset = viewportHeight - screenHeight + window.scrollY;
		if (stickyOffset > 0) stickyOffset = 0;
	};

	const getDesktopSize = () => {
		desktopRect = desktop.getBoundingClientRect();
		desktopFiles = desktopFiles.map((file) => {
			if (file.x < 0) file.x = 0;
			if (file.x > desktopRect.width - fileSize) file.x = desktopRect.width - fileSize;
			if (file.y < 0) file.y = 0;
			if (file.y > desktopRect.height - fileSize) file.y = desktopRect.height - fileSize;
			return file;
		});

		if (notepad.width > desktopRect.width) notepad.width = desktopRect.width;
		if (notepad.height > desktopRect.height) notepad.height = desktopRect.height;

		if (notepad.x < 0) notepad.x = 0;
		if (notepad.x > desktopRect.width - notepad.width) {
			notepad.x = desktopRect.width - notepad.width;
		}
		if (notepad.y < 0) notepad.y = 0;
		if (notepad.y > desktopRect.height - notepad.height) {
			notepad.y = desktopRect.height - notepad.height;
		}
	};

	const dragWindowStart = (e: MouseEvent) => {
		if (e.target instanceof HTMLButtonElement) return;
		if (e.target instanceof HTMLImageElement) return;
		draggingWindow = true;
		offset = {
			x: e.clientX - notepad.x,
			y: e.clientY - notepad.y
		};
	};

	const maximize = () => {
		if (!maximizedNotepad) {
			lastNotePadData = { ...notepad };
			notepad = {
				x: 0,
				y: 0,
				width: desktopRect.width,
				height: desktopRect.height
			};
		} else {
			notepad = { ...lastNotePadData };
		}

		maximizedNotepad = !maximizedNotepad;
	};

	onMount(() => {
		getDesktopSize();
		handleStickyOffset;
		window.addEventListener('resize', () => {
			getDesktopSize();
			handleStickyOffset();
		});
		window.addEventListener('scroll', handleStickyOffset);
		window.addEventListener('mousemove', drag);
		window.addEventListener('mouseup', dragEnd);

		return () => {
			window.removeEventListener('resize', () => {
				getDesktopSize();
				handleStickyOffset();
			});
			window.removeEventListener('scroll', handleStickyOffset);
			window.removeEventListener('mousemove', drag);
			window.removeEventListener('mouseup', dragEnd);
		};
	});
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="screen" bind:this={screen} on:click={handleScreenClick}>
	<div class="desktop" bind:this={desktop}>
		{#if openedNotepad && !minimizedNotepad}
			<div
				class="window"
				style={`left: ${notepad.x}px; top: ${notepad.y}px;   width: ${notepad.width}px; height: ${notepad.height}px;`}
			>
				<div class="title-bar swiper-no-swiping" on:mousedown={(e) => dragWindowStart(e)}>
					<div class="title-bar-text">
						<img src={Notepad} alt="" draggable="false" />
						<span>Untitled - Notepad</span>
					</div>
					<div class="controls">
						<button
							on:click={() => {
								minimizedNotepad = !minimizedNotepad;
							}}
						>
							<img src={Minimize} alt="" draggable="false" />
						</button>
						<button
							on:click={() => {
								maximize();
							}}
						>
							<img src={Window} alt="" draggable="false" />
						</button>
						<button on:click={() => (openedNotepad = false)}>
							<img src={Close} alt="" draggable="false" />
						</button>
					</div>
				</div>
				<div class="window-body">
					<textarea bind:value={article} />

					<div class="article">
						<!-- <h1>Windows XP</h1>
						<h2>2001</h2> -->
						<!-- <p>WINDOWS X</p>
						<p>2001</p>
						<p>
							Windows XP is considered one of Microsoft's most successful operating systems. It
							combined the reliability and security of professional-grade operating systems with the
							usability and compatibility features that made consumer-grade systems popular. XP
							introduced features like a revamped GUI, fast user switching, and system restore, all
							of which contributed to its broad adoption. Its longevity also made it noteworthy;
							despite subsequent OS releases, XP remained in use in many sectors for well over a
							decade.
						</p> -->
					</div>
				</div>
			</div>
		{/if}

		{#if draggingFile !== -1}
			<div class="file" style={`left:${oldPosition.x}px; top:${oldPosition.y}px; `}>
				<img src={desktopFiles[draggingFile].img} alt="" draggable="false" />
				<p style="background-color: rgba(40,100,200,0.9);">
					{desktopFiles[draggingFile].name}
				</p>
			</div>
		{/if}

		{#each desktopFiles as file, index (file)}
			<button
				on:click={() => {
					if (file.name === 'Notepad') {
						openedNotepad = true;
						minimizedNotepad = false;
					}
				}}
				class="file swiper-no-swiping"
				style={`left:${desktopFiles[index].x}px; top:${desktopFiles[index].y}px;`}
				on:mousedown={(e) => dragFileStart(e, index)}
			>
				<img src={file.img} alt={file.name} draggable="false" />
				<p>
					{file.name}
				</p>
			</button>
		{/each}
	</div>
	<!-- <div class="sticky" style={`transform: translateY( ${stickyOffset}px);`}> -->
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
		</div>
	{/if}

	<div class="taskbar">
		<button
			class="start"
			on:click={handleStartButton}
			style={`${startMenuOpen ? 'filter: brightness(0.95)' : ''}`}
		>
			<img src={Start} alt="start" draggable="false" />
		</button>
		<div class="tabs">
			{#if openedNotepad}
				<button
					class={`notepad-tab ${minimizedNotepad ? 'minimized' : ''}`}
					on:click={() => {
						minimizedNotepad = !minimizedNotepad;
					}}
				>
					<img src={Notepad} alt="" draggable="false" />
					<span>Notepad</span>
				</button>
			{/if}
		</div>

		<div class="tray">
			<img src={Virus} alt="" draggable="false" />
			<img src={Volume} alt="" draggable="false" />
			<p>4:23 AM</p>
		</div>
	</div>
	<!-- </div> -->
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

	.screen {
		padding: var(--carousel-padding);
		box-sizing: border-box;
		display: flex;
		background: linear-gradient(to top, #b8b8b8 5%, white 30%, white 40%, #49869e);
		flex-direction: column;
		width: 100%;
		min-height: 100svh;
		height: 100%;
		align-items: center;
		justify-content: center;
		cursor: default;
		background-image: url('./assets/xp-background.jpg');
		background-size: cover;
		background-position: center;
		font-family: 'Tahoma', sans-serif;
		-webkit-user-select: none;
		-khtml-user-select: none;
		-moz-user-select: none;
		-o-user-select: none;
		user-select: none;
		z-index: -1;
	}

	.desktop {
		position: relative;
		width: 100%;
		height: 100%;
		z-index: -1;
	}

	.sticky {
		position: absolute;
		bottom: 0;
		width: 100%;
		height: 100%;
		pointer-events: none;
	}

	.sticky * {
		pointer-events: all;
	}

	.taskbar {
		position: absolute;
		bottom: 0;
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
		/* overflow: hidden; */
		z-index: 1;
	}

	.tabs {
		height: 100%;
		padding: 2px;
		box-sizing: border-box;
		margin-left: 10px;
	}
	.notepad-tab {
		height: 100%;
		width: 150px;
		max-width: 100%;
		border-radius: 5px;
		background: linear-gradient(
			hsl(223, 59%, 40%) 0%,
			hsl(214, 60%, 39%) 5%,
			hsl(207, 58%, 34%) 18%,
			hsl(221, 77%, 36%) 100%
		);
		box-shadow: inset -1px -1px 1px 1px #134cb6, inset 1px 0px 1px 1px #134cb665;
		border: none;
		display: flex;
		align-items: center;
		padding: 5px 5px;
		overflow: hidden;
		cursor: pointer;
	}

	.notepad-tab span {
		color: white;
		white-space: nowrap;
		font-family: 'Tahoma Pixel', sans-serif;
		font-size: 17px;
		margin-left: 5px;
	}

	.notepad-tab.minimized {
		background: linear-gradient(
			hsl(223, 71%, 57%) 0%,
			hsl(214, 60%, 53%) 5%,
			hsl(207, 54%, 48%) 18%,
			hsl(221, 73%, 50%) 100%
		);
	}

	.notepad-tab img {
		height: 100%;
		aspect-ratio: 1;
		object-fit: contain;
	}

	.window {
		position: absolute;
		max-width: 100%;
		max-height: 100%;
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 73%, 58%) 5%,
			hsl(223, 72%, 49%) 18%,
			hsl(221, 73%, 50%) 90%,
			hsl(222, 74%, 35%) 100%
		);
		border-radius: 10px 10px 0 0;
		overflow: hidden;
		z-index: 10;
		display: flex;
		flex-direction: column;
		align-items: center;
		z-index: -1;
	}

	.window-body {
		background: white;
		width: calc(100% - 6px);
		height: 100%;
		margin-bottom: 3px;
		box-sizing: border-box;
		color: black;
		line-height: 0.9;
		font-family: 'Tahoma Pixel', sans-serif;
		font-size: 22px;
		line-height: 0.9;
		padding: 5px;
		cursor: text;
		border: transparent;
		padding-right: 0;
	}

	.window-body * {
		color: black;
	}

	.window-body *::selection {
		background-color: #0075dc;
		color: white;
	}

	.article p {
		font-size: 20px;
	}

	.window-body:focus {
		outline: none;
	}

	.window-body textarea {
		width: 100%;
		height: 100%;
		border: none;
		resize: none;
		padding: 5px;
		box-sizing: border-box;
		font-family: 'Tahoma Pixel', sans-serif;
		font-size: 22px;
		background: transparent;
		color: black;
	}

	.window-body textarea:focus {
		outline: none;
	}

	.window-body textarea::selection {
		background-color: #0075dc;
		color: white;
	}

	.title-bar {
		width: 100%;
		height: 35px;
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 73%, 58%) 5%,
			hsl(223, 72%, 49%) 18%,
			hsl(221, 73%, 50%) 90%,
			hsl(222, 74%, 35%) 100%
		);
		font-family: 'Tahoma', sans-serif;
		box-sizing: border-box;
		display: flex;
		padding: 0 5px;
		align-items: center;
	}

	.title-bar-text {
		display: flex;
		align-items: center;
		gap: 5px;
		height: 100%;
		padding: 5px;
		box-sizing: border-box;
		font-size: 14px;
		pointer-events: none;
	}

	.title-bar-text img {
		height: 90%;
		aspect-ratio: 1;
		object-fit: contain;
		box-sizing: border-box;
	}

	.controls {
		margin-left: auto;
		display: flex;
		gap: 5px;
		height: 100%;
		padding: 5px 0;
		box-sizing: border-box;
	}

	.controls button {
		height: 100%;
		aspect-ratio: 1;
		border-radius: 3px;
		border: 1px solid white;
		background-color: white;
		padding: 0;
	}

	.controls button:hover {
		filter: brightness(1.1);
	}

	.controls button img {
		width: 100%;
		height: 100%;
		object-fit: contain;
	}

	.controls button {
		background-color: #216bf9;
		background-image: linear-gradient(160deg, hsl(217, 37%, 75%) 0%, transparent 40%);
		box-shadow: inset 0px 0px 5px 1px #134cb6;
		cursor: pointer;
	}

	.controls button:nth-child(3) {
		background-color: #e46138;
		background-image: linear-gradient(160deg, hsl(17, 86%, 89%) 0%, transparent 35%);
		box-shadow: inset 0px 0px 5px 1px #a73f1f;
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
		box-sizing: border-box;
	}

	.start:hover {
		cursor: pointer;
		filter: brightness(1.15);
	}

	.start img {
		width: 90px;
		height: 90%;
		object-fit: contain;
		/* background-color: red; */
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
		bottom: 35px;
		left: 0;
		width: 300px;
		height: min(350px, 85%);
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
		z-index: -1;
	}

	.menu .top {
		height: 70px;

		display: flex;
		align-items: center;
		padding: 0 5px;
		gap: 10px;
		font-size: 14px;
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 80%, 63%) 1%,
			hsl(223, 72%, 49%) 5%,
			hsl(214, 80%, 58%) 97.5%,
			hsl(223, 72%, 49%) 100%
		);
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
		z-index: -2;
		border: 0;
		background-color: transparent;
	}

	.file img {
		width: 45px;
		aspect-ratio: 1;
		filter: drop-shadow(0px 0px 3px hsla(0, 0%, 0%, 0.5));
	}

	.file p {
		color: white;
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
		overflow: hidden;
		flex-shrink: 1;
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
		background: linear-gradient(
			hsl(223, 72%, 49%) 0%,
			hsl(214, 80%, 58%) 5%,
			hsl(223, 72%, 49%) 99%,
			hsl(223, 76%, 29%) 100%
		);
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
		/* width: 500px; */
		/* max-width: 90vw; */
	}
</style>
