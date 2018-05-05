<template>
  <div id="app">
    <Search></Search>
  </div>
</template>

<script>
import Search from './components/Search'

export default {
  components: {
    Search
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  height: 100%;
  background-size: 100% 100%;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: absolute;
  width: 600px;
  top: 35%;
  left: 50%;
  margin-top: -100px;
  margin-left: -300px;
}
</style>
