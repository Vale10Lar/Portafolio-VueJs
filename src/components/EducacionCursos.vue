<template>
  <!-- Contenedor principal de la sección de Educación y Cursos -->
  <section id="educacion" class="contenedor-educacion-cursos">
    <h2 class="titulo-seccion">Educación y Cursos</h2>
    
    <!-- Contenedor principal para la línea de tiempo interactiva (Desktop: Horizontal, Mobile: Vertical) -->
    <div class="contenedor-interactivo-tiempo">
      
      <!-- 1. Contenedor de los Años (Navegación) -->
      <div class="contenedor-anos-tiempo">
        <div 
          v-for="(item, indice) in educacion" 
          :key="indice" 
          :class="['boton-ano', { 'activo': indiceActivo === indice }]"
          :style="{ '--accent-color': coloresItems[indice % coloresItems.length].color }"
          @click="alternarActivo(indice)" 
        >
          <span :class="{ 'pulso-neon': indiceActivo === -1 }">
            {{ item.fecha }}
          </span>
        </div>
      </div>

      <!-- 2. Contenedor del Contenido Desplegable (Tarjeta de Contenido) -->
      <div class="contenedor-muestra-contenido-tiempo">
        
        <!-- Tarjeta de BIENVENIDA (se muestra cuando indiceActivo es -1) -->
        <div 
          v-if="indiceActivo === -1"
          class="contenedor-contenido tarjeta-bienvenida"
        >
          <div class="grupo-titulo-muestra">
            <h3 class="titulo titulo-bienvenida">¡Bienvenido!</h3>
          </div>
          <div class="descripcion">
            Explora mi trayectoria academica. 
          </div>
          
          <!-- Botón "Empezar" -->
          <button 
            class="boton-accion-nuevo pulso-enlace" 
            :style="{ '--clr': coloresItems[0].color }"
            @click="manejarClicEmpezar($event)" 
          >
            <span>Empezar {{ educacion[0].fecha }}</span>
            <i></i>
          </button>
        </div>
        
        <!-- Tarjeta de CONTENIDO (se muestra cuando hay un itemActivo) -->
        <div 
          v-else-if="itemActivo"
          :key="itemActivo.fecha"
          class="contenedor-contenido tarjeta-contenido-activa"
          :style="{ '--accent-color': coloresItems[indiceActivo % coloresItems.length].color }"
        >
          <!-- Botón de Cerrar [X] -->
          <button 
            class="boton-cerrar" 
            @click="generarParticulas($event); establecerActivo(-1);"
          >
            [ X ]
          </button>
          
          <div class="grupo-titulo-muestra">
            <h3 class="titulo">{{ itemActivo.title }}</h3>
            <span class="fecha-muestra-movil">{{ itemActivo.fecha }}</span>
          </div>
          <div class="descripcion">{{ itemActivo.descripcion }}</div>
          
          <!-- Botón de Enlace a Certificado/Detalles -->
          <a 
            class="boton-accion-nuevo enlace-accion" 
            :href="itemActivo.enlace" 
            target="_blank" 
            :style="{ '--clr': coloresItems[indiceActivo % coloresItems.length].color }"
            
            @click="generarParticulas($event); establecerActivo(-1);"
          >
            <span>
              {{ itemActivo.enlaceTexto }}
            </span>
            <i></i>
          </a>
        </div>
      </div>
    </div>
    
    <!-- Contenedor para las partículas: Ahora dentro del contenedor principal de la sección -->
    <div id="contenedor-particulas" ref="contenedorParticulas"></div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue';

const indiceActivo = ref(-1); 

const establecerActivo = (indice) => {
    indiceActivo.value = indice;
};

const alternarActivo = (indice) => {
    if (indiceActivo.value === indice) {
        indiceActivo.value = -1;
    } else {
        indiceActivo.value = indice;
    }
};

// --- Template Ref para el contenedor de partículas ---
const contenedorParticulas = ref(null);

// --- Lógica del Efecto de Partículas (CORRECCIÓN CLAVE) ---
const generarParticulas = (evento) => {
    const contenedor = contenedorParticulas.value;
    if (!contenedor) return;

    // 1. Obtener la posición del clic (o centro del botón) en la ventana
    const rectBoton = evento.currentTarget.getBoundingClientRect();
    const clickX = rectBoton.left + rectBoton.width / 2;
    const clickY = rectBoton.top + rectBoton.height / 2;

    // 2. Obtener la posición del contenedor de partículas
    const rectContenedor = contenedor.getBoundingClientRect();
    
    // 3. Calcular la posición inicial de la partícula relativa al contenedor
    const centroX = clickX - rectContenedor.left;
    const centroY = clickY - rectContenedor.top;
    
    const cantidadParticulas = Math.floor(Math.random() * 8) + 8; 

    for (let i = 0; i < cantidadParticulas; i++) {
        const particula = document.createElement('span');
        particula.className = 'particula-estrella';
        
        // Aplicar la posición inicial calculada
        particula.style.left = `${centroX}px`;
        particula.style.top = `${centroY}px`;
        
        const tamano = Math.random() * 3 + 1; 
        particula.style.width = `${tamano}px`;
        particula.style.height = `${tamano}px`;
        
        const colores = ['var(--vt-c-cyan)', 'var(--vt-c-indigo)', 'var(--vt-c-magenta)'];
        const colorParticula = colores[Math.floor(Math.random() * colores.length)];
        particula.style.backgroundColor = colorParticula;
        particula.style.setProperty('--particula-color', colorParticula);
        
        // Dispersión
        const dx = (Math.random() - 0.5) * 150; 
        const dy = (Math.random() - 0.5) * 150; 
        const duracion = Math.random() * 0.8 + 0.4; 
        
        particula.style.setProperty('--dx', `${dx}px`);
        particula.style.setProperty('--dy', `${dy}px`);
        particula.style.animationDuration = `${duracion}s`;

        contenedor.appendChild(particula);

        particula.onanimationend = () => {
            particula.remove();
        };
    }
};

const manejarClicEmpezar = (evento) => {
    generarParticulas(evento);
    alternarActivo(0);
};

// --- Datos y Estilos ---
const coloresItems = ref([
    { color: '#39FF14' }, // Verde Neón
    { color: '#0FF0FC' }, // Azul/Cian Neón
    { color: '#FF44CC' }, // Rosa/Magenta Neón
    { color: '#8A2BE2' }, // Morado
]);

const educacion = ref([
    {
        fecha: '2025', 
        title: 'Técnico Universitario en Programación (UTN)',
        descripcion: 'Foco en desarrollo de software, análisis de sistemas, y metodologías ágiles. Preparación para soluciones Full Stack.',
        enlace: 'https://utn.edu.ar/frsr/',
        enlaceTexto: 'Ver Título'
    },
    {
        fecha: '2024', 
        title: 'Manejo Avanzado de Bases de Datos SQL',
        descripcion: 'Estudio intensivo en consultas complejas, optimización de rendimiento y diseño de esquemas relacionales.',
        enlace: 'https://ejemplo.com/certificado-sql',
        enlaceTexto: 'Ver Certificado '
    },
    {
        fecha: '2023', 
        title: 'Fundamentos de JavaScript y Backend',
        descripcion: 'Adquisición de bases de programación y lógica del lado del servidor para el desarrollo de APIs web.',
        enlace: 'https://ejemplo.com/certificado-js',
        enlaceTexto: 'Ver Certificado '
    },
    {
        fecha: '2022', 
        title: 'Introducción a Sistemas Operativos',
        descripcion: 'Comprensión de la arquitectura, gestión de memoria y procesos en sistemas operativos como Linux y Windows.',
        enlace: '#',
        enlaceTexto: 'Ver Detalles'
    },
]);

const itemActivo = computed(() => indiceActivo.value !== -1 ? educacion.value[indiceActivo.value] : null);

</script>

<style scoped>
/* ============================================== */
/* VARIABLES Y ESTILOS BASE */
/* ============================================== */

:root {
  --vt-c-cyan: #00ffff;
  --vt-c-indigo: #a855f7; 
  --vt-c-magenta: #ff00ff; 
  --vt-c-black: #03091b; 
  --vt-c-white: #ebe0ff;
  --vt-c-black-soft: #151b33; 
  --vt-c-black-mute: #283049;
  --bgColor: var(--vt-c-black-soft); 
  
  --welcome-gradient-border: linear-gradient(90deg, var(--vt-c-indigo) 0%, var(--vt-c-cyan) 50%, var(--vt-c-magenta) 100%);
  --welcome-title-color: var(--vt-c-indigo); 

  /* Variable específica para el fondo interno del nuevo botón */
  --btn-inner-bg: #272822; 
}


/* Estilos generales del contenedor y título... */
.contenedor-educacion-cursos {
  padding: 2rem 1rem;
  margin: 0 auto;
  max-width: 1000px;
  display: flex;
  flex-direction: column;
  position: relative; 
}
.titulo-seccion {
  font-family: "Orbitron", sans-serif;
  font-size: 2.2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 2rem;
  color: var(--vt-c-indigo);
  text-shadow: 0 0 5px var(--vt-c-indigo), 0 0 15px rgba(168, 85, 247, 0.5);
  border-bottom: 2px solid var(--vt-c-black-mute);
  padding-bottom: 0.5rem;
}

/* ============================================== */
/* 2. Estructura de la Línea de Tiempo */
/* ============================================== */

.contenedor-interactivo-tiempo {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

/* Contenedor de los Años */
.contenedor-anos-tiempo {
  display: flex;
  justify-content: space-around;
  align-items: flex-end; 
  width: 100%;
  padding-bottom: 1rem;
  /* Línea horizontal debajo de los años, simulando la línea de tiempo */
  border-bottom: 3px dashed var(--vt-c-cyan);
}

.boton-ano {
  font-family: "Orbitron", sans-serif;
  font-weight: 700;
  font-size: 1.2rem;
  color: var(--vt-c-white);
  padding: 0.5rem 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  background: var(--bgColor);
  border: 2px dashed #ce5ece;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(52, 8, 228, 0.7);
}

.pulso-neon {
    display: inline-block;
    animation: neon-pulse 1.5s infinite alternate ease-in-out;
}
@keyframes neon-pulse {
    from {
        text-shadow: 0 0 5px var(--vt-c-white), 0 0 10px var(--vt-c-white);
        opacity: 0.8;
    }
    to {
        text-shadow: 0 0 15px var(--vt-c-cyan), 0 0 25px var(--vt-c-cyan);
        opacity: 1;
    }
}

.boton-ano:hover, .boton-ano.activo {
  color: var(--vt-c-black); 
  transform: translateY(-5px); 
  background-color: var(--accent-color);
  box-shadow: 
    0 0 5px var(--accent-color), 
    0 0 15px var(--accent-color), 
    0 5px 20px rgba(0, 0, 0, 0.8);
}
.boton-ano.activo .pulso-neon {
    animation: none;
}

.boton-ano::after {
  content: "";
  position: absolute;
  bottom: -1.7rem; 
  left: 50%;
  transform: translateX(-50%);
  width: 1rem;
  height: 1rem;
  background: var(--vt-c-black);
  border: 0.25rem solid var(--accent-color); 
  border-radius: 4px;
  box-shadow: 0 0 8px var(--accent-color);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  animation: rotate 3s linear infinite;
}
@keyframes rotate {
  from { transform: translateX(-50%) rotate(0deg); }
  to { transform: translateX(-50%) rotate(360deg); }
}

.boton-ano.activo::after {
  box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
}

.contenedor-muestra-contenido-tiempo {
  display: flex;
  justify-content: center;
  padding-top: 1rem;
}

/* Sombra Neón a la Tarjeta de Contenido Activa */
.contenedor-contenido {
  background: var(--bgColor);
  padding: 1.5rem;
  border-radius: 10px;
  
  /* ESTILOS BASE */
  border: 2px solid var(--accent-color); 
  box-shadow: 
    0 0 25px rgba(0, 0, 0, 0.9), 
    inset 0 0 5px var(--accent-color); 
    
  animation: fadeIn 0.5s ease-out;
  width: 90%; 
  max-width: 600px; /* Mantenemos el máximo para desktop */
  position: relative; 
}

/* APLICAMOS LA SOMBRA NEÓN DINÁMICA AQUÍ (Solo para la tarjeta activa) */
.tarjeta-contenido-activa {
    /* La sombra interior y el borde ya usan --accent-color, ahora agregamos una exterior */
    box-shadow: 
        0 0 5px var(--accent-color), /* Sombra pequeña y cercana */
        0 0 15px rgba(0, 0, 0, 0.9), /* Sombra oscura para profundidad */
        0 0 30px rgba(0, 0, 0, 0.8), /* Sombra oscura más grande */
        inset 0 0 5px var(--accent-color), /* Sombra interior */
        0 0 20px var(--accent-color); /* Sombra de neón exterior fuerte */
}

/* Tarjeta de Bienvenida: Estilo de Borde Degradado */
.tarjeta-bienvenida {
    border: none; 
    padding: 2px; 
    background: var(--welcome-gradient-border) border-box; 
    border-radius: 10px;
    /* La sombra de la bienvenida se mantiene en el degradado de colores */
    box-shadow: 
      0 0 20px rgba(168, 85, 247, 0.2), 
      0 0 20px rgba(0, 255, 255, 0.2),
      inset 0 0 10px rgba(0, 0, 0, 0.5); 
}

/* Estilos internos de la tarjeta */
.tarjeta-bienvenida > .grupo-titulo-muestra, 
.tarjeta-bienvenida > .descripcion {
    background: var(--bgColor); 
    border-radius: 8px;
}
.tarjeta-bienvenida > .grupo-titulo-muestra { padding-top: 0.5rem; }
.tarjeta-bienvenida > .descripcion { padding-block: 1rem 1.5rem; }

.titulo-bienvenida { color: var(--welcome-title-color); }

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.grupo-titulo-muestra {
  padding-bottom: 0.5rem;
  border-bottom: 1px dashed var(--vt-c-black-mute);
}
.titulo {
  font-weight: 700;
  font-size: 1.5rem;
  color: var(--vt-c-cyan); 
  font-family: "Orbitron", sans-serif;
  text-align: center;
  filter: brightness(0.85); 
}
.fecha-muestra-movil { display: none; }
.descripcion {
  padding-block: 1rem 1.5rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white); 
  line-height: 1.4;
  text-align: center;
}

/* ============================================== */
/* 3. Estilos del NUEVO BOTÓN */
/* ============================================== */

/* Botón base */
.boton-accion-nuevo {
  position: relative;
  background: var(--btn-inner-bg);
  color: #fff;
  text-decoration: none;
  text-transform: uppercase;
  border: none;
  /* AJUSTE: Reducción del espaciado entre letras */
  letter-spacing: 0.05rem; 
  /* AJUSTE: Reducción del tamaño de fuente */
  font-size: 0.9rem; 
  /* AJUSTE: Reducción del padding */
  padding: 0.7rem 2rem; 
  transition: 0.2s;
  cursor: pointer;
  
  /* Layout */
  display: flex;
  justify-content: center;
  align-items: center;
  /* AJUSTE: Reducción del ancho máximo del botón */
  width: 60%; 
  max-width: 200px; 
  margin: 0.8rem auto 0;
  border-radius: 0;
  font-family: "Orbitron", sans-serif;
}

/* Estado Hover/Focus */
.boton-accion-nuevo:hover, .boton-accion-nuevo:focus {
  /* AJUSTE: Ajustar el espaciado y padding del hover */
  letter-spacing: 0.1rem;
  padding: 0.8rem 2.1rem;
  background: var(--clr);
  color: var(--clr);
  animation: box 3s infinite;
  outline: none;
}

/* Fondo interno */
.boton-accion-nuevo::before {
  content: "";
  position: absolute;
  inset: 2px;
  background: var(--btn-inner-bg);
  transition: 0.2s;
  z-index: 0;
}
.boton-accion-nuevo:hover::before {
    background: #18191f;
}

/* Texto (span) */
.boton-accion-nuevo span {
  position: relative;
  z-index: 1;
}

/* Contenedor del efecto de borde (i) */
.boton-accion-nuevo i {
  position: absolute;
  inset: 0;
  display: block;
  z-index: 2;
}

/* Tiras superiores/izquierdas */
.boton-accion-nuevo i::before {
  content: "";
  position: absolute;
  width: 8px; /* Reducción de ancho */
  height: 2px;
  left: 80%;
  top: -2px;
  border: 2px solid var(--clr);
  background: var(--btn-inner-bg);
  transition: 0.2s;
}

.boton-accion-nuevo:hover i::before {
  width: 12px; /* Ajuste del hover */
  left: 20%;
  animation: move 3s infinite;
}

/* Tiras inferiores/derechas */
.boton-accion-nuevo i::after {
  content: "";
  position: absolute;
  width: 8px; /* Reducción de ancho */
  height: 2px;
  left: 20%;
  bottom: -2px;
  border: 2px solid var(--clr);
  background: var(--btn-inner-bg);
  transition: 0.2s;
}

.boton-accion-nuevo:hover i::after {
  width: 12px; /* Ajuste del hover */
  left: 80%;
  animation: move 3s infinite;
}

/* Animaciones */
@keyframes move {
  0% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(5px);
  }
  100% {
    transform: translateX(0);
  }
}

@keyframes box {
  0% {
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
  }
  50% {
    box-shadow: 0 0 20px var(--clr); /* Ligeramente menos intenso */
  }
  100% {
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
  }
}

.pulso-enlace {
    animation: pulso-btn-accion 2s infinite alternate ease-in-out;
}
@keyframes pulso-btn-accion {
    from {
        box-shadow: 0 0 5px var(--clr);
        transform: scale(1);
    }
    to {
        box-shadow: 0 0 10px var(--clr), 0 0 20px var(--clr); /* Menos intenso que antes */
        transform: scale(1.01);
    }
}


/* --- Botón de Cerrar [X] --- */
.boton-cerrar {
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    background-color: transparent;
    border: none;
    color: var(--vt-c-magenta);
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    transition: color 0.3s, transform 0.3s; 
    z-index: 20;
}
.boton-cerrar:hover {
    color: var(--vt-c-magenta);
    text-shadow: 0 0 5px var(--vt-c-magenta);
    transform: scale(1.1) rotate(5deg); 
}

/* ============================================== */
/* 4. Efecto de Partículas (REVISIÓN CSS) */
/* ============================================== */

/* CORRECCIÓN CLAVE: El contenedor debe tener la propiedad 'position: relative' 
   o 'absolute' para que las partículas, que son 'position: absolute',
   se posicionen correctamente respecto a él. Se usa 'absolute' para cubrir todo. */
#contenedor-particulas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
  /* Z-index alto para que esté siempre por encima de otros elementos, pero debajo de la modal. */
  z-index: 5; 
}

.particula-estrella {
  position: absolute;
  transform: rotate(45deg); 
  border-radius: 1px;
  opacity: 1; 
  /* Efecto de brillo para que se vean bien */
  filter: drop-shadow(0 0 1px var(--particula-color)) drop-shadow(0 0 4px var(--particula-color)); 
  animation: star-fly 1s forwards ease-out;
}

@keyframes star-fly {
  0% {
    opacity: 1;
    /* Usamos translate en lugar de left/top para animar */
    transform: translate(0, 0) rotate(45deg) scale(1); 
  }
  20% {
    opacity: 1;
  }
  100% {
    /* Mueve la partícula a la posición de dispersión */
    transform: translate(var(--dx), var(--dy)) rotate(45deg) scale(0);
    opacity: 0; 
  }
}


/* ============================================== */
/* 5. RESPONSIVE MÓVIL/TABLET */
/* ============================================== */
@media (max-width: 768px) {
  .contenedor-educacion-cursos {
    padding: 1rem;
  }
  
  .contenedor-interactivo-tiempo {
    flex-direction: column;
    gap: 1.5rem;
  }
  
  /* MODO MÓVIL para los botones de año */
  .contenedor-anos-tiempo {
    flex-direction: column; 
    align-items: center;    
    border-bottom: none;    
    padding-bottom: 0;
  }
  
  .boton-ano {
    width: 80%; 
    max-width: 300px;
    font-size: 1rem;
    position: relative; 
    margin-bottom: 0.5rem; 
    transform: none !important;
  }
  .boton-ano:hover {
      transform: scale(1.03) !important;
  }
  
  .boton-ano::after {
    content: none; 
  }

  .contenedor-contenido {
    padding: 1rem;
    width: 95%; 
    max-width: none;
  }
  
  .tarjeta-bienvenida {
    display: block; 
  }
  
  .titulo {
    font-size: 1.2rem;
  }
  
  .descripcion {
    font-size: 0.9rem;
  }
  
  /* Ajuste de botón en móvil */
  .boton-accion-nuevo {
    width: 90%; 
    max-width: 250px;
    padding: 0.6rem 1rem; 
    font-size: 0.75rem; 
    letter-spacing: 0.05rem;
  }
  .boton-accion-nuevo:hover, .boton-accion-nuevo:focus {
    padding: 0.7rem 1.1rem;
    letter-spacing: 0.1rem;
  }
}
</style>
