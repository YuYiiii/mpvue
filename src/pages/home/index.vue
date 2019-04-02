<template>
  <div>
    <!-- 搜索框 -->
    <div class="search-bar">
      <div class="search-input">
        <!-- <icon type="search" color="#f40" size='16px'></icon> -->
        <input placeholder="搜索">
      </div>
    </div>
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      swiper: [],
      menu: []
    };
  },
  methods: {
    //   获取首页轮播图数据
    swiperData() {
      var that = this;
      mpvue.request({
        url: "https://www.zhengzhicheng.cn/api/public/v1/home/swiperdata",
        success: function(res) {
          let { message } = res.data;
          that.swiper = message;
        }
      });
    },
    // 获取首页菜单选项
    menuData() {
      var that = this;
      mpvue.request({
        url: "https://www.zhengzhicheng.cn/api/public/v1/home/catitems",
        success: function(res) {
          console.log(res);
          let { message } = res.data;
          that.menu = message;
        }
      });
    }
  },
  mounted() {
    this.swiperData();
    this.menuData();
  }
};
</script>

<style>
.search-bar {
  background-color: #f00;
  padding: 20rpx;
}
.search-input {
  background-color: #ccc;
  padding-top: 5rpx;
  padding-bottom: 5rpx;
  text-align: center;
  color: #fef;
  font-size: 16px;
}
.slide-image {
  width: 750rpx;
}
.menu {
  display: flex;
  justify-content: space-around;
}
.menu-item img {
  width: 128rpx;
  height: 140rpx;
}
</style>
