<template>
<div class="container">
  <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1632564338/Risidio.com/main_bg.svg" alt="">
  <div class="market_introduction_text">
  <h1 class="market_intro_h1">Gallery</h1>
  <p class="market_intro">Risidio is a NFT minting platform and marketplace that enables users to register their assets in different formats on the Bitcoin blockchain using Stacks. Regardless of their experience with NFTs, Ruma allows all creatives to secure ownership rights and set royalties for further monetisation of work easily and securely.</p>
  </div>
 <search-bar class="mainSearchBar" :showPrepend="true" v-on="$listeners"/>
</div>
</template>

<script>
import SearchBar from '@/components/marketplace/SearchBar'
// import GalleryNft from '@/components/marketplace/GalleryNft'
const STX_CONTRACT_ADDRESS = process.env.VUE_APP_STACKS_CONTRACT_ADDRESS
const STX_CONTRACT_NAME = process.env.VUE_APP_STACKS_CONTRACT_NAME

export default {
  name: 'NftGallery',
  components: {
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
        this.resultSet = results
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
  left: 0;
  width: 100vw;
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
</style>
