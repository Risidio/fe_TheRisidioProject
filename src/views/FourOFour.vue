<template>
<div class="four_0_four_container">
<img class="four_0_four" src="https://res.cloudinary.com/risidio/image/upload/v1632579152/Risidio.com/four0four_zucqyl.svg" alt="">
<router-link class="text-white four_0_four_home_link" to="/">Back to home page</router-link>
</div>
</template>

<script>
export default {
  name: 'FourOFour',
  components: {
  },
  data () {
    return {
    }
  },
  methods: {
  },
  computed: {
  }
}
</script>
<style lang="scss">
.four_0_four{
  display: flex;
  margin: auto;
  width: 50vw;
  height: 50vh;
  // object-fit: contain;
}

.four_0_four_home_link{
  display: flex;
  background: orange;
  padding: 20px;
  border-radius: 50px;
  margin: auto;
  width: 200px;
}

.four_0_four_home_link:hover{
    background: rgb(235, 152, 0);
}

</style>
