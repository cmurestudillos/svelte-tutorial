<script>
	import CustomButton from './CustomButton.svelte';

	function handleClick() {
		alert('clicked');
	}	
</script>

<main>
	<CustomButton on:click={handleClick} />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>