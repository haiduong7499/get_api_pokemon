<template>
  <div>
    <div v-for="pokemon in listItems" :key="pokemon.id" class="pokemon-card">
      <h2>{{ pokemon.number }}. {{ pokemon.name }}</h2>
      <p>Type: {{ pokemon.type_1 }}{{ pokemon.type_2 ? ' / ' + pokemon.type_2 : '' }}</p>
      <p>Total Stats: {{ pokemon.total }}</p>
      <p>HP: {{ pokemon.hp }}</p>
      <p>Attack: {{ pokemon.attack }}</p>
      <p>Defense: {{ pokemon.defense }}</p>
      <p>Special Attack: {{ pokemon.sp_atk }}</p>
      <p>Special Defense: {{ pokemon.sp_def }}</p>
      <p>Speed: {{ pokemon.speed }}</p>
      <p>Generation: {{ pokemon.generation }}</p>
      <p>{{ pokemon.legendary ? 'Legendary' : 'Non-Legendary' }}</p>
      <p>Created At: {{ formatDate(pokemon.created_at) }}</p>
      <p>Updated At: {{ formatDate(pokemon.updated_at) }}</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  listItems: {
    type: Array,
    required: true
  }
});

function formatDate(dateString) {
  return new Date(dateString).toLocaleString();
}
</script>

<style>
.pokemon-card {
  border: 1px solid #ccc;
  padding: 16px;
  margin: 8px;
}
</style>