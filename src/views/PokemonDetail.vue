<template>
  <div>
    {{ console.log('check', updatedPokemon) }}
    <h1>Pokemon Detail Page</h1>
    <div class="pokemon-details">
      <img :src="updatedPokemon?.sprites?.front_default" alt="Pokemon" />
      <h3>{{ updatedPokemon?.name }}</h3>
      <p>Type: {{ updatedPokemon?.types[0].type.name }}</p>
      <p>Weight: {{ updatedPokemon?.weight }}kg</p>
      <p>Height: {{ updatedPokemon?.height }}m</p>
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
.pokemon-details {
  color: black;
  background-color: #f0f0f0;
  margin: 1%;
  min-width: 180px;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  text-align: center;
}

img {
  max-width: 100%;
  border-radius: 4px;
  margin-bottom: 8px;
}

h3 {
  margin: 8px 0;
}

p {
  color: #888;
}
</style>
