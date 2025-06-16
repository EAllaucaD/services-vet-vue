<template>
  <div class="carrusel">
    <div class="imagen-container">
      <img 
        :src="servicios[actual].src" 
        alt="Servicio" 
        @click="seleccionar(servicios[actual])" 
      />
      <div class="texto-sobre-imagen" @click="seleccionar(servicios[actual])">
        {{ servicios[actual].texto }}
      </div>
    </div>

    <div class="navegacion">
      <span
        v-for="(item, index) in servicios"
        :key="index"
        :class="{ punto: true, activo: index === actual }"
        @click="irA(index)"
      >●</span>
    </div>

    <button class="flecha izq" @click="anterior">‹</button>
    <button class="flecha der" @click="siguiente">›</button>
  </div>
</template>

<script>
export default {
  name: 'Carrusel',
  props: {
    modalAbierto: Boolean
  },
  data() {
    return {
      actual: 0,
      intervalId: null,
      servicios: [
        {
          src: '/img/Vet.jpg',
          texto: 'Consulta Veterinaria',
          descripcion: 'Atención médica general para mascotas con diagnóstico clínico.',
          detalles: [
            '✔️ Examen físico completo',
            '✔️ Diagnóstico y tratamiento',
            '✔️ Recomendaciones de alimentación y vacunas'
          ]
        },
        {
          src: '/img/Consultas.jpg',
          texto: 'Consultas Canina',
          descripcion: 'Consulta especializada en salud y comportamiento de perros.',
          detalles: [
            '✔️ Especialistas en etología',
            '✔️ Evaluación de conducta',
            '✔️ Plan de entrenamiento'
          ]
        },
        {
          src: '/img/peluqueria.jpg',
          texto: 'Peluquería',
          descripcion: 'Baño, corte, cepillado y estilizado de pelaje para todas las razas.',
          detalles: [
            '✔️ Corte higiénico y estético',
            '✔️ Productos dermatológicos',
            '✔️ Atención sin sedación'
          ]
        },
        {
          src: '/img/RayosX.jpg',
          texto: 'Rayos X',
          descripcion: 'Radiografías digitales para diagnóstico de fracturas o enfermedades internas.',
          detalles: [
            '✔️ Imagen digital de alta resolución',
            '✔️ Diagnóstico por especialista',
            '✔️ Resultados en minutos'
          ]
        },
        {
          src: '/img/Urgencias.jpg',
          texto: 'Cirugía Menor',
          descripcion: 'Procedimientos quirúrgicos sencillos realizados bajo anestesia local.',
          detalles: [
            '✔️ Esterilizaciones',
            '✔️ Limpiezas dentales',
            '✔️ Extracción de cuerpos extraños'
          ]
        },
        {
          src: '/img/Consultas.jpg',
          texto: 'Guardería',
          descripcion: 'Cuidado diurno con atención y juegos mientras estás fuera.',
          detalles: [
            '✔️ Supervisión continua',
            '✔️ Actividades recreativas',
            '✔️ Espacio seguro y climatizado'
          ]
        },
        {
          src: '/img/24horas.jpeg',
          texto: 'Urgencias 24h',
          descripcion: 'Atención veterinaria inmediata para emergencias a cualquier hora.',
          detalles: [
            '✔️ Atención sin cita previa',
            '✔️ Personal disponible 24/7',
            '✔️ Transporte veterinario (opcional)'
          ]
        }
      ]
    }
  },
  methods: {
    siguiente() {
      this.actual = (this.actual + 1) % this.servicios.length;
      this.reiniciarIntervalo();
    },
    anterior() {
      this.actual = (this.actual - 1 + this.servicios.length) % this.servicios.length;
      this.reiniciarIntervalo();
    },
    irA(index) {
      this.actual = index;
      this.reiniciarIntervalo();
    },
    seleccionar(item) {
      this.$emit('seleccionar', item);
    },
    iniciarAuto() {
      this.intervalId = setInterval(() => {
        if (!this.modalAbierto) this.siguiente();
      }, 4000);
    },
    reiniciarIntervalo() {
      if (this.intervalId) {
        clearInterval(this.intervalId);
      }
      this.iniciarAuto();
    }
  },
  mounted() {
    this.iniciarAuto();
  },
  beforeUnmount() {
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }
  }
}
</script>

<style scoped>
.carrusel {
  position: relative;
  max-width: 900px;
  margin: auto;
  overflow: hidden;
  text-align: center;
}
.imagen-container {
  position: relative;
}
.imagen-container img {
  width: 100%;
  max-height: 450px;
  object-fit: cover;
  border-radius: 10px;
  cursor: pointer; /* Cambia cursor para indicar que es clickeable */
}
.texto-sobre-imagen {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: rgba(0, 0, 0, 0.5); /* Oscurecer fondo para texto */
  color: #fff;
  font-size: 1.6rem;
  padding: 12px;
  cursor: pointer;
  font-weight: bold;
  border-radius: 0 0 10px 10px;
  transition: background 0.3s;
}
.texto-sobre-imagen:hover {
  background: rgba(0, 0, 0, 0.7);
}
.navegacion {
  margin-top: 10px;
}
.punto {
  font-size: 2rem;
  margin: 0 5px;
  cursor: pointer;
  color: #ccc;
  transition: color 0.3s;
}
.punto.activo {
  color: #00aaff;
}
.flecha {
  position: absolute;
  top: 50%;
  font-size: 2rem;
  background: rgba(255, 255, 255, 0.8);
  border: none;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 50%;
  transform: translateY(-50%);
}
.izq {
  left: 10px;
}
.der {
  right: 10px;
}
</style>
