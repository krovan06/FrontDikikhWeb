<template>
  <div class="HeaderColumn" ref="headerColumn">
    <div class="HeaderBlock" id="ElasticBlock">
      <a href="#" @click.prevent="selectLink('about')" class="AboutMe oswald-text">ОБО МНЕ</a>
      <a href="#" @click.prevent="selectLink('portfolio')" class="Portfolio-link oswald-text">ПОРТФОЛИО</a>
      <a href="#" @click.prevent="selectLink('services')" class="Services-link oswald-text">УСЛУГИ</a>
      <a href="#" @click.prevent="selectLink('contacts')" class="Contacts-link oswald-text">КОНТАКТЫ</a>
    </div>
  </div>
</template>

<style scoped>
.HeaderColumn {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  width: 100vw;
  height: 10vh;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.HeaderBlock {
  position: relative;
  min-width: 15%;
  padding-left: 20px;
  padding-right: 20px;
  height: 65%;
  border-radius: 15px;
  backdrop-filter: blur(4px);
  background: rgba(208, 212, 220, 0.06);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 50px;
  transition: color 0.5s ease, background-color 0.5s ease;
}

.oswald-text {
  font-family: 'Oswald', sans-serif;
  font-weight: 400;
  transition: color 0.5s ease-in-out;
  text-decoration: none;
  font-size: calc((1vh + 1vw) * 0.9);
  white-space: nowrap;
  transition: opacity 0.3s ease-in-out;
}

.text-light { color: #ffffff; }
.text-dark { color: #000000; }

.fade { opacity: 0.7; }

.active-link {
  text-shadow: 0px 0px 14px rgba(255, 255, 255, 0.58);
  color: rgb(125, 127, 131);
}

@keyframes ShowInUp {
  from { margin-bottom: 5vh; opacity: 0; }
  to { margin-bottom: 0; opacity: 1; }
}

@keyframes fadeIn {
  from { opacity: 0.7; }
  to { opacity: 1; }
}
</style>

<script>
import { throttle } from 'lodash';

export default {
  data() {
    return {
      selectedLink: null,
      observer: null,
      isMounted: false,
      scrollListener: null,
      resizeListener: null,
      isScrolling: false,
      scrollTimeout: null,
    };
  },
  methods: {
    selectLink(link) {
      if (!this.isMounted) return;
      
      const linkElement = this.getLinkElement(link);
      if (!linkElement) {
        console.warn(`Элемент ${link} не найден`);
        return;
      }

      this.selectedLink = link;
      this.scrollToSection(link);
      this.updateActiveLink();
    },

    scrollToSection(link) {
      const targetId = this.getSectionId(link);
      const target = document.getElementById(targetId);
      target?.scrollIntoView({ behavior: 'smooth' });
    },

    getSectionId(link) {
      return {
        about: 'about-text',
        services: 'services-section',
        portfolio: 'portfolio-section',
        contacts: 'contacts-section'
      }[link] || null;
    },

    getLinkElement(link) {
      const selectorMap = {
        about: '.AboutMe',
        services: '.Services-link',
        portfolio: '.Portfolio-link',
        contacts: '.Contacts-link'
      };
      const selector = selectorMap[link];
      return selector ? this.$el.querySelector(selector) : null;
    },

    getBackgroundColor() {
      const header = this.$refs.headerColumn;
      if (!header) return 'rgb(255, 255, 255)';

      const rect = header.getBoundingClientRect();
      const x = rect.left + rect.width / 2;
      const y = rect.bottom + 1;

      const originalVisibility = header.style.visibility;
      header.style.visibility = 'hidden';

      try {
        let element = document.elementFromPoint(x, y);
        if (!element) return 'rgb(255, 255, 255)';

        let bgColor = window.getComputedStyle(element).backgroundColor;
        while (bgColor === 'rgba(0, 0, 0, 0)' && element.parentElement) {
          element = element.parentElement;
          bgColor = window.getComputedStyle(element).backgroundColor;
        }

        return bgColor || 'rgb(255, 255, 255)';
      } finally {
        header.style.visibility = originalVisibility;
      }
    },

    getContrastColor(bgColor) {
      if (!bgColor) return '#000000';
      
      const rgbMatch = bgColor.match(/\d+/g);
      if (!rgbMatch || rgbMatch.length < 3) return '#000000';

      const [r, g, b] = rgbMatch.map(Number);
      const brightness = (r * 299 + g * 587 + b * 114) / 1000;
      return brightness > 128 ? '#000000' : '#ffffff';
    },

    updateHeaderColors: throttle(function() {
      if (!this.isMounted) return;

      const bgColor = this.getBackgroundColor();
      const textColor = this.getContrastColor(bgColor);
      const isLight = textColor === '#ffffff';

      this.$el.querySelectorAll('.oswald-text').forEach(el => {
        el.classList.remove('text-light', 'text-dark');
        el.classList.add(isLight ? 'text-light' : 'text-dark');
      });
    }, 200),

    handleScroll() {
      if (!this.isMounted) return;

      this.isScrolling = true;
      clearTimeout(this.scrollTimeout);

      this.scrollTimeout = setTimeout(() => {
        this.isScrolling = false;
        this.updateHeaderColors();
      }, 150);
    },

    handleIntersection(entries) {
      let bestEntry = null;
      let maxRatio = 0;
      entries.forEach(entry => {
        if (entry.isIntersecting && entry.intersectionRatio > maxRatio) {
          maxRatio = entry.intersectionRatio;
          bestEntry = entry;
        }
      });
      if (bestEntry) {
        const link = this.getLinkFromTarget(bestEntry.target);
        if (link) {
          this.selectedLink = link;
          this.updateActiveLink();
        }
      }
    },

    getLinkFromTarget(target) {
      return {
        'about-text': 'about',
        'services-section': 'services',
        'portfolio-section': 'portfolio',
        'contacts-section': 'contacts'
      }[target.id] || null;
    },

    updateActiveLink() {
      this.$el.querySelectorAll('.oswald-text').forEach(el => {
        el.classList.remove('active-link');
      });
      const activeLink = this.getLinkElement(this.selectedLink);
      activeLink?.classList.add('active-link');
    },
  },
  mounted() {
    this.isMounted = true;

    this.$nextTick(() => {
      const links = this.$el.querySelectorAll('.oswald-text');
      links.forEach(el => {
        el.classList.add('text-dark');
        el.style.animation = 'ShowInUp 2.5s forwards';
      });

      this.scrollListener = () => {
        this.handleScroll();
        this.updateHeaderColors();
      };
      
      this.resizeListener = () => this.updateHeaderColors();
      
      window.addEventListener('scroll', this.scrollListener);
      window.addEventListener('resize', this.resizeListener);

      this.observer = new IntersectionObserver(this.handleIntersection.bind(this), {
        threshold: 0.2
      });

      const sections = document.querySelectorAll('#about-text, #services-section, #portfolio-section, #contacts-section');
      
      sections.forEach(section => this.observer.observe(section));

      // Force initial check
      window.dispatchEvent(new Event('resize'));
    });
  },
  beforeUnmount() {
    this.isMounted = false;
    this.observer?.disconnect();
    window.removeEventListener('scroll', this.scrollListener);
    window.removeEventListener('resize', this.resizeListener);
  }
};
</script>