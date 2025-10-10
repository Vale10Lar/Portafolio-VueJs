<template>
  <nav class="navbar" ref="navbarRef">
    
    <button class="hamburger-button" @click="toggleMenu" :aria-expanded="isMenuOpen">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
      >
        <line x1="3" y1="12" x2="21" y2="12"></line>
        <line x1="3" y1="6" x2="21" y2="6"></line>
        <line x1="3" y1="18" x2="21" y2="18"></line>
      </svg>
    </button>

    <div class="navbar-menu">
      <ul>
        <a
          v-for="nav in navegacion"
          :key="nav.nombre"
          :href="nav.enlace"
          class="nav-item"
          @click="closeMenu"
        >
          {{ nav.nombre }}
        </a>
      </ul>
    </div>

    <transition name="slide-in">
      <div v-if="isMenuOpen" class="off-canvas-menu" :style="{ top: menuTop }">
        <button class="close-button-mobile" @click="closeMenu">&times;</button>

        <ul class="nav-links-mobile">
          <li v-for="nav in navegacion" :key="nav.id">
            <a :href="nav.enlace" class="nav-button-mobile" @click="closeMenu">
              {{ nav.nombre }}
            </a>
          </li>
        </ul>
      </div>
    </transition>

    <div v-if="isMenuOpen" class="menu-overlay" @click="closeMenu"></div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

// Referencias y Estado Reactivo
const navbarRef = ref(null); // Referencia al elemento NAV (la barra superior)
const isMenuOpen = ref(false); // Estado para controlar si el menú lateral está abierto
const menuTop = ref("0px"); // Posición 'top' del menú lateral (calculado dinámicamente)

// Función para calcular la altura de la barra superior
const calculateMenuTop = () => {
  if (navbarRef.value) {
    // Establece la posición Y del menú justo al final de la barra superior
    menuTop.value = `${navbarRef.value.offsetHeight}px`;
  }
};

// Ciclo de vida: Montado
onMounted(() => {
  calculateMenuTop();
  // Recalcular la posición TOP si se cambia el tamaño de la ventana (responsive)
  window.addEventListener("resize", calculateMenuTop);
});

// Ciclo de vida: Desmontado
onUnmounted(() => {
  window.removeEventListener("resize", calculateMenuTop);
});

// Funciones de Control del Menú
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value; // Alterna el estado del menú
};

const closeMenu = () => {
  isMenuOpen.value = false; // Cierra el menú
};

// Datos de Navegación
const navegacion = ref([
  { id: 1, nombre: "Educación", enlace: "#educacion" },
  { id: 2, nombre: "Experiencia", enlace: "#experiencia" },
  { id: 3, nombre: "Proyectos", enlace: "#proyectos" },
  { id: 4, nombre: "Habilidades", enlace: "#habilidades" },
  { id: 5, nombre: "Intereses", enlace: "#intereses" },
]);
</script>

<style scoped>
/*  SOLUCIÓN CONTRA ERROR 500 (la solucion que anda es definir las variables en : root): 
 * Definición simplificada de variables para asegurar la compilación scoped. */
:root {
  --vt-c-cyan: #00ffff;
  --vt-c-indigo: #a855f7; 
  --vt-c-black-soft: #151b33;
  --vt-c-white: #ebe0ff;
  --vt-c-magenta: #ff00ff;
}

/* ESTILOS DE ESCRITORIO  */

.navbar {
  /* Degradado de fondo */
  background: linear-gradient(90deg, var(--vt-c-indigo) 10%, var(--vt-c-black-soft) 95%);

  /* Diseño y Sombra */
  padding: 15px 20px;
  border-radius: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5), 0 0 5px rgba(0, 255, 255, 0.3);
  transition: box-shadow 0.3s ease-in-out;

  /* Posicionamiento y Capa (z-index) */
  position: relative;
  z-index: 20;
  display: flex;
  justify-content: center;
  width: auto;
}

/* Efecto de luz al pasar el mouse por la barra (Contenedor) */
.navbar:hover {
  box-shadow: 
    0 4px 15px rgba(216, 7, 106, 0.4), 
    0 0 15px var(--vt-c-indigo), 
    0 0 30px var(--vt-c-cyan);
}

/* Contenedor del menú de enlaces (Escritorio) */
.navbar-menu {
  display: flex;
  justify-content: flex-end; /* Alinea los enlaces a la derecha */
  width: 100%;
}

.navbar-menu ul {
  list-style: none;
  display: flex;
  gap: 1.5rem;
  padding: 0;
  margin: 0;
  align-items: center;
}

/* ESTILO BASE DE LOS BOTONES DE NAVEGACIÓN (.nav-item) */
.nav-item {
  /* Fondo con degradado sutil */
  background: linear-gradient(180deg, rgba(30, 10, 50, 0.7), rgba(41, 22, 83, 0.7));

  /* Diseño */
  border-radius: 5px;
  color: var(--vt-c-white);
  text-decoration: none;
  padding: 8px 15px;
  display: inline-block;

  /* Tipografía */
  font-family: "Orbitron", sans-serif;
  font-weight: 500;
  font-size: 0.9rem;
  letter-spacing: 1px;

  /* Borde e Sombra Neón */
  border: 1px dashed rgba(255, 255, 255, 0.3);
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
  transition: all 0.2s ease-in-out;

  /* Efecto de texto neón sutil */
  text-shadow: 0 0 1px var(--vt-c-cyan), 0 0 1px var(--vt-c-cyan);

  position: relative;
}

/* EFECTO HOVER: Elevación y Brillo Cian/Morado */
.nav-item:hover {
  transform: translateY(-3px); /* Ligeramente elevado */

  color: var(--vt-c-cyan);
  text-shadow: 0 0 8px var(--vt-c-cyan), 0 0 15px rgba(0, 255, 255, 0.6);

  box-shadow: 
    0 5px 10px rgba(0, 0, 0, 0.5), 
    0 0 5px var(--vt-c-cyan), 
    0 0 20px var(--vt-c-indigo);

  border-color: var(--vt-c-cyan);
}

/* Elementos para celu: Ocultos en escritorio */
.hamburger-button,
.off-canvas-menu,
.menu-overlay,
.close-button-mobile {
  display: none;
}

/* RESPONSIVE DESIGN (CELU)  */
 

@media (max-width: 768px) {
  /* 1. OCULTAR el menú de ESCRITORIO */
  .navbar-menu {
    display: none;
  }

  /* BARRA DE NAVEGACIÓN REDUCIDA Y CENTRADA */
  .navbar {
    padding: 10px 15px;
    justify-content: center;
    align-items: center;
    width: 60px; /* Ancho fijo pequeño para que solo quepa la hamburguesa */
    margin: 0 auto; /* Centra la pequeña barra */
  }

  /* 2. MOSTRAR el botón Hamburguesa */
  .hamburger-button {
    display: block;
    background: none;
    border: none;
    color: var(--vt-c-cyan);
    cursor: pointer;
    padding: 0;
    height: 30px;
    width: 30px;
    filter: drop-shadow(0 0 5px var(--vt-c-cyan));
    z-index: 25;
  }

  .hamburger-button svg {
    width: 100%;
    height: 100%;
  }

  /* Botón de Cierre dentro del Menú Lateral */
  .close-button-mobile {
    display: block;
    position: absolute;
    top: 5px; 
    right: 5px; 
    background: none;
    border: none;
    color: var(--vt-c-magenta); 
    font-size: 24px;
    font-weight: bold;
    cursor: pointer;
    padding: 5px;
    z-index: 101;
    text-shadow: 0 0 5px var(--vt-c-magenta);
  }

  /* 3. Estilo del Menú Lateral (Off-Canvas) */
  .off-canvas-menu {
    display: block;
    position: fixed;
    /* 'top' es calculado por el script Vue para iniciar bajo la barra NAV */
    left: 0;

    width: 150px; 
    height: auto;
    max-height: calc(100vh - 50px);
    transform: none;

    /* Fondo transparente sin blur para rendimiento en móvil */
    background-color: rgba(29, 29, 39, 0.8); /* Aumenté la opacidad para mejor legibilidad */

    padding: 30px 5px 15px 5px; 
    z-index: 100;
    overflow-y: auto;
    border-radius: 0 15px 15px 0;

    backdrop-filter: none;

    /* Sombra Neón lateral */
    box-shadow: 
      5px 0 15px rgba(0, 0, 0, 0.5), 
      -2px 0 10px var(--vt-c-cyan),
      2px 0 8px var(--vt-c-magenta);
  }

  /* Estilos de la lista (Vertical) */
  .nav-links-mobile {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  /* ESTILO DE TEXTO NEÓN (COMPACTO) */
  .nav-button-mobile {
    /* Reseteo de estilos de botón */
    background: none;
    border: none;
    box-shadow: none;

    /* Color y layout */
    color: var(--vt-c-cyan);
    text-decoration: none;
    display: block;
    width: 100%;
    text-align: left;
    padding: 3px 5px;

    /* Tipografía */
    font-family: "Orbitron", sans-serif;
    font-size: 0.7rem;
    font-weight: 500;

    /* EFECTO NEÓN CIAN EN EL TEXTO */
    text-shadow: 0 0 3px var(--vt-c-cyan), 0 0 6px rgba(0, 255, 255, 0.3);

    transition: color 0.2s, text-shadow 0.2s, background-color 0.2s;
  }

  /* Efecto de hover simple para el celu */
  .nav-button-mobile:hover {
    color: white;
    text-shadow: 0 0 5px white, 0 0 8px var(--vt-c-cyan);
    background-color: rgba(255, 255, 255, 0.08);
    transform: none;
  }

  /* 4. Estilo del Overlay oscuro */
  .menu-overlay {
    display: block;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    z-index: 99;
  }

  /* Transición para el efecto de deslizamiento (slide-in) */
  .slide-in-enter-active,
  .slide-in-leave-active {
    transition: transform 0.3s ease-out;
  }
  /* Posición inicial (fuera de pantalla a la izquierda) */
  .slide-in-enter-from,
  .slide-in-leave-to {
    transform: translateX(-100%);
  }
}
</style>
