<template>
  <div id="app">
    <search></search>
  </div>
</template>

<script>
import Search from './components/Search'
export default {
  components:{
    Search
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
html {
  height: 100%;
  overflow: hidden;
}

body {
  height: 100%;
  background:url('./assets/pc_bg.jpg') no-repeat;
  background-size: 100% 100%
}
@media screen and (max-width: 768px){
  body{
    background:url('./assets/mobile_bg.jpg') no-repeat;
  }
}
</style>
