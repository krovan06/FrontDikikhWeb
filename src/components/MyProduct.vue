<template>
  <div class="PortfolioBlock" ref="portfolioRef">
    <div class="ContentContainerBlock section" id="portfolio-section">
      <div
        class="ContentLine"
        v-for="(line, lineIndex) in lines"
        :key="lineIndex"
        :ref="el => setLineRef(el, lineIndex)"
      >
        <div
          v-for="(block, blockIndex) in line"
          :key="blockIndex"
          :class="block.blockClass"
          v-show="visibleLines[lineIndex]"
        >
          <div :class="['MyProjectPhoto', block.photoClass]"></div>
          <div class="MyProjectInfo">
            <div class="MyProjectInfo_Logo"></div>
            <div class="MyProjectInfo_Text">
              <div class="ProjectName poppins-text">{{ block.projectName }}</div>
              <div class="OtherProjectInfo oswald-text">{{ block.info }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .PortfolioBlock {
    position: relative;
    min-height: 100vh;
    width: 100%;
    background-color: rgb(21, 23, 27);
    display: flex;
    align-items: center;
    justify-content: center;

    transition: background-color 0.6s ease;
  }

  .ContentContainerBlock {
    display: flex;
    width: 90%;

    flex-direction: column;

    gap: 5vh;
  }

  .ContentLine {
    width: 100%;
    height: 80vh;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .ContentBlock {
    display: flex;
    flex-direction: column;
    gap: 2vh;
  }

  .MyProjectPhoto {
    background-color: azure;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeats;  
  }

  .Normal {
    width: 28.5vw;
    height: 700px;
    border-radius: 15px;
  }

  .Medium {
    width: 58.8vw;
    height: 700px;
    border-radius: 15px;
  }

  .Big {
    width: calc(100vw * 0.89);
    height: 700px;
    border-radius: 15px;
  }

  .MyProjectInfo {
    width: 100%;
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 1vw;
  }

  .MyProjectInfo_Logo {
    width: 52px;
    height: 52px;
    border-radius: 13px;
    background-color: rgb(48, 50, 50);
  }

  .ProjectName {
    font-size: 20px;
    color: aliceblue;
  }

  .OtherProjectInfo {
    font-size: 15px;
    color: rgb(102, 104, 107);
  }

  .AnimationShow {
    animation: ShowUp 2s forwards;
    animation-delay: 0s; /* задержка */
  }

  /* Анимации */

  @keyframes ShowUp {
    0% {
      opacity: 0;
      margin-top: 10vh;
    }

    100% {
      opacity: 1;
      margin-top: 0;
    }
  }

  /* Адаптивность */

  @media (max-width: 1000px) {
    .Normal {
      width: calc(100vw * 0.89);
    }

    .BigPicture {
      display: none;
    }
  }

  /* ШРИФТ */
  .oswald-text {
    font-family: 'Oswald', sans-serif;
    font-weight: 500;
  }

  .poppins-text {
    font-family: 'Poppins', sans-serif;
    font-weight: 550; 
    font-style: normal;
  }
</style>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

const portfolioRef = ref(null)

// Данные линий
const lines = ref([
  [
    {
      photoClass: 'Normal',
      projectName: 'One',
      info: 'PRODUCT DESIGN, WEBSITE',
      blockClass: 'ContentBlock AnimationShow',
    },
    {
      photoClass: 'Medium',
      projectName: 'Podyx',
      info: 'PRODUCT DESIGN, WEBSITE',
      blockClass: 'ContentBlock BigPicture AnimationShow',
    },
  ],
  [
    {
      photoClass: 'Normal',
      projectName: 'Podyx',
      info: 'PRODUCT DESIGN, WEBSITE',
      blockClass: 'ContentBlock AnimationShow',
    },
    {
      photoClass: 'Normal',
      projectName: 'Podyx',
      info: 'PRODUCT DESIGN, WEBSITE',
      blockClass: 'ContentBlock BigPicture AnimationShow',
    },
    {
      photoClass: 'Normal',
      projectName: 'Podyx',
      info: 'PRODUCT DESIGN, WEBSITE',
      blockClass: 'ContentBlock BigPicture AnimationShow',
    },
  ],
  [
    {
      photoClass: 'Big',
      projectName: 'Podyx',
      info: 'PRODUCT DESIGN, WEBSITE',
      blockClass: 'ContentBlock AnimationShow',
    },
  ],
])

// Реактивные переменные
const visibleLines = ref(Array(lines.value.length).fill(false))
const lineRefs = ref([])
const observers = ref([])

// Функция для установки ссылок
const setLineRef = (el, index) => {
  if (el) {
    lineRefs.value[index] = el
  }
}

onMounted(async () => {
  // Наблюдатель для изменения цвета фона PortfolioBlock
  if (portfolioRef.value) {
    const portfolioObserver = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          portfolioRef.value.style.backgroundColor = '#04030F'
        } else {
          portfolioRef.value.style.backgroundColor = '#15171b'
        }
      },
      { threshold: 0.38 }
    )
    portfolioObserver.observe(portfolioRef.value)
    observers.value.push(portfolioObserver)
  }

  // Ждём обновления DOM для lineRefs
  await nextTick()

  // Наблюдатели для линий
  lineRefs.value.forEach((el, index) => {
    if (el) {
      const observer = new IntersectionObserver(
        ([entry]) => {
          if (entry.isIntersecting) {
            visibleLines.value[index] = true
            observer.unobserve(entry.target)
          }
        },
        { threshold: 0.3 }
      )
      observer.observe(el)
      observers.value.push(observer)
    }
  })
})

onUnmounted(() => {
  observers.value.forEach(observer => observer.disconnect())
})
</script>