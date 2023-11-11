<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <router-link :to="'/pokemon/' + pokemon.id">
    <div class="pokemon-card">
      <div class="sprite-container">
        <img :src="pokemon?.sprites?.front_default" alt="Pokemon" />
      </div>
      <div class="description-container">
        <h3>{{ capitalize(pokemon.name) }}</h3>
        <p>Type: {{ capitalize(pokemon.types[0].type.name) }}</p>
        <p>Weight: {{ pokemon.weight }}kg</p>
      </div>
    </div>
  </router-link>
</template>

<script>
export default {
  props: {
    pokemon: Object
  },
  methods: {
    capitalize(text) {
      if (!text) return ''
      return text.charAt(0).toUpperCase() + text.slice(1)
    }
  }
}
</script>

<style scoped>
.pokemon-card {
  position: relative;
  color: black;
  background-color: aliceblue;
  margin: 1%;
  min-width: 180px;
  border: 1px solid #ccc;
  border-radius: 1%;
  text-align: center;
  cursor: pointer;
}

.sprite-container {
  padding: 16px;
  background-color: black;
}

.description-container {
  padding: 16px;
}

img {
  max-width: 100%;
  border-radius: 4px;
  margin-bottom: 8px;
}

h2 {
  margin: 8px 0;
}

p {
  color: #888;
}
</style>
