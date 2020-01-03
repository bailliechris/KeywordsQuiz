<template>
    <div>
        <label for="optionSelect">Choose a Topic:</label>
        <select v-model="topic" id="optionSelect">
            <option v-for="option in options" v-bind:value="option.value" v-bind:key="option.value">
                {{ option.text }}
            </option>
        </select>

        <div v-if="topic">
            <QuestionMan v-bind:topic="topic"/>
        </div>
        <div v-else>
            <p>Please choose an option!</p>
        </div>

    </div>
</template>

<script>
//Add button to refresh / force reload questions?

import QuestionMan from './QuestionManager'

export default {
  name: 'TopicMan',
  components: {
    QuestionMan
  },
  data(){
    return{
        topic: ""
    }
  },
  created(){
    this.options = new Array();
    this.options = require("../data/topiclist.json");
  }
}
</script>

<style scoped>

</style>