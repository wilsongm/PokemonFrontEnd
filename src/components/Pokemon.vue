
<template>
  <v-simple-table>
    <v-data-table
      :headers="headers"
      :items="data"
      :search="search"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Pokemones</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-text-field
            class="text-xs-center"
            v-model="search"
            label="Busqueda"
            single-line
            hide-details
          ></v-text-field>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <v-card>
              <v-card-title>
                <span class="text-h5" style="position: relative; left: 7rem"
                  >Detalle del Pokemon</span
                >
              </v-card-title>
              <v-card-text>
                <v-container>
                  <CardDetalle :detalle="detalle" />
                </v-container>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="viewItem(item)"> mdi-eye </v-icon>
      </template>
    </v-data-table>
  </v-simple-table>
</template>
    
<script>
import CardDetalle from "@/components/CardDetalle";
import axios from "axios";

export default {
  components: { CardDetalle },
  props: {
    data: [],
    detalle: {},
  },
  compotents: {
    CardDetalle,
  },
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Pokemon Name",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Actions", value: "actions", sortable: false },
    ],
    search: "",
    editedIndex: -1,
    editedItem: {},
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created: function () {
    this.getPokemonByUrl();
  },

  methods: {
    getPokemonByUrl(model) {
      axios.post("https://localhost:5001/Pokemon", model).then((resp) => {
        this.detalle = resp.data;
      });
    },

    viewItem(item) {
      this.getPokemonByUrl(item);
      this.dialog = true;
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.data[this.editedIndex], this.editedItem);
      } else {
        this.data.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>