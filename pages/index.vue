<template>
  <div id="app">
    <h1>Sortable List</h1>
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
      items: [
        { id: 1, name: 'Item 1' },
        { id: 2, name: 'Item 2' },
        { id: 3, name: 'Item 3' },
        { id: 4, name: 'Item 4' },
        { id: 5, name: 'Item 5' },
      ]
    }
  }
}
</script>
