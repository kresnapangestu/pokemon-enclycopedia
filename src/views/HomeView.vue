<template>
  <main>
    <Navbar @search-pokemon="handleSearch" />
    <div class="pokemon-container">
      <Card v-for="pokemon in filteredPokemons" :key="pokemon.id" :pokemon="pokemon" />
    </div>
    <div style="text-align: center" v-if="!loading && searchedPokemons.length === 0">
      No Pokémon named: {{ searchKey }}
    </div>
  </main>
</template>

<script>
import axios from 'axios'
import Navbar from '../components/Navbar.vue'
import Card from '../components/Card.vue'

export default {
  components: {
    Navbar,
    Card
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
      return this.pokemons?.filter((pokemon) =>
        pokemon?.name?.toLowerCase().includes(this.searchTerm ? this.searchTerm.toLowerCase() : '')
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

        const pokemonDetailsPromises = response?.data?.results?.map((pokemon) =>
          axios.get(pokemon?.url).then((response) => response?.data)
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
        if (searchTerm) {
          const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${searchTerm}`)
          const pokemonDetails = [response?.data]
          this.pokemons = [...pokemonDetails]
        }
      } catch (error) {
        if (error.response && error.response.status === 404 && searchTerm) {
          this.pokemons = []
          console.error(`No Pokémon named ${searchTerm} found`)
        } else {
          this.loadPokemons()
        }
      }
    },
    handleScroll() {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 100) {
        this.loadPokemons()
      }
    },
    handleSearch(searchTerm) {
      if (searchTerm) {
        this.searchPokemon(searchTerm?.toLowerCase())
        this.searchKey = searchTerm
      } else {
        this.loadPokemons()
      }
    }
  }
}
</script>

<style>
.pokemon-container {
  padding: 0 1%;
  margin-top: 1%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
</style>
