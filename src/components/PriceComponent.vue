<template>
  <div class="PriceBlock">
    <div class="PriceContentContainer">
      <div class="PriceContentColumn section" id="price-section">
        <div class="PriceTextBlock" v-for="block in priceBlocks" :key="block.id">
          <div class="PriceText">
            <h1 class="BigText poppins-text">{{ block.title }}</h1>
            <p class="SmallText oswald-text">{{ block.main_text }}</p>
            <Transition name="slide">
              <div v-if="block.isAdditionalTextVisible" class="additional-text-wrapper">
                <p class="SmallText oswald-text additional-text">{{ block.additional_text }}</p>
              </div>
            </Transition>
            <p class="PriceLine oswald-text">~{{ block.price }} руб.</p>
            <button class="MoreInfomation" @click="toggleText(block)">
              <p class="BtnText oswald-text">{{ block.isAdditionalTextVisible ? 'Скрыть' : 'Узнать больше' }}</p>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  .PriceBlock {
    width: 100%;
    min-height: 100vh;
    background-color: rgb(21, 23, 27);
  }

  .PriceContentContainer {
    width: 100%;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .PriceContentColumn {
    position: relative;
    width: 90%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .PriceTextBlock {
    width: 60%;
    margin-top: 10vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .PriceText {
    flex-direction: column;
    display: flex;
    position: relative;
    justify-content: left;
    /*align-items: center;*/
    padding-top: 5%;
    padding-bottom: 5%;
    gap: calc((1vh + 1vw) * 0.32);
    width: 100%;
    border-bottom: 2px solid aliceblue;
  }

  .BigText {
    text-align: left;
    color: rgb(208, 212, 220);
    font-size: calc((1vw + 1vh) * 1.8);
  }

  .SmallText {
    color: rgb(208, 212, 220);
    font-size: calc((1vw + 1vh) * 0.7);
    line-height: calc((1vh + 1vw) * 2.4);
    letter-spacing: calc((1vh + 1vw) * 0.25);
    text-align: left;
  }

  .PriceLine {
    margin-top: 5%;
    color: rgb(208, 212, 220);
    font-size: 20px;
  }

  .MoreInfomation {
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: left;
    margin-top: 3%;
    background-color: rgb(21, 23, 27);
    width: 20%;
    height: 5vh;
    transition: 0.8s;
  }

  .MoreInfomation:hover {
    text-shadow: 0px 0px 14px rgb(205, 211, 224);
    border-bottom: 1px solid aliceblue;
  }

  .MoreInfomation:active {
    transform: scale(0.98);
  }

  .BtnText {
    color: aliceblue;
    font-size: calc((1vh + 1vw) * 0.8);
  }

  .additional-text-wrapper {
    overflow: hidden;
  }

  .slide-enter-active,
  .slide-leave-active {
    transition: max-height 1.5s ease;
  }

  .slide-enter-from,
  .slide-leave-to {
    max-height: 0;
  }

  .slide-enter-to,
  .slide-leave-from {
    max-height: 500px;
  }

  .oswald-text {
    font-family: 'Oswald', sans-serif;
    font-weight: 400;
  }

  .poppins-text {
    font-family: 'Poppins', sans-serif;
    font-weight: 700;
    font-style: normal;
  }
</style>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      priceBlocks: [] // Массив для хранения блоков из API
    };
  },
  mounted() {
    this.fetchPriceBlocks(); // Загружаем данные при монтировании компонента
  },
  methods: {
    async fetchPriceBlocks() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/price-blocks/');
        this.priceBlocks = response.data.map(block => ({
          ...block,
          isAdditionalTextVisible: false // Добавляем флаг для управления видимостью текста
        }));
      } catch (error) {
        console.error('Ошибка при загрузке блоков:', error);
      }
    },
    toggleText(block) {
      block.isAdditionalTextVisible = !block.isAdditionalTextVisible; // Переключаем видимость для конкретного блока
    }
  }
};
</script>