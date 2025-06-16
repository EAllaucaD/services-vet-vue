<template>
  <div>
    <Encabezado />

    <div class="contenido-servicios">
      <Carrusel :modalAbierto="modalVisible" @seleccionar="mostrarModal" />
    </div>

    <ModalServicio
      v-if="modalVisible"
      :item="itemSeleccionado"
      @cerrar="modalVisible = false"
    />
  </div>
</template>

<script>
import Encabezado from './components/Encabezado.vue'
import Carrusel from './components/Carrusel.vue'
import ModalServicio from './components/ModalServicio.vue'

export default {
  name: 'App',
  components: {
    Encabezado,
    Carrusel,
    ModalServicio
  },
  data() {
    return {
      modalVisible: false,
      itemSeleccionado: null
    };
  },
  methods: {
    mostrarModal(item) {
      this.itemSeleccionado = item;
      this.modalVisible = true;
    }
  }
};
</script>

<style scoped>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
}

.contenido-servicios {
  padding-top: 2rem; /* Espacio entre encabezado y carrusel */
  padding-left: 1rem;
  padding-right: 1rem;
}

/* Responsive para móviles */
@media (max-width: 640px) {
  .contenido-servicios {
    padding-top: 1.2rem;
    padding-left: 0.5rem;
    padding-right: 0.5rem;
  }
}
</style>
