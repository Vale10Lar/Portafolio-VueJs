<script setup>
import { computed, ref, onMounted, onUnmounted } from "vue";

// =========================================================
// IMPORTACIONES DE ASSETS (Imágenes y SVGs)
// =========================================================
// Importación de la foto de perfil y el logo (rutas relativas a la carpeta assets)
import fotoPerfil from "../assets/valeria_profile.jpg";
import valeriaLogo from "../assets/valeria_logo_neon.png";

// Importaciones de íconos de Redes Sociales (SVGs)
import iconLinkedIn from "../assets/in_linked_linkedin_media_social_icon_124259.svg";
import iconGitHub from "../assets/github_git_icon_145985.svg";
import iconInstagram from "../assets/instagram_108043.svg";
import iconCV from "../assets/curriculum_vitae_paper_document_icon_219507.svg";
import iconWhatsAppSocial from "../assets/whatsapp.svg";

// Importaciones de ÍCONOS DE CONTACTO (SVGs)
import iconPhone from "../assets/phone_78665.svg";
import iconEmail from "../assets/email_icon_133230.svg";
import iconLocation from "../assets/location_icon_159350.svg";

// =========================================================
// ESTADO Y DATOS REACTIVOS
// =========================================================

// Datos estáticos del perfil
const nombreCompleto = "Valeria Elena LARDÍN";
const titulo = "Técnico Universitario en Programación-UTN";
const descripcion =
  "Hola, Bienvenido a mi portafolio. Soy un desarrollador con experiencia en desarrollo de aplicaciones web y backend";

// Estado para la animación de la tarjeta giratoria (controlada por el usuario y NavBar)
const isFlipped = ref(false);

// Estado para la foto (controla la visibilidad al hacer hover sobre el nombre en la cara posterior)
const isHovered = ref(false);

// Clase calculada para el nombre, usada para aplicar estilos condicionales durante el hover
const nombreDisplayClass = computed(() => ({
  "nombre-titulo": true,
  "hover-active": isHovered.value,
}));

// Datos de Contacto para la cara posterior (incluye ícono, valor, y clase de color neón para el texto)
const contacto = [
  // La clase 'text-blue-neon' está definida en <style scoped> y usa: --color-neon-cyan
  {
    type: "Cel",
    value: "+54 9 2622-641424",
    href: "tel:+5492622641424",
    src: iconPhone,
    color: "text-blue-neon",
  },
  // La clase 'text-pink-neon' está definida en <style scoped> y usa: --vt-c-magenta-pure
  {
    type: "Correo",
    value: "valelar81@gmail.com",
    href: "mailto:valelar81@gmail.com",
    src: iconEmail,
    color: "text-pink-neon",
  },
  // Se introduce una clase 'text-purple-neon' para el color Índigo
  {
    type: "Dirección",
    value: "Tupungato, Mendoza, Argentina",
    href: "#",
    src: iconLocation,
    color: "text-purple-neon",
  },
];

// Redes Sociales (incluye URL, ícono y el color NEÓN dinámico para la sombra/borde)
const redesSociales = [
  // La propiedad 'shadow' pasa directamente la VARIABLE CSS GLOBAL (de base.css) al estilo inline.
  {
    name: "LinkedIn",
    url: "https://www.linkedin.com/in/valeria-lardin-833984370/",
    src: iconLinkedIn,
    shadow: "var(--vt-c-indigo)",
  },
  {
    name: "WhatsApp",
    url: "https://wa.me/5492622641424",
    src: iconWhatsAppSocial,
    shadow: "var(--color-neon-cyan)",
  },
  {
    name: "GitHub",
    url: "https://github.com/Vale10Lar/proyectos",
    src: iconGitHub,
    shadow: "var(--vt-c-white-soft)",
  },
  {
    name: "Instagram",
    url: "https://www.instagram.com/vale_.lar/",
    src: iconInstagram,
    shadow: "var(--vt-c-magenta-base)",
  },
  { name: "CV", url: "#", src: iconCV, shadow: "var(--vt-c-purple-electric)" },
];

// =========================================================
// MÉTODOS Y EXPOSICIÓN
// =========================================================

// Función principal para girar la tarjeta (toggle)
function flipCard() {
  isFlipped.value = !isFlipped.value;
}

// Hace la función flipCard() accesible para el componente padre (App.vue)
defineExpose({
  flipCard,
});

// =========================================================
// LÓGICA DEL RASTRO DE PARTÍCULAS NEÓN (GLOBAL)
// =========================================================

// Crea y anima una partícula neón en las coordenadas (x, y)
const spawnParticle = (x, y) => {
  const particle = document.createElement("div");
  particle.className = "cursor-particle";

  // Alterna aleatoriamente entre los dos colores principales neón
  const isPink = Math.random() > 0.5;
  // Nombres de variables globales corregidos
  const magentaColor = "var(--vt-c-magenta-pure)";
  const cyanColor = "var(--color-neon-cyan)";

  particle.style.backgroundColor = isPink ? magentaColor : cyanColor;
  particle.style.boxShadow = isPink
    ? `0 0 5px ${magentaColor}, 0 0 10px ${magentaColor}` // Sombra magenta
    : `0 0 5px ${cyanColor}, 0 0 10px ${cyanColor}`; // Sombra cian

  particle.style.left = `${x}px`;
  particle.style.top = `${y}px`;

  // Tamaño aleatorio para variar la apariencia
  const size = Math.random() * 5 + 2;
  particle.style.width = `${size}px`;
  particle.style.height = `${size}px`;

  // Calcula el destino aleatorio de la partícula (movimiento en un ángulo y distancia)
  const angle = Math.random() * 2 * Math.PI;
  const distance = Math.random() * 40 + 20;

  const finalX = x + Math.cos(angle) * distance;
  const finalY = y + Math.sin(angle) * distance;
  const duration = Math.random() * 1.5 + 0.5;

  document.body.appendChild(particle);

  // Aplica la transición y el movimiento después de un pequeño retraso
  setTimeout(() => {
    particle.style.transition = `all ${duration}s ease-out`;
    particle.style.transform = `translate(${finalX - x}px, ${finalY - y}px) scale(0)`;
    particle.style.opacity = "0";
  }, 10);

  // Elimina la partícula del DOM después de que termine la animación
  setTimeout(() => {
    particle.remove();
  }, duration * 1000 + 100);
};

let lastSpawn = 0;
const spawnInterval = 50; // Limita la aparición de partículas (cada 50ms)

const handleMouseMove = (event) => {
  const now = Date.now();
  // Limita la velocidad de aparición de partículas para optimizar el rendimiento
  if (now - lastSpawn > spawnInterval) {
    spawnParticle(event.clientX, event.clientY);
    lastSpawn = now;
  }
};

// Se ejecuta cuando el componente está montado
onMounted(() => {
  // Añade el detector de movimiento del ratón a la ventana (efecto global)
  window.addEventListener("mousemove", handleMouseMove);
});

// Se ejecuta cuando el componente se desmonta (limpieza para evitar fugas de memoria)
onUnmounted(() => {
  window.removeEventListener("mousemove", handleMouseMove);
});
// FIN DE LA LÓGICA DEL RASTRO DE PARTÍCULAS
</script>

<template>
  <div id="perfil-card-target" class="datos-personales-container">
    <div class="flip-card-outer" :class="{ flipped: isFlipped }" @click="flipCard">
      <div class="perfil-card flip-card-inner">
        <div class="flip-card-front">
          <div class="logo-area animated-pulse">
            <img :src="valeriaLogo" alt="Logo Valeria Lardín" class="main-logo-image" />
          </div>
        </div>

        <div class="flip-card-back">
          <div :class="['foto-perfil-ancla', { 'foto-visible': isHovered }]">
            <img :src="fotoPerfil" alt="Foto de Perfil de Valeria Lardín" class="foto-perfil" />
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
              v-for="(red, index) in redesSociales"
              :key="red.name"
              :href="red.url"
              target="_blank"
              :aria-label="red.name"
              class="red-social-link"
            >
              <div
                class="social-circle"
                :style="{
                  // Pasa el color neón como variable CSS para la sombra/borde dinámico
                  '--shadow-color': red.shadow,
                  // Aplica un retraso escalonado a la animación de flotación
                  'animation-delay': `${index * 0.1}s`,
                }"
              >
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
/* ========================================================= */
/* 1. DEFINICIÓN DE VARIABLES LOCALES Y UTILIDADES DE COLOR */
/* (Uso las variables globales de 'base.css' para la consistencia) 
/* ========================================================= */

.text-pink-neon {
  /* Utiliza la variable Magenta Pura para un tono fuerte y único */
  color: var(--vt-c-magenta-pure);
  text-shadow: 0 0 5px var(--vt-c-magenta-pure), 0 0 10px rgba(255, 0, 255, 0.5);
}

.text-blue-neon {
  /* Utiliza el alias Semántico Cian Neón */
  color: var(--color-neon-cyan);
  text-shadow: 0 0 5px var(--color-neon-cyan), 0 0 10px rgba(0, 255, 255, 0.5);
}

.text-purple-neon {
  /* Utiliza la variable Índigo/Púrpura, color primario de la tarjeta */
  color: var(--vt-c-indigo);
  text-shadow: 0 0 5px var(--vt-c-indigo), 0 0 10px rgba(168, 85, 247, 0.5);
}

/* ========================================================= */
/* 2. DEFINICIONES DE ANIMACIÓN KEYFRAMES */
/* ========================================================= */

/* Animación de Flotación (para iconos sociales) */
@keyframes float-neon {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px);
  }
  100% {
    transform: translateY(0);
  }
}

/* Animación de Parpadeo/Flicker (para iconos sociales) */
@keyframes flicker-neon {
  0%,
  19%,
  21%,
  23%,
  25%,
  54%,
  56%,
  100% {
    /* Sombra base suave, usa la variable dinámica --shadow-color */
    box-shadow: 0 0 5px var(--shadow-color), inset 0 0 2px var(--shadow-color);
    opacity: 1;
  }
  20%,
  24%,
  55% {
    /* Pico de brillo (parpadeo) */
    box-shadow: 0 0 15px var(--shadow-color), inset 0 0 7px var(--shadow-color);
    opacity: 0.95;
    filter: brightness(1.2);
  }
}

/* Animación de Pulso de Borde para la Tarjeta Principal (Índigo) */
@keyframes pulse-border {
  0% {
    /* Uso de la variable Índigo principal y su RGBA */
    box-shadow: 0 0 10px var(--vt-c-indigo), inset 0 0 5px var(--vt-c-indigo-40);
  }
  100% {
    box-shadow: 0 0 25px var(--vt-c-indigo), inset 0 0 10px var(--vt-c-indigo-40);
  }
}

/* Animación de Pulso de Texto para el Subtítulo (Índigo) */
@keyframes neon-pulse-subtitulo {
  0%,
  100% {
    text-shadow: 0 0 3px var(--vt-c-indigo), 0 0 6px var(--vt-c-indigo-40);
  }
  50% {
    text-shadow: 0 0 5px var(--vt-c-indigo), 0 0 10px var(--vt-c-indigo); /* Brillo máximo */
  }
}

/* Animación para el Logo en el Frente (Escala y Sombra Combinada: Magenta y Índigo) */
@keyframes pulse-scale {
  0% {
    transform: scale(1);
    box-shadow: 0 0 10px var(--vt-c-magenta-pure), 0 0 30px var(--vt-c-indigo);
  }
  50% {
    transform: scale(1.05);
    box-shadow: 0 0 20px var(--vt-c-magenta-pure), 0 0 50px var(--vt-c-indigo);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 10px var(--vt-c-magenta-pure), 0 0 30px var(--vt-c-indigo);
  }
}

/* Animación de Borde Eléctrico (Transición de colores neón: Cian y Magenta) */
@keyframes electric-border {
  0% {
    border: 3px solid transparent;
    box-shadow: 0 0 5px var(--vt-c-magenta-pure), inset 0 0 5px var(--color-neon-cyan);
  }
  50% {
    border-color: var(--color-neon-cyan);
    box-shadow: 0 0 15px var(--color-neon-cyan), inset 0 0 10px var(--vt-c-magenta-pure);
  }
  100% {
    border: 3px solid transparent;
    box-shadow: 0 0 5px var(--vt-c-magenta-pure), inset 0 0 5px var(--color-neon-cyan);
  }
}

/* ========================================================= */
/* 3. ESTILOS DE LA TARJETA (Flip Card)LAYOUT GRAL. */
/* ========================================================= */

.datos-personales-container {
  width: 100%;
  max-width: 650px;
  perspective: 1000px;
  box-sizing: border-box;
  min-height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  overflow: hidden;
}

/* Contenedor externo que maneja el giro */
.flip-card-outer {
  width: 100%; /* Ocupa todo el ancho del contenedor padre */
  max-height: 95vh; /* Limita la altura máxima para evitar overflow */
  overflow: auto; /* Habilita el scroll si el contenido excede la altura */
}

/* Contenedor interno que gira (tanto el frente como el reverso) */
.perfil-card {
  width: 100%; /* Ocupa todo el ancho del contenedor padre */
  position: relative;
  transform-style: preserve-3d; /* Habilita el efecto 3D */
  transition: transform 0.8s;

  /* Fondo con degradado oscuro y patrón sutil de circuito neón (Uso de variables) */
  background: linear-gradient(135deg, var(--vt-c-black-soft) 0%, var(--vt-c-black-mute) 100%),
    repeating-radial-gradient(
      circle at center,
      var(--vt-c-magenta-pure-70) 0%,
      var(--vt-c-neon-blue-15) 1px,
      /* Magenta neón sutil */ transparent 50px,
      /* Cian neón sutil */ transparent 70px
    ),
    transparent;

  border: 2px solid var(--vt-c-indigo);
  border-radius: 16px;
  padding: 1rem 2.5rem; /* Espaciado interno */
  text-align: center;
  box-shadow: 0 0 20px var(--vt-c-indigo-40); /* Sombra inicial con RGBA de Índigo */
  animation: pulse-border 5s infinite alternate; /* Animación de pulso de borde */
  min-height: 350px; /* Altura mínima para la tarjeta */
}

/* Aplica la rotación al contenedor interno cuando se activa 'flipped' */
.flip-card-outer.flipped .perfil-card {
  transform: rotateY(180deg); /* Gira 180 grados en el eje Y */
}

/* Estilos comunes para ambas caras (Front y Back) */
.flip-card-front,
.flip-card-back {
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
  padding: 0.5rem 2rem 1rem 2rem; /* Espaciado interno */
  border-radius: 16px;
}

/* Posiciona y asegura el fondo de la cara posterior */
.flip-card-back {
  transform: rotateY(180deg); /* Gira 180 grados para el reverso */
  background: inherit; /* Hereda el fondo del contenedor padre */
}

/* ESTILOS CARA FRONTAL */
/* Área del logo principal */
.logo-area {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  border: 3px solid transparent;
}

/* Clase que combina el borde eléctrico y el pulso de escala */
.animated-pulse {
  animation: electric-border 4s infinite alternate, pulse-scale 3s infinite ease-in-out;
}
/* Imagen del logo principal */
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
  color: var(--color-neon-cyan); /* Usando el alias global */
  /* Sombra de texto Cian neón: Utilizando variable RGBA del Cian */
  text-shadow: 0 0 8px var(--color-neon-cyan), 0 0 20px var(--vt-c-neon-blue-50);
  transition: opacity 0.3s;
  cursor: pointer;
}

/* Efecto de atenuación al hacer hover para revelar la foto */
.nombre-titulo.hover-active {
  opacity: 0.5;
}
/* Subtítulo con animación de pulso neón */
.subtitulo {
  font-size: 1.2rem;
  font-weight: 500;
  color: var(--vt-c-indigo);
  margin-bottom: 0.8rem; /* Espaciado inferior */
  animation: neon-pulse-subtitulo 2s infinite alternate; /* Animación de pulso neón */
}

/* Párrafo descriptivo */
.descripcion-parrafo {
  font-size: 1rem;
  line-height: 1.6;
  color: var(--color-text-light); /* Alias semántico del texto light */
  max-width: 700px; /* Ancho máximo para mejor legibilidad */
  margin: 0 auto 1rem auto; /* Centrado y espaciado inferior */
}

/* FOTO DE PERFIL (Hover-to-Reveal) */
/* Contenedor de la foto de perfil */
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
/* Clase activa para mostrar la foto */
.foto-perfil-ancla.foto-visible {
  opacity: 1;
  transform: scale(0.8);
  pointer-events: all;
}
/* Imagen de la foto de perfil */
.foto-perfil {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  /* Borde y sombra usan la variable de Magenta Pura */
  border: 3px solid var(--vt-c-magenta-pure);
  box-shadow: 0 0 15px var(--vt-c-magenta-pure);
}

/* SECCIÓN DE CONTACTO */
/* Contenedor de los enlaces de contacto */
.contacto-wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem;
  padding: 0.8rem 0;

  /* Bordes punteados con efecto neón (Púrpura/Índigo) */
  border-top: 1px dashed var(--vt-c-indigo-40);
  border-bottom: 1px dashed var(--vt-c-indigo-40);
  /* Sombra sutil para simular el brillo del borde */
  box-shadow: 0 -2px 5px var(--vt-c-indigo-20), 0 2px 5px var(--vt-c-indigo-20);

  margin-bottom: 1rem;
  overflow: hidden;
}

.contacto-item {
  /* Enlaces de contacto*/
  display: flex;
  align-items: center;
  text-decoration: none;
  font-size: 0.95rem;
  font-weight: 500;
  transition: transform 0.2s, opacity 0.2s;
  white-space: nowrap;
  min-width: 0;
  padding: 0 0.4rem;
}
/* Efecto hover para levantar ligeramente el ítem */
.contacto-item:hover {
  transform: translateY(-3px); /* Levanta el ítem */
  opacity: 0.9;
}

/* Estilo base para el ícono SVG local */
/* La clase 'svg-icon-fix' asegura que los SVGs hereden el color del texto */
.contacto-icon {
  width: 20px;
  height: 20px;
  margin-right: 0.5rem;
  /* 'currentColor' hace que el SVG herede el color del texto (ej. .text-pink-neon) */
  filter: drop-shadow(0 0 5px currentColor);
  flex-shrink: 0;
}

/* SECCIÓN DE REDES SOCIALES */
.redes-sociales {
  display: flex;
  justify-content: center;
  gap: 1rem;
}
/* Enlace individual de red social */
.red-social-link {
  display: inline-block;
  transition: none;
}

/* Círculo que contiene el ícono social */
.social-circle {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--vt-c-card-dark); /* Fondo oscuro de la tarjeta */
  border: 2px dashed var(--shadow-color); /* Color dinámico (de la prop 'shadow') */

  /* Animaciones combinadas */
  animation: float-neon 3s ease-in-out infinite alternate, flicker-neon 5s linear infinite;
  /* Sombra inicial usando la variable dinámica --shadow-color */
  box-shadow: 0 0 7px var(--shadow-color), inset 0 0 3px var(--shadow-color);
  transition: none;
}

/* Efecto hover sobre el círculo: aumenta escala y brillo */
.red-social-link:hover .social-circle {
  transform: scale(1.1);
  box-shadow: 0 0 15px var(--shadow-color), inset 0 0 8px var(--shadow-color);
  filter: brightness(1.5);
}
/* Ícono social dentro del círculo */
.social-icon {
  width: 25px;
  height: 25px;
}

/* ========================================================= */
/* DISEÑO RESPONSIVO */
/* ========================================================= */

/* Tablets (≤1024px) */
@media (max-width: 1024px) {
  .perfil-card {
    padding: 1.5rem;
    min-height: 400px;
  }

  .logo-area {
    width: 180px;
    height: 180px;
  }

  .nombre-titulo {
    font-size: 2rem;
  }

  .descripcion-parrafo {
    max-width: 90%;
  }
}

/* Móviles grandes (≤768px) */
@media (max-width: 768px) {
  .flip-card-outer {
    max-height: unset;
  }

  .perfil-card {
    padding: 1.2rem;
    min-height: 420px;
  }

  .nombre-titulo {
    font-size: 1.8rem;
  }

  .subtitulo {
    font-size: 1.1rem;
  }

  .descripcion-parrafo {
    font-size: 0.95rem;
  }

  .foto-perfil-ancla {
    top: 15px;
    right: 50%;
    transform: translateX(50%) scale(0.6);
  }
  .foto-perfil-ancla.foto-visible {
    transform: translateX(50%) scale(0.8);
  }

  .contacto-wrapper {
    flex-direction: column;
    align-items: center;
  }

  .social-circle {
    width: 40px;
    height: 40px;
  }
}

/* Móviles pequeños (≤480px) */
@media (max-width: 480px) {
  .perfil-card {
    padding: 1rem;
    min-height: 380px;
  }

  .nombre-titulo {
    font-size: 1.5rem;
  }

  .subtitulo {
    font-size: 1rem;
  }

  .descripcion-parrafo {
    font-size: 0.9rem;
    line-height: 1.4;
  }

  .social-circle {
    width: 35px;
    height: 35px;
  }

  .contacto-icon {
    width: 18px;
    height: 18px;
  }
}

/*  ESTILO GLOBAL PARA EL RASTRO DE PARTÍCULAS- */

.cursor-particle {
  position: fixed;
  border-radius: 50%;
  opacity: 1;
  pointer-events: none;
  z-index: 9999;
}
</style>
