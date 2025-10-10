<template>
  <section class="datos-personales-container">
    <div class="card-neon">
      <div class="header-content">
        <h1 @mouseover="showPhoto = true" @mouseleave="showPhoto = false">
          {{ datosPersonales.nombre }}
        </h1>

        <transition name="fade">
          <img
            v-if="showPhoto"
            :src="datosPersonales.fotoPerfil"
            alt="Foto de Perfil de Valeria Lardín"
            class="profile-photo"
          />
        </transition>
      </div>

      <h2>{{ datosPersonales.profesion }}</h2>

      <p>{{ datosPersonales.perfil }}</p>

      <div class="contacto-info-lineal">
        <a
          v-for="item in datosPersonales.contacto"
          :key="item.label"
          class="contact-item"
          :href="
            item.icon === 'Mail'
              ? 'mailto:' + item.value
              : item.icon === 'Phone'
              ? 'tel:' + item.value
              : null
          "
          :target="item.icon === 'Phone' || item.icon === 'Mail' ? '_blank' : '_self'"
          @click.prevent="item.icon === 'Mail' ? copyToClipboard(item.value) : null"
          :style="{ cursor: item.icon === 'Phone' || item.icon === 'Mail' ? 'pointer' : 'default' }"
        >
          <svg v-if="item.icon === 'Phone'" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="contact-icon">
            <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.63A2 2 0 0 1 3.08 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path>
          </svg>
          <svg v-else-if="item.icon === 'Mail'" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="contact-icon">
            <rect width="20" height="16" x="2" y="4" rx="2"></rect>
            <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"></path>
          </svg>
          <svg v-else-if="item.icon === 'MapPin'" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="contact-icon">
            <path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path>
            <circle cx="12" cy="10" r="3"></circle>
          </svg>

          <span class="label">{{ item.label }}:</span>
          <span class="value">{{ item.value }}</span>
        </a>
      </div>

      <ul class="redes-sociales">
        <li v-for="red in datosPersonales.redesSociales" :key="red.name">
          <a :href="red.url" target="_blank" :aria-label="red.name">
            <img
              :src="red.iconPath"
              :alt="'Icono de ' + red.name"
              class="icon-social"
              :onerror="'this.src=\'' + red.fallback + '\''"
            />
          </a>
        </li>
      </ul>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";

// === 1. IMPORTAR ASSETS (Archivos SVG e Imagen) ===
import linkedinIcon from "../assets/in_linked_linkedin_media_social_icon_124259.svg";
import githubIcon from "../assets/github_git_icon_145985.svg";
import whatsappIcon from "../assets/whatsapp.svg";
import instagramIcon from "../assets/instagram_108043.svg";
import cvIcon from "../assets/curriculum_vitae_paper_document_icon_219507.svg";
import profilePhoto from "../assets/valeria_profile.jpg"; 

// Estado reactivo para controlar la visibilidad de la foto al hacer hover
const showPhoto = ref(false);

// === 2. LÓGICA DE COPIADO AL PORTAPAPELES ===

const copyToClipboard = (text) => {
  // 1. Usa Clipboard API (moderno)
  if (navigator.clipboard) {
    navigator.clipboard
      .writeText(text)
      .then(() => {
        alert("¡Correo copiado al portapapeles!");
      })
      .catch((err) => {
        console.error("Error al copiar el texto con Clipboard API:", err);
        // Fallback si falla
        fallbackCopyTextToClipboard(text);
      });
  } else {
    // 2. Método de respaldo (antiguo)
    fallbackCopyTextToClipboard(text);
  }
};

const fallbackCopyTextToClipboard = (text) => {
  const textArea = document.createElement("textarea");
  textArea.value = text;
  
  // Ocultar y agregar al cuerpo
  textArea.style.position = "fixed";
  textArea.style.opacity = "0";
  document.body.appendChild(textArea);

  // Seleccionar y copiar
  textArea.focus();
  textArea.select();

  try {
    document.execCommand("copy"); // Método de copia de respaldo
    alert("¡Correo copiado al portapapeles!");
  } catch (err) {
    console.error("Fallback: Error al copiar el texto: ", err);
  }

  // Remover el textarea
  document.body.removeChild(textArea);
};

// === 3. ARREGLO DE DATOS PERSONALES UNIFICADO PARA v-for ===
const datosPersonales = {
  nombre: "Valeria Elena LARDÍN",
  profesion: "Técnico Universitario en programación-UTN",
  perfil:
    "Hola, Bienvenido a mi portafolio de Proyectos. Soy un desarrollador con experiencia en el desarrollo de aplicaciones web y backend.",
  fotoPerfil: profilePhoto,

  contacto: [
    { label: "Mi Cel", value: "+54 9 2622-641424", icon: "Phone" },
    { label: "Mi Mail", value: "valelar81@gmail.com", icon: "Mail" },
    { label: "Mi Dirección", value: "Tupungato, Mendoza, Argentina", icon: "MapPin" },
  ],

  redesSociales: [
    { name: "LinkedIn", url: "https://www.linkedin.com/in/valeria-lardin-833984370/", iconPath: linkedinIcon, fallback: "https://placehold.co/40x40/1d4ed8/ffffff?text=L" },
    { name: "Github", url: "https://github.com/Vale10Lar", iconPath: githubIcon, fallback: "https://placehold.co/40x40/000000/ffffff?text=G" },
    { name: "WhatsApp", url: "https://wa.me/5492622641424", iconPath: whatsappIcon, fallback: "https://placehold.co/40x40/25D366/ffffff?text=W" },
    { name: "Instagram", url: "https://www.instagram.com/vale_.lar/", iconPath: instagramIcon, fallback: "https://placehold.co/40x40/e1306c/ffffff?text=I" },
    { name: "Currículum Vitae", url: "https://docs.google.com/document/d/tu-cv-url", iconPath: cvIcon, fallback: "https://placehold.co/40x40/cccccc/000000?text=CV" },
  ],
};
</script>

<style scoped>
/* * 💡 SOLUCIÓN CONTRA ERROR 500: 
 * Este bloque es una definición simplificada de las variables principales. 
 * Asegura que el compilador de Vue/Vite encuentre las variables scoped 
 * antes de que se cargue el CSS global, evitando errores de referencia.
 */
:root {
  --vt-c-cyan: #00ffff;
  --vt-c-magenta: #ff00ff;
  --vt-c-indigo: #a855f7; 
  --vt-c-black-soft: #151b33;
  --vt-c-black-mute: #283049;
  --vt-c-black: #03091b; 
  --vt-c-white: #ebe0ff;
}

/* ============================================== */
/* =========== ESTILOS DEL COMPONENTE =========== */
/* ============================================== */

/* ESTILOS GENERALES DE SECCIÓN */
section {
  padding: 2rem;                       /* Relleno alrededor del contenido de la sección */
  border-bottom: 1px solid var(--vt-c-black-mute); /* Línea divisoria en la parte inferior */
  margin: 3rem 0rem 2rem 0rem;         /* Margen: Arriba(3) Der(0) Abajo(2) Izq(0) */
}

/* CONTENEDOR FLEXIBLE DE DATOS PERSONALES */
.datos-personales-container {
  display: flex;             /* Habilita Flexbox */
  justify-content: center;   /* Centra el contenido horizontalmente */
  align-items: flex-start;   /* Alinea elementos al inicio (arriba) */
  padding: 0 1rem;           /* Relleno lateral */
  width: 100%;               /* Ancho completo */
  margin-top: 1rem;          /* Margen superior */
  margin-bottom: 5rem;       /* Margen inferior */
}

/* ESTILOS DE LA TARJETA PRINCIPAL (CARD NEON) */
.card-neon {
  /* Degradado de fondo */
  background: linear-gradient(to bottom, var(--vt-c-indigo) 3%, var(--vt-c-black-soft) 92%);
  border-radius: 20px;       /* Bordes redondeados */
  padding: 2rem 4rem;        /* Relleno interior */
  width: 100%;               /* Ancho total dentro del contenedor */
  max-width: 100%;

  position: relative;        /* Establece el contexto para la foto de perfil absoluta */

  /* Sombra Neón base */
  box-shadow: 0 0 10px var(--vt-c-indigo), 0 0 20px rgba(0, 255, 255, 0.4);
  /* Borde con imagen (aunque no es visible, mantiene la propiedad) */
  border-image: linear-gradient(to right, var(--vt-c-indigo), var(--vt-c-cyan), var(--vt-c-indigo)) 1;
}

/* Efecto de brillo al pasar el ratón */
.card-neon:hover {
  box-shadow: 0 0 20px var(--vt-c-indigo, #3b82f6), 0 0 40px var(--vt-c-cyan, #06b6d4);
}

/* CONTENEDOR DEL NOMBRE Y LA FOTO */
.header-content {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: relative;
}

/* ESTILOS DE TIPOGRAFÍA: Nombre (h1) */
h1 {
  font-family: "Orbitron", sans-serif;
  color: var(--vt-c-cyan, #0b92aa);
  font-size: 2rem;
  margin-bottom: 0;
  text-shadow: 0 0 10px rgba(0, 255, 255, 0.7); /* Efecto neón en el texto */
  text-align: center;
  cursor: pointer;
}

/* FOTO DE PERFIL (Para Escritorio: Posición Absoluta) */
.profile-photo {
  position: absolute;        /* Posicionamiento absoluto para sacarla del flujo */
  right: 20px;               /* 20px del borde derecho */
  left: auto;                
  transform: none;           

  top: -10px;                /* Ligeramente fuera del borde superior */
  width: 100px;
  height: 100px;

  border-radius: 50%;        /* Forma circular */
  object-fit: cover;         /* Asegura que la imagen cubra el área */
  margin: 0;
  display: block;
  border: 3px solid var(--vt-c-cyan);
  box-shadow: 0 0 10px var(--vt-c-cyan), 0 0 20px rgba(0, 255, 255, 0.4);
  z-index: 10;               /* Asegura que esté por encima de otros elementos */
}

/* Transiciones para la foto (Entrada/Salida suave - Opacidad) */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* PROFESIÓN (h2) */
.datos-personales-container .card-neon h2 {
  color: var(--vt-c-black);
  font-size: 1.2rem;
  font-weight: 500;
  margin-top: 0.5rem;
  margin-bottom: 1rem;
  text-align: center;
  /* Efecto de sombra doble neón */
  text-shadow: 
    1px 1px 0px rgba(0, 0, 0, 0.1), 
    0 0 8px rgba(168, 85, 247, 0.9),
    0 0 15px rgba(168, 85, 247, 0.5);
}

/* Descripción (párrafo) */
p {
  color: #b2afaf;
  font-size: 1.1rem;
  line-height: 1.5;
  margin-bottom: 1.5rem;
  text-align: center;
}

/* CONTACTO LINEAL (contenedor) */
.contacto-info-lineal {
  display: flex;
  flex-wrap: wrap;           /* Permite que los ítems salten de línea en espacios reducidos */
  justify-content: center;   /* Centra los ítems */
  gap: 1.5rem;               /* Espacio entre ítems */
  padding: 0.5rem 0;
  margin-bottom: 1.5rem;
}

/* Ítem de contacto (botón/enlace) */
.contact-item {
  margin: 0;
  font-size: 0.9rem;
  font-weight: 500;
  display: flex;
  align-items: center;
  padding: 5px 10px;
  border-radius: 8px;
  background-color: rgba(0, 0, 0, 0.2);
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  color: inherit;
  text-decoration: none;
}

.contact-item:hover {
  background-color: rgba(0, 255, 255, 0.1);
  box-shadow: 0 0 5px rgba(0, 255, 255, 0.2);
}

/* Íconos de contacto (Teléfono, Mail, Mapa) */
.contact-icon {
  margin-right: 6px;
  height: 18px;
  width: 18px;
  color: var(--vt-c-magenta, #d946ef); /* Color púrpura/magenta */
  filter: drop-shadow(0 0 3px var(--vt-c-magenta, #d946ef)); /* Efecto neón en el ícono */
}

.contact-item .label {
  color: var(--vt-c-cyan, #06b6d4);
  font-weight: 700;
  margin-right: 0.5rem;
  text-shadow: 0 0 3px rgba(0, 255, 255, 0.3);
}

.contact-item .value {
  color: #fff;
  font-weight: 400;
}

/* REDES SOCIALES (Íconos) */
.redes-sociales {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 1rem 0 0;
  gap: 15px;
  list-style: none;
  margin: 0;
}

.redes-sociales li a {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  background-color: transparent;
  border: 1px dashed rgba(255, 255, 255, 0.3);
  box-shadow: 0 0 5px var(--vt-c-indigo);
  transition: all 0.3s ease-in-out;
}

/* Efecto hover en el ícono */
.redes-sociales li a:hover {
  transform: scale(1.1); /* Ligeramente más grande */
  box-shadow: 0 0 15px var(--vt-c-cyan); /* Más brillo */
  border: 1px dashed var(--vt-c-cyan);
}

.icon-social {
  height: 2.2rem;
  width: 2.2rem;
  filter: none;
  transition: none;
}

/*  RESPONSIVE DESIGN  */


/* Ajuste para Celus/Tablets (max-width: 768px) */
@media (max-width: 768px) {
  .datos-personales-container {
    padding: 0 0.5rem;
  }
  .card-neon {
    padding: 1.5rem 2rem;
  }

  /* Reorganiza el contenido para poner la foto encima del nombre */
  .header-content {
    flex-direction: column; /* Apila los elementos (foto y nombre) */
    align-items: center;    /* Centra la columna de elementos */
  }

  h1 {
    order: 2; /* Mueve el nombre debajo de la foto */
    font-size: 1.8rem;
    margin-top: 1.5rem; /* Espacio entre la foto y el nombre */
  }

  /* La foto ahora fluye con el contenido */
  .profile-photo {
    order: 1;             /* Mueve la foto arriba */
    position: relative;   /* Quita el posicionamiento absoluto para que fluya */
    top: auto;            /* Desactiva la elevación de escritorio */
    right: auto;
    left: auto;
    width: 100px;
    height: 100px;
    margin-bottom: 0.5rem;
  }

  .datos-personales-container .card-neon h2 {
    font-size: 1rem;
    margin-top: 0.3rem;
  }
  p {
    font-size: 1rem;
  }
  
  /* Apila la información de contacto */
  .contacto-info-lineal {
    flex-direction: column; 
    align-items: center;
    gap: 0.5rem;
  }
  .contact-item {
    font-size: 0.85rem;
    width: 100%;             /* Ocupa el ancho completo */
    justify-content: center;
  }
}

/* Ajuste para Teléfonos Pequeños (max-width: 600px) */
@media (max-width: 600px) {
  .card-neon {
    padding: 1rem;
  }
  h1 {
    font-size: 1.5rem;
    margin-top: 1rem;
  }
  .profile-photo {
    width: 80px;
    height: 80px;
    margin-bottom: 0.3rem;
  }

  .datos-personales-container .card-neon h2 {
    font-size: 0.85rem;
    margin-top: 0.2rem;
  }
  p {
    font-size: 0.9rem;
  }
  .icon-social {
    height: 2rem;
    width: 2rem;
  }
}
</style>