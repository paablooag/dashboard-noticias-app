<!-- src/components/SearchBar.vue -->
<template>
  <div class="search-bar">
    <input type="text" v-model="query" placeholder="Buscar noticias..." />
    <button @click="search">Buscar</button>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits();
const query = ref('');

const search = () => {
  emit('search', query.value);
  query.value = ''; // Limpia el campo de búsqueda
};
</script>

<style scoped>
.search-bar {
  display: flex;
  justify-content: center;
}

input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
  flex: 1;
  max-width: 300px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0056b3;
}
</style>
