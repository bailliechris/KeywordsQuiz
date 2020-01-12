<template>
  <section class="section">
      <div v-if="end === false">
        <h2 class="title is-4"> {{topic.text}} Question - {{current}} </h2>
        <h3 class="subtitle is-5">Score: {{score}} / {{max}}</h3>
        <Question v-bind:question="questions[index]" v-on:next="nextQuestion" />
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
  props: ["topic"],
  watch: { topic: function() {
      this.questions = require("../data/" + this.topic.value + ".json");
      this.score = 0;
      this.index = 0;
      this.current = 1;
      this.max = this.questions.length;
      this.end = false;
      this.showscore = false;
  } },
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
      showscore:false
    }
  },
  methods:{
    nextQuestion(scoreChange){
      this.score = this.score + scoreChange;
      this.index +=1;
      this.current +=1;
      if (this.index > (this.questions.length -1)){
        this.index = 0;
        this.end = true;
        this.showscore = true;
      }
    }
  },

  created(){
    this.questions = new Array();
    this.questions = require("../data/" + this.topic.value + ".json");
    this.max = this.questions.length;
    this.end = false;
    this.showscore = false;
  }
}
</script>

<style scoped>


</style>