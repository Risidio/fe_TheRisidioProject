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
              <h1>{{content.howsectionslide1[0].howslidetitle[0].text}}</h1>
              <!-- <h2> Step <br><span>0{{slide.id}} </span> </h2> -->
              <div class = "textContainer">
                    <p>
                      {{content.howsectionslide1[0].howslidetext[0].text}}<br><br>
                      {{content.howsectionslide1[0].howslidetext1[0].text}}
                    </p>
                    <button class="button"> Download A Wallet </button>
              </div>
            </div>
            <div v-if="slide.id==2" class = "container">
              <h1>{{content.howsectionslide2[0].howslidetitle[0].text}}</h1>
              <!-- <h2> Step <br><span>0{{slide.id}} </span> </h2> -->
              <div class = "textContainer">
                    <p>{{content.howsectionslide2[0].howslidetext[0].text}}<br><br>
                      {{content.howsectionslide2[0].howslidetext1[0].text}} <br><br>
                    </p>
                    <button class="button"> Get STX </button>
              </div>
            </div>
            <div v-if="slide.id==3" class = "container">
              <h1>{{content.howsectionslide3[0].howslidetitle[0].text}}</h1>
              <!-- <h2> Step <br><span>0{{slide.id}} </span> </h2> -->
              <div class = "textContainer">
                    <p>{{content.howsectionslide3[0].howslidetext[0].text}}<br><br>
                      {{content.howsectionslide3[0].howslidetext1[0].text}}  <br><br>
                    </p>
                    <button class="button"> Register </button>
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
  margin-top: 5%;
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
  font-weight: 300;
  font-size: 1.2em;
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}
.container{
  color:white;
  justify-content: center;
  align-items: center;
}
.textContainer{
  margin:auto;
  margin-top: 5vw;
}
.button{
  margin: 0 auto;
  display: block;
  margin-top: 50px;
  width: 200px;
  height: 60px;
  border-radius: 100px;
  border: none;
  background-color: rgb(0, 177, 201);
  font-size: 14px;
  font-weight:400;
  color: white;
}
 @media only screen and (max-width: 770px)  {
  .vueperslides--fixed-height {height: 80vh;}
}
/*
@media only screen and (max-width: 500px)  {
  .vueperslides--fixed-height {height: 120vh;}
} */
</style>
