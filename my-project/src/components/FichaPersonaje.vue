<template>
  <div class="flex flex-row flex-wrap gap-5 bg-cover bg-no-repeat  rounded-lg px-10 py-10 mt-10 mx-10 drop-shadow-2xl "
  style="background-image: url(../../imagenes/bb.jpg)">
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
      //de momentos sin usar favoritos: []

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
        //ocupacion es un array originalmente, lo convierto en un String
        this.personajes=this.personajes.map((el) =>{
          el.occupation=el.occupation.toString()
          return el;
        } )


           FichaDatos.personajesRecibidos = this.personajes;
    },
    getMessage(value) {
      this.busqueda = value;
    },
    verFavoritos() {
      let favoritos = JSON.parse(localStorage.getItem("favoritos"));
      this.personajes = favoritos;
    },
    //devuelve los personajes sin los favoritos SIN USAR
    comprobarFavoritos(busquedaPersonajes) {
      let favoritos = JSON.parse(localStorage.getItem("favoritos"));
      if(favoritos!=null){
      let filtrado = busquedaPersonajes.filter((el) => {
        if (favoritos.reduce((acc, elfavo) => elfavo.name == el.name ? ++acc : acc, 0) == 0) {
          return el;
        };
      })
   
       return filtrado;
    }
  }
  }
};

</script>
<style>
.miniaturas {
  @apply flex w-28 h-28 rounded-3xl hover:scale-110 transition duration-500;
}

.btn {
  @apply ml-10 bg-green-500 rounded-full px-3 py-1 text-xs font-semibold text-white mr-2 mb-2 mt-7 hover:bg-green-700 focus:outline-none;
}
</style>
