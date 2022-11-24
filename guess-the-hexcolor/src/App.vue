<template>
  <HexcolorGame />
</template>

<script>
import HexcolorGame from './components/HexcolorGame.vue'

export default {
  name: 'App',
  components: { HexcolorGame }
}
  //font-family: Avenir, Helvetica, Arial, sans-serif;
</script>

<style>
#app {
	font-family: Josefin Sans, Avenir, Helvetica;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #4f3c66;
  margin-top: 40px;
}

body {
  background: #e6e6e6;
  margin: 0;
}
</style>
