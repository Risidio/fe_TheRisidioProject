<template>
    <div>
        <input ref="inputField" type="file" id="input" multiple>
        <img id="output"/>
        <button v-on:click="three()"> Clickidy click click</button>
        <div class = "threeD">
        <canvas /></div>
    </div>
</template>

<script>
import * as Three from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

export default {
  data () {
    return {
      image: null
    }
  },
  methods: {
    threeObjectHandler () {
      const selectedFile = document.getElementById('input').files[0]
      console.log(selectedFile)
      const url = URL.createObjectURL(selectedFile)
      console.log(url)

      return {
        url
      }
    },
    printFile () {
      console.log(this.mediaItem)
    },
    // threeObjectHandler () {
    //   const url = this.mediaItem.fileUrl
    //   console.log(url)
    //   return {
    //     url
    //   }
    // },
    threeLights () {
      const ambientLight = new Three.AmbientLight(0xffffff)

      return {
        ambientLight
      }
    },
    threeControls (camera, renderer) {
      const controls = new OrbitControls(camera, renderer.domElement)
    },
    three () {
      const scene = new Three.Scene()
      const sizes = {
        width: window.innerWidth * 0.15,
        height: window.innerHeight * 0.2
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
      scene.add(lights.ambientLight)
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
        sizes.width = window.innerWidth * 0.15
        sizes.height = window.innerHeight * 0.2

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
      // controls.addEventListener('change', render)
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
