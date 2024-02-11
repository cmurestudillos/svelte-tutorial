<script>
	let cats = [
		{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
		{ id: 'z_AbfPXTKms', name: 'Maru' },
		{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
	];	
</script>

<main>
	<h1>The Famous Cats of YouTube</h1>

	<ul>
		{#each cats as { id, name }, i}
			<li>
				<a target="_blank" rel="noreferrer" href="https://www.youtube.com/watch?v={id}">
					{i + 1}: {name}
				</a>
			</li>
		{/each}
	</ul>	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>