<template>
  <router-link :to="'/pokemon/' + pokemon.id">
    <div class="pokemon-card">
      <img :src="pokemon?.sprites?.front_default" alt="Pokemon" />
      <h3>{{ pokemon.name }}</h3>
      <p>Type: {{ pokemon.types[0].type.name }}</p>
      <p>Weight: {{ pokemon.weight }}kg</p>
    </div>
  </router-link>
</template>

<script>
export default {
  props: {
    pokemon: Object
  }
}
</script>

<style scoped>
.pokemon-card {
  color: black;
  background-color: aliceblue;
  margin: 1%;
  min-width: 180px;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  text-align: center;
  cursor: pointer;
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
