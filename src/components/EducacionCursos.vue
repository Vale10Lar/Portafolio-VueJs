<template>
  <!-- CONTENEDOR PRINCIPAL DE LA SECCION -->
  <!-- Se utiliza 'educacion' como ID para la navegación interna -->
  <section id="educacion" class="contenedor-educacion-cursos">
    <!-- TÍTULO PRINCIPAL DE LA SECCIÓN (Estilo Neón) -->
    <h2 class="titulo-seccion">Educación y Cursos</h2>

    <!-- Contenedor general para la interacción de la línea de tiempo -->
    <div class="contenedor-interactivo-tiempo">
      <!-- Contenedor de los botones de Años/Eventos (la "Línea de Tiempo") -->
      <div class="contenedor-anos-tiempo">
        <!-- Iteración sobre el array 'educacion' para crear los botones de la línea de tiempo -->
        <button
          v-for="(item, indice) in educacion"
          :key="indice"
          :class="['boton-ano', { activo: indiceActivo === indice }]"
          :style="{ '--accent-color': coloresItems[indice % coloresItems.length].color }"
          @click="alternarActivo(indice)"
          :aria-label="`Ver detalle de ${item.fecha}`"
          :aria-expanded="indiceActivo === indice"
        >
          <!-- Pulso Neón para indicar que la sección está inactiva y lista para interactuar -->
          <span :class="{ 'pulso-neon': indiceActivo === -1 }">
            {{ item.fecha }}
          </span>
        </button>
      </div>

      <!-- Contenedor para mostrar el Contenido Detallado de la selección -->
      <div class="contenedor-muestra-contenido-tiempo">
        <!-- Transición Vue: Utiliza el modo 'out-in' para que la tarjeta actual salga antes de que la nueva entre -->
        <Transition name="slide-fade" mode="out-in">
          <!-- TARJETA 1: Estado de Bienvenida (Se muestra cuando indiceActivo es -1) -->
          <div
            v-if="indiceActivo === -1"
            key="bienvenida"
            class="contenedor-contenido tarjeta-bienvenida"
          >
            <div class="grupo-titulo-muestra">
              <h3 class="titulo titulo-bienvenida">¡Bienvenido!</h3>
            </div>

            <div class="descripcion">Explora mi trayectoria académica.</div>

            <!-- Botón de Acción para Iniciar la Línea de Tiempo -->
            <button
              class="boton-accion-nuevo pulso-enlace"
              :style="{ '--clr': coloresItems[0].color }"
              @click="alternarActivo(0)"
            >
              <span>Empezar {{ educacion[0].fecha }}</span>
              <i></i>
            </button>
          </div>

          <!-- TARJETA 2: Contenido Activo (Se muestra cuando se selecciona un ítem) -->
          <div
            v-else-if="itemActivo"
            :key="itemActivo.fecha"
            class="contenedor-contenido tarjeta-contenido-activa"
            :style="{ '--accent-color': coloresItems[indiceActivo % coloresItems.length].color }"
          >
            <!-- Botón para cerrar la tarjeta y volver al estado de bienvenida (-1) -->
            <button class="boton-cerrar" @click="establecerActivo(-1)" aria-label="Cerrar detalle">
              [ X ]
            </button>

            <div class="grupo-titulo-muestra">
              <h3 class="titulo">{{ itemActivo.title }}</h3>
              <!-- Fecha solo visible en dispositivos móviles -->
              <span class="fecha-muestra-movil">{{ itemActivo.fecha }}</span>
            </div>

            <!-- Descripción del curso/logro -->
            <div class="descripcion">{{ itemActivo.descripcion }}</div>

            <!-- Enlace de Acción con estilo Neón Dinámico -->
            <a
              class="boton-accion-nuevo enlace-accion"
              :href="itemActivo.enlace"
              target="_blank"
              :style="{ '--clr': coloresItems[indiceActivo % coloresItems.length].color }"
              @click="establecerActivo(-1)"
            >
              <span>
                {{ itemActivo.enlaceTexto }}
              </span>
              <i></i>
            </a>
          </div>
        </Transition>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";

// ==============================================
// 1. Estado Reactivo
// ==============================================

// Estado que guarda el índice del elemento de educación activo.
// -1: Estado inicial o de bienvenida.
const indiceActivo = ref(-1);

// ==============================================
// 2. Funciones de Lógica
// ==============================================

// Establece un índice específico como activo.
const establecerActivo = (indice) => {
  indiceActivo.value = indice;
};

// Alterna el estado activo: si está activo, lo desactiva (-1); si está inactivo, lo activa.
const alternarActivo = (indice) => {
  if (indiceActivo.value === indice) {
    indiceActivo.value = -1; // Desactivar
  } else {
    indiceActivo.value = indice; // Activar
  }
};

// ==============================================
// 3. Datos y Constantes
// ==============================================

// Array de colores para dar un acento visual diferente a cada ítem.
const coloresItems = ref([
  { color: "var(--vt-c-neon-green)" }, // Verde Neón
  { color: "var(--color-neon-cyan)" }, // Cian Neón
  { color: "var(--vt-c-magenta-pure)" }, // Magenta Puro
  { color: "var(--vt-c-indigo)" }, // Índigo Neón (Púrpura)
]);

// Datos de la trayectoria académica y cursos.
const educacion = ref([
  {
    fecha: "2025",
    title: "Técnico Universitario en Programación ",
    descripcion:
      "Foco en desarrollo de software, análisis de sistemas, y metodologías ágiles. Preparación para soluciones Full Stack.",
    enlace: "https://utn.edu.ar/frsr/",
    enlaceTexto: "Título",
  },
  {
    fecha: "2024",
    title: "Manejo Avanzado de Bases de Datos ",
    descripcion:
      "Estudio intensivo en consultas complejas, optimización de rendimiento y diseño de esquemas relacionales.",
    enlace: "https://ejemplo.com/certificado-sql",
    enlaceTexto: " Certificado ",
  },
  {
    fecha: "2023",
    title: "Fundamentos de JavaScript y Backend",
    descripcion:
      "Adquisición de bases de programación y lógica del lado del servidor para el desarrollo de APIs web.",
    enlace: "https://ejemplo.com/certificado-js",
    enlaceTexto: "Certificado ",
  },
  {
    fecha: "2022",
    title: "Introducción a Sistemas Operativos",
    descripcion:
      "Comprensión de la arquitectura, gestión de memoria y procesos en sistemas operativos como Linux y Windows.",
    enlace: "#",
    enlaceTexto: " Certificado",
  },
]);

// ==============================================
// 4. Propiedad Computada
// ==============================================

// Obtiene el objeto de educación completo del ítem activo. Retorna 'null' si está en estado de bienvenida (-1).
const itemActivo = computed(() =>
  indiceActivo.value !== -1 ? educacion.value[indiceActivo.value] : null
);
</script>

<style scoped>
/* ============================================== */
/* ESTILOS (CSS) - INICIO */
/* ============================================== */

/* ============================================== */
/* 1. Definición de Variables Locales (Derivadas de las Globales) */
/* ============================================== */
.contenedor-educacion-cursos {
  /* Variables RGBA locales creadas a partir de las variables globales para opacidades específicas */
  --rgba-indigo-05: rgba(168, 85, 247, 0.5); /* Sombra de botones */
  --rgba-indigo-02: rgba(168, 85, 247, 0.2);
  --rgba-cyan-02: rgba(0, 255, 255, 0.2);
  --rgba-magenta-02: rgba(255, 0, 255, 0.2);
  --rgba-magenta-07: rgba(255, 0, 255, 0.7);
  --rgba-green-07: rgba(57, 255, 20, 0.7);
  --bgColor: var(--vt-c-black-soft); /* Color de fondo oscuro consistente */

  /* Degradado para el borde de la tarjeta de bienvenida (Neón Múltiple) */
  --welcome-gradient-border: linear-gradient(
    90deg,
    var(--vt-c-indigo) 0%,
    var(--color-neon-cyan) 50%,
    var(--vt-c-magenta-pure) 100%
  );
  --welcome-title-color: var(--vt-c-indigo); /* Color del título de bienvenida */
  --btn-inner-bg: #272822; /* Fondo interno del botón de acción */
}

/* ============================================== */
/* 2. ESTILOS GENERALES DEL COMPONENTE Y TÍTULO */
/* ============================================== */

/* Contenedor Principal (Flexbox, Centrado, Altura Mínima) */
.contenedor-educacion-cursos {
  padding: 0.5 rem 1rem 1rem 1rem; /*espaciado interno(superior, iferior, izquierda derecha)*/
  max-width: auto; /*ancho maximo automatico*/
  margin: 0 auto; /*centrado horizontal */
  display: flex; /*flexbox para alineacion*/
  flex-direction: column; /*direccion de los elementos de la columna*/
  position: relative;
  box-sizing: border-box;
  text-align: center;
  min-height: 100vh; /* Asegura ocupar la altura total del viewport */
  justify-content: flex-start;
  align-items: center;
  background-color: var(--color-theme-base);
}

/* Título de la Sección (Estilo Neón Brillante) */
.titulo-seccion {
  position: relative; /*para efectos de posicionamiento*/
  margin-top: 0; /*sin margen superior*/
  margin-bottom: 4rem; /* margen inferior para separar el titulo del contenido*/
  font-family: "Orbitron", sans-serif;
  font-size: 2.8rem;
  font-weight: 700;
  color: var(--vt-c-white);
  /* Sombra de texto para el efecto neón (Blanco, y acento Verde) */
  text-shadow: 0 0 10px var(--vt-c-white), 0 0 25px var(--vt-c-white),
    0 0 40px rgba(57, 255, 20, 0.8);
  padding-bottom: 0.5rem;
  border-bottom: 2px solid rgba(168, 85, 247, 0.2); /* Línea decorativa */
  z-index: 10;
}

/* ============================================== */
/* 3. Estilos de la Línea de Tiempo (Años y Botones) */
/* ============================================== */

/* Contenedor Interactivo (Permite desplazamiento horizontal si es necesario) */
.contenedor-interactivo-tiempo {
  overflow-x: auto;
  padding-bottom: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center; /* Se ajusta a center para mejor consistencia */
  width: 100%; /* Ocupa el 100% del padre para gestionar el scroll */
}

/* Contenedor de los Botones de Años */
.contenedor-anos-tiempo {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: space-between;
  /* Ancho mínimo que fuerza el scroll en pantallas pequeñas, creando la línea de tiempo */
  min-width: 1000px;
  width: 100%;
  align-items: center;
  padding-bottom: 1rem;
  /* Línea horizontal que representa la línea de tiempo base */
  border-bottom: 3px dashed var(--color-neon-cyan);
}

/* Botón de Año (Estilo Caja con Borde Neón y Sombra) */
.boton-ano {
  font-family: "Orbitron", sans-serif;
  font-weight: 700;
  font-size: 1.2rem;
  margin: 0 0.5rem;
  color: var(--vt-c-white);
  padding: 0.5rem 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  background: var(--bgColor);
  border: 2px dashed var(--vt-c-magenta-pure);
  box-shadow: 0 0 10px var(--rgba-indigo-05); /* Sombra inicial sutil */
  flex-shrink: 0; /* Evita que los botones se achiquen */
}

/* Animación de Pulso para el texto (Estado inicial, cuando ninguno está activo) */
.pulso-neon {
  display: inline-block;
  animation: neon-pulse 1.5s infinite alternate ease-in-out;
}
/*animaciones*/
@keyframes neon-pulse {
  from {
    text-shadow: 0 0 5px var(--vt-c-white), 0 0 10px var(--vt-c-white);
    opacity: 0.8;
  }
  to {
    text-shadow: 0 0 15px var(--color-neon-cyan), 0 0 25px var(--color-neon-cyan);
    opacity: 1;
  }
}

/* Estilos para el estado :hover y .activo */
.boton-ano:hover,
.boton-ano.activo {
  color: var(--vt-c-black);
  transform: translateY(-5px); /* Efecto de elevación */
  background-color: var(--accent-color); /* Color de fondo dinámico */
  /* Sombra Neón dinámica, basada en el color de acento */
  box-shadow: 0 0 5px var(--accent-color), 0 0 15px var(--accent-color),
    0 5px 20px rgba(0, 0, 0, 0.8);
}

/* Se detiene la animación de pulso cuando un botón está activo */
.boton-ano.activo .pulso-neon {
  animation: none;
}

/* Indicador de Punto en la Línea de Tiempo (Círculo con Rotación) */
.boton-ano::after {
  content: "";
  position: absolute;
  bottom: -1.7rem; /* Se posiciona debajo de la línea */
  left: 50%;
  transform: translateX(-50%);
  width: 1rem;
  height: 1rem;
  background: var(--bgColor);
  border: 0.25rem solid var(--accent-color); /* Borde con color de acento dinámico */
  border-radius: 3px;
  box-shadow: 0 0 8px var(--accent-color);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  animation: rotate 3s linear infinite; /* Animación de rotación continua */
}

@keyframes rotate {
  from {
    transform: translateX(-50%) rotate(0deg);
  }
  to {
    transform: translateX(-50%) rotate(360deg);
  }
}

/* Efecto de pulso en el indicador cuando el botón está activo */
.boton-ano.activo::after {
  box-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color);
}

/* ============================================== */
/* 4. Contenedor de Muestra de Contenido (Tarjetas) */
/* ============================================== */

.contenedor-muestra-contenido-tiempo {
  display: flex;
  justify-content: center;
  padding-top: 1rem;
  width: 100%;
  position: relative;
  min-height: 250px; /* Altura mínima consistente */
}

/* Animaciones de Transición Vue (Deslizamiento y Desvanecimiento) */
.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all 0.4s cubic-bezier(0.19, 1, 0.22, 1);
}

.slide-fade-enter-from {
  opacity: 0;
  transform: translateX(20px);
}

.slide-fade-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}

/* Ajuste de posición para la transición para evitar que el layout salte */
.contenedor-muestra-contenido-tiempo > .slide-fade-leave-active {
  position: absolute;
  top: 1rem;
  width: 90%;
  max-width: 600px;
}

/* Estilos base de la Tarjeta de Contenido */
.contenedor-contenido {
  background: var(--bgColor);
  padding: 1.5rem;
  border-radius: 20px;
  border: 2px solid var(--vt-c-indigo); /* Borde por defecto */
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.9), inset 0 0 5px var(--vt-c-indigo);
  width: 90%;
  max-width: 600px;
  position: relative;
}

/* Estilo para la Tarjeta de Contenido Activo (Borde y Sombra Neón Dinámica) */
.tarjeta-contenido-activa {
  border: 2px dashed var(--accent-color);
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
    /* Uso de RGBA del magenta */ 0 0 20px rgba(0, 255, 255, 0.2),
    /* Uso de RGBA del cian (Mantenido) */ inset 0 0 10px rgba(0, 0, 0, 0.5);
}

/* Tarjeta de Bienvenida (Borde de Degradado Múltiple) */
.tarjeta-bienvenida {
  border: 2px solid transparent;
  padding: 2px;
  /* El truco del 'border-box' para crear un borde de degradado */
  background: linear-gradient(var(--bgColor), var(--bgColor)) padding-box,
    var(--welcome-gradient-border) border-box;
  border-radius: 10px;
  box-shadow: 0 0 10px var(--vt-c-magenta-pure), 0 0 20px var(--color-neon-cyan),
    inset 0 0 5px rgba(255, 255, 255, 0.1);
}

/* Estilos internos de la tarjeta de bienvenida (para cubrir el degradado) */
.tarjeta-bienvenida > .grupo-titulo-muestra,
.tarjeta-bienvenida > .descripcion {
  background: var(--bgColor);
  border-radius: 8px;
}

.tarjeta-bienvenida > .grupo-titulo-muestra {
  padding-top: 0.5rem;
}

.tarjeta-bienvenida > .descripcion {
  padding-block: 1rem 1.5rem;
}

.titulo-bienvenida {
  color: var(--welcome-title-color); /* Índigo */
}

.grupo-titulo-muestra {
  padding-bottom: 0.5rem;
  border-bottom: 1px dashed var(--vt-c-black-mute);
}

/* Título del Ítem Activo (Color Cian Neón) */
.titulo {
  font-weight: 700;
  font-size: 1.5rem;
  color: var(--color-neon-cyan);
  font-family: "Orbitron", sans-serif;
  text-align: center;
  filter: brightness(0.85);
}

/* Ocultar fecha en desktop, solo se usa en responsive */
.fecha-muestra-movil {
  display: none;
}

/* Estilo de la Descripción */
.descripcion {
  padding-block: 1rem 1.5rem;
  font-weight: 400;
  font-size: 1rem;
  color: var(--vt-c-white);
  line-height: 1.4;
  text-align: center;
}

/* ============================================== */
/* 5. Estilos del Botón de Acción (Efecto Cyberpunk/Neón) */
/* ============================================== */

.boton-accion-nuevo {
  position: relative;
  background: var(--btn-inner-bg);
  color: #fff;
  text-decoration: none;
  text-transform: uppercase;
  border: none;
  letter-spacing: 0.05rem;
  font-size: 0.9rem;
  padding: 0.7rem 2rem;
  transition: 0.2s;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 60%;
  max-width: 200px;
  margin: 0.8rem auto 0;
  border-radius: 0;
  font-family: "Orbitron", sans-serif;
}

/* Efecto Hover/Focus (Aumento de tamaño y cambio de fondo) */
.boton-accion-nuevo:hover,
.boton-accion-nuevo:focus {
  letter-spacing: 0.1rem;
  padding: 0.8rem 2.1rem;
  background: var(--clr); /* Color de acento dinámico */
  color: var(--clr);
  animation: box 3s infinite; /* Animación de pulso de caja */
  outline: none;
}

/* Animación de Pulso de Sombra para el Botón (Basada en --clr) */
@keyframes box {
  0% {
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
  }
  50% {
    box-shadow: 0 0 20px var(--clr);
  }
  100% {
    box-shadow: 0 0 0 rgba(0, 0, 0, 0);
  }
}

/* Pulso Neón Adicional para el botón de Bienvenida */
.pulso-enlace {
  animation: pulso-btn-accion 2s infinite alternate ease-in-out;
}

@keyframes pulso-btn-accion {
  from {
    box-shadow: 0 0 5px var(--clr);
    transform: scale(1);
  }
  to {
    box-shadow: 0 0 10px var(--clr), 0 0 20px var(--clr);
    transform: scale(1.01);
  }
}

/* Capa interior que permite ver el borde neón de fondo en hover */
.boton-accion-nuevo::before {
  content: "";
  position: absolute;
  inset: 2px;
  background: var(--btn-inner-bg);
  transition: 0.2s;
  z-index: 0;
}

.boton-accion-nuevo:hover::before {
  background: #18191f; /* Fondo ligeramente diferente en hover */
}

.boton-accion-nuevo span {
  position: relative;
  z-index: 1; /* Asegura que el texto esté por encima de la capa interior */
}

/* Elementos decorativos (i) para las tiras */
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
  width: 8px;
  height: 2px;
  left: 80%;
  top: -2px;
  border: 2px solid var(--clr); /* Color de acento dinámico */
  background: var(--btn-inner-bg);
  transition: 0.2s;
}

/* Efecto Hover en las tiras superiores */
.boton-accion-nuevo:hover i::before {
  width: 12px;
  left: 20%;
  animation: move 3s infinite;
}

/* Tiras inferiores/derechas */
.boton-accion-nuevo i::after {
  content: "";
  position: absolute;
  width: 8px;
  height: 2px;
  left: 20%;
  bottom: -2px;
  border: 2px solid var(--clr); /* Color de acento dinámico */
  background: var(--btn-inner-bg);
  transition: 0.2s;
}

/* Efecto Hover en las tiras inferiores */
.boton-accion-nuevo:hover i::after {
  width: 12px;
  left: 80%;
  animation: move 3s infinite;
}

/* Animación de movimiento sutil de las tiras */
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

/* ============================================== */
/* 6. Botón de Cerrar [X] */
/* ============================================== */

.boton-cerrar {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background-color: transparent;
  border: none;
  color: var(--vt-c-magenta-pure); /* Magenta para cerrar/alerta */
  font-size: 1.2rem;
  font-weight: bold;
  cursor: pointer;
  transition: color 0.3s, transform 0.3s;
  z-index: 20;
}

.boton-cerrar:hover {
  color: var(--vt-c-magenta-pure);
  text-shadow: 0 0 5px var(--vt-c-magenta-pure);
  transform: scale(1.1) rotate(5deg);
}

/* ============================================== */
/* 7. RESPONSIVE MÓVIL/TABLET */
/* ============================================== */

@media (max-width: 768px) {
  .contenedor-educacion-cursos {
    padding: 0 1rem 1rem 1rem;
    /* Altura dinámica para mejor ajuste móvil */
    min-height: calc(100dvh - 2rem);
  }

  .titulo-seccion {
    font-size: 2rem;
    margin-bottom: 1.5rem;
  }

  /* El contenedor permite scroll horizontal en móviles */
  .contenedor-interactivo-tiempo {
    overflow-x: auto;
    gap: 1.5rem;
    padding-bottom: 0.5rem;
  }

  .contenedor-anos-tiempo {
    flex-direction: row;
    justify-content: flex-start; /* Fuerza el scroll horizontal */
    min-width: 550px;
    width: auto;
    padding-bottom: 1rem;
    border-bottom: 3px dashed var(--color-neon-cyan);
  }

  .boton-ano {
    font-size: 1rem;
    margin: 0 0.25rem;
    transform: none !important;
  }

  .boton-ano:hover {
    transform: scale(1.03) !important;
  }

  /* Se mantiene el indicador ::after en el móvil */
  .boton-ano::after {
    content: "";
  }

  .contenedor-contenido {
    padding: 1rem;
    width: 95%;
    max-width: none;
  }

  /* Botones de acción más grandes y centrados en móvil */
  .boton-accion-nuevo {
    width: 90%;
    max-width: 250px;
    padding: 0.6rem 1rem;
    font-size: 0.75rem;
    letter-spacing: 0.05rem;
  }

  .boton-accion-nuevo:hover,
  .boton-accion-nuevo:focus {
    padding: 0.7rem 1.1rem;
    letter-spacing: 0.1rem;
  }
}
</style>
