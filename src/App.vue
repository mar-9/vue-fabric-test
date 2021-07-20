<template>
  <div id="app">
    <FabricCanvas :width=600 :height=400 />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import FabricCanvas from './components/FabricCanvas.vue';

export default Vue.extend({
  name: 'App',
  components: {
    FabricCanvas
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
