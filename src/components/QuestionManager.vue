<template>
  <section class="section">
      <div v-if="end === false">
        <h2 class="title is-4"> {{topic.text}} Question - {{current}} </h2>
        <h3 class="subtitle is-5">Score: {{score}} / {{max}}</h3>

        <div v-if="max > 20">
          <b-button v-on:click="limitQuestions(1)" class="limitButton" type="is-warning">Limit to 20 Questions</b-button>
        </div>
        <div v-if="limited === true">
          <b-button v-on:click="limitQuestions(0)" class="limitButton" type="is-info">Return to All Questions</b-button>
        </div>

        <Question v-bind:correct="correct" v-bind:answers="multiAnswers" v-bind:question="questions[index]" v-on:next="nextQuestion" />
      </div>
      <div v-if="end === true">
        <ScoresMan v-bind:result="score" v-bind:topic="topic.text" v-bind:max="max" />
      </div>
  </section>
</template>

<script>
import Question from "./aQuestion"
import ScoresMan from "./ScoresManager"

export default {
  name: 'QuestionMan',
  props: {
        topic: {type: Object, required: true, }
    },
  watch: { 
    topic: function () {
      this.questions = require("../data/" + this.topic.value + ".json");
      this.questionsCopy = [...this.questions];
      this.resetDetails();
    }
  },
  components: {
    Question,
    ScoresMan
  },
  data(){
    return{
      //questions - appears here after creation and load
      score:0,
      index:0,
      max:0,
      current:1,
      end:false,
      showscore:false,
      multiAnswers: [],
      correct:"",
      limited:false,
      questions:[],
      questionsCopy:[]
    }
  },
  methods:{
    resetTopic: function() {
      this.questions = [...this.questionsCopy]
    },

    resetDetails: function() {
      this.score = 0;
      this.index = 0;
      this.current = 1;
      this.max = this.questions.length;
      this.end = false;
      this.limit = false;
      this.showscore = false;
      this.shuffleQuestions();
      this.generateAnswers();
    },


    //Limit questions to 20 (or return to the full list)
    limitQuestions: function(i) {
      if (i === 1 ) {
        this.questions.splice(20, this.questions.length);
        this.resetDetails();
        this.limited = true;
      }

      if (i === 0 ) {
        this.resetTopic();
        this.resetDetails();
        this.limited = false;
      }
    },

    //Call on emit from Answer - change score and set up next question
    //Or Set the end of that quiz section to launch the high score table
    nextQuestion: function (scoreChange)  {
      this.score = this.score + scoreChange;
      this.index +=1;
      this.current +=1;

      if (this.index > (this.questions.length -1)){
        this.index = 0;
        this.end = true;
        this.showscore = true;
      }
      else {
        this.generateAnswers();
      }
    },

    //Shuffle contents of loaded topic array
    shuffleQuestions: function() {
        for (let i = this.questions.length - 1; i > 0; i--) {
          let j = Math.floor(Math.random() * (i + 1));
          [this.questions[i], this.questions[j]] = [this.questions[j], this.questions[i]];
        }
    },

    generateAnswers: function() {
      //Start
      //Creates a set of possible answers for a question (including the
      //correct answer for the existing question)
      let templateAns = {"id": 0, "a":"Blank"};
      let completeAns = new Array();
      //Create new array holding only answer words
      let questionWords = new Array();
      let answerList = new Array();
      this.questions.forEach(x => questionWords.push(x.word));

      //Record the correct answer
      let correctAnswer = this.questions[this.index].word;
      this.correct = correctAnswer;

      //Remove correct answer from possible words
      questionWords.splice(this.index, 1);

      //If there are enough answers to create 3 alternatives - loop accordingly
      if(questionWords.length >= 3){
          for(let i=0; i< 3; i++) {
            let r = Math.floor(Math.random()*questionWords.length);
            answerList.push(questionWords[r]);
            questionWords.splice(r, 1);
          }
      // Otherwise use all available
      } else {
          for(let i=0; i< questionWords.length; i++) {
          let r = Math.floor(Math.random()*questionWords.length);
          answerList.push(questionWords[r]);
          questionWords.splice(r, 1);
          }
      }

      if (answerList.length > 0) {
          let r = Math.floor(Math.random()*answerList.length);
          answerList.splice(r, 0, correctAnswer);
      } else {
        answerList = [
          {"id": "1", "a":"Error 1"},
          {"id": "2", "a":"Error 2"},
          {"id": "3", "a":"Error 3"},
          {"id": "4", "a":"Error 4"}
          ];
      }

      //Create new answer array containing objects with an index and cloning the answer appropriately
      for(let i = 0; i<answerList.length; i++){
        templateAns.id = i;
        templateAns.a = answerList[i];
        let clone = {...templateAns};
        completeAns.push(clone);
      }    
      
      this.multiAnswers = completeAns;
      //End
    }
  },

  created(){
    this.questions = require("../data/" + this.topic.value + ".json");
    this.questionsCopy = [...this.questions];
    this.max = this.questions.length;
    this.end = false;
    this.showscore = false;
    this.shuffleQuestions();
    this.generateAnswers();
  }
}
</script>

<style scoped>


</style>