<script>
	let yes = false;
</script>

<main>
	<label>
		<input type="checkbox" bind:checked={yes} />
		Yes! Send me regular email spam
	</label>
	
	{#if yes}
		<p>Thank you. We will bombard your inbox and sell your personal details.</p>
	{:else}
		<p>You must opt-in to continue. If you're not paying, you're the product.</p>
	{/if}
	
	<button disabled={!yes}> Subscribe </button>	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
</style>