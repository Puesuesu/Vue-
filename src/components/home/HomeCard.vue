<template>
  <div class="home-card">
      <div class="home-card-inner">
          <div class="user-info">
              <div class="avatar-wrapper">
                <ImageView
                  :src="avatar"
                  round
                  height="100%"
                  mode="scaleToFill"
                />
              </div>
              <div class="nickname">{{nickname}}</div>
              <div class="shelf-text">书架共有{{data.num}}本好书</div>
              <div class="round-item"></div>
              <div class="shelf-text">特别精选</div>
          </div>                
          <div class="book-info">
            <div class="book-wrapper">   
              <div class="book-img" v-for="(item,index) in bookList" :key="index" @click="onBookClick(item)">  
                <ImageView
                :src="item.cover"
                />
              </div>

            </div>
            <div class="shelf-wrapper" @click="gotoShelf">
              <div class="shelf">书架</div>
              <van-icon class="arrow" name="arrow" size="11px" color="#828489"></van-icon>
            </div>
          </div>
          <div class="feedback-wrapper" @click="onFeedBackClick">
            <span>反馈</span>
          </div>
      </div>
      <van-dialog id="van-dialog"></van-dialog>
  </div>
</template>

<script>
  import ImageView from '../../components/base/ImageView'
  import Dialog from 'vant-weapp/dist/dialog/dialog'
  export default{
    components:{ImageView},
    props:{
        data:Object,
        hasSign:{
            type:Boolean,
            default:false,
        },
        signDay:{
            type:Number,
            default:0

        }
    },
    computed:{
      avatar(){
        return (this.data && this.data.userInfo && this.data.userInfo.avatarUrl) || ''
      },
      nickname(){
        return (this.data && this.data.userInfo && this.data.userInfo.nickName) || ''
      },
      bookList(){
        return (this.data && this.data.bookList) || []
      }
    },
    methods:{
        gotoShelf(){
          this.$router.push('/pages/shelf/main')
        },
        onBookClick(item){
          this.$emit('onClick',item)
        },
        sign(){},
        onFeedBackClick(){
            Dialog.confirm({
              title : '反馈',
              message:'您是否确认提交反馈信息？',
              confirmButtonText:'是',
              cancelButtonText:'否'

            }).then(()=>{
                console.log('点击是之后的事件')
            }).catch(()=>{
                console.log('点击否之后的事件')
            })

        }
    }

  }
</script>

<style lang="scss" scoped>
  .home-card{
      background-image: linear-gradient(-90deg, #54575F 0%, #595B60 100%);
      border-radius: 15px;
      margin: 22px 20px 0;
      .home-card-inner{
          padding: 21.5px 17px 20px 20px;
          box-sizing: border-box;
          position: relative;
          .user-info{
              display: flex;
              align-items: center;
              .avatar-wrapper{
                  width:15px;
                  height:15px;
              
              }
              .nickname{
                  font-size: 12px;
                  color: #FFFFFF;
                  margin: 0 8.5px;
              }
              .shelf-text{
                  font-size: 12px;
                  color: #FFFFFF;
                  opacity: 0.7;
              }
              .round-item{
                  width: 4px;
                  height: 4px;
                  background : #A2A2A2;
                  border-radius: 50%; 
                  margin: 0 8px;
              }
          }
          .book-info{
            display: flex;
            margin-top: 16.5px;
            .book-wrapper{
                display: flex;
                flex:1;
                justify-content: space-around;
                .book-img
                {
                  width:72px;
                  height:101px

                }
            }
            .shelf-wrapper{
                display: flex;
                align-items: center;
                .shelf{
                    font-size: 11px;
                    width:11px;
                    color: #FFFFFF;
                    opacity: 0.8;
                }
            }
          }
          .feedback-wrapper{
              position:absolute;
              right:0;
              top:19.5px;
              width:44.5px;
              height:23.5px;
              line-height: 23.5px;
              text-align: center;
              border-radius: 100px 0 0 100px;
              
              background: #707070;
              span{
              font-size: 11px;
              color: #FFFFFF;
              }
          }
              
      }
      
  }

</style>