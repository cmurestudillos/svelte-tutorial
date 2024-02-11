<script>
	let string = `here's some <strong>HTML!!!</strong>`;
</script>

<main>
	<p>{@html string}</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>