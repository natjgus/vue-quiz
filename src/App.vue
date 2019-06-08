<template>
  <div id="app">
    <Header 
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />
    <h1 slot="header">Wanna gain some knowledge?!</h1>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionTopics v-on:setTopic="getTopicQuestions($event)"
            
          />
          <!-- I can pass data and methods to a component using v-bind -->
          <!-- We need to wait for the api call to render this QuestionBox -->
          <!-- Using v-if and having it set to false until the questions are populated -->
          <QuestionBox 
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next" 
            :increment="increment"  
          />
        </b-col>
      </b-row>
    </b-container>  
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import QuestionTopics from './components/QuestionTopics.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    QuestionTopics
  },
  // Need to create a data function to store my api call data
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      topicOptions: [],
      selectedId: 0
    }
  },
  // We need to increment through the index with the method below 
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect){
      if (isCorrect){
        this.numCorrect++;
      }
      this.numTotal++;
    },
    getTopicQuestions: function (e) {
      this.selectedId = e;
      fetch(`https://opentdb.com/api.php?amount=10&category=${this.selectedId}&type=multiple`, {
      method: 'get'
    })
      .then((response) => {
      return response.json();
    })
      .then((jsonData) => {
      this.questions = jsonData.results
    })
    }
  },
  // updated : function(){
  //   fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
  //     method: 'get'
  //   })
  //   .then((response) => {
  //     return response.json()
  //   })
  //   .then((jsonData) => {
  //       this.questions = jsonData.results
  //   })
  // }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
