<template>
  <div>
    <div class="qingjia-wrapper">
      <img class="a" :src="imgUrl"/>
    </div>
  </div>  
</template>
<script>
import ImageView from '../../components/base/ImageView'
export default {
  components : {ImageView},
  data(){
    return {
      imgUrl:require("../qingjia2/2.jpg")
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
