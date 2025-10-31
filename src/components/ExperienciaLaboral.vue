<template>
  <section id="experiencia" class="contenedor-experiencia">
    
    <h2 class="titulo-seccion">Experiencia Laboral</h2>

    <div class="contenedor-interactivo-tiempo">

      <div class="contenedor-anos-experiencia">
        <div
          v-for="(item, indice) in experiencia"
          :key="indice"
          :class="['boton-etapa', { activo: indiceActivo === indice }]"
          :style="{ '--accent-color': coloresItems[indice % coloresItems.length].color }"
          @click="alternarActivo(indice)"
        >
          <span :class="{ 'pulso-neon': indiceActivo === -1 }">
            {{ item.empresa }}
          </span>
        </div>
      </div>

      <div class="contenedor-muestra-contenido-experiencia">
        <div v-if="indiceActivo === -1" class="contenedor-contenido tarjeta-bienvenida">
          <div class="grupo-titulo-muestra">
            <h3 class="titulo titulo-bienvenida">Trayectoria Profesional</h3>
          </div>
          <div class="descripcion">
            Haz clic en una empresa para ver los detalles de mi rol y logros.
          </div>
          <button
            class="boton-accion-nuevo pulso-enlace"
            :style="{ '--clr': coloresItems[0].color }"
            @click="manejarClicEmpezar($event)"
          >
            <span>Ver mi último rol</span>
            <i></i>
            <i></i>
            <i></i>
            <i></i>
          </button>
        </div>

        <div
          v-else-if="itemActivo"
          :key="itemActivo.empresa"
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
            <h3 class="titulo">{{ itemActivo.rol }}</h3>
            <span class="subtitulo-detalle">@ {{ itemActivo.empresa }} ({{ itemActivo.fecha }})</span>
          </div>

          <div class="descripcion-scrollable">
            <p>{{ itemActivo.resumen }}</p>
            <ul class="lista-logros">
              <li v-for="(logro, i) in itemActivo.logros" :key="i">
                <span class="logro-bullet" :style="{ 'color': coloresItems[indiceActivo % coloresItems.length].color }">◆</span> {{ logro }}
              </li>
            </ul>
          </div>
          <a
            class="boton-accion-nuevo enlace-accion"
            :href="itemActivo.enlace"
            target="_blank"
            :style="{ '--clr': coloresItems[indiceActivo % coloresItems.length].color }"
            @click="generarParticulas($event)"
          >
            <span>{{ itemActivo.enlaceTexto }}</span>
            <i></i>
            <i></i>
            <i></i>
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
    <div id="contenedor-particulas-experiencia" ref="contenedorParticulas"></div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";

// FUNCIÓN AGREGADA: Volver Arriba
const volverArriba = () => {
  window.scrollTo({
    top: 0, 
    behavior: 'smooth' 
  });
};


const indiceActivo = ref(-1);

const establecerActivo = (indice) => { indiceActivo.value = indice; };
const alternarActivo = (indice) => {
  if (indiceActivo.value === indice) {
    indiceActivo.value = -1;
  } else {
    indiceActivo.value = indice;
  }
};
const manejarClicEmpezar = (evento) => {
  generarParticulas(evento);
  alternarActivo(0);
};

// --- Lógica del Efecto de Partículas (Mantenida) ---
const contenedorParticulas = ref(null);
const generarParticulas = (evento) => {
  const contenedor = contenedorParticulas.value;
  if (!contenedor) return;

  const rectBoton = evento.currentTarget.getBoundingClientRect();
  const clickX = rectBoton.left + rectBoton.width / 2;
  const clickY = rectBoton.top + rectBoton.height / 2;
  const rectContenedor = contenedor.getBoundingClientRect();
  const centroX = clickX - rectContenedor.left;
  const centroY = clickY - rectContenedor.top;
  
  const cantidadParticulas = Math.floor(Math.random() * 8) + 8;
  const colores = [coloresItems.value[0].color, coloresItems.value[1].color, coloresItems.value[2].color];

  for (let i = 0; i < cantidadParticulas; i++) {
    const particula = document.createElement("span");
    particula.className = "particula-estrella-exp"; 
    particula.style.left = `${centroX}px`;
    particula.style.top = `${centroY}px`;
    
    const tamano = Math.random() * 3 + 1;
    particula.style.width = `${tamano}px`;
    particula.style.height = `${tamano}px`;
    
    const colorParticula = colores[Math.floor(Math.random() * colores.length)];
    particula.style.backgroundColor = colorParticula;
    particula.style.setProperty("--particula-color", colorParticula);

    const dx = (Math.random() - 0.5) * 150;
    const dy = (Math.random() - 0.5) * 150;
    const duracion = Math.random() * 0.8 + 0.4;

    particula.style.setProperty("--dx", `${dx}px`);
    particula.style.setProperty("--dy", `${dy}px`);
    particula.style.animationDuration = `${duracion}s`;

    contenedor.appendChild(particula);
    particula.onanimationend = () => { particula.remove(); };
  }
};


// --- Datos de Experiencia (Mantenidos) ---
const coloresItems = ref([
  { color: "#39FF14" }, // Verde Neón
  { color: "#FF44CC" }, // Rosa/Magenta Neón
  { color: "#0FF0FC" }, // Azul/Cian Neón
  { color: "#A855F7" }, // Morado
]);

const experiencia = ref([
  {
    fecha: "2025 - Actualidad",
    empresa: "Bytenine Team (Estudio)",
    rol: "Colaborador & Desarrollador Frontend/Backend Jr.",
    resumen: "Participación activa en un entorno de aprendizaje y desarrollo colaborativo. Este es un texto largo de prueba para simular que hay mucho contenido y el scroll interno debería activarse, permitiendo al usuario leer toda la descripción de mi rol y logros sin que la página principal tenga que desplazarse. Esto asegura una experiencia de usuario fluida y coherente con el diseño de pantalla completa.",
    logros: [
      "Diseño y prototipado de interfaces web creativas utilizando JavaScript/Vue(detalles estéticos y UX)prestando especial atención a la estética y la experiencia del usuario (UX).",
      "Implementación de lógica backend básica con Java/Python para manejar la persistencia de datos de proyectos internos.",
      "Optimización de la carga inicial de componentes de Vue en un 30%"
    ],
    enlace: "https://github.com/HotCode2025/ByteNine-Segundo-Semestre", 
    enlaceTexto: "Ver Proyectos del Equipo",
  },
  {
    fecha: "2022 - 2024",
    empresa: "Winery Optimizers ",
    rol: "Asistente de Optimización de Procesos (IT Focus)",
    resumen: "Rol de transición, transformando la experiencia en administración/contable en un enfoque IT. Dedicado a la identificación de ineficiencias y la propuesta de soluciones basadas en el análisis de datos para mejorar los flujos de trabajo. **Perfeccionista en la búsqueda de la eficiencia.** Aquí también colocamos texto adicional largo para probar el scroll interno y confirmar que funciona correctamente en esta segunda entrada, manteniendo la coherencia visual.",
    logros: [
      "Implementación de macros y scripts en JavaScript para optimizar la entrada y validación de datos contables",
      "Creación de material de capacitación detallado para el personal sobre las nuevas herramientas digitales, asegurando una adopción fluida."
    ],
    enlace: "#",
    enlaceTexto: "Análisis de Datos",
  },
  {
    fecha: "2020 - 2022",
    empresa: "Digital Art Studio ",
    rol: "Asistente de Producción Creativa",
    resumen: "Apoyo en la gestión de activos digitales y la optimización de flujos de trabajo creativos.",
    logros: [
      "Desarrollo de pequeños programas en Python para automatizar el cambio de formato y tamaño de imágenes",
      "Gestión de repositorios y control de versiones para los activos creativos, facilitando la colaboración en equipo."
    ],
    enlace: "#",
    enlaceTexto: "Ver Portafolio de Arte",
  },
]);

const itemActivo = computed(() =>
  indiceActivo.value !== -1 ? experiencia.value[indiceActivo.value] : null
);
</script>

<style scoped>

/*----- VARIABLES Y ANIMACIONES (Mantenidas) ----- */
:root {
  --vt-c-cyan: #00ffff;
  --vt-c-indigo: #a855f7;
  --vt-c-magenta-original: #ff1493;
  --vt-c-black: #03091b;
  --vt-c-white: #ebe0ff;
  --vt-c-white-soft: #c8c8c8; 
  --vt-c-black-soft: #151b33;

  /* VARIABLES AGREGADAS para el botón de regreso */
  --neon-green: #39FF14; 
  --neon-blue: #0FF0FC; 
  --neon-pink: #FF44CC; 
}
@keyframes neon-pulse {
  from { text-shadow: 0 0 5px var(--vt-c-white), 0 0 10px var(--vt-c-white); opacity: 0.8; }
  to { text-shadow: 0 0 15px var(--vt-c-indigo), 0 0 25px var(--vt-c-indigo); opacity: 1; } 
}
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes active-neon-pulse {
    0% { box-shadow: 0 0 5px var(--accent-color), 0 0 15px var(--accent-color); }
    50% { box-shadow: 0 0 15px var(--accent-color), 0 0 30px var(--accent-color); }
    100% { box-shadow: 0 0 5px var(--accent-color), 0 0 15px var(--accent-color); }
}
@keyframes move {
    0% { transform: translate(0); }
    100% { transform: translate(100%); }
}
@keyframes pulso-enlace-anim {
    0% { filter: brightness(1) drop-shadow(0 0 5px var(--clr)); }
    50% { filter: brightness(1.2) drop-shadow(0 0 10px var(--clr)); }
    100% { filter: brightness(1) drop-shadow(0 0 5px var(--clr)); }
}
@keyframes star-fly-exp {
  0% { opacity: 1; transform: translate(0, 0) rotate(45deg) scale(1); }
  20% { opacity: 1; }
  100% { transform: translate(var(--dx), var(--dy)) rotate(45deg) scale(0); opacity: 0; }
}


/* ------------------------------------------------------------- */
/* LAYOUT PRINCIPAL */
/* ------------------------------------------------------------- */
.contenedor-experiencia {
  padding: 0; 
  max-width: 100%;
  /* ✨ CAMBIO CRUCIAL 1: Añadir position: relative para que el botón se posicione dentro. */
  position: relative; 
  text-align: center;
  
  height: 100vh; 
  display: flex;
  justify-content: center; 
  align-items: center;
  flex-direction: column;
}

.titulo-seccion {
  position: absolute;
  top: 2rem; 
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
  margin-bottom: 0; 
  
  font-family: "Orbitron", sans-serif;
  font-size: 2.5rem;
  font-weight: 700;
  text-align: center;
  color: var(--vt-c-cyan);
  text-shadow: 0 0 5px var(--vt-c-cyan), 0 0 15px rgba(0, 255, 255, 0.5);
  padding-bottom: 0.5rem;
  border-bottom: 2px solid rgba(168, 85, 247, 0.2);
}

.contenedor-interactivo-tiempo {
  display: flex;
  flex-direction: column; 
  align-items: center;
  gap: 2rem;
  max-height: 85vh; 
  width: 95%; 
  max-width: 1400px; 
}


/* ------------------------------------------------------------- */
/* BOTONES DE ACCIÓN (CRÍTICO: Animación de borde) */
/* ------------------------------------------------------------- */
/* Botón de Acción Principal (Usado para 'Ver...' y 'Empezar') */
.boton-accion-nuevo {
  /* Variables locales para el color del botón, usando --clr pasado por Vue */
  --clr: var(--clr); 
  --inner-bg: #151b33; 
  
  position: relative;
  display: inline-block;
  padding: 0.8rem 2.5rem;
  margin-top: 1.5rem;
  color: var(--clr);
  text-decoration: none;
  font-size: 1rem;
  font-family: 'Orbitron', sans-serif;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  overflow: hidden; 
  transition: 0.5s;
  background: var(--inner-bg);
  border: none;
  cursor: pointer;
  border-radius: 5px;
  z-index: 10;
}
.boton-accion-nuevo span {
    position: relative;
    z-index: 1; 
}
.pulso-enlace {
    animation: pulso-enlace-anim 2s infinite ease-in-out;
}
.boton-accion-nuevo:hover {
    color: var(--vt-c-black-soft); 
    background: var(--clr);
    box-shadow: 0 0 10px var(--clr), 0 0 25px var(--clr), 0 0 50px var(--clr);
}
.boton-accion-nuevo i {
    position: absolute;
    display: block;
}
/* Top */
.boton-accion-nuevo i:nth-child(1) {
    top: 0;
    left: 0;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--clr));
    animation: move 1s linear infinite;
}
/* Right */
.boton-accion-nuevo i:nth-child(2) {
    top: -100%;
    right: 0;
    width: 2px;
    height: 100%;
    background: linear-gradient(180deg, transparent, var(--clr));
    animation: move 1s linear infinite;
    animation-delay: 0.25s;
}
/* Bottom */
.boton-accion-nuevo i:nth-child(3) {
    bottom: 0;
    right: 0;
    width: 100%;
    height: 2px;
    background: linear-gradient(270deg, transparent, var(--clr));
    animation: move 1s linear infinite;
    animation-delay: 0.5s;
}
/* Left */
.boton-accion-nuevo i:nth-child(4) {
    bottom: -100%;
    left: 0;
    width: 2px;
    height: 100%;
    background: linear-gradient(360deg, transparent, var(--clr));
    animation: move 1s linear infinite;
    animation-delay: 0.75s;
}


/* ------------------------------------------------------------- */
/* PANEL DE SELECCIÓN (LISTA DE EMPRESAS) */
/* ------------------------------------------------------------- */

.contenedor-anos-experiencia {
    display: flex;
    flex-direction: column; 
    gap: 1rem;
    width: 90%;
    max-width: 350px;
    padding: 0; 
    border-left: none;
}

.boton-etapa {
    font-family: "Orbitron", sans-serif;
    font-weight: 700;
    font-size: 1.2rem;
    color: var(--vt-c-white);
    padding: 0.8rem 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
    
    background: var(--vt-c-black-soft); 
    border: 2px solid var(--accent-color); 
    border-radius: 8px;
    box-shadow: 0 0 5px rgba(168, 85, 247, 0.5); 
    
    text-align: center; 
    margin-left: 0; 
}

.boton-etapa:hover {
    transform: scale(1.03); 
    background-color: var(--accent-color);
    color: var(--vt-c-black);
    box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
}

.boton-etapa.activo {
    transform: scale(1.02); 
    background-color: var(--accent-color);
    color: var(--vt-c-black);
    box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
    animation: active-neon-pulse 2s infinite ease-in-out; 
}
.boton-etapa::before {
    content: none; 
}


/* ------------------------------------------------------------- */
/* TARJETA DE CONTENIDO: SCROLL INTERNO Y GEOMETRÍA */
/* ------------------------------------------------------------- */

.contenedor-muestra-contenido-experiencia {
  display: flex;
  justify-content: center;
  padding-top: 1rem;
  width: 100%;
}

.contenedor-contenido {
  background: var(--vt-c-black-soft);
  padding: 1.5rem;
  border-radius: 20px;
  box-shadow: 
    0 0 10px var(--vt-c-magenta-original), 
    0 0 20px var(--vt-c-cyan), 
    inset 0 0 5px rgba(255, 255, 255, 0.1); 
  border: 2px solid transparent; 
  width: 100%; 
  max-width: 900px; 
  position: relative;
  animation: fadeIn 0.5s ease-out;
  display: flex; 
  flex-direction: column; 
  align-items: center;

  /* AJUSTE CLAVE: Altura base flexible para centrar la tarjeta de bienvenida */
  min-height: 400px;
  height: 60vh; 
  max-height: 550px;
}

/* ------------------------------------------------------------- */
/* TARJETA DE BIENVENIDA (AJUSTE CRÍTICO) */
/* ------------------------------------------------------------- */

.tarjeta-bienvenida {
    /* Mantenemos el borde degradado */
    border: 2px solid transparent;
    background: linear-gradient(var(--vt-c-black-soft), var(--vt-c-black-soft)) padding-box,
      linear-gradient(90deg, var(--vt-c-magenta-original) 0%, var(--vt-c-cyan) 50%, var(--vt-c-indigo) 100%) border-box;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(255, 0, 255, 0.2), 0 0 20px rgba(0, 255, 255, 0.2),
      inset 0 0 10px rgba(0, 0, 0, 0.5);
    
    /* CRÍTICO: Reducimos tamaño y centramos */
    justify-content: center; 
    height: auto; 
    min-height: auto;
    max-height: 350px; 
    max-width: 600px; 
    padding: 2rem;
}

/* Descripción de la Tarjeta de Bienvenida (AJUSTE DE COLOR) */
.descripcion {
  padding-block: 1rem 1.5rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white-soft); 
  line-height: 1.6;
  text-align: center; 
}


/* Contenedor Scrollable: ¡SCROLL INTERNO HABILITADO! */
.descripcion-scrollable {
  flex-grow: 1; 
  overflow-y: auto; 

  padding-right: 10px; 
  padding-bottom: 1rem;
  width: 100%; 
  text-align: left; 
  color: var(--vt-c-white);
  line-height: 1.6;

  /* Estilos de Scrollbar */
  &::-webkit-scrollbar {
    width: 8px;
  }
  &::-webkit-scrollbar-track {
    background: var(--vt-c-black-soft);
    border-radius: 10px;
  }
  &::-webkit-scrollbar-thumb {
    background: var(--accent-color); 
    border-radius: 10px;
    border: 2px solid var(--vt-c-black-soft);
  }
  &::-webkit-scrollbar-thumb:hover {
    background: var(--vt-c-white);
  }
}

.descripcion-scrollable p {
  padding-block: 0 1rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white);
  line-height: 1.6;
  text-align: left; 
}

.lista-logros {
    list-style: none;
    padding-left: 0;
    margin-top: 1rem;
    text-align: left;
}

.lista-logros li {
    margin-bottom: 0.5rem;
    position: relative;
    padding-left: 1.5rem;
    font-size: 0.95rem;
}

.logro-bullet {
    position: absolute;
    left: 0;
    top: 0;
    font-weight: 900;
    font-size: 0.8em;
}

/* ------------------------------------------------------------- */
/* 5. NUEVO: CONTENEDOR CIRCULAR para el BOTÓN "Volver Arriba" (¡CORREGIDO!) */
/* ------------------------------------------------------------- */
.contenedor-btn-regreso-neon {
    /* ✨ CAMBIO CRUCIAL 2: Cambiar fixed a absolute para que se posicione con el contenedor */
    position: absolute;
    bottom: 30px; /* Distancia desde abajo */
    right: 30px; /* Distancia desde la derecha */
    z-index: 10; /* Z-index reducido para no interferir con elementos flotantes globales */

    /* Dimensiones y forma del contenedor */
    width: 65px; 
    height: 65px;
    border-radius: 50%; 
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer; 
    
    /* Fondo y borde para que se destaque */
    background: radial-gradient(circle at 50% 50%, #2a2a2e 0%, var(--vt-c-black-soft) 100%);
    border: 2px solid var(--neon-green); 

    /* Múltiples sombras de neón animadas para el CONTENEDOR */
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


/* ------------------------------------------------------------- */
/* 6.--- RESPONSIVE MÓVIL/TABLET ----*/
/* ------------------------------------------------------------- */

@media (min-width: 900px) {
    /* Escritorio: Diseño de dos columnas */
    .contenedor-interactivo-tiempo {
        flex-direction: row;
        align-items: flex-start;
        justify-content: center;
        gap: 1.5rem; 
    }
    .contenedor-anos-experiencia {
        min-width: 200px; 
        width: 200px;
        flex-shrink: 0;
        margin-top: 0.5rem;
    }
    .contenedor-muestra-contenido-experiencia {
        width: 75%; 
        max-width: 1000px; 
        padding-left: 0; 
    }
    .contenedor-contenido {
        height: 65vh; 
        max-height: 580px;
    }
    .tarjeta-bienvenida {
      align-self: center; 
      height: auto;
    }
}

@media (max-width: 899px) {
    /* Móvil: Diseño de una columna (apilado) - Mantenido */
    .contenedor-interactivo-tiempo {
        flex-direction: column;
        gap: 1.5rem;
        overflow-x: hidden;
    }

    .contenedor-anos-experiencia {
        width: 90%;
        max-width: 400px;
        align-items: center;
    }

    .boton-etapa {
        width: 100%;
        max-width: 350px;
    }
    
    .contenedor-muestra-contenido-experiencia {
        padding: 0;
        width: 90%;
    }

    .titulo {
        font-size: 1.4rem;
    }
    .descripcion-scrollable p, .descripcion-scrollable .lista-logros li {
        font-size: 0.9rem;
    }

    /* Ajuste del contenedor flotante para móviles (Mantiene absolute) */
    .contenedor-btn-regreso-neon {
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

    /* Ajuste del botón interno para móviles (Mantenido) */
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