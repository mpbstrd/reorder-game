<template>
    <div>
        <h1>Reorder Game</h1>
        <div v-show="playButton" >
            <button @click="showCategory = true; playButton = false;">Play</button>
        </div>
        
        <div v-show="usernameInput">
            <input type="text" v-model="username" />
        </div>

        <div v-show="showCategory">
            <button @click=" selectCategory(0); showDifficulty = true; showCategory = false;" >Geography</button><br>
            <button @click=" selectCategory(2); showDifficulty = true; showCategory = false;">History</button>
        </div>

        <div v-show="showDifficulty">
            <p>You have selected {{categoryText}}. Choose a difficulty.</p>
            <button @click=" selectDifficulty(0); randomizeList(); showGame = true; showDifficulty = false">Easy</button><br>
            <button @click=" selectDifficulty(1); randomizeList(); showGame = true; showDifficulty = false">Intermediate</button><br>
            <button @click=" selectDifficulty(2); randomizeList(); showGame = true; showDifficulty = false">Difficult</button>
        </div>

        <div v-show="showGame">
            <!-- 
                game start
                if correct proceed to next game
                then increase score
                if wrong show correct answer
                then proceed to next game
                if max games reached show score
             -->


            <p>You have selected {{difficultyText}}. Goodluck!</p>
            <button @click="playButton = true; showGame = false"> Quit </button>
            <div class="Instruction">
                <p>Drag the countries in order of population descending (from highest population to lowest)</p>
            </div>
            
            <div v-show="gameDraggable">
                <div>
                    <p>Question {{ gameCount }}/{{ maxGames }}</p>
                </div>
                <p>Stars: {{ stars }}</p>
                <ul>
                <li v-for="(item, index) in random_items" 
                    :key="item.id" 
                    :draggable="true" 
                    @dragstart="dragStart(index)" 
                    @dragover.prevent 
                    @drop="drop(index)">
                    {{ item.id }}
                </li>
                </ul>

                <div v-show="displayCorrectAnswer">
                    <!-- modal to -->
                    <p>Correct Answer is: {{ getCorrectOrder() }} </p>
                </div>
                
                <button @click="checkListIfCorrect();">Submit</button>
            </div>

            <div v-show="showResults">

            </div>

        </div>

    </div>
</template>
  
<script>
export default {
    data() {
        return {
            draggedItemIndex: null,
            playButton: true,
            showCategory: false,
            showDifficulty: false,
            showGame: false,
            displayCorrectAnswer: false,
            gameDraggable: true,

            difficultyText: '',
            categoryText: '',

            category: 0,
            stars: 0,
            repeatCount: 1,
            maxGames: 5,
            gameCount: 0,
            
            random_items: [],
            categorySelection: [
                {
                    id: 0,
                    name: 'Country Population',
                    tag: 'Geography',
                    data: [
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
                    ]
                },
                {
                    id: 1,
                    name: 'Country Area',
                    tag: 'Geography',
                    data: [
                        { id: 1, name: 'CA China', area: 9640821 },
                        { id: 2, name: 'India', area: 3287590 },
                        { id: 3, name: 'United States', area: 9826675 },
                        { id: 4, name: 'Indonesia', area: 1904569 },
                        { id: 5, name: 'Pakistan', area: 803940 },
                        { id: 6, name: 'Brazil', area: 8515767 },
                        { id: 7, name: 'Nigeria', area: 923768 },
                        { id: 8, name: 'Bangladesh', area: 147570 },
                        { id: 9, name: 'Russia', area: 17098242 },
                        { id: 10, name: 'Mexico', area: 1964375 },
                    ]
                },
                {
                    id: 2,
                    name: 'World War I Timeline',
                    tag: 'History',
                    data: [
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
                        { id: 8, name: 'German submarine sinks the passenger liner Lusitania during crossing from New York to Liverpool, \
                                        England, killing 128 Americans.', date: 'May 7, 1915'},
                        { id: 9, name: 'Italy declares war on Austria-Hungary.', date: 'August 4, 1914'},
                    ]
                }
            ],
        };
    },
    computed: {
        gameCount() {
            const maxGames = 5;
            for (let i = 0; i < maxGames; i++) {
                return i;
            }
        },
    },
    methods: {
        dragStart(index) {
            this.draggedItemIndex = index;
        },
        drop(index) {
            const draggedItem = this.random_items.splice(this.draggedItemIndex, 1)[0];
            this.random_items.splice(index, 0, draggedItem);
        },
        selectDifficulty(difficultyIndex) {
            switch(difficultyIndex) {
                case 0:
                    this.difficulty = this.easy;
                    this.difficultyText = 'Easy';
                    break;
                case 1:
                    this.difficulty = this.intermediate;
                    this.difficultyText = 'Intermediate';
                    break;
                case 2:
                    this.difficulty = this.difficult;
                    this.difficultyText = 'Difficult';
                    break;
            }
        },
        selectCategory(categoryIndex) {
            switch(categoryIndex) {
                case 0:
                    this.category = this.categorySelection[0].data;
                    this.categoryText = this.categorySelection[0].name;
                    break;
                case 1:
                    this.category = this.categorySelection[1].data;
                    this.categoryText = this.categorySelection[1].name;
                    break;
                case 2:
                    this.category = this.categorySelection[2].data;
                    this.categoryText = this.categorySelection[2].name;
                    break;
            }
        },
        randomizeList() {
            const maxItems = 5;
            const list = [];
            const usedIndexes = new Set(); // Keep track of used indexes
            while (list.length < maxItems && usedIndexes.size < this.category.length) {
                const randomIndex = Math.floor(Math.random() * this.category.length);
                if (!usedIndexes.has(randomIndex)) { // Check if index has already been used
                    list.push(this.category[randomIndex]);
                    usedIndexes.add(randomIndex);
                }
            }
            this.random_items = list;
        },
        getCurrentOrder() {
            const currentOrder = [];
            for (let i = 0; i < this.random_items.length; i++) {
                currentOrder.push(this.random_items[i].id);
            }
            return currentOrder;
        },
        getCorrectOrder() {
            const sortedList = this.random_items.slice().sort((a, b) => a.id - b.id);
            return sortedList.map(item => item.id);
        },
        checkListIfCorrect() {
            const currentOrder = this.getCurrentOrder();
            const correctOrder = this.getCorrectOrder();
            if (JSON.stringify(currentOrder) === JSON.stringify(correctOrder)) {
                this.stars+=1;
                this.gameCount += 1;
                this.displayCorrectAnswer = false;
                this.randomizeList();
            } else {
                // display correct order
                this.gameCount += 1;
                this.displayCorrectAnswer = true;
                this.randomizeList();
            }
        },
        // if gamecount is 5, then display show results

    },
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