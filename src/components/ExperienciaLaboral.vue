<template>
  <section id="experiencia" class="contenedor-experiencia">
    <h2 class="titulo-seccion">Experiencia Laboral</h2>

    <div class="contenedor-interactivo-tiempo">
      <div class="contenedor-anos-experiencia">
        <div
          v-for="(item, indice) in experiencia"
          :key="indice"
          :class="['boton-etapa', { activo: indiceActivo === indice }]"
          :style="{ '--accent-color': coloresItems[indice % coloresItems.length].color }"
          @click="alternarActivo(indice)"
        >
          <span :class="{ 'pulso-neon': indiceActivo === -1 }">
            {{ item.empresa }}
          </span>
        </div>
      </div>

      <div class="contenedor-muestra-contenido-experiencia">
        <div v-if="indiceActivo === -1" class="contenedor-contenido tarjeta-bienvenida">
          <div class="grupo-titulo-muestra">
            <h3 class="titulo titulo-bienvenida">Trayectoria Profesional</h3>
          </div>
          <div class="descripcion">Explora mi trayectoria Laboral.</div>
          <button
            class="boton-accion-nuevo pulso-enlace"
            :style="{ '--clr': coloresItems[0].color }"
            @click="manejarClicEmpezar($event)"
          >
            <span>Ver mi último rol</span>
            <i></i>
            <i></i>
            <i></i>
            <i></i>
          </button>
        </div>

        <div
          v-else-if="itemActivo"
          :key="itemActivo.empresa"
          class="contenedor-contenido tarjeta-contenido-activa"
          :style="{ '--accent-color': coloresItems[indiceActivo % coloresItems.length].color }"
        >
          <button
            class="boton-cerrar"
            @click="
              generarParticulas($event);
              establecerActivo(-1);
            "
          >
            [ X ]
          </button>

          <div class="grupo-titulo-muestra">
            <h3 class="titulo">{{ itemActivo.rol }}</h3>
            <span class="subtitulo-detalle"
              >@ {{ itemActivo.empresa }} ({{ itemActivo.fecha }})</span
            >
          </div>

          <div class="descripcion-scrollable">
            <p>{{ itemActivo.resumen }}</p>
            <ul class="lista-logros">
              <li v-for="(logro, i) in itemActivo.logros" :key="i">
                <span
                  class="logro-bullet"
                  :style="{ color: coloresItems[indiceActivo % coloresItems.length].color }"
                  >◆</span
                >
                {{ logro }}
              </li>
            </ul>
          </div>
          <a
            class="boton-accion-nuevo enlace-accion"
            :href="itemActivo.enlace"
            target="_blank"
            :style="{ '--clr': coloresItems[indiceActivo % coloresItems.length].color }"
            @click="generarParticulas($event)"
          >
            <span>{{ itemActivo.enlaceTexto }}</span>
            <i></i>
            <i></i>
            <i></i>
            <i></i>
          </a>
        </div>
      </div>
    </div>

    <div class="contenedor-btn-regreso-neon">
      <button
        @click="volverArriba"
        class="btn-flotante-volver-arriba"
        aria-label="Volver a Navegación Principal"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="2"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M4.5 10.5 12 3m0 0 7.5 7.5M12 3v18"
          />
        </svg>
      </button>
    </div>
    <div id="contenedor-particulas-experiencia" ref="contenedorParticulas"></div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";

// FUNCIÓN AGREGADA: Volver Arriba
const volverArriba = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};
//--Logica seleccion experiencia Laboral--
const indiceActivo = ref(-1);

// Funcion para manejar la seleccion de experiencaia
const establecerActivo = (indice) => {
  indiceActivo.value = indice;
};
// Alternar la seleccion activa/inactiva
const alternarActivo = (indice) => {
  if (indiceActivo.value === indice) {
    indiceActivo.value = -1;
  } else {
    indiceActivo.value = indice;
  }
};
// Manejar el click en el boton de empezar
const manejarClicEmpezar = (evento) => {
  generarParticulas(evento);
  alternarActivo(0);
};

// --- Lógica del Efecto de Partículas (Mantenida) ---
const contenedorParticulas = ref(null);
// Funcion para generar particulas en la posicion del click
const generarParticulas = (evento) => {
  const contenedor = contenedorParticulas.value;
  if (!contenedor) return;

  const rectBoton = evento.currentTarget.getBoundingClientRect(); // Obtener la posicion del boton clickeado
  const clickX = rectBoton.left + rectBoton.width / 2; // Centro del boton
  const clickY = rectBoton.top + rectBoton.height / 2; // Centro del boton
  const rectContenedor = contenedor.getBoundingClientRect();
  // Posición relativa al contenedor de partículas (que es la sección completa)
  const centroX = clickX - rectContenedor.left;
  const centroY = clickY - rectContenedor.top;

  const cantidadParticulas = Math.floor(Math.random() * 8) + 8; // Entre 8 y 15 partículas
  const colores = [
    coloresItems.value[0].color, // Usar los colores definidos
    coloresItems.value[1].color,
    coloresItems.value[2].color,
  ];
  // Generar las partículas
  for (let i = 0; i < cantidadParticulas; i++) {
    const particula = document.createElement("span"); // Crear el elemento de la partícula
    particula.className = "particula-estrella-exp"; // Clase para estilos y animaciones
    particula.style.left = `${centroX}px`; // Posición inicial
    particula.style.top = `${centroY}px`; // Posición inicial

    const tamano = Math.random() * 3 + 1; // Tamaño aleatorio entre 1 y 4px
    particula.style.width = `${tamano}px`; // Tamaño aleatorio entre 1 y 4px
    particula.style.height = `${tamano}px`; // Tamaño aleatorio entre 1 y 4px

    const colorParticula = colores[Math.floor(Math.random() * colores.length)];
    particula.style.backgroundColor = colorParticula;
    particula.style.setProperty("--particula-color", colorParticula); // Color de la partícula

    const dx = (Math.random() - 0.5) * 150; // Movimiento aleatorio en X
    const dy = (Math.random() - 0.5) * 150; // Movimiento aleatorio en Y
    const duracion = Math.random() * 0.8 + 0.4; // Duración aleatoria entre 0.4 y 1.2 segundos

    particula.style.setProperty("--dx", `${dx}px`); // Variables CSS para la animación
    particula.style.setProperty("--dy", `${dy}px`); //
    particula.style.animationDuration = `${duracion}s`; // Duración de la animación

    contenedor.appendChild(particula); // Añadir la partícula al contenedor
    particula.onanimationend = () => {
      particula.remove(); // Eliminar la partícula después de la animación
    };
  }
};

// --- Datos de Experiencia (Variables de Color Neón Corregidas) ---
const coloresItems = ref([
  { color: "var(--vt-c-neon-green)" }, // Usa el nombre global para el Verde Neón

  { color: "var(--vt-c-magenta-pure)" }, // Usa el nombre global para el Magenta puro

  { color: "var(--color-neon-cyan)" }, // Usa el alias global para el Cian

  { color: "var(--vt-c-indigo)" }, // Usa el nombre global para el Morado/Índigo
]);

// Datos de experiencia laboral
const experiencia = ref([
  {
    fecha: "2025 - Actualidad",
    empresa: "Bytenine Team ",
    rol: "Colaborador & Desarrollador Frontend/Backend Jr.",
    resumen:
      "Participación activa en un entorno de aprendizaje y desarrollo colaborativo. Este es un texto largo de prueba para simular que hay mucho contenido y el scroll interno debería activarse, permitiendo al usuario leer toda la descripción de mi rol y logros sin que la página principal tenga que desplazarse. Esto asegura una experiencia de usuario fluida y coherente con el diseño de pantalla completa.",
    logros: [
      "Diseño y prototipado de interfaces web creativas utilizando JavaScript/Vue(detalles estéticos y UX)prestando especial atención a la estética y la experiencia del usuario (UX).",
      "Implementación de lógica backend básica con Java/Python para manejar la persistencia de datos de proyectos internos.",
      "Optimización de la carga inicial de componentes de Vue en un 30%",
    ],
    enlace: "https://github.com/HotCode2025/ByteNine-Segundo-Semestre",
    enlaceTexto: "Ver Proyectos del Equipo",
  },
  {
    fecha: "2022 - 2024",
    empresa: "Winery Optimizers ",
    rol: "Asistente de Optimización de Procesos (IT Focus)",
    resumen:
      "Rol de transición, transformando la experiencia en administración/contable en un enfoque IT. Dedicado a la identificación de ineficiencias y la propuesta de soluciones basadas en el análisis de datos para mejorar los flujos de trabajo. **Perfeccionista en la búsqueda de la eficiencia.** Aquí también colocamos texto adicional largo para probar el scroll interno y confirmar que funciona correctamente en esta segunda entrada, manteniendo la coherencia visual.",
    logros: [
      "Implementación de macros y scripts en JavaScript para optimizar la entrada y validación de datos contables",
      "Creación de material de capacitación detallado para el personal sobre las nuevas herramientas digitales, asegurando una adopción fluida.",
    ],
    enlace: "https://www.instagram.com/losnoqueswines/?hl=es",
    enlaceTexto: "Análisis de Datos",
  },
  {
    fecha: "2020 - 2022",
    empresa: "Digital Art Studio ",
    rol: "Asistente de Producción Creativa",
    resumen:
      "Apoyo en la gestión de activos digitales y la optimización de flujos de trabajo creativos.",
    logros: [
      "Desarrollo de pequeños programas en Python para automatizar el cambio de formato y tamaño de imágenes",
      "Gestión de repositorios y control de versiones para los activos creativos, facilitando la colaboración en equipo.",
    ],
    enlace:
      "https://es.wix.com/blog/portafolios-de-arte-y-diseno?utm_source=google&utm_medium=cpc&utm_campaign=23166757638^188414526698^search%20-%20dsa&experiment_id=^^7",
    enlaceTexto: "Ver Portafolio de Arte",
  },
]);

// Computed para obtener el item activo basado en el índice seleccionado
const itemActivo = computed(() =>
  indiceActivo.value !== -1 ? experiencia.value[indiceActivo.value] : null
);
</script>

<style scoped>
/*----- VARIABLES Y ANIMACIONES  ----- */

@keyframes neon-pulse {
  from {
    text-shadow: 0 0 5px var(--vt-c-white), 0 0 10px var(--vt-c-white);
    opacity: 0.8;
  }
  to {
    /* Pulso Neón con Índigo/Morado para la sección */
    text-shadow: 0 0 15px var(--vt-c-indigo), 0 0 25px var(--vt-c-indigo);
    opacity: 1;
  }
}
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@keyframes active-neon-pulse {
  0% {
    box-shadow: 0 0 5px var(--accent-color), 0 0 15px var(--accent-color);
  }
  50% {
    box-shadow: 0 0 15px var(--accent-color), 0 0 30px var(--accent-color);
  }
  100% {
    box-shadow: 0 0 5px var(--accent-color), 0 0 15px var(--accent-color);
  }
}
/* Animación de Borde para el Botón de Acción */
@keyframes move {
  0% {
    transform: translate(0);
  }
  100% {
    transform: translate(100%);
  }
}
@keyframes pulso-enlace-anim {
  0% {
    filter: brightness(1) drop-shadow(0 0 5px var(--clr));
  }
  50% {
    filter: brightness(1.2) drop-shadow(0 0 10px var(--clr));
  }
  100% {
    filter: brightness(1) drop-shadow(0 0 5px var(--clr));
  }
}
@keyframes star-fly-exp {
  0% {
    opacity: 1;
    transform: translate(0, 0) rotate(45deg) scale(1);
  }
  20% {
    opacity: 1;
  }
  100% {
    transform: translate(var(--dx), var(--dy)) rotate(45deg) scale(0);
    opacity: 0;
  }
}

/* ------------------------------------------------------------- */
/* LAYOUT PRINCIPAL */
/* ------------------------------------------------------------- */
/* Contenedor Principal de la Sección */
.contenedor-experiencia {
  padding: 0; /* Eliminado el padding para maximizar el uso del espacio */
  max-width: 100%; /* Asegura que el contenedor use todo el ancho disponible */
  position: relative; /* Para posicionar elementos absolutos dentro */
  text-align: center; /* Centrar el texto */
  min-height: 90vh; /* Asegura que la sección ocupe al menos el 90% de la altura de la ventana */
  max-height: 100dvh; /* Asegura que no exceda la altura de la ventana en dispositivos móviles */
  height: auto; /* Altura automática para adaptarse al contenido */
  display: flex; /* Flexbox para centrar contenido */
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

/* Título de la Sección */
.titulo-seccion {
  position: relative;
  margin-bottom: 2rem;
  font-family: "Orbitron", sans-serif;
  font-size: 2.8rem;
  font-weight: 700;
  color: var(--vt-c-white); /* Uso de variable global */
  text-shadow: 0 0 10px var(--vt-c-white), 0 0 25px var(--vt-c-white),
    /* Uso de RGBA del verde (Mantenido) */ 0 0 40px rgba(57, 255, 20, 0.8);
  padding-bottom: 0.5rem;
  /* Uso de RGBA del índigo (Mantenido) */
  border-bottom: 2px solid rgba(168, 85, 247, 0.2);
  z-index: 10;
}

/* Contenedor Interactivo de Tiempo y Contenido */
.contenedor-interactivo-tiempo {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  max-height: 85vh; /* Limita la altura para evitar overflow */
  width: 95%; /* Uso del 95% del ancho disponible */
  max-width: 1400px; /* Limita el ancho máximo */
  margin-top: 6rem; /* Separación del título */
  z-index: 20; /* Asegura que esté por encima del fondo */
}

/* ------------------------------------------------------------- */
/* BOTONES DE ACCIÓN ( Animación de borde) */
/* ------------------------------------------------------------- */
.boton-accion-nuevo {
  /* La variable --clr se inyecta desde Vue */
  --inner-bg: var(--vt-c-black-soft); /* Uso de variable global */

  position: relative;
  display: inline-block;
  padding: 0.8rem 2.5rem; /* Aumentado para mejor usabilidad */
  margin-top: 1.5rem; /* Separación del contenido */
  color: var(--clr); /* La variable --clr se inyecta desde Vue */
  text-decoration: none; /* Elimina el subrayado predeterminado */
  font-size: 1rem; /* Aumentado para mejor legibilidad */
  font-family: "Orbitron", sans-serif;
  letter-spacing: 0.05em;
  text-transform: uppercase; /* Texto en mayúsculas */
  overflow: hidden; /* Oculta los bordes animados fuera del botón */
  transition: 0.5s; /* Transición suave para hover */
  background: var(--inner-bg);
  border: none;
  cursor: pointer;
  border-radius: 5px; /* Bordes redondeados */
  z-index: 10; /* Asegura que el contenido esté por encima de los bordes */
}
/* Contenido del botón */
.boton-accion-nuevo span {
  position: relative;
  z-index: 1; /* Asegura que el texto esté por encima de los bordes */
}

.pulso-enlace {
  /* Clase para aplicar la animación de pulso */
  animation: pulso-enlace-anim 2s infinite ease-in-out; /* Animación de pulso */
}
/* Bordes animados */
.boton-accion-nuevo:hover {
  /* Efecto hover para el botón */
  color: var(--vt-c-black-soft); /* Uso de variable global */
  background: var(--clr);
  box-shadow: 0 0 10px var(--clr), 0 0 25px var(--clr), 0 0 50px var(--clr);
}

/* Elementos de borde animados */
.boton-accion-nuevo i {
  position: absolute;
  display: block;
}
/* Las animaciones de borde (i:nth-child) usan la variable --clr, que es correcta. */

/* ------------------------------------------------------------- */
/* PANEL DE SELECCIÓN (LISTA DE EMPRESAS) */
/* ------------------------------------------------------------- */

/* Contenedor de Años de Experiencia (Botones) */
.contenedor-anos-experiencia {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 90%; /* Uso del 90% del ancho disponible */
  max-width: 350px; /* Limita el ancho máximo */
  padding: 0;
  border-left: none;
}

/* Botones de Etapas (Empresas) */
.boton-etapa {
  font-family: "Orbitron", sans-serif;
  font-weight: 700; /* Aumentado para mejor legibilidad */
  font-size: 1.2rem;
  color: var(--vt-c-white); /* Uso de variable global */
  padding: 0.8rem 1rem; /* Aumentado para mejor usabilidad */
  cursor: pointer; /* Cambia el cursor al pasar por encima */
  transition: all 0.3s ease; /* Transición suave para hover y activo */
  position: relative;

  background: var(--vt-c-black-soft); /* Uso de variable global */
  border: 2px solid var(--accent-color);
  border-radius: 8px;
  /* Uso de RGBA del índigo (Mantenido) */
  box-shadow: 0 0 5px rgba(168, 85, 247, 0.5);

  text-align: center;
  margin-left: 0;
}

/* Efecto Hover y Activo */
.boton-etapa:hover {
  transform: scale(1.03);
  background-color: var(--accent-color);
  color: var(--vt-c-black); /* Uso de variable global */
  box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
}

.boton-etapa.activo {
  /* Estilo para el botón activo */
  transform: scale(1.02); /* Ligero escalado para indicar selección */
  background-color: var(--accent-color);
  color: var(--vt-c-black); /* Uso de variable global */
  box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
  animation: active-neon-pulse 2s infinite ease-in-out; /* Animación de pulso neón */
}
.boton-etapa::before {
  /* Elimina cualquier contenido antes del botón */
  content: none;
}

/* ------------------------------------------------------------- */
/* TARJETA DE CONTENIDO: SCROLL INTERNO Y GEOMETRÍA */
/* ------------------------------------------------------------- */

/* Contenedor del contenido de experiencia */
.contenedor-muestra-contenido-experiencia {
  display: flex;
  justify-content: center;
  padding-top: 1rem; /* Separación superior */
  width: 100%; /* Uso del 100% del ancho disponible */
}

.contenedor-contenido {
  background: var(--vt-c-black-soft); /* Uso de variable global */
  padding: 1.5rem;
  border-radius: 20px;
  box-shadow: 0 0 10px var(--vt-c-magenta-pure), 0 0 20px var(--color-neon-cyan),
    inset 0 0 5px rgba(255, 255, 255, 0.1);
  border: 2px solid transparent;
  width: 100%;
  max-width: 900px;
  position: relative;
  animation: fadeIn 0.5s ease-out;
  display: flex;
  flex-direction: column;
  align-items: center;

  min-height: 400px;
  height: 60vh;
  max-height: 550px;
}

.tarjeta-contenido-activa {
  /* El borde y la sombra se definen por la variable --accent-color inyectada en línea */
  border-color: var(--accent-color);
}

/* ------------------------------------------------------------- */
/* TARJETA DE BIENVENIDA */
/* ------------------------------------------------------------- */

.tarjeta-bienvenida {
  border: 2px solid transparent;
  /* Degradado de borde: Usa variables globales corregidas */
  background: linear-gradient(var(--vt-c-black-soft), var(--vt-c-black-soft)) padding-box,
    linear-gradient(
        90deg,
        var(--vt-c-magenta-pure) 0%,
        /* Magenta Puro */ var(--color-neon-cyan) 50%,
        /* Cian Neón */ var(--vt-c-indigo) 100% /* Índigo/Morado */
      )
      border-box;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(255, 68, 204, 0.2),
    /*Uso de la variable global Magenta */ 0 0 20px rgba(0, 255, 255, 0.2),
    /* Uso de RGBA del cian  */ inset 0 0 10px rgba(0, 0, 0, 0.5); /* Sombra interna para profundidad */

  justify-content: center;
  height: auto; /* Altura automática para adaptarse al contenido */
  min-height: auto;
  max-height: 350px; /* Limita la altura máxima */
  max-width: 600px;
  padding: 2rem;
}

.descripcion {
  padding-block: 1rem 1.5rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white-soft); /* Uso de variable global */
  line-height: 1.6;
  text-align: center;
}

/* Contenedor Scrollable: ¡SCROLL INTERNO HABILITADO! */
.descripcion-scrollable {
  flex-grow: 1;
  overflow-y: auto;
  padding-right: 10px;
  padding-bottom: 1rem;
  width: 100%;
  text-align: left;
  color: var(--vt-c-white); /* Uso de variable global */
  line-height: 1.6;

  /* Estilos de Scrollbar  */
  &::-webkit-scrollbar {
    width: 8px;
  }
  &::-webkit-scrollbar-track {
    background: var(--vt-c-black-soft); /* Uso de variable global */
    border-radius: 10px;
  }
  &::-webkit-scrollbar-thumb {
    background: var(--accent-color);
    border-radius: 10px;
    background-clip: padding-box; /* Asegura que el color se aplique solo al thumb */
    border: 2px solid var(--vt-c-black-soft); /* Uso de variable global */
  }
  &::-webkit-scrollbar-thumb:hover {
    background: var(--vt-c-white); /* Uso de variable global */
  }
}

.descripcion-scrollable p {
  padding-block: 0 1rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white); /* Uso de variable global */
  line-height: 1.6;
  text-align: left;
}
.subtitulo-detalle {
  font-size: 1rem;
  font-weight: 500;
  color: var(--vt-c-white-soft); /* Uso de variable global */
  display: block;
  margin-top: 0.2rem;
}

.lista-logros {
  list-style: none;
  padding-left: 0;
  margin-top: 1rem;
  text-align: left;
}

.lista-logros li {
  margin-bottom: 0.5rem;
  position: relative;
  padding-left: 1.5rem;
  font-size: 0.95rem;
}

.logro-bullet {
  position: absolute;
  left: 0;
  top: 0;
  font-weight: 900;
  font-size: 0.8em;
}

/* ------------------------------------------------------------- */
/* Botón Flotante (Volver Arriba) */
/* ------------------------------------------------------------- */
.contenedor-btn-regreso-neon {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 100;
}

.btn-flotante-volver-arriba {
  /*Se usa el verde neón como color de acento principal */
  --back-clr: var(--vt-c-neon-green);
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: var(--vt-c-black-soft); /* Uso de variable global */
  border: 2px solid var(--back-clr);
  color: var(--back-clr);
  cursor: pointer;
  box-shadow: 0 0 10px var(--back-clr);
  transition: all 0.3s ease-in-out;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn-flotante-volver-arriba:hover {
  background: var(--back-clr);
  color: var(--vt-c-black); /* Uso de variable global */
  box-shadow: 0 0 20px var(--back-clr), 0 0 40px var(--back-clr);
  transform: translateY(-3px) scale(1.05);
}

.btn-flotante-volver-arriba svg {
  width: 24px;
  height: 24px;
}

/* ------------------------------------------------------------- */
/* Contenedor de Partículas (Efecto de Explosión) */
/* ------------------------------------------------------------- */
#contenedor-particulas-experiencia {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
  z-index: 50; /* Por encima del contenido interactivo, pero debajo del botón flotante */
}

.particula-estrella-exp {
  position: absolute;
  transform-origin: center;
  border-radius: 50%;
  animation: star-fly-exp var(--duracion) ease-out forwards;
  box-shadow: 0 0 5px var(--particula-color), 0 0 10px var(--particula-color);
}

/* ------------------------------------------------------------- */
/* 6.--- RESPONSIVE MÓVIL/TABLET ----*/
/* ------------------------------------------------------------- */

@media (min-width: 900px) {
  /* Escritorio: Diseño de dos columnas */
  .contenedor-interactivo-tiempo {
    flex-direction: row;
    align-items: flex-start;
    justify-content: center;
    gap: 1.5rem;
  }
  .contenedor-anos-experiencia {
    min-width: 200px;
    width: 200px;
    flex-shrink: 0;
    margin-top: 0.5rem;
  }
  .contenedor-muestra-contenido-experiencia {
    width: 75%;
    max-width: 1000px;
    padding-left: 0;
  }
  .contenedor-contenido {
    height: 65vh;
    max-height: 580px;
  }
  .tarjeta-bienvenida {
    align-self: center;
    height: auto;
  }
}

@media (max-width: 899px) {
  /* Móvil: Diseño de una columna (apilado) - Mantenido */
  .contenedor-interactivo-tiempo {
    flex-direction: column;
    gap: 1.5rem;
    overflow-x: hidden;
  }

  .contenedor-anos-experiencia {
    width: 90%;
    max-width: 400px;
    align-items: center;
  }

  .boton-etapa {
    width: 100%;
    max-width: 350px;
  }

  .contenedor-muestra-contenido-experiencia {
    padding: 0;
    width: 90%;
  }

  .titulo {
    font-size: 1.4rem;
  }
  .descripcion-scrollable p,
  .descripcion-scrollable .lista-logros li {
    font-size: 0.9rem;
  }
}
</style>
