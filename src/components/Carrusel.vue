<template>
  <div class="carrusel">
    <div class="imagenes-triple">
      <div 
        v-for="(servicio, index) in servicios" 
        :key="index"
        :class="clasePosicion(index)"
        @click="seleccionar(servicio)"
      >
        <img :src="servicio.src" :alt="servicio.texto" />
        <div class="texto-sobre-imagen">{{ servicio.texto }}</div>
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
    },
    clasePosicion(index) {
      // índice anterior y siguiente, con wrap-around
      const anterior = (this.actual - 1 + this.servicios.length) % this.servicios.length;
      const siguiente = (this.actual + 1) % this.servicios.length;

      if (index === this.actual) return 'central';
      if (index === anterior) return 'izquierda';
      if (index === siguiente) return 'derecha';
      return 'oculto'; // los demás no se muestran
    }
  },
  mounted() {
    this.iniciarAuto();
  },
  beforeUnmount() {
    if (this.intervalId) clearInterval(this.intervalId);
  }
}
</script>

<style scoped>
.carrusel {
  position: relative;
  max-width: 900px;
  margin: auto;
  overflow: visible;
  text-align: center;
  height: 480px; /* espacio suficiente para el carrusel */
  user-select: none;
}

.imagenes-triple {
  position: relative;
  height: 450px;
  perspective: 1000px; /* da un efecto 3D */
  display: flex;
  justify-content: center;
  align-items: center;
}

.imagenes-triple > div {
  position: absolute;
  top: 0;
  width: 300px;
  height: 450px;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  transition: all 0.5s ease;
  box-shadow: 0 10px 20px rgba(0,0,0,0.2);
}

.imagenes-triple > div img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  border-radius: 10px;
  user-select: none;
}

/* Posición central: imagen más grande y frontal */
.central {
  left: 50%;
  transform: translateX(-50%) scale(1) translateZ(0);
  z-index: 10;
  opacity: 1;
  box-shadow: 0 15px 30px rgba(0,0,0,0.4);
}

/* Imagen izquierda: más pequeña, ligeramente hacia la izquierda, algo rotada */
.izquierda {
  left: 20%;
  transform: translateX(-50%) scale(0.7) translateZ(-100px) rotateY(25deg);
  opacity: 0.6;
  z-index: 5;
  filter: brightness(0.85);
}

/* Imagen derecha: más pequeña, ligeramente hacia la derecha, algo rotada */
.derecha {
  left: 80%;
  transform: translateX(-50%) scale(0.7) translateZ(-100px) rotateY(-25deg);
  opacity: 0.6;
  z-index: 5;
  filter: brightness(0.85);
}

/* Oculto los demás */
.oculto {
  opacity: 0;
  pointer-events: none;
  transform: scale(0);
  width: 0;
  height: 0;
}

/* Texto sobre la imagen */
.texto-sobre-imagen {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: rgba(0,0,0,0.5);
  color: white;
  font-weight: bold;
  font-size: 1.4rem;
  padding: 10px;
  border-radius: 0 0 10px 10px;
  user-select: none;
  transition: background 0.3s;
}

.texto-sobre-imagen:hover {
  background: rgba(0,0,0,0.7);
}

.navegacion {
  margin-top: 20px;
  user-select: none;
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
  transform: translateY(-50%);
  font-size: 3rem;
  color: #00aaff;
  background: transparent;
  border: none;
  cursor: pointer;
  user-select: none;
  transition: color 0.3s;
  z-index: 20;
}

.flecha:hover {
  color: #0077cc;
}

.flecha.izq {
  left: 5px;
}

.flecha.der {
  right: 5px;
}
</style>
