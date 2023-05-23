<script lang="ts">
	import { onMount, afterUpdate } from 'svelte';
	import Computer from './assets/5150.png';

	let currentDirectory: string[] = ['A:'];
	let inputText: string = '';
	let terminalText: string[] = [''];
	let input: HTMLSpanElement;
	let terminal: HTMLDivElement;
	let bootStage: number = 0;
	let memory: number = 0;
	let directories: { [key: string]: any } = {
		'A:': {
			WINDOWS: {
				'system.ini': 'Your system is feeling a bit.ini today.',
				MyDinoPics: {
					't-rex.txt': 'An ASCII art of a T-rex.',
					'triceratops.txt': 'ASCII art of a Triceratops.'
				}
			},
			SYSTEM32: {
				drivers: {
					'audio.sys': 'Play that funky music, white DOS!'
				},
				'time-travel.bat':
					'You run the program and...\n...nothing happens.\nBut maybe it worked yesterday?'
			},
			CONFIG: {
				'dos.conf': "Trust me, I'm a .conf file!"
			},
			'hello.exe': 'Hello world!',
			SECRET: {
				'Atlantis.txt': 'I found Atlantis!',
				'diary.txt': 'Dear diary, I am going to find Atlantis.',
				'busyboyfriend.txt':
					'Dont make me sad, dont make me cry. Sometimes love is not enough and the road gets tough, I dont know why.Keep making me laugh Lets go get high. The road is long, we carry on. Try to have fun in the meantime.'
			}
		}
	};

	const getCurrentDirectoryObject = () => {
		let current = directories;
		for (let dir of currentDirectory) {
			current = current[dir];
		}
		return current;
	};

	const handleKeyDown = (e: KeyboardEvent) => {
		if (e.key === 'Enter') {
			let parts = inputText.trim().split(' ');
			let command = parts[0];
			let argument = parts.slice(1).join(' ');
			terminalText = [...terminalText, currentDirectory.join('\\') + '\\' + inputText];
			if (command === 'dir') {
				let currentDirObject = getCurrentDirectoryObject();
				let newLines = Object.keys(currentDirObject);
				terminalText = [...terminalText, ...newLines];
			} else if (command === 'cd') {
				let currentDirObject = getCurrentDirectoryObject();
				if (argument === '..') {
					if (currentDirectory.length > 1) {
						currentDirectory = [...currentDirectory.slice(0, -1)];
					}
				} else if (currentDirObject[argument] && typeof currentDirObject[argument] === 'object') {
					currentDirectory = [...currentDirectory, argument];
				} else {
					terminalText = [...terminalText, 'Invalid directory.'];
				}
			} else if (command === 'run') {
				let currentDirObject = getCurrentDirectoryObject();
				if (currentDirObject[argument]) {
					if (typeof currentDirObject[argument] === 'string') {
						terminalText = [...terminalText, currentDirObject[argument]];
					} else {
						terminalText = [...terminalText, 'Cannot execute directory.'];
					}
				} else {
					terminalText = [...terminalText, 'File not found.'];
				}
			} else if (command === 'help') {
				terminalText = [
					...terminalText,
					'dir - list files and directories in the current directory.'
				];
				terminalText = [...terminalText, 'cd - change directory.'];
				terminalText = [...terminalText, 'run - run a file.'];
			} else {
				terminalText = [...terminalText, 'Bad command or file name.'];
			}
			terminalText = [...terminalText, ' '];
			inputText = '';
			e.preventDefault();
		}
	};

	// function that counts the memory up to 65536
	const countUp = () => {
		if (memory < 81152) {
			memory += Math.floor(Math.random() * 5000);
			if (memory > 81152) memory = 81152;
			setTimeout(() => {
				countUp();
			}, 100);
		} else bootHandler();
	};

	const bootHandler = async () => {
		bootStage = 1;
		await new Promise((resolve) => setTimeout(resolve, 1000));
		bootStage = 2;
		await new Promise((resolve) => setTimeout(resolve, 800));
		bootStage = 3;
		await new Promise((resolve) => setTimeout(resolve, 800));
		bootStage = 4;
		await new Promise((resolve) => setTimeout(resolve, 600));
		bootStage = 5;
	};

	onMount(() => {
		countUp();
	});

	afterUpdate(() => {
		terminal.scrollTop = terminal.scrollHeight;
	});
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="container">
	<div class="content">
		<div class="computer">
			<img src={Computer} alt="" />

			<div
				class="terminal"
				bind:this={terminal}
				on:click={() => {
					if (input) input.focus();
				}}
			>
				{#if bootStage === 0 || bootStage === 1}
					<p>{memory} <span>KB OK</span></p>

					{#if bootStage === 1}
						<p><span class="caret">█</span></p>
					{/if}
				{:else if bootStage === 2}
					<p><span class="caret">█</span></p>
				{:else if bootStage === 3 || bootStage === 4 || bootStage === 5}
					<p>Starting MS-DOS...</p>
					<br />
					{#if bootStage === 3}
						<p><span class="caret">█</span></p>
					{/if}
					{#if bootStage === 4 || bootStage === 5}
						<p>The IBM Personal Computer DOS</p>
						<p>Version 3.30</p>
						<p>(C)Copyright International Business Machines Corp. 1981</p>
						<p>(C)Copyright Microsoft Corp 1901</p>
						{#if bootStage === 4}
							<p><span class="caret">█</span></p>
						{/if}
						<br />
					{/if}
					{#if bootStage === 5}
						{#each terminalText as line}
							<p>{line}</p>
							{#if line === ' '}
								<br />
							{/if}
						{/each}
						<div class="command-line">
							<span>{currentDirectory.join('\\')}\</span>
							<input
								bind:this={input}
								type="text"
								bind:value={inputText}
								on:keydown={handleKeyDown}
							/>
						</div>
					{/if}
				{/if}
			</div>
		</div>

		<div class="article">
			<h1 class="article-title">MSDOS & IBM PC 5150</h1>
			<h2 class="article-subtitle">1981</h2>
			<p class="article-text">
				MS-DOS,running on the IBM PC 5150, marked a turning point in personal computing. As a
				versatile, user-friendly operating system, MS-DOS facilitated widespread accessibility to
				computing technology, enabling a new era of software development and personal productivity.
			</p>
		</div>
	</div>
</div>

<style>
	@font-face {
		font-family: 'DOSVGA';
		src: url('./assets/DOSVGA.ttf');
	}

	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: var(--carousel-padding);
		width: 100%;
		min-height: 100svh;
		height: 100% !important;
		padding-top: 100px;
		box-sizing: border-box;
		background: linear-gradient(to bottom, #121311 50%, rgb(39, 89, 32) 100%);
	}

	.content {
		max-width: var(--max-content-width);
		display: grid;
		grid-template-columns: 2fr 1fr;
		width: 100%;
		height: 100%;
		align-items: center;
		justify-content: center;
	}

	@media (max-width: 1000px) {
		.content {
			grid-template-columns: 1fr;
			grid-template-rows: 1 1;
			/* align-items: center; */
		}

		.computer {
			align-self: start;
		}
		.description {
			align-self: start;
		}
	}

	.computer {
		/* position: relative;
		aspect-ratio: 1.3441;
		max-width: 100%;
		max-height: 100%; */
		position: relative;
		/* width: 100%; */
		/* height: 100%; */
		z-index: 1;
	}

	.computer img {
		width: 1000px;

		max-width: 100%;
		max-height: 100%;
		z-index: 1;
		pointer-events: none;
	}

	.article {
		max-width: 100%;
		pointer-events: none;
	}

	.article h1 {
		font-family: 'DOSVGA', monospace;
		/* word-spacing: -10px; */
		font-size: 30px;
		color: rgb(75, 236, 54);
	}

	.article h2 {
		color: var(--text-color-primary);
	}

	.terminal {
		position: absolute;
		width: 36.5%;
		height: 35%;
		left: 20%;
		top: 12%;
		box-sizing: border-box;
		padding: 3%;
		padding-bottom: 2%;
		background-color: black;
		background: radial-gradient(
			circle at bottom center,
			rgb(44, 42, 42) 0%,
			rgb(30, 31, 31) 50%,
			rgba(0, 0, 0, 1) 100%
		);
		overflow: hidden;
		box-shadow: inset 0 0 50px 0 rgba(0, 0, 0, 0.5);
		cursor: pointer;
		z-index: -1;
	}

	.command-line {
		display: flex;
		width: 100%;
		justify-content: center;
	}

	input {
		background-color: transparent;
		min-width: 20px;
		outline: none;
		border: none;
		/* width: 100%; */
		overflow: hidden;
		max-lines: 1;
		font-family: 'DOSVGA', monospace;
		font-size: 17px;
		width: 100%;
		cursor: pointer;
	}

	.terminal *::selection {
		background-color: rgb(75, 236, 54);
		color: black;
	}

	.caret {
		cursor: text;
		animation: blink 1s infinite;
	}

	@keyframes blink {
		0% {
			opacity: 1;
		}
		50% {
			opacity: 1;
		}
		51% {
			opacity: 0;
		}
		100% {
			opacity: 0;
		}
	}

	.terminal *:not(br) {
		font-family: 'DOSVGA', monospace;
		margin: 0;
		color: rgb(75, 236, 54);
		font-size: 17px;
	}

	@media (max-width: 500px) {
		.terminal *:not(br) {
			font-size: 7px;
		}

		br {
			display: none;
		}
	}
</style>
