<template>
  <main class="bg-dark">
    <div v-if="loading" class="loading-wrap bg-dark">
      <AppLoader />
    </div>
    <div v-else class="container movie-details">
      <div class="movie-preview">
        <img :src="movie?.Poster" :alt="movie?.Title" class="preview-image" />
        <div class="play">
          <img src="@/assets/svgs/play.svg" alt="play" />
        </div>

        <div class="button-group">
          <button class="app-button">
            <img src="@/assets/svgs/play.svg" alt="play" />
            <p>Video Stream</p>
          </button>
          <button class="app-button">
            <img src="@/assets/svgs/play.svg" alt="play" />
            <p>My cloud</p>
          </button>
          <button class="app-button">
            <img src="@/assets/svgs/play.svg" alt="play" />
            <p>Paid Cloud</p>
          </button>
        </div>
      </div>

      <div class="movie-info">
        <img :src="movie?.Poster" :alt="movie?.Title" />
        <div>
          <h1 class="text-lg text-white">{{ movie?.Title }}</h1>
          <div class="tags">
            <div class="tag">{{ movie?.imdbRating }}</div>
            <div class="tag">{{ movie?.Rated }}</div>
          </div>
          <p class="text-white text-sm">{{ movie?.Plot }}</p>
          <br />
          <div class="info">
            <p class="text-white text-xs first">Type:</p>
            <p class="text-white text-xs">{{ movie?.Type }}</p>
          </div>
          <div class="info">
            <p class="text-white text-xs first">Country:</p>
            <p class="text-white text-xs">{{ movie?.Country }}</p>
          </div>
          <div class="info">
            <p class="text-white text-xs first">Release:</p>
            <p class="text-white text-xs">{{ movie?.Released }}</p>
          </div>
          <div class="info">
            <p class="text-white text-xs first">Director:</p>
            <p class="text-white text-xs">{{ movie?.Director }}</p>
          </div>
          <div class="info">
            <p class="text-white text-xs first">Language:</p>
            <p class="text-white text-xs">{{ movie?.Language }}</p>
          </div>
          <div class="info">
            <p class="text-white text-xs first">Production:</p>
            <p class="text-white text-xs">{{ movie?.Production }}</p>
          </div>
          <div class="info">
            <p class="text-white text-xs first">Actors:</p>
            <p class="text-white text-xs">{{ movie?.Actors }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<script setup lang="ts">
import type AppLoader from '@/components/AppLoader.vue'
import { Movie } from '@/types'
import axios from 'axios'
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

const movie = ref<Movie | null>(null)

const loading = ref(false)

const route = useRoute()

const getMovie = async () => {
  try {
    loading.value = true
    const response = await axios.get(`http://www.omdbapi.com?i=${route.params.id}&apikey=9061f0dc`)

    if (response) {
      movie.value = response.data || null
    }
  } catch (error) {
    console.log(error)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  getMovie()
})
</script>
<style scoped>
@import '@/assets/css/details.css';
</style>
