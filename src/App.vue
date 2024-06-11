// App.vue
<template>
  <div id="app">
    <h1>Rick and Morty Characters</h1>
    <div>
      <label for="name">Name:</label>
      <input type="text" id="name" v-model="nameFilter" />
    </div>
    <div>
      <label for="status">Status:</label>
      <select id="status" v-model="statusFilter">
        <option value="">All</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
    </div>
    <button @click="applyFilters">Apply</button>
    <div>
      <button @click="prevPage" :disabled="page === 1">Previous</button>
      <button @click="nextPage" :disabled="page >= totalPages">Next</button>
    </div>
    <div class="cards">
      <div v-for="character in characters" :key="character.id">
        <div class="card">
          <img :src="character.image" alt="Character" />
          <div><strong>Name:</strong> {{ character.name }}</div>
          <div><strong>Status:</strong> {{ character.status }}</div>
          <div><strong>Species:</strong> {{ character.species }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, watchEffect } from 'vue';

export default {
  setup() {
    const characters = ref([]);
    const nameFilter = ref('');
    const statusFilter = ref('');
    const page = ref(1);
    const totalPages = ref(1);

    const fetchCharacters = async () => {
      const url = new URL('https://rickandmortyapi.com/api/character');
      url.searchParams.append('page', page.value);
      if (nameFilter.value) {
        url.searchParams.append('name', nameFilter.value);
      }
      if (statusFilter.value) {
        url.searchParams.append('status', statusFilter.value);
      }

      try {
        const response = await fetch(url);
        const data = await response.json();
        characters.value = data.results;
        totalPages.value = data.info.pages;
      } catch (error) {
        console.error('Error fetching characters:', error);
      }
    };

    const applyFilters = () => {
      page.value = 1;
      fetchCharacters();
    };

    const nextPage = () => {
      page.value++;
      fetchCharacters();
    };

    const prevPage = () => {
      page.value--;
      fetchCharacters();
    };

    watchEffect(fetchCharacters);

    return {
      characters,
      nameFilter,
      statusFilter,
      page,
      totalPages,
      applyFilters,
      nextPage,
      prevPage,
    };
  },
};
</script>

<style>
.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  width: 200px;
}

img {
  width: 100%;
  border-radius: 5px 5px 0 0;
}
</style>
