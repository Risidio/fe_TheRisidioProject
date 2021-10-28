<template>
<div class = "web3">
<div class= "container">
    <img src="https://res.cloudinary.com/risidio/image/upload/v1633002004/RisidioMarketplace/web_uywlea.svg" rounded="circle" alt="Circle image">
  <h1> {{content.aboutsection3[0].abouttitle[0].text}}</h1>
  <p> {{content.aboutsection3[0].abouttext[0].text}} <br><br>
      {{content.aboutsection3[0].abouttext1[0].text}}<br><br>
      {{content.aboutsection3[0].abouttext2[0].text}}

</p>
</div>
</div>
</template>

<script>

export default {
  name: 'AboutSection3',
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
.web3{
  margin-top: 150px;
  background-color: white;
  font-family: inherit;
  min-height: 90vh;
}
.container{
  text-align: center;
  max-width: 750px;
}
h1{
  padding-top: 50px;
  font-size: clamp(4rem, 5rem, 6rem);
  font-weight: 300;
}
p{
  padding-top: 50px;
  font-weight: 500;
  max-width: 1000px;
  margin: 0 auto;
}

</style>
