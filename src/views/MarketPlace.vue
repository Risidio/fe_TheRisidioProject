<template>
<div>
    <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1633609373/RisidioMarketplace/Group_-304_ofssmk.svg" alt="">
  <div class="container section1">
      <div class="market_introduction_text">
        <!-- <p class="market_intro_Ex">Explore the New Era of Digital Art</p> -->
        <!-- <h1 class="market_intro_h1"> Risidio Marketplace</h1> -->
        <h1 class="market_intro_h1"> {{content.heroarea[0].herotitle[0].text}}</h1>
        <p class="market_intro">{{content.heroarea[0].herotext[0].text}}</p>
      </div>
    <search-bar class="mainSearchBar" :showPrepend="true" v-on="$listeners"/>
    <!-- <div class="gallery_highlights">
          <div class="row">
          <div v-for="(item, index) in resultSet" :key="index" class="col-md-3 col-6" >
            <div class="mb-4 gallery__items" v-if="index < 4">
              <GalleryNft class="mb-2" :item="item"/>
                  </div>
                </div>
            </div>
          </div> -->
    <div class="gallery_public">
      <div>
    <div>
      <b-nav class="galleryNav" >
        <div class="galleryNavContainer" >
        <b-nav-item class="galleryNavItem">Discover</b-nav-item>
        <b-nav-item class="galleryNavItem">Popular</b-nav-item>
        <b-nav-item class="galleryNavItem">Collections</b-nav-item>
        <b-nav-item class="galleryNavItem">Your NFT's</b-nav-item>
        </div>
      </b-nav>
    </div>
        </div>
        <div class="gallery__collections--view-all d-lg-block d-none">
        </div>
      </div>
      <div class="" v-if="resultSet && resultSet.length > 0">
        <div class="row">
          <div v-for="(item, index) in resultSet" :key="index" class="col-sl-6" style="display:flex; margin:auto" >
            <div class="gallery__items" style="display:flex; margin:10px">
              <GalleryNft class="mb-10" :item="item"/>
                  </div>
                </div>
            </div>
          </div>
        <div class="container" style="min-height: 85vh;" v-else>
      <b-container class="text-white mt-5">
        <h1>No Gallery NFTs</h1>
        <p>Our Gallery is coming online soon - please come back soon...</p>
      </b-container>
      <button>See more collectables</button>
    </div>
    <!-- <CollectionsInfo/> -->
    <!-- <div class="section3">
     <div style="font-size:5rem; font-weight:300; margin:auto; text-align:center">The New Collectables Marketplace</div>
     <div style="font-size:3rem; font-weight:500; margin:20px auto; text-align:center">Technology is progressing so art is evolving.</div>
     <div style="font-size:1.5rem; font-weight:300; margin:40px auto; text-align:center; width:50%; ">Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua</div>
     <div>
       <div style="display: flex; margin: auto;background:#50B1B5; width: 150px; color:white; text-align:center; padding:10px; padding-left:20px; border-radius:20px;">How It Works</div>
     </div>
    </div> -->
    <!-- <div class="section4">
          <img style="width:100vw; margin-left:-25.1%;object-fit:contain; z-index:-10;" src="https://res.cloudinary.com/risidio/image/upload/v1633609373/RisidioMarketplace/Group_-304_ofssmk.svg" alt="">
      <div style="margin-top:-45vh;">
     <div style="color:white; font-size:4rem; font-wight:100; text-align:center; margin: 50px auto auto auto;">Start Trading Collectables</div>
          <div  style="color:white; font-size:2rem; font-wight:100; text-align:center; margin: 50px auto auto auto;">Create an account and join our marketplace.</div>
          <div  style="color:white; font-size:2rem; font-wight:100; text-align:center; margin: 10px auto;">It’s Quick, Safe and Free.</div>
      </div>
    </div> -->
  </div>
</div>
</template>

<script>
import SearchBar from '@/components/marketplace/SearchBar'
import GalleryNft from '@/components/marketplace/GalleryNft'
import { APP_CONSTANTS } from '@/app-constants'
// import CollectionsInfo from '@/components/marketplace/CollectionsInfo.vue'
const STX_CONTRACT_ADDRESS = process.env.VUE_APP_STACKS_CONTRACT_ADDRESS
const STX_CONTRACT_NAME = process.env.VUE_APP_STACKS_CONTRACT_NAME

export default {
  name: 'MarketPlace',
  components: {
    GalleryNft,
    SearchBar
    // CollectionsInfo
  },
  data () {
    return {
      resultSet: [],
      loaded: false
    }
  },
  mounted () {
    this.findAssets()
  },
  methods: {
    findAssets () {
      // const pid = STX_CONTRACT_NAME.split('-')[0]
      this.$store.dispatch('rpaySearchStore/findByProjectId', STX_CONTRACT_ADDRESS + '.' + STX_CONTRACT_NAME).then((results) => {
        // console.log(results)
        do {
          this.resultSet = results
        } while (!results.attributes.coverImage.size === null)
        // for (let i = 0; i < results.length; i++) {
        //   console.log(results[i])
        //   if (results[i].attributes.coverImage.size !== null) {
        //     this.resultSet = results
        //   } else {
        //     return
        //   }
        // }
      })
    }
  },
  computed: {
    content () {
      const content = this.$store.getters[APP_CONSTANTS.KEY_CONTENT_MARKET]
      return content
    }
  }
}
</script>

<style lang="scss" scoped>
// .container{
//   // margin-top: 10%;
//   // width: 50%;
//   // min-width: 800px;
//   // width: 1400px;
// }
.banner{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 50rem;
  object-fit: cover;
  z-index: -10;
  transform: rotate(180deg);
}

.galleryNav{
  margin: auto;
  margin-bottom: 70px;
  width: 100%;
  justify-items: center;
  align-items: center;
  border-bottom: solid rgba(128, 128, 128, 0.112) 1px;
}
  .galleryNavContainer{
    margin: auto;
    margin-bottom: -1px;
    // width: 50%;
    display: flex;
    flex-direction: row;
  }

.galleryNavItem{
  // max-width: fit-content;
  max-width: 500px;
  padding: 10px;
  margin: auto;
  border: solid rgba(255, 255, 255, 0)  2px;
}

.galleryNavItem:hover{
    border-bottom: 2px solid #50B1B5;
}

.market_introduction_text{
  margin: auto;
  max-width: 1600px;
  text-align: center;
  color: white;
  margin: auto;
}

.market_intro_h1{
  margin-bottom: 25px;
  color: white;
  font-weight: 400;
  font-size: clamp(2rem, 5rem, 7rem);
  word-wrap: break-word;
}
.market_intro{
  font-size: 1em;
  font-weight: 400;
  width: 100%;
  max-width: 1000px;
  margin: auto;
}
.market_intro_Ex{
  font-size: 1.2em;
}

.mainSearchBar{
  margin-top: 10rem;
}

.gallery_public{
  margin-top: 50px;
}

.section2{
 background: transparent linear-gradient(180deg, #3715b4 0%,#150764 100%) 0% 0% no-repeat padding-box;
 width: 100vw;
 height: 150vh;
 margin-left: -10.1%;
 padding: 5% 10%;
 color: white;
}
.section3{
 background: white;
 margin: 10vh auto;
 width: 80%;
 height: 80vh;
 padding-bottom: 50px;
 color: black;
}
.section4{
 background: white;
 margin: 10vh auto;
 width: 80%;
 height: 40vh;
 color: black;
}

.exampleCollection{
  display: flex;
  flex-direction: row;
  width: 90rem;
  margin: 10vh auto;
   color: black;
}

.singleNftGalleryContainer{
  display: flex;
    margin: auto;
    flex-direction: column;
    background: #ffffff4d;
    padding-top: 3rem;
    z-index: 1;
    border-radius: 30px;
    height: 40rem;
    width: 28rem;
    margin-bottom: 50px;
     color: white;
}

.singleNftGalleryContainer:nth-of-type(1n){

  transform: rotateZ(-15deg);
  z-index: 2;
}
.singleNftGalleryContainer:nth-of-type(2n){
  margin-top: -50px;
  transform: rotateZ(0deg);
  z-index: 1;
}
.singleNftGalleryContainer:nth-of-type(3n){

  transform: rotateZ(15deg);
  z-index: 2;
}
.singleNftGalleryContainer img{
  // padding: 10px;
  width: 22rem;
  height: auto;
  margin: auto;
  border-radius: 20px;
  margin-top: 1%;
}

.fakeDetails{
  display: flex;
  flex-direction: column;
  flex-wrap:wrap;
  margin-bottom: 6rem;
  margin-left: 2.5rem;
  width: 80%;
  height: 100px;
}

@media only screen and (max-width: 570px){
  .galleryNavContainer{
    flex-direction: column;
  }
}
</style>
