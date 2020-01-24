<template>
  <div>
    <div v-if="nameadded===false">
        <h2 class="title is-4"> You scored: {{result}} of {{max}} in {{topic}}</h2>
        <form>
          <b-field label="Name">
            <b-input v-model="name"></b-input>
          </b-field>
          <button class="button is-medium is-success" @click="add">
            Add Name to High Scores
          </button>
          <button class="button is-medium is-danger" @click="clear">
            Clear Saves
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
import uuid from "uuid";
import ScoresTable from './ScoresTable'

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
      add: function () {
        this.scorelist.push({topic: this.topic, name: this.name, index: uuid.v4(), score: this.result, max: this.max});
        const data = JSON.stringify(this.scorelist)
        window.localStorage.setItem('scorelist', data);
        this.nameadded=true;
        this.hasscores=true;
      },
      clear: function() {
        window.localStorage.removeItem('scorelist');
        while (this.scorelist.length > 0){
          this.scorelist.pop();
        }
        this.hasscores=false;
      }
  },
  created(){
    if (JSON.parse(window.localStorage.getItem('scorelist')) !== null){
       this.scorelist = JSON.parse(window.localStorage.getItem('scorelist'));
       this.hasscores=true;
    }

    this.nameadded=false;
   
  } 
}
</script>

<style scoped>

</style>
