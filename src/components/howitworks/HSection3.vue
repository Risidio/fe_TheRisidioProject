<template>
  <div id="frame">
    <div class = "hS3_container">
      <vueper-slides fixed-height="80vh">
      <vueper-slide
        v-for="i of 3"
        :key="i"
        :title="`title ${i.toString()}`"
        :content="`slide ${i.toString()}`"
      />
    </vueper-slides>
  </div>
</template>

<script>
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'
export default {
  name: 'Group3',
  components: {
    VueperSlides,
    VueperSlide
  },
  props: ['content'],
  data: () => ({
    slides: [
      {
        title: 'Slide #1',
        content: 'Slide content.'
      }
    ]
  }),
  computed: {
  }
}
</script>

<style scoped>
.vueperslide {
  background-color: #170A6D;
}
.vueperslides--fixed-height { height: 500px; }
</style>
