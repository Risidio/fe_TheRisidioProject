<template>
<div v-if="item && item.attributes" >
  <div class="singleNftGalleryContainer">
  <b-link :to="assetUrl" class="mediaItem">
    <MediaItemGeneral :classes="'item-image text-center'" :style="'box-shadow: 10px 10px 30px #0000002F; width: 25rem; height: auto; margin:auto; display:block; border-radius: 5px;'" v-on="$listeners" :options="videoOptions" :mediaItem="item.attributes.coverImage"/>
  </b-link>
  <div class="" style="max-width: 100%">
    <div class="mt-4 mb-2">
      <div v-if="item.contractAsset" v-bind="price" style="font-size: 1.5rem; font-weight:500; color:black">
        <b-link :to="assetUrl" class="itemName">
          <span style="color:black; font-weight: 500; text-transform: capitalize">{{item.name}}</span> <span style="float: right; font-weight: 300; color: black; font-size: 15px; margin: 5px 0">{{this.item.contractAsset.saleData.buyNowOrStartingPrice}} STX</span>
        </b-link>
      <div class="mt-1" style="font-weight:300; font-size: 1.2rem; color:black">
        By <span style="font-weight:600; font-size: 1.2rem; color:black">{{item.artist}} <span style="float: right; font-weight: 300; color: black; font-size: 15px;">{{this.item.contractAsset.saleData.buyNowOrStartingPrice * 1.9}} $</span> </span>
      </div>
        <div class="mt-1" style="font-weight:300; font-size: 1.2rem; color:black">
        <!-- Price: {{this.item.contractAsset.saleData.buyNowOrStartingPrice}} stx -->
        </div>
      </div>
      <!-- <div class="mt-1" style="font-weight:200; font-size: 1.5rem; color:black; display:flex;">Price:{{item.attributes.editionCost}} stx</div> -->
    </div>
  </div>
  </div>
</div>
</template>

<script>
import { APP_CONSTANTS } from '@/app-constants'
import MediaItemGeneral from '@/components/upload/MediaItemGeneral'

export default {
  name: 'GalleryNft',
  components: {
    MediaItemGeneral
  },
  props: ['item'],
  data () {
    return {
      dims: { width: 360, height: 360 },
      explorer: 'https://explorer.stacks.co/txid/'
    }
  },
  methods: {
    marketplaceLink: function () {
      return process.env.VUE_APP_MARKETPLACE_URL + '/nfts/' + this.item.contractAsset.nftIndex
    },
    mintedEvent (data) {
      // this.$store.commit('setModalMessage', 'Transaction sent. <br/>While you are waiting please take a minute to fill in <a style="font-size: 2.0rem; color: blue" href="https://shrl.ink/HPyh" target="_blank" rel="noopener noreferrer"> this survey.</a><br/> You can find your NFT on the /my-nfts page here and in your Stacks Wallet.<br/><br/><a href="' + this.explorer + data.txId + '?chain=mainnet" target="_blank">Track the transaction here</a>')
      this.$store.commit('setModalMessage', 'Transaction sent.<br/>You can find your NFTs here by clicking on My NFTs in the account section.<br/><a class="text-white" href="' + this.explorer + data.txId + '?chain=mainnet" target="_blank">Track your transaction here</a>')
      this.$root.$emit('bv::show::modal', 'waiting-modal')
    },
    salesButtonLabel () {
      if (!this.item.contractAsset) return 'NOT MINTED'
      return this.$store.getters[APP_CONSTANTS.KEY_SALES_BUTTON_LABEL](this.item.contractAsset.saleData.saleType)
    }
  },
  computed: {
    contractAsset () {
      return this.item.contractAsset
    },
    price () {
      return this.item.editionCost
    },
    videoOptions () {
      let file = this.item.attributes.artworkFile
      if (!file) {
        file = this.item.attributes.artworkClip
      }
      if (!file) return {}
      const videoOptions = {
        emitOnHover: true,
        playOnHover: false,
        bigPlayer: true,
        assetHash: this.item.assetHash,
        autoplay: false,
        muted: false,
        controls: true,
        showMeta: false,
        dimensions: 'max-width: 100%; max-height: auto;',
        aspectRatio: '1:1',
        poster: (this.item.attributes.coverImage) ? this.item.attributes.coverImage.fileUrl : null,
        sources: [
          { src: this.item.attributes.artworkFile.fileUrl, type: this.item.attributes.artworkFile.type }
        ],
        fluid: false
      }
      return videoOptions
    },
    assetUrl () {
      if (this.item.contractAsset) {
        return '/nfts/' + this.item.contractAsset.nftIndex
      } else {
        return '/assets/' + this.item.contractAsset.tokenInfo.assetHash + '/1'
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.galleryNFTContainer{
  display: flex;
  margin: 0;
}
.singleNftGalleryContainer{
  display: flex;
  flex-direction: column;
  background: #7b7b7b1d;
  padding: 3rem;
  z-index: 1;
  border-radius: 30px;
  height: 420px;
  width: 325px;
  margin-bottom: 50px;
  // box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}
.itemName{
  font-size: 20px;
}
</style>
