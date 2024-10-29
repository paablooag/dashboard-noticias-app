<!-- src/components/CategoryFilter.vue -->
<template>
  <div class="category-filter">
    <select @change="filter">
      <option value="">Todas las Categorías</option>
      <option value="business">Negocios</option>
      <option value="entertainment">Entretenimiento</option>
      <option value="health">Salud</option>
      <option value="science">Ciencia</option>
      <option value="sports">Deportes</option>
      <option value="technology">Tecnología</option>
    </select>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits();

const filter = (event) => {
  emit('filter', event.target.value);
};
</script>

<style scoped>
.category-filter {
  display: flex;
  justify-content: center;
}

select {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
