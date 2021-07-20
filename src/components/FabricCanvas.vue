<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
  <div>
    <canvas id="base-canvas">
    </canvas>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { fabric } from "fabric";

interface DataType {
  canvas: fabric.Canvas | undefined;
  background: string;
}

export default Vue.extend({
  name: "FabricCanvas",
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

      this.canvas = canvas;
    });
  },
});
</script>
