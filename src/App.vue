<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
  <div id="app">
    <FabricCanvas 
      :width=600 
      :height=400 
      :rects="rects"
      @coord-change="handleCoordChange" />
    <div class="rectText" v-for="(rect, index) in rects"
      :key="index">
      {{index}} : {{ rect }}
    </div> 
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import FabricCanvas from '@/components/FabricCanvas.vue';

export interface rectRecord {
  x: number;
  y: number;
  w: number;
  h: number;
  fill: string;
}

interface DataType {
  rects: rectRecord[];
}

export default Vue.extend({
  name: 'App',
  components: {
    FabricCanvas
  },
  data() : DataType {
    return {
      rects: [
        {x: 100, y:100, w:200, h:200, fill: 'red'},
        {x: 400, y:200, w:100, h:100, fill: 'blue'},
      ],
    };
  },
  methods: {
    handleCoordChange: function(index: number, coord: number[], event: string) {
      console.log(index, coord, event);
      this.rects[index].x = coord[0];
      this.rects[index].y = coord[1];
      this.rects[index].w = coord[2];
      this.rects[index].h = coord[3];
    }
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
.rectText {
  display: flex;
}
</style>
