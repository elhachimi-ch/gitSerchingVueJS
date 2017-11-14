<template>
  <div id="app">
    <img src="./assets/logo.png">
    <GitSearch/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import GitSearch from './components/GitSearch'


export default {
  name: 'app',
  components: {
    HelloWorld,
    GitSearch
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0px;
}
</style>
