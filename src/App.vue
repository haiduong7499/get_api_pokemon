<template>
  <main>
    <ListPokemon
      :listItems="pokemonList"
      :currentPage="meta.current_page"
      :totalPages="meta.last_page"
      :type="meta.type"
      :showModal="showModal"
      :selectedPokemon="pokemonDetail"
      @prevPage="prevPage"
      @nextPage="nextPage"
      @sortBy="sortBy"
      @showDetails="showDetails"
      @closeModal="closeModal"
      v-if="pokemonList.length"
    />
    <p v-else>Loading...</p>
  </main>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import ListPokemon from './components/ListPokemon.vue'
const pokemonList = ref([]);
const imagePokemon = ref('');
let pokemonDetail = ref({});
let showModal = ref(false);
const meta = ref({
  current_page: 1,
  type: 'number',
});
const getPokemon = async () => {
  try {
    const response = await fetch(`https://api.vandvietnam.com/api/pokemon-api/pokemons?page[number]=${meta.value.current_page}&page[size]=10&sort=${meta.value.type}`);
    const data = await response.json();
    pokemonList.value = data.data;
    meta.value = data.meta;
  } catch (error) {
    console.error('Error fetching Pokémon data:', error);
  }
};

const getPokemonDetail = async (id) => {
  try {
    const response = await fetch(`https://api.vandvietnam.com/api/pokemon-api/pokemons/${id}/sprite`);
    const data = await response;
    imagePokemon.value = data.url;
  } catch (error) {
    console.error('Error fetching Pokémon data:', error);
  }
};

async function prevPage(page) {
  meta.value.current_page = meta.value.current_page - 1
  await getPokemon();
}
async function nextPage(page) {
  meta.value.current_page = meta.value.current_page + 1
  await getPokemon();
}
async function sortBy(type) {
  meta.value.type = type;
  await getPokemon();
}

async function showDetails(pokemon) {
  await getPokemonDetail(pokemon.id)
  pokemonDetail.value = { image:imagePokemon.value, ...pokemon}
  console.log(pokemonDetail.value)
  showModal.value = true;
}
function closeModal() {
  showModal.value = !showModal.value;
}
onMounted(() => {
  getPokemon(meta.value);
});
</script>
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

