<template>
  <section id="intereses" class="contenedor-intereses">
    <FondoCibernetico />

    <div
      class="contenedor-btn-regreso-neon boton-absolute-local"
      @click="volverArriba"
      aria-label="Volver a la navegacion principal"
    >
      <button class="btn-flotante-volver-arriba">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="2"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M4.5 10.5 12 3m0 0 7.5 7.5M12 3v18"
          />
        </svg>
      </button>
    </div>

    <h2 class="titulo-seccion">Interes Profesional</h2>

    <p class="subtitulo-intereses">
      Explora las áreas donde mi enfoque didáctico, mi pasión por la calidad y mi autonomía aportan
      un valor diferencial al desarrollo.
    </p>

    <div class="grid-intereses">
      <div
        v-for="interes in interesesData"
        :key="interes.id"
        class="tarjeta-interes-wrapper"
        :style="{ '--color-glow': interes.color }"
      >
        <div
          class="tarjeta-interes"
          @click="interes.flip = !interes.flip"
          :class="{ flipped: interes.flip }"
        >
          <div class="tarjeta-frente">
            <div class="tarjeta-icono" v-html="interes.icono"></div>
            <h3 class="tarjeta-titulo">{{ interes.titulo }}</h3>
            <p class="tarjeta-click-info">Toca para descubrir mi valor</p>
          </div>

          <div class="tarjeta-dorso">
            <p class="tarjeta-descripcion">{{ interes.descripcion }}</p>
            <span class="tarjeta-click-info">Regresar</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { reactive } from "vue";
import FondoCibernetico from "./FondoCibernetico.vue";

// Lógica para el botón "Volver Arriba"
const volverArriba = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};

// --- Datos de los Intereses Adaptados al Perfil Profesional ---
// Se utilizan iconos SVG inline para evitar dependencias externas.
const interesesData = reactive([
  {
    id: 1,
    titulo: "Arte, Creatividad y Didáctica",
    // Aplicación profesional: Creación de UX/UI y contenido de valor.
    descripcion:
      "Mi motor es la fusión de creatividad y tecnología para diseñar soluciones didácticas e intuitivas. Especializada en transformar sistemas complejos en experiencias de usuario (UX) simples y valiosas, aplicable a Web, Móvil, Desktop y Gamificación.",

    color: "var(--vt-c-magenta-pure)",
    // Icono: Paleta y pincel (arte y creatividad)
    icono:
      '<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><path d="M16.2 7.8L7.8 16.2c-.8.8-2.2.8-3 0-.8-.8-.8-2.2 0-3L13.2 4.8c.8-.8 2.2-.8 3 0 .8.8.8 2.2 0 3z"></path></svg>',
    flip: false,
  },
  {
    id: 2,
    titulo: "Debugging y Calidad (QA)",
    // Aplicación profesional: Búsqueda incesante de la solución óptima.
    descripcion:
      "Mi habilidad para detectar detalles y buscar errores donde nadie más los ve es un activo crucial en Quality Assurance (QA) y optimización de código. No busco una solución, sino la más eficiente y sin fallos. Obsesión positiva por la mejora continua.",
    // [CORREGIDO] Usa el alias global para el Cian
    color: "var(--color-neon-cyan)",
    // Icono: Lupa (detección/búsqueda de errores)
    icono:
      '<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line><line x1="11" y1="8" x2="11" y2="14"></line><line x1="8" y1="11" x2="14" y2="11"></line></svg>',
    flip: false,
  },
  {
    id: 3,
    titulo: "Autonomía e Hiperfoco",
    // Aplicación profesional: Alto rendimiento en ambientes de trabajo individualizado.
    descripcion:
      "La programación es mi medio ideal para un rendimiento máximo: me permite operar de forma autónoma, sin interrupciones sociales, para lograr un enfoque profundo (hiperfoco). Mi productividad es impulsada por mi capacidad de autoaprendizaje constante y lectura intensa.",
    // [CORREGIDO] Usa el nombre global para el Verde Neón
    color: "var(--vt-c-neon-green)",
    // Icono: Engranaje/Sistema (autonomía y pensamiento sistémico)
    icono:
      '<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="3"></circle><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0-.33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09A1.65 1.65 0 0 0 15 4.6a1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82v.09a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path></svg>',
    flip: false,
  },
  {
    id: 4,
    titulo: "Búsqueda de la Verdad (Evolución)",
    // Aplicación profesional: Compromiso con el conocimiento de vanguardia.
    descripcion:
      'La búsqueda incesante de "la respuesta a todo" se traduce profesionalmente en un compromiso de ir siempre más allá del conocimiento base. Me obsesiona el aprendizaje constante y me permite adaptarme rápidamente a nuevas tecnologías o arquitecturas de vanguardia.',
    // [CORREGIDO] Usa el nombre global para el Amarillo
    color: "var(--vt-c-neon-yellow)",
    // Icono: Cohete (ir más allá, evolución)
    icono:
      '<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2L6 12h12z"></path><path d="M12 22V12"></path><path d="M12 12V2"></path><path d="M12 12L18 22"></path><path d="M12 12L6 22"></path><path d="M6 12l6-10 6 10"></path></svg>',
    flip: false,
  },
]);
</script>

<style scoped>
/* ------------------------------------------------------------- */
/* ANIMACIONES BASE (CLAVE PARA EL ESTILO NEÓN) */
/* ------------------------------------------------------------- */
@keyframes neon-flicker-int {
  0%,
  100% {
    box-shadow: 0 0 5px var(--color-glow), 0 0 15px var(--color-glow);
  }
  50% {
    box-shadow: 0 0 10px var(--color-glow), 0 0 30px var(--color-glow),
      0 0 50px var(--vt-c-white-rgba-10);
  }
}

@keyframes pulse {
  0%,
  100% {
    opacity: 0.6;
  }
  50% {
    opacity: 1;
  }
}

/* ------------------------------------------------------------- */
/* LAYOUT PRINCIPAL (AJUSTE DE ALTURA DINÁMICO) */
/* ------------------------------------------------------------- */
.contenedor-intereses {
  min-height: 100dvh; /*  Altura dinámica que se adapta al alto visible real de la ventana (100dvh) */
  padding: 3rem 1rem 3rem 1rem;
  max-width: 100%;

  position: relative; /* position: relative para contener el fondo absoluto */
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;

  background-color: var(--color-theme-base); /* Fondo al color negro morado profundo */
  overflow: hidden;
  z-index: 10;
}

/* Asegura que todo el contenido (texto, botón y grid) esté por encima del fondo */
.titulo-seccion,
.subtitulo-intereses,
.grid-intereses {
  position: relative;
  z-index: 10;
}

.titulo-seccion {
  position: relative;
  margin-bottom: 2rem;
  font-family: "Orbitron", sans-serif;
  font-size: 2.8rem;
  font-weight: 700;

  color: var(--vt-c-white);
  text-shadow: 0 0 10px var(--vt-c-white), 0 0 25px var(--vt-c-white),
    0 0 40px var(--vt-c-neon-green-rgba-80);
  padding-bottom: 0.5rem;

  border-bottom: 2px solid var(--vt-c-indigo-rgba-20);
  z-index: 10;
}

.subtitulo-intereses {
  font-family: "Inter", sans-serif;
  font-size: 1.1rem;
  /*  Color a blanco suave */
  color: var(--color-theme-text-light);
  /* RGBA de Blanco suave */
  text-shadow: 0 0 5px var(--vt-c-white-rgba-50);
  margin-bottom: 3rem;
  max-width: 700px;
}

/* ------------------------------------------------------------- */
/* GRID Y TARJETAS FLIP NEÓN */
/* ------------------------------------------------------------- */
.grid-intereses {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 30px;
  width: 90%;
  max-width: 1200px;
  margin-top: 1rem;
  margin-bottom: 2rem;
}

.tarjeta-interes-wrapper {
  /* Contenedor que maneja la perspectiva y el glow exterior */
  perspective: 1000px;
  position: relative;
  height: 250px; /* Altura fija para el efecto flip */
  transition: all 0.3s;

  /* Sombra inicial sutil de Blanco */
  box-shadow: 0 0 5px var(--vt-c-white-rgba-10);
  border-radius: 10px;
}

.tarjeta-interes {
  /* Contenedor que realmente se voltea */
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.8s cubic-bezier(0.4, 0.2, 0.2, 1);
  cursor: pointer;
  border-radius: 10px;

  background-color: var(--vt-c-black-soft);
  border: 2px solid var(--color-glow);
  box-shadow: 0 0 5px var(--color-glow);
}

.tarjeta-interes.flipped {
  transform: rotateY(180deg);
}

/* Estilo para el hover del wrapper */
.tarjeta-interes-wrapper:hover .tarjeta-interes {
  box-shadow: 0 0 15px var(--color-glow), 0 0 35px var(--color-glow);
  animation: neon-flicker-int 3s infinite alternate;
}

.tarjeta-frente,
.tarjeta-dorso {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  border-radius: 8px;

  color: var(--vt-c-white);

  background-color: var(--vt-c-black-soft);
}

.tarjeta-frente {
  z-index: 2;
  transform: rotateY(0deg);
}

.tarjeta-dorso {
  transform: rotateY(180deg);
  text-align: center;
}

.tarjeta-icono svg {
  width: 50px;
  height: 50px;
  stroke: var(--color-glow);
  filter: drop-shadow(0 0 5px var(--color-glow));
  margin-bottom: 10px;
  transition: all 0.4s;
}
.tarjeta-interes-wrapper:hover .tarjeta-icono svg {
  transform: scale(1.1) rotateZ(5deg);
}

.tarjeta-titulo {
  font-family: "Orbitron", sans-serif;
  font-size: 1.4rem;

  color: var(--vt-c-white);
  text-shadow: 0 0 5px var(--color-glow);
  margin-bottom: 15px;
}

.tarjeta-descripcion {
  font-family: "Inter", sans-serif;
  font-size: 1rem;
  line-height: 1.5;

  color: var(--vt-c-white);

  text-shadow: 0 0 2px var(--vt-c-white-rgba-20);
}

.tarjeta-click-info {
  font-size: 0.75rem;
  /* [CORREGIDO] Gris */
  color: var(--vt-c-gray);
  margin-top: 10px;
  position: absolute;
  bottom: 15px;
  animation: pulse 2s infinite;
}

/* ------------------------------------------------------------- */
/* RESPONSIVE */
/* ------------------------------------------------------------- */
@media (max-width: 768px) {
  .titulo-seccion {
    font-size: 2rem;
  }
  .grid-intereses {
    grid-template-columns: 1fr; /* Una columna en móvil */
    gap: 20px;
    width: 100%;
  }
  .tarjeta-interes-wrapper {
    height: 220px; /* Reducir un poco la altura en móvil */
  }
  .tarjeta-titulo {
    font-size: 1.2rem;
  }
  .tarjeta-descripcion {
    font-size: 0.95rem;
  }
}
</style>
