<script setup>
import { ref } from 'vue'; 
import NavBar from './components/NavBar.vue' 
import DatosPersonales from './components/DatosPersonales.vue'
import EducacionCursos from './components/EducacionCursos.vue' 

const datosPersonalesRef = ref(null); 

function handleContactoClick() {
    // Lógica para voltear la tarjeta de perfil
    const cardElement = document.getElementById('perfil-card-target');
    
    if (cardElement) {
        const rect = cardElement.getBoundingClientRect();
        const sidebarWidth = 200; 
        
        const isVisible = rect.top >= 50 && 
                          rect.bottom <= window.innerHeight - 50 &&
                          rect.left >= sidebarWidth; 

        if (isVisible) {
            if (datosPersonalesRef.value && typeof datosPersonalesRef.value.flipCard === 'function') {
                datosPersonalesRef.value.flipCard();
            }
        } else {
            cardElement.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }
    } else {
        console.error("Error: Elemento con ID 'perfil-card-target' no encontrado. ¿Está en DatosPersonales.vue?");
    }
}
</script>

<template>
    <header class="sidebar-container">
        <NavBar @contacto-click="handleContactoClick" />
    </header>
    
    <div class="content-wrapper">
        <div class="profile-area">
            <DatosPersonales ref="datosPersonalesRef" />
        </div>

        <main class="scrollable-content">
            <section id="educacion" class="portfolio-section full-viewport-section">
                <EducacionCursos />
            </section>
            
            <section id="experiencia" class="portfolio-section full-viewport-section centered-content">
                <h2 class="section-title">Experiencia Laboral</h2>
                <div class="placeholder-card">
                    <p>Esta sección se cargará una vez que generemos ExperienciaLaboral.vue.</p>
                </div>
            </section>
        </main>
    </div>
</template>

<style scoped>
/* ------------------------------------------------------------- */
/* ESTILOS DE LAYOUT CRUCIALES (SOLUCIÓN BARRA FIJA Y NEÓN) */
/* ------------------------------------------------------------- */

/* 1. BARRA LATERAL: FIJA Y CON ESTILO NEON DE TARJETA */
.sidebar-container {
    position: fixed; /* CLAVE: Mantiene la barra pegada al viewport */
    top: 20px; /* Margen de la parte superior */
    left: 20px; /* Margen de la parte izquierda */
    width: 200px; 
    height: calc(100vh - 40px); /* 100% de la altura menos los 20px de top y 20px de bottom */
    z-index: 100; 
    
    /* ESTILO NEÓN DE TARJETA */
    background: var(--vt-c-card-dark); 
    border: 2px solid var(--vt-c-indigo); 
    border-radius: 16px;
    box-shadow: 
        0 0 10px rgba(168, 85, 247, 0.7), 
        0 0 20px rgba(168, 85, 247, 0.3), 
        inset 0 0 5px rgba(168, 85, 247, 0.5); 

    overflow-y: auto; /* Permite scroll interno si la barra es demasiado pequeña */
}

/* 2. CONTENEDOR DE CONTENIDO (LADO DERECHO) */
.content-wrapper {
    /* CLAVE: Empuja el contenido 240px a la derecha (200px barra + 20px margen izq + 20px separación) */
    margin-left: 240px; 
    width: calc(100% - 240px);
    display: grid;
    grid-template-columns: 1fr; 
    grid-template-rows: 100vh auto; 
    min-height: 100vh;
}

/* 3. ÁREA DE PERFIL (TARJETA): Fila 1 */
.profile-area {
    grid-row: 1 / 2; 
    width: 100%;
    height: 100vh; 
    display: flex; 
    align-items: center; 
    justify-content: center; 
    padding: 20px;
}

/* 4. CONTENIDO SCROLLABLE: Fila 2 */
.scrollable-content {
    grid-row: 2 / 3; 
    width: 100%; 
    padding-left: 10px; 
    padding-right: 10px;
}

/* ... (Otros estilos auxiliares) ... */
.full-viewport-section {
    min-height: auto; 
    padding-top: 5rem; 
    padding-bottom: 5rem;
}
.centered-content {
    display: flex;
    flex-direction: column;
    justify-content: center; 
    align-items: center;
}
.portfolio-section {
    padding: 3rem 1rem;
    border-bottom: 1px dashed rgba(168, 85, 247, 0.2); 
    margin-bottom: 2rem;
}
.section-title {
    font-family: 'Inter', sans-serif;
    font-size: 2rem;
    font-weight: 700;
    color: var(--vt-c-indigo); 
    text-align: center;
    margin-bottom: 2.5rem;
    text-shadow: 
     0 0 5px var(--vt-c-indigo), 
     0 0 15px rgba(168, 85, 247, 0.5);
}
.placeholder-card {
    background-color: var(--vt-c-black-soft);
    border: 1px dashed var(--vt-c-indigo);
    border-radius: 10px;
    padding: 2rem;
    text-align: center;
    color: var(--vt-c-white);
}
@media (max-width: 600px) {
    .portfolio-section {
        padding: 2rem 0.5rem;
    }
    .section-title {
        font-size: 1.5rem;
    }
}
</style>