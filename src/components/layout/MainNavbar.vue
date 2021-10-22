<template>
<div>
<div class="navbar_container">
   <img class="nav_banner" src="https://res.cloudinary.com/risidio/image/upload/v1633609222/RisidioMarketplace/gradienta-m_-1_v4hs5p.svg" alt="">
</div>
  <div class = "mainNavbar">
        <router-link class="risidioLogo" to="/"><img width="150px;" :src="logo" alt="risidio-logo"/></router-link>
        <div v-if=" profile.loggedIn" class="navbar_links">
          <div class="nav-start">
            <router-link class="nav-items" to="/">Gallery</router-link>
            <router-link class="nav-items" to="/">Collections</router-link>
          </div>
          <div class="nav-end">
            <router-link class="nav-items nav-end text-white" to="/how-it-works">How It Works</router-link>
            <router-link class="nav-items nav-end text-white" to="/about">About Risidio </router-link>
            <router-link class="nav-items nav-end navBtn" to="/my_account"> My NFT's </router-link>
          </div>
        </div>
         <div v-else class="navbar_links_not_logged">
          <router-link class="nav-items text-white" to="/how-it-works">How It Works</router-link>
          <router-link class="nav-items text-white" to="/about">About Risidio </router-link>
          <div @click.prevent="startLogin(); events();" id="login" class ="login nav-items text-white">Login</div>
          <div @mouseover="isHidden = !isHidden" @blur="isHidden = !isHidden" class=" nav-items navBtn text-white" id="register" v-on:click="startRegister()"> Register <div v-show="isHidden" class="registerTooltip"> Click Register to download the Hiro Wallet extension and get started!</div></div>
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
      logo: require('@/assets/img/risidio_white_logo.svg'),
      isHidden: false
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
    events () {
      console.log('clicked')
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

.nav-start{
  justify-items: flex-start;
  align-items: flex-start;
  color: white;
  padding: 20px;
  font-size: 1.2rem;
}
// .nav-start:hover{
//   color: white;
// }
.nav-end{
  justify-self: flex-end;
  align-self: flex-end;
  color: white;
  padding: 20px;
  font-size: 1.2rem;
  width:fit-content;
}
.nav-start:hover{
  color: white;
}

.navbar_links_not_logged{
  justify-content: flex-end;
  align-content: flex-end ;
  display: flex;
  flex-direction: row;
  width: 100%;
}
.navbar_links{
  justify-content: space-between;
  align-content: space-between ;
  display: flex;
  flex-direction: row;
  width: 100%;
}

.nav-items{
    padding: 20px;
    width: 12rem;
    font-size: 1.2rem;
    text-align: center;
    margin-top: 0px;
    color: white;
    cursor: pointer;
}
.nav-items:hover{
  color: white;
}

.navBtn{
  background: rgba(255, 255, 255, 0.247);
  padding: 15px;
  border-radius: 50px;
  margin-top: 8px;
  margin-left: 5px;
  width: 12rem;
  height: 4rem;
  text-align: center;
  justify-items: center;
  align-items: center;
  color: #5FBDC1;
  cursor: pointer;
  outline: none;
}
.navBtn:hover{
  color: white;
}

.registerTooltip{
  margin-top: 25px;
  margin-left: -10.2rem;
  width: 20rem;
  padding: 10px;
  background: rgb(234, 247, 255);
  color: black;
  border-radius: 15px;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    cursor: default;
}

</style>
