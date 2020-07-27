<template>
  <div class="image-view" @click="onClick" :style="{height}">
      <img
        :class="round?'round image':'image'"
        :style="{height}"
        :src="src"
        :mode="mode"
        :lazy-load="lazyLoad"
        @load="onLoad"
        @error="onError"
        v-show="!isLoading||!error"
      />
      <!-- 占位图有待解决 -->
      <img
        :class="round?'round image':'image'"
        style="height:0px"
        :src="Img"
        :mode="mode"
        :lazy-load="lazyLoad"
        v-show="isLoading||error"
      />
  </div>
</template>

<script>
export default{
    props:{
        src:{
            type:String,
            default:''
        },
        mode:{
            type:String,
            default:'widthFix'
        },
        lazyLoad:{
            type:Boolean,
            default:true
        },
        round:{
            type:Boolean,
            default:false

        },
        height:{
            type:String,
            default:'auto'
        }
    },
    watch:{
      src(newValue,preValue){
          if(newValue&&newValue.length > 0&& newValue !== preValue){
              this.$nextTick(() =>{
              this.isLoading = true
              this.error = false
              })
          }
      }
    },
    data(){
        return{
            isLoading:true,
            error:false,
            Img:require("../base/imgs/zwf.jpeg"),
            
        }
    },
    methods:{
      onClick(){
          this.$emit('onClick')
      },
      onLoad(){
          this.isLoading = false
          this.error = false
          console.log('onLoad')     
      },
      onError(){
          this.isLoading = false
          this.error = true
          console.log('onErr') 
      }
    }

}

</script>

<style lang="scss" scoped>
  .image-view{
      width:100%;
      .image{
          width:100%;
          &.round{
              border-radius: 50%;
          }
      }
  }

</style>
