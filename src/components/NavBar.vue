<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['contacto-click']);

// Mantuvimos tu estructura de navItems
const navItems = [
 { name: 'Educación', href: '#educacion' },
 { name: 'Experiencia', href: '#experiencia' },
 { name: 'Proyectos', href: '#proyectos' },
 { name: 'Habilidades', href: '#habilidades' },
 { name: 'Intereses', href: '#intereses' } 
];

function handleContactoClick() {
  // El botón Contacto lleva al área del perfil para voltear la tarjeta
    const profileArea = document.getElementById('profile-area-target');
    if (profileArea) {
        profileArea.scrollIntoView({ behavior: 'smooth', block: 'center' });
    }
    emit('contacto-click');
}

/**
 * Función para desplazarse a la sección objetivo.
 * Vuelve a ser un desplazamiento estándar.
 */
function scrollToSection(id) {
    const targetElement = document.getElementById(id);
    if (targetElement) {
        // Desplazamiento suave al inicio de la sección. No restamos altura.
        window.scrollTo({
            top: targetElement.offsetTop, 
            behavior: 'smooth'
        });
    } else {
        console.error(`Error: Elemento con ID '${id}' no encontrado.`);
    }
}
</script>

<template>
 <nav class="navbar-content">
  <div class="nav-wrapper">
   <ul class="nav-list">
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
            <button @click="handleContactoClick" class="neon-button contacto-button">
                CONTACTO
            </button>
        </li>
   </ul>
  </div>
 </nav>
</template>

<style scoped>
/* Contenedor Principal: Vuelve a estar en el flujo normal */
.navbar-content {
 width: 100%;
 height: auto; 
 padding: 15px 20px; 
 box-sizing: border-box;
    /* CLAVE: Eliminamos las propiedades 'fixed' */
    position: relative; /* Vuelve a su posición normal */
    top: auto;
    left: auto;
    z-index: 10; 
    background-color: var(--vt-c-black, #03091b); 
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Mantenemos una sombra suave */

 display: flex;
 justify-content: center; 
 align-items: flex-start; 
}

/* WRAPPER (Mantenido) */
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

/* Estilos de botones (Mantenidos) */
.neon-button {
 display: inline-block; 
 padding: 8px 15px; 
 text-decoration: none;
 font-family: 'Inter', sans-serif;
 font-weight: 500;
 font-size: 0.9rem;
 color: var(--vt-c-text-light); 
 background-color: transparent; 
 
 border: 1px solid var(--vt-c-indigo);
 border-radius: 8px;
 box-shadow: 0 0 5px rgba(168, 85, 247, 0.3); 
 transition: all 0.3s ease-in-out;
 cursor: pointer;
}

.neon-button:hover {
 color: var(--vt-c-cyan);
 background-color: rgba(3, 9, 27, 0.5); 
 border-color: var(--vt-c-cyan);
 box-shadow:
  0 0 8px var(--vt-c-cyan),
  0 0 15px rgba(0, 255, 255, 0.5);
 transform: translateY(-2px);
}

.contacto-list-item {
    margin-left: 10px; 
}

.contacto-button {
 font-weight: 700;
 border-color: var(--vt-c-magenta-original); 
 box-shadow: 0 0 5px var(--vt-c-magenta-original);
 color: var(--vt-c-magenta-original);
}

.contacto-button:hover {
 border-color: var(--vt-c-magenta-original);
 color: white; 
 box-shadow:
  0 0 10px var(--vt-c-magenta-original),
  0 0 20px rgba(255, 20, 147, 0.7);
}

/* Responsive Design (Mantenido) */
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
</style>