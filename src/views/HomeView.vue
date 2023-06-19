<template>
  <main>
    <section v-if="firstMovie" class="hero">
      <img class="hero-image" :src="firstMovie.Poster" :alt="firstMovie.Title" />
      <div class="container" style="position: relative">
        <div class="hero-text">
          <h1 class="text-lg font-bold text-white">{{ firstMovie.Title }}</h1>
          <br />
          <p class="text-white">
            Africa is a continent pulsating with vibrant energy and limitless possibilities, and has
            become an irresistible magnet for the curious souls of Generation Z in the past few
            years. With its untamed wilderness, vibrant cultural tapestry, and immersive
            experiences, Africa offers a one-of-a-kind journey that resonates deeply.
          </p>

          <div class="hero-buttons">
            <button class="app-button">
              <img src="@/assets/svgs/play.svg" alt="play" />
              <p>watch now</p>
            </button>
          </div>
        </div>
      </div>
    </section>

    <section class="bg-dark">
      <div class="container movies-list">
        <MovieCard v-for="movie in movies" :key="movie.imdbID" :movie="movie" />
      </div>
      <div id="trigger-loader"></div>
      <AppLoader v-if="loading" />
      <br />
      <br />
    </section>
  </main>
</template>
<script setup lang="ts">
import { onMounted, ref, computed, watch } from 'vue'
import { Movie } from '@/types'
import axios from 'axios'
import { useRoute } from 'vue-router'
import MovieCard from '@/components/MovieCard.vue'
import AppLoader from '@/components/AppLoader.vue'

const route = useRoute()
const page = ref(1)

const loading = ref(true)
const stopFetching = ref(false)

const movies = ref<Movie[]>([])

const firstMovie = computed(() => movies.value[0] || null)

const isInViewport = (el) => {
  const rect = el.getBoundingClientRect()
  return (
    rect.top >= 0 &&
    rect.left >= 0 &&
    rect.bottom <= (window.innerHeight || document.documentElement.clientHeight) &&
    rect.right <= (window.innerWidth || document.documentElement.clientWidth)
  )
}

const getMovies = async () => {
  try {
    loading.value = true
    const searchValue = route.query.search || 'all'
    const response = await axios.get(
      `http://www.omdbapi.com?s=${searchValue}&page=${page.value}&apikey=9061f0dc`
    )

    if (response) {
      if (!response.data.Search) {
        stopFetching.value = true
        return;
      }
      if(page.value === 1) {
        movies.value = response.data.Search || [];
        return;
      }
      movies.value = [...movies.value, ...response.data.Search]
    }
  } catch (error) {
    console.log(error)
  } finally {
    loading.value = false;
  }
}

onMounted(() => {
  window.addEventListener('scroll', (e) => {
    const element = document.getElementById('trigger-loader')
    if (isInViewport(element) && movies.value.length && !loading.value && !stopFetching.value) {
      page.value++
      getMovies()
    }
  })
  getMovies()
})

watch(route, (newRoute) => {
  if (newRoute) {
    page.value = 1;
    getMovies()
  }
})
</script>
<style scoped>
@import '@/assets/css/home.css';
</style>
