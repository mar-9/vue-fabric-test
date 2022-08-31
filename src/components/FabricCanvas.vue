<!--
    Copyright(c) 2021 TECHaas.com, all rights reserved. 
-->
<template>
  <div>
    <v-row>
      <canvas id="base-canvas">
        <FabricRect v-for="(rect, index) in rects" :key="index" :rect-rec="rect" :canvas="canvas" fillColor="red"
          @coord-change="(coord, event) => $emit('coord-change', index, coord, event)" />
      </canvas>
    </v-row>
    <v-row>
      <v-col>
        <button v-on:click="addText">テキスト</button>
        <button v-on:click="addLine">線</button>
        <button v-on:click="addRect">四角</button>
        <button v-on:click="addCircle">丸</button>
        <button v-on:click="removeObj">削除</button>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        図形の選択
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn :class="[status == 0 ? 'primary' : 'normal']" v-on:click="changeState(0)">
          選択
        </v-btn>
        <v-btn :class="[status == 1 ? 'primary' : 'normal']" v-on:click="changeState(1)">
          線
        </v-btn>
        <v-btn :class="[status == 2 ? 'primary' : 'normal']" v-on:click="changeState(2)">
          四角
        </v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        色の選択
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn :class="[colorStatus == 0 ? 'primary' : 'normal']" v-on:click="changeColor(0)">
          赤
        </v-btn>
        <v-btn :class="[colorStatus == 1 ? 'primary' : 'normal']" v-on:click="changeColor(1)">
          青
        </v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-btn-toggle v-model="toggle_exclusive">
        <v-btn active-class="aa" @click="changeState(1)">線</v-btn>
        <v-btn active-class="aa" @click="changeState(2)">四角</v-btn>
      </v-btn-toggle>
    </v-row>
  </div>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import { fabric } from "fabric";
import FabricRect from '@/components/FabricRect.vue';
import { rectRecord } from '@/App.vue';

interface DataType {
  toggle_exclusive: any;
  canvas: fabric.Canvas | undefined;
  background: string;
  selectionMode: boolean;
  startX: number;
  startY: number;
  status: number;
  colorStatus: number;
  last: any;
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
  data(): DataType {
    return {
      toggle_exclusive: [],
      canvas: undefined,
      background: 'lightgray',
      selectionMode: false,
      startX: 0.0,
      startY: 0.0,
      status: 0,
      colorStatus: 0,
      last: undefined,
    };
  },
  mounted: function () {
    let self = this;
    this.$nextTick(function () {
      const canvas = new fabric.Canvas("base-canvas", {
        width: this.width,
        height: this.height,
        backgroundColor: this.background,
      });
      canvas.preserveObjectStacking = true;
      canvas.stateful = true;
      var imgUrl = require('@/assets/img/bg_image.png');
      fabric.Image.fromURL(imgUrl, function (img) {
        canvas.setBackgroundImage(img, canvas.renderAll.bind(canvas), {
          // scaleX: canvas.width / img.width,
          // scaleY: canvas.height / img.height
        });
      });

      fabric.Object.prototype.transparentCorners = false;

      canvas.on("text:changed", adjustTextWidth);
      canvas.on("mouse:down", function (options) {
        console.log(options);
        self.startSelect(options);
      });
      canvas.on("mouse:up", function (options) {
        console.log(options);
        self.endSelect(options);
      });
      canvas.on("mouse:move", function (options) {
        console.log(options);
        self.drag(options);
      });

      this.canvas = canvas;
    });
  },
  methods: {
    changeColor: function (colorStatus: number) {
      this.colorStatus = colorStatus;
      let color = colorStatus == 0 ? 'red' : 'blue';
      this.canvas?.getActiveObject().set("fill", color);
      this.canvas?.renderAll();
    },
    changeState: function (status: number) {
      this.status = status;
      console.log(this.status);
    },
    addText: function () {
      const text = new fabric.Textbox('注釈を記述',
        {
          width: 450
        });
      this.canvas?.add(text);
    },
    addLine: function () {
      const line = new fabric.Line(
        [50, 100, 200, 200],
        {
          left: 170,
          top: 150,
          stroke: 'red'
        });
      this.canvas?.add(line);
    },
    addRect: function () {
      const rect = new fabric.Rect(
        {
          left: 200,
          top: 100,
          fill: 'red',
          width: 31,
          height: 31,
        });
      this.canvas?.add(rect);
    },
    addCircle: function () {
      const circle = new fabric.Circle(
        {
          radius: 20,
          left: 100,
          top: 100,
          angle: 45,
          startAngle: 0,
          endAngle: Math.PI,
          stroke: '#000',
          strokeWidth: 3,
          fill: '',
        });
      this.canvas?.add(circle);
    },
    removeObj: function () {
      this.canvas?.remove(this.canvas?.getActiveObject());
    },
    startSelect(e: fabric.IEvent) {
      if (this.status == 1 || this.status == 2) {
        if (this.canvas !== undefined) {
          this.canvas.isDrawingMode = true;
        }
        this.startX = e.pointer?.x ?? 0;
        this.startY = e.pointer?.y ?? 0;
      }
    },
    drag(e: fabric.IEvent) {
      if (this.canvas?.isDrawingMode) {
        let shape = this.createShape(this.status, e);
        if (shape !== undefined) {
          this.canvas?.remove(this.last);
          this.last = shape;
          this.canvas?.add(shape);
        }
      }
    },
    endSelect(e: fabric.IEvent) {
      let shape = this.createShape(this.status, e);
      if (shape !== undefined) {
        this.canvas?.remove(this.last);
        this.canvas?.add(shape);
        if (this.canvas !== undefined) {
          this.canvas.isDrawingMode = false;
        }
      }
    },
    createShape: function (type: number, e: fabric.IEvent) {
      let shape: fabric.Object | undefined = undefined;
      const w = (e.pointer?.x ?? 0) - this.startX;
      const h = (e.pointer?.y ?? 0) - this.startY;
      switch (this.status) {
        case 1:
          shape = new fabric.Line(
            [this.startX, this.startY, e.pointer?.x ?? 0, e.pointer?.y ?? 0],
            {
              stroke: 'blue'
            }
          );
          break;
        case 2:
          shape = new fabric.Rect(
            {
              left: this.startX,
              top: this.startY,
              width: w,
              height: h,
              stroke: 'blue',
              fill: 'gray'
            }
          );
          break;
      }
      return shape;
    },
  }
});

function adjustTextWidth(evt: fabric.IEvent) {
  if (evt.target instanceof fabric.IText) {
    const text = evt.target.text || ""
    while (evt.target.textLines.length > text.split("\n").length) {
      evt.target.set({
        width: evt.target.getScaledWidth() + 1
      })
    }
  }
}
</script>

<style>
.aa {
  background: blue;
}
</style>