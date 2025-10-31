<script setup>

import { computed, ref, onMounted, onUnmounted, defineExpose } from 'vue';



// Importaciones de imágenes y SVGs de Redes Sociales

import fotoPerfil from '../assets/valeria_profile.jpg';

import valeriaLogo from '../assets/valeria_logo_neon.png';

import iconLinkedIn from '../assets/in_linked_linkedin_media_social_icon_124259.svg';

import iconGitHub from '../assets/github_git_icon_145985.svg';

import iconInstagram from '../assets/instagram_108043.svg';

import iconCV from '../assets/curriculum_vitae_paper_document_icon_219507.svg';

import iconWhatsAppSocial from '../assets/whatsapp.svg';



// ÍCONOS DE CONTACTO (Usando SVGs locales)

import iconPhone from '../assets/phone_78665.svg';

import iconEmail from '../assets/email_icon_133230.svg';

import iconLocation from '../assets/location_icon_159350.svg';



// Estado y Datos

const nombreCompleto = 'Valeria Elena LARDÍN';

const titulo = 'Técnico Universitario en Programación-UTN';

const descripcion = 'Hola, Bienvenido a mi portafolio de Proyectos. Soy un desarrollador Full Stack con experiencia en desarrollo de aplicaciones web y backend';



// Estado para la animación de la tarjeta giratoria

const isFlipped = ref(false);



// Estado para la foto (oculta por defecto, se revela al hacer hover en el nombre en la cara posterior)

const isHovered = ref(false);



const nombreDisplayClass = computed(() => ({

'nombre-titulo': true,

'hover-active': isHovered.value

}));



// Datos de Contacto usando SVGs locales

const contacto = [

{ type: 'Cel', value: '+54 9 2622-641424', href: 'tel:+5492622641424', src: iconPhone, color: 'text-blue-neon' },

{ type: 'Correo', value: 'valelar81@gmail.com', href: 'mailto:valelar81@gmail.com', src: iconEmail, color: 'text-pink-neon' },

{ type: 'Dirección', value: 'Tupungato, Mendoza, Argentina', href: '#', src: iconLocation, color: 'text-blue-neon' },

];



// Redes Sociales

const redesSociales = [

{ name: 'LinkedIn', url: 'https://www.linkedin.com/in/valeria-lardin-833984370/', src: iconLinkedIn, shadow: '#0077B5' },

{ name: 'WhatsApp', url: 'https://wa.me/5492622641424', src: iconWhatsAppSocial, shadow: '#14D1FF' },

{ name: 'GitHub', url: 'https://github.com/Vale10Lar', src: iconGitHub, shadow: '#E8E8E8' },

{ name: 'Instagram', url: 'https://www.instagram.com/vale_.lar/', src: iconInstagram, shadow: '#FF1493' },

{ name: 'CV', url: '#', src: iconCV, shadow: '#A855F7' }

];



function flipCard() {

isFlipped.value = !isFlipped.value;

}



// Hacemos la función flipCard() accesible para el componente padre (App.vue)

defineExpose({

  flipCard

});





// LÓGICA DEL RASTRO DE PARTÍCULAS

const spawnParticle = (x, y) => {

 const particle = document.createElement('div');

 particle.className = 'cursor-particle';



 const isPink = Math.random() > 0.5;

 particle.style.backgroundColor = isPink ? '#FF1493' : '#14D1FF';

 particle.style.boxShadow = isPink

  ? '0 0 5px #FF1493, 0 0 10px #FF1493'

  : '0 0 5px #14D1FF, 0 0 10px #14D1FF';



 particle.style.left = `${x}px`;

 particle.style.top = `${y}px`;



 const size = Math.random() * 5 + 2;

 particle.style.width = `${size}px`;

 particle.style.height = `${size}px`;



 const angle = Math.random() * 2 * Math.PI;

 const distance = Math.random() * 40 + 20;



 const finalX = x + Math.cos(angle) * distance;

 const finalY = y + Math.sin(angle) * distance;

 const duration = Math.random() * 1.5 + 0.5;



 document.body.appendChild(particle);



 setTimeout(() => {

  particle.style.transition = `all ${duration}s ease-out`;

  particle.style.transform = `translate(${finalX - x}px, ${finalY - y}px) scale(0)`;

  particle.style.opacity = '0';

 }, 10);



 setTimeout(() => {

  particle.remove();

 }, duration * 1000 + 100);

};



let lastSpawn = 0;

const spawnInterval = 50;



const handleMouseMove = (event) => {

 const now = Date.now();

 if (now - lastSpawn > spawnInterval) {

  spawnParticle(event.clientX, event.clientY);

  lastSpawn = now;

 }

};



onMounted(() => {

 window.addEventListener('mousemove', handleMouseMove);

});



onUnmounted(() => {

 window.removeEventListener('mousemove', handleMouseMove);

});

// FIN DE LA LÓGICA DEL RASTRO DE PARTÍCULAS

</script>





<template>

<div id="perfil-card-target" class="datos-personales-container">

<div class="flip-card-outer" :class="{ 'flipped': isFlipped }" @click="flipCard">



<div class="perfil-card flip-card-inner">



 <div class="flip-card-front">

  <div class="logo-area animated-pulse">

   <img :src="valeriaLogo" alt="Logo Valeria Lardín" class="main-logo-image" />

  </div>

 </div>



 <div class="flip-card-back">



  <div :class="['foto-perfil-ancla', { 'foto-visible': isHovered }]">

   <img :src="fotoPerfil" alt="Foto de Perfil de Valeria Lardín" class="foto-perfil">

  </div>



  <h1

   :class="nombreDisplayClass"

   @mouseover="isHovered = true"

   @mouseleave="isHovered = false"

  >

   {{ nombreCompleto }}

  </h1>



  <h2 class="subtitulo neon-pulsante">{{ titulo }}</h2>



  <p class="descripcion-parrafo">

   {{ descripcion }}

  </p>



  <div class="contacto-wrapper">

   <a

    v-for="item in contacto"

    :key="item.type"

    :href="item.href"

    target="_blank"

    :class="['contacto-item', item.color]"

    :title="item.type + ': ' + item.value"

   >

    <img :src="item.src" :alt="item.type" class="contacto-icon svg-icon-fix" />

  

    <span class="contacto-text">

     <span class="contacto-value">{{ item.value }}</span>

    </span>

   </a>

  </div>



  <div class="redes-sociales">

   <a

    v-for="red in redesSociales"

    :key="red.name"

    :href="red.url"

    target="_blank"

    :aria-label="red.name"

    class="red-social-link"

   >

    <div class="social-circle" :style="{ '--shadow-color': red.shadow }">

     <img :src="red.src" :alt="red.name" class="social-icon" />

    </div>

   </a>

  </div>

 </div>

</div>

</div>

</div>
</template>

<style scoped>
/* Las variables de color globales (ej. --vt-c-indigo) están en base.css */
:root {
/* CLAVE: Limita el ancho de la tarjeta para que no ocupe todo el espacio. */
--card-max-width: 750px; 
}


/* Colores Neón Personalizados */

.text-pink-neon { color: #FF1493; text-shadow: 0 0 5px #FF1493; }
.text-blue-neon { color: #14D1FF; text-shadow: 0 0 5px #14D1FF; }
.text-purple-neon { color: #A855F7; text-shadow: 0 0 5px #A855F7; }

/* Variables CSS para las sombras de los círculos de redes */
:root {
--shadow-color: #A855F7;
}

/* Animaciones */
@keyframes pulse-border {
0% { box-shadow: 0 0 10px rgba(168, 85, 247, 0.7), inset 0 0 5px rgba(168, 85, 247, 0.5); }
100% { box-shadow: 0 0 25px rgba(168, 85, 247, 0.9), inset 0 0 10px rgba(168, 85, 247, 0.7); }

}

@keyframes neon-pulse-subtitulo {
0%, 100% {
text-shadow: 0 0 3px #A855F7, 0 0 6px rgba(168, 85, 247, 0.7);

}
50% {
text-shadow: 0 0 5px #A855F7, 0 0 10px rgba(168, 85, 247, 1);
}

}

/* Animación para el Logo en el Frente */
@keyframes pulse-scale {

 0% { transform: scale(1); box-shadow: 0 0 10px #FF1493, 0 0 30px #A855F7; }
 50% { transform: scale(1.05); box-shadow: 0 0 20px #FF1493, 0 0 50px #A855F7; }
 100% { transform: scale(1); box-shadow: 0 0 10px #FF1493, 0 0 30px #A855F7; }

}

/* CONTENEDOR PRINCIPAL Y DISEÑO DE LA TARJETA PERFIL */

.datos-personales-container {
width: 100%;
max-width: var(--card-max-width); /* APLICACIÓN DEL LÍMITE */
perspective: 1000px; 
height: 100%; 
box-sizing: border-box;

}

.flip-card-outer {
width: 100%;
height: 100%;
}

.perfil-card {
width: 100%;
height: 100%;
position: relative;
transform-style: preserve-3d;
transition: transform 0.8s;
/* FONDO: SIMULACIÓN DE CIRCUITO NEÓN SUTIL */
background:
 /* 1. Degradado para dar profundidad (es la capa más cercana al usuario) */

 linear-gradient(135deg, rgba(16, 15, 32, 0.95) 0%, rgba(10, 10, 21, 0.95) 100%),
 /* 2. Patrón de puntos/líneas neón (Circuitos abstractos) */
 repeating-radial-gradient(
  circle at center,
  rgba(255, 20, 147, 0.05) 0%, /* Rosa muy sutil */
  rgba(20, 209, 255, 0.05) 1px, /* Azul muy sutil */
  transparent 50px,
  transparent 70px
 ),

 /* 3. Fondo oscuro base */

 #0a0a0a;
border: 2px solid var(--vt-c-indigo);
border-radius: 16px;
padding: 1.5rem 2.5rem ;
text-align: center;
/* box-shadow externo para el brillo general */
box-shadow: 0 0 20px rgba(168, 85, 247, 0.7);
animation: pulse-border 5s infinite alternate;
min-height: 350px;

}

/* CLASE PARA LA ROTACIÓN */
.flip-card-outer.flipped .perfil-card {
transform: rotateY(180deg);
}



.flip-card-front, .flip-card-back {
position: absolute;
width: 100%;
height: 100%;
-webkit-backface-visibility: hidden;
backface-visibility: hidden;
top: 0;
left: 0;
display: flex;
flex-direction: column;
justify-content: center;
align-items: center;
padding: 1rem 2rem 1.5rem 2rem;

}



.flip-card-back {
transform: rotateY(180deg);
position: absolute; /* CLAVE: DEBE SER ABSOLUTE para el volteo 3D */

/* FONDO CARA TRASERA (igual que el del frente)*/
background:

 linear-gradient(135deg, rgba(16, 15, 32, 0.95) 0%, rgba(10, 10, 21, 0.95) 100%),

 repeating-radial-gradient(

  circle at center,

  rgba(255, 20, 147, 0.05) 0%,

  rgba(20, 209, 255, 0.05) 1px,

  transparent 50px,

  transparent 70px

 ),

 #0a0a0a;
padding: 1rem 2rem 1.5rem 2rem;

}



/* ESTILOS CARA FRONTAL */

.logo-area {
width: 180px;
height: 180px;
border-radius: 50%;
background: transparent;
display: flex;
justify-content: center;
align-items: center;
margin: 0 auto;

}

.animated-pulse {
 animation: pulse-scale 3s infinite ease-in-out;

}



.main-logo-image {
 width: 100%;
 height: 100%;
 object-fit: contain;
 border-radius: 50%;

}

/* ESTILOS DE LA CARA POSTERIOR (BACK) */

.nombre-titulo {
font-size: 2.2rem;
font-weight: 800;
margin-bottom: 0.3rem;
color: #14D1FF;
text-shadow: 0 0 8px #14D1FF, 0 0 20px rgba(20, 209, 255, 0.8);
transition: opacity 0.3s;
cursor: pointer;

}

.nombre-titulo.hover-active {
opacity: 0.5;

}



.subtitulo {
font-size: 1.2rem;
font-weight: 500;
color: #A855F7;
margin-bottom: 1rem;
animation: neon-pulse-subtitulo 2s infinite alternate;

}



.descripcion-parrafo {
font-size: 1rem;
line-height: 1.6;
color: var(--vt-c-white-soft); /* Usar la variable semántica */
max-width: 700px;
margin: 0 auto 1.2rem auto;

}



/* FOTO DE PERFIL */

.foto-perfil-ancla {
position: absolute;
top: 5px;
right: 20px;
z-index: 10;
opacity: 0;
transform: scale(0.5);
transition: opacity 0.4s ease-out, transform 0.4s ease-out;
pointer-events: none;

}



.foto-perfil-ancla.foto-visible {
opacity: 1;
transform: scale(0.8);
pointer-events: all;

}

.foto-perfil {
width: 100px;
height: 100px;
border-radius: 50%;
object-fit: cover;
border: 3px solid #FF1493;
box-shadow: 0 0 15px #FF1493;

}

/* SECCIÓN DE CONTACTO */

.contacto-wrapper {
display: flex;
flex-wrap: nowrap;
justify-content: center;
gap: 1rem;
padding: 1rem 0;
border-top: 1px dashed rgba(168, 85, 247, 0.2);
border-bottom: 1px dashed rgba(168, 85, 247, 0.2);
margin-bottom: 1.5rem;
overflow: hidden;

}

.contacto-item {
display: flex;
align-items: center;
text-decoration: none;
font-size: 0.95rem;
font-weight: 500;
transition: transform 0.2s, opacity 0.2s;
flex-grow: 0;
flex-shrink: 0;
white-space: nowrap;
overflow: hidden;
min-width: 0;
padding: 0 0.4rem;

}



.contacto-item:hover {
transform: translateY(-3px);
opacity: 0.9;
}



/* CSS para el ícono SVG local */

.contacto-icon {
width: 20px;
height: 20px;
margin-right: 0.5rem;
filter: drop-shadow(0 0 5px currentColor);
flex-shrink: 0;

}



.contacto-text {
display: inline-block;
}



/* SECCIÓN DE REDES SOCIALES */

.redes-sociales {
display: flex;
justify-content: center;
gap: 1rem;

}

.red-social-link {
display: inline-block;
transition: transform 0.3s;
text-decoration: none;

}

.red-social-link:hover {
transform: scale(1.05) translateY(-3px);

}



.social-circle {
width: 45px;
height: 45px;
border-radius: 50%;
display: flex;
justify-content: center;
align-items: center;
background-color: var(--vt-c-black-mute);
border: 2px dashed var(--shadow-color); /*Borde punteado de color */
box-shadow: 0 0 7px var(--shadow-color), inset 0 0 3px var(--shadow-color);
transition: box-shadow 0.3s, border-color 0.3s;

}



.social-circle:hover {
box-shadow: 0 0 12px var(--shadow-color), inset 0 0 5px var(--shadow-color);

}



.social-icon {
width: 25px;
height: 25px;
filter: none;

}

/* MEDIAS QUERY */

@media (max-width: 1080px) {

.flip-card-outer, .perfil-card {

max-width: 90%;

}

}

/* En CELULAR permitimo que salte a varias líneas para evitar desbordamiento */

@media (max-width: 768px) {

.contacto-wrapper {

flex-direction: column;

gap: 0.5rem;

flex-wrap: wrap;

}

.contacto-item {

justify-content: center;

}



.foto-perfil-ancla {

top: 15px;

right: 50%;

transform: translateX(50%) scale(0.45);

}

.foto-perfil-ancla.foto-visible {

transform: translateX(50%) scale(0.7);

}



.nombre-titulo {

font-size: 2rem;

}

.subtitulo {

font-size: 1.1rem;

}

.descripcion-parrafo {

font-size: 0.95rem;

}

}

</style>