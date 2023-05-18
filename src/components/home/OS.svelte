<script lang="ts">
	import Image from './assets/xp-image.png';
</script>

<div class="background">
	<div class="content">
		<div class="image">
			<img src={Image} alt="" />
		</div>
		<div class="description">
			<a href="/os/windows">> Operating Systems</a>
			<p>
				From white text on a black screen to the modern day GUI, operating systems have come a long
				way. Here are some of the most popular operating systems from the past and present.
			</p>
		</div>
	</div>
</div>

<style>
	.background {
		border-top: 2px solid rgb(71, 70, 74);
		border-bottom: 2px solid rgb(71, 70, 74);
		padding: 100px 0;
		background-color: rgb(15, 14, 14);
		box-sizing: border-box;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.content {
		display: flex;
		align-items: center;
		gap: 50px;
		width: var(--content-width);
		max-width: calc(100% - 40px);
		flex-wrap: wrap;
		justify-content: center;
	}

	.image {
		width: 500px;
		border-radius: 5px;
		overflow: hidden;
	}

	.image img {
		width: 100%;
		height: 100%;
	}

	.description {
		width: 400px;
	}

	.description a {
		font-size: var(--title-small-size);
		font-weight: var(--title-small-weight);
		color: var(--text-color-accent);
		text-decoration: none;
		transition: color 0.2s ease-in-out;
	}

	.description a:hover {
		color: var(--text-color-primary);
	}

	.description p {
		font-weight: var(--text-weight);
		font-size: var(--text-size);
		color: var(--text-color-secondary);
	}
</style>
