<template>
<div>
  <!-- <button v-on:click="printFile(), three()"> click me</button> -->
  <div v-if="contentType === 'threed'" :style="videoOptions.dimensions" id="video-demo-container">
    {{three()}}
    <canvas v-on="$listeners" @error="setAltImg()" :alt="attributes.artworkFile.name" :style="dimensions()" />
  </div>
  <div v-else-if="contentType === 'video'" :style="videoOptions.dimensions" id="video-demo-container">
    <VideoJsPlayer v-on="$listeners" :style="videoOptions.dimensions" @error="setAltImg()" :options="videoOptions"/>
  </div>
  <div v-else-if="contentType === 'audio'" id="audio-demo-container">
    <img v-on="$listeners" :src="attributes.coverImage.fileUrl" @error="setAltImg()" :alt="attributes.artworkFile.name" :style="dimensions()">
    <audio v-on="$listeners" controls :src="attributes.artworkFile.fileUrl" :style="dimensions()">
      Your browser does not support the <code>audio</code> element.
    </audio>
  </div>
  <div v-else-if="contentType === 'document'">
    <img v-on="$listeners" :src="attributes.coverImage.fileUrl" @error="setAltImg()" :alt="attributes.artworkFile.name" :style="dimensions()">
  </div>
  <div v-else-if="contentType === 'image'">
    <img v-on="$listeners" :src="attributes.coverImage.fileUrl" @error="setAltImg()" :alt="attributes.artworkFile.name" :style="dimensions()">
  </div>
  <div v-if="videoOptions.showMeta" :style="videoOptions.dimensions" style="font-size: 1.2rem;">
    <div class="p-2 d-flex justify-content-start">
      <div class="mr-3 text-small">NFT File:</div>
      <div>{{attributes.artworkFile.name}}</div>
    </div>
    <div class="p-2 d-flex justify-content-between">
      <div>{{contentType}}  ({{getNFTSizeMeg()}})</div>
      <div><a v-b-tooltip.hover="{ variant: 'light' }" :title="'The NFT file requires a cover image to display in the Risidio marketplace'" href="#" class="text-small text-primary"><b-icon icon="question-circle"/></a></div>
    </div>
    <div class="p-2 d-flex justify-content-start" v-if="attributes.coverImage">
      <div class="text-small">Cover File:</div>
      <div>{{attributes.coverImage.name}}</div>
    </div>
    <div class="p-2 d-flex justify-content-between" v-if="attributes.coverImage">
      <div>{{attributes.coverImage.type || 'image'}}  ({{getCoverImageSizeMeg()}})</div>
      <div v-if="deleteAllowed()"><a v-b-tooltip.hover="{ variant: 'light' }" :title="'Replace the cover image?'" href="#" @click.prevent="deleteCoverImage()" class="text-small text-danger"><b-icon icon="trash"/></a></div>
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
  name: 'MediaItem',
  components: {
    VideoJsPlayer
  },
  props: ['videoOptions', 'targetItem', 'attributes', 'dims'],
  data () {
    return {
      mediaObjects: [],
      waitingImage: 'https://images.prismic.io/radsoc/f60d92d0-f733-46e2-9cb7-c59e33a15fc1_download.jpeg?auto=compress,format',
      missing: '/img/logo.png',
      contentType: null
    }
  },
  mounted () {
    if (this.attributes && this.attributes.artworkFile) {
      const aft = this.attributes.artworkFile.type
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
      const contractAsset = this.$store.getters[APP_CONSTANTS.KEY_ASSET_FROM_CONTRACT_BY_HASH](this.videoOptions.assetHash)
      return contractAsset
    }
  },
  methods: {
    setAltImg: function (event) {
      event.target.src = this.waitingImage
    },
    deleteAllowed: function () {
      return true
    },
    dimensions: function () {
      if (this.dims) {
        // return 'width: ' + this.dims.width + 'px; height: ' + this.dims.height + 'px;'
        return 'width: 100%; max-height: 300px; min-height: 50px;'
      }
      return 'width: 100%; height: auto'
    },
    getNFTSizeMeg () {
      if (!this.attributes.artworkFile) return 0
      const ksize = this.attributes.artworkFile.size / 1000000
      return Math.round(ksize * 100) / 100 + ' Mb'
    },
    getCoverImageSizeMeg () {
      if (!this.attributes.coverImage) return 0
      const ksize = this.attributes.coverImage.size / 1000000
      return Math.round(ksize * 100) / 100 + ' Mb'
    },
    deleteCoverImage: function () {
      this.$emit('deleteMediaItem', this.attributes.coverImage.id)
    },
    printFile () {
      console.log(this.attributes.artworkFile)
    },
    threeObjectHandler () {
      // const selectedFile = document.getElementById('input').files[0]
      // console.log(selectedFile)
      // const binaryData = []
      // binaryData.push(this.attributes.artworkFile)
      // const url = URL.createObjectURL(new Blob(binaryData, { type: '' }))
      const url = this.attributes.artworkFile.fileUrl
      console.log(url)
      return {
        url
      }
    },
    threeLights () {
      const pointLight = new Three.PointLight(0xffffff)
      const ambientLight = new Three.AmbientLight(0xffffff)
      pointLight.position.set(20, 20, 20)

      return {
        pointLight,
        ambientLight
      }
    },
    threeControls (camera, renderer) {
      const controls = new OrbitControls(camera, renderer.domElement)
    },
    three () {
      const scene = new Three.Scene()
      const sizes = {
        width: window.innerWidth * 0.25,
        height: window.innerHeight * 0.4
      }
      const camera = new Three.PerspectiveCamera(75, sizes.width / sizes.height, 0.10, 1000)
      const loader = new GLTFLoader()
      const material = new Three.MeshStandardMaterial({ color: 0xFF6347, wireframe: true })
      const renderer = new Three.WebGLRenderer({ canvas: document.getElementsByTagName('canvas')[0] })

      const controls = this.threeControls(camera, renderer)
      const lights = this.threeLights()
      const url = this.threeObjectHandler()

      scene.background = new Three.Color(0xf2f2f2)
      renderer.setSize(sizes.width, sizes.height)

      // camera.position.setX(100)
      let box = new Three.Box3()

      scene.add(lights.pointLight, lights.ambientLight)
      let obj = ''
      loader.load(url.url,
        function (gltf) {
          obj = gltf.scene
          obj.material = material
          obj.scale.set(0.35, 0.35, 0.35)
          scene.add(obj)
          console.log('model loaded')
          console.log(obj)
          box = box.setFromObject(obj)
        },
        function (xhr) { // called while loading is progressing
          console.log((xhr.loaded / xhr.total * 100) + '% loaded')
        },
        function (error) {
          console.log('An error happened' + error)
        }
      )
      // These need to be run AFTER the the object has rendered

      // camera.position.setY(0)
      // camera.position.setX(0)
      camera.position.set(5, 0, 0)

      window.addEventListener('resize', () => {
        // Update sizes
        sizes.width = window.innerWidth * 0.25
        sizes.height = window.innerHeight * 0.4

        // Update camera
        camera.aspect = sizes.width / sizes.height
        camera.updateProjectionMatrix()

        // Update renderer
        renderer.setSize(sizes.width, sizes.height)
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
      })

      function animate () {
        // obj.rotation.y += 0.02
      }
      function render () {
        renderer.render(scene, camera)
      }
      function start () {
        requestAnimationFrame(start)
        animate()
        render()
        // console.log('start has run once')
        // camera.position.setY(box.min.y)
        // camera.position.setX(box.min.x)
        // camera.position.setZ(box.min.z)
      }
      function fullStart () {
        render()
        start()
        console.log('fullstart has run once ')
      }
      setTimeout(() => {
        camera.position.setY(box.min.y)
        camera.position.setX(box.min.x)
        camera.position.setZ(box.min.z)
        console.log('camera adjusted')
      }, 8000)
      fullStart()
    }
  }
}
</script>
<style scoped>

</style>
