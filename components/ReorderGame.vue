<template>
    <div class="container" style="text-align: center;">
        <h1>REORDER GAME</h1>
        <div v-show="playButton" >
            <button @click="showCategory = true; playButton = false;">Play</button>
        </div>

        <div v-show="showCategory">
            <button @click=" selectCategory(0); showCategory = false; showGame = true;" >Geography</button><br>
            <button @click=" selectCategory(1); showCategory = false; showGame = true;">History</button><br>
            <button @click="playButton = true; showCategory = false"> Back </button>
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

            
            <div class="Instruction">
                <p> {{ categoryText }} </p>
            </div>
            
            <div v-show="gameDraggable">
                <div>
                    <p>Question {{ gameCount+1 }}/{{ maxGames }}</p>
                </div>
                <p>Stars: {{ stars }}</p>
                <ul class="itemList">
                <li v-for="(item, index) in random_items" 
                    :key="item.id" 
                    :draggable="true" 
                    @dragstart="dragStart(index)" 
                    @dragover.prevent 
                    @drop="drop(index)">
                    {{ item.name }}
                </li>
                </ul>
                
                <button @click="submitAnswers();">Submit</button>
            </div>
            
            <div v-show="displayCorrectAnswer">
                <div class="modal">
                   <div class="modal-content">
                       <p>Correct Answer is: {{ getCorrectOrderString() }}</p>
                           <div class="modal-footer">
                               <button type="button" class="btn btn-secondary" @click="closeModal" data-dismiss="modal">Proceed</button>
                               <button type="button" class="btn btn-primary"  v-if="correctAnswer">Next</button>
                           </div>
                       </div>
                   </div>
               </div>

            <button @click="playButton = true; showGame = false; resetGame(); "> Quit </button>
        </div>

        <div v-show="showResults">
            <div>
                <p>{{ resultPrompts() }}</p>
                <ScoreStars :score="stars" />
            </div>
            <div>{{ starPrompt }}</div>
            <button @click="randomizeList(); resetGame(); showCategory = false; showGame = true;">Try again?</button>
            <button @click="randomizeList(); resetGame(); playButton = true; showResults = false; showCategory = false"> Main Menu </button>
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
            showResults: false,
            displayCorrectAnswer: false,
            gameDraggable: true,
            correctAnswer: false,

            catIndex: 0,
            promptResults: '',
            starPrompt: '',
            categoryText: '',
            globalCatString: '',

            category: 0,
            stars: 0,
            repeatCount: 1,
            maxGames: 5,
            gameCount: 0,
            
            random_items: [],
            questionData: [
                {
                    id: 0,
                    tag: 'Geography',
                    instruction: 'Arrange the countries in order of population descending (from highest population to lowest)',
                    data: [
                        { id: 1, name: 'China', info: "1,439,323,776" },
                        { id: 2, name: 'India', info: "1,380,004,385" },
                        { id: 3, name: 'United States', info: "331,002,651" },
                        { id: 4, name: 'Indonesia', info: "273,523,615" },
                        { id: 5, name: 'Brazil', info: "212,559,417" },
                    ]
                },
                {
                    id: 1,
                    tag: 'Geography',
                    instruction: 'Arrange the countries in order of area descending (from highest area to lowest).',
                    data: [
                        { id: 1, name: 'Russia', info: "17,098,242 km^2" },
                        { id: 2, name: 'Canada', info: "9,984,670 km^2" },
                        { id: 3, name: 'China', info: "9,596,960 km^2" },
                        { id: 4, name: 'India', info: "3,287,263 km^2" },
                        { id: 5, name: 'Egypt', info: "1,001,450 km^2" },
                    ]
                },
                {
                    id: 2,
                    tag: 'History',
                    instruction: 'Arrange the inventions in order of date descending (from earliest to latest).',
                    data: [
                        { id: 1, name: 'Wheel', info: '5000 BC'},
                        { id: 2, name: 'Gun Powder', info: '9th century'},
                        { id: 3, name: 'Electric Lamp', info: '1879'},
                        { id: 4, name: 'Car', info: '1886'},
                        { id: 5, name: 'Airplane', info: '1903'},
                    ]
                },
                {
                    id: 3,
                    tag: 'History',
                    instruction: 'Arrange which country that has a highest casualties in World War II.',
                    data: [
                        { id: 1, name: 'Soviet Union', info: '20 to 27 million'},
                        { id: 2, name: 'China', info: '15 to 20 million'},
                        { id: 3, name: 'Germany', info: '6 to 7.4 million'},
                        { id: 4, name: 'Poland', info: '5.9 to 6 million'},
                        { id: 5, name: 'Japan', info: '2.5 to 3.1 million'},
                    ]
                },
                {
                    id: 4,
                    tag: 'History',
                    instruction: 'Arrange who has the longest term being president of the philippines.',
                    data: [
                        { id: 1, name: 'Ferdinand Marcos', info: '20 yrs, 57 days'},
                        { id: 2, name: 'Gloria Macapagal Arroyo', info: '9 yrs, 161 days'},
                        { id: 3, name: 'Manuel L. Quezon', info: '8 yrs, 260 days'},
                        { id: 4, name: 'Corazon Aquino', info: '6 yrs, 126 days'},
                        { id: 5, name: 'Fidel V. Ramos', info: '6 yrs, 0 day'},
                    ]
                },
                {
                    id: 5,
                    tag: 'Geography',
                    instruction: 'Arrange which country has most islands to least.',
                    data: [
                        { id: 1, name: 'Sweden', info: '267,570'},
                        { id: 2, name: 'Norway', info: '239,057'},
                        { id: 3, name: 'Finland', info: '178,947'},
                        { id: 4, name: 'Canada', info: '52,455'},
                        { id: 5, name: 'Philippines', info: '7,641'},
                    ]
                },
                {
                    id: 6,
                    tag: 'Geography',
                    instruction: 'Arrange which country has highest covid 19 cases.',
                    data: [
                        { id: 1, name: 'United States', info: '106,359,724'},
                        { id: 2, name: 'India', info: '44,745,104'},
                        { id: 3, name: 'France', info: '39,827,031'},
                        { id: 4, name: 'Germany', info: '38,368,891'},
                        { id: 5, name: 'Brazil', info: '37,319,254'},
                    ]
                },
                {
                    id: 7,
                    tag: 'Geography',
                    instruction: 'Arrange which country has a lowest crime rate.',
                    data: [
                        { id: 1, name: 'Iceland', info: '1.107'},
                        { id: 2, name: 'New Zealand', info: '1.269'},
                        { id: 3, name: 'Ireland', info: '1.288'},
                        { id: 4, name: 'Denmark', info: '1.296'},
                        { id: 5, name: 'Austria', info: '1.3'},
                    ]
                },
                {
                    id: 8,
                    tag: 'Geography',
                    instruction: 'Arrange which country has a highest suicidal rates.',
                    data: [
                        { id: 1, name: 'Lesotho', info: '72.4'},
                        { id: 2, name: 'Guyana', info: '40.3'},
                        { id: 3, name: 'Eswatini', info: '29.4'},
                        { id: 4, name: 'South Korea', info: '28.6'},
                        { id: 5, name: 'Kiribati', info: '28.3'},
                    ]
                },
                {
                    id: 9,
                    tag: 'History',
                    instruction: 'Arrange which application was created first.',
                    data: [
                        { id: 1, name: 'Yahoo', info: '1994'},
                        { id: 2, name: 'Google', info: '1998'},
                        { id: 3, name: 'LinkedIn', info: '2002'},
                        { id: 4, name: 'Skype', info: '2003'},
                        { id: 5, name: 'Facebook', info: '2004'},
                    ]
                },
                {
                    id: 10,
                    tag: 'Geography',
                    instruction: 'Arrange which countries has most beauty pageants crowns.',
                    data: [
                        { id: 1, name: 'Venezuela', info: '23'},
                        { id: 2, name: 'Philippines', info: '15'},
                        { id: 3, name: 'India', info: '10'},
                        { id: 4, name: 'United Kingdom', info: '7'},
                        { id: 5, name: 'Sweden', info: '6'},
                    ]
                },
                {
                    id: 11,
                    tag: 'Geography',
                    instruction: 'Arrange the countries according to their English literacy rates.',
                    data: [
                        { id: 1, name: 'United States', info: '100%'},
                        { id: 2, name: 'Canada', info: '97%'},
                        { id: 3, name: 'Ireland', info: '91%'},
                        { id: 4, name: 'South Africa', info: '51%'},
                        { id: 5, name: 'India', info: '12%'},
                    ]
                },
                {
                    id: 12,
                    tag: 'Geography',
                    instruction: 'Arrange the countries from highest to lowest based on their olympic ranking.',
                    data: [
                        { id: 1, name: 'Great Britain', info: '883 medals'},
                        { id: 2, name: 'France', info: '731 medals'},
                        { id: 3, name: 'Italy', info: '725 medals'},
                        { id: 4, name: 'Sweden', info: '652 medals'},
                        { id: 5, name: 'Australia', info: '526 medals'},
                    ]
                },
                {
                    id: 14,
                    tag: 'Science',
                    instruction: '',
                    data: [
                        { id: 1, name: '', info: ''},
                        { id: 2, name: '', info: ''},
                        { id: 3, name: '', info: ''},
                        { id: 4, name: '', info: ''},
                        { id: 5, name: '', info: ''},
                    ]
                },
                {
                    id: 15,
                    tag: 'Science',
                    instruction: '',
                    data: [
                        { id: 1, name: '', info: ''},
                        { id: 2, name: '', info: ''},
                        { id: 3, name: '', info: ''},
                        { id: 4, name: '', info: ''},
                        { id: 5, name: '', info: ''},
                    ]
                },
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
        selectCategory(categoryIndex) {
            switch(categoryIndex) {
                case 0: // geography
                    this.globalCatString = "Geography";
                    this.categorizer("Geography");
                    this.randomizeList();
                    break;
                case 1: // history
                    this.globalCatString = "History";
                    this.categorizer("History");
                    this.randomizeList();
                    break;
                case 2:
                    this.category = this.questionData[2].data;
                    this.categoryText = this.questionData[2].name;
                    break;
            }
        },
        categorizer(categoryString){
            let geographyQuestions = this.questionData.filter(item => item.tag === categoryString);
            let geographyIndex = Math.floor(Math.random() * geographyQuestions.length);
            this.category = this.questionData.filter(item => item.tag === categoryString)[geographyIndex].data;
            this.categoryText = this.questionData.filter(item => item.tag === categoryString)[geographyIndex].instruction;
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
        getCorrectOrderString() {
            const sortedList = this.random_items.slice().sort((a, b) => a.id - b.id);
            const correctOrder = [];
            for (let i = 0; i < sortedList.length; i++) {
                correctOrder.push(sortedList[i].name);
            }   
            return correctOrder.join(', ');
        },
        getCorrectOrder() {
            const sortedList = this.random_items.slice().sort((a, b) => a.id - b.id);
            const correctOrder = [];
            for (let i = 0; i < sortedList.length; i++) {
                correctOrder.push(sortedList[i].id);
            }   
            return correctOrder;
        },

        submitAnswers() {
            if (this.gameCount == this.maxGames-1) {
                this.showGame = false;
                this.showResults = true;
            } else {
                this.checkListIfCorrect();
            }
        },
        checkListIfCorrect() {
            const currentOrder = this.getCurrentOrder();
            const correctOrder = this.getCorrectOrder();
            console.log("current: " + currentOrder);
            console.log("correct: " + correctOrder);
            if (JSON.stringify(currentOrder) === JSON.stringify(correctOrder)) {
                this.stars+=1;
                this.gameCount += 1;
                this.displayCorrectAnswer = false;
                this.categorizer(this.globalCatString);
                this.randomizeList();
            } else {
                // display correct order
                this.gameCount += 1;
                this.displayCorrectAnswer = true;
            }
        },
        resultPrompts() {
            if (this.stars == 1){
                return this.promptResults = "Good try! You got 1 star!";
            } else if (this.stars == 2) {
                return this.promptResults =  "Good job! You got 2 stars!";
            } else if (this.stars == 3) {
                return this.promptResults =  "Nice! You got 3 stars!";
            } else if (this.stars == 4) {
                return this.promptResults =  "Very good! You got 4 stars!";
            } else if (this.stars == 5) {
                return this.promptResults =  "Excellent! You got a perfect score!";
            } else {
                return this.promptResults =  "You didn't get any stars. Try again!";
            }
        },
        getGeographyData() {
            const geographyData = [];
            for (let i = 0; i < this.questionData.length; i++) {
                if (this.questionData[i].tag === 'Geography') {
                    geographyData.push(this.questionData[i].data);
                }
            }
            return geographyData;
        },
        resetGame() {
            this.showGame = false;
            this.showResults = false;
            this.stars = 0;
            this.gameCount = 0;
        },
        closeModal() {
            this.categorizer(this.globalCatString); 
            this.randomizeList(); 
            this.displayCorrectAnswer = false;
        },

    },
};
</script>
  
<style>

.container {
  background-color: transparent ;
  position: relative;
  z-index: 1;   
}

.container::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: black;
  background-image: url("@/static/bg.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  height: 100vh;
  filter: blur(30px);
  z-index: -1;
}

h1 {
    text-align: center;
    padding-top: 50px;
    color:#eee;
    font-size: 3rem;
}

p {
    font-size: 1vw;
    color: white;
}
.itemList{
    list-style: none;
    padding: 0;
    text-align: center  ;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center ;
}
.itemList Li {
    padding: 10px;
    margin-bottom: 5px;
    background: linear-gradient(to top, #354cfd,  #4c49fe, #6f6dfb);
    color: #ffffff;
    cursor: pointer;
    width: 50%;
    align-items: center;
    display: inline-block;
}

.modal-content p{
    color:black;
}

.modal-content button {
    background: transparent;
    border:none;
    color: rgb(0, 30, 255);
    cursor:pointer;
    font-size:14px;
    padding: 10px 20px;
}

.modal-content button:hover{
    background: linear-gradient(to top, #455af8,  #4542ff, #918fff);
    color:rgb(255, 255, 255);
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  color:#000000;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #fff;
  padding: 1rem;
  border-radius: 0.5rem;
}

button {
  background: linear-gradient(to top, #455af8,  #4542ff, #918fff);
  color: #ffffff;
  border: none;
  text-align: center;
  max-width: 100%;
  padding: 10px 20px;
  border-radius: 5px;   
  font-size: 16px;
  margin-bottom: 10px;
  transition: transform 0.2s ease-in-out;
}

button:hover {
  transform: scale(1.1);
}

/* Styles for screens smaller than 768px (e.g. smartphones) */
@media (max-width: 767px) {
    .container {
        width: 100%;
    }

    h1 {
        font-size: 3rem;
    }

    p {
    font-size:1rem;
    }

    button {
    font-size: 14px;
    padding: 8px 12px;
  }

  button:hover {
  transform: scale(1.1);
}
}

/* Styles for screens between 768px and 1023px (e.g. tablets) */
@media (min-width: 768px) and (max-width: 1023px) {
    .container {
        width: 80%;
    }

    h1 {
        font-size: 4rem;
    }

    p {
    font-size:1.5rem;
    }

    button {
    font-size: 16px;
    padding: 10px 14px;
  }

  button:hover {
  transform: scale(1.1);
}
}

/* Styles for screens larger than 1023px (e.g. desktops) */
@media (min-width: 1024px) {
    .container {
        width: 100%;
    }

    h1 {
        font-size: 3rem;
    }

    p {
        font-size:1rem
    }
    }

    button {
    font-size: 18px;
    padding: 12px 16px;
  }

button:hover {
  transform: scale(1.1);
}

.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}



</style>