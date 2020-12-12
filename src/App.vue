<template>
  <div id="app">
    <ocr/>
    <footer class="mt-2"> <p style="font-size:10px;"><a href="https://github.com/mohdshariqnawaz/Vue-OCR">Github Code</a> | Made by <a href="https://github.com/mohdshariqnawaz">Mohammed Shariq Nawaz</a></p></footer>
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
  /* margin-top: 20px; */
}

</style>
