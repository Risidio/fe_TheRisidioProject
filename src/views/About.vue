<template>
<div>
  <section id="about" class="about">
    <AboutSection0 :content="content"/>
    <AboutSection1 :content="content"/>
    <AboutSection2 :content="content"/>
    <AboutSection3 :content="content"/>
  </section>
</div>
</template>

<script>
// import PrismicItems from '@/components/prismic/PrismicItems'
import AboutSection0 from '@/components/about/AboutSection0'
import AboutSection1 from '@/components/about/AboutSection1'
import AboutSection2 from '@/components/about/AboutSection2'
import AboutSection3 from '@/components/about/AboutSection3'
export default {
  name: 'about',
  components: {
    // PrismicItems,
    AboutSection0,
    AboutSection1,
    AboutSection2,
    AboutSection3
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
