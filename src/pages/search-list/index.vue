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
import searchBar from '../../components/searchbar'
import request from '../../utils/request.js'

export default {
  data () {
    return {
      tabNames: ['综合', '销量', '价格'],
      keyword: '',
      currentIndex: 0,
      list: [],
      pagenum: 1,
      total: 0,
      // 保证接口调用完成之后才可以再次调用接口，如果接口正在获取数据，那么在这个过程中是不允许再次触发接口调用
      isLoading: false,
      hasMore: false
    }
  },
  methods: {
    tabHandle(index) {
      //   修改当前tab的索引
      this.currentIndex = index;
    },
    async loadData() {
      // 如果没有更多数据,就禁止请求
      //是否加载数据完成
      if (this. hasMore || this. isLoading) {
        return;
      }
      //   禁止再次触发接口调用
      this.isLoading = true;
      // 根据关键字加载匹配的商品列表数据
      let res = await request("goods/search", "get", {
        query: this.keyword,
        pagenum: this.pagenum
      });
      let { message } = res.data;
      let goods = [...this.list, ...message.goods];
      this.list = goods;
      this.pagenum = parseInt(message.pagenum);
      this.total = message.total;
      if (this.list.length >= this.total) {
        //  进来则证明没有更多数据了
        this.hasMore = true;
      }
      // 加载完成数据之后使得页码加一
      this.pagenum = this.pagenum + 1;
      //   接口数据返回之后才允许在再次发起请求
      this.isLoading = false;
    }
  },
  //   小程序的生命周期------路径处理-展示在input中默认显示的值
  onLoad(param) {
    //   参数param表示路径传递过来的参数
    this.keyword = param.query;
    // 页面初次展示的时候调用该方法加载数据
    this.loadData();
  },
  onReachBottom() {
    //   用户下拉触底事件-滚动条触底触发事件
    // console.log(41411);
    this.loadData();
    // console.log(this.pagenum);
  },
  components: {
    searchBar
  }
};
</script>

<style lang="scss" scoped>
@import "main.scss";
</style>