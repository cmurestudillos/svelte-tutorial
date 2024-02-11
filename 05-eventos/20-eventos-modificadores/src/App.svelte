<script>
	function handleClick() {
		alert('no more alerts');
	}
</script>

<main>
	<button on:click|once={handleClick}> Click me </button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>