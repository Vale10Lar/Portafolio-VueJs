<template>
  <section id="habilidades" class="contenedor-habilidades">
    <h2 class="titulo-seccion">Habilidades Técnicas</h2>

    <div class="contenedor-botones-filtro">
      <button
        v-show="mensajeAbierto"
        v-for="(cat, index) in categorias"
        :key="cat.id"
        :class="[
          'boton-filtro',
          {
            activo: categoriaActiva === cat.id,
            'oculto-en-activo': categoriaActiva && categoriaActiva !== cat.id,
          },
        ]"
        :style="{ '--clr': cat.color, 'animation-delay': index * 0.1 + 's' }"
        @click="establecerCategoria(cat.id)"
      >
        <span class="pulso-neon">{{ cat.nombre }}</span>
      </button>
    </div>

    <div
      v-if="categoriaActiva"
      class="contenedor-iconos-desplegable"
      :class="{ 'fade-out': isLoading }"
    >
      <h3
        class="subtitulo-categoria-activa"
        :style="{ color: categorias.find((c) => c.id === categoriaActiva).color }"
      >
        {{ categorias.find((c) => c.id === categoriaActiva).nombre }}
      </h3>

      <div class="prisma-contenedor">
        <div class="prisma-iconos-carrusel">
          <div
            v-for="(habilidad, index) in habilidadesFiltradas"
            :key="habilidad.nombre"
            class="habilidad-item"
            :style="{ '--accent-color': getColorByIndex(index) }"
          >
            <div class="habilidad-circulo-neon" :title="habilidad.nombre">
              <img :src="habilidad.icono" :alt="habilidad.nombre" class="habilidad-icono" />
              <div class="habilidad-halo-magico"></div>
            </div>
            <p class="habilidad-nombre">{{ habilidad.nombre }}</p>
          </div>
        </div>
      </div>
    </div>

    <div
      v-else-if="!mensajeAbierto"
      :class="['mensaje-instruccion-contenedor']"
      @click="toggleMensaje"
    >
      <div :class="['lampara-boton']" aria-label="Encender o apagar la lámpara de instrucciones">
        <div class="portal-magico-animado"></div>
      </div>

      <p class="instruccion-pulso-central">💡 ¡Enciende la luz para descubrir mis habilidades!</p>
    </div>

    <div
      v-else
      :class="['mensaje-instruccion-contenedor', 'mensaje-flotante']"
      @click="toggleMensaje"
    >
      <div :class="['lampara-boton', 'lampara-encendida']" aria-label="Volver al estado inicial">
        <div class="portal-magico-animado"></div>
      </div>

      <p class="instruccion-pulso-flotante">👋 ¡Apaga la luz antes de irte!</p>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";

// --- LÓGICA DE LA LÁMPARA Y MENSAJE DE ESTADO ---
const mensajeAbierto = ref(false); // Inicia APAGADA (Lámpara grande visible)

const toggleMensaje = () => {
  // Si la luz está apagada (CTA grande), la encendemos (muestra categorías y flotante)
  if (!mensajeAbierto.value) {
    mensajeAbierto.value = true;
  } else {
    // Si la luz está encendida (flotante), la apagamos (esconde todo, vuelve a CTA grande)
    mensajeAbierto.value = false;
    categoriaActiva.value = null; // Reiniciamos la categoría activa
  }
};

// --- IMPORTACIONES DE ÍCONOS ---
import djangoIcon from "./icons/django_icon_130645.svg";
import dockerIcon from "./icons/docker_original.svg";
import figmaIcon from "./icons/figma.svg";
import gitIcon from "./icons/git_icon.svg";
import htmlIcon from "./icons/html.svg";
import javaIcon from "./icons/java.svg";
import javascriptIcon from "./icons/javascript.svg";
import mysqlIcon from "./icons/mysql.svg";
import nodejsIcon from "./icons/node-js.svg";
import reactIcon from "./icons/react.svg";
import vueIcon from "./icons/vue.svg";
import sqlIcon from "./icons/sql.svg";

// --- LÓGICA DE DESPLIEGUE Y FILTRADO ---
const categorias = ref([
  { id: "Lenguajes", nombre: "Lenguajes de Programación", color: "var(--color-neon-cyan)" },
  { id: "Frameworks", nombre: "Frameworks y Librerías", color: "var(--vt-c-magenta-pure)" },
  { id: "Herramientas", nombre: "Herramientas y Software", color: "var(--vt-c-neon-green)" },
]);

const todasHabilidades = [
  // Lenguajes
  { nombre: "Java", icono: javaIcon, categoria: "Lenguajes" },
  { nombre: "JavaScript", icono: javascriptIcon, categoria: "Lenguajes" },
  { nombre: "HTML5", icono: htmlIcon, categoria: "Lenguajes" },
  { nombre: "SQL", icono: sqlIcon, categoria: "Lenguajes" },

  // Frameworks y Librerías
  { nombre: "Vue.js", icono: vueIcon, categoria: "Frameworks" },
  { nombre: "React", icono: reactIcon, categoria: "Frameworks" },
  { nombre: "Django", icono: djangoIcon, categoria: "Frameworks" },
  { nombre: "Node.js", icono: nodejsIcon, categoria: "Frameworks" },

  // Herramientas y Software
  { nombre: "Git", icono: gitIcon, categoria: "Herramientas" },
  { nombre: "Docker", icono: dockerIcon, categoria: "Herramientas" },
  { nombre: "Figma", icono: figmaIcon, categoria: "Herramientas" },
  { nombre: "MySQL", icono: mysqlIcon, categoria: "Herramientas" },
];

const categoriaActiva = ref(null);
const isLoading = ref(false);

const establecerCategoria = (id) => {
  // Si clica en la categoría activa, la desactiva (reset), volviendo a la vista de botones de categoría.
  if (categoriaActiva.value === id) {
    categoriaActiva.value = null;
    return;
  }

  isLoading.value = true;

  setTimeout(() => {
    categoriaActiva.value = id;
    // mensajeAbierto se mantiene TRUE aquí, por lo que el flotante sigue visible.
    isLoading.value = false;
  }, 350);
};

const habilidadesFiltradas = computed(() => {
  if (!categoriaActiva.value) {
    return [];
  }
  return todasHabilidades.filter((h) => h.categoria === categoriaActiva.value);
});

// Colores Neón para los círculos de las habilidades
const coloresHabilidades = ref([
  "var(--vt-c-neon-green)",
  "var(--vt-c-magenta-pure)",
  "var(--color-neon-cyan)",
  "var(--vt-c-indigo)",
  "var(--vt-c-yellow-pure)",
]);

const getColorByIndex = (index) => {
  return coloresHabilidades.value[index % coloresHabilidades.value.length];
};
</script>

<style scoped>
/* ------------------------------------------------------------- */
/* ANIMACIONES (Mantenidas) */
/* ------------------------------------------------------------- */

@keyframes neon-flicker-active {
  0%,
  100% {
    box-shadow: 0 0 5px var(--clr), 0 0 10px var(--clr);
  }
  50% {
    box-shadow: 0 0 15px var(--clr), 0 0 30px var(--clr);
  }
}

@keyframes border-rotate {
  0% {
    transform: rotate(0deg);
    filter: hue-rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
    filter: hue-rotate(360deg);
  }
}

@keyframes neon-pulse-border {
  0% {
    border-color: var(--vt-c-magenta-pure);
    box-shadow: 0 0 5px var(--vt-c-magenta-pure);
  }
  50% {
    border-color: var(--color-neon-cyan);
    box-shadow: 0 0 15px var(--color-neon-cyan), 0 0 30px var(--vt-c-magenta-pure);
  }
  100% {
    border-color: var(--vt-c-magenta-pure);
    box-shadow: 0 0 5px var(--vt-c-magenta-pure);
  }
}

@keyframes lampara-glitch-luz {
  0%,
  100% {
    opacity: 0.5;
    filter: hue-rotate(0deg);
  }
  50% {
    opacity: 0.8;
    filter: hue-rotate(180deg);
  }
}

@keyframes neon-pulse-instruccion-off {
  0%,
  100% {
    text-shadow: 0 0 8px var(--vt-c-neon-green), 0 0 16px var(--vt-c-neon-green);
  }
  50% {
    text-shadow: 0 0 12px var(--vt-c-neon-green), 0 0 25px var(--vt-c-neon-green),
      0 0 40px var(--vt-c-neon-green-rgba-70);
  }
}

@keyframes rotation-float {
  0% {
    transform: translateY(0px) rotateZ(0deg);
  }
  50% {
    transform: translateY(-3px) rotateZ(1deg);
  }
  100% {
    transform: translateY(0px) rotateZ(0deg);
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

@keyframes glitch-color {
  0% {
    filter: hue-rotate(0deg);
  }
  100% {
    filter: hue-rotate(360deg);
  }
}

@keyframes prisma-rotate {
  0% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(360deg);
  }
}

/* ------------------------------------------------------------- */
/* LAYOUT PRINCIPAL Y FONDO */
/* ------------------------------------------------------------- */
.contenedor-habilidades {
  min-height: 100vh;
  padding: 4rem 1rem 4rem 1rem;
  max-width: 100%;
  position: relative;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  background-color: var(--color-theme-base);
  overflow: hidden;
  z-index: 10;
}

.titulo-seccion {
  position: relative;
  margin-bottom: 2rem;
  font-family: "Orbitron", sans-serif;
  font-size: 2.8rem;
  font-weight: 700;
  color: var(--vt-c-white);
  text-shadow: 0 0 10px var(--vt-c-white), 0 0 25px var(--vt-c-white),
    0 0 40px var(--vt-c-neon-green-rgba-80);
  padding-bottom: 0.5rem;
  border-bottom: 2px solid var(--vt-c-indigo-rgba-20);
  z-index: 10;
}

/* ------------------------------------------------------------- */
/* BOTONES DE FILTRADO NEÓN */
/* ------------------------------------------------------------- */
.contenedor-botones-filtro {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 15px;
  margin-bottom: 1rem;
  width: 95%;
  max-width: 900px;
  z-index: 10;
  transition: all 0.5s ease-in-out;
}

.boton-filtro {
  --clr: var(--clr);
  position: relative;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 0.7rem 1.2rem;
  color: var(--clr);
  text-decoration: none;
  font-size: 0.95rem;
  font-family: "Orbitron", sans-serif;
  letter-spacing: 0.02em;
  text-transform: uppercase;
  transition: 0.3s;
  background: var(--vt-c-black-soft);
  border: 2px solid var(--clr);
  cursor: pointer;
  border-radius: 5px;
  box-shadow: 0 0 5px var(--vt-c-white-rgba-10);

  animation: rotation-float 4s ease-in-out infinite alternate;
}

.boton-filtro.activo {
  margin: 0 10px;
  animation: neon-flicker-active 2s infinite ease-in-out;
  border-style: solid;
  box-shadow: 0 0 10px var(--clr), 0 0 20px var(--clr);
}

/* ------------------------------------------------------------- */
/* LÁMPARA DINÁMICA (CTA de HABILIDADES) */
/* ------------------------------------------------------------- */
.mensaje-instruccion-contenedor {
  position: relative;
  min-height: 40vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
  z-index: 1000;
  margin-top: 1rem;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

/* AJUSTE DE POSICIÓN DE LA LÁMPARA FLOTANTE (Transformación a Pequeño) */
.mensaje-instruccion-contenedor.mensaje-flotante {
  position: absolute; /* CLAVE para el movimiento a la esquina */
  top: auto;
  bottom: 50px;
  left: 50px;

  min-height: 0;
  width: auto;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 5px;

  z-index: 99999;
  background: none !important;
  padding: 0;
}

/* ESTILOS DE LÁMPARA GRANDE (Luz Apagada) */
.lampara-boton {
  position: relative;
  overflow: hidden;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 5px solid var(--vt-c-magenta-pure);
  display: flex;
  justify-content: center;
  align-items: center;
  background: radial-gradient(circle at 50% 50%, var(--vt-c-black) 0%, var(--vt-c-black) 100%);
  cursor: pointer;

  animation: neon-pulse-border 3s infinite alternate ease-in-out;
  transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
.lampara-boton:hover {
  transform: scale(1.05);
  box-shadow: 0 0 20px var(--color-neon-cyan), 0 0 40px var(--vt-c-magenta-pure);
  border-color: var(--color-neon-cyan);
}
.lampara-boton::before {
  content: "";
  position: absolute;
  top: -150%;
  left: -150%;
  width: 300%;
  height: 300%;
  border-radius: 50%;
  background: conic-gradient(
    transparent 0deg,
    transparent 180deg,
    var(--vt-c-neon-green) 180deg,
    var(--color-neon-cyan) 270deg,
    var(--vt-c-magenta-pure) 360deg
  );
  animation: border-rotate 2.5s linear infinite;
  opacity: 0;
  z-index: 1;
}

/* ESTILOS DE LÁMPARA FLOTANTE (Luz Encendida) */
.lampara-boton.lampara-encendida {
  /* Estilo "Encendido" (FLOTANTE) */
  width: 50px; /* Reducir tamaño */
  height: 50px;
  border: 2px solid var(--vt-c-neon-green);
  background: radial-gradient(
    circle at 50% 50%,
    var(--vt-c-neon-green-rgba-50) 0%,
    var(--vt-c-black) 100%
  );
  box-shadow: 0 0 10px var(--vt-c-neon-green), 0 0 20px var(--color-neon-cyan);
  animation: none;
  z-index: 100;
}
/* Borde Eléctrico Giratorio: Se mantiene visible en estado ENCENDIDO */
.lampara-boton.lampara-encendida::before {
  opacity: 1;
  display: block;
}

/* ESTILOS DEL PORTAL (Luz Apagada) */
.portal-magico-animado {
  position: relative;
  z-index: 10;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: radial-gradient(
    circle at 50% 50%,
    var(--color-neon-cyan) 0%,
    var(--vt-c-magenta-pure-rgba-50) 100%
  );
  box-shadow: 0 0 15px var(--color-neon-cyan);
  animation: lampara-glitch-luz 2s infinite alternate;
  transition: all 0.5s;
  opacity: 1;
}
/* ESTILOS DEL PORTAL (Luz Encendida - Pequeño) */
.lampara-boton.lampara-encendida .portal-magico-animado {
  width: 30px;
  height: 30px;
  box-shadow: 0 0 5px var(--vt-c-neon-green);
  background: radial-gradient(
    circle at 50% 50%,
    var(--vt-c-neon-green) 0%,
    var(--vt-c-neon-green-rgba-10) 100%
  );
  animation: glitch-color 5s linear infinite alternate;
  opacity: 1;
}

/* ESTILOS DE INSTRUCCIONES */
.instruccion-pulso-central {
  /* CTA de ENCIENDE LA LUZ (Verde) - Estado Grande */
  font-family: "Orbitron", sans-serif;
  font-size: 1.1rem;
  color: var(--vt-c-neon-green);
  text-shadow: 0 0 5px var(--vt-c-neon-green), 0 0 10px var(--vt-c-neon-green);
  animation: neon-pulse-instruccion-off 3s infinite alternate ease-in-out;
  padding: 1rem;
  max-width: 600px;
}
.instruccion-pulso-flotante {
  /* Mensaje de APAGA LA LUZ (Blanco/Cian) - Estado Flotante */
  font-family: "Orbitron", sans-serif;
  font-size: 0.8rem;
  color: var(--vt-c-white);
  text-shadow: 0 0 3px var(--color-neon-cyan);
  margin-top: 5px;
  white-space: nowrap;
  padding: 0;
  text-align: left;
}

/* ------------------------------------------------------------- */
/* CONTENIDO DESPLEGABLE: EL PRISMA 3D */
/* ------------------------------------------------------------- */
.contenedor-iconos-desplegable {
  width: 100%;
  animation: fadeIn 0.5s ease-out;
  z-index: 10;
}

.subtitulo-categoria-activa {
  font-family: "Orbitron", sans-serif;
  font-size: 1.6rem;
  margin-bottom: 1.5rem;
  font-weight: 500;
  color: var(--vt-c-white);
  text-shadow: 0 0 8px var(--vt-c-white), 0 0 20px var(--vt-c-white-rgba-50);
  transition: color 0.3s;
}
.prisma-contenedor {
  width: 95%;
  max-width: 900px;
  margin: 0 auto;
  perspective: 1000px;
  overflow: hidden;
  padding: 30px 0;
}

.prisma-iconos-carrusel {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 40px;
  transform-style: preserve-3d;
  transform: rotateY(0deg);
  animation: prisma-rotate 40s linear infinite;
  padding: 20px 0;
}

.habilidad-circulo-neon {
  position: relative;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--vt-c-black-soft);
  border: 2px solid var(--accent-color);
  transform: none;
  box-shadow: 0 0 8px var(--accent-color), 0 0 18px var(--accent-color);
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  cursor: pointer;
  margin-bottom: 10px;
  overflow: hidden;
}
.habilidad-nombre {
  margin: 0;
  font-size: 0.9em;
  font-family: "Orbitron", sans-serif;
  color: var(--vt-c-white);
  text-shadow: 0 0 5px var(--vt-c-white-rgba-30);
  transition: color 0.3s ease;
}

/* ------------------------------------------------------------- */
/* RESPONSIVE (Asegura la posición flotante en móvil) */
/* ------------------------------------------------------------- */
@media (max-width: 768px) {
  /* Ajuste de la lámpara flotante en móvil */
  .mensaje-instruccion-contenedor.mensaje-flotante {
    top: auto;
    bottom: 40px;
    left: 30px;
  }
  .instruccion-pulso-flotante {
    display: block;
    font-size: 0.75rem;
  }

  .lampara-boton.lampara-encendida {
    width: 45px;
    height: 45px;
  }

  .lampara-boton.lampara-encendida .portal-magico-animado {
    width: 25px;
    height: 25px;
  }
}

@media (max-width: 480px) {
  /* Ajuste fino para móviles pequeños */
  .mensaje-instruccion-contenedor.mensaje-flotante {
    bottom: 20px;
    left: 20px;
  }
}
</style>
