<template>
<div class = "Htitle">
  <div class = "title-container">
      <h1>How to mint an item ?</h1>
  </div>
  <div class = "main-container" >
    <div class = "container">
      <img src="https://res.cloudinary.com/risidio/image/upload/v1633609692/RisidioMarketplace/008-chain_onkzfk.svg"></div>
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

  }
}
</script>

<style scoped>
.Htitle{
  min-height: 90vh;
}
.title-container {
  font-family: inherit;
  padding: max(150px) min(12%) 0 min(12%);
  align-items: center;
  text-align: center;
}
.title-container h1{
  font-family: inherit;
  font-size: clamp(50px, 3vw, 6vw);
  font-weight: 200;
  padding-bottom: 50px;
}
.main-container{
  /* text-align: justify; */
  display:flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.main-container > *{
  flex: 1 1 500px;
}
.main-container p{
  padding: 100px 10% 100px 50px;
  font-size: 1.2em;
  font-weight:500;
}
.container > *{
  display:flex;
  margin-left: auto;
  margin-right:auto;

}
img{
  min-height: 350px;
  min-width: 350px;
}
@media only screen and (max-width: 1000px)  {
  p{text-align: center;}
}
</style>
