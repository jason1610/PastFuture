<script lang="ts">
</script>

<div class="container">
	<!-- <img src={Background} alt="" /> -->
	<div class="article">
		<h1>IBM System/360</h1>
		<h2>1968</h2>
		<p>
			The IBM System/360 Model 85 stands as a landmark in computing history due to its pioneering
			use of out-of-order execution and cache memory. The double-H shaped CPU unit in the center,
			weighed over 7 tons.Cabinets in front are core memory storage, holding 256 kilobytes each.
			This image is 4.5 megabytes. Cabinets on the right are I/O channels, connected to I/O devices
			at the back: tape drives, printers, disk drives, and card readers.
		</p>
	</div>
</div>

<style>
	.container {
		width: 100%;
		height: 100%;
		overflow: hidden;
		background-image: url(./assets/IBM360.png);
		background-repeat: no-repeat;
		background-size: cover;
		animation: pan 20s infinite ease-in-out alternate;
		display: flex;
		justify-content: center;
		align-items: flex-end;
		box-sizing: border-box;
	}

	.article {
		max-width: 100%;
		width: 500px;
		background-color: rgba(0, 0, 0, 0.5);
		background: linear-gradient(to top, rgba(0, 0, 0, 0.85) 0%, rgba(0, 0, 0, 0.5) 100%);
		border-radius: 20px 20px 0 0;
		padding: 20px;
		box-sizing: border-box;
	}

	.article h2 {
		color: var(--text-color-primary);
	}

	@media (max-width: 700px) {
		.article {
			width: 100%;
			border-radius: 0;
		}
	}

	@keyframes pan {
		0% {
			background-position: 0% 0%;
		}
		100% {
			background-position: 100% 100%;
		}
	}

	img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}
</style>
