<template>
  <div class="HeaderColumn" ref="headerColumn">
    <div class="HeaderBlock" id="ElasticBlock">
      <div class="SelectCircle"></div>
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

.SelectCircle {
  position: absolute;
  border-radius: 100%;
  width: calc((1vh + 1vw) * 0.32);
  height: calc((1vh + 1vw) * 0.32);
  margin-top: 8.5%;
  left: 0;
  transform: translateX(-50%);
  opacity: 0;
  transition: transform 1s ease-in-out,
              opacity 0.3s ease,
              background-color 0.5s ease;
  will-change: transform, opacity, background-color;
}

.AboutMe,
.Portfolio-link,
.Services-link,
.Contacts-link {
  text-decoration: none;
  font-size: calc((1vh + 1vw) * 0.9);
  white-space: nowrap;
  transition: opacity 0.3s ease-in-out;
}

.AboutMe { animation-delay: 0.5s; }
.Portfolio-link { animation-delay: 0.8s; }
.Services-link { animation-delay: 1.1s; }
.Contacts-link { animation-delay: 1.4s; }

@keyframes ShowInUp {
  from { margin-bottom: 5vh; opacity: 0; }
  to { margin-bottom: 0; opacity: 1; }
}

.oswald-text {
  font-family: 'Oswald', sans-serif;
  font-weight: 400;
  transition: color 0.5s ease-in-out;
}

.text-light {
  color: #ffffff;
}

.text-dark {
  color: #000000;
}

.bg-light {
  background-color: #ffffff;
}

.bg-dark {
  background-color: #000000;
}

.fade {
  opacity: 0.7;
}

@keyframes fadeIn {
  from { opacity: 0.7; }
  to { opacity: 1; }
}
</style>

<script>
import { throttle, debounce } from 'lodash';

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
      isHovering: false,
      currentColorState: null,
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
      this.moveCircleToLink(link);
      this.scrollToSection(link);
    },

    scrollToSection(link) {
      const targetId = this.getSectionId(link);
      const target = document.getElementById(targetId);
      target?.scrollIntoView({ behavior: 'smooth' });
    },

    moveCircleToLink(link) {
      if (!this.isMounted) return;
      
      const circle = this.$el.querySelector('.SelectCircle');
      const linkElement = this.getLinkElement(link);
      
      if (!circle || !linkElement) {
        console.warn('Элементы не найдены:', { circle, linkElement });
        return;
      }

      const linkRect = linkElement.getBoundingClientRect();
      const headerRect = this.$el.querySelector('.HeaderBlock').getBoundingClientRect();
      const position = linkRect.left - headerRect.left + linkRect.width / 2;

      circle.style.transform = `translateX(${position}px)`;
      circle.style.opacity = '1';
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

    returnToSelected() {
      if (this.isHovering || !this.selectedLink || !this.isMounted) return;
      this.moveCircleToLink(this.selectedLink);
    },

    getBackgroundColor() {
      const header = this.$refs.headerColumn;
      if (!header) {
        console.warn('Шапка не найдена');
        return 'rgb(255, 255, 255)';
      }

      const rect = header.getBoundingClientRect();
      const x = rect.left + rect.width / 2;
      const y = rect.bottom + 1;

      const originalDisplay = header.style.display;
      header.style.display = 'none';

      try {
        let element = document.elementFromPoint(x, y);
        if (!element) {
          return 'rgb(255, 255, 255)';
        }

        let bgColor = window.getComputedStyle(element).backgroundColor;
        while (bgColor === 'rgba(0, 0, 0, 0)' && element.parentElement) {
          element = element.parentElement;
          bgColor = window.getComputedStyle(element).backgroundColor;
        }

        return bgColor || 'rgb(255, 255, 255)';
      } finally {
        header.style.display = originalDisplay;
      }
    },

    getContrastColor(bgColor) {
      if (!bgColor || typeof bgColor !== 'string') {
        console.warn('Недопустимый цвет фона:', bgColor);
        return '#000000';
      }

      const rgbMatch = bgColor.match(/\d+/g);
      if (!rgbMatch || rgbMatch.length < 3) {
        console.warn('Не удалось разобрать RGB из:', bgColor);
        return '#000000';
      }

      const [r, g, b] = rgbMatch.map(Number);
      const brightness = (r * 299 + g * 587 + b * 114) / 1000;
      return brightness > 128 ? '#000000' : '#ffffff';
    },

    updateHeaderColors: throttle(function() {
      if (!this.isMounted) return;

      const bgColor = this.getBackgroundColor();
      const textColor = this.getContrastColor(bgColor);
      const isLight = textColor === '#ffffff';

      if (this.currentColorState === isLight) return;
      this.currentColorState = isLight;

      const links = this.$el.querySelectorAll('.oswald-text');
      const circle = this.$el.querySelector('.SelectCircle');

      links.forEach(el => {
        el.classList.remove('text-light', 'text-dark');
        el.classList.add(isLight ? 'text-light' : 'text-dark');
      });

      if (circle) {
        circle.classList.remove('bg-light', 'bg-dark');
        circle.classList.add(isLight ? 'bg-light' : 'bg-dark');
      }

      console.log('Цвет фона:', bgColor, 'isLight:', isLight);
    }, 200),

    handleScroll() {
      if (!this.isMounted) return;

      const links = this.$el.querySelectorAll('.oswald-text');
      links.forEach(el => {
        el.classList.add('fade');
        el.style.animation = 'none';
      });

      clearTimeout(this.scrollTimeout);
      this.isScrolling = true;

      this.scrollTimeout = setTimeout(() => {
        this.isScrolling = false;
        links.forEach(el => {
          el.classList.remove('fade');
          el.style.animation = 'fadeIn 0.3s ease-in-out forwards';
        });
      }, 150);
    },

    updateCirclePosition: debounce(function(link) {
      if (!this.isMounted || !link) return;
      this.selectedLink = link;
      this.moveCircleToLink(link);
      console.log('Шар перемещён к:', link);
    }, 300),

    handleIntersection(entries) {
      if (!this.isMounted) return;

      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const link = this.getLinkFromTarget(entry.target);
          if (link && link !== this.selectedLink) {
            this.updateCirclePosition(link);
          }
        }
      });
    },

    getLinkFromTarget(target) {
      return {
        'about-text': 'about',
        'services-section': 'services',
        'portfolio-section': 'portfolio',
        'contacts-section': 'contacts'
      }[target.id] || null;
    }
  },
  mounted() {
    this.isMounted = true;
    console.log('Шапка смонтирована');

    this.$nextTick(() => {
      const links = this.$el.querySelectorAll('.oswald-text');
      links.forEach(el => {
        el.classList.add('text-dark');
        el.style.animation = 'ShowInUp 2.5s forwards';
      });

      this.selectedLink = 'about';
      this.moveCircleToLink('about');

      this.updateHeaderColors();
      this.scrollListener = () => {
        this.handleScroll();
        this.updateHeaderColors();
      };
      this.resizeListener = () => this.updateHeaderColors();
      window.addEventListener('scroll', this.scrollListener);
      window.addEventListener('resize', this.resizeListener);
    });

    this.$el.querySelectorAll('.oswald-text').forEach(link => {
      link.addEventListener('mouseenter', () => {
        const linkType = [...link.classList].find(cls => 
          ['AboutMe', 'Services-link', 'Portfolio-link', 'Contacts-link'].includes(cls)
        );
        
        if (!linkType) return;
        
        const linkName = linkType.replace(/-link$/, '').toLowerCase();
        this.isHovering = true;
        this.moveCircleToLink(linkName);
      });
      
      link.addEventListener('mouseleave', () => {
        this.isHovering = false;
        this.returnToSelected();
      });
    });

    const sections = document.querySelectorAll(
      '#about-text, #services-section, #portfolio-section, #contacts-section'
    );
    
    if (sections.length) {
      this.observer = new IntersectionObserver(this.handleIntersection, {
        threshold: 0.2
      });
      
      sections.forEach(section => this.observer.observe(section));
    } else {
      console.warn('Секции не найдены.');
    }
  },
  beforeUnmount() {
    this.isMounted = false;
    this.observer?.disconnect();
    window.removeEventListener('scroll', this.scrollListener);
    window.removeEventListener('resize', this.resizeListener);
  }
};
</script>