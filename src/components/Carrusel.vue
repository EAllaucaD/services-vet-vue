<template>
  
    <!-- Huellitas decorativas -->
    <span class="paw paw-top-right"></span>
    <span class="paw paw-bottom-right"></span>
    <span class="paw paw-center-top"></span>
    <span class="paw paw-bottom-left"></span>
    <span class="paw paw-center-bottom"></span>
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

    <button 
      v-if="!modalAbierto" 
      class="flecha izq" 
      @click="anterior"
    >‹</button>
    <button 
      v-if="!modalAbierto" 
      class="flecha der" 
      @click="siguiente"
    >›</button>

    <ModalServicio 
      v-if="modalAbierto" 
      :item="servicioSeleccionado" 
      @cerrar="cerrarModal" 
    />
  </div>
</template>

<script>
import ModalServicio from './ModalServicio.vue'
import pawPrint from '/src/assets/HuellitaAzul.png'  // Importa la imagen de huellita

export default {
  name: 'Carrusel',
  components: { ModalServicio },
  data() {
    return {
      actual: 0,
      intervalId: null,
      modalAbierto: false,
      servicioSeleccionado: null,
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
      if (this.modalAbierto) return;
      this.actual = (this.actual + 1) % this.servicios.length;
      this.reiniciarIntervalo();
    },
    anterior() {
      if (this.modalAbierto) return;
      this.actual = (this.actual - 1 + this.servicios.length) % this.servicios.length;
      this.reiniciarIntervalo();
    },
    irA(index) {
      if (this.modalAbierto) return;
      this.actual = index;
      this.reiniciarIntervalo();
    },
    seleccionar(servicio) {
      this.servicioSeleccionado = servicio;
      this.modalAbierto = true;
      this.reiniciarIntervalo();
    },
    cerrarModal() {
      this.modalAbierto = false;
      this.servicioSeleccionado = null;
      this.reiniciarIntervalo();
    },
    iniciarAuto() {
      this.intervalId = setInterval(() => {
        if (!this.modalAbierto) this.siguiente();
      }, 4000);
    },
    reiniciarIntervalo() {
      if (this.intervalId) clearInterval(this.intervalId);
      this.iniciarAuto();
    },
    clasePosicion(index) {
      const anterior = (this.actual - 1 + this.servicios.length) % this.servicios.length;
      const siguiente = (this.actual + 1) % this.servicios.length;

      if (index === this.actual) return 'central';
      if (index === anterior) return 'izquierda';
      if (index === siguiente) return 'derecha';
      return 'oculto';
    }
  },
  mounted() {
    // Setear la variable CSS --paw-print con la ruta de la imagen importada
    document.documentElement.style.setProperty('--paw-print', `url(${pawPrint})`);
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
  height: 480px;
  user-select: none;
  padding: 0 10px;
  background-color: white;
}



.imagenes-triple {
  position: relative;
  height: 450px;
  perspective: 1000px;
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

.central {
  left: 50%;
  transform: translateX(-50%) scale(1) translateZ(0);
  z-index: 10;
  opacity: 1;
  box-shadow: 0 15px 30px rgba(0,0,0,0.4);
}

.izquierda {
  left: 20%;
  transform: translateX(-50%) scale(0.7) translateZ(-100px) rotateY(25deg);
  opacity: 0.6;
  z-index: 5;
  filter: grayscale(20%);
}

.derecha {
  left: 80%;
  transform: translateX(-50%) scale(0.7) translateZ(-100px) rotateY(-25deg);
  opacity: 0.6;
  z-index: 5;
  filter: grayscale(20%);
}

.oculto {
  opacity: 0;
  pointer-events: none;
  transform: scale(0) translateZ(-200px);
  z-index: 0;
}

.texto-sobre-imagen {
  position: absolute;
  bottom: 15px;
  width: 100%;
  color: white;
  font-weight: 700;
  font-size: 1.4rem;
  text-shadow: 0 0 5px #000;
  user-select: none;
}

/* Navegación puntos */
.navegacion {
  margin-top: 10px;
  user-select: none;
}

.navegacion .punto {
  cursor: pointer;
  font-size: 1.5rem;
  margin: 0 4px;
  color: #888;
  transition: color 0.3s ease;
}

.navegacion .activo {
  color: #2196f3;
}

/* Flechas */
.flecha {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 3rem;
  color: #2196f3;
  border: none;
  background: none;
  cursor: pointer;
  user-select: none;
  z-index: 20;
  transition: color 0.3s ease;
}

.flecha:hover {
  color: #0b7dda;
}

.flecha.izq {
  left: 10px;
}

.flecha.der {
  right: 10px;
}


/* Huellitas */
.paw {
  position: absolute;
  width: 100px;
  height: 100px;
  background-image: var(--paw-print);
  background-repeat: no-repeat;
  background-size: contain;
  pointer-events: none;
  z-index: 0;
}

.paw-top-right {
  top: 60px;     /* más abajo (antes estaba sin unidad: 30 -> 80px) */
  right: 150px;   /* más hacia la izquierda (mayor valor en right = más al centro) */
  transform: rotate(-45deg);
}

.paw-bottom-right {
  bottom: 50px;
  right: 150px;
  transform: rotate(-20deg);
}

.paw-bottom-left {
  bottom: 120px;
  left: 70px;
  transform: rotate(-25deg);
}

.paw-center-top {
  top: 40%;
  left: 80px;
  transform: translateY(-145%) rotate(45deg);
}


.paw-center-bottom {
  bottom: 30px;
  right: 50%;
  transform: translateX(-50%) rotate(50deg);
}
</style>
