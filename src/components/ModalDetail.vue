<template>
  <div v-if="showModal" class="modal">
    <div class="modal-content">
      <div class="modal-left">
        <div class="image-container">
          <img :src="selectedPokemon.image" alt="Pokemon Image" />
        </div>
      </div>
      <div class="modal-right">
        <p>Name: {{ selectedPokemon.name }}</p>
        <p>Type:
          {{ selectedPokemon.type_name }}
        </p>
        <p>Total: {{ selectedPokemon.total }}</p>
        <p>HP: {{ selectedPokemon.hp }}</p>
        <p>Attack: {{ selectedPokemon.attack }}</p>
        <p>Defense: {{ selectedPokemon.defense }}</p>
        <p>Special Attack: {{ selectedPokemon.sp_atk }}</p>
        <p>Special Defense: {{ selectedPokemon.sp_def }}</p>
        <p>Speed: {{ selectedPokemon.speed }}</p>
        <p>Generation: {{ selectedPokemon.generation }}</p>
        <p>Legendary: {{ selectedPokemon.legendary ? 'Legendary' : 'Non-Legendary' }}</p>
        <span class="close" @click="closeModal">&times;</span>
      </div>
    </div>
  </div>
</template>
<script setup>
import { defineProps, computed, defineEmits } from 'vue';
const emits = defineEmits(['closeModal'])
const props = defineProps({
  showModal: {
    type: Boolean,
    default: false,
  },
  selectedPokemon: {
    type: Object,
    default: {},
  },
});
const closeModal = () => {
  emits('closeModal');
};
</script>
<style>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0,0,0,0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 40%;
  display: flex;
  position: relative;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  position: absolute;
  top: 0;
  right: 10px;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.modal-left {
  flex: 1;
}

.modal-left img {
  max-width: 100%;
  max-height: 100%;
}

.modal-right {
  flex: 1;
  padding-left: 20px;
}

.modal-right h2 {
  margin-top: 0;
}
.image-container {
  border: 2px solid #ccc;
  padding: 5px;
  width: 300px;
  height: 300px;
}

.image-container img {
  width: 100%;
  height: auto;
}
</style>
