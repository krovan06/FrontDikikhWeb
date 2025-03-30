<template>
  <div class="StartPage">
    <canvas ref="particleCanvas" class="particle-canvas"></canvas>

    <div class="HelloText">
      <h1 class="Name poppins-text">
        <span v-for="(letter, index) in 'DIKIKH'" :key="index" :style="{ transform: `translateY(${scrollY * 0.5}px)`, '--i': index }">
          {{ letter }}
        </span>
      </h1>
      <h2 class="SmallName oswald-text" :style="{ transform: `translateY(${scrollY * 0.4}px)` }">Сайт находится в состоянии разработки</h2>
    </div>

    <div class="Waves">
      <WavesInStartPage/>
    </div>
  </div> 
</template>

<style scoped>

  :root {
    --index: calc(1vh + 1vw);
  }

  .particle-canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none; /* Чтобы canvas не мешал взаимодействию */
  }

  .StartPage {
    position: relative;
    width: 100%;
    height: 100vh;
    background-color: rgb(231, 244, 255);

    display: flex;
    justify-content: center;
  }

  .HelloText {
    display: flex;
    align-items: center;  
    flex-direction: column;
  }

  .Name {
    cursor: default;
    margin-top: 25vh;
    text-align: center;
    display: flex;
    justify-content: center;
    color: rgb(21, 23, 27);
    font-size: calc(20px + 8vw);
    letter-spacing: 32px;
    animation: ShowOutShadow 3s forwards;
  }

  @media (max-width: 800px) {
    .Name {
      letter-spacing: 10px;
    }
  }

  .Name span {
    display: inline-block; /* Для корректной работы transform */
  }

  .Name:hover span {
    animation: blick 1s forwards;
    animation-delay: calc(0.1s * var(--i));
  }
  
  @keyframes ShowOutShadow {
    0% {
      opacity: 0;
      margin-top: 26vh;
      filter: blur(10px);
    }

    100% {
      opacity: 1;
      margin-top:25vh;
      filter: blur(0px);
    }
  }

  @keyframes ShowOutShadowSmallName {
    0% {
      opacity: 0;
      margin-top: 1vh;
      filter: blur(10px);
    }

    100% {
      opacity: 1;
      margin-top:0vh;
      filter: blur(0px);
    }
  }

  @keyframes blick {
    0%, 100% {
      color: rgb(21, 23, 27);
    }
    50% {
      color: rgb(75, 84, 100);
    }
  }

  .SmallName {
    color: rgb(21, 23, 27);
    font-size: calc((1vh + 1vw) * 1.2);
    letter-spacing: 7px;
    animation: ShowOutShadowSmallName 3s forwards;
    text-align: center;
  }

  .Waves {
    position: absolute;
    bottom: 0;
    width: 100%; /* Растягиваем по ширине родителя */
  }

  /* Шрифты */
  .oswald-text {
    font-family: 'Oswald', sans-serif;
    font-weight: 400; /* Укажите нужный вес, например, 200..700 */
  }

  .poppins-text {
    font-family: 'Poppins', sans-serif;
    font-weight: 700; 
    font-style: normal;
  }
</style>

<script>
  import { inject, ref, onMounted, onUnmounted } from 'vue';
  import WavesInStartPage from './WavesInStartPage';

  export default {
    name: 'StartPage',
    components: {
      WavesInStartPage,
    },
    setup() {
      const { scrollY } = inject('scrollData');
      const particleCanvas = ref(null);
      let ctx, particlesArray = [];

      // Настраиваемые параметры
      const particleSettings = {
        count: 15, // Количество частиц
        minSize: 5, // Минимальный размер
        maxSize: 12, // Максимальный размер
        minSpeedY: 0.5, // Минимальная скорость вверх
        maxSpeedY: 2, // Максимальная скорость вверх
        speedXRange: 2, // Диапазон скорости по X
        boundaryTop: window.innerHeight / 2, // Верхняя граница (в пикселях от верха)
        boundaryBottom: window.innerHeight // Нижняя граница (в пикселях от верха)
      };

      // Класс частицы
      class Particle {
        constructor() {
          this.x = Math.random() * window.innerWidth;
          this.y = particleSettings.boundaryBottom; // Начало снизу
          this.size = Math.random() * (particleSettings.maxSize - particleSettings.minSize) + particleSettings.minSize;
          this.speedX = (Math.random() - 0.5) * particleSettings.speedXRange;
          this.speedY = -(Math.random() * (particleSettings.maxSpeedY - particleSettings.minSpeedY) + particleSettings.minSpeedY); // Скорость вверх
          this.opacity = 1;
        }

        update() {
          this.x += this.speedX;
          this.y += this.speedY; // Движение вверх

          // Уменьшение размера и прозрачности по мере приближения к центру
          const centerY = window.innerHeight / 2;
          const distanceToCenter = Math.abs(this.y - centerY);
          const maxDistance = (particleSettings.boundaryBottom - particleSettings.boundaryTop) / 2;
          const scaleFactor = distanceToCenter / maxDistance;
          this.size = Math.max(0, this.initialSize * scaleFactor); // Уменьшение от начального размера
          this.opacity = Math.max(0, scaleFactor); // Уменьшение прозрачности

          // Перезапуск частицы, если она достигла верхней границы или центра
          if (this.y < particleSettings.boundaryTop || this.size <= 0) {
            this.x = Math.random() * window.innerWidth;
            this.y = particleSettings.boundaryBottom;
            this.size = Math.random() * (particleSettings.maxSize - particleSettings.minSize) + particleSettings.minSize;
            this.initialSize = this.size; // Сохраняем начальный размер
            this.speedX = (Math.random() - 0.5) * particleSettings.speedXRange;
            this.speedY = -(Math.random() * (particleSettings.maxSpeedY - particleSettings.minSpeedY) + particleSettings.minSpeedY);
            this.opacity = 1;
          }
        }

        draw() {
          ctx.beginPath();
          ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
          ctx.fillStyle = `rgba(21, 23, 27, ${this.opacity})`;
          ctx.fill();
        }
      }

      // Инициализация частиц
      const initParticles = () => {
        particlesArray = [];
        for (let i = 0; i < particleSettings.count; i++) {
          const particle = new Particle();
          particle.initialSize = particle.size; // Сохраняем начальный размер
          particlesArray.push(particle);
        }
      };

      // Анимация частиц
      const animateParticles = () => {
        if (!ctx) return;
        ctx.clearRect(0, 0, window.innerWidth, window.innerHeight);
        for (let i = 0; i < particlesArray.length; i++) {
          particlesArray[i].update();
          particlesArray[i].draw();
        }
        requestAnimationFrame(animateParticles);
      };

      onMounted(() => {
        const canvas = particleCanvas.value;
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
        ctx = canvas.getContext('2d');
        initParticles();
        animateParticles();

        window.addEventListener('resize', () => {
          canvas.width = window.innerWidth;
          canvas.height = window.innerHeight;
          particleSettings.boundaryBottom = window.innerHeight; // Обновляем нижнюю границу
        });
      });

      onUnmounted(() => {
        window.removeEventListener('resize', () => {});
      });

      return { scrollY, particleCanvas };
    }
  };
</script>