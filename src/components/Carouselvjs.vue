<template>
  <div 
    class="carousel flex items-center"
    :style="{ width: styles.width + 'px', height: styles.height + 'px' }"
  >
    <div 
      class="arrow flex justify-center items-center"
      @click="previousCarouselItem"
    >
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-left" viewBox="0 0 16 16">
        <path fill-rule="evenodd" d="M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z"/>
      </svg>
    </div>
    <img
      class="carousel-item fadeIn"
      :class="{ invisible: ! image.visible, animated: animate  }"
      v-for="image in images" 
      :key="image.id" 
      :src="image.path" 
    >
    <div 
      class="arrow flex justify-center items-center"
      @click="nextCarouselItem"
    >
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-right" viewBox="0 0 16 16">
        <path fill-rule="evenodd" d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"/>
      </svg>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

type Image = {
  id: number,
  path: string,
  visible: boolean
}

type Data = {
  currentIndex: number,
  images: Array<Image>
  styles: {
    width: string,
    height: string,
  }
}

export default defineComponent({
  name: 'Carouselvjs',
  
  data() {
    return {
      currentIndex: 0,
      images: [],
      styles: {
        width: '',
        height: '',
      }
    } as Data;
  },

  props: {
    imagesPath: {
      type: Array,
      required: true
    },

    automatic: {
      type: Boolean,
      required: false,
      default: false
    },

    animate: {
      type: Boolean,
      required: false,
      default: false
    },

    width: {
      type: String,
      required: false,
    },

    height: {
      type: String,
      required: false,
    },
  },

  created() {
    this.imagesPath.forEach((imagePath, index) => {
      const imageAttributes = {
        id: index,
        path: imagePath as string,
        visible: false
      };

      if (index === 0) {
        imageAttributes.visible = true  
      }

      this.images.push(imageAttributes)
    });
  },

  beforeMount() {
    if (this.automatic) {
      this.automaticCarousel();
    }

    this.styles.width = this.width ? this.width : this.styles.width;
    this.styles.height = this.height ? this.height : this.styles.height;
  },

  methods: {
    nextCarouselItem() {  
      this.toggleCurrentItemVisibility();

      if (this.currentIndex + 1 > this.images.length - 1) {
        this.currentIndex = 0
      } else {
        this.incrementCarouselIndex()
      }

      this.toggleCurrentItemVisibility();
    },

    previousCarouselItem() {
      if (this.currentIndex - 1 < 0) return;      
      
      this.toggleCurrentItemVisibility();
      
      this.decrementCarouselIndex()

      this.toggleCurrentItemVisibility();
    },

    toggleCurrentItemVisibility() {
      const itemVisibility = this.images[this.currentIndex].visible;

      this.images[this.currentIndex].visible = ! itemVisibility
    },

    incrementCarouselIndex() {
      this.currentIndex += 1;
    },

    decrementCarouselIndex() {
      this.currentIndex -= 1;
    },

    automaticCarousel() {
      window.setInterval(() => {
        this.nextCarouselItem()
      }, 2500)
    }
  },

  components: {}
})
</script>

<style>
.carousel {
  width: 400px;
  height: 400px;
  user-select: none;
}

.carousel-item {
  width: 100%;
}

.arrow {
  width: 40px;
  height: 40px;
  margin: 10px;
  cursor: pointer;
}

.invisible {
  display: none;
}

/* Effects */
.animated {
  -webkit-animation-duration: 2s;
  animation-duration: 2s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}

@-webkit-keyframes fadeIn {
  0% {opacity: 0;}
  100% {opacity: 1;}
}
         
@keyframes fadeIn {
  0% {opacity: 0;}
  100% {opacity: 1;}
}
         
.fadeIn {
  -webkit-animation-name: fadeIn;
  animation-name: fadeIn;
}

/* Flex Rules */
.flex {
  display: flex;
}

.justify-between {
  justify-content: space-between;
}

.justify-center {
  justify-content: center;
}

.items-center {
  align-items: center;
}
</style>
