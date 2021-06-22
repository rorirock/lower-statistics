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
@font-face {
    font-family: "Gibson";
    src:url('https://lvp-fonts.s3-eu-west-1.amazonaws.com/Gibson/GibsonRegular.woff') format("woff");
    src:url('https://lvp-fonts.s3-eu-west-1.amazonaws.com/Gibson/GibsonBoldIt.woff') format("woff");
   }

#app {
  font-family: "Gibson";
  text-align: center;
  color: #ffffff;
  margin-top: 60px;
}

</style>
