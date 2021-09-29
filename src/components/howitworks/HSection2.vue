<template>
  <div id="howitworks">
<div class = "Htitle">
  <div class = "title-container">
      <h1>What is Ruma ?</h1>
  </div>
  <div class = "main-container" >
      <p> Quick explanation on what and why mint an item :D <br><br>
      adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut Lorem ipsum dolor sit amet,
      consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut <br><br>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut<br><br>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim</p>
  </div>
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
</template>

<script>
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'
export default {
  name: 'WRuma',
  components: { VueperSlides, VueperSlide },
  data () {
    return {
      colors: ['#22007C'],
      banner: 'https://images.prismic.io/digirad/6e5bb3a5-21b7-4bcb-b5a7-85128b6e6e8a_Rumba_bg_small.png?auto=compress,format'
    }
  }
}
</script>

<style scoped>
.Htitle{
  min-height: 90vh;
}
.title-container {
  font-family: inherit;
  padding: max(150px) min(12%) 0 min(12%);
  align-items: center;
  text-align: center;
}
.title-container h1{
  font-family: inherit;
  font-size: clamp(50px, 3vw, 6vw);
  font-weight: 200;
}
.main-container{
  text-align: justify;
  display: grid;
  grid-template-columns: repeat( 2, 1fr);
}
.main-container p{
  grid-column: 2;
  padding: 100px 15% 0 0;
  font-size: 1.2em;
  font-weight:500;
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
