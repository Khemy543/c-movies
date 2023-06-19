<template>
  <div class="movie">
    <div class="movie-img">
      <img :src="movie.Poster" :alt="movie.Title" />
      <p class="review">4.5</p>
    </div>
    <h2 class="title text-white text-md">{{ movie.Title }}</h2>
    <div class="info">
      <p class="text-light">{{ movie.Year }}</p>
      <div class="tag">
        <p class="text-light text-xs">{{ movie.Type }}</p>
      </div>
      <p class="text-light">93 min</p>
    </div>
    <RouterLink :to="`/movie/${movie.imdbID}`" style="text-decoration: none;">
      <button class="app-button button-light full">Get More Info</button>
    </RouterLink>
  </div>
</template>
<script setup lang="ts">
import { RouterLink } from 'vue-router'

defineProps({
  movie: {
    type: Object,
    default: () => ({})
  }
})
</script>
<style scoped>
@import '@/assets/css/movie-card.css';
</style>
