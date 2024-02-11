<script>
	import { marked } from 'marked';
	let text = `Some words are *italic*, some are **bold**`;	
</script>

<main>
	<textarea bind:value={text} />

	{@html marked(text)}	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	textarea {
		width: 100%;
		height: 200px;
	}
</style>