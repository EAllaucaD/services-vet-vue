<template>
  <div class="modal" @click="cerrarModalPorFondo">
    <div class="contenido-modal" @click.stop>
      <button class="cerrar" @click="$emit('cerrar')">✕</button>
      <h2>{{ item.texto }}</h2>
      <img :src="item.src" :alt="item.texto" />
      <p class="descripcion">{{ item.descripcion }}</p>
      <ul>
        <li v-for="(detalle, index) in item.detalles" :key="index">
          {{ detalle }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalServicio',
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  methods: {
    cerrarModalPorFondo(event) {
      // Emitimos evento para cerrar solo si el clic fue en el fondo (modal), no en el contenido
      this.$emit('cerrar');
    }
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0,0,0,0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 50;
  user-select: none;
}

.contenido-modal {
  background: white;
  padding: 25px 40px;
  border-radius: 12px;
  max-width: 600px;
  width: 90%;
  text-align: center;
  position: relative;
  box-shadow: 0 10px 40px rgba(0,0,0,0.3);
}

.cerrar {
  position: absolute;
  top: 15px;
  right: 15px;
  background: transparent;
  border: none;
  font-size: 1.8rem;
  cursor: pointer;
  color: #444;
  user-select: none;
  transition: color 0.3s;
}

.cerrar:hover {
  color: #0077cc;
}

.contenido-modal h2 {
  margin-bottom: 20px;
  font-weight: 700;
  font-size: 2.4rem;
  color: #005f99;
}

.contenido-modal img {
  max-width: 100%;
  max-height: 250px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 20px;
}

.descripcion {
  font-size: 1.2rem;
  color: #333;
  margin-bottom: 15px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  font-size: 1.1rem;
  margin: 8px 0;
  color: #444;
  text-align: left;
  padding-left: 20px;
  position: relative;
}

li::before {
  content: '✔️';
  position: absolute;
  left: 0;
}
</style>
