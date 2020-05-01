<template>
  <div id="app">
    <shuffle/>
  </div>
</template>

<script>
import Shuffle from "./components/Shuffle";

export default {
  name: 'App',
  components: {
    Shuffle
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
