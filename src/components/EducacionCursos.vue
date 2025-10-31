<template>
  <section id="educacion" class="contenedor-educacion-cursos">
    <h2 class="titulo-seccion">Educación y Cursos</h2>

    <div class="contenedor-interactivo-tiempo">
      <div class="contenedor-anos-tiempo">
        <div
          v-for="(item, indice) in educacion"
          :key="indice"
          :class="['boton-ano', { activo: indiceActivo === indice }]"
          :style="{ '--accent-color': coloresItems[indice % coloresItems.length].color }"
          @click="alternarActivo(indice)"
        >
          <span :class="{ 'pulso-neon': indiceActivo === -1 }">
            {{ item.fecha }}
          </span>
        </div>
      </div>

      <div class="contenedor-muestra-contenido-tiempo">
        <div v-if="indiceActivo === -1" class="contenedor-contenido tarjeta-bienvenida">
          <div class="grupo-titulo-muestra">
            <h3 class="titulo titulo-bienvenida">¡Bienvenido!</h3>
          </div>

          <div class="descripcion">Explora mi trayectoria academica.</div>

          <button
            class="boton-accion-nuevo pulso-enlace"
            :style="{ '--clr': coloresItems[0].color }"
            @click="manejarClicEmpezar($event)"
          >
            <span>Empezar {{ educacion[0].fecha }}</span>

            <i></i>
          </button>
        </div>

        <div
          v-else-if="itemActivo"
          :key="itemActivo.fecha"
          class="contenedor-contenido tarjeta-contenido-activa"
          :style="{ '--accent-color': coloresItems[indiceActivo % coloresItems.length].color }"
        >
          <button
            class="boton-cerrar"
            @click="
              generarParticulas($event);
              establecerActivo(-1);
            "
          >
            [ X ]
          </button>

          <div class="grupo-titulo-muestra">
            <h3 class="titulo">{{ itemActivo.title }}</h3>

            <span class="fecha-muestra-movil">{{ itemActivo.fecha }}</span>
          </div>

          <div class="descripcion">{{ itemActivo.descripcion }}</div>

          <a
            class="boton-accion-nuevo enlace-accion"
            :href="itemActivo.enlace"
            target="_blank"
            :style="{ '--clr': coloresItems[indiceActivo % coloresItems.length].color }"
            @click="
              generarParticulas($event);
              establecerActivo(-1);
            "
          >
            <span>
              {{ itemActivo.enlaceTexto }}
            </span>

            <i></i>
          </a>
        </div>
      </div>
    </div>

    <div class="contenedor-btn-regreso-neon">
      <button 
        @click="volverArriba" 
        class="btn-flotante-volver-arriba"
        aria-label="Volver a Navegación Principal"
      >
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 10.5 12 3m0 0 7.5 7.5M12 3v18" />
        </svg>
      </button>
    </div>

    <div id="contenedor-particulas" ref="contenedorParticulas"></div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";

// Método para hacer scroll a la parte superior de la ventana (donde está la barra de navegación)
const volverArriba = () => {
  window.scrollTo({
    top: 0, 
    behavior: 'smooth' 
  });
};

const indiceActivo = ref(-1);

const establecerActivo = (indice) => {
  indiceActivo.value = indice;
};

const alternarActivo = (indice) => {
  if (indiceActivo.value === indice) {
    indiceActivo.value = -1;
  } else {
    indiceActivo.value = indice;
  }
};

// --- Template Ref para el contenedor de partículas ---

const contenedorParticulas = ref(null);

// --- Lógica del Efecto de Partículas ---

const generarParticulas = (evento) => {
  const contenedor = contenedorParticulas.value;

  if (!contenedor) return;

  // 1. Obtener la posición del clic (o centro del botón) en la ventana

  const rectBoton = evento.currentTarget.getBoundingClientRect();

  const clickX = rectBoton.left + rectBoton.width / 2;

  const clickY = rectBoton.top + rectBoton.height / 2;

  // 2. Obtener la posición del contenedor de partículas

  const rectContenedor = contenedor.getBoundingClientRect();

  // 3. Calcular la posición inicial de la partícula relativa al contenedor

  const centroX = clickX - rectContenedor.left;

  const centroY = clickY - rectContenedor.top;

  const cantidadParticulas = Math.floor(Math.random() * 8) + 8;

  const colores = ["var(--vt-c-cyan)", "var(--vt-c-indigo)", "var(--vt-c-magenta)"];


  for (let i = 0; i < cantidadParticulas; i++) {
    const particula = document.createElement("span");

    particula.className = "particula-estrella";

    // Aplicar la posición inicial calculada

    particula.style.left = `${centroX}px`;

    particula.style.top = `${centroY}px`;

    const tamano = Math.random() * 3 + 1;

    particula.style.width = `${tamano}px`;

    particula.style.height = `${tamano}px`;

    const colorParticula = colores[Math.floor(Math.random() * colores.length)];

    particula.style.backgroundColor = colorParticula;

    particula.style.setProperty("--particula-color", colorParticula);

    // Dispersión

    const dx = (Math.random() - 0.5) * 150;

    const dy = (Math.random() - 0.5) * 150;

    const duracion = Math.random() * 0.8 + 0.4;

    particula.style.setProperty("--dx", `${dx}px`);

    particula.style.setProperty("--dy", `${dy}px`);

    particula.style.animationDuration = `${duracion}s`;

    contenedor.appendChild(particula);

    particula.onanimationend = () => {
      particula.remove();
    };
  }
};

const manejarClicEmpezar = (evento) => {
  generarParticulas(evento);

  alternarActivo(0);
};

// --- Datos y Estilos ---

const coloresItems = ref([
  { color: "#39FF14" }, // Verde Neón

  { color: "#0FF0FC" }, // Azul/Cian Neón

  { color: "#FF44CC" }, // Rosa/Magenta Neón

  { color: "#8A2BE2" }, // Morado
]);

const educacion = ref([
  {
    fecha: "2025",

    title: "Técnico Universitario en Programación ",

    descripcion:
      "Foco en desarrollo de software, análisis de sistemas, y metodologías ágiles. Preparación para soluciones Full Stack.",

    enlace: "https://utn.edu.ar/frsr/",

    enlaceTexto: "Ver Título",
  },

  {
    fecha: "2024",

    title: "Manejo Avanzado de Bases de Datos ",

    descripcion:
      "Estudio intensivo en consultas complejas, optimización de rendimiento y diseño de esquemas relacionales.",

    enlace: "https://ejemplo.com/certificado-sql",

    enlaceTexto: "Ver Certificado ",
  },

  {
    fecha: "2023",

    title: "Fundamentos de JavaScript y Backend",

    descripcion:
      "Adquisición de bases de programación y lógica del lado del servidor para el desarrollo de APIs web.",

    enlace: "https://ejemplo.com/certificado-js",

    enlaceTexto: "Ver Certificado ",
  },

  {
    fecha: "2022",

    title: "Introducción a Sistemas Operativos",

    descripcion:
      "Comprensión de la arquitectura, gestión de memoria y procesos en sistemas operativos como Linux y Windows.",

    enlace: "#",

    enlaceTexto: "Ver Certificado",
  },
]);

const itemActivo = computed(() =>
  indiceActivo.value !== -1 ? educacion.value[indiceActivo.value] : null
);

</script>

<style scoped>

/* VARIABLES Y ESTILOS BASE */

:root {
  --vt-c-cyan: #00ffff;
  --vt-c-indigo: #a855f7;
  --vt-c-magenta: #ff00ff;
  --vt-c-black: #03091b;
  --vt-c-white: #ebe0ff;
  --vt-c-black-soft: #151b33;
  --vt-c-black-mute: #283049;
  --bgColor: var(--vt-c-black-soft);
  --neon-green: #39FF14; /* Color para el botón de retorno */
  --neon-blue: #0FF0FC; /* Añadido para el efecto de luz */
  --neon-pink: #FF44CC; /* Añadido para el efecto de luz */


  --welcome-gradient-border: linear-gradient(
    90deg,
    var(--vt-c-indigo) 0%,
    var(--vt-c-cyan) 50%,
    var(--vt-c-magenta) 100%
  );

  --welcome-title-color: var(--vt-c-indigo);

  /* Variable específica para el fondo interno del nuevo botón */
  --btn-inner-bg: #272822;
}

/* 1. Contenedor Principal de Educación */
.contenedor-educacion-cursos {
  padding: 2rem 1rem;
  max-width: 100%; 
  display: flex;
  flex-direction: column;
  /* CAMBIO CRUCIAL: Posicionamiento relativo para que el botón absoluto funcione dentro */
  position: relative; 
  text-align: center; 
}

.titulo-seccion {
  font-family: "Orbitron", sans-serif;
  font-size: 2.2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 2rem;
  color: var(--vt-c-indigo);
  text-shadow: 0 0 5px var(--vt-c-indigo), 0 0 15px rgba(168, 85, 247, 0.5);
  border-bottom: 2px solid var(--vt-c-black-mute);
  padding-bottom: 0.5rem;
}

/* Contenedor de los Años */

.contenedor-anos-tiempo {
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
  /*minimo ancho para la línea de tiempo horizontal. Si el viewport es más pequeño, activa el scroll interno del padre. */
  min-width: 650px;/* Asegura un mínimo para que los 4 botones se separen bien */
  width: 100%;
  align-items: flex-end;
  padding-bottom: 1rem;
  border-bottom: 3px dashed var(--vt-c-cyan);
}

.boton-ano {
  font-family: "Orbitron", sans-serif;
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--vt-c-white);
  padding: 0.5rem 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  background: var(--bgColor);
  border: 2px dashed #ce5ece;
  box-shadow: 0 0 10px rgba(52, 8, 228, 0.7);

  /* Aseguramos que no crezcan más de lo necesario */

  flex-shrink: 0;
}

.pulso-neon {
  display: inline-block;
  animation: neon-pulse 1.5s infinite alternate ease-in-out;
}

@keyframes neon-pulse {
  from {
    text-shadow: 0 0 5px var(--vt-c-white), 0 0 10px var(--vt-c-white);
    opacity: 0.8;
  }

  to {
    text-shadow: 0 0 15px var(--vt-c-cyan), 0 0 25px var(--vt-c-cyan);
    opacity: 1;
  }
}

.boton-ano:hover,
.boton-ano.activo {
  color: var(--vt-c-black);
  transform: translateY(-5px);
  background-color: var(--accent-color);
  box-shadow: 0 0 5px var(--accent-color), 0 0 15px var(--accent-color),
    0 5px 20px rgba(0, 0, 0, 0.8);
}

.boton-ano.activo .pulso-neon {
  animation: none;
}

.boton-ano::after {
  content: "";
  position: absolute;
  bottom: -1.7rem;
  left: 50%;
  transform: translateX(-50%);
  width: 1rem;
  height: 1rem;
  background: var(--vt-c-black);
  border: 0.25rem solid var(--accent-color);
  border-radius: 4px;
  box-shadow: 0 0 8px var(--accent-color);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  animation: rotate 3s linear infinite;
}

@keyframes rotate {
  from {
    transform: translateX(-50%) rotate(0deg);
  }

  to {
    transform: translateX(-50%) rotate(360deg);
  }
}

.boton-ano.activo::after {
  box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
}

.contenedor-muestra-contenido-tiempo {
  display: flex;
  justify-content: center;
  padding-top: 1rem;
}

/* Sombra Neón a la Tarjeta de Contenido Activa */

.contenedor-contenido {
  background: var(--bgColor);
  padding: 1.5rem;
  border-radius: 20px;

  /* ESTILOS BASE */

  border: 2px solid var(--accent-color);
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.9), inset 0 0 5px var(--accent-color);
  animation: fadeIn 0.5s ease-out;
  width: 90%;
  max-width: 600px;
  position: relative;
}

/* Aplico la sombra neon dinamica para la tarjeta activa */

.tarjeta-contenido-activa {
  box-shadow: 0 0 5px var(--accent-color), 0 0 15px rgba(0, 0, 0, 0.9), 0 0 30px rgba(0, 0, 0, 0.8),
    inset 0 0 5px var(--accent-color), 0 0 20px var(--accent-color);
}

/* Tarjeta de Bienvenida: Estilo de Borde Degradado */

.tarjeta-bienvenida {
  border: none;
  padding: 2px;
  background: var(--welcome-gradient-border) border-box;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(168, 85, 247, 0.2), 0 0 20px rgba(0, 255, 255, 0.2),
    inset 0 0 10px rgba(0, 0, 0, 0.5);
}

/* Estilos internos de la tarjeta */

.tarjeta-bienvenida > .grupo-titulo-muestra,
.tarjeta-bienvenida > .descripcion {
  background: var(--bgColor);
  border-radius: 8px;
}

.tarjeta-bienvenida > .grupo-titulo-muestra {
  padding-top: 0.5rem;
}

.tarjeta-bienvenida > .descripcion {
  padding-block: 1rem 1.5rem;
}

.titulo-bienvenida {
  color: var(--welcome-title-color);
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.grupo-titulo-muestra {
  padding-bottom: 0.5rem;
  border-bottom: 1px dashed var(--vt-c-black-mute);
}

.titulo {
  font-weight: 700;
  font-size: 1.5rem;
  color: var(--vt-c-cyan);
  font-family: "Orbitron", sans-serif;
  text-align: center;
  filter: brightness(0.85);
}

.fecha-muestra-movil {
  display: none;
}

.descripcion {
  padding-block: 1rem 1.5rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white);
  line-height: 1.4;
  text-align: center;
}

/* ============================================== */

/* 3. ----Estilos del boton----*/

/* Botón base */

.boton-accion-nuevo {
  position: relative;
  background: var(--btn-inner-bg);
  color: #fff;
  text-decoration: none;
  text-transform: uppercase;
  border: none;
  letter-spacing: 0.05rem;
  font-size: 0.9rem;
  padding: 0.7rem 2rem;
  transition: 0.2s;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60%;
  max-width: 200px;
  margin: 0.8rem auto 0;
  border-radius: 0;
  font-family: "Orbitron", sans-serif;
}

/* Estado Hover/Focus */

.boton-accion-nuevo:hover,
.boton-accion-nuevo:focus {
  letter-spacing: 0.1rem;
  padding: 0.8rem 2.1rem;
  background: var(--clr);
  color: var(--clr);
  animation: box 3s infinite;
  outline: none;
}

/* Fondo interno */

.boton-accion-nuevo::before {
  content: "";
  position: absolute;
  inset: 2px;
  background: var(--btn-inner-bg);
  transition: 0.2s;
  z-index: 0;
}

.boton-accion-nuevo:hover::before {
  background: #18191f;
}

/* Texto (span) */

.boton-accion-nuevo span {
  position: relative;
  z-index: 1;
}

/* Contenedor del efecto de borde (i) */

.boton-accion-nuevo i {
  position: absolute;
  inset: 0;
  display: block;
  z-index: 2;
}

/* Tiras superiores/izquierdas */

.boton-accion-nuevo i::before {
  content: "";
  position: absolute;
  width: 8px;
  height: 2px;
  left: 80%;
  top: -2px;
  border: 2px solid var(--clr);
  background: var(--btn-inner-bg);
  transition: 0.2s;
}

.boton-accion-nuevo:hover i::before {
  width: 12px;
  left: 20%;
  animation: move 3s infinite;
}

/* Tiras inferiores/derechas */

.boton-accion-nuevo i::after {
  content: "";
  position: absolute;
  width: 8px;
  height: 2px;
  left: 20%;
  bottom: -2px;
  border: 2px solid var(--clr);
  background: var(--btn-inner-bg);
  transition: 0.2s;
}

.boton-accion-nuevo:hover i::after {
  width: 12px;

  left: 80%;

  animation: move 3s infinite;
}

/* Animaciones */

@keyframes move {
  0% {
    transform: translateX(0);
  }

  50% {
    transform: translateX(5px);
  }

  100% {
    transform: translateX(0);
  }
}

@keyframes box {
  0% {
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
  }

  50% {
    box-shadow: 0 0 20px var(--clr);
  }

  100% {
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
  }
}

.pulso-enlace {
  animation: pulso-btn-accion 2s infinite alternate ease-in-out;
}

@keyframes pulso-btn-accion {
  from {
    box-shadow: 0 0 5px var(--clr);

    transform: scale(1);
  }

  to {
    box-shadow: 0 0 10px var(--clr), 0 0 20px var(--clr);

    transform: scale(1.01);
  }
}

/* --- Botón de Cerrar [X] --- */

.boton-cerrar {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background-color: transparent;
  border: none;
  color: var(--vt-c-magenta);
  font-size: 1.2rem;
  font-weight: bold;
  cursor: pointer;
  transition: color 0.3s, transform 0.3s;
  z-index: 20;
}

.boton-cerrar:hover {
  color: var(--vt-c-magenta);

  text-shadow: 0 0 5px var(--vt-c-magenta);

  transform: scale(1.1) rotate(5deg);
}

/* ============================================== */

/* 4.----- Efecto de Partículas---- */



#contenedor-particulas {
  position: absolute;

  top: 0;

  left: 0;

  width: 100%;

  height: 100%;

  pointer-events: none;

  overflow: hidden;

  z-index: 5;
}

.particula-estrella {
  position: absolute;

  transform: rotate(45deg);

  border-radius: 1px;

  opacity: 1;

  filter: drop-shadow(0 0 1px var(--particula-color)) drop-shadow(0 0 4px var(--particula-color));

  animation: star-fly 1s forwards ease-out;
}

@keyframes star-fly {
  0% {
    opacity: 1;

    transform: translate(0, 0) rotate(45deg) scale(1);
  }

  20% {
    opacity: 1;
  }

  100% {
    transform: translate(var(--dx), var(--dy)) rotate(45deg) scale(0);

    opacity: 0;
  }
}

/* ------------------------------------------------------------- */
/* 5. CONTENEDOR CIRCULAR para el BOTÓN "Volver Arriba" (¡CORREGIDO!) */
/* ------------------------------------------------------------- */
.contenedor-btn-regreso-neon {
    /* ✨ CAMBIO CLAVE: position: absolute ✨ */
    position: absolute; 
    bottom: 30px; /* Distancia desde abajo de la sección */
    right: 30px; /* Distancia desde la derecha de la sección */
    /* Z-index bajo porque está dentro del flujo del documento */
    z-index: 10; 

    /* Dimensiones y forma del contenedor (Mantenidas) */
    width: 65px; 
    height: 65px;
    border-radius: 50%; 
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    
    /* Fondo y borde para que se destaque (Mantenidos) */
    background: radial-gradient(circle at 50% 50%, #2a2a2e 0%, var(--bgColor) 100%);
    border: 2px solid var(--neon-green);
    
    /* Múltiples sombras de neón animadas (Mantenidas) */
    box-shadow: 
        0 0 10px rgba(57, 255, 20, 0.7), 
        0 0 25px var(--neon-blue), 
        0 0 40px var(--neon-pink); 
    animation: container-neon-pulse 3.5s infinite alternate ease-in-out; 
    transition: all 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

/* Animación de luz y color para el CONTENEDOR (Mantenida) */
@keyframes container-neon-pulse {
    0% {
        box-shadow: 
            0 0 10px rgba(57, 255, 20, 0.7), 
            0 0 25px var(--neon-blue), 
            0 0 40px var(--neon-pink);
        border-color: var(--neon-green);
        transform: scale(1) translateY(0);
    }
    33% {
        box-shadow: 
            0 0 15px var(--neon-blue), 
            0 0 30px var(--neon-pink), 
            0 0 50px var(--neon-green);
        border-color: var(--neon-blue);
        transform: scale(1.02) translateY(-2px);
    }
    66% {
        box-shadow: 
            0 0 12px var(--neon-pink), 
            0 0 28px var(--neon-green), 
            0 0 45px var(--neon-blue);
        border-color: var(--neon-pink);
        transform: scale(0.98) translateY(2px);
    }
    100% {
        box-shadow: 
            0 0 10px rgba(57, 255, 20, 0.7), 
            0 0 25px var(--neon-blue), 
            0 0 40px var(--neon-pink);
        border-color: var(--neon-green);
        transform: scale(1) translateY(0);
    }
}

.contenedor-btn-regreso-neon:hover {
    border-color: var(--vt-c-white); 
    box-shadow: 
        0 0 15px var(--neon-green), 
        0 0 30px var(--neon-green), 
        0 0 60px var(--neon-green); 
    transform: scale(1.08); 
    animation: none; 
}


/* Estilos del BOTÓN INTERNO (Simplificado) (Mantenidos) */
.btn-flotante-volver-arriba {
    width: 50px; 
    height: 50px;
    border-radius: 50%; 
    background: transparent; 
    border: none; 
    color: var(--neon-green); 
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 0 5px rgba(57, 255, 20, 0.3); 
}

.btn-flotante-volver-arriba:hover {
    color: var(--vt-c-white); 
    box-shadow: 0 0 8px rgba(255, 255, 255, 0.5); 
    transform: scale(1.05); 
}

.btn-flotante-volver-arriba svg {
    width: 26px; 
    height: 26px;
    transition: transform 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55); 
    transform: translateY(0px) rotateZ(0deg); 
}

.btn-flotante-volver-arriba:hover svg {
    transform: translateY(-8px) rotateZ(5deg); 
    filter: drop-shadow(0 0 5px var(--vt-c-white)); 
}


/* 6.--- RESPONSIVE MÓVIL/TABLET ----*/

@media (max-width: 768px) {
  .contenedor-educacion-cursos {
    padding: 1rem;
  }

  .contenedor-interactivo-tiempo {
    flex-direction: column;

    gap: 1.5rem;

    overflow-x: hidden;
  }

  /* MODO MÓVIL para los botones de año */

  .contenedor-anos-tiempo {
    flex-direction: column;

    align-items: center;

    border-bottom: none;

    padding-bottom: 0;

    min-width: auto;

    width: 100%;
  }

  .boton-ano {
    width: 80%;

    max-width: 300px;

    font-size: 1rem;

    position: relative;

    margin-bottom: 0.5rem;

    transform: none !important;

    flex-shrink: 1;
  }

  .boton-ano:hover {
    transform: scale(1.03) !important;
  }

  .boton-ano::after {
    content: none;
  }

  .contenedor-contenido {
    padding: 1rem;

    width: 95%;

    max-width: none;
  }

  .tarjeta-bienvenida {
    display: block;
  }

  .titulo {
    font-size: 1.2rem;
  }

  .descripcion {
    font-size: 0.9rem;
  }

  /* Ajuste de botón en móvil */

  .boton-accion-nuevo {
    width: 90%;

    max-width: 250px;

    padding: 0.6rem 1rem;

    font-size: 0.75rem;

    letter-spacing: 0.05rem;
  }

  .boton-accion-nuevo:hover,
  .boton-accion-nuevo:focus {
    padding: 0.7rem 1.1rem;

    letter-spacing: 0.1rem;
  }

  /* Ajuste del contenedor flotante para móviles */
  .contenedor-btn-regreso-neon {
    /* Mantiene absolute */
    bottom: 20px;
    right: 20px;
    width: 55px; 
    height: 55px;
    box-shadow: 
        0 0 8px rgba(57, 255, 20, 0.5), 
        0 0 15px var(--neon-blue), 
        0 0 25px var(--neon-pink); 
    animation: container-neon-pulse 3.5s infinite alternate ease-in-out; 
  }

  .contenedor-btn-regreso-neon:hover {
      transform: scale(1.05); 
  }

  /* Ajuste del botón interno para móviles */
  .btn-flotante-volver-arriba {
    width: 40px;
    height: 40px;
  }

  .btn-flotante-volver-arriba svg {
    width: 22px;
    height: 22px;
  }

  .btn-flotante-volver-arriba:hover svg {
    transform: translateY(-6px) rotateZ(3deg); 
  }
}
</style>