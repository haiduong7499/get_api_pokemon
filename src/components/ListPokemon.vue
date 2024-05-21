<template>
  <div>
    <table>
      <thead>
        <tr>
          <th v-for="head in headers" :key="head.id">{{ head.name }}<img v-if="head.sortable" src="../assets/arrow-down.svg" @click="sortBy(`${head.type}`)" :class="getSortClass(`${head.type}`)"></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pokemon in listItems" :key="pokemon.id">
          <td>{{ pokemon.number }}</td>
          <td>{{ pokemon.name }}</td>
          <td>{{ pokemon.total }}</td>
          <td>{{ pokemon.hp }}</td>
          <td>{{ pokemon.attack }}</td>
          <td>{{ pokemon.defense }}</td>
          <td>{{ pokemon.sp_atk }}</td>
          <td>{{ pokemon.sp_def }}</td>
          <td>{{ pokemon.speed }}</td>
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
import { defineProps, computed, defineEmits, ref } from 'vue';
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
  type: {
    type: String,
  },
  headers: {
    type: Array,
  }
});
const sortStatus = ref('asc')
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
const getSortClass = (type) => {
  if (props.type === type) {
    return 'asc';
  }
  return '';
};
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
th img {
  margin-left: 5px;
  transition: 0.5s;
}
tr:nth-child(even) {
  background-color: #f9f9f9;
}
tr:hover {
  background-color: #f1f1f1;
}
.asc {
  transform: rotate(180deg);
}
</style>
