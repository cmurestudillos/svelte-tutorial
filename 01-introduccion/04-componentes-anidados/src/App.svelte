<script>
	import Nested from './Nested.svelte';
</script>

<main>
	<p>These styles...</p>
	<Nested />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	p {
		color: purple;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>