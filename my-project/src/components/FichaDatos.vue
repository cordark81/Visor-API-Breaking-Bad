<template>
    <div class=" flex flex-row w-96 h-64 rounded-3xl overflow-hidden shadow-lg mt-10 ml-10">
        <img class="w-48 h-64" :src="imagenRecibida" alt="Foto">
        <div class="w-full" style="background-image: url(../../imagenes/bb.jpg)">
            <div class="font-bold text-xl text-center mb-2 text-amber-400 mt-3"> {{ nombreRecibido }} </div>
            <p class="text-white text-sm text-center ml-3">
                Fecha Nacimiento: <br>{{ cumpleañosRecibido }}<br>
                Apodo: {{ apodoRecibido }}<br>
                Ocupacion: {{ ocupacionRecibida }}
            </p>
            <div class="container flex flex-col flew-row place-items-center pt-12">
                <button v-if="anaidirBtn" class="btn" @click="anaidirFavoritos">Añadir a
                    favoritos</button>
                <AlertaModal v-if="mensajeAnaidir" :modal="true" :mensaje="modalAnaidir" />
                <button v-if="eliminarBtn" class="btn" @click="eliminarFavoritos">Eliminar de
                    favoritos</button>
                <AlertaModal v-if="mensajeEliminar" :modal="true" :mensaje="modalEliminar" />
            </div>
        </div>
    </div>
</template>
<script>

import AlertaModal from "./AlertaModal.vue";

export default {
    components: {
        AlertaModal,

    },
    props: {
        nombreRecibido: { type: String },
        imagenRecibida: { type: String },
        cumpleañosRecibido: { type: String },
        apodoRecibido: { type: String },
        ocupacionRecibida: { type: String },
        personajesRecibidos: { type: Array },
        actualizarFavoritos: {
            type: Boolean,

        },
    },
        data() {
            return {
                actualizacion: false,
                mensajeAnaidir: false,
                mensajeEliminar: false,
                modalAnaidir: "Exito al añadir a favoritos",
                modalEliminar: "Personaje elimiando de favoritos",
            }
        },
        methods: {

            anaidirFavoritos() {
                this.mensajeAnaidir = true;
                let datos = JSON.parse(localStorage.getItem("favoritos"));
                //si favoritos esta vació, almacenamos en localStorage los datos de la ficha seleccionada
                if (datos == null) {
                    localStorage.setItem("favoritos", JSON.stringify(this.personajesRecibidos.filter((el) => el.name == this.nombreRecibido)));
                } else {

                    //si la función es igual a 0 modificamos los datos y añadimos la nueva ficha a almacenar 
                    if (datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 0) {
                        datos = datos.concat(this.personajesRecibidos.filter((el) => el.name == this.nombreRecibido))
                        localStorage.setItem("favoritos", JSON.stringify(datos))
                        this.actualizacion = true;

                    }
                }

            },
            eliminarFavoritos() {

                this.mensajeEliminar = true;

                let datos = JSON.parse(localStorage.getItem("favoritos"));

                //cambiamos las condiciones para el borrado
                if (datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 1) {
                    datos = datos.filter((el) => el.name != this.nombreRecibido);
                    localStorage.setItem("favoritos", JSON.stringify(datos))
                    this.actualizacion = true;
                    // actualiza favoritos 
                    console.log(this.actualizarFavoritos);
                    if (this.actualizarFavoritos) {
                        this.$parent.verFavoritos();
                    }

                }
            },

        },


        computed: {
            //cambian el estado de aparición de los botones según se pulsan lógica de añadir o eliminar
            anaidirBtn() {
                if (this.actualizacion)
                    this.actualizacion = false

                const favoritos = JSON.parse(localStorage.getItem("favoritos"));

                return (favoritos != null) ? (favoritos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 0) : true;
            },
            eliminarBtn() {
                if (this.actualizacion)
                    this.actualizacion = false

                const favoritos = JSON.parse(localStorage.getItem("favoritos"));

                return (favoritos != null) ? (favoritos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) != 0) : false;
            }

        }
    }

</script>