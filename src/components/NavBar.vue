<template>
  <!--Barra de Navegacion(de nuestra cabecera)-->
  <nav class="navbar">
    <div class="navbar-menu">
      <ul>
        <!-- enlace de valor dinámico `nav.enlace` -->
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
/*Ajustes de altura y estilo neón*/

.navbar {
 
  background-color: var(--vt-c-black-soft); /* color oscuro neón de la paleta para mayor contraste. */
  padding: 0.3rem 1rem; /* Reduce el padding vertical a 0.3rem  para ahorrar espacio. */
  color: #fff;
  align-items: center;
  border-bottom: 2px solid var(--vt-c-indigo);/*  borde inferior de neón para darle estilo */
  box-shadow: 0 4px 15px rgba(216, 7, 106, 0.4);
  transition: box-shadow 0.3s ease-in-out; /* AGREGADO: TRANSICIÓN PARA EL EFECTO DE LUZ DE LA BARRA */
}
/* estilo luz al pasar el mouse por la barra */
.navbar:hover {
    /* sombra base y l un brillo neón suave */
    box-shadow: 
        /* Sombra base */
        0 4px 15px rgba(216, 7, 106, 0.4),
        /* Resplandor neón magenta/cian (simulando el borde de la tarjeta) */
        0 0 15px var(--vt-c-indigo),
        0 0 30px var(--vt-c-cyan);
}

.navbar-menu {
  display: flex;
  justify-content: flex-end;
}

.navbar-menu ul {
  list-style: none; /* Elimina viñetas */
  display: flex; /* Alinea los elementos de la lista horizontalmente */
  gap: 0.5rem; /* Espacio entre enlaces */
  padding: 0;
  margin: 0;
}

/* estilo de base neon y efecto 3 d */

.nav-item {
  color: var(--vt-c-white); /* Color de texto suave */
  text-decoration: none;
  margin: 0 0.5rem; 
  padding: 0.5rem 0.8rem;
  border: none; /* Borde sutil */
  border-radius: 5px;
  transition: all 0.2s ease-in-out; /* Transición suave para todos los cambios (sombra, color, posición) */
  position: relative; /* Posicion para que la elevación funcione correctamente */
  font-family: 'Orbitron', sans-serif; 
  font-weight: 500;
  font-size: 0.9rem;
}

.nav-item:hover {
  /* efecto de elevacion y brillo*/
  
  /* 1. Elevacion: Mueve el elemento 3px hacia arriba */
  transform: translateY(-3px); 
  /* 2. TEXTO: Se vuelve cian y tiene un brillo más intenso */
  color: var(--vt-c-cyan); 
  text-shadow: 0 0 8px var(--vt-c-cyan), 0 0 15px rgba(0, 255, 255, 0.6);
  /* 3. SOMBRA DE PROYECCIÓN/BRILLO DE BORDE */
  box-shadow: 
    0 5px 10px rgba(0, 0, 0, 0.5), /* Sombra de Proyección sutil (efecto 3D hacia abajo) */
    /* Borde Iluminado de Neón (Cian y Rosa) */
    0 0 5px var(--vt-c-cyan),
    0 0 20px var(--vt-c-indigo); 

  /* 4. fondo transparente */
  background-color: transparent; 
  border-color: var(--vt-c-cyan); /* El borde se ilumina ligeramente */
}


/* MEDIA QUERY(Responsivo) */

@media (max-width: 768px){
  .navbar-menu {
    display:flex;
    justify-content: center; /* Centrar en celular */
    width: 100%;
  }
  .nav-item {
    font-size: 0.7rem;
    margin: 0 0.2rem;
    padding: 0.4rem 0.6rem;
  }
}
</style>
