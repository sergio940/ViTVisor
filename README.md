<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pokédex - Vitvisor PokéWorld</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(180deg, #cc0000, #111);
      color: white;
      margin: 0;
      padding: 0;
      text-align: center;
    }

    header {
      background: #e00000;
      padding: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.4);
    }

    h1 {
      font-size: 2rem;
      margin: 0;
      letter-spacing: 2px;
    }

    #controls {
      margin: 20px 0;
    }

    input, select {
      padding: 10px;
      border: none;
      border-radius: 10px;
      margin: 5px;
      font-size: 1rem;
    }

    #pokemon-container {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 15px;
      padding: 20px;
      max-width: 1200px;
      margin: auto;
    }

    .pokemon-card {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 15px;
      padding: 15px;
      transition: transform 0.2s, background 0.3s;
      box-shadow: 0 0 10px rgba(0,0,0,0.3);
    }

    .pokemon-card:hover {
      transform: scale(1.05);
      background: rgba(255,255,255,0.2);
    }

    .pokemon-card img {
      width: 120px;
      height: 120px;
      image-rendering: pixelated;
    }

    .types {
      margin-top: 5px;
      display: flex;
      justify-content: center;
      gap: 8px;
    }

    .type {
      padding: 4px 8px;
      border-radius: 8px;
      background: rgba(0,0,0,0.4);
      font-size: 0.8rem;
      text-transform: capitalize;
    }

    footer {
      margin-top: 30px;
      padding: 15px;
      font-size: 0.9rem;
      opacity: 0.8;
    }
  </style>
</head>
<body>
  <header>
    <h1>Pokédex - Vitvisor PokéWorld</h1>
  </header>

  <section id="controls">
    <input type="text" id="search" placeholder="Buscar Pokémon por nombre o número...">
    <select id="generation">
      <option value="all">Todas las generaciones</option>
      <option value="1">Generación I</option>
      <option value="2">Generación II</option>
      <option value="3">Generación III</option>
      <option value="4">Generación IV</option>
      <option value="5">Generación V</option>
      <option value="6">Generación VI</option>
      <option value="7">Generación VII</option>
      <option value="8">Generación VIII</option>
      <option value="9">Generación IX</option>
    </select>
  </section>

  <section id="pokemon-container"></section>

  <footer>
    Vitvisor PokéWorld © 2025 - Datos desde PokéAPI
  </footer>

  <script>
    const container = document.getElementById('pokemon-container');
    const searchInput = document.getElementById('search');
    const generationSelect = document.getElementById('generation');

    let allPokemon = [];

    async function fetchAllPokemon() {
      const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=1025');
      const data = await response.json();
      allPokemon = data.results.map((p, index) => ({
        name: p.name,
        url: p.url,
        id: index + 1
      }));
      renderPokemon(allPokemon);
    }

    async function renderPokemon(pokemonList) {
      container.innerHTML = '';
      for (const pokemon of pokemonList) {
        const res = await fetch(pokemon.url);
        const data = await res.json();
        const types = data.types.map(t => `<span class="type">${t.type.name}</span>`).join('');
        const card = document.createElement('div');
        card.classList.add('pokemon-card');
        card.innerHTML = `
          <img src="${data.sprites.other['official-artwork'].front_default}" alt="${pokemon.name}">
          <h3>#${pokemon.id.toString().padStart(4, '0')} ${pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)}</h3>
          <div class="types">${types}</div>
        `;
        container.appendChild(card);
      }
    }

    searchInput.addEventListener('input', () => {
      const value = searchInput.value.toLowerCase();
      const filtered = allPokemon.filter(p => p.name.includes(value) || p.id.toString() === value);
      renderPokemon(filtered.slice(0, 50)); // limit to 50 for performance
    });

    generationSelect.addEventListener('change', async () => {
      const gen = generationSelect.value;
      if (gen === 'all') {
        renderPokemon(allPokemon.slice(0, 100));
      } else {
        const genResponse = await fetch(`https://pokeapi.co/api/v2/generation/${gen}`);
        const genData = await genResponse.json();
        const pokemonNames = genData.pokemon_species.map(p => p.name);
        const filtered = allPokemon.filter(p => pokemonNames.includes(p.name));
        renderPokemon(filtered);
      }
    });

    fetchAllPokemon();
  </script>
</body>
</html>
