<script>
  import { createEventDispatcher } from 'svelte';
  import Dexie from 'dexie';
  import { Button, Icon } from 'svelte-materialify';
  import { mdiFileImport, mdiFileExport } from '@mdi/js';
  import Swal from 'sweetalert2';
  import packageJson  from '../../package.json'

  const dispatch = createEventDispatcher();

  const handleExport = async () => {
    const db = new Dexie('SnippetsDatabase');
    db.version(1).stores({
      snippets: '++id, language, code, description',
    });

    const snippets = await db.snippets.toArray();
    const data = JSON.stringify(snippets);

    const blob = new Blob([data], { type: 'application/json' });
    const url = URL.createObjectURL(blob);

    const a = document.createElement('a');
    a.href = url;
    a.download = 'snippets.json';

    // Agregar el enlace al DOM
    document.body.appendChild(a);

    // Simular clic en el enlace para iniciar la descarga
    a.click();

    // Eliminar el enlace del DOM después de un breve retraso
    setTimeout(() => {
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }, 100);
  };

  const handleImport = async () => {
    const fileInput = document.createElement('input');
    fileInput.type = 'file';

    fileInput.addEventListener('change', async (event) => {
      const file = event.target.files[0];
      if (file) {
        try {
          const jsonData = await file.text();
          const data = JSON.parse(jsonData)
          const importedSnippets = data || [];
          
          const result = await Swal.fire({
              title: 'Importar Snippets',
              text: '¿Estás seguro de que deseas importar los datos?',
              icon: 'question',
              showCancelButton: true,
              confirmButtonText: 'Confirmar',
              confirmButtonColor: "#2ecc71",
              cancelButtonText: 'Cancelar',
              cancelButtonColor: "#d33",
          });

          // Si el usuario confirma, importar los datos el snippet
          if (result.isConfirmed) {
            const db = new Dexie('SnippetsDatabase');
            db.version(1).stores({
              snippets: '++id, language, code, description',
            });

            await db.transaction('rw', db.snippets, async () => {
              // Eliminamos todos los snippets actuales
              await db.snippets.clear();
              // Insertamos los nuevos snippets
              await Promise.all(importedSnippets.map(snippet => db.snippets.add(snippet)));
            });

            dispatch('snippetSaved'); // Notificar al componente padre que se ha guardado un snippet

            // Modal de confirmación
            Swal.fire({
                position: "top-end",
                icon: "success",
                title: "Snippets importados correctamente",
                showConfirmButton: false,
                timer: 1500
            });
          }
        } catch (error) {
          // Mostrar mensaje de error
          Swal.fire({
            title: 'Error de importación',
            text: 'Hubo un error al importar los snippets. Asegúrate de que el archivo sea válido.',
            icon: 'error',
            confirmButtonColor: '#e74c3c',
          });
        }
      }
    });
    fileInput.click();
  };

</script>

<header class="site-header">
    <div class="site-identity">
      <h2>Snippet Manager</h2> <i>v{packageJson.version}</i>
    </div>  
    <div style="flex-grow:1" />
    <div class="buttons">
      <Button tile on:click={handleExport} class="blue  white-text">
        <Icon path={mdiFileExport} />Exportar
      </Button>
      <Button tile on:click={handleImport} class="blue  white-text">
        <Icon path={mdiFileImport} />Importar
      </Button>
    </div>
</header>
  

<style>
.site-header { 
  border-bottom: 1px solid #ccc;
  padding: .5em 1em;
  display: flex;
  justify-content: space-between;
}

.site-identity h2 {
  margin: .3em 0;
  display: inline-block;
  font-style: italic;
}


.site-navigation ul, 
.site-navigation li {
  margin: 0; 
  padding: 0;
}

.site-navigation li {
  display: inline-block;
  margin: 1.4em 1em 1em 1em;
}

.buttons button {
    margin-left: 1em;
    padding: 0.5em 1em;
    cursor: pointer;
  }
</style>