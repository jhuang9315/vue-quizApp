<template>
  <div id="app">
    <Header
      v-bind:numCorrect="numCorrect"
      v-bind:numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm='6' offset ='3'>
<!-- Passing data to question box component -->
          <QuestionBox
            v-if="questions.length > 0"
            v-bind:currentQuestion='questions[index]'
            v-bind:next ="next"
            v-bind:increment ="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: { //receiving other components
    Header,
    QuestionBox
  },
  data(){
    return{
      questions: [], //declaring array question
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods:{
    next:function(){
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect){
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
        return response.json()
      })
      .then((jsonData) => {
        this.questions = jsonData.results
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
