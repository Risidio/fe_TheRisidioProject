<template>
<div class = "web3">
<div class= "container">
    <!-- <img src="https://images.prismic.io/radsoc/5b53f28e-8589-4e38-815d-d669ea0ac9e0_0642604bc88897185fc0eb0801195add.png?auto=compress,format" rounded="circle" alt="Circle image"> -->
    <img :src="web" alt="web" class="web"/>
  <h1> Web 3.0 </h1>
  <p> Risidio is a UK start-up created in the midst of the global pandemic. The
    pandemic pushed for accelerated digitisation and an ultra-connected world.
    We deem it necessary to lay out the bui ding blocks to thrive in this
    environment. We aim to build a better and safer Internet by using new
    technologies and partaking in the evolution of the decentralised web, Web
    3.0. <br><br>
    We provide you with the right technology to elevate your work. On these
    grounds, our integration of sustainable and environmental measures into the
    market follows a holistic approach.<br><br>
    We aim to empower and accompany companies harshly affected by the
    pandemic, like in the creative industries, in order to provide them with fairer
    opportunities and a chance to become independent.

</p>
</div>
</div>
</template>

<script>

export default {
  name: 'AboutSection3',
  components: {
  },
  props: ['content'],
  data () {
    return {
      web: require('@/assets/img/web.svg')
    }
  },
  computed: {
  }
}
</script>

<style scoped>
.web3{
  margin-top: 150px;
  background-color: white;
  font-family: inherit;
  min-height: 90vh;
}
.container{
  text-align: center;
  max-width: 750px;
}
.center_image{
  margin:auto;
  align-content: center;
  align-items: center;
  align-self: center;
  justify-content: center;
  justify-self: center;
}
h1{
  padding-top: 50px;
  font-size: clamp(4rem, 5rem, 6rem);
  font-weight: 300;
}
p{
  padding-top: 50px;
  font-size: 1.2em;
  font-weight: 500;
}

</style>
