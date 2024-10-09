<template>
    <div class="Api">
      <h1>Consumo de APIs</h1>
  
      <div class="button-container">
        <button @click="fetchData(1)">Ver población por países</button>
        <button @click="fetchData(2)">Recomendaciones de anime</button>
        <button @click="fetchData(3)">Anime aleatorio</button>
        <button @click="fetchData(4)">Manga aleatorio</button>
      </div>
  
      <div v-if="loading" class="cargando">
        <p>Cargando...</p>
      </div>

      <div v-if="data && !loading" class="datos">
        <h3>Datos obtenidos:</h3>
        <div v-if="apiIndex === 1">
          <ul>
            <li v-for="country in data.data" :key="country.country">
              <strong>{{ country.country }}:</strong> {{ country.populationCounts[country.populationCounts.length - 1].value.toLocaleString() }} habitantes
            </li>
          </ul>
        </div>
        

        <div v-if="apiIndex === 2">
          <ul>
            <li v-for="anime in data.data" :key="anime.entry.mal_id">
              <strong>{{ anime.entry.title }} :</strong> Recomendado por {{ anime.user.username }}
            </li>
          </ul>
        </div>
  
        <div v-if="apiIndex === 3 || apiIndex === 4">
          <h4>{{ data.data.title }}</h4>
          <p><strong>Sinopsis:</strong> {{ data.data.synopsis }}</p>
          <p><strong>Tipo:</strong> {{ data.data.type }}</p>
          <p><strong>Episodios/Capítulos:</strong> {{ data.data.episodes || data.data.chapters }}</p>
          <p><strong>Puntuación:</strong> {{ data.data.score }}</p>
          <a :href="data.data.url" target="_blank">Ver en MyAnimeList</a>
        </div>
      </div>
  
      <div v-if="error" class="error">
        <p>{{ error }}</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import axios from 'axios';
  
  const data = ref(null);
  const error = ref('');
  const loading = ref(false);
  const apiIndex = ref(null); 

  const urls = [
    'https://countriesnow.space/api/v0.1/countries/population',
    'https://api.jikan.moe/v4/recommendations/anime',
    'https://api.jikan.moe/v4/random/anime',
    'https://api.jikan.moe/v4/random/manga'
  ]
  
  const fetchData = async (index) => {
    data.value = null;
    error.value = '';
    loading.value = true;
    apiIndex.value = index;
  
    try {
      const response = await axios.get(urls[index - 1]);
      data.value = response.data;
    } catch (e) {
      error.value = 'Ocurrió un error al obtener los datos: ' + e.message;
    } finally {
      loading.value = false;
    }
  };
  </script>

<style scoped>

</style>
