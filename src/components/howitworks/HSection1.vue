<template>
<div class = "Htitle">
  <div class = "container">
    <img class="banner" src="https://res.cloudinary.com/risidio/image/upload/v1633609373/RisidioMarketplace/Group_-304_ofssmk.svg" alt="">
      <h1>{{content.howsection0[0].howtitle[0].text}}</h1>
      <p>{{content.howsection0[0].howtext[0].text}} </p>
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
    }
  },
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
.HTitle{
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
  margin:auto;
  padding-top: 20px;
  font-size: 1.2em;
  font-weight: 300;
  max-width: 750px;
}
.button{
  margin-top: 50px;
  width: 220px;
  height: 55px;
  border-radius: 100px;
  border: none;
  background-color: white;
  font-size: 12px;
  font-weight:700;
  color: rgb(0, 177, 201);
}
a, a:after, a:hover{
    color:black;
}
</style>
