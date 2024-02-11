<script>
	let count = 0;

	$: if (count >= 10) {
		alert(`El contador esta creciendo peligrosamente!!`);
		count = 9;
	}

	function handleClick() {
		count += 1;
	}	
</script>

<main>
	<button on:click={handleClick}>
		Clicked {count}
		{count === 1 ? 'click' : 'clicks'}
	</button>	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>