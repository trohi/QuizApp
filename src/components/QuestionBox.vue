<template>
    <div class="question-box-container">
  <b-jumbotron>

    <template slot="lead">
      {{ currentQuestion.question }}
    </template>

    <hr class="my-4">

    <b-list-group>
      <b-list-group-item
      v-for="(answer, index) in answers" 
      :key="index"
      @click="selectedAsnwer(index)"
      :class="answerClass(index)"
      >
      {{ answer }}</b-list-group-item>
    </b-list-group>

    <div>
    <b-button
      v-if="totalQuestion < 10" 
      variant="primary"
      @click="submitAnswer"
      :disabled="selectedIndex === null || answered"
    >
      Submit
      </b-button>
    <b-button 
    v-if="totalQuestion < 10" 
    @click.prevent="next" 
    variant="success" 
    :disabled="answered === false"
    >Next</b-button>

    <b-button variant="success" v-if="totalQuestion === 10" @click="restartQuiz">Retake quiz</b-button>
  </div>

  </b-jumbotron>
</div>
</template>

<script>
import _ from "lodash"

export default {
  props:{
    currentQuestion: Object,
    next: Function,
    increment: Function,
    totalQuestion: Number,
    restartQuiz: Function
  },
  data(){
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed:{
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      console.log(this.currentQuestion.correct_answer)
      console.log(answers)
      answers = _.shuffle([...answers])
      console.log(answers)
      return answers
    }
  },
  watch:{
    currentQuestion:{
      immediate: true,
      handler(){
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    } 
  },
  methods:{
    selectedAsnwer(index){
      this.selectedIndex = index
    },
    submitAnswer(){
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex){
        isCorrect = true
      }
      this.answered = true

      this.increment(isCorrect)
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers) 
      console.log(this.shuffledAnswers)
      this.correctIndex = this.answers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      let answerClass = ""
      if( !this.answered && this.selectedIndex === index){
        answerClass = 'selected'
      } else if( this.answered && this.correctIndex === index){
        answerClass = 'correct'
      } else if(
        this.answered && this.selectedIndex === index && 
        this.correctIndex !== index
        ){
        answerClass = ' incorrect'
      }

      return answerClass
    }
  }
}
</script>

<style scoped>
.list-group{
  margin-bottom: 15px
}

.list-group-item:hover{
  background-color: #EEE;
  cursor: pointer;
}

.btn{
  margin: 0 5px;
}

.selected{
  background-color: aqua;
}

.correct{
  background-color: green;
}

.incorrect{
  background-color: red;
}
</style>
