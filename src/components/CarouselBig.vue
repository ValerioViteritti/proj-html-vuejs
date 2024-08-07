<script>
export default {
  name: 'CarouselBig',
  data() {
    return {
      currentSlide: 0, // Slide corrente
      slides: [
        {  
          image: '/src/assets/slider.jpg',
          id: 1
        },
        {  
          image: '/src/assets/H3-slider3.jpg',
          id: 2
        },
      ],
      isHovered: false, // Flag per indicare se il carosello è in stato di hover
      isNavigating: false, // Flag per indicare se è in corso una navigazione tra slide
      isAboutUsHovered: false, // Flag per hover sul pulsante "About Us"
      isOurHistoryHovered: false, // Flag per hover sul pulsante "Our History"
      autoSlideInterval: null, // Intervallo per il cambio automatico delle slide
      hoverTimeout: null, // Timeout per l'hover
      navigateTimeout: null, // Timeout per la navigazione
    };
  },
  mounted() {
    // Avvia il cambio automatico delle slide dopo 6 secondi
    this.startAutoSlide(6000); 
  },
  beforeDestroy() {
    // Pulisce tutti i timer quando il componente viene distrutto
    this.clearAllTimers(); 
  },
  methods: {
    // Avvia il cambio automatico delle slide
    startAutoSlide(interval) {
      this.clearAutoSlideTimer();
      this.autoSlideInterval = setInterval(() => {
        if (!this.isNavigating) {
          this.nextSlide();
        }
      }, interval);
    },
    // Resetta il timer per il cambio automatico delle slide
    resetAutoSlideTimer(interval) {
      this.clearAutoSlideTimer();
      this.startAutoSlide(interval);
    },
    // Gestisce l'evento di mouse over sul carosello
    handleMouseOver() {
      this.isHovered = true;
      this.resetAutoSlideTimer(8000);
    },
    // Gestisce l'evento di mouse leave sul carosello
    handleMouseLeave() {
      this.isHovered = false;
      this.resetAutoSlideTimer(6000);
    },
    // Passa alla slide successiva
    nextSlide() {
      this.currentSlide = (this.currentSlide + 1) % this.slides.length;
      this.handleNavigation();
    },
    // Torna alla slide precedente
    prevSlide() {
      this.currentSlide = (this.currentSlide - 1 + this.slides.length) % this.slides.length;
      this.handleNavigation();
    },
    // Gestisce la navigazione tra le slide
    handleNavigation() {
      this.isNavigating = true;
      this.clearNavigateTimeout();
      this.navigateTimeout = setTimeout(() => {
        this.isNavigating = false;
        this.resetAutoSlideTimer(this.isHovered ? 8000 : 6000);
      }, 10000);
    },
    // Naviga alla sezione "About Us"
    goToAboutUs() {
      console.log('Navigating to About Us');
      this.$router.push({ name: 'About' });
    },
    // Naviga alla sezione "Our History"
    goToOurHistory() {
      console.log('Navigating to Our History');
      this.$router.push({ name: 'Contact' });
    },
    // Pulisce il timer per il cambio automatico delle slide
    clearAutoSlideTimer() {
      if (this.autoSlideInterval) {
        clearInterval(this.autoSlideInterval);
        this.autoSlideInterval = null;
      }
    },
    // Pulisce il timeout per la navigazione
    clearNavigateTimeout() {
      if (this.navigateTimeout) {
        clearTimeout(this.navigateTimeout);
        this.navigateTimeout = null;
      }
    },
    // Pulisce tutti i timer
    clearAllTimers() {
      this.clearAutoSlideTimer();
      if (this.hoverTimeout) {
        clearTimeout(this.hoverTimeout);
        this.hoverTimeout = null;
      }
      this.clearNavigateTimeout();
    },
    // Gestisce l'hover sul pulsante "About Us"
    handleAboutUsMouseOver() {
      this.isAboutUsHovered = true;
    },
    // Gestisce il leave dell'hover sul pulsante "About Us"
    handleAboutUsMouseLeave() {
      this.isAboutUsHovered = false;
    },
    // Gestisce l'hover sul pulsante "Our History"
    handleOurHistoryMouseOver() {
      this.isOurHistoryHovered = true;
    },
    // Gestisce il leave dell'hover sul pulsante "Our History"
    handleOurHistoryMouseLeave() {
      this.isOurHistoryHovered = false;
    }
  }
};
</script>

<template>
  <div class="carousel-container" @mouseover="handleMouseOver" @mouseleave="handleMouseLeave">
    <div class="carousel-slide" v-for="(slide, index) in slides" :key="slide.id" :style="{ backgroundImage: `url(${slide.image})`, opacity: index === currentSlide ? 1 : 0 }">
      <div class="carousel-content">
        <h2 class="carousel-subtitle">GOGRIN ORGANIC FOOD STORE</h2>
        <h1 class="carousel-title">
          Eating naturally & organic is<br>
          always be healthy.
        </h1>
        <div class="buttons">
          <button class="button orange_bg" 
                  @click="goToAboutUs" 
                  @mouseover="handleAboutUsMouseOver" 
                  @mouseleave="handleAboutUsMouseLeave"
                  :class="{ hover: isAboutUsHovered }">About Us</button>
          <button class="button green_bg" 
                  @click="goToOurHistory" 
                  @mouseover="handleOurHistoryMouseOver" 
                  @mouseleave="handleOurHistoryMouseLeave"
                  :class="{ hover: isOurHistoryHovered }">Our History</button>
        </div>
      </div>
      <button class="nav-button prev" @mousedown="prevSlide" @touchstart="prevSlide"><i class="fa-solid fa-chevron-left"></i></button>
      <button class="nav-button next" @mousedown="nextSlide" @touchstart="nextSlide"><i class="fa-solid fa-chevron-right"></i></button>
    </div>
  </div>
</template>

<style scoped lang="scss">
@use '/src/style/general.scss' as *;
@import '/src/style/partials/variables';

.carousel-container {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.carousel-slide {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 1s;
}

.carousel-content {
  text-align: center;
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.carousel-title {
  font-size: 6rem; 
  margin-bottom: 30px; 
  line-height: 1.2;
  font-weight: 600;
}

.carousel-subtitle { 
  font-size: 1.5rem;
  margin-bottom: 10px;
  font-weight: 600;
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 30px; 
  margin-top: 30px;

  button {
    padding: 18px 60px;
    font-weight: 600;
    font-size: 20px;
    transition: background-color $time;

    &.orange_bg {
      background-color: $orange;
    }

    &.green_bg {
      background-color: $green;
    }

    &.hover {
      background-color: $green;
    }

    &.green_bg.hover {
      background-color: $orange;
    }
  }
}

.nav-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0, 0, 0, 0.0);
  color: white;
  border: none;
  padding: 20px;
  cursor: pointer;
  font-size: 2rem; 
  z-index: 1;
  transition: all 0.7s ease;
  border-radius: 10%; 
}

.nav-button:hover {
  background-color: white;
  color: $orange;
  transform: scale(1.2); 
}

.prev {
  left: 10px; 
}

.next {
  right: 30px; 
}

@media (max-width: 1200px) {
  .carousel-title {
    font-size: 3rem;
  }

  .carousel-subtitle {
    font-size: 1.25rem;
  }

  .buttons {
    gap: 20px;
    margin-top: 20px;
  }

  .nav-button {
    padding: 15px;
    font-size: 1.5rem;
  }
}

@media (max-width: 768px) {
  .carousel-title {
    font-size: 2rem;
  }

  .carousel-subtitle {
    font-size: 1rem;
  }

  .buttons {
    flex-direction: column;
    gap: 15px;
    margin-top: 15px;
  }

  .nav-button {
    padding: 10px;
    font-size: 1.25rem;
  }
}

@media (max-width: 480px) {
  .carousel-title {
    font-size: 1.5rem;
  }

  .carousel-subtitle {
    font-size: 0.875rem;
  }

  .buttons {
    gap: 10px;
    margin-top: 10px;
  }

  .nav-button {
    padding: 8px;
    font-size: 1rem;
  }
}
</style>