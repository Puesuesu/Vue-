<template>
  <div>
    <ShelfUserInfo
      :user-info="userInfo"
      :read-day="readDay"
      :num="shelfList.length"
    />
    <ShelfList :shelf-list="shelfList"/>
  </div>  
</template>
<script>
import ShelfUserInfo from '../../components/shelf/ShelfUserInfo'
import ShelfList from '../../components/shelf/ShelfList'
import {getStorageSync} from '../../api/wechat'
import {userDay,bookIsInShelf} from '../../api/index'
export default {
  components:{
    ShelfUserInfo,
    ShelfList
  },
  data(){
    return{
      userInfo:{},
      readDay:0,
      shelfList:[]
    }
  },
  methods:{
    getReadDay(){
      const openId = getStorageSync('openId')
      userDay({openId}).then(response=>{
      this.readDay = response.data.data.day
      })
    },
    getBookShelf(){
      const openId = getStorageSync('openId')
      bookIsInShelf({openId}).then(response =>{
        this.shelfList = response.data.data
        this.shelfList.push({})
      })
    }
  },
  onShow(){
    this.userInfo = getStorageSync('userInfo')
    this.getReadDay()
    this.getBookShelf()
  }
}
</script>

<style lang="scss" scoped>

</style>
