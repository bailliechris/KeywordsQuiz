<template>
  <div class = "tile is-ancestor">
    <div class= "tile is-4 is-vertical is-parent">
      <div class = "tile is-child box">
        <b-field label="Choose a Subject:">
            <b-select v-model="subject" placeholder="Select a Subject" simple>
              <option v-for="subject in subjects" v-bind:value="subject.value" v-bind:key="subject.value">
                  {{ subject.text }}
              </option>
            </b-select>
        </b-field>
      </div>
      <div class="tile is-child box">
        <div v-if="subject">
          <b-field label="Choose a Topic:">
            <b-select v-model="topic" placeholder="Select a Topic" simple>
              <option v-for="topic in topics" v-bind:value="topic" v-bind:key="topic.value">
                {{ topic.text }}
              </option>
            </b-select>
          </b-field>
        </div>
      </div>
    </div>

    <div class="tile is-parent">
      <div class="tile is-child box">
        <div v-if="topic">
            <QuestionMan v-bind:topic="topic"/>
        </div>
      </div>
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
  watch: { subject: function() {
      this.topics = new Array();
      this.topics = require("../data/" + this.subject + ".json");
    }
  },
  data(){
    return{
      subject:"",
      topic: ""
    }
  },
  created(){
    this.subjects = new Array();
    this.subjects = require("../data/subjectlist.json");
  }
}
</script>

<style scoped>

</style>