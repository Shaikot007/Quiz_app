<template>
  <div id="app">
    <QuizHeader 
      v-bind:numCorrect="numCorrect"
      v-bind:numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
            v-if="question.length !== 0"
            v-bind:currentQuestion="question[index]"
            v-bind:next="next"
            v-bind:increament="increament"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import QuizHeader from './components/QuizHeader.vue';
import QuestionBox from './components/QuestionBox.vue';

export default {
  name: 'App',
  components: {
    QuizHeader,
    QuestionBox
  },
  data() {
    return {
      question: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increament(isCorrect) {
      if(isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method: 'get'
    })
    .then((response) => {
      return response.json();
    })
    .then((jsonData) => {
      this.question = jsonData.results
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
