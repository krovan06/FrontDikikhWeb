<template>
  <div class="PortfolioBlock">
    <div class="PortfolioContainer">
      <div class="PortfolioContentColumn section" id="portfolio-section">
        <div class="Portfolio" v-if="projects.length > 0">
          <!-- Левая колонка с текстовой информацией -->
          <div class="PortfolioText">
            <div class="NameProduct">
              <h1 class="Name poppins-text" id="name">
                {{ projects[currentProjectIndex].name }}
              </h1>
            </div>
            <div class="InformationOrProduct">
              <p class="Information oswald-text" id="info">
                {{ projects[currentProjectIndex].info }}
              </p>
            </div>
            <div class="OtherInformation">
              <p class="Other oswald-text" id="other">
                {{ projects[currentProjectIndex].other }}
              </p>
            </div>
            <div class="ActivePortfolioBlcok">
              <div class="ProfileBTN GoToLink" @click="goToProject">
                <p class="Other oswald-text">К проекту</p>
              </div>
              <div class="ProfileBTN BtnArrayLeft" @click="prevProject">
                <p class="Other oswald-text">Вперед</p>
              </div>
              <div class="ProfileBTN BtnArrayRight" @click="nextProject">
                <p class="Other oswald-text">Назад</p>
              </div>
            </div>
          </div>
          
          <div class="PortfolioProductContainer">
            <div
              class="PortfolioProduct"
              :key="currentProjectIndex"
              :class="{'hide-product': isProductAnimating, 'fade-in-product': showProductFadeIn}"
              :style="{ backgroundImage: 'url(' + currentImage + ')' }"
            ></div>
          </div>
        </div>
        <div v-else class="loading-message">
          <p class="oswald-text">Загрузка проектов...</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  .PortfolioBlock {
    min-height: 100vh;
    width: 100%;
    background-color: rgb(21, 23, 27);
  }

  .PortfolioContainer {
    width: 100%;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .PortfolioContentColumn {
    position: relative;
    width: 90%;
    height: 100%;
    display: flex;
    justify-content: center;
  }

  .Portfolio {
    width: 100%;
    height: 80vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .PortfolioText {
    width: 50%;
    height: 100%;
  }

  .PortfolioProductContainer {
    width: 50%;
    height: 100%;
    display: flex;
    align-items: end;
    justify-content: center;
  }

  .PortfolioProduct {
    width: 90%;
    height: 90%;
    border: 1px solid white;
    border-radius: 20px;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  }

  .NameProduct {
    width: 100%;
    height: 15%;
    display: flex;
    align-items: end;
    justify-content: left;
  }

  .InformationOrProduct {
    width: 100%;
    height: 50%;
    border-bottom: 2px solid aliceblue;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .OtherInformation {
    width: 100%;
    height: 20%;
    border-bottom: 2px solid aliceblue;
    display: flex;
    align-items: center;
    justify-content: left;
  }

  .Name,
  .Information,
  .Other {
    text-align: left;
    color: rgb(208, 212, 220);
  }

  .Name {
    font-size: calc((1vh + 1vw) * 1.6);
  }

  .Information {
    font-size: calc((1vh + 1vw) * 0.8);
    line-height: calc((1vh + 1vw) * 2.32);
    letter-spacing: 6px;
  }

  .Other {
    font-size: calc((1vh + 1vw) * 0.8);
  }

  .ActivePortfolioBlcok {
    height: 10%;
    display: flex;
    align-items: center;
    justify-content: right;
    gap: calc((1vh + 1vw) * 3.2);
  }

  .ProfileBTN {
    padding: calc((1vh + 1vw) * 0.4);
    border-radius: 10px;
    transition: 0.3s;
    cursor: pointer;
  }

  .ProfileBTN:hover {
    color: white;
    text-shadow: 0px 0px 14px rgb(197, 201, 211);
  }

  /* Анимации */
  .fade-out {
    animation: fadeOut 0.5s forwards;
  }

  .fade-in {
    animation: fadeIn 0.5s forwards;
  }

  .hide-product {
    animation: fadeOut 0.5s forwards;
  }

  .fade-in-product {
    animation: fadeIn 0.5s forwards;
  }

  .loading-message {
    color: rgb(208, 212, 220);
    font-size: calc((1vh + 1vw) * 1.2);
    text-align: center;
    padding: 20px;
  }

  @keyframes fadeOut {
    0% {
      transform: translateY(0);
      filter: blur(0);
      opacity: 1;
    }
    100% {
      transform: translateY(-20px);
      filter: blur(10px);
      opacity: 0;
    }
  }

  @keyframes fadeIn {
    0% {
      transform: translateY(20px);
      filter: blur(10px);
      opacity: 0;
    }
    100% {
      transform: translateY(0);
      filter: blur(0);
      opacity: 1;
    }
  }

  @keyframes fadeOut {
    0% {
      transform: translateY(0);
      filter: blur(0);
      opacity: 1;
    }
    100% {
      transform: translateY(-20px);
      filter: blur(10px);
      opacity: 0;
    }
  }

  @keyframes fadeIn {
    0% {
      transform: translateY(20px);
      filter: blur(10px);
      opacity: 0;
    }
    100% {
      transform: translateY(0);
      filter: blur(0);
      opacity: 1;
    }
  }

  /* Шрифты */
  .oswald-text {
    font-family: "Oswald", sans-serif;
    font-weight: 400;
  }

  .poppins-text {
    font-family: "Poppins", sans-serif;
    font-weight: 700;
    font-style: normal;
  }

  @media (max-width: 1000px) {
    .Portfolio {
      flex-direction: column;
    }

    .Information {
    letter-spacing: 3px;
  }
  }

</style>

<script>
import axios from 'axios';

export default {
  name: "PortfolioBlock",
  data() {
    return {
      projects: [], // Изначально пустой массив
      currentProjectIndex: 0,
      isAnimating: false,
      isProductAnimating: false,
      showProductFadeIn: false,
      defaultImage: "/static/APP1/images/default.jpg"
    };
  },
  computed: {
    currentImage() {
      return this.projects[this.currentProjectIndex]?.image || this.defaultImage;
    }
  },
  mounted() {
    this.fetchProjects();
  },
  methods: {
    async fetchProjects() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/portfolio-projects/');
        this.projects = response.data;
        if (this.projects.length === 0) {
          this.projects = [{
            name: "Нет проектов",
            info: "Добавьте проекты в админ-панели",
            other: "",
            image: this.defaultImage
          }];
        }
      } catch (error) {
        console.error('Ошибка при загрузке проектов:', error);
        this.projects = [{
          name: "Ошибка загрузки",
          info: "Не удалось загрузить проекты",
          other: "",
          image: this.defaultImage
        }];
      }
    },
    updateProject(index) {
      if (this.isAnimating || this.projects.length === 0) return;
      this.isAnimating = true;
      
      const nameEl = document.getElementById("name");
      const infoEl = document.getElementById("info");
      const otherEl = document.getElementById("other");

      nameEl.classList.add("fade-out");
      infoEl.classList.add("fade-out");
      otherEl.classList.add("fade-out");

      this.isProductAnimating = true;

      setTimeout(() => {
        this.currentProjectIndex = index;

        nameEl.classList.remove("fade-out");
        infoEl.classList.remove("fade-out");
        otherEl.classList.remove("fade-out");

        nameEl.classList.add("fade-in");
        infoEl.classList.add("fade-in");
        otherEl.classList.add("fade-in");

        this.isProductAnimating = false;
        this.showProductFadeIn = true;

        setTimeout(() => {
          nameEl.classList.remove("fade-in");
          infoEl.classList.remove("fade-in");
          otherEl.classList.remove("fade-in");
          this.showProductFadeIn = false;
          this.isAnimating = false;
        }, 500);
      }, 500);
    },
    nextProject() {
      const nextIndex = (this.currentProjectIndex + 1) % this.projects.length;
      this.updateProject(nextIndex);
    },
    prevProject() {
      const prevIndex = (this.currentProjectIndex - 1 + this.projects.length) % this.projects.length;
      this.updateProject(prevIndex);
    },
    goToProject() {
      console.log("Переход на страницу проекта:", this.projects[this.currentProjectIndex].name);
    }
  }
};
</script>