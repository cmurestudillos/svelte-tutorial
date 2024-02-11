<script>
	let todos = [
		{ done: false, text: 'finish Svelte tutorial' },
		{ done: false, text: 'build an app' },
		{ done: false, text: 'world domination' }
	];

	function add() {
		todos = todos.concat({ done: false, text: '' });
	}

	function clear() {
		todos = todos.filter((t) => !t.done);
	}

	$: remaining = todos.filter((t) => !t.done).length;	
</script>

<main>
	<h1>Todos</h1>

	{#each todos as todo}
		<div>
			<input type="checkbox" bind:checked={todo.done} />
	
			<input placeholder="What needs to be done?" bind:value={todo.text} disabled={todo.done} />
		</div>
	{/each}
	
	<p>{remaining} remaining</p>
	
	<button on:click={add}> Add new </button>
	
	<button on:click={clear}> Clear completed </button>	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>