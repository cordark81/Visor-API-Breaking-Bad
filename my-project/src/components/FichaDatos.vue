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
            <button
                class="btn inline-block">Añadir a favoritos</button>
           
            <button
                class="btn inline-block">Remover favoritos</button>
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
        ocupacionRecibida :{type:Array}
        
    }

}





</script>