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
          <div :class="['MyProjectPhoto', block.photoClass]" :style="{backgroundImage: block.img}"></div>
          <div class="MyProjectInfo">
            <div class="MyProjectInfo_Logo" :style="{backgroundImage: block.logoImg}"></div>
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
    background-repeat: no-repeat;  
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
    background-position: center;
    background-size: cover;
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
import axios from 'axios'

// ------------ 1. Рефы и реактивка ------------
const portfolioRef = ref(null)
const lines        = ref([])
const visibleLines = ref([])
const lineRefs     = ref([])
const observers    = ref([])

const template = [
  [
    { photoClass: 'Normal', blockClass: 'ContentBlock AnimationShow' },
    { photoClass: 'Medium', blockClass: 'ContentBlock BigPicture AnimationShow' },
  ],
  [
    { photoClass: 'Normal', blockClass: 'ContentBlock AnimationShow' },
    { photoClass: 'Normal', blockClass: 'ContentBlock BigPicture AnimationShow' },
    { photoClass: 'Normal', blockClass: 'ContentBlock BigPicture AnimationShow' },
  ],
  [
    { photoClass: 'Big',    blockClass: 'ContentBlock AnimationShow' },
  ],
]

// Строит структуру lines из API-ответа
function buildLines(apiData) {
  const groups = apiData.reduce((acc, item) => {
    (acc[item.photo_class] = acc[item.photo_class] || []).push(item)
    return acc
  }, {})

  return template.map(row =>
    row.map(slot => {
      const project = (groups[slot.photoClass] || []).shift() || {}
      return {
        photoClass: slot.photoClass,
        projectName: project.project_name || 'NoName',
        info:        project.info         || 'NoInfo',
        img:         project.imageUrl     ? `url(${project.imageUrl})` : '',
        logoImg:     project.logoUrl      ? `url(${project.logoUrl})`  : `url(/default-logo.png)`,
        blockClass:  slot.blockClass,
      }
    })
  )
}

// Помогает собрать refs из v-for
const setLineRef = (el, idx) => {
  if (el) lineRefs.value[idx] = el
}

onMounted(async () => {
  // --- A) Загрузка данных и инициализация lines/visibleLines ---
  try {
    const { data } = await axios.get('http://localhost:8000/api/portfolio-projects/')
    lines.value = buildLines(data)
  } catch (err) {
    console.error('Ошибка при загрузке портфолио:', err)
    lines.value = buildLines([])
  }
  visibleLines.value = Array(lines.value.length).fill(false)

  // Ждём, чтобы Vue добавил все containers в DOM
  await nextTick()

  // --- B) Observer для фона PortfolioBlock ---
  if (portfolioRef.value) {
    const portObs = new IntersectionObserver(
      ([entry]) => {
        portfolioRef.value.style.backgroundColor =
          entry.isIntersecting ? '#08080a' : '#15171b'
      },
      { threshold: 0.38 }
    )
    portObs.observe(portfolioRef.value)
    observers.value.push(portObs)
  }

  // --- C) Observer для каждой строки lines ---
  lineRefs.value.forEach((el, idx) => {
    if (!el) return
    const obs = new IntersectionObserver(
      ([entry], observer) => {
        if (entry.isIntersecting) {
          visibleLines.value[idx] = true
          observer.unobserve(el)
        }
      },
      { threshold: 0.3 }
    )
    obs.observe(el)
    observers.value.push(obs)
  })
})

onUnmounted(() => {
  observers.value.forEach(o => o.disconnect())
})
</script>