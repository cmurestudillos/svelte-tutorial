<script>
  import { createEventDispatcher } from 'svelte';
  import { mdiDelete } from '@mdi/js';
  import { MaterialApp, Icon, ExpansionPanel, ExpansionPanels, Button, TextField } from 'svelte-materialify';
  import { afterUpdate, onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import Swal from 'sweetalert2';

  export let snippets = []; // Recibe la lista de snippets como una propiedad
  let searchTerm = '';
  const filteredSnippetsStore = writable([]);
  const dispatch = createEventDispatcher();

  $: filteredSnippets = $filteredSnippetsStore;

  function filterSnippets() {
    const filtered = snippets.filter(snippet => 
      snippet.language.toLowerCase().includes(searchTerm.toLowerCase()) ||
      snippet.description.toLowerCase().includes(searchTerm.toLowerCase()) ||
      snippet.code.toLowerCase().includes(searchTerm.toLowerCase())
    );
    filteredSnippetsStore.set(filtered);
  }

  // Función para manejar el clic en el botón Eliminar
  const handleDelete = async (id, description) => {
    const result = await Swal.fire({
      title: 'Eliminar Snippet',
      text: `¿Estás seguro de que deseas eliminar el snippet "${description}"?`,
      icon: 'warning',
      showCancelButton: true,
      confirmButtonText: 'Sí, eliminar',
      confirmButtonColor: "#2ecc71",
      cancelButtonText: 'Cancelar',
      cancelButtonColor: "#d33",
    });

    if (result.isConfirmed) {
      dispatch('deleteSnippet', { id }); // Emitir el evento con el ID en el detalle
    }
  }

  onMount(() => {
    filterSnippets(); // Filtrar snippets al cargar el componente
  });

  // Filtrar snippets después de cada actualización
  afterUpdate(() => {
    filterSnippets();
  });
  
</script>

<div class="title">
  <h3 class="h2">Snippets Guardados</h3>
  <hr class="hr">
</div>

<div class="search-container">
  <MaterialApp>
    <TextField bind:value={searchTerm} placeholder="Buscar..." outlined>Buscar</TextField>
  </MaterialApp>
</div>

<div class="scrollable-container">
  {#if filteredSnippets.length > 0}
    {#each filteredSnippets as { id, language, code, description }}
      <ExpansionPanels multiple>
        <ExpansionPanel>
          <span slot="header">
            <div class="accordeon-header">
              <div class="accordeon-actions">
                <Button fab on:click={() => handleDelete(id, description)} class="red white-text" size="x-small">
                  <Icon size="16px" path={mdiDelete} />
                </Button>
              </div>
              <div class="accordeon-text">
                <span class="text-capitalize font-weight-bold">{language}</span> / {description}
              </div>
            </div>
          </span>
          <pre class="snippet">
            <code class={`language-${language}`}>{code}</code>
          </pre>  
        </ExpansionPanel>
      </ExpansionPanels>
    {/each}
  {:else}
    <p style="color: #e74c3c;">No hay snippets guardados.</p>
  {/if}
</div>

<style>
  .title .h2, .hr {
    color: #3498db;
    margin-bottom: 1rem;
  }

  .search-container {
    margin-bottom: 1rem;
  }

  .scrollable-container {
    height: calc(100vh - 20rem); 
    overflow-y: auto; 
    border: 1px dotted grey;
    padding: 0.75rem;
  }

  .scrollable-container::-webkit-scrollbar {
    width: 0.5rem;
  }

  .scrollable-container::-webkit-scrollbar-thumb {
    background-color: #ff3d00; 
    border-radius: 6px; 
  }

  .scrollable-container::-ms-scrollbar {
    width: 12px;
  }

  .accordeon-header{
    display: flex;
    align-items: center;
  }

  .accordeon-actions{
    margin-right: 1rem;
  }

  .accordeon-text{
    margin-left: 1rem;
  }

  .snippet {
    display: flex;
    margin-bottom: 1em;
    white-space: pre;
    text-align: left;
    font-size: 0.75rem;
    font-family: monospace;
  }
</style>
