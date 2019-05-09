<template>
  <div>
    <!-- search -->
    <search></search>

    <!-- banner -->
    <swiper class="banner" autoplay indicator-dots interval="2000" circular>

      <swiper-item :key="key" v-for="(list , key) in bannerList">
        <navigator :url="list.navigator_url">
          <image :src="list.image_src" />
        </navigator>
      </swiper-item>

    </swiper>

    <!-- nav -->
    <div class="nav">
      <div class="public" :key="key" v-for="(list , key ) in navList">
        <navigator
          url="list.navigator_url"
          open-type="navigate"
          hover-class="none">
          <image
            :src="list.image_src"
            mode="scaleToFill"
            lazy-load="false">
          </image>
        </navigator>
      </div>

    </div>

    <!-- floor -->
    <div class="floors">

      <div class="floor" :key="key" v-for="(list , key) in floorList">
        <div class="title">
          <image
            :src="list.floor_title.image_src"
            mode="scaleToFill"
            lazy-load="false">
          </image>
        </div>
        <div class="pics">
          <navigator
            :url="item.navigator_url"
            :key="index"
            v-for="(item,index) in list.product_list">
            <image :src='item.image_src' ></image>
          </navigator>
          <!-- <navigator
            url=""
            open-type="navigate"
            hover-class="none">
            <image src='/static/uploads/pic_floor01_2@2x.png' ></image>
          </navigator>
          <navigator
            url=""
            open-type="navigate"
            hover-class="none">
            <image src='/static/uploads/pic_floor01_3@2x.png' ></image>
          </navigator>
          <navigator
            url=""
            open-type="navigate"
            hover-class="none">
            <image src='/static/uploads/pic_floor01_4@2x.png' ></image>
          </navigator>
          <navigator
            url=""
            open-type="navigate"
            hover-class="none">
            <image src='/static/uploads/pic_floor01_5@2x.png' ></image>
          </navigator> -->
        </div>
      </div>

    </div>

    <span class="goTop" @click="goTop" v-show="!isTop"></span>

  </div>
</template>

<script>
import request from '@/utils/request'
import search from '@/components/search'
export default {
  data(){
return{
  bannerList:[],
  navList:[],
  floorList:[],
  isTop:true
}
  },
  components:{
    search
  },
  methods:{
    async getBanner () {
      let {message} = await request ({
        url:'/api/public/v1/home/swiperdata'
      })
      this.bannerList = message
    },
    async getNavs(){
        let {message} = await request ({
      url: '/api/public/v1/home/catitems'
      })
      this.navList= message
    },
    async getFloors(){
        let {message} = await request({
          url: '/api/public/v1/home/floordata',
        });

        console.log(message);
        this.floorList = message;
    },
    goTop(){
      mpvue.pageScrollTo({
  scrollTop: 0,
  duration: 300
})
    }
  },
  mounted(){
    console.log('it is ok ')
      this.getBanner();
      this.getNavs();
      this.getFloors();
  },
  onPullDownRefresh() {
    console.log(2222)
     this.getBanner();
      this.getNavs();
      this.getFloors();
      mpvue.stopPullDownRefresh()

  },
  onPageScroll(ev){
    console.log(ev)
    this.isTop = ev.scrollTop<250
  }
};
</script>

<style scoped lang="less">
/* search */

.search .input-box {
  background-color: rgb(17, 16, 16);
  padding: 20rpx 30rpx;
}
.search .input-box input {
  background-color: #fff;
  border-radius: 10rpx;
  height: 50rpx;
  padding: 20rpx;
  font-size: 40rpx;
}

/* banner */
.banner {
  width: 750rpx;
  height: 340rpx;
}
.banner navigator {
  width: 100%;
  height: 100%;
}
/* nav */
.nav {
  background-color: #ccc;
  height: 192rpx;
  display: flex
}
.nav .public {
  flex: 1;
  padding: 25rpx
  /* border: 1rpx solid #000 */
}
.nav .public navigator{
   width: 100%;
  height: 100%;
}
/* floor */

.floors{
  overflow: hidden;
}
.floor{
  margin-top: 20rpx;
}
.floor title{
  background-color: #ccc;
  height: 100%;
  width: 100%;
}
.floor .title image{
  height: 89rpx;
  width: 100%;
}
.pics{
  height: 426rpx;
  width: 100%
}
.pics navigator{
  height: 188rpx;
}
.pics navigator{
  float: left;
  margin-left: 15rpx;
  margin-top: 10rpx
}
.pics navigator:first-child{
  width: 232rpx;
  height: 386rpx;
}
.pics navigator:nth-child(2){
  width: 232rpx;
}
.pics navigator:nth-child(3){
  width: 232rpx;
}
.pics navigator:nth-child(4){
  width: 232rpx;
}
.pics navigator:last-child{
  width: 232rpx;
}

.goTop{
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  // display: block;
  position: fixed;
  right: 30rpx;
  bottom: 30rpx;
  background-color: #ccc;
}
</style>
