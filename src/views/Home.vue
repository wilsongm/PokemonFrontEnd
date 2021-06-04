<template>
  <div>
    <Pokemon :data="data"/>
  </div>
</template>

<script>
import Pokemon from "@/components/Pokemon";
import axios from "axios";

export default {
  name: "Home",

  components: {
    Pokemon,
  },
  data() {
    return {
      data: [],
  
    };
  },
  methods: {
    getPokemon() {
      axios.get("https://localhost:5001/pokemon").then((resp) => {
        this.data = resp.data;
      })
    },
     
  },
  created: function () {
    this.getPokemon();
    
  },
};
</script>
