<template>
  <div id="app">
    <Header />   
    <TopicMan />
  
  </div>
</template>

<script>
import Header from './components/Header'
import TopicMan from './components/TopicManager'

export default {
  name: 'app',
  components: {
    Header,
    TopicMan
  }
}
</script>

<style>
#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: auto;
}
</style>

