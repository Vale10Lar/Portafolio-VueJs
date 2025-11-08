<template>
  <div class="cyber-background">
    <div class="cyber-particles"></div>
    <div class="cyber-grid"></div>
  </div>
</template>

<script setup>
// No necesita lógica en Vue, solo estilizado.
</script>

<style scoped>
/* ------------------------------------------------------------- */
/* ANIMACIONES */
/* ------------------------------------------------------------- */

/* Desplazamiento sutil para las partículas/células */
@keyframes flow {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(100px, 100px);
  }
}

/* Pulso de brillo para la rejilla */
@keyframes grid-pulse {
  0% {
    opacity: 0.05;
  }
  50% {
    opacity: 0.15;
  }
  100% {
    opacity: 0.05;
  }
}

/* ------------------------------------------------------------- */
/* CONTENEDOR PRINCIPAL DEL FONDO */
/* ------------------------------------------------------------- */
.cyber-background {
  /* Ocupa el 100% del padre */
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  /* [CORREGIDO] Usa variable de negro puro */
  background-color: var(--vt-c-black);
  z-index: 1; /* Debe estar detrás del contenido */
}

/* ------------------------------------------------------------- */
/* CAPA DE PARTÍCULAS / FLUJO ABSTRACTO */
/* ------------------------------------------------------------- */
.cyber-particles {
  width: 200%;
  height: 200%;
  /* Patrón de puntos o celdas sutiles */
  background: radial-gradient(circle, var(--vt-c-cyan-rgba-10) 1px, transparent 1px),
    radial-gradient(circle, var(--vt-c-magenta-rgba-10) 1px, transparent 1px);
  background-size: 50px 50px; /* Distancia entre puntos */
  opacity: 0.6;

  /* Animación lenta */
  animation: flow 120s linear infinite alternate;
}

/* ------------------------------------------------------------- */
/* CAPA DE REJILLA / MATRIZ */
/* ------------------------------------------------------------- */
.cyber-grid {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* Rejilla usando gradientes */
  background: repeating-linear-gradient(
      to right,
      var(--vt-c-cyan-rgba-40) 1px,
      transparent 1px,
      transparent 50px
    ),
    repeating-linear-gradient(
      to bottom,
      var(--vt-c-magenta-rgba-40) 1px,
      transparent 1px,
      transparent 50px
    );
  background-size: 50px 50px;

  /* Animación de pulso de brillo */
  animation: grid-pulse 6s ease-in-out infinite alternate;
}
</style>
