<template>
<section class="itemPreviewSection" id="section-minting">
  <div class="nFTInfo">
    <p> <span>Last Update</span> <br/>
    <span class="spanDate">{{moment(1)}}</span><br>
     <span class="spanDate1">{{moment(2)}}</span><br>
      <span class="spanDate">{{moment(3)}}</span></p>
  </div>
  <div class="mt-3" v-if="!item">
    {{message}}
  </div>
  <div :key="componentKey" class="itemPreviewBody container" v-if="item">
    <div><router-link class="backBtn" to="/my_account"><b-icon icon="chevron-left" shift-h="-3"></b-icon> Back </router-link></div>
    <div class= "itemPreviewContainer" >
      <div class = "itemPreviewSubContainer">
        <div class="itemPreviewNFT">
          <NftCoverImage :item="item" :displayHeader="false"/>
        </div>
        <!-- <MediaItemGeneral :classes="'item-image-preview'" :options="options" :mediaItem="getMediaItem().coverImage"/> -->
        <div class="mintButton">
        <MintingTools class="w-100" :item="item" v-if="iAmOwner || edition === 0" />
        </div>
      </div>
      <div class= "itemPreviewContainerDetails">
        <div>
          <div class="mb-2 d-flex justify-content-between">
            <h1 class= "assetName"> {{item.name}}</h1>
            <ItemActionMenu :item="item" />
          </div>
        </div>
        <h2 class= "assetArtist">By: <span>{{item.artist}}</span></h2>
        <div class="w-100 " v-html="item.description"></div>
        <!--
        <div class="text-small" v-if="pending">
          Transaction has been sent to the blockchain
        </div>
        -->
        <MintInfo :item="item"/>
        <PendingTransactionInfo v-if="txPending.length > 0" :contractAsset="item.contractAsset" :assetHash="item.assetHash"/>
        <div v-else>
          <!-- <MintingTools class="w-100" :item="item" v-if="iAmOwner || edition === 0" /> -->
        </div>
      </div>
    </div>
  </div>
</section>
</template>

<script>
import PendingTransactionInfo from '@/components/toolkit/PendingTransactionInfo'
import moment from 'moment'
import MintInfo from '@/components/toolkit/mint-setup/MintInfo'
import MintingTools from '@/components/toolkit/MintingTools'
import { APP_CONSTANTS } from '@/app-constants'
import ItemActionMenu from '@/components/items/ItemActionMenu'
// import MediaItemGeneral from '@/components/upload/MediaItemGeneral'
import NftCoverImage from '@/components/upload/NftCoverImage'

export default {
  name: 'ItemPreview',
  components: {
    MintingTools,
    ItemActionMenu,
    PendingTransactionInfo,
    MintInfo,
    // MediaItemGeneral,
    NftCoverImage
  },
  data: function () {
    return {
      showHash: false,
      pending: false,
      componentKey: 0,
      assetHash: null,
      message: 'No item available...'
    }
  },
  mounted () {
    this.loading = false
    this.assetHash = this.$route.params.assetHash
    this.edition = Number(this.$route.params.edition)
    if (this.edition > 0) {
      this.$store.dispatch('rpayStacksContractStore/fetchAssetByHashAndEdition', { assetHash: this.assetHash, edition: this.edition })
    }
    if (window.eventBus && window.eventBus.$on) {
      const $self = this
      window.eventBus.$on('rpayEvent', function (data) {
        if (data.opcode === 'stx-transaction-sent' || data.opcode === 'stx-transaction-update') {
          // save transaction but not on gaia asset
          if (data.txId) {
            $self.pending = true
            if (data.txStatus !== 'pending') {
              $self.pending = false
            }
            if (data.functionName === 'mint-token') {
              if (data.txStatus !== 'pending') {
                if (data && data.functionName === 'mint-token') {
                  $self.item.mintInfo = {
                    txId: data.txId,
                    txStatus: data.txStatus
                  }
                }
                $self.$store.dispatch('rpayMyItemStore/saveItem', $self.item)
              }
            }
          }
        }
      })
    }
  },
  methods: {
    moment: function (val) {
      const month = moment(this.item.updated).format('MMMM')
      const day = moment(this.item.updated).format('Do')
      const year = moment(this.item.updated).format('YYYY')
      if (val === 1) {
        return (month)
      } else if (val === 2) {
        return (day)
      } else if (val === 3) {
        return (year)
      } else {
        return (null)
      }
    },
    getMediaItem () {
      const attributes = this.$store.getters[APP_CONSTANTS.KEY_WAITING_IMAGE](this.item)
      return attributes
    },
    deleteMediaItem: function (mediaId) {
      this.$store.dispatch('rpayMyItemStore/deleteMediaItem', { item: this.item, id: mediaId }).then(() => {
        this.$emit('delete-cover')
      })
    },
    preserveWhiteSpace: function (content) {
      return '<span class="text-description" style="white-space: break-spaces;">' + content + '</span>'
    },
    targetItem: function () {
      return this.$store.getters[APP_CONSTANTS.KEY_TARGET_FILE_FOR_DISPLAY](this.item)
    }
  },
  computed: {
    txPending () {
      let transactions = []
      if (this.item.contractAsset) {
        transactions = this.$store.getters[APP_CONSTANTS.KEY_TX_PENDING_BY_TX_ID](this.item.contractAsset.nftIndex)
      } else {
        transactions = this.$store.getters[APP_CONSTANTS.KEY_TX_PENDING_BY_ASSET_HASH](this.item.assetHash)
      }
      return transactions
    },
    contractAsset () {
      const contractAsset = this.$store.getters[APP_CONSTANTS.KEY_ASSET_FROM_CONTRACT_BY_HASH](this.videoOptions.assetHash)
      return contractAsset
    },
    options () {
      const videoOptions = {
        emitOnHover: true,
        playOnHover: false,
        bigPlayer: true,
        assetHash: this.assetHash,
        autoplay: false,
        muted: false,
        controls: true,
        showMeta: false,
        poster: (this.item.attributes.coverImage) ? this.item.attributes.coverImage.fileUrl : null,
        sources: [
          { src: this.item.attributes.artworkFile.fileUrl, type: this.item.attributes.artworkFile.type }
        ],
        fluid: true
      }
      return videoOptions
    },
    item () {
      // get the item from my uploads - then try my nfts
      let item = this.$store.getters[APP_CONSTANTS.KEY_MY_ITEM](this.assetHash)
      if (this.edition > 0) {
        item = this.$store.getters[APP_CONSTANTS.KEY_GAIA_ASSET_BY_HASH_EDITION]({ assetHash: this.assetHash, edition: this.edition })
        if (!item) {
          item = this.$store.getters[APP_CONSTANTS.KEY_MY_ITEM](this.assetHash)
        }
      }
      return item
    },
    profile () {
      const profile = this.$store.getters['rpayAuthStore/getMyProfile']
      return profile
    },
    iAmOwner () {
      if (process.env.VUE_APP_NETWORK === 'local') {
        return this.item.contractAsset && (this.item.contractAsset.owner === 'STFJEDEQB1Y1CQ7F04CS62DCS5MXZVSNXXN413ZG' || this.item.contractAsset.owner === this.profile.stxAddress)
      }
      return this.item.contractAsset && this.item.contractAsset.owner === this.profile.stxAddress
    },
    minted () {
      // const profile = this.$store.getters['rpayAuthStore/getMyProfile']
      // return !this.item.contractAsset && this.item.contractAsset.owner === profile.stxAddress
      return this.item.contractAsset
    },
    attributes () {
      return this.item.attributes
    }
  }
}
</script>

<style lang=scss scoped>
.itemPreviewSection{
  background-color: white;
  height: 70vh;
}
.nFTInfo{
  position: absolute;
  background-color: #5154A1;
  color: white;
  padding: 10px 0 10px 30px;
  min-width: 175px;
  min-height: 100px;
  right: 0;
  top: 200px;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  span{
    color: white !important;
    font-size: bolder;
  }
  .spanDate{
    font-weight: 100;
    font-size: 13px;
  }
  .spanDate1{
    font-weight: 500;
    font-size: 20px;
  }
}

.itemPreviewSubContainer{
  max-width: 500px;
}
.backBtn{
  color: rgb(0, 0, 138);
  font-weight: 700;
}
.assetArtist{
  font-weight: 400;
  font-size: 16px;
}
.assetName{
  font-size: 40px;
  font-weight: 400;
  letter-spacing: 1.5px;
}
.itemPreviewBody{
  margin-top: 150px;
}
.itemPreviewContainer{
  display: flex;
  flex-wrap: wrap;
  /* gap: 60px; */
}
.itemPreviewContainer >*{
  margin: 0 auto;
  flex: 1 1 400px;
}
.itemPreviewContainerDetails{
  max-width: 900px;
}
.itemPreviewNFT{
  margin: auto;
  max-width: 400px;
  min-height: 500px;
  padding: 30px;
  background-color: #8181813f;
  border-radius: 30px;
}
.mintButton{
  display: block;
  width: 140px;
  margin: auto;
}

</style>
