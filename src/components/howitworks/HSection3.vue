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
              <h1>How to create an account ?</h1>
              <!-- <h2> Step <br><span>0{{slide.id}} </span> </h2> -->
              <div class = "textContainer">
                    <p>
                      In this decentralised world, you don't need to create multiple accounts anymore. Instead you simply
                      connect with your secure Wallet and we don't hold any information about you! <br><br>
                      You want to join us and don't have a stacks wallet ? Get it now, just download the browser extension,
                      it's free and easy to use <br><br>
                    </p>
                    <button class="button"> Download A Wallet </button>
              </div>
            </div>
            <div v-if="slide.id==2" class = "container">
              <h1>How to buy an NFT ?</h1>
              <!-- <h2> Step <br><span>0{{slide.id}} </span> </h2> -->
              <div class = "textContainer">
                    <p>
                      To buy an NFT, all you need to do is make sure you are connected with your stacks wallet and
                      have enough stacks on it to afford the item. This process is 100% secure and we don't keep any information
                      about you!<br><br>
                      You don't have any STX in your wallet ? <br><br><br><br>
                    </p>
                    <button class="button"> Get STX </button>
              </div>
            </div>
            <div v-if="slide.id==3" class = "container">
              <h1>How to mint an item?</h1>
              <!-- <h2> Step <br><span>0{{slide.id}} </span> </h2> -->
              <div class = "textContainer">
                    <p>
                      (Little explanation on what it is to mint an item )<br><br>
                      You can mint your own NFT on Risidio Marketplace. All you need to do is make sure you're connected.
                      Go to your profile, and click on the &lt;&lt; + &gt;&gt; sign. You will load a file and get all the information.
                      Once that's done, you're ready to mint. Little bonus, you can set Rotalties on your item and ensure You get
                      a portion of the sale price everytime your artwork is resold.  <br><br>
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
