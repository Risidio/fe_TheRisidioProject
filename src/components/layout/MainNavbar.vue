<template>
<div class=" navbar_container">
<b-navbar id="navbar">
   <img class="nav_banner" src="https://res.cloudinary.com/risidio/image/upload/v1632564338/Risidio.com/main_bg.svg" alt="">
  <b-navbar-toggle target="my-sidebar">
    <!--
    <template #default="{ expanded }">
      <b-icon v-if="expanded" icon="chevron-bar-up"></b-icon>
      <b-icon v-else icon="chevron-bar-down"></b-icon>
    </template>
    -->
    <b-button v-b-toggle:my-collapse>
      <span class="when-open"><b-icon icon="chevron-bar-up"></b-icon></span><span class="when-closed"><b-icon icon="chevron-bar-down"></b-icon></span>
    </b-button>
  </b-navbar-toggle>
  <!-- Mobile Design for login menu -->

  <b-sidebar id="my-sidebar" sidebar-class="border-left border-secondary" bg-variant="white" text-variant="dark" aria-label="Sidebar" right>
    <template #default="{ hide }">
      <div class="pb-5 mt-5 border-bottom text-center">
        <h1>Welcome</h1>
        <h2>{{username}}</h2>
      </div>
      <div class="p-3 mb-5 border-bottom text-left">
        <h4><b-icon class="mr-3" icon="arrow-up-right"/> Navigation</h4>
        <div class="d-flex justify-content-center">
          <nav class="mt-4 mx-auto">
            <b-nav vertical class="text-small">
              <b-nav-item v-if="profile" active><b-link to="/my-nfts"><b-icon class="mr-2 mb-2" icon="files"></b-icon>My NFTs</b-link></b-nav-item>
              <b-nav-item v-if="profile.superAdmin" active><b-link to="/app-admin"><b-icon class="mr-2 mb-2" icon="person"></b-icon> App Admin</b-link></b-nav-item>
              <b-nav-item @click="hide"><b-link @click.prevent="logout()"><b-icon icon="person" class="mr-2 mb-2"/> Logout</b-link></b-nav-item>
            </b-nav>
          </nav>
        </div>
        <div class="mt-4 d-flex justify-content-center">
          <b-button variant="warning" class="mr-2 mb-2"><b-link class="text-white" to="/create">Upload New Item</b-link></b-button>
        </div>
      </div>
      <div class="p-3 mb-5 border-bottom text-left">
        <h4 id="sidebar-no-header-title"><b-icon class="mr-3" icon="wallet2"/> Wallet</h4>
        <p class="text-center">
          <nav class="mb-3">
            <b-nav vertical>
              <b-nav-item><span>Address</span></b-nav-item>
              <b-nav-item><span class="stx-address">{{profile.stxAddress}}</span></b-nav-item>
              <b-nav-item v-if="profile.accountInfo" active><span>Balance: <span class="text-secondary">{{profile.accountInfo.balance}}</span> STX</span></b-nav-item>
            </b-nav>
          </nav>
        </p>
      </div>
      <div class="p-3 mb-5 border-bottom text-left">
        <div class="text2 mb-2">
          <router-link to="/admin-app"><b-icon class="mr-2" icon="gear" /> Developers</router-link>
        </div>
      </div>
    </template>
  </b-sidebar>
    <!--Mobile Navbar-->
    <div class = "mainNavbar">
      <router-link class="risidioLogo" to="/"><img width="150px;" :src="logo" alt="risidio-logo"/></router-link>
      <a href= "#" class = "toggle-button" v-on:click="mobileNavebar()">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </a>
      <div v-if="profile.loggedIn" class="navbar_links">
        <ul>
          <li><a class="nav-items" ><router-link class="text-white" to="/">Marketplace</router-link></a></li>
          <li><a class="nav-items"><router-link class="text-white" to="/how-it-works">How It Works</router-link></a></li>
          <li><a class="nav-items"><router-link class="text-white" to="/my-nfts">Your NFTs</router-link></a></li>
          <li><a class="nav-items"><router-link class="text-white" to="/create">Mint an NFT</router-link></a></li>
          <li><div><a class="nav-items" v-b-toggle.my-sidebar><b-icon icon="person"/>Account</a></div></li>
        </ul>
      </div>
      <div v-else class="navbar_links">
        <ul>
          <!-- <b-nav-item class="mr-5 mt-0 align-self-center"><router-link class="text-white" to="/nft-gallery">Public Gallery</router-link></b-nav-item> -->
          <li><a class="nav-items" ><router-link class="text-white" to="/">Marketplace</router-link></a></li>
          <li><a class="nav-items"><router-link class="text-white" to="/how-it-works">How It Works</router-link></a></li>
          <li><a class="nav-items" ><router-link class="text-white" to="/about">About Risidio </router-link></a></li>
          <button @click.prevent="startLogin()" href="#" id="login" class = "login">Login</button>
        </ul>
      </div>
    </div>
</b-navbar>
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
#navbar{
  padding-top: 20px;
}
nav.navbar {
  font-size: 1.5rem;
  width: 100%;
  padding-right: 20px;
  padding-left: 20px;
  position: absolute!important;
  top: 0;
  left: 0;
}
.nav_links{
  display:flex;
}
/* NAV ITEMS STYLE */
.nav-text a {
  font-weight: 700;
}
// .navbar-light .navbar-nav .nav-link {
//     color: #fff !important;
//     padding: 20px;
// }

.header_menu{
    color: #fff;
    background: transparent;
    padding: 10px;
    border-radius: 20px;
    margin-right: 10px;
}
.navbar-light .navbar-nav .nav-link {
    color: #fff !important;
}
.b-sidebar > .b-sidebar-header {
    padding: 50px 10px;
}
#login{
  border-radius: 50px;
  background-color: rgba(255, 255, 255, 0.192);
  min-width: 120px;
  min-height:50px;
  text-align: center;
  color: orange;
  border:none;
}

.mainNavbar{
  font-weight:300;
  display:flex;
  justify-content:space-between;
  align-items: center;
  color:white;
  width:100%;
}

.navbar_links{
  margin-left:auto;
}
.navbar_links ul{
  margin:0;
  padding:0;
  display:flex;
}
.navbar_links li{
  list-style: none;
  padding-top: 15px;
}
.navbar_links li a{
  text-decoration:none;
}
.nav-items{
  margin-right: 25px;
  margin-top: 0;
  color:white;
}
.toggle-button{
  position:absolute;
  top: 3.7rem;
  right: 3.5rem;
  display:none;
  flex-direction:column;
  justify-content: space-between;
  width: 30px;
  height: 21px;
}
.toggle-button .bar{
  height: 2px;
  width:90%;
  background-color: #fff;
  border-radius:10px;
}

//Styling for mobile responsiveness
@media only screen and (max-width: 900px){
  .toggle-button{
    display:flex;
  }
  .mainNavbar{
    flex-direction:column;
    align-items: flex-start;
  }
  .navbar_links{
    display:none;
    width:100%;
  }
  .navbar_links ul{
    width:100%;
    flex-direction: column;
  }
  .navbar_links li {
    text-align: center;
    margin:10px;
    padding: 10px;
    align-self: center;
  }
  .nav-items {
    margin:10px;
  }

  #login{
    min-width: 200px;
    margin-left:auto;
    margin-right:auto;
    margin-top: 20px;
  }
  .navbar_links.active {
    padding: 15px;
    display:flex;
  }
  .mainNavbar.active{
    // position:absolute;
    // z-index: 20;
    transition:all ease-in .1s;
    padding-top: 50px;
    background: linear-gradient(#261399,#13086c);
    align-items: center;
    padding-bottom: 100px;
    margin-top: -20px;
  }
}
</style>
