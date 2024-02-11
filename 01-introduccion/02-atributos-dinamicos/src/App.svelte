<script>
	export let src = './img/image.gif';
	export let name = 'Rick Astley';
</script>

<main>
	<img {src} alt="{name} dancing" />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>