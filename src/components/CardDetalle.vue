<template>
  <v-card :loading="loading" class="mx-auto my-12" max-width="374">
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>
    
    

    <v-img
      :src="detalle.sprites.front_default"
      :style="{ backgroundColor: colorType(detalle.types[0].type.name) }"
    ></v-img>
<div id="txt">
    <v-card-title>Nombre: {{ detalle.name }} </v-card-title>

    <v-card-text>
      <v-row align="center" class="mx-0"> </v-row>
      
        <span>
          <strong>Tipo de Pokemon: </strong>{{detalle.types[0].type.name}}
          <br />
        </span>
        <span
          ><strong> Habilidades:</strong> <br />
          1-{{ detalle.abilities[0].ability.name }}<br />
          2-{{ detalle.abilities[1].ability.name }} <br
        /></span>
        <span
          ><strong>Estadistica: </strong> <br />
          <strong>{{ detalle.stats[0].stat.name }} </strong> :
          {{ detalle.stats[0].base_stat }}
          <strong>{{ detalle.stats[1].stat.name }} </strong> :
          {{ detalle.stats[1].base_stat }}
          <strong>{{ detalle.stats[2].stat.name }} </strong> :
          {{ detalle.stats[2].base_stat }}
          <strong>{{ detalle.stats[5].stat.name }} </strong> :
          {{ detalle.stats[5].base_stat }} <br />
          <strong>{{ detalle.stats[3].stat.name }} </strong> :
          {{ detalle.stats[3].base_stat }}
          <strong>{{ detalle.stats[4].stat.name }} </strong> :
          {{ detalle.stats[4].base_stat }}
        </span>

        <div>
      <v-btn color="red" style="
    position: relative;
    left: 18rem;
    top: 27px;
" @click="saveFile">
      <v-icon>mdi-printer</v-icon>
    </v-btn>
    </div>
      
    </v-card-text>
</div>
    <v-divider class="mx-4"></v-divider>
  </v-card>
</template>

<script>
import { saveAs } from "file-saver";

export default {
  props: {
    detalle: {},
  },
  data: () => ({
    loading: false,
    selection: 1,
  }),

  methods: {
    colorType(type) {
      switch (type) {
        case "grass":
          return "#48d0b1";
        case "fire":
          return "#fb6c6c";
        case "water":
          return "#76bdfe";
        case "bug":
          return "#f7786b";
        case "normal":
          return "#b1736c";
          case "electric":
            return "#FFD700"
      }
    },
    reserve() {
      this.loading = true;

      setTimeout(() => (this.loading = false), 2000);
    },

    saveFile() {
      var FileSaver = require("file-saver");
      let content =  document.getElementById("txt")
      var blob = new Blob([content.textContent], { type: "text/plain;charset=utf-8" });
      FileSaver.saveAs(blob, "Detalle");
    },
  },
};
</script>

