<template>
  <div class="ContactsHolst" ref="contactsHolst">
    <div class="ContactsBlock">
      <div class="ContactsContentBlock">
        <!-- Контент блока -->
      </div>
      <div class="AvtorName">
        <div class="AvtorText poppins-text">
          <span
            v-for="(letter, index) in letters"
            :key="index"
            class="letter"
            :style="{
              transform: `translateY(${getTranslateY(index)}px)`,
              opacity: getOpacity(index),
              transition: `transform 0.5s ease-out, opacity 0.5s ease-out`
            }"
          >
            {{ letter }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ContactsHolst {
  width: 100%;
  height: 100vh;
  background-color: black;
  position: relative;
  overflow: hidden;
}

.ContactsBlock {
  width: 100%;
  border-radius: 25px;
  height: 90%;
  margin-top: 1vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(231, 244, 255);
}

.AvtorName {
  display: flex;
  height: 50vh;
  justify-content: center;
  margin-top: 70vh;
  position: absolute;
}

.AvtorText {
  display: flex;
  color: rgb(21, 23, 27);
  font-size: calc((1vh + 1vw) * 17);
  letter-spacing: calc((1vh + 1vw) * 1.5);
}

.letter {
  display: inline-block;
}

.poppins-text {
  font-family: 'Poppins', Arial, sans-serif;
  font-weight: 600;
  font-style: normal;
}
</style>

<script>
import { inject, ref, onMounted } from 'vue';

export default {
  name: 'StartPage',
  setup() {
    const scrollY = inject("scrollData");
    const contactsHolst = ref(null);
    const letters = ['D', 'I', 'K', 'I', 'K', 'H'];
    const numLetters = letters.length;
    const speeds = Array.from({ length: numLetters }, (_, i) => -0.1 - 0.02 * i); // Разные скорости для параллакса
    let componentHeight = ref(0);

    onMounted(() => {
      // Получаем высоту компонента
      componentHeight.value = contactsHolst.value.offsetHeight;
      // Обновляем высоту при изменении размера окна
      window.addEventListener('resize', () => {
        componentHeight.value = contactsHolst.value.offsetHeight;
      });
    });

    const getTranslateY = (index) => {
      const maxScroll = componentHeight.value; // Максимальная прокрутка внутри компонента
      const translateY = speeds[index] * scrollY.value;
      // Ограничиваем движение, чтобы буквы выровнялись внизу
      if (scrollY.value >= maxScroll) {
        return 0; // Все буквы выровнены в конце
      }
      return Math.min(translateY, 100); // Ограничиваем движение вниз
    };

    const getOpacity = (index) => {
      const fadeStart = componentHeight.value - numLetters * 100 + index * 100;
      const fadeDistance = 100;
      if (scrollY.value < fadeStart) {
        return 0; // Буква невидима
      } else if (scrollY.value >= fadeStart + fadeDistance) {
        return 1; // Буква полностью видима
      } else {
        return (scrollY.value - fadeStart) / fadeDistance; // Плавное появление
      }
    };

    return {
      scrollY,
      letters,
      getTranslateY,
      getOpacity,
      contactsHolst,
    };
  },
};
</script>