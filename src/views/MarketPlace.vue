<template>
<div class="container">
  <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1632564338/Risidio.com/main_bg.svg" alt="">
  <div class="market_introduction_text">
  <h1 class="market_intro_h1">Marketplace</h1>
  <p class="market_intro">Risidio is a NFT minting platform and marketplace that enables users to register their assets in different formats on the Bitcoin blockchain using Stacks. Regardless of their experience with NFTs, Ruma allows all creatives to secure ownership rights and set royalties for further monetisation of work easily and securely.</p>
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
      <h1>NFT Public Gallery</h1>
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
.banner{
  position: absolute;
  top: 0;
  left: -25px;;
  width: 120%;
  height: 50rem;
  object-fit: cover;
  z-index: -10;
  transform: rotate(180deg);
}

.market_introduction_text{
  margin: auto;
  width: 50vw;
}

.market_intro_h1{
  text-align: center;
  margin: auto;
  margin-bottom: 100px;
  color: white;
  font-weight: 400;
  font-size: 5rem;
}

.market_intro{
  text-align: center;
  margin: auto;
  color: white;
  font-weight: 400;
}

.mainSearchBar{
  margin-top: 11rem;
}

.gallery_public{
  margin-top: 100px;
}
</style>
