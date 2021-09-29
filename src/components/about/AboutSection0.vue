<template>
<div class = "Htitle">
  <div class = "container" :style="bannerImage">
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
      banner: 'https://images.prismic.io/digirad/1f147c92-5945-4d4e-a578-aa085e6603ef_Ruma_bg.png?auto=compress,format'
    }
  },
  computed: {
    bannerImage () {
      return {
        height: 'auto',
        width: '100%',
        'background-repeat': 'no-repeat',
        'background-image': `url(${this.banner})`,
        'background-position': 'center center',
        '-webkit-background-size': 'cover',
        '-moz-background-size': 'cover',
        '-o-background-size': 'cover'
        // 'background-size': 'cover',
        // '-webkit-transform': 'rotate(180deg)',
        // '-moz-transform': 'rotate(180deg)',
        // '-ms-transform': 'rotate(180deg)',
        // '-o-transform': 'rotate(180deg)'
      }
    }
  }
}
</script>

<style scoped>
.Htitle{
  background:rgb(20, 20, 110);
  width:100%;
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
  font-size: clamp(4rem, 5rem, 6rem);
  font-weight: 300;
}
.container p{
  font-size: 1.5em;
  font-weight: 300;
  padding-top: 20px;
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
