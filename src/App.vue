<template>

  <div id="app" class="min-h-screen bg-gray-50 flex justify-center items-center">
    <AnimeSearch />
    <Analytics />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import AnimeSearch from './components/AnimeSearch.vue';

export default defineComponent({
  name: 'App',
  components: {
    AnimeSearch
  }
});
</script>

<style>
/* Global Styles */
body {
  font-family: 'Arial', sans-serif;
  background-color: #f3f4f6;
  margin: 0;
  padding: 0;
  width: 100%;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  width: 100%;
  padding: 20px;
  /* Add padding for smaller screens */
}

/* Media Queries for Responsiveness */
@media (max-width: 768px) {
  #app {
    flex-direction: column;
    padding: 10px;
  }
}

@media (max-width: 480px) {
  #app {
    padding: 5px;
  }
}
</style>
