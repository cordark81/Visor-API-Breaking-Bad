<template>
      <div class="flex flex-row flex-wrap gap-5 bg-cover rounded-lg px-10 py-10 mt-10 mx-10 drop-shadow-2xl bg-[url('../imagenes/enhanced-1422-1590164550-2.jpg')]"
      >      
      <BannerPrincipal />
      <SpinnerCarga v-if="this.spinnerActivo" />
      <div class="flex flex-col justify-around gap-5 lg:gap-05">
        <BuscaPersonajes @busqueda="getMessage" />
        <div class="flex flex-col gap-4">
          <button @click="verFavoritos" type="button" class="btnBanner">Ver favoritos</button>
          <div v-if="favoritosVacios">
            <AlertaModal :modal="true" :mensaje="modalFavoritos" />
          </div>
          <button @click="conseguirDatos(busqueda)" type="button" class="btnBanner">
            Obtener personaje
          </button>
          <AlertaModal v-if="this.exitoBusqueda" :modal="true" :mensaje="modalBusqueda" />
        </div>
      </div>
    </div>
   <div class="grid place-items-center">
    <div class="w-min mt-10 rounded-3xl drop-shadow-2xl  bg-[url('../imagenes/bb.jpg')]" >
      <h1 class=" px-5 text-center font-mono text-5xl text-amber-500">{{ cabecera }}</h1>
    </div>
  </div>
  <div class="flex flex-row flex-wrap justify-center">
    <FichaDatos :personajesRecibidos="personajes" v-for="personaje in personajes" :key="personaje.char_id"
      :nombreRecibido="personaje.name" :imagenRecibida="personaje.img" :cumpleañosRecibido="personaje.birthday"
      :apodoRecibido="personaje.nickname" :ocupacionRecibida="personaje.occupation"
      :actualizarFavoritos=origenFavoritos />
  </div>


</template>
<script>
import axios from "axios";
import BuscaPersonajes from "./components/BuscaPersonajes.vue";
import FichaDatos from "./components/FichaDatos.vue";
import BannerPrincipal from "./components/BannerPrincipal.vue";
import AlertaModal from "./components/AlertaModal.vue";
import SpinnerCarga from "./components/SpinnerCarga.vue";

export default {
  name: "App",
  components: {
    BuscaPersonajes,
    FichaDatos,
    BannerPrincipal,
    AlertaModal,
    SpinnerCarga,
  },
  data() {
    return {
      cabecera: "",
      personajes: [],
      exitoBusqueda: false,
      favoritosVacios: false,
      spinnerActivo: false,
      origenFavoritos: false,
      modalBusqueda: "No se encuentra personaje con ese nombre",
      modalFavoritos: "No hay favoritos guardados",


    };
  },

  methods: {
    async conseguirDatos(nombre) {

      this.origenFavoritos = false;
      this.exitoBusqueda = false;
      this.spinnerActivo = true;
      try {
        const response = await axios.get(
          `https://www.breakingbadapi.com/api/characters?name=${nombre}`
        );

        this.personajes = response.data;


        (this.personajes.length === 0) ? this.exitoBusqueda = true : this.exitoBusqueda = false;
        this.spinnerActivo = false;
        this.cabecera = "Resultados Búsqueda"
      } catch (error) {
        console.log(error);
      }
      //ocupación es un array originalmente, lo convierto en un String
      this.personajes = this.personajes.map((el) => {
        el.occupation = el.occupation.toString()
        return el;
      })

    },
    getMessage(value) {
      this.busqueda = value;
    },

    verFavoritos() {
      this.origenFavoritos = true;
      this.favoritosVacios = false;

      // retraso la emisión de esta parte del código para que darle tiempo a evaluar la variable de "favoritosVacios" en el v-if, 
      // sin este retardo solo da el mensaje en una ocasión.
      // Esto también nos permite ver el modal de eliminar.

      setTimeout(() => {
        this.cabecera = "Favoritos";
        let favoritos = JSON.parse(localStorage.getItem("favoritos"));

        if (favoritos.length > 0) {
          favoritos.length == 0 ? this.favoritosVacios = true : this.favoritosVacios = false;

        } else {
          this.favoritosVacios = true;
        }
        this.personajes = favoritos;

      }, 1500);

    },

  },

};

</script>
<style>
/*elementos repetidos generamos una clase para cada uno de ellos*/
.miniaturas {
  @apply flex w-28 h-28 rounded-3xl hover:scale-110 transition duration-500;
}

.btnBanner {
  @apply ml-10 bg-green-500 rounded-full px-3 py-1 text-xs font-semibold text-white mr-2 mb-2 mt-7 hover:bg-green-700 focus:outline-none;
}

.btn {
  @apply bg-green-500 rounded-full px-3 py-1 text-xs font-semibold text-white hover:bg-green-700 focus:outline-none;
}
</style>
