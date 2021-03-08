<template>
    <div class="question-box-container">
        <div>
        <b-jumbotron>

            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group >
                <b-list-group-item
                v-for="(answer, index) in shuffledAnswers" 
                v-bind:key="index"
                @click="selectAnswer(index)"
                :class="answerClass(index)">

                {{ answer }}

                </b-list-group-item>
            </b-list-group>

            
            <b-button variant="primary"
            @click="submitAnswer"
            :disabled="selectedIndex === null || answered">
                Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next


            </b-button>
        </b-jumbotron>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: { //lo que le pasan
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() { //las variables
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered: false,
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers] //Copy the array
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch: {  // Whenever something changes in the data this is going to be called
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
        // currentQuestion() {
        //     this.selectedIndex = null
        //     this.shuffleAnswers()
        // }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
            console.log(index)
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false
            if (this.selectedIndex === this.correctIndex ) {
                isCorrect = true
            }

            this.increment(isCorrect)
            this.answered = true
        },
        answerClass(index){
            let answerClass = ''
            if (!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if(this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                answerClass = 'incorrect'
            }
            return answerClass
        }
    },
    mounted(){
        console.log(this.currentQuestion)
    }
    
}
</script>

<style scoped>
.list-group {
    margin-bottom: 15px;
}
.list-group-item:hover {
    background-color: #EEE;
    cursor: pointer;

}
.btn {
    margin: 0 5px;
}

.selected {
    background-color: lightblue;
}

.correct {
    background-color: lightgreen;
}

.incorrect {
    background-color: red;
}

</style>