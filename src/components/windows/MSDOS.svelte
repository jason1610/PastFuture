<script lang="ts">
	import { onMount, afterUpdate } from 'svelte';
	import { bind } from 'svelte/internal';

	let inputText: string = 'C:\\';
	let terminalText = 'Welcome to Svelte Terminal';
	let input: HTMLSpanElement;
	let terminal: HTMLDivElement;

	const handleInput = (e: KeyboardEvent) => {
		if (e.key === 'Enter') {
			terminalText += '> ' + inputText + '\n';
			inputText = '';
			e.preventDefault();
		}
	};

	const handleKeyDown = (e: KeyboardEvent) => {
		if (e.key === 'Backspace' && inputText.length <= 3) {
			e.preventDefault();
		}

		if (e.key === 'Enter') {
			terminalText += '> ' + inputText + '\n';
			inputText = '';
			e.preventDefault();
		}
	};

	const checkDeleted = () => {
		if (inputText.slice(0, 3) !== 'C:\\') {
			inputText = 'C:\\';
		}
	};

	onMount(() => {
		input.focus();
	});

	afterUpdate(() => {
		terminal.scrollTop = terminal.scrollHeight;
	});
</script>

<div class="container">
	<div
		class="terminal"
		bind:this={terminal}
		on:click={() => {
			input.focus;
		}}
		on:keypress={() => {
			input.focus();
		}}
	>
		<input
			bind:this={input}
			type="text"
			bind:value={inputText}
			on:keydown={handleKeyDown}
			on:input={checkDeleted}
		/>
	</div>
	<div class="description">
		<h1>MSDOS</h1>

		<p>
			MSDOS is a command line interface. It was the first operating system to be used on IBM PCs. It
			was released in 1981.
		</p>
	</div>
</div>

<style>
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 50px 0;
	}
	.terminal {
		background-color: black;
		color: white;
		padding: 20px;
		overflow: auto;
		border-radius: 25px;
		height: 400px;
		aspect-ratio: 1;
		max-width: calc(100vw - 40px);
		overflow: hidden;
	}

	input {
		background-color: rgb(0, 0, 0);
		color: white;
		width: 20px;
		outline: none;
		border: none;
		width: 100%;
		overflow: hidden;
	}

	.caret {
		cursor: text;
	}
</style>
