<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['contacto-click']);

const navItems = [
    { name: 'Educación', href: '#educacion' },
    { name: 'Experiencia', href: '#experiencia' },
    { name: 'Proyectos', href: '#proyectos' },
    { name: 'Habilidades', href: '#habilidades' },
    { name: 'Intereses', href: '#intereses' }
];

function handleContactoClick() {
    emit('contacto-click');
}
</script>

<template>
    <nav class="navbar-content">
        <div class="nav-wrapper"> 
            <ul class="nav-list">
                <li v-for="item in navItems" :key="item.name" class="nav-item">
                    <a :href="item.href" class="neon-button">
                        {{ item.name }}
                    </a>
                </li>
            </ul>
            
            <div class="contacto-area">
                <button @click="handleContactoClick" class="neon-button contacto-button">
                    CONTACTO
                </button>
            </div>
        </div>
    </nav>
</template>

<style scoped>
/* 1. Contenedor Principal: Ocupa el espacio del padre (200px fijo en App.vue) */
.navbar-content {
    width: 100%; 
    height: 100%; 
    padding: 20px 10px; 
    box-sizing: border-box; 
    display: flex;
    flex-direction: column;
}

/* 2. WRAPPER para el control de espaciado */
.nav-wrapper {
    display: flex;
    flex-direction: column;
    gap: 30px; /* Espacio entre el grupo de botones principales y el botón CONTACTO */
    padding-top: 20px; 
}

.nav-list {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 15px; /* Espacio entre los botones principales */
}

.nav-item {
    text-align: center;
}

/* 3. Estilos de botones (Ajuste de transparencia y sombra) */
.neon-button {
    display: block;
    width: 100%;
    padding: 10px 0;
    text-decoration: none;
    font-family: 'Inter', sans-serif;
    font-weight: 500;
    font-size: 1rem;
    color: var(--vt-c-white-soft);
    background-color: transparent; /* CLAVE: Fondo transparente para ver el fondo de la barra */
    
    /* Efecto Neón del borde y la sombra */
    border: 1px solid var(--vt-c-indigo);
    border-radius: 8px;
    box-shadow: 0 0 5px rgba(168, 85, 247, 0.3); /* Sombra más sutil */
    transition: all 0.3s ease-in-out;
    cursor: pointer;
}

.neon-button:hover {
    color: var(--vt-c-cyan);
    background-color: var(--vt-c-black-mute);
    border-color: var(--vt-c-cyan);
    box-shadow: 
        0 0 8px var(--vt-c-cyan),
        0 0 15px rgba(0, 255, 255, 0.5);
    transform: translateY(-2px);
}

.contacto-area {
    padding-top: 0; 
}

.contacto-button {
    font-weight: 700;
    border-color: var(--vt-c-magenta-original); 
    box-shadow: 0 0 5px var(--vt-c-magenta-original);
    color: var(--vt-c-magenta-original);
}

.contacto-button:hover {
    border-color: var(--vt-c-magenta-original);
    color: var(--vt-c-white);
    box-shadow: 
        0 0 10px var(--vt-c-magenta-original),
        0 0 20px rgba(255, 20, 147, 0.7);
}
</style>

