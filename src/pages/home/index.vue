<template>
  <div>
    <!-- 搜索框 -->
    <search-bar></search-bar>
    <!-- <div class="search-bar">
      <div class="search-input">
        <input placeholder="搜索">
      </div>
    </div> -->
    <!-- 轮播图 -->
    <swiper indicator-dots='true'>
      <swiper-item :key="item.goods_id" v-for="item in swiper">
        <image :src="item.image_src" class="slide-image" />
      </swiper-item>
    </swiper>
    <!-- 菜单选项 -->
    <div class="menu">
      <div class="menu-item" :key="index" v-for="(item,index) in menu">
        <img :src="item.image_src" alt="">
      </div>
    </div>
    <!-- 商品列表 -->
    <div class="floor" v-for="(item,index) in floor" :key="index">
      <div class="floor-title">
        <img :src="item.floor_title.image_src" mode='aspectFill'>
      </div>
      <div class="floor-content">
        <dic class="left">
          <img :src="item.product_list[0].image_src" mode='aspectFill'>
        </dic>
        <dic class="right">
          <img v-if="i>0" v-for="(img,i) in item.product_list" :key="i" :src="img.image_src" mode='aspectFill'>
        </dic>
      </div>
    </div>
    <!-- 回到顶部按钮 -->
    <div class="toTop" @click="totopHandle" v-if="isShow">
      ︿
      <p>顶部</p>
    </div>
  </div>
</template>
<script>
import request from "../../utils/request.js";
import searchBar from '../../components/searchbar'
export default {
  data() {
    return {
      swiper: [],
      menu: [],
      floor: [],
      isShow: false
    };
  },
  components:{
    searchBar
  },
  methods: {
    async queryData(path) {
      let res = await request(path);
      return res.data.message;
    },
    // 代码未封装之前的步骤~
    BestView(){
      // //   获取首页轮播图数据
      // async swiperData() {
      //   // var that = this;
      //   // mpvue.request({
      //   //   url: "https://www.zhengzhicheng.cn/api/public/v1/home/swiperdata",
      //   //   success: function(res) {
      //   //     let { message } = res.data;
      //   //     that.swiper = message;
      //   //   }
      //   // });
      //   // 通过promise获取结果处理
      //   // request(url).then(res=>{
      //   //   // console.log(res);
      //   //   let {message} = res.data
      //   //   this.swiper = message
      //   // })
      //   // async+await 处理
      //   this.swiper = await this.queryData("home/swiperdata");
      // },
      // // 获取首页菜单选项
      // async menuData() {
      //   // var that = this;
      //   // mpvue.request({
      //   //   url: "https://www.zhengzhicheng.cn/api/public/v1/home/catitems",
      //   //   success: function(res) {e
      //   //     console.log(res);
      //   //     let { message } = res.data;
      //   //     that.menu = message;
      //   //   }
      //   // });
      //   this.menu = await this.queryData("home/catitems");
      // },
      // // 获取楼层数据
      // async floorData() {
      //   this.floor = await this.queryData("home/floordata");
      // },
      // 返回顶部
    },
    totopHandle() {
      mpvue.pageScrollTo({
        scrollTop: 0
      });
    },
    async initData() {
      this.floor = await this.queryData("home/floordata");
      this.swiper = await this.queryData("home/swiperdata");
      this.menu = await this.queryData("home/catitems");
    }
  },

  mounted() {
    // this.swiperData();
    // this.menuData();
    // this.floorData();
    this.initData();
  },
  //小程序生命周期,监听页面滚动
  onPageScroll(event) {
    this.isShow = event.scrollTop > 50;
  },
  //小程序生命周期,下拉刷新,重新加载页面数据
  onPullDownRefresh() {
    this.initData();
  }
};
</script>

<style scoped lang='scss'>
  @import 'main.scss'
</style>

