<template>
<div class="itemDisplay">
  <div className="itemDisplayContainer">
    <b-col class="col-12">
      <h1 class="h1-modal">Mint Your NFT</h1>
    </b-col>
    <div class="itemDisplayContainer">
      <embed style=" width: 100%; height:250px; box-shadow: 10px 10px 30px rgba(0, 0, 0, 0.18); border-radius: 5px;" :src="item.image"/>
      <div class="">
        <div class=""><h2 style="font-weight:300; text-align:left;">{{item.name}}</h2></div>
        <div><p style="font-weight:300; font-size: 16px; float: left;">By <span style="font-weight:600; font-size: 15px; color:black">{{item.artist}}</span></p> </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>

export default {
  name: 'ItemDisplay',
  components: {
  },
  props: ['item'],
  data () {
    return {
    }
  },
  methods: {
  },
  computed: {
  }
}
</script>
<style lang="scss" scoped>

.itemDisplayContainer{
  display: block;
  background: rgba(129, 129, 129, .12);
  border-radius: 26px;
  margin: 50px auto 0 auto;
  width: 300px;
  height: 400px;
  padding: 20px;
}
</style>
