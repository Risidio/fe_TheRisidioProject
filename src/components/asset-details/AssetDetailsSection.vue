<template>
<section :key="componentKey" id="asset-details-section" v-if="gaiaAsset && gaiaAsset.contractAsset" class="mt-5">
  <div class="assetMainContainer">
    <div><router-link class="backBtn" to="/"><b-icon icon="chevron-left" shift-h="-3"></b-icon> Back </router-link></div>
    <div class="itemContainer">
      <!-- <div ><a href="#" @click.prevent="back()" ><b-icon icon="chevron-left"/> Back</a></div> -->
      <div class = "assetContainer">
        <div id="video-column" class="assetView">
          <MediaItem :videoOptions="videoOptions" :attributes="gaiaAsset.attributes" :targetItem="targetItem()"/>
        </div>
      </div>
      <div>
        <div class = "assetTop">
          <h1 class= "assetName"> {{gaiaAsset.name}}</h1>
        </div>
        <div>
        <h2 class= "assetArtist">By: <span>{{gaiaAsset.artist}}</span></h2>
        <div class = "priceContainer">
        <div class = "price">
          <h1> <span1>PRICE</span1>  <span3>{{gaiaAsset.contractAsset.saleData.buyNowOrStartingPrice}}</span3> <span2>STX &nbsp;</span2></h1>
          <hr>
          </div>
          <button class="button" variant="success" @click="openPurchaceDialog()" v-on:click="assetDeets()">{{salesButtonLabel}}</button>
        </div>
        <div class = "assetDetails">
          <!-- <div class="w-100" v-html="preserveWhiteSpace(gaiaAsset.description)">{{gaiaAsset.description}}</div> -->
          <div class="w-100"> <p class="assetText">{{gaiaAsset.description}}....and Lorem ipsum dolor sit amet, consectetur adipiscing elit,
              sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
              tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
              quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Ut enim ad minim</p></div>
        </div>
            <div class="w-100">
              </div>
              <MintInfo :item="gaiaAsset" />
              <PendingTransactionInfo v-if="txPending.length > 0" :contractAsset="gaiaAsset.contractAsset" :assetHash="gaiaAsset.assetHash"/>
        </div>
      </div>
      <div class = "assetRight">
          <div class = "sale" v-if="isOwner"><router-link class="" to="/my-nfts">{{salesBadgeLabel}}</router-link></div>
          <div class = "sale" v-else>{{salesBadgeLabel}}</div>
          <div class = "sale" v-if="showEndTime()">{{biddingEndTime()}}</div>
        </div>
    </div>
  <b-modal size="lg" id="asset-offer-modal" class="text-left">
    <PurchaseFlow v-if="showRpay === 1" :gaiaAsset="gaiaAsset" :forceOfferFlow="forceOfferFlow"/>
    <AssetUpdatesModal v-if="showRpay === 2" @registerForUpdates="registerForUpdates"/>
    <template #modal-header="{ close }">
      <div class=" text-warning w-100 d-flex justify-content-end">
        <b-button size="sm" variant="white" @click="close()"  class="m-0 p-1 text-dark" style="max-width: 30px !important; max-height: 30px !important;">
          <b-icon icon="x"/>
        </b-button>
      </div>
    </template>
    <template #modal-footer>
      <div class="w-100 d-flex justify-content-between">
      </div>
    </template>
  </b-modal>
  <b-modal id="result-modal" class="">
    <b-row>
      <b-col cols="12" class="w-50">
        <h1>Confirmation</h1>
        <h4 class="text-center mb-5"><a :href="transactionUrl" target="_blank">Transaction sent to Stacks Blockchain</a></h4>
        <p v-if="txData">Transaction Status: {{txData.txStatus}}</p>
      </b-col>
    </b-row>
    <template #modal-footer class="text-center">
    </template>
  </b-modal>
  </div>
</section>
</template>

<script>
import Vue from 'vue'
import AssetUpdatesModal from './offers/AssetUpdatesModal'
import PurchaseFlow from './offers/PurchaseFlow'
import { APP_CONSTANTS } from '@/app-constants'
import MediaItem from '@/components/upload/MediaItem'
import moment from 'moment'
import utils from '@/services/utils'
import MintInfo from '@/components/toolkit/mint-setup/MintInfo'
import PendingTransactionInfo from '@/components/toolkit/PendingTransactionInfo'

export default {
  name: 'AssetDetailsSection',
  components: {
    PendingTransactionInfo,
    AssetUpdatesModal,
    PurchaseFlow,
    MediaItem,
    MintInfo
  },
  props: ['gaiaAsset'],
  data: function () {
    return {
      forceOfferFlow: false,
      cross: require('@/assets/img/navbar-footer/cross.svg'),
      showRpay: 0,
      videoHeight: 0,
      componentKey: 0,
      showHash: false,
      assetHash: null,
      txData: null
    }
  },
  watch: {
    '$route' () {
      this.assetHash = this.$route.params.assetHash
      this.componentKey++
    }
  },
  mounted () {
    this.assetHash = this.$route.params.assetHash
    const $self = this
    this.$store.commit(APP_CONSTANTS.SET_RPAY_FLOW, { flow: 'purchase-flow', asset: this.gaiaAsset })
    this.resizeContainers()
    if (window.eventBus && window.eventBus.$on) {
      window.eventBus.$on('rpayEvent', function (data) {
        $self.txData = data
        if (data.opcode.indexOf('stx-transaction-sent') > -1 || data.opcode.indexOf('stx-transaction-update') > -1) {
          $self.$bvModal.hide('asset-offer-modal')
          if (data.txStatus === 'success') {
            // $self.$notify({ type: 'success', title: 'Buy Now', text: 'Congratulations! This NFT is now yours - redirecting to your NFT Library. ' })
          } else if (data.txStatus === 'pending') {
            // $self.$notify({ type: 'warning', title: 'Buy Now', text: 'Buy Now In Progress. ' })
          } else {
            $self.$notify({ type: 'error', title: 'Buy Now', text: 'Transaction failed: ' + data.txStatus })
          }
          $self.$bvModal.show('result-modal')
        }
      })
    }
    Vue.nextTick(function () {
      const vid = document.getElementById('video-column')
      if (vid) this.videoHeight = vid.clientHeight
    }, this)
  },
  methods: {
    update (data) {
      this.$store.commit('setModalMessage', 'Request to mint an edition sent to the blockchain.<br/> Transaction Id: ' + data.txId)
      this.$root.$emit('bv::show::modal', 'success-modal')
    },
    mintedEvent () {
      this.componentKey++
    },
    resizeContainers () {
      let resizeTimer
      const $self = this
      window.addEventListener('resize', function () {
        clearTimeout(resizeTimer)
        resizeTimer = setTimeout(function () {
          const vid = document.getElementById('video-column')
          if (vid) $self.videoHeight = vid.clientHeight
        }, 400)
      })
    },
    formatNumber: function (number) {
      return utils.formatNumber(number)
    },
    preserveWhiteSpace: function (content) {
      return '<span class="text-description" style="white-space: break-spaces;">' + content + '</span>'
    },
    back: function () {
      this.$bvModal.hide('result-modal')
    },
    showEndTime: function () {
      return this.gaiaAsset.contractAsset.saleData.saleType === 2
    },
    biddingEndTime: function () {
      return moment(this.gaiaAsset.contractAsset.saleData.biddingEndTime).format('ddd, MMMM Do, h:mma') + ' BST'
    },
    targetItem: function () {
      return this.$store.getters[APP_CONSTANTS.KEY_TARGET_FILE_FOR_DISPLAY](this.gaiaAsset)
    },
    dimensions () {
      const dims = { width: '100%', height: '100%' }
      return 'max-width: ' + dims.height + '; max-height: ' + dims.height + ';'
    },
    poster: function () {
      if (this.gaiaAsset.attributes.coverImage) {
        return this.gaiaAsset.attributes.coverImage.fileUrl
      }
    },
    getArtist: function () {
      if (this.gaiaAsset.artist) {
        return this.gaiaAsset.artist
      } else if (this.gaiaAsset.owner) {
        return this.gaiaAsset.owner.substring(0, this.gaiaAsset.owner.indexOf('.'))
      } else {
        return 'Unknown Artist'
      }
    },
    openUpdates: function () {
      this.showRpay = 2
      this.$bvModal.show('asset-offer-modal', { assetHash: this.assetHash })
    },
    getSaleType: function () {
      return this.gaiaAsset.contractAsset.saleData.saleType
    },
    openPurchaceDialog: function () {
      this.forceOfferFlow = false
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (!profile.loggedIn && this.gaiaAsset.contractAsset.saleData.saleType !== 3) {
        this.$store.dispatch('rpayAuthStore/startLogin').then(() => {
          this.$emit('registerByConnect')
        }).catch((err) => {
          console.log(err)
          // https://chrome.google.com/webstore/detail/stacks-wallet/ldinpeekobnhjjdofggfgjlcehhmanlj
          this.webWalletNeeded = true
        })
      } else {
        if (this.gaiaAsset.contractAsset.saleData.saleType === 0) {
          return
        }
        this.showRpay = 1
        this.$bvModal.show('asset-offer-modal')
      }
    },
    openOfferPurchaceDialog: function () {
      this.forceOfferFlow = true
      this.showRpay = 1
      this.$bvModal.show('asset-offer-modal')
    },
    emailText () {
      const emailText = this.$store.getters[APP_CONSTANTS.KEY_EMAIL_TEXT]('registeremail')
      const answer = (emailText) ? emailText[0].text : 'Interest Registered'
      return answer
    },
    registerForUpdates: function (email) {
      const data = {
        emailContent: this.emailText(),
        status: 1,
        domain: location.host,
        assetHash: this.assetHash,
        email: email
      }
      this.$store.dispatch('rpayPurchaseStore/registerForUpdates', data).then(() => {
        this.$store.commit('setModalMessage', 'Thanks for registering an interest - we will keep you updated.')
        this.showRpay = 0
        this.$bvModal.hide('asset-offer-modal')
        this.$root.$emit('bv::show::modal', 'success-modal')
      }).catch(() => {
        this.$store.commit('setModalMessage', 'Thanks for re-registering an interest - we will keep you updated.')
        this.showRpay = 0
        this.$bvModal.hide('asset-offer-modal')
        this.$root.$emit('bv::show::modal', 'success-modal')
      })
    },
    assetDeets () {
      console.log(this.gaiaAsset)
    }
  },
  computed: {
    transactionUrl: function () {
      if (!this.gaiaAsset.mintInfo || !this.gaiaAsset.mintInfo.txId) return '#'
      const stacksApiUrl = process.env.VUE_APP_STACKS_EXPLORER
      return stacksApiUrl + '/txid/' + this.gaiaAsset.mintInfo.txId + '?chain=' + process.env.VUE_APP_NETWORK
    },
    txPending () {
      let transactions = []
      if (this.gaiaAsset.contractAsset) {
        transactions = this.$store.getters[APP_CONSTANTS.KEY_TX_PENDING_BY_TX_ID](this.gaiaAsset.contractAsset.nftIndex)
      } else {
        transactions = this.$store.getters[APP_CONSTANTS.KEY_TX_PENDING_BY_ASSET_HASH](this.gaiaAsset.assetHash)
      }
      return transactions
    },
    editionsAvailable: function () {
      return this.gaiaAsset.contractAsset.editionCounter < this.gaiaAsset.contractAsset.tokenInfo.maxEditions
    },
    webWalletLink () {
      if (this.$browserDetect.isFirefox) {
        return this.$store.getters[APP_CONSTANTS.KEY_WEB_WALLET_LINK_FIREFOX]
      }
      return this.$store.getters[APP_CONSTANTS.KEY_WEB_WALLET_LINK_CHROME]
    },
    usdAmount () {
      try {
        const tickerRates = this.$store.getters[APP_CONSTANTS.KEY_TICKER_RATES]
        const rate = tickerRates.find((o) => o.currency === 'USD')
        const offer = this.$store.getters[APP_CONSTANTS.KEY_HIGHEST_OFFER_ON_ASSET](this.gaiaAsset.contractAsset.tokenInfo.assetHash)
        const currentOffer = (offer && offer.amount) ? offer.amount : 0
        const minimumOffer = Math.max(currentOffer, (this.gaiaAsset.contractAsset.saleData.reservePrice))
        const amountUsd = Number(utils.toDecimals(rate.stxPrice * minimumOffer)).toLocaleString()
        return 'Current highest offer: ' + amountUsd + ' USD'
      } catch (e) {
        return null
      }
    },
    currentCost: function () {
      return this.gaiaAsset.contractAsset.tokenInfo.editionCost
    },
    salesButtonLabel () {
      if (this.webWalletNeeded) return 'GET WALLET'
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (!profile.loggedIn) return 'LOGIN'
      // let label = 'BUY NOW ' + this.gaiaAsset.contractAsset.saleData.buyNowOrStartingPrice + ' STX'
      let label = 'BUY IT NOW'
      if (this.gaiaAsset.contractAsset.saleData.saleType !== 1) {
        label = 'NOT FOR SALE'
      }
      return label
    },
    salesBadgeLabel () {
      if (this.webWalletNeeded) return 'GET WALLET'
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (!profile.loggedIn) return 'LOGIN'
      // let label = 'BUY NOW ' + this.gaiaAsset.contractAsset.saleData.buyNowOrStartingPrice + ' STX'
      let label = 'ON SALE'
      if (this.gaiaAsset.contractAsset.saleData.saleType !== 1) {
        label = 'NOT ON SALE'
      }
      return label
    },
    configuration () {
      const configuration = this.$store.getters[APP_CONSTANTS.KEY_RPAY_CONFIGURATION]
      return configuration
    },
    videoOptions () {
      const videoOptions = {
        emitOnHover: true,
        playOnHover: false,
        bigPlayer: true,
        assetHash: this.assetHash,
        autoplay: false,
        muted: false,
        controls: true,
        showMeta: false,
        dimensions: 'max-width: 100%; max-height: auto;',
        aspectRatio: '1:1',
        poster: (this.gaiaAsset.attributes.coverImage) ? this.gaiaAsset.attributes.coverImage.fileUrl : null,
        sources: [
          { src: this.gaiaAsset.attributes.artworkFile.fileUrl, type: this.gaiaAsset.attributes.artworkFile.type }
        ],
        fluid: false
      }
      return videoOptions
    },
    isOwner: function () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (!this.gaiaAsset.contractAsset || !profile || !profile.loggedIn) return false
      return profile.stxAddress === this.gaiaAsset.contractAsset.owner
    },
    owner () {
      return this.gaiaAsset.contractAsset.owner
    },
    webWalletNeeded () {
      const webWalletNeeded = this.$store.getters[APP_CONSTANTS.KEY_WEB_WALLET_NEEDED]
      return webWalletNeeded
    }
  }
}
</script>

<style>
.on-auction-text {
  text-transform: capitalize;
  font-weight: 500;
  font-size: 1.1rem;
}
#asset-offer-modal .modal-content {
  text-align: left !important;
}
.assetMainContainer{
  margin: 0 auto;
  max-width: 85%
}
.backBtn{
  color: rgb(0, 0, 138);
  font-weight: 700;
}
.assetContainer{
  min-width: 400px;
  min-height: 600px;
  /* background-color: rgb(185, 185, 185); */
}
.itemContainer{
  margin-top: 4%;
  display: flex;
  gap: 50px;
}

.button{
  width: 300px;
  height: 50px;
  border-radius: 100px;
  border: none;
  background-color: #50B1B5;
  font-size: 12px;
  font-weight:700;
  color: white;
}
.sale{
  background: #5154A1;
  color: white;
  font-weight: 500;
  font-size: 14px;
  text-align: center;
  padding: 20px;
  width: 150px;
  margin-left: auto;
}
.assetTop{
  display:flex;
}
.assetRight{
  margin-right: auto;
}
.assetName{
  font-size: 40px;
  font-weight: 400;
  letter-spacing: 1.5px;
}
.assetArtist{
  font-weight: 400;
  font-size: 16px;
}
span{
  color: #50B1B5;
  font-weight: bolder;
}
span1{
  font-weight: 200;
  font-size: 20px;
}
span2{
  float: right;
  font-weight: 200;
}
span3{
  float: right;
  color: #5154A1;
}
.price{
  width: 70%;
}
.priceContainer{
  margin-top: 100px;
  display:flex;
  gap: 10px;
}
.assetDetails{
  color: black;
  margin-top: 100px;
}
.assetText{
  max-width: 730px;
  font-size: 14px;
}
hr{
  background-color: #5154A1;
  height: 2px;
}
@media only  screen and (max-width: 1225px){
  .itemContainer{flex-direction: column;}
  .assetContainer{width: 100%; min-height: auto;}
  .assetText{min-width: 100%; font-size: 14px;}
}

</style>
