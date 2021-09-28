<template>
<div>
  <section id="about" class="about">
    <AboutSection0 />
    <AboutSection1 />
    <AboutSection2 />
  </section>
</div>
</template>

<script>
// import PrismicItems from '@/components/prismic/PrismicItems'
import AboutSection0 from '@/components/about/AboutSection0'
import AboutSection1 from '@/components/about/AboutSection1'
import AboutSection2 from '@/components/about/AboutSection2'
export default {
  name: 'about',
  components: {
    // PrismicItems,
    AboutSection0,
    AboutSection1,
    AboutSection2

  },
  props: ['about'],
  data () {
    return {
      response: null
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
<style scoped >
#about{
 min-width: 300px;
 min-height: 300px;
 height: auto;
}
.h1{
text-align: left;
}
#b-button{
  color: aqua;
}
</style>
