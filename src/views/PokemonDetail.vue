<template>
  <div class="detail-container">
    <div class="title-container">
      <router-link to="/">
        <svg
          class="back-icon"
          xmlns="http://www.w3.org/2000/svg"
          width="30"
          height="30"
          viewBox="0 0 24 24"
        >
          <path
            fill="currentColor"
            d="M20 11v2H8l5.5 5.5l-1.42 1.42L4.16 12l7.92-7.92L13.5 5.5L8 11h12Z"
          />
        </svg>
      </router-link>
      <h1>{{ capitalize(updatedPokemon?.name) }} Detail Page</h1>
      <span></span>
    </div>
    <div class="pokemon-details">
      <img :src="updatedPokemon?.sprites?.front_default" alt="Pokemon" class="pokemon-sprite" />
      <div class="details-container">
        {{ console.log(updatedPokemon) }}
        <div
          style="display: flex; flex-direction: column; text-align: center; margin-bottom;: 1rem"
        >
          <span style="font-size: 2rem; margin: 0">#{{ updatedPokemon?.id }}</span>
          <span style="margin: 0">{{ capitalize(updatedPokemon?.name) }}</span>
        </div>
        <div class="pokemon-info">
          <div>
            <p>
              {{ capitalize(updatedPokemon?.types[0].type.name) }} &
              {{ capitalize(updatedPokemon?.types[1].type.name) }} Pokemon
            </p>
            <p>Weight: {{ updatedPokemon?.weight }}kg</p>
            <p>Height: {{ updatedPokemon?.height }}m</p>
          </div>
          <div style="margin-left: 2%">
            <p>Abilities</p>
            <p v-for="(ability, index) in updatedPokemon?.abilities" :key="index">
              {{ index + 1 }}. {{ capitalize(ability.ability.name) }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { ref, onMounted, getCurrentInstance } from 'vue'

export default {
  props: {
    pokemon: Object
  },
  methods: {
    capitalize(text) {
      if (!text) return ''
      return text.charAt(0).toUpperCase() + text.slice(1)
    }
  },
  setup(props) {
    const updatedPokemon = ref(props.pokemon)

    const loadPokemonDetails = async (id) => {
      try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
        updatedPokemon.value = response.data
        console.log('checks', response)
      } catch (error) {
        console.error('Error loading Pokemon details:', error)
      }
    }

    onMounted(() => {
      const instance = getCurrentInstance()
      const id = instance?.appContext.config.globalProperties.$route.params.id
      loadPokemonDetails(id)
    })

    return {
      updatedPokemon: updatedPokemon
    }
  }
}
</script>

<style scoped>
.title-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 16px;
}
.detail-container {
  width: 99vw;
}

.pokemon-details {
  align-items: center;
  display: flex;
  flex-direction: column;
  color: black;
  margin: 1%;
  min-width: 180px;
  border-radius: 8px;
  padding: 16px;
}

.pokemon-info {
  display: flex;
  justify-content: space-around;
}
.pokemon-sprite {
  width: 20rem;
}

.details-container {
  padding: 2rem;
  width: 50rem;
  border: 1px solid #ccc;
  border-radius: 1em;
  background-color: #f0f0f0;
  margin: 0 2rem;
}

.back-icon {
  left: 0;
  color: white;
  cursor: pointer;
}

h1 {
  color: white;
  text-align: center;
  font-size: 3rem;
}
img {
  max-width: 100%;
  border-radius: 4px;
  margin-bottom: 8px;
}

span {
  font-size: 3rem;
  font-weight: bold;
  margin: 8px 0;
}

p {
  font-size: 20px;
  color: #888;
}
</style>
