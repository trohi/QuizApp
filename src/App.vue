<template>
  <div id="app">
    <Header 
    :numbCorrect ="numbCorrect"
    :numbTotal ="numbTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
          v-if="questions.length"
          :currentQuestion ="questions[index]"
          :next ="next"
          :restartQuiz="restartQuiz"
          :increment ="increment"
          :totalQuestion="numbTotal"
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
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      questions:[],
      index:0,
      numbCorrect:0,
      numbTotal:0
    }
  },
  methods:{
    next(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numbCorrect++
      }
      this.numbTotal++
    },
    restartQuiz(){
      this.fetcher()
      this.index = 0
      this.numbCorrect = 0
      this.numbTotal = 0
    },
    fetcher(){
      fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple', {
      method:'get'
    })
    .then((response)=>{
      return response.json()
    })
    .then((jsonData) =>{
      this.questions = jsonData.results
    })
    }
  },
  mounted: function() {
    this.fetcher()
}
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
