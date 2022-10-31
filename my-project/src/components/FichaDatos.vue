<template>
    <div class=" max-w-sm rounded overflow-hidden shadow-lg mt-10 ml-10">
        <img class="w-full h-96" :src="imagenRecibida" alt="Sunset in the mountains">
        <div class="px-6 py-4 bg-white">
            <div class="font-bold text-xl mb-2"> {{ nombreRecibido }} </div>
            <p class="text-gray-700 text-base">
                Fecha Nacimiento: {{ cumpleañosRecibido }}<br>
                Apodo: {{ apodoRecibido }}<br>
                Ocupacion: {{ ocupacionRecibida }}
            </p>
        </div>
        <div class="px-6 pt-4 pb-2 bg-white">
            <button class="btn inline-block" @click="anaidirFavoritos">Añadir a favoritos</button>

            <button class="btn inline-block" @click="verFavoritos">Remover favoritos</button>


        </div>
    </div>
</template>
<script>

import fs from "fs"
import { pushScopeId } from "vue";

export default {
    components: {
        fs,
    },

    props: {
        nombreRecibido: { type: String },
        imagenRecibida: { type: String },
        cumpleañosRecibido: { type: String },
        apodoRecibido: { type: String },
        ocupacionRecibida: { type: Array },
        personajesRecibidos: { type: Array }


    },
    data() {
        return {

        }
    },
    methods: {
        anaidirFavoritos() {
            if (localStorage.getItem("favoritos") == null) {
                localStorage.setItem("favoritos", JSON.stringify(this.personajesRecibidos.filter((el) => el.name == this.nombreRecibido)));
            } else {
                let datos = JSON.parse(localStorage.getItem("favoritos"));

                if ((datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0)) == 0) {
                    datos = datos.concat(this.personajesRecibidos.filter((el) => el.name == this.nombreRecibido))
                    localStorage.setItem("favoritos", JSON.stringify(datos))
                }
            }
        },
        verFavoritos() {
            let prueba = JSON.parse(localStorage.getItem("favoritos"));
           
        }
    }

}

</script>