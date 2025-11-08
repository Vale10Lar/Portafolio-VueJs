<script setup>
// =========================================================
// 1. IMPORTACIONES DE COMPONENTES
// =========================================================
import { ref, computed } from "vue";
// Componente de navegación superior fijo
import NavBar from "./components/NavBar.vue";
// Componente principal de la tarjeta (perfil/contacto)
import DatosPersonales from "./components/DatosPersonales.vue";
// Sección de formación académica y cursos
import EducacionCursos from "./components/EducacionCursos.vue";
// Sección de historial de trabajo
import ExperienciaLaboral from "./components/ExperienciaLaboral.vue";
// Sección de carrusel de proyectos
import ProyectoCarrusel from "./components/ProyectoCarrusel.vue";
// Sección de habilidades técnicas y blandas
import HabilidadesTecnicas from "./components/HabilidadesTecnicas.vue";
//Seccion de intereses personales
import InteresesPersonales from "./components/InteresesPersonales.vue";

// =========================================================
// EFECTO REACTIVO AL CURSOR - SEPARADOR NEÓN
// =========================================================
import { onMounted, onUnmounted } from "vue";// Importa los hooks del ciclo de vida

onMounted(() => {
  const separators = document.querySelectorAll(".section-separator");
// Manejador de movimiento del ratón
  function handleMouseMove(e) {
    separators.forEach((sep) => {
      const rect = sep.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const intensity = Math.max(0, 1 - Math.abs(x - rect.width / 2) / rect.width);
      sep.style.boxShadow = `
        0 0 ${15 + intensity * 20}px var(--vt-c-neon-blue),
        0 0 ${30 + intensity * 40}px var(--vt-c-magenta-pure),
        0 0 ${60 + intensity * 60}px rgba(255, 0, 255, ${0.4 + intensity * 0.3})
      `;
    });
  }
// Añade el listener al movimiento del ratón
  window.addEventListener("mousemove", handleMouseMove);
// Limpia el listener al desmontar el componente
  onUnmounted(() => {
    window.removeEventListener("mousemove", handleMouseMove);
  });
});
// Referencia al componente hijo (tarjeta de perfil)
const datosPersonalesRef = ref(null);

// =========================================================
// 2. LÓGICA DEL FOOTER
// =========================================================

// Calcula el año actual para el footer
const currentYear = computed(() => {
  return new Date().getFullYear();
});

// Define la información estática del footer
const miNombre = "Valeria E. Lardín"; //
const infoCarrera = "Portafolio web - Diseño para estudio utn FRSR";

// =========================================================
// 3. FUNCIONES DE DESPLAZAMIENTO Y MANEJO DE EVENTOS
// =========================================================

// Desplaza suavemente al área del perfil
function scrollToProfileArea() {
  const profileArea = document.getElementById("profile-area-target");
  if (profileArea) {
    profileArea.scrollIntoView({ behavior: "smooth", block: "start" });
  } else {
    console.error("Error: Elemento con ID 'profile-area-target' no encontrado.");
  }
}


// Maneja el click en 'Contacto' desde la Navbar
function handleContactoClick() {
  if (datosPersonalesRef.value && typeof datosPersonalesRef.value.flipCard === "function") {
    datosPersonalesRef.value.flipCard();
  }
  scrollToProfileArea();
}
</script>

<template>
  <div id="app-portfolio">
    <header class="navbar-container">
      <NavBar @contacto-click="handleContactoClick" />
    </header>

    <div class="content-wrapper">
      <div id="profile-area-target" class="portfolio-section profile-area">
        <DatosPersonales ref="datosPersonalesRef" />
      </div>
      <div class="section-separator"></div>
      <main class="scrollable-content">
        <section id="educacion" class="portfolio-section scrollable-section">
          <EducacionCursos :on-return-to-top="scrollToProfileArea" />
        </section>
        <div class="section-separator"></div>
        <section id="experiencia" class="portfolio-section scrollable-section">
          <ExperienciaLaboral :on-return-to-top="scrollToProfileArea" />
          <!--Separador de seccion animado-->
        </section>
        <div class="section-separator"></div>
        <section
          id="proyectos"
          class="portfolio-section scrollable-section centered-content relative-section"
        >
          <ProyectoCarrusel :on-return-to-top="scrollToProfileArea" />
        </section>
        <div class="section-separator"></div>
        <section
          id="habilidades"
          class="portfolio-section scrollable-section centered-content relative-section"
        >
          <HabilidadesTecnicas :on-return-to-top="scrollToProfileArea" />
          <!---->
        </section>
        <div class="section-separator"></div>
        <section
          id="intereses"
          class="portfolio-section scrollable-section centered-content relative-section"
        >
          <InteresesPersonales :on-return-to-top="scrollToProfileArea" />
        </section>

        <footer class="portfolio-footer">
          <div class="footer-contenido-inline">
            <button
              @click="scrollToProfileArea"
              class="scroll-to-top-button"
              aria-label="Volver al principio de la página"
            >
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
                <path
                  d="M12 2a10 10 0 1010 10A10 10 0 0012 2zm3.71 11.29l-3-3a1 1 0 00-1.42 0l-3 3a1 1 0 001.42 1.42L12 11.41l2.29 2.3a1 1 0 001.42-1.42z"
                />
              </svg>
              Volver Arriba
            </button>

            <p class="footer-texto-inline">
              &copy; {{ currentYear }} | {{ miNombre }} - {{ infoCarrera }}
            </p>
          </div>
        </footer>
      </main>
    </div>
  </div>
</template>


<style scoped>
/* Contenedor Principal */

#app-portfolio {
  background-color: transparent;
  min-height: 100vh;/* Asegura que ocupe toda la altura de la ventana */
  width: 100vw;/* Asegura que ocupe todo el ancho de la ventana */
  display: flex;/*flexbox para organizar los elementos */
  flex-direction: column;/* Coloca los elementos en columna */
  overflow-x: hidden;/* Evita el desbordamiento horizontal */
}

/* ======================================================= */
/* 1. BARRA DE NAVEGACIÓN FIJA */
/* ======================================================= */
.navbar-container {
  position: sticky;/* Fija la barra en la parte superior al hacer scroll */
  top: 0;/* Posición superior */
  width: 100%;/* Ancho completo */
  height: var(--navbar-height);/* Altura definida en variables CSS */
  z-index: 1000;/* Asegura que esté por encima de otros elementos */
  background: var(--color-element-card);/* Usando variable para fondo de Navbar */
  /* Usando variables para borde e iluminación */
  border-bottom: 2px solid var(--vt-c-indigo);
  box-shadow: 0 5px 10px var(--vt-c-indigo-50), 0 0 10px var(--vt-c-indigo-30);
  display: flex;
  justify-content: center;
  align-items: center;
  box-sizing: border-box;/* Incluye padding y border en el tamaño total */
}

/* ======================================================= */
/* 2. CONTENEDOR PRINCIPAL */
/* ======================================================= */
/* Contenedor principal que envuelve todo el contenido debajo de la Navbar */
.content-wrapper {
  width: 100%;
  min-height: calc(100vh - var(--navbar-height));/* Asegura que ocupe al menos la altura restante de la ventana */
  display: flex;
  flex-direction: column;
  padding-bottom: 20px;
  background-color: transparent;
}

/* ======================================================= */
/* 3. ÁREA DE PERFIL (Tarjeta principal) */
/* ======================================================= */
.profile-area {
  /* Altura inicial del perfil forzada para que siempre ocupe la pantalla inicial */
  height: calc(100svh - var(--navbar-height));
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
  background-color: transparent;
}

/* ======================================================= */
/* 4. CONTENIDO SCROLLABLE */
/* ======================================================= */
.scrollable-content {
  width: 100%;
  padding: var(--section-v-padding) 0;/* Espaciado vertical entre secciones */
  box-sizing: border-box;/* Incluye padding y border en el tamaño total */
  background-color: transparent;
}

/* SECCIONES DE CONTENIDO (Educación, Experiencia, etc.) */

.portfolio-section.scrollable-section {
  /* Altura mínima para ocupar al menos una pantalla */
  min-height: calc(100svh - var(--navbar-height));
  padding: var(--section-v-padding) var(--content-gap);/* Espaciado interno */
  box-sizing: border-box;/* Incluye padding y border en el tamaño total */
  max-width: var(--max-content-width);/* Ancho máximo para centrar el contenido */
  margin: 0 auto;/* Centra horizontalmente */
  background-color: transparent;/* Fondo transparente */
}

/* Elimina el borde inferior de la última sección antes del footer */
.portfolio-section:last-of-type {
  /* Elimina el borde inferior de la última sección antes del footer */
  border-bottom: none;
}

/* Estilos del Footer INLINE */
.portfolio-footer {
  width: 100%;
  margin-top: 2rem;
  background-color: var(--vt-c-black-darker);
  padding: 1.5rem 1rem;/* Espaciado interno */
  text-align: center;
  border-top: 1px solid var(--vt-c-neon-blue-15);
  position: relative;
  z-index: 50;
  box-sizing: border-box;
}
/* Contenido inline del footer */
.footer-contenido-inline {
  max-width: var(--max-content-width);
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}
/* Estilos del texto del footer */
.footer-texto-inline {
  font-family: "Inter", sans-serif;
  font-size: 0.9rem;
  font-weight: 300;
  color: var(--vt-c-grey-medium);
  margin: 0;

  /* Efecto Neón sutil */
  text-shadow: 0 0 5px rgba(0, 255, 255, 0.4), 0 0 10px var(--vt-c-neon-blue-20);

  /* Animación de pulso de color */
  animation: neon-pulse-footer 5s infinite alternate ease-in-out;
}
/* Animación de pulso neón para el texto del footer */
@keyframes neon-pulse-footer {
  0% {
    color: var(--vt-c-grey-medium);
    text-shadow: 0 0 4px rgba(0, 255, 255, 0.3); /* Valor similar a --vt-c-neon-blue-30 */
  }
  100% {
    color: var(--vt-c-white-brighter);
    text-shadow: 0 0 6px rgba(0, 255, 255, 0.6), 0 0 15px rgba(0, 255, 255, 0.4);
  }
}
/* Ajustes responsivos para el footer */
@media (max-width: 768px) {
  .portfolio-footer {
    padding: 1rem 0.5rem;
  }
  .footer-texto-inline {
    font-size: 0.8rem;
  }
}

.section-separator {/* Separador de secciones animado */
  transition: box-shadow 0.25s ease-out;
}



/* ======================================================= */
/* RESPONSIVE DESIGN */
/* ======================================================= */
/* Ajustes para pantallas pequeñas */
@media (max-width: 850px) {
  .portfolio-section.scrollable-section {
    max-width: 100%;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .profile-area {
    height: calc(100svh - var(--navbar-height));
  }
}

/* ======================================================= */
/* SCROLL-TO-TOP BUTTON (ADAPTADO AL FOOTER) */
/* ======================================================= */

.scroll-to-top-button {
  position: static;
  margin: 0 auto;

  width: 200px;
  height: 40px;
  padding: 0 1rem;
  border-radius: 9999px;

  background-color: var(--vt-c-indigo);
  color: var(--vt-c-white-soft);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  cursor: pointer;
  font-weight: 600;

  /* Estilo del borde y sombra neón */
  border: 2px solid var(--vt-c-indigo);
  box-shadow: 0 0 10px rgba(168, 85, 247, 0.7),
    /* Simil a --vt-c-indigo-70 */ 0 0 20px var(--vt-c-indigo-50);
  transition: all 0.3s ease;
}

.scroll-to-top-button:hover {
  background-color: transparent;
  color: var(--color-neon-cyan);
  border-color: var(--color-neon-cyan);
  box-shadow: 0 0 8px var(--color-neon-cyan), 0 0 15px var(--vt-c-neon-blue-50);
  transform: translateY(-3px);
}

.scroll-to-top-button svg {
  width: 1.25rem;
  height: 1.25rem;
  fill: currentColor;
  filter: drop-shadow(0 0 1px currentColor);
}

/* Responsive adjustments for mobile */
@media (max-width: 600px) {
  .scroll-to-top-button {
    width: 160px;
    height: 35px;
    font-size: 0.8rem;
  }
}
</style>
