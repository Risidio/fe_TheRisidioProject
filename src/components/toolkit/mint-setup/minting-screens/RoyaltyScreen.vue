<template>
<div deck v-if="item" class="card">
  <div v-if="royalties == false">
    <div class="text-center" header-tag="header" footer-tag="footer">
    <!-- <header-screen :allowEdit="false" :item="item"/> -->
    <ItemDisplay class="my-5" :item="item"/>
    <div class="d-flex justify-content-center"><p class="w-50 py-3 px-5 mb-5 royaltyButton"><a class="text-white" style="display: block; margin: 5px auto" href="#" @click="royalties = !royalties">Set Your Royalties</a></p></div>
    <!-- <beneficiaries class="mb-5 text-left" v-if="showBeneficiaries" :beneficiaries="beneficiaries" v-on="$listeners" :item="item"/> -->
    <div class="my-4 text-danger" v-html="errorMessage"></div>
      <div class="royaltyButtons">
        <button @click="saveData()" class="rButton">save mint later</button>
        <button @click="sendMintEvent()" v-if="allowMint()"  class="rButton">mint now</button>
      </div>
      </div>
  </div>
  <div v-else>
    <!-- <div class="d-flex justify-content-center"><p class="w-50 py-3 px-5 mb-5 royaltyButton"><a class="text-white" style="display: block; margin: 5px auto" href="#" @click="royalties = !royalties">Set Your Royalties</a></p></div> -->
    <h1 class="h1-modal" style="text-align: center; margin-bottom: 80px;">Set Your Royalties</h1>
    <beneficiaries class="mb-5 text-left" v-if="showBeneficiaries" :beneficiaries="beneficiaries" v-on="$listeners" :item="item"/>
    <div class="my-4 text-danger" v-html="errorMessage"></div>
      <div class="royaltyButtons">
        <button @click="saveData()" class="rButton">save mint later</button>
        <button @click="sendMintEvent()" v-if="allowMint()"  class="rButton">mint now</button>
      </div>
  </div>
</div>
</template>

<script>
import { APP_CONSTANTS } from '@/app-constants'
import Beneficiaries from './Beneficiaries'
import ItemDisplay from './ItemDisplay'

export default {
  name: 'RoyaltyScreen',
  components: {
    Beneficiaries,
    ItemDisplay
  },
  props: ['item', 'beneficiaries', 'errorMessage'],
  data () {
    return {
      mintedMessage: null,
      showBeneficiaries: true,
      royalties: false
    }
  },
  methods: {
    rangeEvent (displayCard) {
      this.$store.commit('rpayStore/setDisplayCard', displayCard)
    },
    getRangeValue () {
      const displayCard = this.$store.getters[APP_CONSTANTS.KEY_DISPLAY_CARD]
      if (displayCard === 100) return 0
      else if (displayCard === 102) return 1
      else if (displayCard === 104) return 2
    },
    allowMint () {
      let sum = 0
      this.beneficiaries.forEach((o) => {
        sum += o.royalty
      })
      sum = Math.round(sum * 100) / 100
      return sum === 100.00
    },
    saveData: function () {
      const configuration = this.$store.getters[APP_CONSTANTS.KEY_CONFIGURATION]
      configuration.opcode = 'cancel-minting'
      window.eventBus.$emit('rpayEvent', configuration)
    },
    sendMintEvent: function () {
      this.$emit('mintToken')
    }
  },
  computed: {
    displayCard () {
      const displayCard = this.$store.getters[APP_CONSTANTS.KEY_DISPLAY_CARD]
      return displayCard
    },
    enableRoyalties () {
      // const configuration = this.$store.getters[APP_CONSTANTS.KEY_CONFIGURATION]
      return true
    }
  }
}
</script>
<style lang="scss" scoped>
.royaltyButton{
  background-color: rgba(80, 177, 181, 1);
  height: 50px;
  border-radius: 50px;
}
.royaltyButtons{
  display: flex;
  justify-content: space-between;
  margin-bottom: 50px;
}
.rButton{
  color: black;
  width: 200px;
  height: 45px;
  border-radius: 15px;
  border: none;
  font-size: 12px;
  font-weight:700;
}
.card{
  min-width: 450px;
  padding: 20px;
}
</style>
