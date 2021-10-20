<template>
<div class="search_bar_container">
  <div class="dropdown_container">
    <div class="dropdown_selector" @click="isHidden = !isHidden"> Search by ▸
      <div class="dropdown_option_container">
    <div class="dropdown_option" v-show="isHidden" value="All">All</div>
    <div class="dropdown_option" v-show="isHidden" value="Category">Category</div>
    <div class="dropdown_option" v-show="isHidden" value="Category">Category</div>
    <div class="dropdown_option" v-show="isHidden" value="Category">Category</div>
    </div>
    </div>
  </div>
  <div>
  <input class="search_input_box" type="text" placeholder=" Looking for something in particular?  "> <span class="searchIcon">&#8981;</span>
  </div>
</div>
</template>

<script>

export default {
  name: 'SearchBar',
  components: {
  },
  props: ['showPrepend'],
  data () {
    return {
      query: null,
      isHidden: false
    }
  },
  methods: {
    doSearch () {
      this.$emit('set-filter', { query: this.query, filter: 'text-search' })
    }
  },
  computed: {
  }
}
</script>
<style lang="scss" scoped>
.search_bar_container{
  display: flex;
  align-content: center;
  justify-content: center;
  z-index: 10;
}

.searchIcon{
  position: absolute;
  font-size: 6rem;
  margin-left: -5rem;
  margin-top: -2rem;
  color: #50B1B5;
  transform: rotate(270deg)
}

.dropdown_selector{
  border-top-left-radius: 40px;
  border-bottom-left-radius: 40px;
  border: none;
  height: 50px;
  width: 155px;
  padding: 10px;
  outline: none;
  background: white;
  transition: 0.3s;
  text-align: center;
  box-shadow: rgba(0, 0, 0, 0.35) -2px 2px 10px;
  z-index: 10;
}

// .dropdown_selector:hover{
//   background: rgb(255, 212, 22);
// transition: 0.3s;
//   z-index: 10;
// }

.search_input_box{
  border-top-right-radius: 40px;
  border-bottom-right-radius: 40px;
  border: none;
  height: 50px;
  width: 50vw;
  outline: none;
  padding: 10px;
  background: white;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 2px 10px, rgba(0, 0, 0, 0.06) 0px 2px 4px 0px inset;
  z-index: 1;
  }

  ::-webkit-input-placeholder {
   text-align: center;
}

:-moz-placeholder { /* Firefox 18- */
   text-align: center;
}

::-moz-placeholder {  /* Firefox 19+ */
   text-align: center;
}

:-ms-input-placeholder {
   text-align: center;
}

.dropdown_container{
  display: flex;
  flex-direction: column;
}
.dropdown_option_container{
  background: none;
  display: flex;
  flex-direction: column;
  align-content: flex-start;
  justify-content: flex-start;
  margin-top: 25px;
  width: 150px;
  margin-left: -5px;
  z-index: 10;
  background: white;
}
.dropdown_option{
    display: none;
  padding: 10px;
  border: 1px solid rgb(255, 212, 22);;
  background: white;
  border-radius: 0px;
  margin: 2px;
  transition: 0.3s;
  z-index: 10;
}

.dropdown_option:hover{
  background: rgb(255, 212, 22);
  border: 1px solid rgb(255, 212, 22);;
  transition: 0.3s;
  z-index: 10;
}

</style>
