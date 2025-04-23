<template>

  <header>
    <HeaderComponent/> 
  </header>

  <main>
    <StartPage/>
    <AboutMe/>
    <Portfolio/>
    <Service/>
    <Contacts/>
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
import { ref, provide, onMounted, onUnmounted } from 'vue';
import AboutMe from "./components/AboutMe.vue";
import StartPage from "./components/StartPage.vue";
import HeaderComponent from "./components/HeaderComponent.vue";
import Portfolio from "./components/MyProduct.vue";
import Service from "./components/ServiceComponent.vue";
import Contacts from "./components/ContactsComponent.vue"
import Lenis from 'lenis';

export default {
  components: {
    HeaderComponent,
    StartPage,
    AboutMe,
    Portfolio,
    Service,
    Contacts,
  },

  setup() {
    const scrollY = ref(0);

    const handleScroll = () => {
      scrollY.value = window.scrollY;
    };

    // Плавный скролл через Lenis
    const lenis = new Lenis({
      duration: 1.8,
      easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
      smooth: true,
      smoothTouch: false,
    });

    const raf = (time) => {
      lenis.raf(time);
      requestAnimationFrame(raf);
    };

    onMounted(() => {
      window.addEventListener("scroll", handleScroll);
      requestAnimationFrame(raf);
    });

    onUnmounted(() => {
      window.removeEventListener("scroll", handleScroll);
    });

    provide('scrollData', { scrollY, handleScroll });

    return {};
  },
};
</script>
