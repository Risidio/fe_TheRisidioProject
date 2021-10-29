<template>
    <div class="section2">
      <div style="font-size:2rem; font-weight:800; text-align:center;">Featured Collections</div>
      <div style="font-size:4rem; font-weight:400; text-align:center; margin:20px">New Collections Coming Soon!</div>
      <div style="display:flex; margin:auto; width:50%; text-align:center;">Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua</div>
      <div class="exampleCollection">
          <div class="singleNftGalleryContainer">
            <img src="https://res.cloudinary.com/risidio/image/upload/v1634828295/RisidioMarketplace/Screenshot_2021-10-21_at_15.57.57_q7chjf.png" alt="" class="">
            <div class="fakeDetails">
              <div>
              <span style="width:20%; font-size:3rem">????</span>
              <span style="width:20%; margin-left:50%">10STX</span>
              </div>
              <span class="fakeDetails2" style="width:20%; margin-left:80%; margin-top:-10%">~$10</span>
            </div>
          </div>
          <div class="singleNftGalleryContainer">
            <img src="https://res.cloudinary.com/risidio/image/upload/v1634828295/RisidioMarketplace/Screenshot_2021-10-21_at_15.57.57_q7chjf.png" alt="" class="">
         <div class="fakeDetails">
              <div>
              <span style="width:20%; font-size:3rem">????</span>
              <span style="width:20%; margin-left:50%">10STX</span>
              </div>
              <span class="fakeDetails2" style="width:20%; margin-left:80%; margin-top:-10%">~$10</span>
            </div>
          </div>
          <div class="singleNftGalleryContainer">
            <img src="https://res.cloudinary.com/risidio/image/upload/v1634828295/RisidioMarketplace/Screenshot_2021-10-21_at_15.57.57_q7chjf.png" alt="" class="">
          <div class="fakeDetails">
              <div>
              <span style="width:20%; font-size:3rem">????</span>
              <span style="width:20%; margin-left:50%">10STX</span>
              </div>
              <span class="fakeDetails2" style="width:20%; margin-left:80%; margin-top:-10%">~$10</span>
            </div>
          </div>
      </div>
        <!-- <div>Find Out More!</div> -->
    </div>
</template>

<script>
import PrismicItems from '@/components/prismic/PrismicItems'

export default {
  name: 'CollectionsInfo',
  components: {
    // PrismicItems
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
