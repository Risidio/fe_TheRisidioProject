<template>
<div class = "Htitle">
  <div class = "title-container">
    <h1>{{content.howsection2[0].howtitle[0].text}}</h1>
  </div>
  <img src="https://res.cloudinary.com/risidio/image/upload/v1633609788/RisidioMarketplace/006-wallet_uy3myg.svg" alt="wallet" class="wallet">
  <div class = "main-container">
    <div class = "container">
      <p>
      {{content.howsection2[0].howtext[0].text}}</p>
      <button class="button"> Buy STX </button>
    </div>
  </div>
  <!-- <hr class= "hr"> -->
</div>
</template>

<script>
export default {
  name: 'Group4',
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
.Htitle{
  min-height: 90vh;
}
.title-container {
  font-family: inherit;
  align-items: center;
  text-align: center;
}
.title-container h1{
  font-family: inherit;
  font-size: clamp(50px, 3vw, 6vw);
  font-weight: 200;
  padding-top: 10%;
  padding-bottom: 25px;
}
.title-container p{
  font-family: inherit;
  font-size: clamp(25px, 1em, 3vw);
  font-weight: 500;
  padding-top: 25px;
  padding-bottom: 20px;
}
.main-container{
  display:flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  text-align: center;
  max-width: 1100px;
  margin: 0 auto;
}
.main-container p{
  font-size: 1.1em;
  font-weight:300;
  padding-top: 20px;
}
.button{
  margin-top: 50px;
  width: 200px;
  height: 60px;
  border-radius: 100px;
  border: none;
  background-color: rgba(0, 162, 184, 0.1);
  font-size: 14px;
  font-weight:700;
  color: rgb(0, 177, 201);
}
.wallet{
  margin: 0 auto 25px auto;
  display: block;
  width: 250px;
  height: 250px;
}
.hr{
  margin-top: 50px;
  color: grey;
  width: 60%;
  height: 3px;
}
</style>
