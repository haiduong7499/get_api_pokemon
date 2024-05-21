<template>
  <main>
    <ListPokemon
      :listItems="pokemonList"
      :currentPage="meta.current_page"
      :totalPages="meta.last_page"
      :type="meta.type"
      :showModal="showModal"
      :selectedPokemon="pokemonDetail"
      :headers="headers"
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
import ListPokemon from './components/ListPokemon.vue';

const pokemonList = ref([]);
const imagePokemon = ref('');
const pokemonDetail = ref({});
const showModal = ref(false);
const meta = ref({
  current_page: 1,
  last_page: 1,
  type: 'number',
});
const headers = ref([
  {
    id: 1,
    name: 'Number',
    type: 'number',
    sortable: true,
  },
  {
    id: 2,
    name: 'Name',
    type: 'name',
    sortable: false,
  },
  {
    id: 3,
    name: 'Total Stats',
    type: 'total',
    sortable: true,
  },
  {
    id: 4,
    name: 'HP',
    type: 'hp',
    sortable: true,
  },
  {
    id: 5,
    name: 'Attack',
    type: 'attack',
    sortable: true,
  },
  {
    id: 6,
    name: 'Defense',
    type: 'defense',
    sortable: true,
  },{
    id: 7,
    name: 'Special Attack',
    type: 'sp_atk',
    sortable: true,
  },
  {
    id: 8,
    name: 'Special Defense',
    type: 'sp_def',
    sortable: true,
  },
  {
    id: 9,
    name: 'Speed',
    type: 'speed',
    sortable: true,
  },
])
const getPokemon = async () => {
  try {
    const response = await fetch(
      `https://api.vandvietnam.com/api/pokemon-api/pokemons?page[number]=${meta.value.current_page}&page[size]=10&sort=${meta.value.type}`
    );
    const data = await response.json();
    pokemonList.value = data.data;
    meta.value.current_page = data.meta.current_page;
    meta.value.last_page = data.meta.last_page;
  } catch (error) {
    console.error('Error fetching Pokémon data:', error);
  }
};

const getPokemonDetail = async (id) => {
  try {
    const response = await fetch(
      `https://api.vandvietnam.com/api/pokemon-api/pokemons/${id}/sprite`
    );
    const data = await response;
    imagePokemon.value = data.url;
  } catch (error) {
    console.error('Error fetching Pokémon data:', error);
  }
};

const prevPage = async () => {
  if (meta.value.current_page > 1) {
    meta.value.current_page -= 1;
    await getPokemon();
  }
};

const nextPage = async () => {
  if (meta.value.current_page < meta.value.last_page) {
    meta.value.current_page += 1;
    await getPokemon();
  }
};

const sortBy = async (type) => {
  if (meta.value.type == type) {
    meta.value.type = '-'+type;
  } else {
    meta.value.type = type;
  }
  await getPokemon();
};

const showDetails = async (pokemon) => {
  await getPokemonDetail(pokemon.id);
  pokemonDetail.value = { image: imagePokemon.value, ...pokemon };
  showModal.value = true;
};

const closeModal = () => {
  showModal.value = false;
};

onMounted(() => {
  getPokemon();
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