<template>

  <div class="waves-container">
    <svg viewBox="0 0 1920 267" preserveAspectRatio="none" style="width: 100%; height: 20vh; display: block;" fill="none" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
      <path id="Прямоугольник 10" d="M0 0C0 0 60.87 20 220 40C379.12 60 417.12 100 636.5 120C855.87 140 967.37 60 1211 40C1454.62 20 1433.75 100 1611 120C1788.25 140 1920 60 1920 60L1920 267L0 267L0 0Z" fill="#15171B" fill-opacity="1.000000" fill-rule="evenodd" :style="{ transform: `translateY(${scrollY * 0.5}px)` }" class="waveFront">
      </path>
      <path id="Прямоугольник-10-2" d="M1920 0C1920 0 1859.12 0.12 1700 28.99C1540.88 57.87 1502.88 114.62 1283.5 115.5C1064.12 116.37 956.45 51 712.82 49C469.2 46.99 486.25 109.99 309 107.49C131.75 105 0 152.5 0 152.5L0 267L1920 267L1920 0Z" fill="#15171B" fill-opacity="0.866286" fill-rule="evenodd" :style="{ transform: `translateY(${scrollY * 0.05}px)` }" class="waveBack">
      </path>
    </svg>
    

  </div>

</template>

<style scoped>
  .waves-container {
    width: 100%;
    max-width: none;
  }

  svg {
    width: 100%; 
    height: 100px; 
    display: block; 
  }

  .waveFront {
    transform: translateY(100%); /* Начальная позиция ниже области видимости */
    animation: waves 2s ease-out forwards; /* Анимация появления снизу вверх */
  }

  .waveBack {
    opacity: 0;
    transform: translateY(100%);
    animation: waves 1.9s ease-out forwards;
    animation-delay: 0.5s;
  }

  @keyframes waves {
    0% {
      opacity: 0;
      transform: translateY(100%); /* Волны начинаются снизу */
    }
    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>

<script>
  import { inject } from 'vue';

  export default {
    setup() {
      const scrollY = inject("scrollData");

      return scrollY;
    },

    name: 'StartPage',

  }
</script>