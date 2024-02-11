<script>
	let user = { loggedIn: false };

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}
</script>

<main>
	{#if user.loggedIn}
	<button on:click={toggle}> Log out </button>
	{/if}

	{#if !user.loggedIn}
		<button on:click={toggle}> Log in </button>
	{/if}
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>