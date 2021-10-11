<template>
  <div id="frame">
    <div class = "hS3_container">
      <vueper-slides fixed-height="true">
        <vueper-slide
          v-for="(slide) in slide"
          :key="slide.id"
          :title="slide.title">
          <template #content>
            <div v-if="slide.id==1" class = "container">
              <h1>How to mint an item?</h1>
              <h2> Step <br><span>0{{slide.id}} </span> </h2>
              <div class = "textContainer">
                    <p>
                      Quick explanation on what and why mint an item :D<br><br>
                      adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna
                      aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris <br><br>
                      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
                      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
                      veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
                      commodo consequat. Duis aut <br><br>
                    </p>
                  <img src="https://res.cloudinary.com/risidio/image/upload/v1633002019/RisidioMarketplace/digitalcurrency_tqf1es.svg" alt="digitalcurrency" class="digitalcurrency">
              </div>
            </div>
            <div v-if="slide.id==2" class = "container">
              <h1>How to mint an item?</h1>
              <h2> Step <br><span>0{{slide.id}} </span> </h2>
              <div class = "textContainer">
                    <p>
                      Quick explanation on what and why mint an item :D<br><br>
                      adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna
                      aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris <br><br>
                      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
                      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
                      veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
                      commodo consequat. Duis aut <br><br>
                    </p>
                  <img src="https://res.cloudinary.com/risidio/image/upload/v1633002019/RisidioMarketplace/digitalcurrency_tqf1es.svg" alt="digitalcurrency" class="digitalcurrency">
              </div>
            </div>
            <div v-if="slide.id==3" class = "container">
              <h1>How to mint an item?</h1>
              <h2> Step <br><span>0{{slide.id}} </span> </h2>
              <div class = "textContainer">
                    <p>
                      Quick explanation on what and why mint an item :D<br><br>
                      adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna
                      aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris <br><br>
                      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
                      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
                      veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
                      commodo consequat. Duis aut <br><br>
                    </p>
                  <img src="https://res.cloudinary.com/risidio/image/upload/v1633002019/RisidioMarketplace/digitalcurrency_tqf1es.svg" alt="digitalcurrency" class="digitalcurrency">
              </div>
            </div>
          </template>
        </vueper-slide>
      </vueper-slides>
    </div>
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
    slide: [
      {
        id: '1',
        text: 'Upload Your Item'
      },
      {
        id: '2',
        text: 'Mint the Bitcoin'
      },
      {
        id: '3',
        text: 'Set Your Royalties'
      }
    ]
  }),
  computed: {
  }
}
</script>

<style scoped>
.vueperslide {
  background-color:#170A6D;
}
.vueperslides--fixed-height {
  height: 70vh;
}
h1{
  color:white;
  font-family: inherit;
  font-size: clamp(50px, 3vw, 6vw);
  font-weight: 200;
  padding-top: 20px;
  text-align: center;
}
h2{
  padding-top: 30px;
  text-align: center;
  color: white;
  font-weight: 400;
}
span{
  font-weight: 600;
  font-size: 1.2em;
}
img{
  height: 250px;
  width: 250px;
  margin-left: 125px;
}
p{
  font-weight: 200;
  font-size: 1em;
}
.container{
  color:white;
  justify-content: center;
  align-items: center;
}
.textContainer{
  margin:auto;
  display:flex;
  flex-wrap: wrap;
  margin-top: 5vw;
}
.textContainer > *{
  display:flex;
  margin-left: auto;
  margin-right:auto;
  flex: 1 1 300px;
}
img{
  margin:auto;
}
@media only screen and (max-width: 770px)  {
  .vueperslides--fixed-height {height: 100vh;}
  .textContainer{text-align:center}
}
@media only screen and (max-width: 500px)  {
  .vueperslides--fixed-height {height: 120vh;}
}
</style>
