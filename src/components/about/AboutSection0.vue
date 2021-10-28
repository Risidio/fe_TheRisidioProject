<template>
<div class = "Htitle">
  <div class = "container">
     <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1633609373/RisidioMarketplace/Group_-304_ofssmk.svg" alt="">
      <h1>{{content.aboutsection0[0].abouttitle[0].text}}</h1>
      <p>{{content.aboutsection0[0].abouttext[0].text}}</p>
      <button class="button" > <a href="https://risidio.com/">Find Out More</a></button>
  </div>
</div>
</template>

<script>
export default {
  name: 'AboutSection0',
  components: {

  },
  props: ['content'],
  computed: {
  }
}
</script>

<style scoped>
.banner{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
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
  font-size: 1.2em;
  font-weight: 300;
  padding-top: 20px;
}
.button{
  margin-top: 30px;
  width: 250px;
  height: 55px;
  border-radius: 100px;
  border: none;
  background-color: rgba(255, 255, 255, 0.25);
  font-size: 12px;
  font-weight:800;
}
a, a:after, a:hover{
    color:var(--cyan);
}
</style>
