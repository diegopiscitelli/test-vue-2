<template>
  <div id="app">
    <GifCatalog msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import GifCatalog from './components/GifCatalog.vue'

export default {
  name: 'App',
  components: {
    GifCatalog
  }
}
</script>

<style>
*{
  margin: 0;
  box-sizing:border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
