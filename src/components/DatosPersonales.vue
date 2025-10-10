<template>
  <section class="datos-personales-container">
    <div class="card-neon">
      <h1>{{ datosPersonales.nombre }}</h1>

      <h2>{{ datosPersonales.profesion }}</h2>

      <p>{{ datosPersonales.perfil }}</p>

      <div class="contacto-info-lineal">
        <div v-for="item in datosPersonales.contacto" :key="item.label" class="contact-item">
          <svg
            v-if="item.icon === 'Phone'"
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="contact-icon"
          >
            <path
              d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.63A2 2 0 0 1 3.08 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"
            ></path>
          </svg>
          <svg
            v-else-if="item.icon === 'Mail'"
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="contact-icon"
          >
            <rect width="20" height="16" x="2" y="4" rx="2"></rect>
            <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"></path>
          </svg>
          <svg
            v-else-if="item.icon === 'MapPin'"
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="contact-icon"
          >
            <path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path>
            <circle cx="12" cy="10" r="3"></circle>
          </svg>

          <span class="label">{{ item.label }}:</span>
          <span class="value">{{ item.value }}</span>
        </div>
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
// *** 1. IMPORTAR los archivos SVG desde src/assets ***
import linkedinIcon from "../assets/in_linked_linkedin_media_social_icon_124259.svg";
import githubIcon from "../assets/github_git_icon_145985.svg";
import whatsappIcon from "../assets/whatsapp.svg"; // Usa el nombre correcto del archivo
import instagramIcon from "../assets/instagram_108043.svg";
import cvIcon from "../assets/curriculum_vitae_paper_document_icon_219507.svg";

// *** ARREGLO DE DATOS PERSONALES UNIFICADO PARA v-for ***
const datosPersonales = {
  // Datos Estáticos (Se inyectan directamente con {{ }})
  nombre: "Valeria Elena LARDÍN",
  profesion: "Técnico Universitario en programación-UTN",
  perfil:
    "Hola, Bienvenido a mi portafolio de Proyectos. Soy un desarrollador con experiencia en el desarrollo de aplicaciones web y backend.",

  // Datos de Contacto (Array de objetos para v-for en la Sección 1)
  contacto: [
    {
      label: "Mi Cel",
      value: "+54 9 2622-641424",
      icon: "Phone", // Usado en v-if para seleccionar el SVG
    },
    {
      label: "Mi Mail",
      value: "valelar81@gmail.com",
      icon: "Mail",
    },
    {
      label: "Mi Dirección",
      value: "Tupungato, Mendoza, Argentina",
      icon: "MapPin",
    },
  ],

  // Enlaces de Redes Sociales
  redesSociales: [
    {
      name: "LinkedIn",
      url: "https://www.linkedin.com/in/valeria-lardin-833984370/",
      iconPath: linkedinIcon,
      fallback: "https://placehold.co/40x40/1d4ed8/ffffff?text=L",
    },
    {
      name: "Github",
      url: "https://github.com/Vale10Lar",
      // 2. USAR la variable importada en lugar de la ruta de cadena:
      iconPath: githubIcon,
      fallback: "https://placehold.co/40x40/000000/ffffff?text=G",
    },
    {
      name: "WhatsApp",
      url: "https://wa.me/5492622641424",
      // Asumo que el que dejaste en public es 'whatsapp_social_media_icon_127369.svg'
      // El de assets es 'whatsapp.svg' - REVISA ESTO EN TUS ARCHIVOS
      iconPath: whatsappIcon, // Usando el de assets/whatsapp.svg
      // Para el que dejaste en public, la ruta original debería funcionar si el nombre es correcto:
      // iconPath: '/whatsapp_social_media_icon_127369.svg',
      fallback: "https://placehold.co/40x40/25D366/ffffff?text=W",
    },
    {
      name: "Instagram",
      url: "https://www.instagram.com/vale_.lar/",
      iconPath: instagramIcon,
      fallback: "https://placehold.co/40x40/e1306c/ffffff?text=I",
    },
    {
      name: "Currículum Vitae",
      url: "https://docs.google.com/document/d/tu-cv-url",
      iconPath: cvIcon,
      fallback: "https://placehold.co/40x40/cccccc/000000?text=CV",
    },
  ],

  // La sección 'habilidades' se ha eliminado.
};
</script>

<style scoped>
/* =======================================
     ESTILOS GENERALES DE SECCIÓN (Movidos de base.css)
     ======================================= */
/* =======================================
ESTILOS GENERALES DE SECCIÓN (Movidos de base.css)
======================================= */
section {
 padding: 2rem;
border-bottom: 1px solid var(--vt-c-black-mute);
 margin: 3rem 0rem 2rem 0rem;
}
/* ELIMINAR AQUI EL BLOQUE section h2 { ... } */

section h3 {
 color: var(--vt-c-white);
 font-family: "Orbitron", sans-serif;
}

/* =======================================
     ESTILOS ESPECÍFICOS DE DATOS PERSONALES
     ======================================= */

.datos-personales-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 0 1rem;
  width: 100%;

  /* CRÍTICO: Reducimos el margen superior para acercar la tarjeta a la barra */
  margin-top: 1rem; /* Ajuste muy preciso y pequeño */
  margin-bottom: 5rem;
}
/* --- ESTILOS DE TARJETAS/CONTENEDORES PRINCIPALES (Aplicado a .card-neon) --- */
.card-neon {
  /* CRÍTICO: DEGRADADO DE ARRIBA A ABAJO (Morado a Oscuro) */
  background: linear-gradient(
    to bottom,
    /* Dirección de Arriba hacia Abajo */ var(--vt-c-indigo) 3%,
    /* Morado neón arriba */ var(--vt-c-black-soft) 92% /* Azul Oscuro abajo */
  );

  border-radius: 20px;
  /* CRÍTICO: Ajustamos el padding vertical (2rem) para proporcionalidad, 
     horizontal (4rem) se mantiene para el ancho. */
  padding: 2rem 4rem; /* 2rem arriba y 2rem abajo */

  width: 100%;
  max-width: 100%;

  /* Mantenemos el resplandor neón exterior de la tarjeta */
  box-shadow: 0 0 10px var(--vt-c-indigo), 0 0 20px rgba(0, 255, 255, 0.4);

  /* Mantenemos el borde para que se vea neón */
  border-image: linear-gradient(to right, var(--vt-c-indigo), var(--vt-c-cyan), var(--vt-c-indigo))
    1;
}

/* Efecto al pasar el mouse (hover): Aumenta el brillo del borde */
.card-neon:hover {
  box-shadow: 0 0 20px var(--vt-c-indigo, #3b82f6), 0 0 40px var(--vt-c-cyan, #06b6d4);
}

/* ESTILOS DE TIPOGRAFÍA */
/* Mi nombre (h1) - Opcional, si lo ves muy grande */
h1 {
  font-family: "Orbitron", sans-serif;
  color: var(--vt-c-cyan, #0b92aa);

  /* Opcional: Reducir de 2.2rem/2.5rem */
  font-size: 2rem;

  margin-bottom: 0.3rem;
  text-shadow: 0 0 10px rgba(0, 255, 255, 0.7);
  text-align: center;
}
/* Mi Profesión (h2) - SOLUCIÓN LIMPIA Y FINAL */
/* Usamos el contenedor principal de la tarjeta para la máxima especificidad */
/* Mi Profesión (h2) - ESTILO FINAL Y DEFINITIVO (Escritorio) */
/* Mi Profesión (h2) - SOLUCIÓN DE COLOR FINAL (Morado Intenso) */
/* Selector de máxima especificidad para forzar el color morado */
.datos-personales-container .card-neon h2 {
  
 /* CRÍTICO: CAMBIAMOS EL COLOR DEL TEXTO A UN AZUL MUY OSCURO (casi negro) */
  /* Esto da el contraste necesario para que el brillo NEÓN DE ABAJO resalte */
  color: var(--vt-c-black-soft); /* Azul medianoche/casi negro */
  
  /* Mantenemos el tamaño y el espaciado correcto */
  font-size: 1.2rem; 
  font-weight: 500;
  margin-top: 0.5rem; 
  margin-bottom: 1rem; 
  text-align: center; 
  
  /* Mantenemos el brillo MORADO NEÓN que sale por debajo del texto oscuro */
  text-shadow: 
    0 0 10px rgba(168, 85, 247, 0.8), /* Brillo fuerte morado neón */
    0 0 20px rgba(168, 85, 247, 0.3); /* Resplandor difuso */
}
/* Breve descripcion de lo que realizo */
p {
  color: #b2afaf;
  font-size: 1.1rem;
  line-height: 1.5;
  margin-bottom: 1.5rem;
  text-align: center;
}

/* ESTILOS DE LA SECCIÓN DE CONTACTO TEXTUAL */
.contacto-info-lineal {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1.5rem;
  padding: 0.5rem 0;
  margin-bottom: 1.5rem;
}

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
  cursor: default;
}

.contact-item:hover {
  background-color: rgba(0, 255, 255, 0.1);
  box-shadow: 0 0 5px rgba(0, 255, 255, 0.2);
}

.contact-icon {
  margin-right: 6px;
  height: 18px;
  width: 18px;
  color: var(--vt-c-magenta, #d946ef);
  filter: drop-shadow(0 0 3px var(--vt-c-magenta, #d946ef));
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

/* ESTILOS DE LA SECCIÓN DE REDES SOCIALES (Íconos) */

.redes-sociales {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 1rem 0 0;
  gap: 15px; /* Espaciado entre los círculos */
  list-style: none;
  margin: 0;
}
/* APLICAMOS EL ESTILO CIRCULAR Y EL BORDE ESPECIAL AL ENLACE (li a) */
.redes-sociales li a {
  /* Estilo de contenedor para hacer el círculo */
  display: flex;
  justify-content: center;
  align-items: center;

  /* Dimensiones del círculo (Ajustado para que se vea como en la imagen) */
  width: 3rem;
  height: 3rem;

  border-radius: 50%; /* Esto crea el círculo */

  background-color: transparent; /* Elimino el background-color, pues los SVGs ya lo traen */

  /* Aplic el borde punteado neón para el efecto 'Instagram' */
  border: 1px dashed rgba(255, 255, 255, 0.3); /* Borde punteado muy suave */

  /* Sombra que simula el resplandor del borde */
  box-shadow: 0 0 5px var(--vt-c-indigo);

  /* Efecto de transición suave */
  transition: all 0.3s ease-in-out;
}

.redes-sociales li a:hover {
  /* Efecto Hover: Hace que el círculo brille y escale */
  transform: scale(1.1);

  /* Aumenta el resplandor y el borde */
  box-shadow: 0 0 15px var(--vt-c-cyan);
  border: 1px dashed var(--vt-c-cyan); /* El borde cambia a cian al hacer hover */
}

/* ESTILO DEL ÍCONO (IMAGEN) DENTRO DEL CÍRCULO */
.icon-social {
  /* Tamaño para el SVG/PNG para que quepa dentro del círculo */
  height: 2.2rem;
  width: 2.2rem;

  /* Aseguramos que no tenga filtros que lo distorsionen */
  filter: none;
  transition: none;
}
/* RESPONSIVE DESIGN */
@media (max-width: 768px) {
  .datos-personales-container {
    padding: 0 0.5rem;
  }
  .card-neon {
    padding: 1.5rem 2rem;
  }
  h1 {
    font-size: 2rem;
  }
  h2 {
    font-size: 1.1rem; /* Mantiene el tamaño pequeño en tabletas */
  }
  .contacto-info-lineal {
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
  }
  .contact-item {
    font-size: 0.85rem;
    width: 100%;
    justify-content: center;
  }
}

@media (max-width: 600px) {
  .card-neon {
    padding: 1rem;
  }
  h1 {
    font-size: 1.6rem;
  }
  h2 {
    font-size: 0.9rem; /* Mantiene el tamaño muy pequeño en móviles */
  }
  .icon-social {
    height: 2.2rem;
    width: 2.2rem;
  }
}
</style>
