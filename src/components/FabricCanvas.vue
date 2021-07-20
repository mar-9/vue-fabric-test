<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
  <div>
    <canvas id="base-canvas">
      <FabricRect :coord="[100, 100, 200, 200]" :canvas="canvas" fillColor="red"/>
    </canvas>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { fabric } from "fabric";
import FabricRect from '@/components/FabricRect.vue';

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
