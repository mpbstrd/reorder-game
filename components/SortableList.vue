<template>
    <ul>
        <li v-for="(item, index) in itemLimit" 
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
    computed: {
        itemLimit() {
            const maxItems = 10;
            const arr = [];
            for (let i = 0; i < maxItems && i < this.items.length; i++) {
                arr.push(this.items[i]);
            }
            return arr;
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
  
<style>
ul {
    list-style: none;
    padding: 0;
}
li {
    padding: 10px;
    margin-bottom: 5px;
    background-color: #eee;
    cursor: pointer;
    width: 50%;
}
</style>