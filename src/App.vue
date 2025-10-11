<script setup>
// Importamos los componentes de tu versión actual
import NavBar from './components/NavBar.vue'
import DatosPersonales from './components/DatosPersonales.vue'

// Importamos el componente de la sección de Educación con el nombre multi-palabra
import EducacionCursos from './components/EducacionCursos.vue' 
</script>

<template>
  <header>
    <NavBar/>
    
    <DatosPersonales/>
  </header>

  <main>
    <!-- Se reemplaza el contenido placeholder por el componente EducacionCursos -->
    <section id="educacion">
      <EducacionCursos />
    </section>
    
    <section id="experiencia"><h2>Experiencia</h2></section>
    <section id="proyectos"><h2>Proyectos</h2></section>
    <section id="habilidades"><h2>Habilidades</h2></section>
    <section id="intereses"><h2>Intereses</h2></section>
  </main>
</template>

<style scoped>
/* No se necesitan estilos con scope en App.vue, ya que main.css maneja el layout general */
</style>