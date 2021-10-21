<template>
<div>
<div class="navbar_container">
   <img class="nav_banner" src="https://res.cloudinary.com/risidio/image/upload/v1633609222/RisidioMarketplace/gradienta-m_-1_v4hs5p.svg" alt="">
</div>
  <div class = "mainNavbar">
        <router-link class="risidioLogo" to="/"><img width="150px;" :src="logo" alt="risidio-logo"/></router-link>
        <div v-if="profile.loggedIn" class="navbar_links">
          <a class="nav-items"> <router-link class="text-white" to="/how-it-works">How It Works</router-link></a>
          <a class="nav-items"> <router-link class="text-white" to="/about">About Risidio </router-link></a>
          <a class="nav-items"> <router-link class="text-white" to="/about"> <span class="navBtn"> My NFT's </span></router-link></a>
        </div>
  </div>
</div>
</template>

<script>
import { APP_CONSTANTS } from '@/app-constants'

export default {
  name: 'MainNavbar',
  components: {
  },
  data () {
    return {
      query: null,
      banner: 'https://images.prismic.io/digirad/6e5bb3a5-21b7-4bcb-b5a7-85128b6e6e8a_Rumba_bg_small.png?auto=compress,format',
      // logo: 'https://images.prismic.io/digirad/136adb87-c542-4439-8bcc-7199007290cc_Groupe+16068%402x.png?auto=compress,format',
      logo: require('@/assets/img/risidio_white_logo.svg')
      // logo1: require('@/assets/img/risidio_white_logo.svg')
    }
  },
  methods: {
    logout () {
      // this.$emit('updateEventCode', { eventCode: 'connect-logout' })
      this.$store.dispatch('rpayAuthStore/startLogout').then(() => {
        if (this.$route.name !== 'home') {
          this.$router.push('/')
        }
        // sessionStorage.clear()
      }).catch((err) => {
        console.log(err)
        this.$store.commit(APP_CONSTANTS.SET_WEB_WALLET_NEEDED)
      })
    },
    startLogin () {
      // this.$emit('updateEventCode', { eventCode: 'connect-login' })
      const myProfile = this.$store.getters['rpayAuthStore/getMyProfile']
      if (myProfile.loggedIn) {
        this.$emit('connect-login', myProfile)
      } else {
        this.$store.dispatch('rpayAuthStore/startLogin').then((profile) => {
          console.log(profile)
          location.reload()
        }).catch(() => {
          this.$store.commit(APP_CONSTANTS.SET_WEB_WALLET_NEEDED)
        })
      }
    },
    mobileNavebar () {
      const navbarLinks = document.getElementsByClassName('navbar_links')[0]
      const mainNavbar = document.getElementsByClassName('mainNavbar')[0]
      navbarLinks.classList.toggle('active')
      mainNavbar.classList.toggle('active')
    },
    startRegister () {
      window.open('https://www.hiro.so/wallet', '_blank')
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
    // bannerImage () {
    //   return {
    //     height: '128px',
    //     width: '100%',
    //     'background-repeat': 'no-repeat',
    //     'background-image': `url(${this.banner})`,
    //     'background-position': 'center center',
    //     '-webkit-background-size': 'cover',
    //     '-moz-background-size': 'cover',
    //     '-o-background-size': 'cover',
    //     'background-size': 'cover'
    //   }
    // },
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
    }
  }
}
</script>

<style lang="scss">
.mainNavbar{
  display: flex;
  width: 90vw;
  margin: auto;
  margin-top: 20px;
}
/* NAVBAR PADDING AND WIDTH */
.nav_banner{
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 128px;
  object-fit: cover;
  z-index: -11;
}

.navbar_links{
  justify-content: flex-end;
  align-content: flex-end;
  display: flex;
  flex-direction: row;
  width: 100%;
}

.nav-items{
  padding: 20px;
}

</style>
