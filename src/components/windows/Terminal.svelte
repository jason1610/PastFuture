<script lang="ts">
	import { onMount, afterUpdate } from 'svelte';
	import { bind } from 'svelte/internal';

	let inputText: string = 'dwdw';
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
		<span>C:\></span>
		<span class="input" bind:this={input} contenteditable="true">{inputText}</span>
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
		max-height: 300px;
		overflow: auto;
		border-radius: 25px;
		width: 500px;
		height: 500px;
		overflow: hidden;
	}

	.input {
		background-color: black;
		color: white;
		width: 20px;
		outline: none;
		border: none;
		width: 100%;
	}

	.caret {
		cursor: text;
	}
</style>
