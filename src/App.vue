<template>
  <div id="app">
    <!-- SecKill -->
    <sec-kill></sec-kill>
  </div>
</template>

<script>
import SecKill from "./SecKill";

export default {
  name: 'App',
  components: {
    SecKill
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
  width:1020px;
  margin-left:2%;
  position:relative;
  background:#FFF;
  margin:0 auto;
}
</style>
