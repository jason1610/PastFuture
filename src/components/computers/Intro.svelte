<script lang="ts">
	import Image from './assets/pc.png';
</script>

<div class="container">
	<div class="content">
		<img src={Image} alt="" />
		<div class="description">
			<h1 class="article-title">Personal Computers</h1>
			<p class="article-text">
				What started off on paper has evolved into a digital world. Here are some of the most
				popular operating systems from the past and present.
			</p>
		</div>
	</div>
</div>

<style>
	.container {
		padding: var(--carousel-padding);
		box-sizing: border-box;
		display: flex;
		background: linear-gradient(to top, #b8b8b8 5%, white 30%, white 40%, #49869e);
		flex-direction: column;
		width: 100%;
		height: 100svh;
		align-items: center;
		justify-content: center;
		cursor: default;
	}
	.content {
		display: flex;
		flex-direction: row;
		max-width: 100%;
		align-items: center;
		box-sizing: border-box;
		flex-wrap: wrap;
		justify-content: center;
	}

	.description {
		width: 500px;
		max-width: 100%;
	}

	img {
		width: 600px;
		max-width: 130%;
	}

	.article-title {
		color: var(--text-color-secondary);
	}
	.article-text {
		color: var(--text-color-secondary);
	}

	@media (max-width: 500px) {
		h1 {
			font-size: 40px;
			line-height: 35px;
		}
	}
</style>
