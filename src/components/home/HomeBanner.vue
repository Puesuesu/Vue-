<template>
  <div class ="home-banner" @click="onClick">
    <div 
      class="bg-img"
      :style="{backgroundImage: bdImage}" 
    >
      <div class="title">{{title}}</div>
      <div class="sub-title">{{subTitle}}</div>
    </div>
  </div>
</template>

<script>
  export default {
    props:{
      title:String,
      subTitle:String,
      img:String
    },
    computed:{
      bdImage(){
        return 'url('+ this.img +')'
      }

    },
    methods:{
      onClick(){
        this.$emit('onClick')
      }
    }

}
</script>

<style lang="scss" scoped>
  .home-banner{
    padding:20px 20px 0 20px;
    .bg-img{
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height:125px;
      border-radius: 10px;
      background-repeat: no-repeat;
      background-size: 100% 100%;
      .title{
        font-size: 18px;
        font-weight: 500;
        color:#fff;
      }
      .sub-title{
        padding:5px 15px;
        background: #4585ff;
        border:1px solid #ccc;
        border-radius: 50px;
        margin-top:10px;
        font-size: 14px;
        color:#fff;
      }
    }
  }

</style>
