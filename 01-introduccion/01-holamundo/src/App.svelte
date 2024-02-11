<script>
	// Este componente Svelte recibe una prop 'name'
	export let name;
</script>

<main>
	<!-- Renderiza un encabezado con el nombre proporcionado -->
	<h1>Hello {name}!</h1>
</main>

<style>
	main {
		/* Estilo del contenedor principal */
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
	h1 {
		/* Estilo del encabezado */
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	@media (min-width: 640px) {
		/* Estilo adicional para pantallas más grandes */
		main {
			max-width: none;
		}
	}
</style>
