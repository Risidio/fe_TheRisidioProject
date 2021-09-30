<template>
<div class = "Htitle">
  <div class = "container">
     <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1632564338/Risidio.com/main_bg.svg" alt="">
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
.banner{
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 50rem;
  object-fit: cover;
  z-index: -10;
  transform: rotate(180deg);
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
