<template>
  <div class="min-h-screen bg-gray-50 py-8 px-4">
    <!-- Header -->
    <h1 class="text-4xl font-bold text-center text-gray-800 mb-8">AnimeDB Search</h1>

    <!-- Search Bar -->
    <div class="flex justify-center mb-6 px-4">
      <input v-model="query" @keyup.enter="searchAnime" type="text"
        class="w-full max-w-md p-3 rounded-full border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent text-gray-800 text-lg"
        placeholder="Search for anime..." />
    </div>


    <!-- Loading Indicator -->
    <div v-if="loading" class="text-center text-blue-600">Searching...</div>

    <!-- Error Message -->
    <div v-if="error" class="text-center text-red-500 mt-4">{{ error }}</div>

    <!-- Anime Results Grid -->
    <div v-if="animeResults.length > 0" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
      <div v-for="anime in animeResults" :key="anime.mal_id"
        class="bg-white p-4 rounded-lg shadow-lg transition-transform transform hover:scale-105">
        <img :src="anime.images.jpg.image_url" alt="anime image" class="w-full h-64 object-cover rounded-lg mb-4" />
        <h2 class="text-xl font-medium text-gray-800">{{ anime.title }}</h2>
        <p class="text-sm text-gray-600 mt-2 line-clamp-3">{{ anime.synopsis }}</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";

export default defineComponent({
  name: "AnimeSearch",
  setup() {
    const query = ref<string>("");
    interface Anime {
      mal_id: number;
      images: { jpg: { image_url: string } };
      title: string;
      synopsis: string;
    }

    const animeResults = ref<Anime[]>([]);
    const loading = ref<boolean>(false);
    const error = ref<string>("");

    const searchAnime = async () => {
      if (!query.value) return;
      loading.value = true;
      error.value = "";
      try {
        const response = await axios.get(`https://api.jikan.moe/v4/anime`, {
          params: { q: query.value, limit: 6 },
        });
        animeResults.value = response.data.data;
      } catch {
        error.value = "Something went wrong!";
      } finally {
        loading.value = false;
      }
    };

    return { query, animeResults, searchAnime, loading, error };
  },
});
</script>

<style scoped>
/* Container max-width yang dinamis */
.container {
  width: 100%;
}

/* Grid Layout Responsif */
.anime-grid {
  display: grid;
  grid-template-columns: repeat(1, minmax(0, 1fr));
  /* Default 1 kolom */
  gap: 1.5rem;
}

@media (min-width: 640px) {
  .anime-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    /* 2 kolom untuk tablet */
  }
}

@media (min-width: 1024px) {
  .anime-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    /* 3 kolom untuk desktop */
  }
}

@media (min-width: 1280px) {
  .anime-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
    /* 4 kolom untuk layar besar */
  }
}

/* Mengatur tampilan teks dan mengurangi padding untuk responsivitas */
.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
