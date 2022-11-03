<template>
  <div class="flex flex-row flex-wrap gap-5 bg-cover bg-no-repeat  rounded-lg px-10 py-10 mt-10 mx-10 drop-shadow-2xl "
  style="background-image: url(../../imagenes/bb.jpg)">
    <BannerPrincipal />
    <div class="flex flex-col justify-around gap-5 lg:gap-05">
      <BuscaPersonajes @busqueda="getMessage" />
      <div class="flex flex-col gap-4">
        <button @click="verFavoritos" type="button" class="btn">Ver favoritos </button>
        <div v-if="favoritosVacios">
        <AlertaModal  :modal="true" :mensaje="modalFavoritos"/>
      </div>
        <button @click="conseguirDatos(busqueda)" type="button" class="btn">
          Obtener personaje
        </button>
        <AlertaModal v-if="this.exitoBusqueda" :modal="true" :mensaje="modalBusqueda"/>
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
import BuscaPersonajes from "./components/BuscaPersonajes.vue";
import FichaDatos from "./components/FichaDatos.vue";
import BannerPrincipal from "./components/BannerPrincipal.vue";
import AlertaModal from "./components/AlertaModal.vue";


export default {
  name: "App",
  components: {
    BuscaPersonajes,
    FichaDatos,
    BannerPrincipal,
    AlertaModal,
  },
  data() {
    return {
      personajes: [],
      exitoBusqueda: false,
      favoritosVacios: false,
      //de momentos sin usar favoritos: []
      modalBusqueda:"No se encuentra personaje con ese nombre",
      modalFavoritos:"No hay favoritos guardados"

    };
  },

  methods: {
    async conseguirDatos(nombre) {
      this.exitoBusqueda=false;
      try {
        const response = await axios.get(
          `https://www.breakingbadapi.com/api/characters?name=${nombre}`
        );
       
        this.personajes = response.data;


        (this.personajes.length === 0) ? this.exitoBusqueda = true : this.exitoBusqueda = false;

      } catch (error) {
        console.log(error);
      }
        //ocupación es un array originalmente, lo convierto en un String
        this.personajes=this.personajes.map((el) =>{
          el.occupation=el.occupation.toString()
          return el;
        } )
        
    },
    getMessage(value) {
      this.busqueda = value;
    },
    
    verFavoritos() {
      this.favoritosVacios=false;
      setTimeout(()=>{
        let favoritos = JSON.parse(localStorage.getItem("favoritos"));
      favoritos.length==0?this.favoritosVacios=true:this.favoritosVacios=false;
      this.personajes = favoritos;
      }, 1500);
                
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
  },
  //NO FUNCIONA!!!!!!!!!!!!!!!!!!!!!!!!SIN USAR
  computed:{
    flagFavoritos() {
            if (this.favoritosVacios)
                this.favoritosVacios = false

            const favoritos = JSON.parse(localStorage.getItem("favoritos"));

            return (favoritos != null)? this.favoritosVacios=true: this.favoritosVacios=false;
        },
  }
};

</script>
<style>
/*elementos repetidos generamos una clase para cada uno de ellos*/
.miniaturas {
  @apply flex w-28 h-28 rounded-3xl hover:scale-110 transition duration-500;
}

.btn {
  @apply ml-10 bg-green-500 rounded-full px-3 py-1 text-xs font-semibold text-white mr-2 mb-2 mt-7 hover:bg-green-700 focus:outline-none;
}
</style>
