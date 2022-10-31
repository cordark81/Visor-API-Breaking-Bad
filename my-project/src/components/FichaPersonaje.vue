<template>
  <div class="flex flex-row flex-wrap gap-5   bg-gray-700  rounded-lg px-10 py-10 mt-10 mx-10 drop-shadow-2xl ">
    <BannerPrincipal />
    <div class="flex flex-col justify-around gap-5 lg:gap-05">
      <BuscaPersonajes @busqueda="getMessage" />
      <div class="flex flex-col gap-4">
        <button @click="verFavoritos" type="button" class="btn">Ver favoritos </button>
        <button @click="conseguirDatos(busqueda)" type="button" class="btn">
          Obtener personaje
        </button>
        <AlertaBusqueda v-if="this.exitoBusqueda" />
      </div>
    </div>
  </div>
  <div class="flex flex-row flex-wrap justify-center">
    <FichaDatos :personajesRecibidos="personajes" v-for="personaje in personajes" :key="personaje.char_id"
      :nombreRecibido="personaje.name" :imagenRecibida="personaje.img" :cumpleañosRecibido="personaje.birthday"
      :apodoRecibido="personaje.nickname" :ocupacionRecibida="personaje.occupation" />
  </div>


</template>
<script>
import axios from "axios";
import BuscaPersonajes from "./BuscaPersonajes.vue";
import FichaDatos from "./FichaDatos.vue";
import BannerPrincipal from "./BannerPrincipal.vue";
import AlertaBusqueda from "./AlertaBusqueda.vue";

export default {
  name: "FichaPersonaje",
  components: {
    BuscaPersonajes,
    FichaDatos,
    AlertaBusqueda,
    BannerPrincipal,
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

        (this.personajes.length === 0) ? this.exitoBusqueda = true : this.exitoBusqueda = false;

      } catch (error) {
        console.log(error);
      }
      FichaDatos.personajesRecibidos = this.personajes;
    },
    getMessage(value) {
      this.busqueda = value;
    },
    verFavoritos() {
      let prueba = JSON.parse(localStorage.getItem("favoritos"));
      this.personajes=prueba;
    }

  },
};
</script>
<style>
.miniaturas {
  @apply flex w-28 h-28 rounded-3xl hover:scale-110 transition duration-500;
}

.btn {
  @apply bg-green-500 rounded-full rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2 hover:bg-green-400 focus:outline-none;
}
</style>
