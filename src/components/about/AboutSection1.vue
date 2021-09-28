<template>
<!-- <section id="about-number-1" v-if="content">
  <b-container class="text-center">
    <b-row style="height: 92vh" align-h="center">
      <b-col class="text-white mx-md-5 mx-sm-3" md="6" sm="6" align-self="center">
        <img class="mb-5" width="300px;" :src="rainbowOne" alt="site logo" />
        <prismic-items :prismicItems="content.section1rtf1"></prismic-items>
      </b-col>
    </b-row>
  </b-container>
</section> -->
    <b-container style="min-height: 60vh" class="pb-4 text-center">
      <b-row align-h="center" style="min-height: 70vh">
        <b-col md="6" sm="10" align-self="center">
          <div class="text-center text">
                <b-img src="https://images.prismic.io/radsoc/26f1e22e-5565-4f32-8761-e1b69a3d8e50_risisdilogo.png?auto=compress,format"></b-img>
          <!-- <h1><prismic-items :prismicItems="content.title1"></prismic-items></h1> -->
                    <prismic-items :prismicItems="content.text1"></prismic-items>
          </div>
        </b-col>
      </b-row>
    </b-container>
</template>

<script>
import PrismicItems from '@/components/prismic/PrismicItems'
export default {
  name: 'AboutSection1',
  components: {
    PrismicItems
  },
  props: ['about'],
  data () {
    return {
      rainbowOne: 'https://images.prismic.io/dbid/a70b331a-ee92-4d98-81e4-659f2edadba1_Number1_build3.gif?auto=compress,format'
    }
  },
  computed: {
    content () {
      const content = this.$store.getters['contentStore/getAbout']
      return content
    }
  }
}
</script>

<style scoped>
</style>
