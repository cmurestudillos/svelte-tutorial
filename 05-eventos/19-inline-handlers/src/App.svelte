<script>
	let m = { x: 0, y: 0 };
</script>

<main>
	<div on:mousemove={(e) => (m = { x: e.clientX, y: e.clientY })}>
		The mouse position is {m.x} x {m.y}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	main div {
		width: 100%;
		height: 100%;
	}
</style>