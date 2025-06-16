<template>
  <header class="encabezado">
    <div class="texto-sin-fondo">
      Servicios para tu Mascota
    </div>
  </header>
</template>

<script>
export default {
  name: "Encabezado"
};
</script>

<style scoped>
.encabezado {
  background-color: #00aaff;
  height: 60px;
  border-bottom: 4px solid #007acc;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  padding: 0;
  margin: 0;
  box-shadow: none;
  outline: none;
  /* Forcing no border or shadow under */
  border: none;
}

.texto-sin-fondo {
  background-color: white;
  color: #00aaff;
  font-weight: bold;
  font-size: 1.5rem;
  height: 100%;
  padding: 0 20px;
  display: flex;
  align-items: center;
  border-radius: 0;
  box-shadow: none;
  user-select: none;
  margin: 0;
  border: none;
  outline: none;
}

/* Reset body margin and padding in global styles if possible */
body {
  margin: 0;
  padding: 0;
}
</style>
