<!-- src/components/NewsDashboard.vue -->
<template>
  <div class="news-dashboard">
    <h1>Dashboard de Noticias</h1>
    <div class="filters">
      <SearchBar @search="fetchNews" />
      <CategoryFilter @filter="fetchNewsByCategory" />
    </div>
    <div class="news-container">
      <h2 class="section-title">Noticias Destacadas</h2>
      <div class="featured-news">
        <NewsCard
          v-for="article in featuredArticles"
          :key="article.title"
          :article="article"
        />
      </div>
      <h2 class="section-title">Más Noticias</h2>

      <!-- Mostrar mensaje cuando no hay noticias -->
      <div v-if="errorMessage" class="error-message">
        <p>{{ errorMessage }}</p>
        <p>Aquí hay algunas noticias generales:</p>
      </div>

      <div class="more-news">
        <NewsCard
          v-for="article in otherArticles"
          :key="article.title"
          :article="article"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import SearchBar from './SearchBar.vue';
import CategoryFilter from './CategoryFilter.vue';
import NewsCard from './NewsCard.vue';

const articles = ref([]);
const featuredArticles = ref([]);
const otherArticles = ref([]);
const apiKey = import.meta.env.VITE_NEWS_API_KEY;
const errorMessage = ref(''); // Estado para manejar mensajes de error

// Función para obtener noticias
const fetchNews = async (query = 'general') => {
  try {
    const response = await axios.get(`https://newsapi.org/v2/top-headlines`, {
      params: { apiKey, q: query, pageSize: 10 },
    });

    articles.value = response.data.articles;

    // Comprobar si no hay noticias
    if (articles.value.length === 0) {
      errorMessage.value = 'No se encontraron noticias para tu búsqueda.';
      fetchGeneralNews(); // Llama a la función para obtener noticias generales
    } else {
      errorMessage.value = ''; // Limpiar mensaje de error si hay noticias
    }

    splitArticles(); // Llama a la función para separar artículos
  } catch (error) {
    console.error(error);
    errorMessage.value = 'Hubo un error al obtener las noticias. Inténtalo de nuevo más tarde.';
  }
};

// Función para separar artículos destacados de los demás
const splitArticles = () => {
  featuredArticles.value = articles.value.slice(0, 3); // 3 artículos destacados
  otherArticles.value = articles.value.slice(3); // Resto de los artículos
};

// Función para obtener noticias por categoría
const fetchNewsByCategory = async (category) => {
  try {
    const response = await axios.get(`https://newsapi.org/v2/top-headlines`, {
      params: { apiKey, category, pageSize: 10 },
    });
    articles.value = response.data.articles;

    // Comprobar si no hay noticias
    if (articles.value.length === 0) {
      errorMessage.value = 'No se encontraron noticias para esta categoría.';
      fetchGeneralNews(); // Llama a la función para obtener noticias generales
    } else {
      errorMessage.value = ''; // Limpiar mensaje de error si hay noticias
    }

    splitArticles(); // Llama a la función para separar artículos
  } catch (error) {
    console.error(error);
    errorMessage.value = 'Hubo un error al obtener las noticias. Inténtalo de nuevo más tarde.';
  }
};

// Función para obtener noticias generales
const fetchGeneralNews = async () => {
  try {
    const response = await axios.get(`https://newsapi.org/v2/top-headlines`, {
      params: { apiKey, category: 'general', pageSize: 5 }, // Obtener 5 noticias generales
    });
    otherArticles.value = response.data.articles; // Asigna a otros artículos
  } catch (error) {
    console.error(error);
  }
};

// Llamada inicial para cargar noticias al montar el componente
onMounted(() => fetchNews());
</script>

<style scoped>
.news-dashboard {
  padding: 20px;
  max-width: 1200px;
  margin: auto;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.filters {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
}

.news-container {
  margin-top: 20px;
}

.section-title {
  margin: 20px 0 10px;
  font-size: 1.5rem;
  color: #333;
  text-align: left;
  border-bottom: 2px solid #007bff;
  padding-bottom: 5px;
}

.featured-news {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.more-news {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.error-message {
  color: red;
  font-weight: bold;
  margin: 20px 0;
}
</style>
