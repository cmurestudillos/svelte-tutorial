<script>
	import { onMount } from 'svelte';
	import Dexie from 'dexie';
	import 'prismjs/themes/prism.css';
	import 'prismjs/components/prism-core';

	// Componentes
	import Header from './components/Header.svelte';
	import SnippetsGuardados from './components/SnippetsGuardados.svelte';
	import SnippetsFormulario from './components/SnippetsFormulario.svelte';
  
	let snippets = [];
	let db;

	onMount(async () => {
		// Configurar y abrir la base de datos IndexedDB con Dexie.js
		db = new Dexie('SnippetsDatabase');
		db.version(1).stores({
			snippets: '++id, language, code, description',
		});

		// Obtener los snippets guardados al cargar la página
		snippets = await getAllSnippets();
	});

	const handleSnippetSaved = async () => {
		// Actualizar la lista de snippets cuando se guarda uno nuevo
		snippets = await getAllSnippets();
	};

	const getAllSnippets = async () => {
		try {
			// Obtener todos los snippets y ordenarlos por el campo "language"
			return await db.snippets.orderBy('language').toArray();
		} catch (error) {
			console.error('Error al obtener snippets:', error);
			return [];
		}
	};

	const handleDeleteSnippet = (event) => {
        const { id } = event.detail;
		// Lógica para eliminar el snippet con el ID proporcionado de tu base de datos
		deleteSnippetById(id);
		snippets = snippets.filter(snippet => snippet.id !== id);
    };

	// Función para eliminar un snippet por su ID usando Dexie
	async function deleteSnippetById(snippetId) {
        try {
            // Usa el método delete para eliminar el snippet de la tabla snippets
            await db.snippets.delete(snippetId);
            console.log(`Snippet con ID ${snippetId} eliminado correctamente.`);
        } catch (error) {
            console.error('Error al eliminar el snippet:', error);
        }
    }

  </script>
	<Header on:snippetSaved={handleSnippetSaved} />
  <main>
	<div class="container">
	  <div class="saved-snippets">
		<SnippetsGuardados {snippets} on:deleteSnippet={handleDeleteSnippet} />
	</div>

	<div class="vl"></div>

	<div class="insert-snippets">
		<SnippetsFormulario on:snippetSaved={handleSnippetSaved} />
	</div>
  </main>
  
  <style>
	main {
		text-align: center;
		padding: 0.25em;
		max-width: 100%;
		margin: 0 auto;
	}
  
	.container {
		display: flex;
		justify-content: space-between;
		margin-top: 1rem;
		height: calc(100vh - 24rem);
	}
  
	.saved-snippets, .insert-snippets {
		width: 48%;
	}

	.vl {
		border-left: 1px solid black;
		height: calc(100vh - 10rem);
	}
</style>
  