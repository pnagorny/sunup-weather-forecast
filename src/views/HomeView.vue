<template>
  <main class="container text-white pt-10">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchVal" @input="getSearchResults" placeholder="Search for a city" class="py-2 px-1 w-full bg-transparent border-b focus:border-secondary-color focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"/>
      <ul class="absolute bg-secondary-color text-white w-full shadow-md py-2 px-1 top-[66px]" v-if="mapBoxSearchResult">
        <p v-if="searchErr">Sorry, something went wrong.</p>
        <p v-if="!serverError && mapBoxSearchResult.length === 0">No results.</p>
        <template v-else>
          <li v-for="result in mapBoxSearchResult" :key="result.id" class="py-2 cursor-pointer"
          @click="previewCity(result)"
          >
            {{result.place_name}}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import {ref, registerRuntimeCompiler} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";


const router = useRouter();
const previewCity = (result) => {
  console.log(result);

  const [city, state] = result.place_name.split(",");
  router.push({
    name: 'cityView',
    params: {state: state.replaceAll(" ",""), city: city},
    query: {
      lat: result.geometry.coordinates[1],
      lng: result.geometry.coordinates[0],
      preview: true,
    },
  })
  
}

const mapBoxUrl ="pk.eyJ1Ijoic2lhYm9qZWsiLCJhIjoiY2xheWI3d3IyMGYzcDNwbXgxYmdjcm11dyJ9.2yGfFpv17uOlNrmKOwdaBw";
const searchVal = ref("");
const searchTimeout = ref(null);
const mapBoxSearchResult = ref(null);
const searchErr = ref(null);

const getSearchResults = () => {
  clearTimeout(searchTimeout.value)
  searchTimeout.value = setTimeout(async () => {
    if (searchVal.value !== "") {
      try {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchVal.value}.json?access_token=${mapBoxUrl}&types=place`);
        mapBoxSearchResult.value = result.data.features;
        console.log(mapBoxSearchResult.value);
      } catch {
        searchErr.value = true; 
      }
      return;
    }
    mapBoxSearchResult.value = null;
  }, 300);
}
</script>
