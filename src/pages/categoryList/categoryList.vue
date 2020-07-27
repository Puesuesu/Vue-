<template>
  <div>
    <HomeBook
      :row="category.length/2"
      :col="2"
      :data="category"
      mode="category"
      :show-btn="false"
      :show-title="false"
      @onBookClick="onCategoryClick"
    />
  </div>  
</template>
<script>
import HomeBook from '../../components/home/HomeBook'
import {categoryList} from '../../api/index'
export default {
    components:{
      HomeBook},
    data(){
      return {
          category:[]
      }
    },
    methods:{
      getcategoryList(){
        categoryList().then(response => {
            this.category = response.data.data
      })
    },
      onCategoryClick(category){
        this.$router.push({
        path:'/pages/list/main',
        query:{
          key:'categoryId',
          text:category.category,
          title:category.categoryText
        }
      })
    }
    },
    mounted(){
      this.getcategoryList()
    }

}
</script>

<style lang="scss" scoped>

</style>
