<template>
  <div>
     <DetailBook :book="book"/>
     <DetailStat
        :readers="book.readers"
        :readerNum="book.readerNum"
        :rankNum="book.rankNum"
        :rankAvg="book.rankAvg" 
     />
     <DetailRate
       :rate-value="book.rateValue"
       @onRateChange = "onRateChange"
     />
     <DetailContents
       :contents="contents"
       @readBook="readBook"
     />
     <!-- <DetailBottom
       :is-in-shelf = "isInShelf"
       @handleShelf = "handleShelf1"
     /> -->
    <div class="detail-bottom">
      <div class="detail-btn-wrapper">
        <van-button
          :custom-class="isInShelf ? 'detail-btn-remove' : 'detail-btn-shelf'"
          round
          @click="handleShelf"
        >
          {{isInShelf ? '移出书架' : '加入书架'}}
        </van-button>
      </div>
      <div class="detail-btn-wrapper">
        <van-button
          custom-class="detail-btn-read"
          round
          @click="() => readBook()"
        >
          阅读
        </van-button>
      </div>
    </div>
  </div>  
</template>
<script>
import DetailBook from '../../components/detail/DetailBook'
import DetailStat from '../../components/detail/DetailStat'
import DetailRate from '../../components/detail/DetailRate'
import DetailContents from '../../components/detail/DetailContents'
import DetailBottom from '../../components/detail/DetailBottom'
import {bookDetail,bookRankSave,bookContents,bookIsInShelf,bookShelfSave,bookShelfRemove} from '../../api/index'
import {getStorageSync} from '../../api/wechat'
export default {
  components:{
    DetailBook,
    DetailStat,
    DetailRate,
    DetailContents,
    DetailBottom
  },
  props:{
    isInShelf: Boolean
  },
  data(){
    return{
      book:{},
      contents:[],
      isInShelf : false
    }
  },
  methods:{
    readBook(href){
      const query={
        fileName: this.book.fileName,
        opf:this.book.opf
      }
      if (href){
        const index = href.indexOf('/')
        if(index >= 0){
          query.navigation = href.slice(index + 1)
        }else{
        query.navigation = href
        }
      }
      
      if(this.book && this.book.fileName){
        this.$router.push({
        path:'/pages/read/main',
        query
      })
      }
      
    },
    /* 加入書架&移出书架 */
    handleShelf(){
      const openId = getStorageSync('openId')
      const {fileName} = this.$route.query
      if(!this.isInShelf){
        bookShelfSave({openId,fileName}).then(() => {
          this.getBookIsInShelf()
        })
    }else{
      /* 通过一下方式拿到this，方便之后this.getBookIsInShelf的使用 */
      const vue = this
      mpvue.showModal({
      title: '提示',
      content: `是否确认将<<${this.book.title}>>移除书架?`,
      success (res) {
        if (res.confirm) {
          /* console.log('用户点击确定') */
          bookShelfRemove({openId,fileName}).then(() => {
          vue.getBookIsInShelf()
        })
        } /* else if (res.cancel) {
          console.log('用户点击取消')
    } */
  }
})
    }
    },
    onRateChange(value){
      /* console.log(value) */
      const openId = getStorageSync('openId')
      const {fileName} = this.$route.query
      bookRankSave({openId,fileName,rank:value}).then(
        response =>{
          /* console.log(response) */
          /* 更新评分 */
          this.getBookDetail()
        }
      )
    },
    /* 将api对接封装成一个方法 */
    getBookDetail(){
      const openId = getStorageSync('openId')
      const {fileName} = this.$route.query
      /* console.log('detail',openId,fileName) */
      if(openId && fileName){
        bookDetail({openId,fileName}).then(response => {
          /* console.log(response) */
          /* 将api数据写入 */
            this.book = response.data.data
      })
    }

    },
    getBookContents(){
      const {fileName} = this.$route.query
      if(fileName){
        bookContents({
          fileName
        }).then(response => {
          this.contents = response.data.data 
        })
      }
    },
    /* 是否在书架api的对接 */
    getBookIsInShelf(){
      const openId = getStorageSync('openId')
      const {fileName} = this.$route.query
      if(openId && fileName){
        bookIsInShelf({openId,fileName}).then(response => {
          const {data} = response.data
          data.length === 0 ? this.isInShelf  = false : this.isInShelf = true
        })
    }
  }
  },
  
  mounted(){
    this.getBookDetail()
    this.getBookContents()
    this.getBookIsInShelf()
  }
}

</script>

<style lang="scss" scoped>
.detail-bottom {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 60px;
    background: #fff;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    padding: 0 15px;
    border-top: 1px solid #eee;
    box-shadow: 0 -2px 4px 0 rgba(0,0,0,.1);

    .detail-btn-wrapper {
      flex: 1;
    }
  }
</style>

<style lang="scss">
  .detail-bottom {
    .detail-btn-read {
      width: 100%;
      border: none;
      color: #fff;
      background: #1EA3F5;
      margin-left: 7.5px;
    }

    .detail-btn-shelf {
      width: 100%;
      color: #1EA3F5;
      background: #fff;
      border: 1px solid #1EA3F5;
      margin-right: 7.5px;
    }

    .detail-btn-remove {
      width: 100%;
      color: #F96128;
      background: rgba(255, 175, 155, .3);
      border: 1px solid #FFAF9B;
      margin-right: 7.5px;
    }
  }
</style>
