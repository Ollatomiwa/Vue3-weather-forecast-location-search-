<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const getSearchResults = () => {
  clearTimeout (queryTimeout.value);
  queryTimeout = setTimeout(async() => {
    if (searchQuery.value !=="") {
      try {
        const result = await axios.get(
         `http://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${openWeatherAPIKey}`

        );
        openweatherSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }
      return;
    }
    openweatherSearchResults.value = null;
  },300);
};

const router = useRouter();
const PreviewPlace =(searchResult) => {
  console.log(searchResult);
  const [city, state] = searchResult.place_name.split(",");
  router.push ({
    name: "placeView",
    params: {state: state.replaceAll(" ", ""), city:city},
    query:{
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};
const openweatherAPIKey = "176800634b23285f81c5044fa2168406";
const searchQuery = ref("");
const queryTimeout = ref(null);
const openweatherSearchResults = ref (null);
const searchError = ref(null);
</script>

<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input 
        v-model="searchQuery"
        @input="getSearchResults"
        type="text" placeholder="Search for a City or State" class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-primary focus:outline-none focus:shadow-[0px_1pxc_0_0_#004E71]"/>
        <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
          v-if="openweatherSearchResults">
          <p v-if="searchError">Sorry, please try again</p>
          <p v-if="!searchError && openweatherSearchResults.length === 0">No result, try different location</p>
          <template v-else>
            <li v-for="searchResult in openweatherSearchResults" :key="searchResult.id" class="py-2 cursor-pointer"
            @click="PreviewPlace(searchResult)"
            >
              {{ searchResult.place_name }}
            </li>
          </template>
        </ul>
    </div>
    
  </main>
</template>
