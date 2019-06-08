<template>
    <div class="question-box-container">
      <b-jumbotron lead="Bootstrap 4 Components for Vue.js 2">
        <template slot="lead">
          <h5 v-html="currentQuestion.question">
          </h5>
        </template>

        <hr class="my-4">
        <b-list-group>
          <b-list-group-item
          v-for="(answer, index) in answers" 
          :key="index"
          @click.prevent="selectedAnswer(index)"
          :class="answerClass(index)"
          >
            {{answer}}
          </b-list-group-item>
        </b-list-group>

        <b-button 
          variant="primary" 
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
          >
          Submit
          </b-button>
        <b-button @click="next" variant="success" href="#">Next Question</b-button>
      </b-jumbotron>
    </div>
</template>


// To get the question to show up in the above template
// we need to pass it through the JS via a script
//We need to reference the props saying that these are coming from the parent 

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data: function(){
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  // Below computed method is pulling the answers from the currentQuestion object
  // To do so we are getting the array of incorrect answers and pushing the correct answer into it
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  // Watch method takes an object of functions and we can watch for changes to our props
  //Watch functions can be made into objects with some options, this way we can shuffle the first question as well 
  //the immediate function will make sure we do this handler function before it first updates
  watch: {
    currentQuestion: {
      immediate: true,
      handler(){
        this.selectedIndex = null
        this.shuffleAnswers()
        this.answered = false
      }
    }
  },
  methods: {
    selectedAnswer(index){
      this.selectedIndex = index
    },
    // We are going to use the lodash library to use a shuffle method
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      let answerClass = ''

      if (!this.answered &&  this.selectedIndex === index ) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index ) {
        answerClass = 'correct'
      } else if (this.answered &&  this.selectedIndex === index && this.correctIndex !== index){
        answerClass = "incorrect"
      }
      return answerClass
    }, 
    submitAnswer(){
      let isCorrect = false

      if (this.selectedIndex === this.correctIndex){
        isCorrect = true;
      }
      this.answered = true
      this.increment(isCorrect)
    }
  }
}
</script>

<style scoped>
  .list-group{
    margin-bottom: 15px;
  }

.list-group-item:hover{
    background: #EEE;
    cursor: pointer;
  }

  .btn{
    margin:0 5px;
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

