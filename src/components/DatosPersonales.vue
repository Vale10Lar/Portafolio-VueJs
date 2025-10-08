<template>
    <!-- Contenedor principal para centrar la tarjeta y manejar el espaciado vertical -->
    <section class="datos-personales-container">
      <!-- Tarjeta principal con el estilo de Neón -->
      <div class="card-neon">
        <!-- Título Principal (Nombre) con estilo NEON cian -->
        <h1>Valeria Elena LARDIN</h1>
        
        <!-- Subtítulo (Profesión) en blanco con brillo rosa -->
        <h2>Técnico Universitario en programación-UTN</h2>
        
        <!-- Párrafo de Perfil -->
        <p>Hola, Bienvenido a mi portafolio de Proyectos. Soy un desarrollador con experiencia en el desarrollo de aplicaciones web y backend.</p>
  

      <!-- SECCIÓN 1: CONTACTO DIRECTO (on iconos UX) -->

      <div class="contacto-info-lineal">
        
        <!-- CELULAR -->
        <div class="contact-item">
          <!-- Ícono de Teléfono -->
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="contact-icon"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.63A2 2 0 0 1 3.08 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
          <span class="label">Mi Cel:</span> 
          <span class="value">+54 9 2622-641424</span>
        </div>

        <!-- EMAIL -->
        <div class="contact-item">
          <!-- Ícono de Correo (Mail) -->
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="contact-icon"><rect width="20" height="16" x="2" y="4" rx="2"></rect><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"></path></svg>
          <span class="label">Mi Mail:</span> 
          <span class="value">valelar81@gmail.com</span>
        </div>

        <!-- datos de direccion con icono ux -->
        <div class="contact-item">
          <!-- Ícono de Mapa (Location Pin) -->
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="contact-icon"><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path><circle cx="12" cy="10" r="3"></circle></svg>
          <span class="label">Mi Dirección:</span> 
          <span class="value">Tupungato, Mendoza, Argentina</span>
        </div>
      </div>

        
        <!-- SECCIÓN 2: REDES SOCIALES Y ENLACES (SOLO ÍCONOS) -->
        
        <ul class="redes-sociales">
          
          <!-- ÍTEM 1: Github -->
          <li>
            <a href="https://github.com/Vale10Lar" target="_blank" aria-label="Github">
              <img src="/github_git_icon_145985.svg" alt="Icono de Github" class="icon-social" onerror="this.src='https://placehold.co/40x40/000000/ffffff?text=G'" />
            </a>
          </li>
  
          <!-- item 2: WHATSAPP (Red Social / Contacto instantáneo) -->
          <li>
            <a href="https://wa.me/5492622641424" target="_blank" aria-label="WhatsApp">
              <img 
                src="/whatsapp_social_media_icon_127369.svg" 
                alt="Icono de WhatsApp" 
                class="icon-social" 
                onerror="this.src='https://placehold.co/40x40/25D366/ffffff?text=W'" 
              />
            </a>
          </li>
  
          <!-- ÍTEM 3: Instagram -->
          <li>
            <a href="https://www.instagram.com/vale_.lar/" target="_blank" aria-label="Instagram">
              <img src="/instagram_108043.svg" alt="Icono de Instagram" class="icon-social" onerror="this.src='https://placehold.co/40x40/e1306c/ffffff?text=I'" />
            </a>
          </li>
          
          <!-- ÍTEM 4: LinkedIn -->
          <li>
            <a href="https://www.linkedin.com/in/valeria-lardin-833984370/" target="_blank" aria-label="LinkedIn">
              <img src="/in_linked_linkedin_media_social_icon_124259.svg" alt="Icono de LinkedIn" class="icon-social" onerror="this.src='https://placehold.co/40x40/1d4ed8/ffffff?text=L'" />
            </a>
          </li>
          
          <!-- ÍTEM 5: Currículum Vitae (Enlace importante) -->
          <li>
            <a href="https://docs.google.com/document/d/tu-cv-url" target="_blank" aria-label="Currículum Vitae">
              <img src="/curriculum_vitae_paper_document_icon_219507.svg" alt="Icono de Currículum Vitae" class="icon-social" onerror="this.src='https://placehold.co/40x40/cccccc/000000?text=CV'" />
            </a>
          </li>
        </ul>
        
      </div>
    </section>
  </template>
  
  <script setup>
  // No se requiere lógica de JavaScript en este componente estático.
  </script>
  
  <!-- Stilos-->
  <style scoped>
  /* Las variables c--vt-c-cyan estan deginidas globalmente en el proyecto */
  /* Contenedor que envuelve la tarjeta */
  .datos-personales-container {
    display: flex;
    justify-content: center;
    padding:0 1rem; /*ensancha horizontalmente la tarjeta*/
    width: 100%;
    margin-bottom: 5rem;  /* Aumento del margen inferior para logrr aque el borde de abajo se vea siempre */
  }
   
  /* Estilos de la tarjeta principal (Ancho, Fondo y Sombra Neón) */
  .card-neon {
    background: linear-gradient(
      to bottom,
      rgba(0, 255, 255, 0.5) 0%, 
      rgba(14, 25, 45, 0.9) 30%,
      rgba(14, 25, 45, 0.9) 70%,
      rgba(216, 7, 106, 0.5) 100%
    );
    
    border-radius: 16px;
  
    padding: 1.5rem 4rem;   /* Reducion del padding interno para compactar la tarjeta */
    width: 100%;
    max-width: 100%;
    text-align: center;
    border: 1px solid var(--vt-c-indigo);
   
    /* Sombra Neón */
    box-shadow: 
    0 0 15px rgba(216, 7, 106, 0.5),
    0 0 30px rgba(0, 255, 255, 0.1);
    
    transition: all 0.3s ease-in-out;
  }
   
  /* Efecto al pasar el mouse(hover) */
  .card-neon:hover {
    box-shadow: 
      0 0 20px var(--vt-c-indigo),
      0 0 40px var(--vt-c-cyan);
  }
   
  /*  Estilos de Texto  */
  h1 {
    font-family: 'Orbitron', sans-serif;
    color: var(--vt-c-cyan);
    font-size: 2.5rem;
    margin-bottom: 0.5rem;
    text-shadow: 0 0 10px rgba(0, 255, 255, 0.7); 
  }
  h2 {
    color: #fff;
    font-size: 1.2rem;
    font-weight: 500;
    margin-bottom: 1rem; 
    text-shadow: 0 0 8px var(--vt-c-indigo); 
  }
  p {
    color: #e0e0e0; 
    font-size: 1.1rem; 
    line-height: 1.5; 
    margin-bottom: 1.5rem; 
  }
  
  /* ESTILOS DE LA SECCIÓN DE CONTACTO TEXTUAL (COMPACTADO) */

  .contacto-info-lineal {
    display: flex; /* Alineación horizontal */
    flex-wrap: wrap; /* Permite que los elementos se envuelvan si es necesario */
    justify-content: center; 
    gap: 1.5rem; 
    padding: 0.5rem 0; /* Reduccion  del padding vertical */
    margin-bottom: 1rem; /* Reduccion del margen inferior */
    /* Líneas divisorias */
    border-top: 1px dashed var(--vt-c-black-mute);
    border-bottom: 1px dashed var(--vt-c-black-mute);
  }
  
  .contact-item {
    margin: 0;
    /* Reducimos aún más el tamaño de la fuente para compactar */
    font-size: 0.9rem; 
    font-weight: 500; 
    display: flex; 
    align-items: center;
  }
  
  .contact-item .label {
    color: var(--vt-c-cyan);
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
    gap: 15px; 
  }
   
  .icon-social {
    height: 2.8rem; 
    width: 2.8rem;
    stroke: currentColor; 
    filter: drop-shadow(0 0 1px var(--vt-c-black)); 
    transition: filter 0.2s, transform 0.2s;
  }
  
  .redes-sociales li a:hover .icon-social {
    transform: translateY(-4px) scale(1.1); 
    filter: drop-shadow(0 0 8px var(--vt-c-cyan)); 
  }
   
  /*Media Query */
  @media (max-width: 768px) {
    /* En tablets,cel, ajuste del padding del contenedor para evitar que la tarjeta toque los bordes */
    .datos-personales-container {
      padding: 0 0.5rem; /* mas cantidad de margen en celus */
    }
    .card-neon {
      padding: 1.5rem 2rem;
    }
    h1 {
      font-size: 2rem;
    }
    h2 {
      font-size: 1.1rem;
    }
    /* En tablets, y celulares pequeños(, apilala línea de contacto para ahorrar espacio ) */
    .contacto-info-lineal {
      flex-direction: column; 
      align-items: center;
      gap: 0.3rem; /* Más compacto */
    }
    .contact-item {
      font-size: 0.85rem; /* Fuente más pequeña para celular */
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
      font-size: 0.9rem; 
    }
    .icon-social {
      height: 2.2rem;
      width: 2.2rem;
    }
  }
  </style>