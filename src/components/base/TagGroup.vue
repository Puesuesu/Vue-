<template>
  <div class="tag-group-wrapper">
    <div class="tag-group-header">
      <div class="header-text">{{headerText}}</div>
      <div class="header-btn" @click="onBtnClick">{{btnText}}</div>
  </div> 
    <div class="tag-group">
      <!-- 遍历value里面的值 -->
      <div 
      class="tag-group-inner"
      v-for="(text,index) in value"
      :key="index"
      >
          
        <Tag :text="text" @onClick="onTagClick(text,index)"/>
      </div>
    </div>    
  </div>
</template>

<script>
  import Tag from './Tag'
  export default {
    components:{Tag},
    props:{
      headerText:String,
      btnText:String,
      value:Array
    },
    methods:{
      onTagClick(text,index){
        this.$emit('onTagClick',text,index)
      },
      onBtnClick(){
        this.$emit('onBtnClick')
      }
    }
};
</script>

<style lang="scss" scoped>
  .tag-group-wrapper{
    margin-top:35px;
    width:100%;
    padding-bottom: 10px;
    .tag-group-header{
      /* 水平布局 */
      display: flex;
      /* 两边撑开 */
      justify-content: space-between;
      padding:0 16.5px;
      font-size: 14px;
      line-height: 20px;
      .header-text{
        color: #333333;
      }
      .header-btn{
        color:#3696EF;
      }
    }
    .tag-group{
      width:100%;
      box-sizing: border-box;
      display: flex;
     /*  可以换行 */
      flex-flow: row wrap;
     /*  上 右 下 左 */
      padding:4px 10px 0 10px;
      .tag-group-inner{
        max-width:100%;
        box-sizing: border-box;
        padding:12px 6px 0 6px;
      }
    }
  }
</style>
