<script>
  let name = "";
  let stock = 0;
  let parts = [];

  async function fetchParts() {
    try {
      const response = await fetch("http://127.0.0.1:3000/api/parts");
      if (!response.ok) throw new Error("Error al obtener las partes");
      parts = await response.json();
    } catch (error) {
      console.error("Error cargando partes:", error);
    }
  }

  async function addPart() {
    try {
      const response = await fetch("http://127.0.0.1:3000/api/parts", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ name, stock }),
      });

      if (!response.ok) throw new Error("Error al agregar la parte");

      // Volver a cargar la lista de partes después de agregar
      fetchParts();
    } catch (error) {
      console.error("Error agregando parte:", error);
    }
  }

  fetchParts();
</script>

<h1>Lista de Partes</h1>
<ul class="parts-list">
  {#each parts as part}
    <li class="part-item">
      {part.name} - {part.stock} en stock
    </li>
  {/each}
</ul>

<div class="add-part-form">
  <input
    type="text"
    bind:value={name}
    placeholder="Nombre de la parte"
    class="input-field"
  />
  <input
    type="number"
    bind:value={stock}
    placeholder="Stock"
    class="input-field"
  />
  <button on:click={addPart} class="add-button">Agregar</button>
</div>

<style>
  /* Estilos globales */
  body {
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
    color: #333;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }

  h1 {
    text-align: center;
    font-size: 2rem;
    color: #444;
  }

  /* Estilos de la lista */
  .parts-list {
    list-style-type: none;
    padding: 0;
    margin: 20px 0;
    width: 100%;
    max-width: 400px;
  }

  .part-item {
    background-color: #fff;
    padding: 10px;
    margin-bottom: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  /* Estilos del formulario */
  .add-part-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    max-width: 400px;
    margin-top: 20px;
  }

  .input-field {
    padding: 10px;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    outline: none;
    width: 100%;
  }

  .input-field:focus {
    border-color: #ffcc00;
  }

  .add-button {
    background-color: #ffcc00;
    color: #000;
    padding: 10px;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    cursor: pointer;
  }

  .add-button:hover {
    background-color: #e6b800;
  }

  /* Estilos responsive */
  @media (max-width: 600px) {
    body {
      padding: 20px;
    }

    .add-part-form {
      width: 100%;
    }

    .input-field {
      font-size: 0.9rem;
    }
  }
</style>
