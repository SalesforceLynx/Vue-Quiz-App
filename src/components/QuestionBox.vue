<template>
<div class = "question-box-container">
 <b-jumbotron>

    <template v-slot:lead>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

    <b-list-group >
      <b-list-group-item v-for="(answer, index) in shuffledAnswers" 
         :key="index"
         @click.prevent="selectAnswer(index)"
         :class="answerClass(index)">
         {{answer}}
      </b-list-group-item>
    </b-list-group>
    
    <b-button 
    variant="primary"
    @click="submitAnswer"
    :disabled="selectedIndex === null || answered" >Submit
    </b-button>
    
    <b-button 
    variant="success" 
    @click="next">Next
    </b-button>
  
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },

  data(){
    return{
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },
  computed: {
    answers(){
      console.log(...this.currentQuestion.incorrect_answers);
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push[this.currentQuestion.correct_answer];
      console.log("answers: "+answers);
      return answers;
    }
  },
  watch: {
     currentQuestion: {
      immediate: true,
      handler(){
      this.selectedIndex = null;
      this.answered = false;
      this.shuffleAnswers();
      console.log(this.currentQuestion);
      
      }
    }
    
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index;
      console.log(index);
    },

    submitAnswer(){
      let isCorrect = false;
      if(this.selectedIndex === this.correctIndex){
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
            
    },

    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      console.log("shuffleAnswers: "+answers);
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
     
    },

    answerClass(index){
      let answerClass = '';
      
      if(!this.answered && this.selectedIndex === index){
        answerClass = 'selected';
      }else if(this.answered && this.correctIndex === index ){
        answerClass = 'correct';
      }else if(this.answered && this.selectedIndex === index && this.correctIndex !== index ){
        answerClass = 'incorrect';
      }
    
      return answerClass;     
    }

  },
  mounted(){
    this.shuffleAnswers();
  }

  
}
</script>

<style>
.list-group{
  margin-bottom:15px;
}

.list-group-item:hover{
  background: #eee;
  cursor: pointer;
}

.btn{
  margin: 0 5px;
}

.selected{
  background-color:lightskyblue;
  color:ivory;
}

.correct{
  background-color:limegreen;
}

.incorrect{
  background-color: crimson;
}
</style>

