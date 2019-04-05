<template>
    <div>
        <!-- 搜索条 -->
        <!-- <search-bar></search-bar> -->
        <div class="search">
            <div class="search-input">
                <icon type="search" size="16" color="#999" /> {{keyword}}
            </div>
        </div>
        <!-- 选项卡 -->
        <div class="tabs">
            <div @click="tabHandle(index)" :class="{active:currentIndex===index}" v-for="(item,index) in tabNames" :key="index" class="tab-item">
                {{item}}
            </div>
        </div>
        <!-- 商品列表 -->
        <div class="goods-list">
            <navigator :url="'/pages/goods_detail/main?goods_id=' + item.goods_id" class='goods-item' :key='index' v-for='(item, index) in list'>
                <img :src="item.goods_small_logo" mode="aspectFill" />
                <div class="goods-right">
                    <h4>
                        {{item.goods_name}}
                    </h4>
                    <div class="price">
                        <span>￥</span>{{item.goods_price}}
                    </div>
                </div>
            </navigator>
        </div>
        <!-- 提示没有更多数据了 -->
        <div class="more" v-if='hasMore'>
            没有更多数据了
        </div>
    </div>
</template>

<script>
import request from "../../utils/request.js";
import searchBar from "@/components/searchbar.vue";
export default {
  data() {
    return {
      keyword: "",
      tabNames: ["综合", "销量", "价格"],
      currentIndex: 0,
      list: [],
      pagenum: 1,
      total: 0
    };
  },
  methods: {
    tabHandle(index) {
      //   修改当前tab的索引
      this.currentIndex = index;
    }
  },
  //   小程序的生命周期------路径处理-展示在input中默认显示的值
  async onLoad(param) {
    //   参数param表示路径传递过来的参数
    this.keyword = param.query;
    let res = await request("goods/search", "get", {
      query: param.query
    });
    this.list = res.data.message.goods;
    this.pagenum = res.data.message.pagenum;
    this.total = res.data.message.total;
  },
  components: {
    searchBar
  }
};
</script>

<style lang="scss" scoped>
@import "main.scss";
</style>