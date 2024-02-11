<script>
    import { onMount, createEventDispatcher } from 'svelte';
    import { Button, Row, Col, Textarea, TextField, Select, MaterialApp } from 'svelte-materialify';
    import Dexie from 'dexie';
    import Swal from 'sweetalert2';
    import { languages } from '../js/languagues.js';

    const dispatch = createEventDispatcher();

    // Configurar y abrir la base de datos IndexedDB con Dexie.js
    const db = new Dexie('SnippetsDatabase');
    db.version(1).stores({
      snippets: '++id, language, code, description',
    });
  
    let code = '';
    let language = ''; 
    let description = '';
    const items = languages;
    let editingSnippetId = null;
  
    // Función para guardar un snippet en la base de datos
    const saveSnippet = async () => {
        const result = await Swal.fire({
            title: 'Guardar Snippet',
            text: '¿Estás seguro de que deseas guardar este snippet?',
            icon: 'question',
            showCancelButton: true,
            confirmButtonText: 'Sí, guardar',
            confirmButtonColor: "#2ecc71",
            cancelButtonText: 'Cancelar',
            cancelButtonColor: "#d33",
        });

        // Si el usuario confirma, guardar el snippet
        if (result.isConfirmed) {
            if (editingSnippetId !== null) {
                await db.snippets.update(editingSnippetId, { language, code, description });
                editingSnippetId = null; // Salir del modo de edición después de la actualización
            } else {
                await db.snippets.add({ language, code, description });
            }

            code = ''; // Limpiar el código después de guardar
            description = ''; // Limpiar la descripción después de guardar
            language = ''; // Limpiar el lenguaje después de guardar
            dispatch('snippetSaved'); // Notificar al componente padre que se ha guardado un snippet

            // Modal de confirmación
            Swal.fire({
                position: "top-end",
                icon: "success",
                title: "Snippet guardado correctamente",
                showConfirmButton: false,
                timer: 1500
            });
        }
    };
  
    const highlightCode = () => {
        Prism.highlightAll();
    };

    onMount(() => {
        highlightCode(); // Llamado al montar el componente
    });
  
</script>
  
<div class="title">
    <h3 class="h2">Snippet Code</h3>
    <hr class="hr">
</div>
  
<MaterialApp>
    <Row>
        <Col cols={12} sm={6} md={4}>
            <Select class="text-left" outlined {items} bind:value={language} placeholder="Lenguajes...">Lenguajes</Select>
        </Col>
        <Col cols={12} sm={6} md={8}>
            <TextField bind:value={description} placeholder="Descripción del snippet" outlined>Descripción</TextField>
        </Col>
    </Row>
    <Row>
        <Col>
            <Textarea rows={17} bind:value={code} placeholder="Ingresa tu código aquí" outlined>Codigo</Textarea>
        </Col>
    </Row>
    <div class="buttons">
        <Button on:click={saveSnippet} class="green white-text">Guardar Snippet</Button>
    </div>
</MaterialApp>
  
<style>
    .title .h2, .hr {
        color: #2ecc71;
        margin-bottom: 1rem;
    }

	textarea {
        width: 100%;
        height: 150px;
        margin-bottom: 1em;
        padding: 10px;
        font-size: 16px;
	}
  
	input {
        width: 100%;
        margin-bottom: 1em;
        padding: 10px;
        font-size: 16px;
	}

    .buttons {
        display: flex;
        justify-content: flex-end;
    }
</style>