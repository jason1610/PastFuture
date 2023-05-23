<script lang="ts">
</script>

<div class="container">
	<div class="screen">
		<p>Hello World...<span class="caret">█</span></p>
	</div>
	<div class="description">
		<a href="/computers">> Computers & Operating Systems</a>
		<p>
			From white text on a black screen to the modern day GUI, operating systems have come a long
			way. Here are some of the most popular operating systems from the past and present.
		</p>
	</div>
</div>

<style>
	@font-face {
		font-family: 'DOSVGA';
		src: url('../computers/assets/DOSVGA.ttf');
	}

	.container {
		background-color: #e4e4e4;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		padding: var(--card-padding);
		border-radius: var(--card-border-radius);
		justify-content: center;
		align-items: center;
		gap: 20px;
		width: 400px;
		max-width: 100vw;
		max-height: 100lvh;
		overflow: hidden;
		box-shadow: var(--card-shadow);
	}

	.description a {
		font-size: var(--title-small-size);
		font-weight: var(--title-small-weight);
		color: var(--text-color-accent);
		text-decoration: none;
	}

	.description p {
		font-weight: var(--text-weight);
		font-size: var(--text-size);
		color: var(--text-color-secondary);
	}

	.screen {
		font-family: 'DOSVGA', monospace;
		color: rgb(64, 235, 64);
		background-color: black;
		width: 100%;
		height: 250px;
		padding: 20px;
		box-sizing: border-box;
		border-radius: 10px;
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
</style>
