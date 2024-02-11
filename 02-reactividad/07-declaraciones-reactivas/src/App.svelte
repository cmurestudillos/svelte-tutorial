<script>
	let count = 1;

	// the `$:` means 're-run whenever these values change'
	$: doubled = count * 2;
	$: quadrupled = doubled * 2;

	function handleClick() {
		count += 1;
	}	
</script>

<main>
	<button on:click={handleClick}>
		Contador: {count}
	</button>
	
	<p>{count} * 2 = {doubled}</p>
	<p>{doubled} * 2 = {quadrupled}</p>	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>