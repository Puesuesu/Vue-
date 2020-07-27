<template>
  <div>
    <div class="qingjia-wrapper" @click="onSearchBarClick">
      <img class="a" :src="imgUrl1" />
<!--       <img class="a" :src="imgUrl2" /> -->
    </div>
  </div>  
</template>
<script>
import ImageView from '../../components/base/ImageView'
export default {
  components : {ImageView},
  data(){
    return {
      imgUrl1:require("../qingjia1/1.png"),
/*       imgUrl2:require("../qingjia1/2.jpg") */
    }
  },
  methods:{
    onSearchBarClick() {
      console.log('qqq')
      this.$router.push('/pages/qingjia2/main')
    }
  }
}
</script>

<style lang="scss" scoped>
  .a{
    width:100%;
    height:100vh;
  }
</style>
