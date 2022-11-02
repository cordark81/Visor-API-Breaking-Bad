<template>
    <div class=" flex flex-row w-96 h-64 rounded-3xl overflow-hidden shadow-lg mt-10 ml-10">
        <img class="w-48 h-64" :src="imagenRecibida" alt="Sunset in the mountains">
        <div class="" style="background-image: url(../../imagenes/bb.jpg)">
            <div class="font-bold text-xl text-center mb-2 text-amber-400"> {{ nombreRecibido }} </div>
            <p class="text-white text-base text-left ml-2">
                Fecha Nacimiento: <br>{{ cumpleañosRecibido }}<br>
                Apodo: {{ apodoRecibido }}<br>
                Ocupacion: {{ ocupacionRecibida }}
            </p>
            <button v-if="anaidirBtn" class="btn" @click="anaidirFavoritos">Añadir a
                favoritos</button>
            <button v-if="eliminarBtn" class="btn" @click="eliminarFavoritos">Eliminar de
                favoritos</button>
        </div>
    </div>
</template>
<script>

export default {

    props: {
        nombreRecibido: { type: String },
        imagenRecibida: { type: String },
        cumpleañosRecibido: { type: String },
        apodoRecibido: { type: String },
        ocupacionRecibida: { type: String },
        personajesRecibidos: { type: Array }


    },
    data() {
        return {
            actualizacion: false

        }
    },
    methods: {

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
                    this.actualizacion = true;

                }
            }
        },
        eliminarFavoritos() {
            let datos = JSON.parse(localStorage.getItem("favoritos"));

            //cambiamos las condiciones para el borrado
            if (datos.reduce((acc, el) => el.name == this.nombreRecibido ? ++acc : acc, 0) == 1) {
                datos = datos.filter((el) => el.name != this.nombreRecibido);
                localStorage.setItem("favoritos", JSON.stringify(datos))
                this.actualizacion = true;
            }
        },
    },

    computed: {

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