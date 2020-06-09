<template>
<div class = "question-box-container">
 <b-jumbotron>

    <template v-slot:lead>
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

    <b-list-group >
      <b-list-group-item v-for="(answer, index) in answers" 
         :key="index"
         @click.prevent="selectAnswer(index)"
         :class="selectedIndex === index ? 'selected' : ''">
         {{answer}}
      </b-list-group-item>
    </b-list-group>
    
    <b-button variant="primary" href="#">Submit</b-button>
    <b-button variant="success" href="#" @click="next">Next</b-button>
  
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data(){
    return{
      selectedIndex: null,
      shuffledAnswers: []
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
    currentQuestion(){
      this.selectedIndex = null;
      this.shuffleAnswers();
    }
    
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index;
      console.log(index);
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      console.log("shuffleAnswers: "+answers);
      this.shuffledAnswers = _.shuffle(answers);
     
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

