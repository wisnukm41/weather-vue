<script setup></script>

<template>
  <main class="container text-white">
    <div class="relative pt-4 mb-8">
      <input type="text" 
      placeholder="Search city or state" 
      v-model="searchQuery"
      @input="getSearchResults"
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]">
      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
      v-if="mapboxSearchResults"
      >
        <p v-if="searchError">Sorry, Something went wrong</p>
        <p v-if="!searchError && mapboxSearchResults.length === 0">No results</p>
        <template v-else>
          <li v-for="result in mapboxSearchResults" :key="result.id" class="py-2 cursor-pointer"
          @click="previewCity(result)"
          >
            {{ result.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from 'axios';
import { useRouter } from "vue-router";
import CityList from "@/components/CityList.vue";
import CityCardSkeleton from "@/components/CityCardSkeleton.vue";


const router = useRouter();
const previewCity = (results) => {
  console.log(results);
  const [city, state] = results.place_name.split(',');
  router.push({
    name: "city",
    params: {
      city,
      state : state.replaceAll(" ", '')
    },
    query : {
      lat: results.geometry.coordinates[1],
      lon: results.geometry.coordinates[0],
    }
    
  })
}

const apiKey = import.meta.env.VITE_MAPBOX_TOKEN;
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);
const searchError = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if(searchQuery.value !== ''){

      try {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${apiKey}&types=place`);
        mapboxSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }


      return;
    }
    mapboxSearchResults.value = null
  }, 300);
}

</script>