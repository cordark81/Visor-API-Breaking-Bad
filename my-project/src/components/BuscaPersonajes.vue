<template>
  <div>
    <input class="rounded-lg border-green-500 border  focus:outline-none focus:ring focus:ring-green-500"
      v-model="busqueda" placeholder="Nombre" />
  </div>
</template>

<script>
export default {
  name: "BarraBusqueda",
  data() {
    return {
      busqueda: "",
    };
  },
  //controla si cambia la casilla de búsqueda y se emite al padre para que el hijo use ese dato
  watch: {
    busqueda: function () {
      this.$emit("busqueda", this.busqueda);
    },
  },
};
</script>
