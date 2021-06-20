<template>
  <Lower/>
</template>

<script>
import Lower from './components/Lower'
export default {
  
  name: 'App',
  components: {
    Lower
  }
}
</script>

<style>

#app {
  font-family: "Gibson","gibsonregular";
  src:'./assets/font/GibsonBoldIt.woff';
  src:'./assets/font/GibsonRegular.woff';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
