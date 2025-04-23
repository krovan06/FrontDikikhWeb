<template>
  <div class="AboutMeBlock">
    <div class="MainContentContainer">
      <div class="MainContentColumn section" id="about-text">
        <div class="TextAboutMe" :style="{ transform: `translateY(${scrollY * -0.2}px)` }">
          <h1 class="AboutText oswald-text"></h1>
        </div>
        <div class="Photo animate-on-scroll" :style="{ backgroundImage: `url(${Me})`}"></div>
        <h1 class="Surname oswald-text animate-on-scroll">
          <span v-for="(letter, index) in 'DIKIKH'" :key="index" :style="{'--i': index }">
            {{ letter }}
          </span>
        </h1>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .AboutMeBlock {
    width: 100%;
    height: 100vh;
    background-color: rgb(21, 23, 27);
  }
  
  .MainContentContainer {
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .MainContentColumn {
    position: relative;
    width: 90%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .Photo {
    opacity: 0;
    position: absolute;
    z-index: 200;
    width: calc((1vh + 1vw) * 15);
    height: calc((1vh + 1vw) * 23);
    margin-left: 70%;
    background-position: center;
    background-size: cover;
    border-radius: 15px;
  }

  .animation-photo {
    animation: ShowPhoto 2s forwards;
  }

  @keyframes ShowPhoto {
    0% {
      opacity: 0;
    }
    100%{
      opacity: 1;
    }
  }

  .Photo::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(23, 23, 23, 0.685);
  }

  .Surname {
    position: absolute;
    color: aliceblue;
    margin-left: 70%;
    margin-bottom: 50%;
    z-index: 2000;
    font-size: 48px;
  }

  .Surname span {
    opacity: 0;
    display: inline-block;
  }

  .Surname.animated span {
    animation: ShowOutShadow 2s forwards;
    animation-delay: calc(0.1s * var(--i));
  }

  @keyframes ShowOutShadow {
    0% {
      opacity: 0;
      margin-top: 1vh;
      filter: blur(10px);
    }
    100% {
      opacity: 1;
      margin-top: 0;
      filter: blur(0px);
    }
  }

  .TextAboutMe {
    position: relative;
    z-index: 250;
    text-shadow: 10px 0px 14px rgb(0, 0, 0);
    width: 60%;
    margin-top: 45vh;
  }

  :root {
    --index: calc(1vh + 1vw);
  }

  .AboutText {
    color: rgb(208, 212, 220);
    /*font-size: 25px;*/
    font-size: calc((1vw + 1vh) * 1.05);
    line-height: calc((1vw + 1vh) * 2);
    letter-spacing: 6px;
    line-height: 68px;
    text-align: left;
  }

  @media (max-width: 1520px) {
    .Surname {
      display: none;
    }
  }

  @media (max-width:700px) {
    .AboutText {
      text-align: justify;
      line-height: 18px;
    }

    .Photo {
      margin-left: 0;
      width: calc((1vw + 1vh) * 12);
      height: calc((1vw + 1vh) * 12);
      border-radius: 100%;
      margin-bottom: 150%;
    }

  }

  .oswald-text {
    font-family: 'Oswald', sans-serif;
    font-weight: 500;
  }
</style>

<script>
  import Me from '@/assets/Me.jpg';
  import { inject } from 'vue';
  import axios from 'axios';

  export default {
    setup() {
      const { scrollY } = inject('scrollData');
      return { scrollY };
    },

    data() {
      return {
        Me,
        aboutText: '',
      };
    },

    mounted() {
      this.fetchAboutText();
      window.addEventListener('scroll', this.handleScroll);
      this.handleScroll();
    },

    beforeUnmount() {
      window.removeEventListener('scroll', this.handleScroll);
    },

    methods: {

      async fetchAboutText() {
        try {
          const response = await axios.get('http://localhost:8000/api/about-text/');
          this.aboutText = response.data.text; // Сохраняем полученный текст
          this.initTyped(); // Инициализируем Typed.js после получения текста
        } catch (error) {
          console.error('Ошибка при загрузке текста:', error);
          this.aboutText = 'Внутри проекта интересуюсь также процессами бизнеса, маркетинга, разработки, продаж и другими вещами, которые могли бы помочь мне углубиться в контекст продукта и взглянуть шире на возможные решения или внедрение новых фич. Смотрю на то, как устроены продукты у других. Серфю интернет, если задача сложная и не имеет единственно верного решения.'; // Текст по умолчанию при ошибке
          this.initTyped(); // Инициализируем с текстом ошибки
        }
      },

      initTyped() {
        const checkTyped = () => {
          // eslint-disable-next-line no-undef
          if (typeof Typed !== 'undefined') {
            // eslint-disable-next-line no-undef
            new Typed('.AboutText', {
              strings: [this.aboutText],
              typeSpeed: 30,
              startDelay: 500,
              backSpeed: 30,
              backDelay: 1000,
              loop: false,
              showCursor: true,
              cursorChar: "|",
            });
          } else {
            console.log('Typed.js еще не загружен, ждем...');
            setTimeout(checkTyped, 100);
          }
        };
        checkTyped();
      },

      isFullyVisible(element) {
        const rect = element.getBoundingClientRect();
        return (
          rect.top >= 0 &&
          rect.bottom <= window.innerHeight
        );
      },

      handleScroll() {
        const photo = document.querySelector('.Photo');
        const surname = document.querySelector('.Surname');
        
        if (this.isFullyVisible(photo) && !photo.classList.contains('animation-photo')) {
          photo.classList.add('animation-photo');
        }
        
        if (this.isFullyVisible(surname) && !surname.classList.contains('animated')) {
          surname.classList.add('animated');
        }
      }
    }
  };
</script>