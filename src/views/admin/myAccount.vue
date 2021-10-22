<template>
  <div class="viewContainer">
    <div class="profileContainer">
        <div class="profile">
          <img class="profileImg" src="https://res.cloudinary.com/risidio/image/upload/v1634901154/RisidioMarketplace/istockphoto-464736044-612x612_h8c5kq.jpg" alt="">
          <span class="pencil">&#9998;</span>
        </div>
        <div class="walletDetails">
          <h1>Your Wallet Information:</h1>
          <h2>Wallet: {{username}}</h2>
          <p>Balance: {{profile.accountInfo.balance}} stx</p>
          <div class="profileBtn">Disconnect</div>
        </div>
    </div>
    <div>
        <b-container class="mt-5" v-if="loaded">
          <!-- <h1>My Library</h1> -->
          <b-tabs justified content-class="mb-5">
            <b-tab :title="'NFTs (' + hasNfts + ')'" active>
              <p class="mt-4">NFTs you currently own - these may be files you
                uploaded and minted and still own or NFTs you bought from other
                users or that were transferred to you. They also include editions
                of NFT files you minted.</p>
              <b-row>
                <b-col class="text-center" v-for="(myNft, index1) in myNfts" :key="index1" lg="3" md="4" sm="6" xs="12">
                  <MySingleNft class="mb-2" :item="myNft" :token="myTokens[index1]"/>
                </b-col>
              </b-row>
            </b-tab>
            <b-tab :title="'Uploads'">
              <p class="mt-4">Files you uploaded to your Gaia storage bucket.</p>
              <p>If you minted them (to create NFTs) you may also have
                sold or transferred the NFT to another wallet. </p>
              <b-row>
                <b-col v-for="(gaiaAsset, index) in gaiaAssets" :key="index" lg="3" md="6" sm="6" xs="12">
                  <MySingleNft class="mb-2" :item="gaiaAsset"/>
                </b-col>
              </b-row>
            </b-tab>
          </b-tabs>
        </b-container>
        <div class="container" style="min-height: 85vh;" v-else>
          <b-container class="mt-5">
            <h1>No NFTs</h1>
            <p>Upload a file and mint it to create your first NFT</p>
          </b-container>
        </div>
    </div>
  </div>
</template>

<script>
// import GalleryNft from '@/components/marketplace/GalleryNft'
import MySingleNft from '@/components/upload/MySingleNft'
import { APP_CONSTANTS } from '@/app-constants'
const STX_CONTRACT_ADDRESS = process.env.VUE_APP_STACKS_CONTRACT_ADDRESS
const STX_CONTRACT_NAME = process.env.VUE_APP_STACKS_CONTRACT_NAME

export default {
  name: 'MyNfts',
  components: {
    MySingleNft
  },
  data () {
    return {
      loaded: false,
      myNfts: [],
      myMintingNfts: []
    }
  },
  mounted () {
    this.data = { stxAddress: this.profile.stxAddress, mine: true }
    const myContractAssets = this.$store.getters[APP_CONSTANTS.KEY_MY_CONTRACT_ASSETS]
    for (let i = 0; i < myContractAssets.length; i++) {
      const ga = this.$store.getters[APP_CONSTANTS.KEY_GAIA_ASSET_BY_HASH](myContractAssets[i].tokenInfo.assetHash)
      ga.contractAsset = Object.assign({}, myContractAssets[i])
      this.myNfts.push(ga)
    }
    this.loaded = true
  },
  methods: {
    canUpload () {
      const hasUploadPriv = this.$store.getters[APP_CONSTANTS.KEY_HAS_PRIVILEGE]('can-upload')
      return hasUploadPriv
    },
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
    },
    closeModal () {
      document.getElementById('linkModal').style.display = 'none'
    }
  },
  computed: {
    projects () {
      const appmap = this.$store.getters[APP_CONSTANTS.KEY_REGISTRY]
      if (appmap) return appmap.apps
      return []
    },
    content () {
      const content = this.$store.getters['contentStore/getHomepage']
      return content
    },
    showAdmin () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      return profile.superAdmin || location.origin.indexOf('local') > -1
    },
    stxAddress () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (profile.wallet && profile.wallet.keyInfo.address) {
        return profile.wallet.keyInfo.address.substring(0, 5) + '...' + profile.wallet.keyInfo.address.substring(profile.wallet.keyInfo.address.length - 5)
      }
      return 'n/a'
    },
    username () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (!profile) {
        return 'unknown'
      } else if (profile.name && profile.name.length > 0) {
        return profile.name
      } else if (profile.username && profile.username.length > 0) {
        return profile.username
      } else {
        return profile.identityAddress
      }
    },
    avatar () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      if (profile.loggedIn) {
        if (profile.avatarUrl) {
          return (
            '<img style="width: 30px; height: 30px; border-radius: 20px;" src="' +
            profile.avatarUrl +
            '"/>'
          )
        }
      }
      return null
    },
    profile () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      return profile
    },
    webWalletNeeded () {
      const webWalletNeeded = this.$store.getters[APP_CONSTANTS.KEY_WEB_WALLET_NEEDED]
      return webWalletNeeded
    },
    gaiaAssets () {
      const gaiaAssets = this.$store.getters[APP_CONSTANTS.KEY_MY_ITEMS]
      return gaiaAssets
    },
    hasNfts () {
      const myContractAssets = this.$store.getters[APP_CONSTANTS.KEY_MY_CONTRACT_ASSETS]
      return myContractAssets && myContractAssets.length
    },
    haspProfile () {
      const profile = this.$store.getters[APP_CONSTANTS.KEY_PROFILE]
      return profile
    },
    myTokens () {
      const myContractAssets = this.$store.getters[APP_CONSTANTS.KEY_MY_CONTRACT_ASSETS]
      return myContractAssets
    }
  }
}
</script>

<style lang="scss" scoped>

.viewContainer{
  margin: auto;
  width: 80vw;
  height: 100%;

  .galleryNav{
  margin: auto;
  margin-top: 50px;
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
  width: fit-content;
  padding: 10px;
  margin: auto;
  border: solid rgba(255, 255, 255, 0)  2px;
}

.galleryNavItem:hover{
    border-bottom: 2px solid #50B1B5;
}
}

.profileContainer{
  display: flex;
  justify-content: space-between;
}

.profile{
  display: flex;
}

.profileImg{
  width: 20rem;
  height: 20rem;
  border-radius: 50%;
  object-fit: cover;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
}

.pencil{
  background: rgb(240, 240, 240);
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  text-align: center;
  margin-top: 3px;
  margin-left: -5rem;
  box-shadow: rgba(0, 0, 0, 0.15) 1.95px 1.95px 2.6px;
  transform: rotateZ(90deg);
  font-size: 2rem;
}

.walletDetails{
  text-align: center;
  padding: 20px;
  width: 60rem;
  height: 30rem;
  background: #c5c5c518;
  border-radius: 20px;
box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
  & > h1 {
    margin: 20px auto;
  }
  & > h2 {
    margin: 20px auto;
    color: navy;
    font-size: 1.2rem;
    font-weight: 600;
  }
  & > .profileBtn{
    margin: auto;
    margin-top: 8rem;
    background: #80576315;
    border-radius: 20px;
    width: 13rem;
    padding: 10px;
    color: orange;
  }

}
</style>
