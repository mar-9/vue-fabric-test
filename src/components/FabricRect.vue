<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
<div />
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import { fabric } from "fabric";

interface DataType {
  rect: fabric.Rect | null;
}

export default Vue.extend({
  name: "FabricRect",
  props: {
    coord: Array as PropType<number[]>,
    fillColor: String, 
    canvas: Object as PropType<fabric.Canvas>,
    handleCoordChange: Function as PropType<(coord:number[]) => void>,
  },
  data(): DataType {
    return {
      rect: null,
    };
  },
  watch: {
    // eslint-disable-next-line no-unused-vars
    canvas: function (_val, _oldVal) {
      const rect = new fabric.Rect({
        left: this.coord[0],
        top: this.coord[1],
        width: this.coord[2],
        height: this.coord[3],
        fill: this.fillColor,
      });
      this.canvas.add(rect);
      this.rect = rect;
    },
  },
});
</script>