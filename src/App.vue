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
      :typeList="typeList"
      @prevPage="prevPage"
      @nextPage="nextPage"
      @sortBy="sortBy"
      @handleSelectChange="handleSelectChange"
      @showDetail="showDetail"
      @closeModal="closeModal"
      v-if="pokemonList.length"
    />
    <div v-else>
      <div class="loader"></div>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ListPokemon from './components/ListPokemon.vue';

const pokemonList = ref([]);
const typeList = ref([]);
const imagePokemon = ref('');
const pokemonDetail = ref({});
const showModal = ref(false);
const typePokemon = ref();
const meta = ref({
  current_page: 1,
  last_page: 1,
  type: 'number',
});
const headers = ref([
  {
    name: 'Number',
    type: 'number',
    sortable: true,
  },
  {
    name: 'Name',
    type: 'name',
    sortable: false,
  },
  {
    name: 'Type',
    type: 'type',
    sortable: false,
  },
  {
    name: 'Total Stats',
    type: 'total',
    sortable: true,
  },
  {
    name: 'HP',
    type: 'hp',
    sortable: true,
  },
  {
    name: 'Attack',
    type: 'attack',
    sortable: true,
  },
  {
    name: 'Defense',
    type: 'defense',
    sortable: true,
  },
  {
    name: 'Special Attack',
    type: 'sp_atk',
    sortable: true,
  },
  {
    name: 'Special Defense',
    type: 'sp_def',
    sortable: true,
  },
  {
    name: 'Speed',
    type: 'speed',
    sortable: true,
  },
])
const getPokemon = async () => {
  try {
    const response = await fetch(
      `https://api.vandvietnam.com/api/pokemon-api/pokemons?page[number]=${meta.value.current_page}
        &page[size]=10
        &sort=${meta.value.type}${typePokemon.value ? typePokemon.value : ''}`
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
const getTypes = async (id) => {
  try {
    const response = await fetch(
      `https://api.vandvietnam.com/api/pokemon-api/types`
    );
    const data = await response.json();
    typeList.value = data.data;
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
const handleSelectChange = async (type) => {
  if(type !== "") {
    typePokemon.value = `&filter[type]=${type}`
  } else {
    typePokemon.value = type
  }
  await getPokemon();
}
const showDetail = async (pokemon) => {
  await getPokemonDetail(pokemon.id);
  pokemonDetail.value = { image: imagePokemon.value, ...pokemon };
  showModal.value = true;
};

const closeModal = () => {
  showModal.value = false;
};

onMounted(() => {
  getPokemon();
  getTypes();
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
.loader {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  animation: spin 2s linear infinite;
  margin: auto;
  margin-top: 50px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
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
