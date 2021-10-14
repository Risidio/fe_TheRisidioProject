<template>
    <div>
        <input ref="inputField" type="file" id="input" multiple v-on:change="loadFile()">
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
    loadFile () {
      const selectedFile = document.getElementById('input').files[0]
      const output = document.getElementById('output')
      output.src = URL.createObjectURL(selectedFile)
      console.log(output.src)
      // console.log(dddd)
      // console.log(selectedFile)
    },
    three () {
      const selectedFile = document.getElementById('input').files[0]
      console.log(selectedFile)
      const url = URL.createObjectURL(selectedFile)
      console.log(url)
      const scene = new Three.Scene()
      const sizes = {
        width: window.innerWidth * 0.5,
        height: window.innerHeight * 0.5
      }
      const camera = new Three.PerspectiveCamera(75, sizes.width / sizes.height, 0.10, 1000)
      const loader = new GLTFLoader()
      const material = new Three.MeshStandardMaterial({ color: 0xFF6347, wireframe: false })
      const renderer = new Three.WebGLRenderer({ canvas: document.getElementsByTagName('canvas')[0] })
      const pointLight = new Three.PointLight(0xffffff)
      const ambientLight = new Three.AmbientLight(0xffffff)
      const controls = new OrbitControls(camera, renderer.domElement)

      scene.background = new Three.Color(0x8fbcd4)
      renderer.setSize(sizes.width, sizes.height)

      // camera.position.setX(100)
      let box = new Three.Box3()
      pointLight.position.set(20, 20, 20)
      scene.add(pointLight, ambientLight)
      // 'http://localhost:8086/Lemon_Sorbet.gltf'
      let obj = ''
      loader.load(url,
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
      const height = box.min.x
      console.log('x =  ' + height)

      camera.position.setY(1.4)
      camera.position.setX(-227)
      camera.position.setZ(882)

      window.addEventListener('resize', () => {
        // Update sizes
        sizes.width = window.innerWidth * 0.5
        sizes.height = window.innerHeight * 0.5

        // Update camera
        camera.aspect = sizes.width / sizes.height
        camera.updateProjectionMatrix()

        // Update renderer
        renderer.setSize(sizes.width, sizes.height)
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
      })
      function animate () {
        requestAnimationFrame(animate)
        obj.rotation.y += 0.02
        renderer.render(scene, camera)
      }
      animate()
    }
  }
}
</script>

<style scoped>

</style>
