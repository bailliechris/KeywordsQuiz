<template>
  <div>
    <div v-if="nameadded===false">
        <h2 class="title is-4"> You scored: {{result}} of {{max}} in {{topic}}</h2>
        <form>
          <b-field label="Name">
            <b-input v-model="name"
              minlength="2"
              maxlength="20">
            </b-input>
          </b-field>
          <button type="submit" class="button is-medium is-success" @click="add">
            Add Name to High Scores
          </button>
        </form>
    </div>
    <div v-if="hasscores===true">
      <h2 class="title is-4"> Scores </h2>
      <ScoresTable v-bind:scorelist="scorelist" />
    </div>
  </div>
</template>

<script>
import uuid from "uuid"
import ScoresTable from './ScoresTable'
import axios from 'axios'

export default {
  name: 'ScoresMan',
  components: {
    ScoresTable
  },

  props: {
    result: {type: Number, required: true, default:0, }, 
    topic: {type: String, required: true, default: "No Topic", },
    max: {type: Number, required: true, default:0, }, 
  },

  data(){
    return{
      name: '',
      scorelist: [],
      hasscores: false,
      nameadded: false
    }
  },

  methods: {
    // https://quiz-scores.herokuapp.com/api/scores
      add: function (e) {
        
        e.preventDefault();
        let now = new Date();
        let data = {
          "topic": this.topic, 
          "name": this.name, 
          "index": uuid.v4(), 
          "score": this.result, 
          "max": this.max,
          "time": now.toDateString()
          };

        axios.post("https://quiz-scores.herokuapp.com/api/scores", data)
        .then(this.scorelist.push(data));


        this.nameadded=true;
        this.hasscores=true;
      }
  },
  created(){
    axios.get("https://quiz-scores.herokuapp.com/api/scores")
      .then((res) => {
        this.scorelist = res.data;
        if(this.scorelist.length > 0) {
          this.hasscores = true;
        } else {
          this.hasscores = false;
        }   
      });
  } 
}
</script>

<style scoped>

</style>
