<template>
  <div id="app">
      <panel></panel>
  </div>
</template>

<script>
import panel from './components/search-panel.vue';


export default {
  components: {
    panel
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  height: 100%;
  background: url('./assets/background.png') no-repeat;
  background-size: 100% 100%
}

#app {
  color: #2c3e50;
  width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
  position: absolute;
  top: 35%;
  left: 50%;
  margin-left: -300px;
  margin-top: -100px;
}

</style>

