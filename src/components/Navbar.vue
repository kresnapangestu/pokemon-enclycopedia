<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="nav-bar" :class="{ sticky: isSticky }">
    <div class="logo-container">
      <img alt="Pokemon logo" class="logo" src="@/assets/Frame 9.png" width="40" height="40" />
      <span>Pokemon Encyclopedia</span>
    </div>
    <SearchBar @search="handleSearch" />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, defineEmits } from 'vue'
import SearchBar from './SearchBar.vue'

const isSticky = ref(false)

const emit = defineEmits(['search-pokemon'])

const handleScroll = () => {
  isSticky.value = window.scrollY > 0
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

const handleSearch = (searchTerm) => {
  console.log('Search term:', searchTerm)
  emit('search-pokemon', searchTerm)
}
</script>

<style scoped>
span {
  font-weight: 900;
  font-size: 2rem;
  position: relative;
}

h3 {
  font-size: 1.2rem;
}

.nav-bar {
  display: flex;
  padding: 1%;
  text-align: center;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  transition: background-color 0.3s ease;

  color: white;
}
.logo-container {
  padding: 0 1%;
}

.sticky {
  position: fixed;
  top: 0;
  width: 100%;
  background-color: white; /* Change the background color as needed */
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1); /* Optional: Add a box shadow for a subtle effect */
  color: black;
}

.nav-bar h1,
.nav-bar h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .nav-bar h3 {
    text-align: left;
  }
}
</style>
