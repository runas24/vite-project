<template>
    <div class="character-list">
      <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
    </div>
  </template>
  
  <script>
  import CharacterCard from './CharacterCard.vue';
  
  export default {
    props: {
      characters: Array,
    },
    components: {
      CharacterCard,
    },
  };
  </script>
  
  <style scoped>
  .character-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    grid-gap: 16px;
  }
  </style>
  