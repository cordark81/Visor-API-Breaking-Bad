<template>
  <div class="flex flex-row flex-wrap gap-5   bg-gray-700  rounded-lg px-10 py-10 mt-10 mx-10 drop-shadow-2xl ">
    <div class="flex flex-row p-2 w-4/6 flex-1">
      <img src="../../../imagenes/d3bbd00fc97e601c6dabca395af2e7f6.png" alt="imagen bb"
        class="rounded-xl hover:scale-110 transition duration-500" width="200" height="200">
      <div class="flex flex-row justify-evenly ml-10 gap-5 flex-1 mt-5 invisible lg:visible">
        <img src="https://s-i.huffpost.com/gen/1317262/images/o-ANNA-GUNN-facebook.jpg" alt="image" class="miniaturas">
        <img
          src="https://vignette.wikia.nocookie.net/breakingbad/images/9/95/JesseS5.jpg/revision/latest?cb=20120620012441"
          alt="image" class="miniaturas">

        <img
          src="https://images.amcnetworks.com/amc.com/wp-content/uploads/2015/04/cast_bb_700x1000_walter-white-lg.jpg"
          alt="image" class="w-24 h-24 rounded-full  hover:scale-110 transition duration-500 ">
        <img
          src="https://vignette.wikia.nocookie.net/breakingbad/images/1/1f/BCS_S4_Gustavo_Fring.jpg/revision/latest?cb=20180824195925"
          alt="image" class="miniaturas">

        <img
          src="https://vignette.wikia.nocookie.net/breakingbad/images/b/b4/Jane.jpg/revision/latest?cb=20090621233653"
          alt="image" class="miniaturas">
      </div>
    </div>
    <div class="flex flex-col justify-around gap-5 lg:gap-05">
      <BuscaPersonajes @busqueda="getMessage" />
      <div class="flex flex-col">
        <button @click="conseguirDatos(busqueda)" type="button" class="btn">
          Obtener personaje
        </button>
        <AlertaBusqueda v-if = "this.exitoBusqueda" />
      </div>
    </div>
  </div>
  <div class="flex flex-row flex-wrap justify-center">
    <FichaDatos v-for="personaje in personajes" :key="personaje.char_id" :nombreRecibido="personaje.name"
      :imagenRecibida="personaje.img" :cumpleañosRecibido="personaje.birthday" :apodoRecibido="personaje.nickname"
      :ocupacionRecibida="personaje.occupation" />
  </div>


</template>
<script>
import axios from "axios";
import BuscaPersonajes from "./BuscaPersonajes.vue";
import FichaDatos from "./FichaDatos.vue";
import AlertaBusqueda from "./AlertaBusqueda.vue";

export default {
  name: "FichaPersonaje",
  components: {
    BuscaPersonajes,
    FichaDatos,
    AlertaBusqueda,
  },
  data() {
    return {
      personajes: [],
      exitoBusqueda: false,
    };
  },

  methods: {
    async conseguirDatos(nombre) {
      try {
        const response = await axios.get(
          `https://www.breakingbadapi.com/api/characters?name=${nombre}`
        );

        this.personajes = response.data;

        if (this.personajes.length === 0) {
          this.exitoBusqueda = true;

        }else{
          this.exitoBusqueda = false;
        }

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
<style>
.miniaturas {
  @apply flex w-24 h-24 rounded-3xl hover:scale-110 transition duration-500;
}

.btn {
  @apply bg-green-500 rounded-full rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2 hover:bg-green-400 focus:outline-none;
}
</style>
