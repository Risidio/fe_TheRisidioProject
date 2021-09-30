<template>
      <b-container style="min-height: 90vh" class=" text-center" align-self="center" id="getStacksWallet">
        <h1> Get your stacks wallet</h1>
        <b-row align-h="center" style="min-height: 30vh">
          <b-col align-self="center">
            <img src="https://res.cloudinary.com/risidio/image/upload/v1633002019/RisidioMarketplace/digitalcurrency_tqf1es.svg" alt="digitalcurrency" class="digitalcurrency">
          </b-col>
          <b-col class = "mt-5" align-self="center" >
            <!-- <prismic-items :prismicItems="content.title3"></prismic-items>
            <prismic-items :prismicItems="content.text3"></prismic-items> -->
            <p>Quick explanation on what and why mint an item :D<br><br>
              adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna
              aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris <br><br>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
              tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
              veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
              commodo consequat. Duis aut <br><br>
            </p>
            <button><router-link to="/">Download Your Wallet</router-link></button>
          </b-col>
        </b-row>
      </b-container>
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
.digitalcurrency{
  height: 300px;
  width: 300px;
}
button{
  width: 220px;
  height: 55px;
  border-radius: 100px;
  border: none;
  color:black;
  background-color: #d8d8d8;
  font-size: 12px;
  font-weight:700;
  align-content: left;
}
button:active, button:hover, button:visited{
  color:black;
}
p{
  font-size: 1.2em;
  text-align: justify;
}
h1{
  padding: 200px 0 100px 0;
  font-size: clamp(50px, 3vw, 6vw);
  font-weight: 200;
}
</style>
