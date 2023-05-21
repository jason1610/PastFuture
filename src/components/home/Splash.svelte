<script lang="ts">
	import Background from './assets/spiral-background.png';
	import ComputersCard from './ComputersCard.svelte';
</script>

<div class="container">
	<div class="title">
		<h1>Past Future</h1>
		<p>Things change with time. This is a collection of things that have changed over time.</p>
	</div>
	<div class="cards">
		<ComputersCard />
	</div>
	<img id="background" src={Background} alt="" />
</div>

<style>
	.container {
		width: 100%;
		overflow: hidden;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		padding: 100px 10%;
		box-sizing: border-box;
		/* margin-top: 100px; */
		gap: 50px;
	}

	.cards {
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	#background {
		width: calc(100% + 300px);
		height: calc(100% + 300px);
		position: absolute;
		top: -150px;
		left: -150px;
		object-fit: cover;
		filter: blur(100px);
		-webkit-filter: blur(100px);
		backface-visibility: hidden;
		-webkit-backface-visibility: hidden;
		-moz-backface-visibility: hidden;
		transform: translate3d(0, 0, 0);
		-webkit-transform: translate3d(0, 0, 0);
		-moz-transform: translate3d(0, 0, 0);
		z-index: -1;
	}

	.title {
	}

	h1 {
		font-size: 90px;
		font-weight: 700;
		text-align: center;
		justify-self: flex-end;
		margin: 0;
	}

	p {
		text-align: center;
		font-size: 25px;
		font-weight: 300;
		width: 700px;
		max-width: 90vw;
	}
</style>
