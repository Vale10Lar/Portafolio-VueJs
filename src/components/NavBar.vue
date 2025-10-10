<template>
  <nav class="navbar">
    <div class="navbar-menu">
      <ul>
        <a v-for="nav in navegacion" :key="nav.nombre" :href="nav.enlace" class="nav-item">{{
          nav.nombre}}</a>
      </ul>
    </div>
  </nav>
</template>

<script setup>
import { ref } from "vue";
const navegacion = ref([
  { id:1, nombre: "Educación", enlace: "#educacion"},
  { id:2, nombre: "Experiencia", enlace: "#experiencia"},
  { id:3, nombre: "Proyectos", enlace: "#proyectos" },
  { id:4, nombre: "Habilidades", enlace: "#habilidades"},
  { id:5, nombre: "Intereses", enlace: "#intereses"},
]);
</script>

<style scoped>
/* =======================================
   ESTILOS ESPECÍFICOS DE LA BARRA DE NAVEGACIÓN
   ======================================= */

.navbar {
  /* CRÍTICO: DEGRADADO (De Morado Neón a Oscuro) */
  background: linear-gradient(90deg, var(--vt-c-indigo) 10%, var(--vt-c-black-soft) 95%);
  
  /* Borde, Sombras y Diseño */
  padding: 15px 20px;
  border-radius: 20px;
  box-shadow: 
    0 4px 15px rgba(0, 0, 0, 0.5), 
    0 0 5px rgba(0, 255, 255, 0.3);

  border: none; 
  border-image: none; 
  transition: box-shadow 0.3s ease-in-out; 
}

/* Efecto de luz al pasar el mouse por la barra (Contenedor) */
.navbar:hover {
  box-shadow: 
    0 4px 15px rgba(216, 7, 106, 0.4), 
    0 0 15px var(--vt-c-indigo), 
    0 0 30px var(--vt-c-cyan); 
}

.navbar-menu {
  display: flex;
  justify-content: flex-end; /* CENTRAMOS EL MENU ENTERO EN LA BARRA */
}

.navbar-menu ul {
  list-style: none; 
  display: flex; 
  gap: 1.5rem; /* CRÍTICO: AUMENTAMOS EL ESPACIADO PARA QUE NO ESTÉN PEGADOS */
  padding: 0;
  margin: 0;
  align-items: center;
}

/* ESTILO BASE DE LOS BOTONES DE NAVEGACIÓN (.nav-item) */
.nav-item {
  background: linear-gradient(
    180deg, 
    rgba(30, 10, 50, 0.7), 
    rgba(41, 22, 83, 0.7) 
  ); 
  
  border-radius: 5px; 
  color: var(--vt-c-white); 
  text-decoration: none; 
  padding: 8px 15px; 
  display: inline-block; 
  
  font-family: 'Orbitron', sans-serif; 
  font-weight: 500;
  font-size: 0.9rem;
  letter-spacing: 1px; 
  
  /* Borde punteado neón */
  border: 1px dashed rgba(255, 255, 255, 0.3);
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.5); 
  transition: all 0.2s ease-in-out; 
  
  text-shadow: 
    0 0 1px var(--vt-c-cyan), 
    0 0 1px var(--vt-c-cyan); 

  position: relative; 
}

/* EFECTO HOVER: Elevación y Brillo Cian/Morado */
.nav-item:hover {
  transform: translateY(-3px); 
  
  color: var(--vt-c-cyan); 
  text-shadow: 0 0 8px var(--vt-c-cyan), 0 0 15px rgba(0, 255, 255, 0.6);
  
  box-shadow: 
    0 5px 10px rgba(0, 0, 0, 0.5), 
    0 0 5px var(--vt-c-cyan), 
    0 0 20px var(--vt-c-indigo); 

  background-color: linear-gradient(180deg, rgba(30, 10, 50, 0.9), rgba(41, 22, 83, 0.9));
  border-color: var(--vt-c-cyan); 
}

/* MEDIA QUERY (Responsivo) */
@media (max-width: 768px){
  .navbar-menu ul {
    gap: 0.5rem; /* Reducimos el espaciado en móviles */
  }
  .nav-item {
    font-size: 0.7rem;
    padding: 0.4rem 0.6rem;
  }
}
</style>
