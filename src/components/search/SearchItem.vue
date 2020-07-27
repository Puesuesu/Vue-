<template>
  <div class="search-item-wrapper" @click="onClick">
    <div class="search-item-icon">
      <div class="icon-wrapper">
        <!-- 黑色0.5的透明度 -->
        <van-icon :name="icon" color="rgba(0,0,0,.5)" :style="{width:'20px',height:'20px'}">
        </van-icon>
      </div>
    </div>
    <div class="search-item-info">
      <div class="search-item-title">{{title}}</div>
      <div class="search-item-sub-title">{{subTitle}}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    subTitle: String,
    icon: String
  },
  methods: {
    onClick() {
      this.$emit("onClick");
    }
  }
};
</script>

<style lang="scss" scoped>
  .search-item-wrapper {
    display: flex;
    align-items: center;
    padding:15px;
    height: 36.5px;
    .search-item-icon {
      .icon-wrapper {
        display: flex;
        /* 水平居中 */
        justify-content: center;
     /*    垂直居中 */
        align-items: center;
        width:32px;
        height:32px;
        /* 圆形 */
        border-radius: 50%;
        background:#f7f7f9;
      }
    }
    .search-item-info {
      /* 距离左边10像素 */
      margin-left:10px;
      width:90%;   
      .search-item-title {
        width:100%;
        overflow: hidden;
        text-overflow: ellipsis;
        /* 规定段落中的文本不进行换行 */
        white-space: nowrap;
        color:#333;
        font-size:14px;
        line-height:20px;
      }
      .search-item-sub-title {
        color:rgba(0,0,0,.45);
        font-size:12px;
        line-height:16.5px;
      }
    }
}
</style>
