<template>
  <div>
    <table>
      <thead>
        <tr>
          <th @click="sortBy('number')">Number</th>
          <th>Name</th>
          <th>Type</th>
          <th @click="sortBy('total')">Total Stats</th>
          <th @click="sortBy('hp')">HP</th>
          <th @click="sortBy('attack')">Attack</th>
          <th @click="sortBy('defense')">Defense</th>
          <th @click="sortBy('sp_atk')">Special Attack</th>
          <th @click="sortBy('sp_def')">Special Defense</th>
          <th @click="sortBy('speed')">Speed</th>
          <th>Generation</th>
          <th>Legendary</th>
          <th>Created At</th>
          <th>Updated At</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pokemon in listItems" :key="pokemon.id">
          <td>{{ pokemon.number }}</td>
          <td>{{ pokemon.name }}</td>
          <td>
            {{ pokemon.type_1

            }}{{ pokemon.type_2 ? ' / ' + pokemon.type_2 : '' }}
          </td>
          <td>{{ pokemon.total }}</td>
          <td>{{ pokemon.hp }}</td>
          <td>{{ pokemon.attack }}</td>
          <td>{{ pokemon.defense }}</td>
          <td>{{ pokemon.sp_atk }}</td>
          <td>{{ pokemon.sp_def }}</td>
          <td>{{ pokemon.speed }}</td>
          <td>{{ pokemon.generation }}</td>
          <td>{{ pokemon.legendary ? 'Legendary' : 'Non-Legendary' }}</td>
          <td>{{ formatDate(pokemon.created_at) }}</td>
          <td>{{ formatDate(pokemon.updated_at) }}</td>
          <td>
            <button @click="showDetails(pokemon)">Details</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">
        Next
      </button>
    </div>
    <ModalDetail
      :showModal="showModal"
      @closeModal="closeModal"
      :selectedPokemon="selectedPokemon"
    />
  </div>
</template>

<script setup>
import { defineProps, computed, defineEmits } from 'vue';
import ModalDetail from './ModalDetail.vue';

const props = defineProps({
  listItems: {
    type: Array,
    required: true
  },
  currentPage: {
    type: Number,
    default: 1,
  },
  totalPages: {
    type: Number,
    default: 1,
  },
  itemsPerPage: {
    type: Number,
    default: 10,
  },
  showModal: {
    type: Boolean,
    default: false,
  },
  selectedPokemon: {
    type: Object,
  },
});
const emits = defineEmits(['prevPage', 'nextPage', 'sortBy', 'showDetails', 'closeModal']);
const formatDate = (date) => {
  const options = { year: 'numeric', month: 'long', day: 'numeric' };
  return new Date(date).toLocaleDateString(undefined, options);
}
function nextPage() {
  emits('nextPage');
}
function prevPage() {
  emits('prevPage');
}
function sortBy(type) {
  emits('sortBy', type);
}
const showDetails = (pokemon) => {
  emits('showDetails', pokemon);
};
const closeModal = () => {
  emits('closeModal');
};
</script>

<style>
table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
  font-size: 16px;
  text-align: left;
}
th, td {
  border: 1px solid #ddd;
  padding: 12px;
}
th {
  background-color: #f4f4f4;
  cursor: pointer;
}
tr:nth-child(even) {
  background-color: #f9f9f9;
}
tr:hover {
  background-color: #f1f1f1;
}
</style>
