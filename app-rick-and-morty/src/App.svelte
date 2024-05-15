<script>
  import { onMount } from 'svelte';

  let characters = [];
  let page = 1;

  async function loadCharacters() {
    try {
      const response = await fetch(`https://rickandmortyapi.com/api/character?page=${page}`);
      const data = await response.json();
      characters = data.results;
    } catch (error) {
      console.error('Error fetching characters:', error);
    }
  }

  function nextPage() {
    page++;
    loadCharacters();
  }

  function previousPage() {
    if (page > 1) {
      page--;
      loadCharacters();
    }
  }

  onMount(() => {
    loadCharacters();
  });
</script>

<h1>Rick y Morty en Svelte</h1>

<div>
  <button on:click={previousPage} disabled={page === 1}>Atrás</button>
  <button on:click={nextPage}>Siguiente</button>
</div>

<div>
  {#each characters as character}
    <div>
      <img src={character.image} alt={character.name}>
      <h1>{character.name}</h1>
      <h3>{character.species}</h3>
    </div>
  {/each}
</div>
