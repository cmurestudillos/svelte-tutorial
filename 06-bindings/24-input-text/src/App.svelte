<script>
	let name = '';
</script>

<main>
	<input bind:value={name} placeholder="enter your name" />
	<p>Hello {name || 'stranger'}!</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>