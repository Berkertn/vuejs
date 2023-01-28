<template>
  <main class="container text-white"></main>
  <div class="pt-4 mb-8 px-5 relative">
    <input type="text"
     v-model="searchQuery"
      @input="getSearchResults" 
      placeholder="Search for a city or state"
      id="searchBar" class="pt-2 px-2 text-white w-full bg-transparent border-b focus:border-weather-secondary
    focus:outline-none focus:shadow-md"
    >
    <ul
    v-if="mapboxSearchResult" 
    class="absolute bg-weather-secondary text-white w-full shadow-md
    py-2 px-1 top-[66px]">
    
    <li v-for="searchResult in mapboxSearchResult" :key="searchResult.id"
    class="py-2 cursor-pointer">{{ searchResult.place_name }}</li>

    </ul>

  </div>

</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios'

const mapboxAPIKey = "pk.eyJ1IjoiYmVya2VydG4iLCJhIjoiY2xkZ2szbmR0MDF2czQwbjZla2kyd3ZhaSJ9.OoJAgW4UFHmhO3JTqBcj0g"
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResult = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value)
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/
      ${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`);
      mapboxSearchResult.value = result.data.features;
      console.log(mapboxSearchResult.value);
      return;
    };
    mapboxSearchResult.value = null;
  }, 300);
};
</script>
