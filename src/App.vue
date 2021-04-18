<template>
  <Carousel 
    :imagesPath="images" 
  />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Carousel from './components/Carouselvjs.vue'

export default defineComponent({
  name: 'App',
  
  data() {
    return {
      images: [
        "src/assets/img/resident_evil.jpg",
        "src/assets/img/street_fighter.jpg",
        "src/assets/img/super_mario.jpg"
      ]
    }
  },

  mounted() {},

  methods: {},

  components: {
    Carousel
  }
})
</script>

<style>
html {
  background-color: black;
  color: #FFF;
}
</style>
