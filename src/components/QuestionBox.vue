<template>
  <div class="question-box-container">
    <b-jumbotron>
<!-- any variables passed from App to comp, to display in HTML, must be refernced in props -->
      <template v-slot:lead>
        {{currentQuestion.question}} 
      </template>

      <hr class="my-4">
      
      <b-list-group>
        <b-list-group-item
          v-for ="(shuffledAnswers,index) in shuffledAnswers" 
          v-bind:key="index"
          @click="selectAnswer(index)"
          v-bind:class="answerClass(index)"
        >
            {{shuffledAnswers}}
        </b-list-group-item>
      </b-list-group>
      
      <b-button 
        variant="primary"
        v-on:click="submitAnswer"
        v-bind:disabled ="selectedIndex===null || answered"
      >
        Submit
      </b-button>
      
      <b-button @click="next" variant="success" href="#">Next Question</b-button> <!-- calling next function -->
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props:{
    currentQuestion: Object, //variable passed from App.vue
    next: Function,
    increment: Function
  },
  data(){
    return{
      selectedIndex:null,
      shuffledAnswers:[],
      answered:false,
      correctIndex:null
    }  
  },
  computed:{
    answers(){
     let answers = [...this.currentQuestion.incorrect_answers]
    answers.push(this.currentQuestion.correct_answer)
    return answers 
    }
  },
  watch:{
    currentQuestion:{
      immediate:true,
      handler(){
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  },
  methods:{
    selectAnswer(index){
      this.selectedIndex=index
    },
    submitAnswer(){
      let isCorrect = false
      if(this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered=true
      this.increment(isCorrect)
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      let answerClass=''
      
      if(!this.answered && this.selectedIndex===index){
        answerClass= 'selected'
      } else if (this.answered && this.correctIndex === index){
        answerClass= 'correct'
      } else if (this.answered && this.selectedIndex===index && this.correctIndex !== index){
        answerClass='incorrect'
      }
      return answerClass
    }
  }
}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px
}
.list-group-item:hover{
  background: #eee;
  cursor:pointer;
}
.btn{
  margin: 0 5px
}
.selected{
  background-color:lightblue
}
.correct{
  background-color:lightgreen;
}
.incorrect{
  background-color:red;
}

</style>
