<template>
  <div>
    <SearchTable :data="data" />
  </div>  
</template>

<script>
import SearchTable from '../../components/search/SearchTable'
import {searchList} from '../../api/index'
import {setNavigationBarTitle, showToast} from '../../api/wechat'
export default {
    components:{
      SearchTable
    },
    data(){
      return{
        data:[],
        page:1
      }
    },
    methods:{
      /* 跳转到详细页面 */
      onBookClick(book){
        this.$router.push({
          path:'/pages/detail/main',
          query:{
            /* 准确跳转 */
            fileName: book.fileName
          }
        })
      },
      getSearchList(){
          const {key,text} = this.$route.query
          const params = {}
          if(key && text){
            /* 存在的话一一对应 */
            params[key] = text
          }
          params.page = this.page
          searchList(params).then(response => {
            const{data} = response.data
            if(data.length > 0){
            this.data.push(...data)
            }else{
              showToast('没有更多数据了')
            }

          })
      }
    },
    mounted(){
      this.page = 1
      this.getSearchList()
      const {title} = this.$route.query
      /* 动态设置标题 */
      setNavigationBarTitle(title)
    },
    onReachBottom(){
      console.log('reach...')
      this.page++
      this.getSearchList()
      

    
  }

}
</script>

<style lang="scss" scoped>

</style>
