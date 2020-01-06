<template>
  <div class = "box">
    <nav class="level">
      <div class="level-left">
        <b-field label="Choose a Subject:">
            <b-select v-model="subject" placeholder="Select a Subject" extended>
              <option v-for="subject in subjects" v-bind:value="subject.value" v-bind:key="subject.value">
                  {{ subject.text }}
              </option>
            </b-select>
        </b-field>
      </div>
      <div class="level-right">
        <div v-if="subject">
          <b-field label="Choose a Topic:">
            <b-select v-model="topic" placeholder="Select a Topic" simple>
              <option v-for="topic in topics" v-bind:value="topic.value" v-bind:key="topic.value">
                {{ topic.text }}
              </option>
            </b-select>
          </b-field>
        </div>
      </div>
    </nav>

    <div class="block center">
      <div v-if="topic">
          <QuestionMan v-bind:topic="topic"/>
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