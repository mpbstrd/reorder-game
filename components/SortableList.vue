<template>
    <div>
        <h1>Reorder Game</h1>
        <div v-show="playButton" >
            <button @click="showCategory = true; playButton = false;">Play</button>
        </div>
        <div v-show="showCategory">
            <button @click="showDifficulty = true; showCategory = false " >Country Population</button>
            <button @click="showDifficulty = true; showCategory = false">Country Area</button>
            <button @click="showDifficulty = true; showCategory = false">World War 1 Timeline</button>
        </div>
        <div v-show="showDifficulty">
            <button @click="showGame = true; showDifficulty = false">Easy</button>
            <button @click="showGame = true; showDifficulty = false">Medium</button>
            <button @click="showGame = true; showDifficulty = false">Hard</button>
        </div>
        <div v-show="showGame">
            <button @click="playButton = true; showGame = false"> Quit </button>
            <div class="Instruction">
                <p>Drag the countries in order of population descending (from highest population to lowest)</p>
            </div>
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
        </div>
    </div>
</template>
  
<script>
export default {
    computed: {
        itemLimit() {
            const maxItems = 10;
            const arr = [];
            for (let i = 0; i < maxItems && i < this.country_population.length; i++) {
                arr.push(this.country_population[i]);
            }
            return arr;
        },
    },
    methods: {
        dragStart(index) {
            this.draggedItemIndex = index;
        },
        drop(index) {
            const draggedItem = this.country_population.splice(this.draggedItemIndex, 1)[0];
            this.country_population.splice(index, 0, draggedItem);
        }
    },
    data() {
        return {
            draggedItemIndex: null,
            playButton: true,
            showCategory: false,
            showDifficulty: false,
            showGame: false,


            categories: ['country_population', 'country_area', 'world_war_1_timeline'],
            country_population: [
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
            world_war_1_timeline: [
                { id: 1, name: 'Archduke Francis Ferdinand is assassinated.', date: 'June 28, 1914'},
                { id: 2, name: 'Austria-Hungary declares war on Serbia, beginning World War I.', date: 'July 28, 1914'},
                { id: 3, name: 'Germany invades Luxembourg and Belgium. France invades Alsace. British forces arrive in France. \
                            Nations allied against Germany were eventually to include Great Britain, Russia, Italy, Australia, \
                                New Zealand, South Africa, Rhodesia, Romania, Greece, France, Belgium, United States, Canada, Serbia, \
                                India, Portugal, Montenegro, and Poland.', date: 'August 2-7, 1914'},
                { id: 4, name: 'Austria-Hungary invades Russia.', date: 'August 10, 1914'},
                { id: 5, name: 'Allied forces halt German advance into France during First Battle of the Marne.', date: 'September 9, 1914'},
                { id: 6, name: 'Germany begins naval blockade of Great Britain.', date: 'February 18, 1915'},
                { id: 7, name: 'Germany begins naval blockade of Great Britain.', date: 'April 25, 1915'},
                { id: 8, name: 'German submarine sinks the passenger liner Lusitania during crossing from New York to Liverpool, England, killing 128 Americans.', date: 'May 7, 1915'},
                { id: 9, name: 'Italy declares war on Austria-Hungary.', date: 'August 4, 1914'},
            ],
            random_items: [],

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