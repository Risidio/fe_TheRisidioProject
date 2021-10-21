<template>
<div class = "Htitle">
  <div class = "title-container">
      <h1>What Is Risidio Marketplace ?</h1>
      <p>Technology is progressing so art is evolving. </p>
  </div>
  <div class = "main-container">
    <div class = "container">
      <p> (What is Risidio Collectible About)<br>
      adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut Lorem ipsum dolor sit amet,
      consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut <br>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aut<br>
      Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim</p>
      <button class="button"> About Risidio </button>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'WRuma',
  components: {

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
  padding-top: 12%;
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

</style>
