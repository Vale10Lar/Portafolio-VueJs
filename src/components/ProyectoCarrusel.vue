<template>
    <section id="proyectos" class="contenedor-proyectos-galeria">
      
      <!-- Botón Volver Arriba movido a la parte superior derecha -->
      <div class="contenedor-btn-regreso-neon">
        <button 
          @click="volverArriba" 
          class="btn-flotante-volver-arriba"
          aria-label="Volver a Navegación Principal"
        >
          <!-- Icono de flecha SVG que apunta hacia arriba -->
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor">
              <!-- La ruta M4.5 10.5 12 3m0 0 7.5 7.5M12 3v18 ya dibuja la flecha hacia arriba -->
              <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 10.5 12 3m0 0 7.5 7.5M12 3v18" />
          </svg>
        </button>
      </div>
  
      <h2 class="titulo-seccion">Proyectos Destacados</h2>
  
      <div class="galeria-carrusel">
        <div 
          class="proyectos-wrapper" 
          ref="carruselRef"
          @scroll="checkScroll"
        >
          <div 
            v-for="(proyecto, indice) in proyectos"
            :key="indice"
            class="tarjeta-flip-container"
            @mouseenter="flipCard(indice, true)"
            @mouseleave="flipCard(indice, false)"
            :class="{ 'flipped': proyectos[indice].isFlipped }"
            :style="{ '--accent-color': coloresItems[indice % coloresItems.length].color }"
          >
            <div class="tarjeta-flip-inner">
              
  
  <div class="tarjeta-flip-front">
                <div class="imagen-contenedor-visual">
                  <img 
                    :src="proyecto.imagenUrl || 'placeholder.jpg'" 
                    :alt="`Miniatura del proyecto ${proyecto.nombre}`" 
                    class="imagen-miniatura-proyecto"
                  />
                  <div class="imagen-overlay">
                      <span class="neon-text-pulse">{{ proyecto.demo ? 'DEMO ONLINE' : 'En Construcción' }}</span>
                  </div>
                </div>
                <h3 class="titulo-proyecto">{{ proyecto.nombre }}</h3>
                <p class="tecnologias-usadas">
                  Tecnologías: 
                  <span v-for="(tech, i) in proyecto.tecnologias" :key="i" class="tag-tech">
                    {{ tech }}
                  </span>
                </p>
              </div>
  
              
  
  <div class="tarjeta-flip-back">
                <h3 class="titulo-proyecto back-title">{{ proyecto.nombre }}</h3>
                <p class="descripcion-extendida">{{ proyecto.descripcionExtendida || proyecto.descripcion }}</p>
                
                <div class="enlaces-proyecto">
                  <a
                    v-if="proyecto.repo"
                    :href="proyecto.repo"
                    target="_blank"
                    class="boton-accion-enlace boton-repo"
                    :style="{ '--clr': coloresItems[indice % coloresItems.length].color }"
                    @click.stop="generarParticulas($event)"
                  >
                    <span>Repositorio</span>
                    <i></i><i></i><i></i><i></i>
                  </a>
  
                  <a
                    v-if="proyecto.demo"
                    :href="proyecto.demo"
                    target="_blank"
                    class="boton-accion-enlace boton-demo"
                    :style="{ '--clr': coloresItems[(indice + 1) % coloresItems.length].color }"
                    @click.stop="generarParticulas($event)"
                  >
                    <span>Ver Demo</span>
                    <i></i><i></i><i></i><i></i>
                  </a>
                </div>
              </div>
            </div>
          </div>
        </div>
  
        <button 
          class="nav-button prev-button" 
          :class="{ hidden: !canScrollLeft }" 
          @click="scrollCarrusel(-1)"
          :style="{ '--clr': coloresItems[0].color }"
          aria-label="Anterior Proyecto"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="3" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M15.75 19.5 8.25 12l7.5-7.5" /></svg>
        </button>
        <button 
          class="nav-button next-button" 
          :class="{ hidden: !canScrollRight }" 
          @click="scrollCarrusel(1)"
          :style="{ '--clr': coloresItems[2].color }"
          aria-label="Siguiente Proyecto"
        >
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="3" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="m8.25 4.5 7.5 7.5-7.5 7.5" /></svg>
        </button>
  
      </div>
  
      <!-- El contenedor de partículas ahora se ubica al final para mantener el orden -->
      <div id="contenedor-particulas-proyectos" ref="contenedorParticulas"></div>
    </section>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from "vue";
  
  // --- Datos Actualizados con URLs de Imagenes y una nueva propiedad isFlipped ---
  const coloresItems = ref([
    { color: "#FF44CC" }, // Rosa/Magenta Neón
    { color: "#39FF14" }, // Verde Neón
    { color: "#0FF0FC" }, // Azul/Cian Neón
    { color: "#A855F7" }, // Morado
  ]);
  
  const proyectos = ref([
    {
      nombre: "ByteNine E-commerce",
      tecnologias: ["Vue.js", "Node.js", "MongoDB"],
      descripcion: "Plataforma de comercio electrónico con carrito de compras y gestión de inventario.",
      descripcionExtendida: "Un robusto e-commerce construido con Vue.js para el frontend, Node.js y Express para la API backend, y MongoDB como base de datos. Incluye autenticación de usuarios, pasarela de pago simulada, gestión de productos y carrito de compras dinámico. Optimizado para una experiencia de usuario fluida y segura.",
      repo: "https://wisdomtech.academy/comercio-electronico-ejemplos/",
      //  ruta relativa apara la imagen del proyecto'
      imagenUrl: "./src/assets/imagenesproyectos/ecom7.webp", 
      isFlipped: false,
    },
    {
      nombre: "Portafolio Neon en Vue ",
      tecnologias: ["Vue 3", "SCSS", "Animaciones CSS"],
      descripcion: "Creación de un diseño de portafolio minimalista con fuerte estética Cyberpunk y efectos de luz neón.",
      descripcionExtendida: "Un diseño de portafolio web con una marcada estética Cyberpunk. Implementado con Vue 3 para componentes dinámicos y SCSS para estilos modulares. Incorpora animaciones CSS complejas para efectos de neón, paralaje y transiciones fluidas que capturan la esencia futurista.",
      repo: "https://github.com/Vale10Lar/Neon-Portfolio-Clone",//Ver para ponerle el elace a github
      demo: "./src/assets/videos_demo/vue.mp4",
     //  ruta relativa apara la imagen del proyecto'
     imagenUrl: "./src/assets/imagenesproyectos/portafolio_vuev1.png", 
      isFlipped: false,
    },
    {
      nombre: "Portafolio",
      tecnologias: ["HTML", "Js", "css"],
      descripcion: "Primer portafolio web.",
      repo: "https://github.com/Vale10Lar/proyecto-cv",
      demo: "./src/assets/videos_demo/portafolio1_demo.mp4",
     //  ruta relativa apara la imagen del proyecto'
     imagenUrl: "./src/assets/imagenesproyectos/portafolio1.png", 
      isFlipped: false,
    },
    {
      nombre: "Ahorjado 2D",
      tecnologias: ["Java"],
      descripcion: "Un pequeño juego de ahorcado cdidactico con intergfaz GUI y lógica de juego básica en java.",
      descripcionExtendida: "Desarrollo de un juego de ahorcado en 2D . Material educativo.",
      repo: "https://github.com/HotCode2025/PI_SegundoSemestre-ByteNine",
      demo: "./src/assets/videos_demo/demo_ahorcado.mp4",
      // Se usa la URL del tercer color neón para este placeholder
      imagenUrl: "./src/assets/imagenesproyectos/juego_ahorcado.png", 
      isFlipped: false,
    },

  ]);
  
  
  // --- Lógica del Carrusel ---
  const carruselRef = ref(null);
  const canScrollLeft = ref(false);
  const canScrollRight = ref(true); 
  
  const checkScroll = () => {
    const el = carruselRef.value;
    if (el) {
      canScrollLeft.value = el.scrollLeft > 0;
      const scrollMax = el.scrollWidth - el.clientWidth;
      canScrollRight.value = el.scrollLeft < scrollMax - 5; 
    }
  };
  
  const scrollCarrusel = (direction) => {
    const el = carruselRef.value;
    if (el) {
      // Un desplazamiento ajustado para ver la siguiente tarjeta completa
      const cardWidth = 330; // Ancho fijo de la tarjeta (300px) + gap (30px)
      el.scrollBy({
        left: direction * cardWidth,
        behavior: 'smooth'
      });
      setTimeout(checkScroll, 500);
    }
  };
  
  onMounted(() => {
    checkScroll();
    window.addEventListener('resize', checkScroll);
    if (carruselRef.value) carruselRef.value.scrollLeft = 0; 
  });
  
  onUnmounted(() => {
    window.removeEventListener('resize', checkScroll);
  });
  
  
  // --- Lógica de Flip Card ---
  const flipCard = (index, doFlip) => {
    proyectos.value[index].isFlipped = doFlip;
  };
  
  
  // --- Lógica de Partículas y Scroll ---
  const volverArriba = () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
  };
  
  const contenedorParticulas = ref(null);
  const generarParticulas = (evento) => {
    // Aseguramos de que el evento no se propague al contenedor de la tarjeta
    // para evitar que se voltee si se hace clic en un botón.
    evento.stopPropagation(); 
  
    const contenedor = contenedorParticulas.value;
    if (!contenedor) return;
  
    const rectBoton = evento.currentTarget.getBoundingClientRect();
    const clickX = rectBoton.left + rectBoton.width / 2;
    const clickY = rectBoton.top + rectBoton.height / 2;
    const rectContenedor = contenedor.getBoundingClientRect();
    const centroX = clickX - rectContenedor.left;
    const centroY = clickY - rectContenedor.top;
    
    const cantidadParticulas = Math.floor(Math.random() * 8) + 8;
    const colores = [coloresItems.value[0].color, coloresItems.value[1].color, coloresItems.value[2].color];
  
    for (let i = 0; i < cantidadParticulas; i++) {
      const particula = document.createElement("span");
      particula.className = "particula-estrella-proy"; 
      particula.style.left = `${centroX}px`;
      particula.style.top = `${centroY}px`;
      
      const tamano = Math.random() * 3 + 1;
      particula.style.width = `${tamano}px`;
      particula.style.height = `${tamano}px`;
      
      const colorParticula = colores[Math.floor(Math.random() * colores.length)];
      particula.style.backgroundColor = colorParticula;
      particula.style.setProperty("--particula-color", colorParticula);
  
      const dx = (Math.random() - 0.5) * 150;
      const dy = (Math.random() - 0.5) * 150;
      const duracion = Math.random() * 0.8 + 0.4;
  
      particula.style.setProperty("--dx", `${dx}px`);
      particula.style.setProperty("--dy", `${dy}px`);
      particula.style.animationDuration = `${duracion}s`;
  
      contenedor.appendChild(particula);
      particula.onanimationend = () => { particula.remove(); };
    }
  };
  </script>
  
  <style scoped>
  /*----- VARIABLES -----*/
  :root {
    --vt-c-cyan: #00ffff;
    --vt-c-indigo: #a855f7;
    --vt-c-magenta-original: #ff1493;
    --vt-c-black: #03091b;
    --vt-c-white: #ebe0ff;
    --vt-c-white-soft: #c8c8c8; 
    --vt-c-black-soft: #151b33;
    --neon-green: #39FF14; 
    --neon-blue: #0FF0FC; 
    --neon-pink: #FF44CC; 
    --navbar-height: 80px; /* Importado para referencia */
  }
  
  /*----- ANIMACIONES COMPARTIDAS (Mantenidas y ajustadas) -----*/
  @keyframes container-neon-pulse { /* Para el botón "Volver Arriba" */
      0% {
          box-shadow: 0 0 10px rgba(57, 255, 20, 0.7), 0 0 25px var(--neon-blue), 0 0 40px var(--neon-pink);
          border-color: var(--neon-green);
      }
      33% {
          box-shadow: 0 0 15px var(--neon-blue), 0 0 30px var(--neon-pink), 0 0 50px var(--neon-green);
          border-color: var(--neon-blue);
      }
      66% {
          box-shadow: 0 0 12px var(--neon-pink), 0 0 28px var(--neon-green), 0 0 45px var(--neon-blue);
          border-color: var(--neon-pink);
      }
      100% {
          box-shadow: 0 0 10px rgba(57, 255, 20, 0.7), 0 0 25px var(--neon-blue), 0 0 40px var(--neon-pink);
          border-color: var(--neon-green);
      }
  }
  @keyframes star-fly-proy { /* Animación para las partículas neón */
    0% { opacity: 1; transform: translate(0, 0) rotate(45deg) scale(1); }
    20% { opacity: 1; }
    100% { transform: translate(var(--dx), var(--dy)) rotate(45deg) scale(0); opacity: 0; }
  }
  @keyframes pulse-neon-text { /* Para el texto "DEMO ONLINE" */
    0%, 100% { text-shadow: 0 0 5px var(--vt-c-white), 0 0 10px var(--vt-c-white); opacity: 0.8; }
    50% { text-shadow: 0 0 15px var(--vt-c-magenta-original), 0 0 25px var(--vt-c-magenta-original); opacity: 1; } 
  }
  @keyframes move { /* Para los bordes del botón de acción */
    0% { transform: translate(0); }
    100% { transform: translate(100%); }
  }
  
  /* ------------------------------------------------------------- */
  /* LAYOUT PRINCIPAL Y TÍTULO (Ajustado para eliminar scroll vertical) */
  /* ------------------------------------------------------------- */
  .contenedor-proyectos-galeria {
    /* Reducido el padding vertical para centrar el contenido y evitar el scroll no deseado */
    padding: 1rem 1rem 3rem; 
    max-width: 100%;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow: hidden; 
    justify-content: center; 
    /* Agregamos un alto mínimo para asegurar que haya espacio en la pantalla */
    min-height: calc(100vh - var(--navbar-height) - 100px); 
  }
  
  .titulo-seccion {
    font-family: "Orbitron", sans-serif;
    font-size: 2.5rem;
    font-weight: 700;
    text-align: center;
    margin-bottom: 2.5rem;
    color: var(--vt-c-magenta-original); 
    text-shadow: 0 0 5px var(--vt-c-magenta-original), 0 0 15px rgba(255, 20, 147, 0.5);
    padding-bottom: 0.5rem;
    border-bottom: 2px solid rgba(168, 85, 247, 0.2);
  }
  
  /* ------------------------------------------------------------- */
  /* CARRUSEL/GALERÍA (Mantenido) */
  /* ------------------------------------------------------------- */
  .galeria-carrusel {
    position: relative;
    width: 95%;
    max-width: 1200px;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 450px; /* Asegura un alto mínimo para las tarjetas */
  }
  
  .proyectos-wrapper {
    display: flex;
    overflow-x: scroll; 
    padding: 1rem 10px; 
    gap: 30px; 
    scroll-snap-type: x mandatory; 
    scrollbar-width: thin; 
    scrollbar-color: var(--vt-c-indigo) var(--vt-c-black-soft); 
    max-width: 100%;
    box-sizing: border-box;
  }
  .proyectos-wrapper::-webkit-scrollbar { height: 8px; }
  .proyectos-wrapper::-webkit-scrollbar-track { background: var(--vt-c-black-soft); border-radius: 10px; }
  .proyectos-wrapper::-webkit-scrollbar-thumb { background: var(--vt-c-indigo); border-radius: 10px; }
  
  
  /* ------------------------------------------------------------- */
  /* TARJETA FLIP - CONTENEDOR PRINCIPAL (Mantenido) */
  /* ------------------------------------------------------------- */
  .tarjeta-flip-container {
    --accent-color: #A855F7; 
    flex-shrink: 0; 
    width: 300px; 
    height: 400px; 
    perspective: 1000px; 
    position: relative;
    scroll-snap-align: center; 
    transition: all 0.3s ease;
  }
  
  .tarjeta-flip-container:hover {
    transform: translateY(-5px) scale(1.02);
  }
  
  .tarjeta-flip-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s; 
    transform-style: preserve-3d; 
    border-radius: 15px;
    border: 2px solid var(--accent-color);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.8), inset 0 0 5px var(--accent-color);
    background: var(--vt-c-black-soft); 
  }
  
  .tarjeta-flip-container.flipped .tarjeta-flip-inner {
    transform: rotateY(180deg); 
  }
  
  /* Estilos comunes para ambas caras */
  .tarjeta-flip-front, .tarjeta-flip-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden; 
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    justify-content: space-between; 
    align-items: center;
    padding: 1rem;
    box-sizing: border-box;
    border-radius: 15px; 
    background: var(--vt-c-black-soft); 
  }
  
  /* CARA FRONTAL */
  .tarjeta-flip-front {
    color: var(--vt-c-white);
  }
  
  /* CARA TRASERA */
  .tarjeta-flip-back {
    background: var(--vt-c-black-soft);
    color: var(--vt-c-white-soft);
    transform: rotateY(180deg); 
    padding: 1.5rem; 
    overflow-y: auto; 
    scrollbar-width: none; 
  }
  .tarjeta-flip-back::-webkit-scrollbar {
      display: none;
  }
  
  .titulo-proyecto { 
    font-family: "Orbitron", sans-serif;
    font-size: 1.3rem; 
    color: var(--accent-color);
    text-shadow: 0 0 5px var(--accent-color);
    margin-bottom: 0.5rem;
    margin-top: 0.5rem;
  }
  
  .back-title {
    font-size: 1.1rem; 
    margin-top: 0; 
  }
  
  .descripcion-extendida {
    font-size: 0.9rem;
    line-height: 1.5;
    color: var(--vt-c-white-soft);
    text-align: left; 
    flex-grow: 1; 
    margin-bottom: 1rem; 
  }
  
  .tecnologias-usadas {
    font-size: 0.8rem; 
    color: var(--vt-c-white-soft);
    font-weight: 600;
    text-align: center;
    margin-top: auto; 
    margin-bottom: 0.5rem;
  }
  
  .tag-tech {
    display: inline-block;
    background-color: rgba(168, 85, 247, 0.15); 
    color: var(--vt-c-cyan);
    border-radius: 4px;
    padding: 2px 6px;
    margin-left: 5px;
    font-weight: 400;
    text-shadow: 0 0 3px rgba(0, 255, 255, 0.5);
    font-size: 0.7rem; 
  }
  
  /* ------------------------------------------------------------- */
  /* CONTENEDOR VISUAL DE IMAGEN (Mantenido) */
  /* ------------------------------------------------------------- */
  .imagen-contenedor-visual {
      position: relative;
      width: 100%;
      padding-top: 62.5%; 
      overflow: hidden;
      border-radius: 8px;
      margin-bottom: 0.8rem; 
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
      border: 1px solid var(--accent-color);
      background-color: var(--vt-c-black);
  }
  
  .imagen-miniatura-proyecto {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
      transition: filter 0.5s ease;
      filter: brightness(0.7) grayscale(0.2); 
  }
  
  .imagen-contenedor-visual:hover .imagen-miniatura-proyecto {
      filter: brightness(1) grayscale(0);
  }
  
  .imagen-overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(21, 27, 51, 0.6); 
      display: flex;
      justify-content: center;
      align-items: flex-end; 
      padding-bottom: 10px;
      z-index: 5;
      pointer-events: none;
  }
  .neon-text-pulse {
      font-family: 'Orbitron', sans-serif;
      color: var(--vt-c-magenta-original);
      text-shadow: 0 0 5px var(--vt-c-magenta-original);
      font-size: 0.7rem; 
      animation: pulse-neon-text 3s infinite alternate;
  }
  
  /* ------------------------------------------------------------- */
  /* ENLACES DE PROYECTO (Mantenido) */
  /* ------------------------------------------------------------- */
  .enlaces-proyecto {
    display: flex;
    justify-content: space-around;
    gap: 10px;
    margin-top: auto; 
    width: 100%;
    flex-shrink: 0; 
  }
  
  .boton-accion-enlace {
    --clr: var(--clr); 
    --inner-bg: #151b33; 
    position: relative;
    display: inline-block;
    padding: 0.5rem 0.8rem; 
    color: var(--clr);
    text-decoration: none;
    font-size: 0.75rem; 
    font-family: 'Orbitron', sans-serif;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    overflow: hidden; 
    transition: 0.5s;
    background: var(--inner-bg);
    border: none;
    cursor: pointer;
    border-radius: 5px;
    z-index: 10;
    width: 48%; 
    white-space: nowrap; 
  }
  .boton-accion-enlace span { position: relative; z-index: 1; }
  .boton-accion-enlace:hover {
      color: var(--vt-c-black-soft); 
      background: var(--clr);
      box-shadow: 0 0 10px var(--clr), 0 0 25px var(--clr);
  }
  .boton-accion-enlace i { position: absolute; display: block; }
  .boton-accion-enlace i:nth-child(1) { top: 0; left: 0; width: 100%; height: 2px; background: linear-gradient(90deg, transparent, var(--clr)); animation: move 1s linear infinite; }
  .boton-accion-enlace i:nth-child(2) { top: -100%; right: 0; width: 2px; height: 100%; background: linear-gradient(180deg, transparent, var(--clr)); animation: move 1s linear infinite; animation-delay: 0.25s; }
  .boton-accion-enlace i:nth-child(3) { bottom: 0; right: 0; width: 100%; height: 2px; background: linear-gradient(270deg, transparent, var(--clr)); animation: move 1s linear infinite; animation-delay: 0.5s; }
  .boton-accion-enlace i:nth-child(4) { bottom: -100%; left: 0; width: 2px; height: 100%; background: linear-gradient(360deg, transparent, var(--clr)); animation: move 1s linear infinite; animation-delay: 0.75s; }
  
  
  /* Botones de Navegación del Carrusel (Mantenido) */
  .nav-button {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: var(--vt-c-black-soft);
    border: 2px solid var(--clr);
    box-shadow: 0 0 10px var(--clr);
    color: var(--clr);
    cursor: pointer;
    transition: all 0.3s ease;
    z-index: 20;
  }
  .nav-button:hover {
    background: var(--clr);
    color: var(--vt-c-black);
    box-shadow: 0 0 15px var(--clr), 0 0 30px var(--clr);
  }
  .nav-button.hidden {
    opacity: 0;
    pointer-events: none;
  }
  .prev-button { left: 0; }
  .next-button { right: 0; }
  .nav-button svg { width: 25px; height: 25px; }
  
  
  /* ------------------------------------------------------------- */
  /* BOTÓN VOLVER ARRIBA (CORREGIDO) */
  /* ------------------------------------------------------------- */
  .contenedor-btn-regreso-neon {
      position: absolute;
      top: 20px; 
      right: 30px; 
      z-index: 30; 
      width: 65px; height: 65px;
      border-radius: 50%; 
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer; 
      background: radial-gradient(circle at 50% 50%, #2a2a2e 0%, var(--vt-c-black-soft) 100%);
      border: 2px solid var(--neon-green); 
      box-shadow: 0 0 10px rgba(57, 255, 20, 0.7), 0 0 25px var(--neon-blue), 0 0 40px var(--neon-pink); 
      animation: container-neon-pulse 3.5s infinite alternate ease-in-out; 
      transition: all 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  }
  
  .contenedor-btn-regreso-neon:hover {
      border-color: var(--vt-c-white); 
      box-shadow: 0 0 15px var(--neon-green), 0 0 30px var(--neon-green), 0 0 60px var(--neon-green); 
      transform: scale(1.08); 
      animation: none; 
  }
  .btn-flotante-volver-arriba {
      width: 50px; height: 50px; border-radius: 50%; background: #ff00ff; border: none; 
      color: var(--neon-green); 
      display: flex; justify-content: center; align-items: center; cursor: pointer;
      transition: all 0.3s ease; box-shadow: 0 0 5px rgba(57, 255, 20, 0.3); 
  }
  .btn-flotante-volver-arriba:hover {
      color: var(--vt-c-white); box-shadow: 0 0 8px rgba(255, 255, 255, 0.5); transform: scale(1.05); 
  }
  .btn-flotante-volver-arriba svg {
      width: 26px; height: 26px; transition: transform 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55); 
      /* ¡CORRECCIÓN! Eliminamos la rotación (rotateZ) para que la flecha apunte hacia arriba. */
      transform: translateY(0px); 
  }
  .btn-flotante-volver-arriba:hover svg {
      /* Mantenemos el desplazamiento vertical para el efecto visual al hacer hover */
      transform: translateY(-8px); filter: drop-shadow(0 0 5px var(--vt-c-white)); 
  }
  
  
  /* ------------------------------------------------------------- */
  /* EFECTO DE PARTÍCULAS (Mantenido) */
  /* ------------------------------------------------------------- */
  #contenedor-particulas-proyectos {
    position: absolute;
    top: 0; left: 0; width: 100%; height: 100%;
    pointer-events: none;
    overflow: hidden;
    z-index: 5;
  }
  
  
  /* ------------------------------------------------------------- */
  /* RESPONSIVE (Ajustado) */
  /* ------------------------------------------------------------- */
  @media (max-width: 768px) {
    .titulo-seccion { font-size: 2rem; margin-bottom: 2rem; }
    
    .contenedor-proyectos-galeria {
       /* Más padding superior en móvil porque la barra de navegación es más grande */
      padding-top: 5rem;
    }
    
    .proyectos-wrapper { 
      padding: 1rem 5px; 
      gap: 15px; 
    }
    .tarjeta-flip-container {
      width: 85vw; 
      max-width: 300px; 
      height: 380px; 
    }
    .enlaces-proyecto { flex-direction: column; gap: 8px; }
    .boton-accion-enlace { 
      width: 100%; 
      font-size: 0.7rem; 
      padding: 0.4rem 0.6rem;
    }
    
    .nav-button { width: 40px; height: 40px; }
    .nav-button svg { width: 20px; height: 20px; }
    .prev-button { left: 5px; }
    .next-button { right: 5px; }
  
    /* Ajuste botón volver arriba en móvil */
    .contenedor-btn-regreso-neon {
      top: 15px; right: 15px; width: 55px; height: 55px;
    }
  }
  </style>
