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
<ul>
  {#each parts as part}
    <li>{part.name} - {part.stock} en stock</li>
  {/each}
</ul>

<h2>Agregar Parte</h2>
<input type="text" bind:value={name} placeholder="Nombre de la parte" />
<input type="number" bind:value={stock} placeholder="Stock" />
<button on:click={addPart}>Agregar</button>
