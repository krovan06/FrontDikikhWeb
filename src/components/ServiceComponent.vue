<template>
  <div class="ServiceComponentHolst">
    <div class="ServiceContainer section" id="services-section">

      <div class="ServiceInteractive">
        <div class="slider-viewport">
          <div
            class="slides-wrapper"
            :style="{ transform: `translateX(-${activeIndex * 100}%)` }"
          >
            <div
              class="slide"
              v-for="(slide, idx) in serviceNames"
              :key="idx"
              :class="{ active: idx === activeIndex, inactive: idx !== activeIndex }"
            >
              Контент слайда «{{ slide }}»
            </div>
          </div>
        </div>
      </div>

      <div class="ServiceContentContainer">
        <div class="Line" v-for="(item, idx) in serviceNames" :key="idx">
          <div class="Service" @mouseenter="setActive(idx)">
            <p class="ServiceText poppins-text">{{ item }}</p>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'ServiceSlider',
  data() {
    return {
      serviceNames: ['Бэкенд', 'Фронтенд', 'Дизайн', 'Сайт под ключ'],
      activeIndex: 0,
    };
  },
  methods: {
    setActive(idx) {
      this.activeIndex = idx;
    },
  },
};
</script>

<style scoped>
.ServiceComponentHolst {
  width: 100%;
  min-height: 130vh;
  overflow: hidden;
  background-color: black;
  display: flex;
  justify-content: center;
}

.ServiceContainer {
  width: 90%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.ServiceInteractive {
  z-index: 1;
  position: relative; 
  width: 100%;
  height: 60vh;
}

.slider-viewport {
  position: absolute;
  top: 0;
  right: 0;
  width: 50%;
  height: 100%;
  z-index: 0; 
}

.slides-wrapper {
  display: flex;
  align-items: center;
  height: 100%;
  width: 50%;
  gap: 100px;
  transition: transform 0.5s ease;
}

.slide {
  flex: 0 0 100%;
  display: flex;
  height: 75%;
  border-radius: 20px;
  justify-content: center;
  align-items: center;
  background: rgba(208, 212, 220, 0.655);
  color: rgb(21, 23, 27);
  font-size: 24px;
  transition: 0.3s;
}

.slide.inactive {
  opacity: 0.5;
  transform: scale(0.9);
  filter: blur(5px);
}

.slide.active {
  opacity: 1;
  transform: scale(1);
}

.ServiceContentContainer {
  width: 80%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: absolute;
  z-index: 2;
}

.Line {
  width: 100%;
  height: 15vh;
  display: flex;
  align-items: center;
  justify-content: center;
  border-top: 1px solid rgb(78, 78, 78);
  position: relative;
}

.Service {
  width: 99%;
  height: 90%;
  display: flex;
  justify-content: left;
  align-items: center;
  transition: 0.3s;
  position: relative;
}

.Service:hover {
  width: 98%;
}

.ServiceText {
  color: rgba(208, 212, 220, 0.655);
  font-size: 48px;
  line-height: 72px;
  transition: 0.3s;
}

.Service:hover .ServiceText {
  text-shadow: 0px 0px 14px rgba(255, 255, 255, 0.58);
  color: rgb(208, 212, 220);
}

/* шрифты */
.oswald-text {
  font-family: 'Oswald', sans-serif;
  font-weight: 500;
}

.poppins-text {
  font-family: 'Poppins', sans-serif;
  font-weight: 600;
  font-style: normal;
}
</style>
