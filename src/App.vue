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
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;500;700;900&display=swap');
  #app {
    font-family: Roboto, Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100vh;
    width:100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #1d3557;
  }
  body {
    margin: 0;
    height: 100vh;
    overflow: hidden;
  }
</style>
