<template>
  <div class="HeaderColumn">
    <div class="HeaderBlock" id="ElasticBlock">
      <div class="SelectCircle"></div>
      <a href="#" @click.prevent="selectLink('about')" class="AboutMe oswald-text">ОБО МНЕ</a>
      <a href="#" @click.prevent="selectLink('price')" class="Price oswald-text">ЦЕНЫ</a>
      <a href="#" @click.prevent="selectLink('portfolio')" class="Portfolio-link oswald-text">ПОРТФОЛИО</a>
    </div>
  </div>
</template>

<style scoped>
.HeaderColumn {
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
  background-color: rgb(231, 244, 255);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 50px;
}

.SelectCircle {
  position: absolute;
  border-radius: 100%;
  background-color: black;
  width: calc((1vh + 1vw) * 0.32);
  height: calc((1vh + 1vw) * 0.32);
  margin-top: 12.5%;
  left: 0;
  transform: translateX(-50%);
  opacity: 0;
  transition: left 0.3s ease, opacity 0.3s ease;
}

.AboutMe,
.Portfolio-link,
.Price {
  opacity: 0;
  text-decoration: none;
  color: rgb(21, 23, 27);
  font-size: calc((1vh + 1vw) * 0.9); /* Уменьшаем размер шрифта */
  white-space: nowrap; /* Запрещаем перенос текста */
  transition: 0.2s;
  animation: ShowInUp 2.5s forwards;
}

.AboutMe {
  animation-delay: 0.5s;
}

.Price {
  animation-delay: 0.8s;
}

.Portfolio-link {
  animation-delay: 1.1s;
}

@keyframes ShowInUp {
  from {
    margin-bottom: 5vh;
    opacity: 0;
  }
  to {
    margin-bottom: 0;
    opacity: 1;
  }
}

.oswald-text {
  font-family: 'Oswald', sans-serif;
  font-weight: 400;
}
</style>

<script>
export default {
  data() {
    return {
      selectedLink: null, // Последняя выбранная ссылка
      observer: null,     // Наблюдатель для секций
    };
  },
  methods: {
    // Обработчик клика по ссылке
    selectLink(link) {
      this.selectedLink = link;
      this.moveCircleToLink(link);
      this.scrollToSection(link);
    },

    // Прокрутка к нужной секции
    scrollToSection(link) {
      let targetId;
      switch (link) {
        case 'about':
          targetId = 'about-text';
          break;
        case 'price':
          targetId = 'price-section';
          break;
        case 'portfolio':
          targetId = 'portfolio-section'; // ID для секции портфолио
          break;
        default:
          console.warn(`Неизвестная ссылка: ${link}`);
          return;
      }
      const target = document.getElementById(targetId);
      if (target) {
        target.scrollIntoView({ behavior: 'smooth' });
      } else {
        console.warn(`Элемент с id "${targetId}" не найден`);
      }
    },

    // Перемещает шарик к ссылке
    moveCircleToLink(link) {
      const circle = this.$el.querySelector('.SelectCircle');
      let linkElement;
      switch (link) {
        case 'about':
          linkElement = this.$el.querySelector('.AboutMe');
          break;
        case 'price':
          linkElement = this.$el.querySelector('.Price');
          break;
        case 'portfolio':
          linkElement = this.$el.querySelector('.Portfolio-link');
          break;
        default:
          return;
      }
      if (linkElement && circle) {
        const linkRect = linkElement.getBoundingClientRect();
        const headerRect = this.$el.querySelector('.HeaderBlock').getBoundingClientRect();
        const leftPosition = linkRect.left - headerRect.left + linkRect.width / 2;
        circle.style.left = `${leftPosition}px`;
        circle.style.opacity = '1'; // Показываем шарик
      }
    },

    // При уходе курсора возвращаем шарик к последней выбранной ссылке
    returnToSelected() {
      if (this.selectedLink) {
        this.moveCircleToLink(this.selectedLink);
      }
    },

    // Обработчик пересечения секций с viewport
    handleIntersection(entries) {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          let link;
          if (entry.target.id === 'about-text') {
            link = 'about';
          } else if (entry.target.id === 'price-section') {
            link = 'price';
          } else if (entry.target.id === 'portfolio-section') {
            link = 'portfolio';
          }
          if (link && link !== this.selectedLink) {
            this.selectedLink = link;
            this.moveCircleToLink(link);
          }
        }
      });
    },
  },
  mounted() {
    // Добавляем события при наведении для плавного перемещения шарика
    const links = this.$el.querySelectorAll('.oswald-text');
    links.forEach((link) => {
      link.addEventListener('mouseenter', () => {
        const linkClass = link.classList.contains('AboutMe') ? 'about' :
                         link.classList.contains('Price') ? 'price' : 'portfolio';
        this.moveCircleToLink(linkClass);
      });
      link.addEventListener('mouseleave', this.returnToSelected);
    });

    // Наблюдаем за секциями
    const sections = document.querySelectorAll('.section');
    if (sections.length) {
      this.observer = new IntersectionObserver(this.handleIntersection, { threshold: 0.5 });
      sections.forEach((section) => {
        this.observer.observe(section);
      });
    } else {
      console.warn('Секции с классом .section не найдены.');
    }
  },
  beforeUnmount() {
    if (this.observer) {
      this.observer.disconnect();
    }
  },
};
</script>