<template>
  <div class="search-bar">
    <input
      type="text"
      placeholder="Search Pokemon here"
      v-model="searchTerm"
      @input="handleInput"
      @keyup.enter="search"
    />
    <button @click="search" class="search-button">
      <svg xmlns="http://www.w3.org/2000/svg" width="19" height="19" viewBox="0 0 24 24">
        <path
          fill="currentColor"
          d="M9.5 3A6.5 6.5 0 0 1 16 9.5c0 1.61-.59 3.09-1.56 4.23l.27.27h.79l5 5l-1.5 1.5l-5-5v-.79l-.27-.27A6.516 6.516 0 0 1 9.5 16A6.5 6.5 0 0 1 3 9.5A6.5 6.5 0 0 1 9.5 3m0 2C7 5 5 7 5 9.5S7 14 9.5 14S14 12 14 9.5S12 5 9.5 5Z"
        />
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: ''
    }
  },
  methods: {
    handleInput() {},
    search() {
      this.$emit('search', this.searchTerm)
    }
  }
}
</script>

<style scoped>
.search-bar {
  place-self: center;
  display: flex;
  align-items: center;
  margin: 16px 0;
}

input {
  padding: 8px;
  margin-right: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.search-button {
  display: flex;
  padding: 8px 16px;
  background-color: #ff3333;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.search-button:hover {
  background-color: #cc0000;
}

.search-button:active {
  background-color: #990000;
}
</style>
