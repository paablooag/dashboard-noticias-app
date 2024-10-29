<!-- src/components/NewsCard.vue -->
<template>
  <div class="news-card">
    <img :src="article.urlToImage" alt="Noticia" v-if="article.urlToImage" />
    <div class="news-content">
      <h3>{{ article.title }}</h3>
      <p class="description">{{ article.description }}</p>
      <a class="read-more" :href="article.url" target="_blank">Leer más</a>
    </div>
  </div>
</template>

<script setup>
defineProps(['article']);
</script>

<style scoped>
.news-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
  background-color: #fff;
}

.news-card:hover {
  transform: translateY(-5px);
}

.news-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.news-content {
  padding: 15px;
}

h3 {
  font-size: 1.25rem;
  margin: 0 0 10px;
  font-weight: bold;
  color: #333;
}

.description {
  font-size: 0.9rem;
  color: #555;
  margin: 0 0 15px;
}

.read-more {
  text-decoration: none;
  color: #007bff;
  font-weight: bold;
}
</style>
