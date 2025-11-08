<template>
  <nav class="navbar-content">
    <div class="nav-wrapper">
      <button
        class="hamburger-button"
        @click="toggleMobileMenu"
        :class="{ 'is-active': isMobileMenuOpen }"
        aria-label="Toggle navigation"
      >
        <span class="hamburger-icon"></span>
      </button>
      <ul class="nav-list" :class="{ 'mobile-open': isMobileMenuOpen }">
        <li v-for="item in navItems" :key="item.name" class="nav-item">
          <a
            :href="item.href"
            @click.prevent="scrollToSection(item.href.substring(1))"
            class="neon-button"
          >
            {{ item.name }}
          </a>
        </li>

        <li class="nav-item contacto-list-item">
          <button @click="handleContactoClick" class="neon-button contacto-button">CONTACTO</button>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script setup>
// =========================================================
// 1. SETUP Y EMIT
// =========================================================
import { defineEmits, ref } from "vue";

// Define el evento que voltea la tarjeta
const emit = defineEmits(["contacto-click"]);
// =========================================================
// 1.5. CONTROL DEL MENÚ MÓVIL (NUEVO)
// =========================================================
const isMobileMenuOpen = ref(false);

function toggleMobileMenu() {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
}
// =========================================================
// 2. CONFIGURACIÓN DE NAVEGACIÓN
// =========================================================
// Lista de elementos de navegación, mapeados a los IDs de las secciones en App.vue
const navItems = [
  { name: "Educación", href: "#educacion" },
  { name: "Experiencia", href: "#experiencia" },
  { name: "Proyectos", href: "#proyectos" },
  { name: "Habilidades", href: "#habilidades" },
  { name: "Intereses", href: "#intereses" },
];

// =========================================================
// 3. FUNCIONES DE MANEJO DE CLICKS
// =========================================================

//  Maneja el clic en el botón CONTACTO (Scroll y Emit)
function handleContactoClick() {
  // Desplaza la vista al área del perfil para asegurar que la tarjeta sea visible
  const profileArea = document.getElementById("profile-area-target");
  if (profileArea) {
    // 'block: center' intenta centrar la tarjeta en la vista
    profileArea.scrollIntoView({ behavior: "smooth", block: "center" });
  }
  // Emite el evento al componente padre (App.vue) para que este voltee la tarjeta
  emit("contacto-click");
}

/**
 * Función para desplazarse a la sección objetivo (para los demás enlaces).
 * Compensada para la altura del Navbar fijo (80px).
 */
function scrollToSection(id) {
  const targetElement = document.getElementById(id);

  const NAVBAR_HEIGHT = 80;

  if (targetElement) {
    const targetPosition = targetElement.offsetTop - NAVBAR_HEIGHT;

    window.scrollTo({
      top: targetPosition,
      behavior: "smooth",
    });
  } else {
    window.scrollTo({ top: 0, behavior: "smooth" });
    console.warn(`Advertencia: Elemento con ID '${id}' no encontrado. Desplazando al inicio.`);
  }
}
</script>

<style scoped>
/* ------------------------------------------------------------- */
/* CONTENEDOR Y LISTA BASE */
/* ------------------------------------------------------------- */
.navbar-content {
  width: 100%;
  height: auto;
  padding: 15px 20px;
  box-sizing: border-box;
  position: relative;
  z-index: 10;
  background-color: var(--vt-c-black, #03091b);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.nav-wrapper {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20px;
  max-width: 1200px;
  width: 100%;
}

.nav-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: row;
  gap: 10px;
  flex-wrap: wrap;
  justify-content: center;
  flex-grow: 1;
}

.nav-item {
  text-align: center;
}

/* ------------------------------------------------------------- */
/* ESTILOS NEÓN BASE (Púrpura / Cian) */
/* ------------------------------------------------------------- */

/* Estilos de botones Neón base (Aplicado a <a> y <button>) */
.neon-button {
  display: inline-block;
  padding: 8px 15px;
  text-decoration: none;
  font-family: "Inter", sans-serif;
  font-weight: 500;
  font-size: 0.9rem;
  color: var(--color-text-light);
  background-color: transparent;

  /* Borde y Sombra: PÚRPURA NEÓN vibrante */
  border: 2px solid var(--vt-c-indigo);
  box-shadow: 0 0 8px var(--vt-c-indigo);
  transition: all 0.3s ease-in-out;
  cursor: pointer;
}

/* Efecto Hover para botones base */
.neon-button:hover {
  color: var(--vt-c-cyan);
  background-color: var(--vt-c-black-soft-rgba-50);
  /* Sombra y Borde: CIAN NEÓN vibrante */
  border-color: var(--vt-c-cyan);
  box-shadow: 0 0 10px var(--vt-c-cyan), 0 0 20px var(--vt-c-cyan-rgba-05);
  transform: translateY(-2px);
}

/* ------------------------------------------------------------- */
/* ESTILOS ESPECÍFICOS DEL BOTÓN CONTACTO (Magenta) */
/* ------------------------------------------------------------- */

.contacto-list-item {
  margin-left: 10px;
}

.contacto-button {
  font-weight: 700;
  /* Borde y Sombra: MAGENTA NEÓN vibrante */
  border-color: var(--vt-c-magenta);
  box-shadow: 0 0 8px var(--vt-c-magenta);
  color: var(--vt-c-magenta);
}

/* Efecto Hover para CONTACTO */
.contacto-button:hover {
  border-color: var(--vt-c-magenta);
  color: var(--vt-c-white);
  /* Sombra: MAGENTA NEÓN más intensa */
  box-shadow: 0 0 15px var(--vt-c-magenta), 0 0 30px var(--vt-c-magenta-rgba-07);
}
/* ------------------------------------------------------------- */
/* ESTILOS DEL BOTÓN HAMBURGUESA (NUEVO) */
/* ------------------------------------------------------------- */

/* Oculto por defecto en escritorio/tablet */
.hamburger-button {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 10px;
  z-index: 20;
  /* ELIMINAR position: relative; de aquí */
}

.hamburger-icon {
  display: block; /* Debe ser block para tener ancho y alto */
  width: 25px; /* Define el ancho de la barra central */
  height: 3px; /* Define la altura de la barra central */
  background-color: var(--vt-c-cyan);
  border-radius: 3px;
  transition: all 0.3s ease-in-out;
  position: relative;
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: "";
  position: absolute;
  /* width/height/background/radius/transition ya están definidos arriba */
  width: 25px; /* Asegura el ancho de las barras */
  height: 3px; /* Asegura la altura de las barras */
  background-color: var(--vt-c-cyan);
  border-radius: 3px;
  transition: all 0.3s ease-in-out;

  left: 0;
}

.hamburger-icon::before {
  transform: translateY(-8px);
}

.hamburger-icon::after {
  transform: translateY(8px);
}

/* Efecto X al abrir el menú */
.hamburger-button.is-active .hamburger-icon {
  background-color: transparent;
}

.hamburger-button.is-active .hamburger-icon::before {
  transform: rotate(45deg);
}

.hamburger-button.is-active .hamburger-icon::after {
  transform: rotate(-45deg);
}

/* ------------------------------------------------------------- */
/* RESPONSIVE DESIGN (Móvil) */
/* ------------------------------------------------------------- */

@media (max-width: 850px) {
  .nav-wrapper {
    flex-direction: column;
    gap: 10px;
  }

  .nav-list {
    justify-content: center;
    width: 100%;
  }

  .neon-button {
    font-size: 0.85rem;
    padding: 6px 10px;
  }

  .contacto-list-item {
    margin-left: 0;
    width: 100%;
  }
  .contacto-button {
    width: 80%;
    margin: 0 auto;
  }
}
@media (max-width: 600px) {
  .nav-wrapper {
    /* Fuerza a que los elementos estén en una fila para alinear logo/hamburguesa */
    flex-direction: row;
    justify-content: flex-end; /* Mueve la hamburguesa a la derecha */
    gap: 0;
  }

  /* Muestra el botón de hamburguesa */
  .hamburger-button {
    display: block;
    background-color: aquamarine;
    border: 2px solid transparent;
    /* Degradado de borde: Usa variables globales corregidas */
    background: linear-gradient(var(--vt-c-black-soft), var(--vt-c-black-soft)) padding-box,
      linear-gradient(
        90deg,
        var(--vt-c-magenta-pure) 0%,
        /* Magenta Puro */ var(--color-neon-cyan) 50%,
        /* Cian Neón */ var(--vt-c-indigo) 100% /* Índigo/Morado */
      );
  }

  /* Oculta y posiciona el menú desplegable por defecto */
  .nav-list {
    display: none; /* OCULTA la lista de enlaces */
    position: absolute;
    top: 100%; /* Justo debajo del navbar */
    left: 0;
    width: 100%;
    flex-direction: column; /* Apila los enlaces */
    background-color: transparent;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.5);
    padding: 20px 0;
    z-index: 15;
  }

  /* Muestra el menú cuando tiene la clase 'mobile-open' */
  .nav-list.mobile-open {
    display: flex;
  }

  /* Ajustes de los ítems del menú móvil */
  .nav-item {
    width: 30%;
  }

  .neon-button {
    width: 90%;
    margin: 5px auto;
  }

  .contacto-list-item {
    margin-left: 0;
    margin-top: 10px;
  }
}

/* Mantenemos tu antigua regla para el rango 601px-850px si es necesario, 
   pero quitamos la columna para evitar el apilamiento feo. 
   Simplemente hacemos los botones más pequeños y permitimos que se envuelvan (wrap). */
@media (max-width: 850px) and (min-width: 601px) {
  .nav-wrapper {
    flex-direction: row;
  }
  .nav-list {
    justify-content: center;
  }

  .neon-button {
    font-size: 0.85rem;
    padding: 6px 10px;
  }
}

/* ------------------------------------------------------------- */
/* ZONA DE TRUCOS: Definición de Variables RGBA (para sombras) */
/* ------------------------------------------------------------- */

:root {
  /* PÚRPURA NEÓN (#a855f7) */
  --vt-c-indigo-rgba: rgba(168, 85, 247, 0.3);

  /* CIAN NEÓN (#00ffff) */
  --vt-c-cyan-rgba-05: rgba(0, 255, 255, 0.5);

  /* MAGENTA NEÓN (#ff00ff) */
  --vt-c-magenta-rgba-07: rgba(255, 0, 255, 0.7);

  /* Fondo negro suave */
  --vt-c-black-soft-rgba-50: rgba(3, 9, 27, 0.5);
}
</style>
