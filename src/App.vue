<template>
  <div id="app">
    <ocr/>
  </div>
</template>

<script>
import Ocr from './components/Ocr.vue'

export default {
  name: 'App',
  components: {
    Ocr
  }
}
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
  padding: 0;
  
  min-height: 100vh;
}
body{
  background-color: #fff !important;
}
#app {
  height: 100%;
 font-family: 'Montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 30px;
}

</style>
