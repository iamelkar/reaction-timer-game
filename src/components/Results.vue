<template>
  <p>Reaction Time: {{ score }} ms</p>
  <p class="rank">{{ rank }}</p>

  <div class="display">
    <h2>All Scores (Highest to Lowest)</h2>
    <ul>
        <li v-for="(storedScore, index) in sortedScores" 
        :key="index"
        :class="{'newest-score': storedScore == score}">
        {{ index + 1 }}. {{ storedScore }} ms
        </li>
    </ul>

    <button @click="resetScores" class="reset-button">Reset Scores</button>

  </div>
</template>

<script>
export default {
    props: ['score'],
    data(){
        return{
            rank: null,
            scores: []
        }
    },
    computed: {
        // Sort the scores from highest to lowest
        sortedScores() {
            return this.scores.sort((a, b) => a - b);
        }
    },
    methods:{
        loadScores(){
            const storedScores = localStorage.getItem('gameScores');
            if(storedScores){
                this.scores = JSON.parse(storedScores)
            }
        },
        saveScores(){
            localStorage.setItem('gameScores', JSON.stringify(this.scores))
        },
        resetScores() {
            this.scores = [];  // Clear the scores array
            localStorage.removeItem('gameScores');  // Remove scores from localStorage
        }
    },
    mounted(){
        this.loadScores()

        if(this.score < 250){
            this.rank = "Speed!"
        } else if(this.score < 400){
            this.rank = "Just okay"
        } else {
            this.rank = "Slow..."
        }

        this.scores.push(this.score)

        this.saveScores();
    }
}
</script>

<style scoped>
    .rank{
        font-size: 1.4em;
        color:blueviolet;
        font-weight: bold;
    }
    .display{
        width: fit-content;
        margin: 0 auto;
        padding: 0px 12px 0px 12px;
        text-align: center;
        border-radius: 4px;
        border: 5px solid black;
    }
    ul{
        list-style: none;
    }
    .newest-score {
        font-weight: bold;
        color: #e91e63; /* Bright color for emphasis */
        background-color: #fce4ec; /* Light background for visibility */
        padding: 5px;
        border-radius: 5px;
        transition: background-color 0.5s ease, color 0.5s ease;
    }
    button{
        background: red;
        color: black;
        font-weight: bold;
    }
</style>