<template>
<div class = "Htitle">
  <div class = "container">
      <h1>About Risidio</h1>
      <p>Take back control of your data</p>
      <button class="button" > <a href="https://risidio.com/">Visit Our Website</a></button>
  </div>
</div>
</template>

<script>
export default {
  name: 'AboutSection0',
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
  width:100%;
}
.container {
  min-width: 50%;
  padding: 50px min(15%);
  align-items: center;
  text-align: center;
  color:white;
  margin-top: -35px;
  min-height: 40vh;
}
.container h1{
  font-family: inherit;
  font-size: clamp(3rem, 4rem, 5rem);
  font-weight: 100;
}
.container p{
  font-size: 1em;
  font-weight: 300;
  padding-top: 20px;
}
.button{
  margin-top: 30px;
  width: 160px;
  height: 40px;
  border-radius: 100px;
  border: none;
  background-color: white;
  font-size: 11px;
  font-weight:700;
}
a, a:after, a:hover{
    color:black;
}
</style>
