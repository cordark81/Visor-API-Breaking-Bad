<template>
  <div>
    <input v-model="busqueda" placeholder="Buscar" />
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
  watch: {
    busqueda: function () {
      this.$emit("busqueda", this.busqueda);
    },
  },
};
</script>
