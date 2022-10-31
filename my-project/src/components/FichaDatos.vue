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
            <button class="btn inline-block" @click="anaidirFavoritos">Añadir a
                favoritos</button>
            <button class="btn inline-block" @click="eliminarFavoritos">Eliminar de favoritos</button>
        </div>
    </div>
</template>
<script>
import { watch } from 'vue';


export default {

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
           // btnEliminar: false, varibales para hacer desaparecer botones en el aire
           // btnAnaidir: true,
        }
    },
    methods: {
        /* pendiente de hacer funcionar
        favoritoCheck() {
            //si esta en localStorage devuelve 1, sino 0
            let datos = JSON.parse(localStorage.getItem("favoritos"));
            return datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0);
        },*/
        anaidirFavoritos() {
            //si favoritos esta vacio, almacenamos en localStorage los datos de la ficha seleccionada
            if (localStorage.getItem("favoritos") == null) {
                localStorage.setItem("favoritos", JSON.stringify(this.personajesRecibidos.filter((el) => el.name == this.nombreRecibido)));
            } else {
                let datos = JSON.parse(localStorage.getItem("favoritos"));
                //si la funcion es igual a 0 modificamos los datos y añadimos la nueva ficha a almacenar 
                if (datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 0) {
                    datos = datos.concat(this.personajesRecibidos.filter((el) => el.name == this.nombreRecibido))
                    localStorage.setItem("favoritos", JSON.stringify(datos))


                }
            }
        },
        eliminarFavoritos(){
            let datos = JSON.parse(localStorage.getItem("favoritos"));
            console.log(datos);
            if(localStorage.getItem("favoritos")!=null){
                //cambiamos las condiciones para el borrado
                if (datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 1) {
                    datos = datos.filter((el)=>el.name!=this.nombreRecibido);
                    localStorage.setItem("favoritos", JSON.stringify(datos))
                }
            }
            
        }
        /*proyecto de algo
        btnsCheck() {
            if (datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 0) {

                this.btnEliminar = true;
            } else {
                this.btnEliminar = false;

            }
        },
        watch: {
            btnEliminar(newValue, oldValue) {
                if (newValue == true) {
                    btnAnaidir = false;
                } else {
                    btnAnaidir = true;
                }
            }
        }*/

    }

}

</script>