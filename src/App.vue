<script setup>
import { ref, onMounted } from 'vue';
import HelloWorld from './components/HelloWorld.vue'
import ListPokemon from './components/ListPokemon.vue'
const pokemonList = ref([]);

const fetchPokemon = async () => {
  try {
    const response = await fetch('https://api.vandvietnam.com/api/pokemon-api/pokemons?page[number]=1&page[size]=10&sort=number');
    const data = await response.json();
    pokemonList.value = data.data; // Adjust this according to the actual structure of the API response
  } catch (error) {
    console.error('Error fetching Pokémon data:', error);
  }
};

onMounted(() => {
  fetchPokemon();
});
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <ListPokemon :listItems="pokemonList" v-if="pokemonList.length" />
    <p v-else>Loading...</p>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
