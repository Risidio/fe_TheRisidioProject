<template>
  <div id="slides">
    <vueper-slides fixed-height="600px">
      <vueper-slide
        v-for="i of 3"
        :key="i"
        :title="`title ${i.toString()}`"
        :content="`slide ${i.toString()}`"
        :style="`background-color: ${colors[i % colors.length]}`"
      />
      <template v-slot:bullet="{ active }">{{ active ? '&ofcir;' : '&olcir;' }}</template>
    </vueper-slides>
  </div>
</template>

<script>
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'

export default {
  name: 'Slides',
  components: { VueperSlides, VueperSlide },
  data () {
    return {
      colors: ['#22007C']
    }
  }
}
</script>

<style>
.vueperslides {
  height: 70vh;
}
.vueperslides__bullet .default {
  background-color: rgba(0, 0, 0, 0.3);
  border: none;
  box-shadow: none;
  transition: 0.3s;
  width: 46px;
  height: 46px;
}

.vueperslides__bullet--active .default {
  background-color: #42b983;
}

.vueperslides__bullet span {
  display: block;
  color: #fff;
  font-size: 20px;
  opacity: 0.8;
}
</style>
