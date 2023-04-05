<template>
  <div id="app">
    <h1>Reorder Game</h1>
    <SortableList :items="items" />
  </div>
</template>

<script>
import SortableList from '@/components/SortableList.vue';

export default {
  name: 'App',
  components: {
    SortableList
  },
  data() {
    return {
      categories: ['country_population', 'country_area',],
      items: [
        { id: 1, name: 'China', population: 1439323776 },
        { id: 2, name: 'India', population: 1380004385 },
        { id: 3, name: 'United States', population: 331002651 },
        { id: 4, name: 'Indonesia', population: 273523615 },
        { id: 5, name: 'Pakistan', population: 273523615 },
        { id: 6, name: 'Brazil', population: 	212559417 },
        { id: 7, name: 'Nigeria', population: 206139589 },
        { id: 8, name: 'Bangladesh', population: 164689383 },
        { id: 9, name: 'Russia', population: 145934462 },
        { id: 10, name: 'Mexico', population: 128932753 },
        { id: 11, name: 'Japan', population:  128932753 },
        { id: 12, name: 'Ethiopia', population: 114963588 },
        { id: 13, name: 'Philippines', population: 109581078 },
        { id: 14, name: 'Egypt', population: 102334404 },
        { id: 15, name: 'Vietnam', population: 97338579 },
        { id: 16, name: 'DR Congo', population: 89561403 },
        { id: 17, name: 'Turkey', population: 84339067 },
        { id: 18, name: 'Iran', population: 83992949 },
        { id: 19, name: 'Germany', population: 83783942 },
        { id: 20, name: 'Thailand', population: 69799978 },
        { id: 21, name: 'United Kingdom', population: 69799978 },
        { id: 22, name: 'France', population: 65273511 },
        { id: 23, name: 'Italy', population: 60461826 },
        { id: 24, name: 'Tanzania', population: 59734218 },
        { id: 25, name: 'South Africa', population: 59308690 },
      ],
      country_area: [
        { id: 1, name: 'China', area: 9640821 },
        { id: 2, name: 'India', area: 3287590 },
        { id: 3, name: 'United States', area: 9826675 },
        { id: 4, name: 'Indonesia', area: 1904569 },
        { id: 5, name: 'Pakistan', area: 803940 },
        { id: 6, name: 'Brazil', area: 8515767 },
        { id: 7, name: 'Nigeria', area: 923768 },
        { id: 8, name: 'Bangladesh', area: 147570 },
        { id: 9, name: 'Russia', area: 17098242 },
        { id: 10, name: 'Mexico', area: 1964375 },
      ],
    }
  }
}
</script>