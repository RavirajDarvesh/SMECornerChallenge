<template>
  <div id="app">
    <PostsTable v-if="items.length>0" :items="items"/>
  </div>
</template>

<script>
import axios from "axios";
import PostsTable from './components/PostsTable.vue';

export default {
  name: 'App',
  components: {
    PostsTable
  },
  data() {
    return {
      items : []
    }
  },
  async mounted() {
    try {
      const response = await axios.get(
        `https://jsonplaceholder.typicode.com/comments`
      );
      this.items = response.data;
    } catch (error) {
      this.items = [];
    }
  },
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
