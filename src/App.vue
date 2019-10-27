<template>
  <div id="app">
    <invest-calc/>
  </div>
</template>

<script>
import InvestCalc from './components/InvestCalc.vue'

export default {
  name: 'app',
  components: {
    InvestCalc
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
}
</style>
