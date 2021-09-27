<template>
<div class = "Htitle">
  <div class = "title-container">
      <h1>What is Ruma ?</h1>
  </div>
  <div class = "main-container" >
      <p> Quick explanation on what and why mint an item :D <br><br>
      adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut Lorem ipsum dolor sit amet,
      consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut <br><br>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut<br><br>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim</p>
  </div>
</div>
</template>

<script>
export default {
  name: 'WRuma',
  components: {

  },
  props: ['content'],
  data () {
    return {
      banner: 'https://images.prismic.io/digirad/6e5bb3a5-21b7-4bcb-b5a7-85128b6e6e8a_Rumba_bg_small.png?auto=compress,format'
    }
  },
  computed: {
  }
}
</script>

<style scoped>
.title-container {
  min-width: 50%;
  padding: max(150px) min(12%) 0 min(12%);
  align-items: center;
  text-align: center;
  min-height: 40vh;
}
.title-container h1{
  font-family: inherit;
  font-size: clamp(50px, 3vw, 6vw);
  font-weight: 600;
}
.main-container{
  text-align: justify;
  margin-top: -80px;
  display: grid;
  grid-template-columns: repeat( 2, 1fr);
}
.main-container p{
  grid-column: 2;
  padding: 0 15% 0 0;
  font-size: .8em;
  font-weight:bolder;
}
</style>
