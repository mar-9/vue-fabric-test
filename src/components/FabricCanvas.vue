<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
  <div>
    <canvas id="base-canvas">
      <FabricRect 
        v-for="(rect, index) in rects"
        :key="index"
        :rect-rec="rect" 
        :canvas="canvas" 
        fillColor="red" 
        @coord-change="(coord, event) => $emit('coord-change', index, coord, event)" />
    </canvas>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import { fabric } from "fabric";
import FabricRect from '@/components/FabricRect.vue';
import { rectRecord } from '@/App.vue';

interface DataType {
  canvas: fabric.Canvas | undefined;
  background: string;
}

export default Vue.extend({
  name: "FabricCanvas",
  components: {
    FabricRect
  },
  props: {
    width: Number,
    height: Number,
    rects: Array as PropType<rectRecord[]>,
  },
  data() : DataType {
    return {
      canvas: undefined,
      background: 'lightgray',
    };
  },
  mounted: function () {
    this.$nextTick(function () {
      const canvas = new fabric.Canvas("base-canvas", {
        width: this.width,
        height: this.height,
        backgroundColor: this.background,
      });
      canvas.preserveObjectStacking = true;
      canvas.stateful = true;
      fabric.Object.prototype.transparentCorners = false;

      this.canvas = canvas;
    });
  },
});
</script>
