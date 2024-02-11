<script>
	let w;
	let h;
	let size = 42;
	let text = 'edit me';	
</script>

<main>
	<input type="range" bind:value={size} />
	<input bind:value={text} />
	
	<p>size: {w}px x {h}px</p>
	
	<div bind:clientWidth={w} bind:clientHeight={h}>
		<span style="font-size: {size}px">{text}</span>
	</div>	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	input {
		display: block;
	}
	div {
		display: inline-block;
	}
</style>