<template>
  <div id="app">
      <Waterparks/>
      
  </div>
</template>

<script>
import Waterparks from './components/Waterparks.vue'

export default {
  name: 'App',

  components: {

    Waterparks

  },
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
