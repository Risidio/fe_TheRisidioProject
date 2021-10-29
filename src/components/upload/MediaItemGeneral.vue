<template>
  <div>
    <div class="singleNftGalleryContainer">
      <div class="singleNftGalleryHolder">
        <div v-if="contentType === 'threed'" id="video-demo-container" class="singleNFTGalleryItem">
          <img v-on="$listeners" :src="options.poster" @error="setAltImg()" :alt="mediaItem.artworkFile.name"/>
        </div>
        <div v-else-if="contentType === 'video'" id="video-demo-container" class="singleNFTGalleryItem">
            <VideoJsPlayer :class="classes" class="singleNFTGalleryItem" v-on="$listeners"  @error="setAltImg()" :options="options"/>
        </div>
        <div v-else-if="contentType === 'audio'" id="audio-demo-container" class="singleNFTGalleryItem">
            <img :class="classes" class="singleNFTGalleryItem" v-on="$listeners" :src="mediaItem.fileUrl" @error="setAltImg()" :alt="mediaItem.name" >
            <audio v-on="$listeners" controls :src="mediaItem.fileUrl">
          Your browser does not support the <code>audio</code> element.
            </audio>
        </div>
        <div v-else-if="contentType === 'document'" class="singleNFTGalleryItem">
            <embed :class="classes" class="singleNFTGalleryItem" v-on="$listeners" :src="mediaItem.fileUrl" @error="setAltImg()" :alt="mediaItem.name">
        </div>
        <div v-else-if="contentType === 'image'" class="singleNFTGalleryItem">
            <img :class="classes" class="singleNFTGalleryItem" v-on="$listeners" :src="mediaItem.fileUrl" @error="setAltImg()" :alt="mediaItem.name">
        </div>
        <div v-if="options.showMeta" class="py-0" style="font-size: 1.2rem;">
            <!--
            <div class="p-2 d-flex justify-content-start">
          <div class="mr-3 text-small">NFT File:</div>
          <div>{{mediaItem.name}}</div>
            </div>
            <div class="p-2 d-flex justify-content-between">
          <div>{{contentType}}  ({{getNFTSizeMeg()}})</div>
          <div><a v-b-tooltip.hover="{ variant: 'light' }" :title="'The NFT file requires a cover image to display in the Risidio marketplace'" href="#" class="text-small text-primary"><b-icon icon="question-circle"/></a></div>
            </div>
            <div class="p-2 d-flex justify-content-start" v-if="mediaItem">
          <div class="text-small">Cover File:</div>
          <div>{{mediaItem.name}}</div>
            </div>
            -->
            <!-- <div class="p-0 d-flex justify-content-between text-bold" v-if="mediaItem"> -->
          <!-- <div>{{mediaItem.type || 'image'}}  ({{getCoverImageSizeMeg()}})</div> -->
          <!-- <div v-if="deleteAllowed()"><a v-b-tooltip.hover="{ variant: 'light' }" :title="'Replace this image?'" href="#" @click.prevent="$emit('deleteMediaItem', mediaItem.id)" class="text-small">change</a>
          </div> -->
        </div>
      </div>

    </div>
  </div>

</template>

<script>
import { APP_CONSTANTS } from '@/app-constants'
import VideoJsPlayer from './VideoJsPlayer'
import * as Three from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

export default {
  name: 'MediaItemGeneral',
  components: {
    VideoJsPlayer
  },
  props: ['classes', 'options', 'mediaItem'],
  data () {
    return {
      mediaObjects: [],
      waitingImage: 'https://images.prismic.io/radsoc/f60d92d0-f733-46e2-9cb7-c59e33a15fc1_download.jpeg?auto=compress,format',
      missing: '/img/logo.png',
      contentType: null
    }
  },
  mounted () {
    if (this.mediaItem) {
      const aft = this.mediaItem.type
      if (!aft) {
        this.contentType = 'image'
      } else {
        if (aft.indexOf('pdf') > -1 || aft.indexOf('plain') > -1) {
          this.contentType = 'document'
        } else if (aft.indexOf('video') > -1 || aft.indexOf('mp3') > -1) {
          this.contentType = 'video'
        } else if (aft.indexOf('audio') > -1 || aft.indexOf('mp3') > -1) {
          this.contentType = 'audio'
        } else if (aft.indexOf('threed') > -1 || aft.indexOf('gltf') > -1 || aft.indexOf('glb') > -1) {
          this.contentType = 'threed'
        } else {
          this.contentType = 'image'
        }
      }
    }
  },
  computed: {
    contractAsset () {
      const contractAsset = this.$store.getters[APP_CONSTANTS.KEY_ASSET_FROM_CONTRACT_BY_HASH](this.options.assetHash)
      return contractAsset
    }
  },
  methods: {
    setAltImg: function (event) {
      if (event) event.target.src = this.waitingImage
    },
    deleteAllowed: function () {
      return true
    },
    getNFTSizeMeg () {
      if (!this.mediaItem) return 0
      const ksize = this.mediaItem.size / 1000000
      return Math.round(ksize * 100) / 100 + ' Mb'
    },
    getCoverImageSizeMeg () {
      if (!this.mediaItem) return 0
      const ksize = this.mediaItem.size / 1000000
      return Math.round(ksize * 100) / 100 + ' Mb'
    }
  }
}
</script>
<style scoped>

/* .singleNftGalleryContainer{
  background: rgb(232, 232, 232);
  padding-top: 3rem;
  z-index: 1;
  border-radius: 30px;
  height: 32rem;
  width: 25rem;
} */
  .singleNftGalleryHolder{
    display: flex;
    margin: auto;
    justify-items: center;
    align-items: center;
    background:white;
    /* padding: 2px; */
    min-height: 23rem;
    min-width: 23rem;
    z-index: 2;
    border-radius: 5px;
    /* border: 1px solid rgba(208, 208, 208, 0.646); */
    /* box-shadow: rgba(22, 22, 133, 0.119) 0px 7px 10px 0px; */
  }
  .singleNFTGalleryItem{
    border-radius: 5px;
    width: 100%;
  }

</style>
