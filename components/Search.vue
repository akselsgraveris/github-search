<template>
  <div>
    <input
      type="search"
      id="git-search"
      name="search"
      v-model="search"
      placeholder="Search GitHub user"
    >

    <button @click="$fetch">Search</button>

    <div class="history">
      <div v-for="user in searchHistory" :key="user.id">
        {{ user.login }}
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data() {
      return {
        searchResults: [],
        searchHistory: []
      }
    },

    async fetch() {
      const { data } = await axios.get(
        `https://api.github.com/search/users?q=${this.search}`
      )

      this.searchResults = data

      // Convert the object into a JSON string and store
      localStorage.setItem('searchedUsers', JSON.stringify(data.items));
      this.searchHistory = JSON.parse(localStorage.getItem('searchedUsers') || "[]")
    }
  }
</script>

<style lang="scss" scoped>
  .history {
    text-align: left;
    padding: 1rem 0;
  }
</style>
