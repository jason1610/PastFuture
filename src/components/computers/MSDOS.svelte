<script lang="ts">
	import { onMount, afterUpdate } from 'svelte';
	import Logo from './assets/MSDOS-boot-logo.png';
	import Computer from './assets/5150.png';

	let directory: string = 'C:\\';
	let inputText: string = '';
	let terminalText: string[] = [''];
	let input: HTMLSpanElement;
	let terminal: HTMLDivElement;
	let bootStage: number = 0;

	const handleKeyDown = (e: KeyboardEvent) => {
		if (e.key === 'Enter') {
			if (inputText === 'dir') {
				const newLine: string = directory + inputText;
				terminalText = [...terminalText, newLine];
				terminalText = [...terminalText, 'WINDOWS'];
				terminalText = [...terminalText, 'SYSTEM32'];
				terminalText = [...terminalText, 'DRIVERS'];
				terminalText = [...terminalText, 'CONFIG'];
				terminalText = [...terminalText, 'AUTOEXEC.BAT'];
				terminalText = [...terminalText, 'COMMAND.COM'];
				terminalText = [...terminalText, 'CONFIG.SYS'];
				terminalText = [...terminalText, 'IO.SYS'];
				terminalText = [...terminalText, 'MSDOS.SYS'];
				terminalText = [...terminalText, 'WIN.COM'];
				terminalText = [...terminalText, 'WIN.INI'];
				terminalText = [...terminalText, 'WINVER.EXE'];
				terminalText = [...terminalText, 'WINVER.INI'];
				terminalText = [...terminalText, 'WINVER.LOG'];
				terminalText = [...terminalText, ' '];
				inputText = '';
			}
			// else if (inputText.startsWith('cd')) {
			// 	const newLine: string = '> ' + inputText;
			// 	terminalText = [...terminalText, newLine];
			// 	const newDirectory: string = inputText.split(' ')[1];
			// 	if (newDirectory === '..') {
			// 		directory = directory.split('\\').slice(0, -1).join('\\') + '\\';
			// 	} else {
			// 		directory = directory + newDirectory + '\\';
			// 	}
			// 	inputText = '';
			// }
			else if (inputText.trim().length === 0) {
				const newLine: string = directory + inputText;
				terminalText = [...terminalText, newLine];
				terminalText = [...terminalText, ' '];
				inputText = '';
			} else {
				const newLine: string = directory + inputText;
				terminalText = [...terminalText, newLine];
				terminalText = [...terminalText, 'Bad command or file name.'];
				terminalText = [...terminalText, ' '];
				inputText = '';
			}
			e.preventDefault();
		}
	};

	const bootHandler = async () => {
		await new Promise((resolve) => setTimeout(resolve, 2000));
		bootStage = 1;
		await new Promise((resolve) => setTimeout(resolve, 400));
		bootStage = 3;
		await new Promise((resolve) => setTimeout(resolve, 1500));
		bootStage = 4;
		await new Promise((resolve) => setTimeout(resolve, 300));
		bootStage = 5;
		await new Promise((resolve) => setTimeout(resolve, 600));
		bootStage = 6;
		await new Promise((resolve) => setTimeout(resolve, 1000));
		bootStage = 7;
		await new Promise((resolve) => setTimeout(resolve, 400));
		bootStage = 8;
		await new Promise((resolve) => setTimeout(resolve, 500));
		bootStage = 9;
		await new Promise((resolve) => setTimeout(resolve, 1000));
		bootStage = 10;
	};

	const scrollToEnd = () => {
		terminal.scrollTop = terminal.scrollHeight;
	};

	onMount(() => {
		bootHandler();
		window.addEventListener('resize', scrollToEnd);
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
				{#if bootStage === 0}
					<p>Phoenix S3 TRI064V+ EDO Turbo booseted. Version 1.02-02</p>
					<p>Copyright 1987-1982 Phoenix Technologies Ltd.</p>
					<p>Copyright 1992-1995 S3 Incorporated</p>
					<p>All Rights Reserved</p>
					<p><span class="caret">█</span></p>
				{:else if bootStage === 3 || bootStage === 4}
					<div class="boot">
						<img src={Logo} alt="" />
						<p>Award Modular BIOS v4.51.PG, An energy Star Ally</p>
						<p>Copyright 1984-98, Award Software, Inc.</p>
						<br />
						<p>GX_M Ver 1.0 09/25/1998</p>
						<p>Processor: Pentium(R) II 300 MHz</p>
						<p>Memory Testing: 65536K OK</p>
						<br />
						<p>Award Plug and Play BIOS Extension v1.0A</p>
						<p>Copyright (C) 1998, Award Software, Inc.</p>
						{#if bootStage === 4}
							<p>CDRPM : LITE-On LTB486S 48x Max</p>
						{/if}
					</div>
				{:else if bootStage >= 6}
					<table>
						<div class="hardware">
							<tr>
								<td>Processor</td>
								<td>: Pentium(R) II 300 MHz</td>
							</tr>
							<tr>
								<td>Co-Processor</td>
								<td>: Installed</td>
							</tr><tr>
								<td>CPU Clock</td>
								<td>: 250MHz</td>
							</tr>
							<tr>
								<td>Memory</td>
								<td>: 65536K</td>
							</tr>
							<tr>
								<td> Extended Memory</td>
								<td>: 64512K</td>
							</tr>
						</div>
						<div class="disk">
							<tr>
								<td>Diskette Drive A</td>
								<td>: 1.44M, 3.5 in.</td>
							</tr>
							<tr>
								<td>Diskette Drive B</td>
								<td>: None</td>
							</tr>
							<tr>
								<td>Pri. Master Disk</td>
								<td>: LBRA .MODE 4,32021MB</td>
							</tr>
							<tr>
								<td>Pri. Slave Disk</td>
								<td>: None</td>
							</tr>
							<tr>
								<td>Sec. Master Disk</td>
								<td>: CDROM,Mode 4</td>
							</tr>
							<tr>
								<td>Sec. Slave Disk</td>
								<td>: None</td>
							</tr>
							<tr>
								<td>Display Type</td>
								<td>: EGA/VGA</td>
							</tr>
							<tr>
								<td>Parallel Port</td>
								<td>: 378</td>
							</tr>
							<tr>
								<td>Serial Port</td>
								<td>: 3F8</td>
							</tr>
						</div>
					</table>
					<br />

					{#if bootStage >= 7}
						<p>PCI device listing ...</p>
						<table class="PCI">
							<tr class="border-bottom">
								<td>Bus No.</td>
								<td>Device No.</td>
								<td>Func No.</td>
								<td>Vendor ID</td>
								<td>Device ID</td>
								<td>Device Class</td>
								<td>IRQ</td>
							</tr>
							<tr>
								<td>0</td>
								<td>0</td>
								<td>0</td>
								<td>8086</td>
								<td>7190</td>
								<td>PCI to ISA Bridge</td>
								<td>9</td>
							</tr>
							<tr>
								<td>0</td>
								<td>1</td>
								<td>0</td>
								<td>8086</td>
								<td>7191</td>
								<td>IDE Controller</td>
								<td>14</td>
							</tr>
							<tr>
								<td>0</td>
								<td>2</td>
								<td>0</td>
								<td>8086</td>
								<td>7192</td>
								<td>USB Controller</td>
								<td>11</td>
							</tr>
						</table>
						{#if bootStage >= 8}
							<p>ISA/PNP device listing...</p>
							<table class="ISA">
								<tr class="border-bottom">
									<td>Card No</td>
									<td>Device No</td>
									<td>DMA</td>
									<td>IRQ</td>
									<td>Device Name</td>
								</tr>
								<tr>
									<td>1</td>
									<td>0</td>
									<td>1</td>
									<td>5</td>
									<td>Standard Floppy Disk Controller</td>
								</tr>
							</table>
							<p>Verifying DMI Pool Data.......</p>
							<p>Starting MS-DOS.......</p>
						{/if}
						{#if bootStage >= 9}
							<br />
							{#if bootStage === 9}
								<p>Testing extended memory.......<span class="caret">█</span></p>
							{:else}
								<p>Testing extended memory.......</p>
							{/if}
						{/if}
						{#if bootStage === 10}
							{#each terminalText as line}
								<p>{line}</p>
								{#if line === ' '}
									<br />
								{/if}
							{/each}

							<div class="command-line">
								<span>{directory}</span>
								<input
									bind:this={input}
									type="text"
									bind:value={inputText}
									on:keydown={handleKeyDown}
								/>
							</div>
						{/if}
					{/if}
				{/if}
			</div>
		</div>

		<div class="description">
			<h1 class="article-title">MSDOS & IBM PC 5150</h1>
			<h2 class="article-subtitle">1981</h2>
			<p class="article-text">
				MS-DOS (Microsoft Disk Operating System) was the dominant operating system for IBM
				PC-compatible computers in the 1980s and early 1990s. It played a crucial role in
				popularizing personal computers and laid the foundation for the success of Microsoft
				Windows.
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
		height: 100lvh;
		box-sizing: border-box;
		background: linear-gradient(to bottom, #b8b8b8, rgb(63, 135, 53) 100%);
	}

	.content {
		width: var(--carousel-width);
		max-width: 100%;
		height: 100%;
		box-sizing: border-box;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		/* padding-top: 50px; */
	}

	.computer {
		position: relative;
		aspect-ratio: 1.3441;
		max-width: 100%;
		max-height: 100%;
	}

	.computer img {
		width: 1000px;
		max-width: 100%;
		max-height: 100%;
		/* background-color: green; */
		z-index: 1;
		pointer-events: none;
	}

	.description {
		max-width: 100%;
		width: 400px;
		pointer-events: none;
	}

	.description h1 {
		color: rgb(75, 236, 54);
	}

	.description h2 {
		color: var(--text-color-primary);
	}

	.description h1 {
		font-family: 'DOSVGA', monospace;
	}

	@media (max-width: 500px) {
		h1 {
			font-size: 20px;
		}

		h2 {
			font-size: 15px;
		}
	}

	@media (max-width: 1000px) {
		.content {
			flex-direction: column;
		}
		.description {
			width: 100%;
		}
	}

	.terminal {
		position: absolute;
		width: 36.5%;
		height: 35%;
		left: 20%;
		top: 12%;
		padding: 3%;
		background-color: black;
		background: radial-gradient(
			circle at bottom center,
			rgb(58, 56, 56) 0%,
			rgb(30, 31, 31) 50%,
			rgba(0, 0, 0, 1) 100%
		);
		box-sizing: border-box;
		overflow: hidden;
		box-shadow: inset 0 0 50px 0 rgba(0, 0, 0, 0.5);
		cursor: pointer;
	}

	.boot {
		height: 100%;
	}

	.boot img {
		width: 200px;
		max-width: 40%;
		float: inline-end;
	}

	table {
		width: 100%;
		border: 1px solid white;
		border-collapse: separate;
		overflow: hidden;
	}

	.PCI {
		border: none;
	}

	.ISA {
		border: none;
	}

	.border-bottom {
		box-shadow: 0 1px 0 0 white;
	}

	@media (max-width: 500px) {
		table {
			display: inline-block;
		}
	}

	.hardware {
		width: 100%;
		padding-bottom: 10px;
	}

	.disk {
		padding-top: 10px;
		border-top: 1px solid white;
	}

	.command-line {
		display: flex;
		width: 100%;
		justify-content: flex-start;
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
