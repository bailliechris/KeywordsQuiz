<template>
  <section class="section">
    <div class="box">
      <div v-if="end === false">
        <h2 class="title is-4"> Question - 
          {{current}} </h2>
        <h3 class="subtitle is-5">Score: {{score}} / {{max}}
        </h3>
        <Question v-bind:question="questions[index]" v-on:next="nextQuestion" />
      </div>
      <div v-else>
        <p>Please choose another topic!</p>
      </div>
    </div>
  </section>
</template>

<script>
import Question from "./aQuestion"

export default {
  name: 'QuestionMan',
  props: ["topic"],
  watch: { topic: function() {
      this.questions = require("../data/" + this.topic + ".json");
      this.score = 0;
      this.index = 0;
      this.current = 1;
      this.max = this.questions.length;
      this.end = false;
  } },
  components: {
    Question
  },
  data(){
    return{
      //questions - appears here after creation and load
      score:0,
      index:0,
      max:0,
      current:1,
      end:false
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
      }
    }
  },

  created(){
    this.questions = new Array();
    this.questions = require("../data/" + this.topic + ".json");
    this.max = this.questions.length;
    this.end = false;
  }
}
</script>

<style scoped>


</style>