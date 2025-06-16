<template>
  <div class="modal-fondo" @click.self="$emit('cerrar')">
    <div class="modal">
      <h3 class="titulo-modal">{{ item.texto }}</h3>
      <p class="descripcion">{{ item.descripcion }}</p>
      <ul>
        <li v-for="(detalle, i) in item.detalles" :key="i">{{ detalle }}</li>
      </ul>
      <button @click="$emit('cerrar')">Cerrar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ModalServicio',
  props: {
    item: Object
  }
}
</script>

<style scoped>
.modal-fondo {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center; /* ← CAMBIO IMPORTANTE */
  z-index: 1000;
}

.modal {
  background-color: white;
  padding: 25px 30px;
  border-radius: 12px;
  max-width: 400px;
  width: 90%;
  color: #00aaff;
  font-family: Arial, sans-serif;
  text-align: center;
}

.titulo-modal {
  margin-top: 0;
  margin-bottom: 15px;
  font-size: 1.5rem;
}

.descripcion {
  margin-bottom: 15px;
}

.modal ul {
  list-style: none;
  padding-left: 0;
  margin-bottom: 20px;
}

.modal ul li {
  margin-bottom: 8px;
  font-weight: 600;
}

.modal button {
  background-color: #00aaff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
  display: block;
  margin: 0 auto; /* Centra el botón */
}
</style>
