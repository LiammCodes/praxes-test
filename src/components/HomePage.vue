<template>
  <h1 class="flex justify-center text-xl text-gray-400 font-bold pb-10">Vehicle Data</h1>
  <div class="flex-col justify-center mx-6 sm:mx-10 md:mx-22 lg:mx-44 xl:mx-60">
    <!-- Display brands to sort by -->
    <div class="flex justify-center pb-3 space-x-2 text-gray-400">
      <div v-for="(brand, index) in brandnames" :key="index" class="w-full">
        <button @click="handleSort(brand)" 
          v-bind:class="{'w-full bg-gray-800 py-1 rounded-lg border border-gray-900 hover:bg-gray-900 hover:border-gray-700}': !sortByBrands.includes(brand),
          'w-full bg-gray-900 py-1 rounded-lg border border-gray-900 hover:bg-gray-900 hover:border-gray-700}': sortByBrands.includes(brand)}">
            {{ brand }}
        </button>
      </div>
    </div>

    <!-- Table displaying cars -->
    <div class="overflow-x-auto relative rounded-lg border border-gray-900">
      <table class="w-full text-sm text-left text-gray-300">
        <thead class="text-sm text-gray-500 bg-gray-900">
          <tr>
            <th scope="col" class="py-3 px-6">
              Brand
            </th>
            <th scope="col" class="py-3 px-6">
              Model
            </th>
          </tr>
        </thead>
        <tbody v-for="(car, index) in cars" :key="index">
          <tr v-if="sortByBrands.includes(car.brand) || sortByBrands.length == 0" class="border-b border-gray-900 bg-gray-800">
            <th scope="row" class="py-4 px-6 font-medium text-gray-300 whitespace-nowrap">
              {{ car.brand }}
            </th>
            <td class="py-4 px-6">
              {{ car.model }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  import axios from "axios"
  export default {
    name: 'HomePage',
    mounted: function() {
      axios.get("https://praxesdemo-default-rtdb.firebaseio.com/brands.json")
        .then(response => this.brandnames = response.data.slice(1))
      axios.get("https://praxesdemo-default-rtdb.firebaseio.com/cars.json")
        .then(response => {
          this.cars = response.data.slice(1) // remove first null element
          console.log(response.data)
        })
    },
    data() {
      return {
        brandnames: [],
        cars: [],
        sortByBrands: []
      }
    },
    methods: {
      handleSort(brand) {
        if (this.sortByBrands.includes(brand)){
          for( var i = 0; i < this.sortByBrands.length; i++){ 
            if ( this.sortByBrands[i] == brand) { 
                this.sortByBrands.splice(i, 1);
            }
          }
          console.log(this.sortByBrands)
        } else {
          this.sortByBrands.push(brand)
          console.log(this.sortByBrands)
        }
      }
    }
  }
</script>
