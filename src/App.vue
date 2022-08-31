<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img alt="Vuetify Logo" class="shrink mr-2" contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png" transition="scale-transition" width="40" />

        <v-img alt="Vuetify Name" class="shrink mt-1 hidden-sm-and-down" contain min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png" width="100" />
      </div>

      <v-spacer></v-spacer>

      <v-btn href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank" text>
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <FabricCanvas :width=600 :height=400 />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import FabricCanvas from './components/FabricCanvas.vue';

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

  data(): DataType {
    return {
      rects: [
        { x: 100, y: 100, w: 200, h: 200, fill: 'red' },
        { x: 400, y: 200, w: 100, h: 100, fill: 'blue' },
      ],
    };
  },
  methods: {
    handleCoordChange: function (index: number, coord: number[], event: string) {
      console.log(index, coord, event);
      this.rects[index].x = coord[0];
      this.rects[index].y = coord[1];
      this.rects[index].w = coord[2];
      this.rects[index].h = coord[3];
    }
  }

});
</script>
