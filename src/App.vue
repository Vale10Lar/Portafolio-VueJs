<script setup>
import { ref } from "vue";
import NavBar from "./components/NavBar.vue";
import DatosPersonales from "./components/DatosPersonales.vue";
import EducacionCursos from "./components/EducacionCursos.vue";
import ExperienciaLaboral from "./components/ExperienciaLaboral.vue";
import ProyectoCarrusel from "./components/ProyectoCarrusel.vue";

const datosPersonalesRef = ref(null);

// FUNCIÓN CLAVE: Desplazamiento al área de la barra de navegación/perfil
function scrollToProfileArea() {
  const profileArea = document.getElementById("profile-area-target");
  if (profileArea) {
    // CLAVE: Desplaza suavemente hasta el inicio del área del perfil
    profileArea.scrollIntoView({ behavior: "smooth", block: "start" });
  } else {
    console.error("Error: Elemento con ID 'profile-area-target' no encontrado.");
  }
}

// Función para el botón de contacto (Mantiene su funcionalidad original)
function handleContactoClick() {
  // 1. Voltear la tarjeta (si está visible)
  if (datosPersonalesRef.value && typeof datosPersonalesRef.value.flipCard === "function") {
    datosPersonalesRef.value.flipCard();
  }

  // 2. Desplazarse al área de la tarjeta
  scrollToProfileArea();
}

/**
 * NOTA: Eliminamos la definición del componente inline 'ScrollToTopButton'
 * para simplificar y usamos directamente un botón en el template para la versión fija.
 */
</script>

<template>
  <div id="app-portfolio">
    <!-- 1. BARRA DE NAVEGACIÓN (PEGADA ARRIBA) -->
    <header class="navbar-container">
      <NavBar @contacto-click="handleContactoClick" />
    </header>

    <!-- 2. CONTENEDOR PRINCIPAL DEL CONTENIDO (Scrollable) -->
    <div class="content-wrapper">
      <!-- ÁREA DE PERFIL (Tarjeta DatosPersonales) - Ocupa el 100vh inicial -->
      <div id="profile-area-target" class="portfolio-section profile-area">
        <!-- El componente DatosPersonales se centra aquí -->
        <DatosPersonales ref="datosPersonalesRef" />
      </div>

      <!-- CONTENIDO SCROLLABLE (Educación y abajo) -->
      <main class="scrollable-content">
        <!-- SECCIÓN EDUCACIÓN: Se le pasa la función de retorno como prop -->
        <section id="educacion" class="portfolio-section scrollable-section">
          <EducacionCursos :on-return-to-top="scrollToProfileArea" />
        </section>

        <!-- SECCIÓN EXPERIENCIA: Se le pasa la función de retorno como prop -->
        <section id="experiencia" class="portfolio-section scrollable-section">
          <!-- IMPORTANTE: El componente ExperienciaLaboral debe implementar esta prop -->
          <ExperienciaLaboral :on-return-to-top="scrollToProfileArea" />
        </section>

        <!-- SECCIÓN PROYECTOS (Placeholder): Se añade el botón directamente -->
        <section
          id="proyectos"
          class="portfolio-section scrollable-section centered-content relative-section"
        >
          <ProyectoCarrusel :on-return-to-top="scrollToProfileArea" />
        </section>

        <!-- SECCIÓN HABILIDADES (Placeholder): Se añade el botón directamente -->
        <section
          id="habilidades"
          class="portfolio-section scrollable-section centered-content relative-section"
        >
          <h2 class="section-title">Habilidades</h2>
          <div class="placeholder-card">
            <p>Contenido de la sección Habilidades.</p>
          </div>
        </section>
      </main>
    </div>

    <!-- BOTÓN DE SCROLL FIJO GLOBAL (Opción adicional para usabilidad) -->
  </div>
</template>

<style scoped>
/* ------------------------------------------------------------- */
/* VARIABLES CLAVE DE DISEÑO */
/* ------------------------------------------------------------- */
:root {
  --navbar-height: 80px; /* Altura de la nueva barra superior */
  --content-gap: 30px;
  --max-content-width: 1100px; /* Ancho máximo para el contenido scrollable */
  /* Colores */
  --vt-c-card-dark: #151b33;
  --vt-c-indigo: #a855f7;
  --vt-c-black: #03091b;
  --vt-c-cyan: #00ffff;
  --vt-c-magenta: #ff00ff;
  --vt-c-white: #ebe0ff;
  --neon-green: #39ff14; /* Color para los botones de retorno */
}

/* El contenedor principal de la App */
#app-portfolio {
  background-color: var(--vt-c-black);
  min-height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  overflow-x: hidden;
}

/* ======================================================= */
/* 1. BARRA SUPERIOR (PEGADA Y CENTRADA)*/
/* ======================================================= */
.navbar-container {
  position: sticky;
  top: 0;
  width: 100%;
  height: var(--navbar-height);
  z-index: 1000;
  background: var(--vt-c-card-dark);
  border-bottom: 2px solid var(--vt-c-indigo);
  box-shadow: 0 5px 10px rgba(168, 85, 247, 0.5), 0 0 10px rgba(168, 85, 247, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  box-sizing: border-box;
}

/* ======================================================= */
/* 2. CONTENEDOR PRINCIPAL DEL CONTENIDO (content-wrapper) */
/* ======================================================= */
.content-wrapper {
  width: 100%;
  min-height: calc(100vh - var(--navbar-height));
  display: flex;
  flex-direction: column;
  padding-bottom: 20px;
}

/* 3. ÁREA DE PERFIL (Centrado de la Tarjeta) */
.profile-area {
  height: calc(100vh - var(--navbar-height));
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
}

/* 4. CONTENIDO SCROLLABLE (El contenedor principal de Educación y abajo) */
.scrollable-content {
  width: 100%;
  padding: 5rem 0;
  box-sizing: border-box;
}

/* ------------------------------------------------------------- */
/* ESTILOS DE SECCIONES SCROLLABLES (EDUCACION, EXPERIENCIA, etc.) */
/* ------------------------------------------------------------- */
.portfolio-section.scrollable-section {
  min-height: 100vh;
  padding: 5rem var(--content-gap);
  border-bottom: 1px dashed rgba(168, 85, 247, 0.2);
  box-sizing: border-box;
  max-width: var(--max-content-width);
  margin: 0 auto;
}

.portfolio-section:last-child {
  border-bottom: none;
  min-height: auto;
}

/* Clase para que el botón local funcione con position: absolute */
.relative-section {
  position: relative;
}

.centered-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.section-title {
  font-family: "Orbitron", sans-serif;
  font-size: 2.5rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 2rem;
  color: var(--vt-c-cyan);
  text-shadow: 0 0 5px var(--vt-c-cyan), 0 0 15px rgba(0, 255, 255, 0.5);
  padding-bottom: 0.5rem;
  border-bottom: 2px solid rgba(168, 85, 247, 0.2);
}

.placeholder-card {
  background: rgba(10, 10, 21, 0.8);
  border: 2px solid var(--vt-c-indigo);
  border-radius: 10px;
  padding: 2rem;
  color: var(--vt-c-white);
  font-family: sans-serif;
  text-align: center;
  max-width: 500px;
  width: 100%;
}

/* ------------------------------------------------------------- */
/* ESTILOS DEL BOTÓN DE RETORNO EN CADA SECCIÓN (LOCAL) */
/* ------------------------------------------------------------- */
.section-return-button {
  /* Posicionamiento y alineación dentro de la sección */
  margin-top: 3rem;
  margin-bottom: 1rem;

  /* Estilos base (similar al de EducacionCursos.vue) */
  background: var(--vt-c-card-dark);
  border: 2px solid var(--neon-green);
  color: var(--neon-green);
  padding: 0.5rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;

  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
  font-family: "Orbitron", sans-serif;
  text-transform: uppercase;

  /* Efecto Neón Pulso */
  box-shadow: 0 0 5px var(--neon-green), 0 0 15px rgba(57, 255, 20, 0.4);
}

.section-return-button:hover {
  background: var(--neon-green);
  color: var(--vt-c-black);
  box-shadow: 0 0 10px var(--neon-green), 0 0 25px var(--neon-green);
  transform: translateY(-2px);
}

.section-return-button svg {
  width: 20px;
  height: 20px;
  transform: rotate(180deg);
}

/* ------------------------------------------------------------- */
/* ESTILOS DEL BOTÓN GLOBAL "REGRESO AL PERFIL" (FIJO) */
/* ------------------------------------------------------------- */

.return-button {
  position: fixed; /* CLAVE: Fijo en la ventana */
  bottom: 1.5rem;
  right: 1.5rem;
  z-index: 100;

  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 2px solid var(--vt-c-magenta);
  background: rgba(10, 10, 21, 0.9);
  color: var(--vt-c-magenta);
  cursor: pointer;
  transition: all 0.3s ease;

  /* Efecto Neón */
  box-shadow: 0 0 5px var(--vt-c-magenta), 0 0 15px rgba(255, 0, 255, 0.4);

  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
}

.return-button:hover {
  background: var(--vt-c-magenta);
  color: var(--vt-c-black);
  box-shadow: 0 0 10px var(--vt-c-magenta), 0 0 25px var(--vt-c-magenta);
  transform: translateY(-3px) scale(1.05);
}

.return-button svg {
  width: 60%;
  height: 60%;
}

/* 5. DISEÑO RESPONSIVE (Móvil) */
@media (max-width: 850px) {
  .portfolio-section.scrollable-section {
    max-width: 100%;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  .profile-area {
    height: auto;
    min-height: 80vh;
  }

  /* Ajuste del botón fijo en móvil */
  .return-button {
    right: 1rem;
    bottom: 1rem;
    width: 45px;
    height: 45px;
  }
}
</style>
