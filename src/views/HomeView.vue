<template>
  <main>
    <Navbar @search-pokemon="handleSearch" />
    <div class="pokemon-container">
      <ImageContainer v-for="pokemon in filteredPokemons" :key="pokemon.id" :pokemon="pokemon" />
    </div>
    {{ console.log('check', searchedPokemons, searchKey) }}
    <div v-if="!loading && searchedPokemons.length === 0">No Pokémon named: {{ searchKey }}</div>
  </main>
</template>

<script>
import axios from 'axios'
import Navbar from '../components/Navbar.vue'
import ImageContainer from '../components/imageContainer/ImageContainer.vue'

export default {
  components: {
    Navbar,
    ImageContainer
  },
  data() {
    return {
      pokemons: [],
      loading: false,
      page: 1,
      searchTerm: '',
      searchKey: ''
    }
  },
  computed: {
    filteredPokemons() {
      return this.searchTerm ? this.searchedPokemons : this.allPokemons
    },
    allPokemons() {
      return this.pokemons
    },
    searchedPokemons() {
      return this.pokemons.filter((pokemon) =>
        pokemon.name.toLowerCase().includes(this.searchTerm.toLowerCase())
      )
    }
  },
  mounted() {
    this.loadPokemons()
    window.addEventListener('scroll', this.handleScroll)
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    async loadPokemons() {
      try {
        this.loading = true
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=36`)

        const pokemonDetailsPromises = response.data.results.map((pokemon) =>
          axios.get(pokemon.url).then((response) => response.data)
        )
        const pokemonDetails = await Promise.all(pokemonDetailsPromises)
        this.pokemons = [...this.pokemons, ...pokemonDetails]
        this.page++
      } catch (error) {
        console.error('Error loading pokemons:', error)
      } finally {
        this.loading = false
      }
    },
    async searchPokemon(searchTerm) {
      try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${searchTerm}`)
        const pokemonDetails = [response.data]
        this.pokemons = [...pokemonDetails]
      } catch (error) {
        if (error.response && error.response.status === 404) {
          this.pokemons = []
          console.log(`No Pokémon named ${searchTerm} found`)
        }
      }
    },
    handleScroll() {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 100) {
        this.loadPokemons()
      }
    },
    handleSearch(searchTerm) {
      console.log('Called handleSearch in HomeView:', searchTerm)
      this.searchPokemon(searchTerm)
      this.searchKey = searchTerm
    }
  }
}
</script>

<style>
#app {
  text-align: center;
  flex-direction: column;
  display: flex;
  width: 100vw;
}

body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  width: 100vw;
}

.content {
  position: relative;
}

.navbar {
  display: flex;
  position: absolute;
  top: 0;
  width: 100%;
  padding: 10px;
}

.navbar ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
}

.navbar li {
  margin: 0 15px;
}

.navbar a {
  text-decoration: none;
  color: white;
  font-weight: bold;
}

.pokemon-container {
  padding: 0 1%;
  margin-top: 1%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

.image-container {
  max-width: 300px;
  margin: 16px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.image-container:hover {
  transform: scale(1.05);
}

.image-container img {
  width: 100%;
  height: auto;
  border-bottom: 1px solid #ccc;
}

.image-container h2 {
  padding: 8px;
  font-size: 1.2rem;
  margin: 0;
}
</style>
