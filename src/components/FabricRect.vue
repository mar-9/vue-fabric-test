<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
<div />
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import { fabric } from "fabric";
import { rectRecord } from '@/App.vue';

interface DataType {
  rect: fabric.Rect | null;
}

export default Vue.extend({
  name: "FabricRect",
  props: {
    rectRec: Object as PropType<rectRecord>,
    fillColor: String, 
    canvas: Object as PropType<fabric.Canvas>,
  },
  data(): DataType {
    return {
      rect: null,
    };
  },
  watch: {
    /* eslint-disable @typescript-eslint/no-unused-vars */
    canvas: function (_val, _oldVal) {
      this.setupRect();
    },
  },
  methods: {
    /* eslint-disable @typescript-eslint/no-non-null-assertion */
    setupRect: function() {
      const rect = new fabric.Rect({
        left: this.rectRec.x,
        top: this.rectRec.y,
        width: this.rectRec.w,
        height: this.rectRec.h,
        originX: 'left',
        originY: 'top',
        fill: this.rectRec.fill,
      });
      rect.on('moved', () => {
        const coord = [
          Math.round(rect.left!),
          Math.round(rect.top!),
          Math.round(rect.width! * rect.scaleX!),
          Math.round(rect.height! * rect.scaleY!),
        ];
        this.$emit('coord-change', coord, 'moved');
      });
      rect.on('scaled', () => {
        const coord = [
          Math.round(rect.left!),
          Math.round(rect.top!),
          Math.round(rect.width! * rect.scaleX!),
          Math.round(rect.height! * rect.scaleY!),
        ];
        this.$emit('coord-change', coord, 'scaled');
      });
      this.canvas.add(rect);
      this.rect = rect;

    }
  },
});
</script>