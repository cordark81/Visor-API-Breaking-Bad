<template>
  <div>
    <BuscaPersonajes @busqueda="getMessage" />

    <br />
    <button
      @click="conseguirDatos(busqueda)"
      class="border-t-lime-900 bg-red-500"
    >
      Obtener personajes
    </button>
    <li v-for="personaje in personajes" :key="personaje.char_id">
      {{ personaje.name }}
      {{ personaje.img }}
      <h1 class="text-3xl font-bold underline">Hello world!</h1>
      <h2>hola</h2>
      <h3>hola</h3>
    </li>
  </div>
</template>
<script>
import axios from "axios";
import BuscaPersonajes from "./BuscaPersonajes.vue";

export default {
  name: "NombreBusqueda",
  components: {
    BuscaPersonajes,
  },
  data() {
    return {
      personajes: [],
    };
  },

  methods: {
    async conseguirDatos(nombre) {
      try {
        const response = await axios.get(
          `https://www.breakingbadapi.com/api/characters?name=${nombre}`
        );

        this.personajes = response.data;
      } catch (error) {
        console.log(error);
      }
    },
    getMessage(value) {
      this.busqueda = value;
    },
  },
};
</script>
