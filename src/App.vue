<template>
  <div id="app">
    <NumerosMobile v-if="$mq === 'mobile'" />

    <Numeros v-if="$mq !== 'mobile'" />

  </div>
</template>

<script>
import Numeros from './components/Numeros.vue'
import NumerosMobile from './components/NumerosMobile.vue';


export default {
  name: 'App',
  components: {
    Numeros,
    NumerosMobile
}
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
