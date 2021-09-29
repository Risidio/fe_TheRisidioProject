<template>
<div class = "Htitle">
  <div class = "container">
      <h1>How it works</h1>
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
        tempor incididunt ut labore et dolore
        magna aliqua. Ut enim ad minim veniam,
        quis nostrud exercitation ullamco laboris nisi ut
        aliquip ex ea commodo consequat. Duis aut</p>
      <button class="button"> Get Started </button>
  </div>
</div>
</template>

<script>
export default {
  name: 'HTitle',
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
.Htitle{
  background:rgb(20, 20, 110);
}
.container {
  min-width: 100%;
  padding: 50px;
  align-items: center;
  text-align: center;
  color:white;
  margin-top: -35px;
  min-height: 40vh;
}
.container h1{
  font-family: inherit;
  padding-top: 20px;
  font-size: clamp(4rem, 5rem, 6rem);
  font-weight: 300;

}
.container p{
  margin:auto;
  padding-top: 50px;
  font-size: 1.2em;
  font-weight: 500;
  max-width: 750px;
}
.button{
  margin-top: 30px;
  width: 220px;
  height: 55px;
  border-radius: 100px;
  border: none;
  background-color: white;
  font-size: 12px;
  font-weight:700;
}
a, a:after, a:hover{
    color:black;
}
</style>
