<template>
    <div>
        <input ref="inputField" type="file" id="input" multiple v-on:change="loadFile()">
        <img id="output"/>
        <button v-on:click="three()"> Clickidy click click</button>
        <canvas />
    </div>
</template>

<script>
import * as Three from 'three'
// import GLTFLoader from 'three-gltf-loader'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'

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
      const camera = new Three.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.10, 1000)
      const width = window.innerWidth
      const height = window.innerHeight

      const loader = new GLTFLoader()
      const material = new Three.MeshStandardMaterial({ color: 0xFF6347, wireframe: false })
      const renderer = new Three.WebGLRenderer({ canvas: document.getElementsByTagName('canvas')[0] })
      const pointLight = new Three.PointLight(0xffffff)
      const ambientLight = new Three.AmbientLight(0xffffff)

      renderer.setSize(width * 0.4, height * 0.4)
      camera.position.setZ(1500)
      camera.position.setY(100)
      camera.position.setX(100)

      pointLight.position.set(20, 20, 20)
      scene.add(pointLight, ambientLight)
      // 'http://localhost:8086/Lemon_Sorbet.gltf'
      loader.load(url,
        function (gltf) {
          const obj = gltf.scene
          obj.material = material
          obj.scale.set(0.35, 0.35, 0.35)
          scene.add(gltf.scene)
          console.log('model loaded')
          console.log(gltf)
        },
        function (xhr) { // called while loading is progressing
          console.log((xhr.loaded / xhr.total * 100) + '% loaded')
        },
        function (error) {
          console.log('An error happened' + error)
        }
      )
      function animate () {
        requestAnimationFrame(animate)
        renderer.render(scene, camera)
      }
      animate()
    }
  }
}
</script>
