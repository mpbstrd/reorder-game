<template>
    <ul>
        <li v-for="(item, index) in items" 
        :key="item.id" 
        :draggable="true" 
        @dragstart="dragStart(index)" 
        @dragover.prevent @drop="drop(index)"
        >
        {{ item.name }}
        </li>
    </ul>
</template>
  
<script>
export default {
props: {
    items: {
    type: Array,
    required: true
    }
},
methods: {
    dragStart(index) {
    this.draggedItemIndex = index;
    },
    drop(index) {
    const draggedItem = this.items.splice(this.draggedItemIndex, 1)[0];
    this.items.splice(index, 0, draggedItem);
    }
},
data() {
    return {
    draggedItemIndex: null
    };
}
};
</script>
  