<!-- eslint-disable no-undef -->
<template>
    <div>
      <!-- search field -->
      <form>
        <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
          <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
          <input
            type="text"
            placeholder="Search for a place"
            class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
            v-model= "searchTerm.query"
            @input="handleSearch"
          />
        </div>
      </form>
      <!-- search suggestions -->
      <div class="bg-white my-2 rounded-lg shadow-lg">
        <div v-if="searchTerm.results !== null">
            <div v-for="place in searchTerm.results" :key="place">
                <button @click="getWeather(place.id)" class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
                  {{ place.name }}, {{ place.region }}, {{ place.country }}
                </button>
              </div>
        </div>
      </div>
    </div>
  </template>

<script setup>

import { reactive} from 'vue';

const emit = defineEmits(['place-data']);
        
            const searchTerm = reactive({
                query: ' ',
                timeout: null,
                results: null,
            })

            const handleSearch = () => {
                if (searchTerm.query  !== '') {
                    clearTimeout(searchTerm.timeout)
                searchTerm.timeout = setTimeout( async() => {
                    const res = await fetch (`http://api.weatherapi.com/v1/search.json?key=d3f8a07555ba4d18be044527240608&q=${searchTerm.query}`)
                    const data = await res.json()
                    searchTerm.results = data
                    console.log(searchTerm.results)
                }, 500);
                } else {
                    searchTerm.results = null
                }
                

                }
                const getWeather = async (Id) => {
                    const res = await fetch (`http://api.weatherapi.com/v1/forecast.json?key=d3f8a07555ba4d18be044527240608&q=Id:${Id}&days=3&aqi=no&alerts=no`)
                    const data = await res.json()
                    emit('place-data', data)

                    searchTerm.query = ' ',
                    searchTerm.results = null

                }

          
    
</script>

<style scoped>

</style>