<template>
<div class="mySingleNFT">
  <div class ="ItemImage" v-if="item && item.attributes">
    <embed :src="item.attributes.coverImage.fileUrl" class= "itemFit" :options="options" :mediaItem="getMediaItem().coverImage"/>
  </div>
  <div class="mt-1 d-flex justify-content-end">
    <div class="text-small text-right">
      <ItemActionMenu :item="item"/>
    </div>
  </div>

  <div class="mySingleNFTName text-left">
    <b-link router-tag="a" :to="assetUrl">{{item.name}}</b-link>
  </div>
  <div class="mySingleNFTArtist text-left">
    <b-link router-tag="a" :to="assetUrl">By: {{item.artist}}</b-link>
  </div>
  <hr class="hr"/>
  <div class="mySingleNFTDetail text-center">
    <div><b-link router-tag="a" :to="assetUrl">{{salesButtonLabel}}</b-link><p class="mySingleNFTDetail text-center">{{item.attributes.artworkFile.type}}</p></div>
  </div>
</div>
</template>

<script>
import { APP_CONSTANTS } from '@/app-constants'
// import MediaItemGeneral from '@/components/upload/MediaItemGeneral'
import ItemActionMenu from '@/components/items/ItemActionMenu'

export default {
  name: 'MySingleNft',
  components: {
    // MediaItemGeneral,
    ItemActionMenu
  },
  props: ['item', 'token'],
  data () {
    return {
    }
  },
  mounted () {
    this.filter = this.$route.params.filter
  },
  methods: {
    getMediaItem () {
      const attributes = this.$store.getters[APP_CONSTANTS.KEY_WAITING_IMAGE](this.item)
      return attributes
    }
  },
  computed: {
    salesButtonLabel () {
      if (!this.item.contractAsset) return 'NOT MINTED'
      return 'MINTED #' + this.item.contractAsset.nftIndex + ' (Ed. ' + this.item.contractAsset.tokenInfo.edition + ' of ' + this.item.contractAsset.tokenInfo.maxEditions + ')'
    },
    options () {
      const attributes = this.getMediaItem()
      return {
        emitOnHover: true,
        playOnHover: true,
        bigPlayer: false,
        assetHash: this.item.assetHash,
        autoplay: false,
        muted: true,
        controls: false,
        showMeta: false,
        aspectRatio: '1:1',
        poster: (attributes.coverImage) ? attributes.coverImage.fileUrl : attributes.artworkFile.fileUrl,
        sources: [
          { src: attributes.artworkFile.fileUrl, type: attributes.artworkFile.type }
        ],
        fluid: false
      }
    },
    assetUrl () {
      if (!this.item.contractAsset) {
        return '/item-preview/' + this.item.assetHash + '/' + 0
      } else {
        if (this.token) {
          return '/item-preview/' + this.item.assetHash + '/' + this.token.tokenInfo.edition
        } else {
          return '/item-preview/' + this.item.assetHash + '/' + this.item.contractAsset.tokenInfo.edition
        }
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.mySingleNFT{
  padding: 28px;
  background:rgba(129, 129, 129, .12);
  // background: rgba(80, 177, 181, .12);
  border-radius: 26px;
  margin: 20px 10px;
}
.mySingleNFT:hover{
  background:rgba(129, 129, 129, .12);
  transition: all 5s ease-out smooth ;
}
.itemFit{
  width: 100%;
  height: 230px;
  border-radius: 5px;
}
.ItemImage{
  box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.18);
}
.mySingleNFTName{
  font-size: 25px;
}
.mySingleNFTDetail{
  font-size: 10px;
}

</style>
