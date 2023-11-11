<template>
  <router-view />
</template>

<script>
export default {
  name: 'App',
  components: {}
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

.navbar {
  display: flex;
  position: absolute;
  top: 0;
  width: 100%;
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
</style>
