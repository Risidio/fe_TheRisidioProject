<template>
<div>
    <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1633609373/RisidioMarketplace/Group_-304_ofssmk.svg" alt="">
<div class="container">
    <div class="market_introduction_text">
      <!-- <p class="market_intro_Ex">Explore the New Era of Digital Art</p> -->
      <!-- <h1 class="market_intro_h1"> Risidio Marketplace</h1> -->
      <h1 class="market_intro_h1"> The New Era of Digital Art</h1>
      <p class="market_intro" style="width:60%">Risidio is a NFT minting platform and marketplace that enables users to register their assets in different formats on the Bitcoin blockchain using Stacks. Regardless of their experience with NFTs, Ruma allows all creatives to secure ownership rights and set royalties for further monetisation of work easily and securely.</p>
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
    <div class=" my-5" v-if="resultSet && resultSet.length > 0">
      <div class="row">
        <div v-for="(item, index) in resultSet" :key="index" class="col-md-3 col-6" >
          <div class="mb-4 gallery__items" >
            <GalleryNft class="mb-2" :item="item"/>
                </div>
              </div>
          </div>
        </div>
      <div class="container" style="min-height: 85vh;" v-else>
    <b-container class="text-white mt-5">
      <h1>No Gallery NFTs</h1>
      <p>Our Gallery is coming online soon - please come back soon...</p>
    </b-container>
  </div>
</div>
</div>
</template>

<script>
import SearchBar from '@/components/marketplace/SearchBar'
import GalleryNft from '@/components/marketplace/GalleryNft'
const STX_CONTRACT_ADDRESS = process.env.VUE_APP_STACKS_CONTRACT_ADDRESS
const STX_CONTRACT_NAME = process.env.VUE_APP_STACKS_CONTRACT_NAME

export default {
  name: 'MarketPlace',
  components: {
    GalleryNft,
    SearchBar
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
  }
}
</script>

<style lang="scss" scoped>
.container{
  // margin-top: 10%;
  // width: 50%;
  min-width: 800px;
  max-width: 1200px;
}
.banner{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  min-height: 50rem;
  object-fit: cover;
  z-index: -10;
  transform: rotate(180deg);
}

.galleryNav{
  margin: auto;
  margin-bottom: 70px;
  max-width: 90%;
  // display: flex;
  // flex-direction: column;
  justify-items: center;
  align-items: center;
  border-bottom: solid rgba(128, 128, 128, 0.112) 1px;
}
  .galleryNavContainer{
    margin: auto;
    margin-bottom: -1px;
    width: 50%;
    display: flex;
    flex-direction: row;
  }

.galleryNavItem{
  width: fit-content;
  padding: 10px;
  margin: auto;
  border: solid rgba(255, 255, 255, 0)  2px;
}

.galleryNavItem:hover{
    border-bottom: 2px solid #50B1B5;
}

.market_introduction_text{
  margin: auto;
  width: 60vw;
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
@media only screen and (max-width: 1380px){
  .banner{
    min-height: 52rem;
  }
}
@media only screen and (max-width: 1078px){
  .banner{
    min-height: 55rem;
  }
}
@media only screen and (max-width: 840px){
  .banner{
    min-height: 57rem;
  }
}
@media only screen and (max-width: 730px){
  .banner{
    min-height: 58rem;
  }
}
@media only screen and (max-width: 685px){
  .banner{
    min-height: 65rem;
  }
}
@media only screen and (max-width: 570px){
  .banner{
    min-height: 69rem;
  }
}
@media only screen and (max-width: 514px){
  .banner{
    min-height: 73rem;
  }
}
@media only screen and (max-width: 456px){
  .banner{
    min-height: 75rem;
  }
}
</style>
