<template>
  <header class="header-wrap bg-dark">
    <div class="container header">
      <RouterLink to="/" class="logo bg-primiary desk" style="text-decoration: none;">
        <h1 class="text-white">C-MOVIES</h1>
      </RouterLink>
      <RouterLink to="/" class="logo bg-primiary mob" style="text-decoration: none;">
        <h1 class="text-white">C</h1>
      </RouterLink>
      <form @submit.prevent="setSearchValue" class="search">
        <img src="@/assets/svgs/search.svg" alt="search" />
        <input
          class="search-input bg-dark-light text-white"
          placeholder="Search movies..."
          v-model="searchKey"
        />
      </form>
      <div class="filters">
        <div class="filter bg-dark-light">
          <p class="text-white sort-by">Rating</p>
        </div>
        <div class="filter bg-dark-light">
          <p class="text-white sort-by">Release date</p>
        </div>
      </div>
    </div>
  </header>
</template>
<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const searchKey = ref('')

const setSearchValue = () => {
  router.push({ query: { search: searchKey.value } })
}
</script>
<style scoped>
@import '../assets/css/header.css';
</style>
