<template>

  <header>
    <HeaderComponent/> 
  </header>

  <main>
    <StartPage/>
    <AboutMe/>
    <PriceComponent/>
    <Portfolio/>
  </main>
</template>

<style>
  * {
    margin: 0;
    padding: 0;
  }

  header {
    position: fixed;
    z-index: 1000;
  }

</style>

<script>
  import { ref, provide, onUnmounted, handleError } from 'vue';
  import AboutMe from "./components/AboutMe.vue";
  import StartPage from "./components/StartPage.vue";
  import HeaderComponent from "./components/HeaderComponent.vue";
  import PriceComponent from "./components/PriceComponent.vue";
  import Portfolio from "./components/MyProduct.vue";


  export default {

    setup() {
      const scrollY = ref(0);

      const handleScroll = () => {
        scrollY.value = window.scrollY;
      };

      provide('scrollData', { scrollY, handleScroll });
      window.addEventListener("scroll", handleScroll);
      
      onUnmounted( () => {
        window.removeEventListener("scroll", handleError);
      });

      return {};
    },

    components: {
      HeaderComponent,
      StartPage,
      AboutMe,
      PriceComponent,
      Portfolio,
    }
  };
</script>