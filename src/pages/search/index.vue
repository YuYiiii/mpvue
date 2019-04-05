<template>
  <div>
    <div class="search">
      <div class="search-content">
        <div class="search-input">
          <icon type='search' size='16' color='red' />
          <input @confirm="confirmHandle" @input="inputHandle" v-model="keyword" placeholder="ヾ(•ω•`。)请输入关键字" />
        </div>
        <button v-if="keyword" class="cancel" @click="cancelHandle">取消</button>
      </div>
      <div v-if='keyword' class="search-modal">
        <div :key='item.goods_id' v-for='item in searchResult' class="search-item">
          {{item.goods_name}}
        </div>
      </div>

    </div>
    <!-- 搜索历史关键字 -->
    <div class="history">
      <h4>搜索历史</h4>
      <icon type="clear" size="16" @click="clearHistory" />
    </div>
    <div class="history-list">
      <div v-for="(item,index) in keywordHistory" :key="index" class="history-item">
        <navigator :url="'/pages/search-list/main?query=' + item">
          {{item}}
        </navigator>
      </div>
    </div>
  </div>
</template>

<script>
import request from "../../utils/request";
export default {
  data() {
    return {
      keyword: "",
      searchResult: [],
      isLoading: false,
      keywordHistory: mpvue.getStorageSync("keyword") || []
      // timer: null
    };
  },
  methods: {
    // 删除历史记录列表
    clearHistory() {
      // 清空本地存储的数据
      mpvue.clearStorageSync();
      // 清空data中的数据
      this.keywordHistory = [];
    },
    // 存储本地搜索记录
    confirmHandle() {
      // 当回车的时候记录关键字到本地存储
      this.keywordHistory.unshift(this.keyword);
      // 进行数组去重
      let keyWH = [...new Set(this.keywordHistory)];
      // 更新页面数据
      this.keywordHistory = keyWH;
      // 把最新的数据覆盖到本地存储中
      mpvue.setStorageSync("keyword", keyWH);
      // 跳转到商品列表页---详情:图片价格文字描述
      mpvue.navigateTo({
        url: `/pages/search-list/main?query=${this.keyword}`
      });
    },
    // inputHandle() {
    //   // 根据输入的关键字调用后台接口查询匹配的数据
    //   // 当表单事件触发时发送请求获取搜索建议,然后把搜索建议渲染在搜索框下边
    //   //   通过isloading控制请求次数,设置定时器触发再修改,这样就可以在规定时间内只发送一次请求,避免请求消耗且增加用户体验
    //   if (this.isLoading) {
    //     return;
    //   }
    //   this.isLoading = true;
    // //   设置定时器->进入后首先清楚原有的定时器->发送请求等一系列操作->改isloading值???->延时后发送请求,延时即保证,1秒内不会再发送任何请求
    //     clearTimeout(this.timer);
    //   let timer = setTimeout(async () => {
    //     let res = await request(`goods/qsearch?query=${this.keyword}`);
    //     console.log(res);
    //     this.searchResult = res.data.message;
    //     this.isLoading = false;
    //   }, 1000);
    // },
    inputHandle() {
      // 防抖是从结果考虑-----windows某种情况改变窗口大小的时候----用户搜索联想的时候
      // 节流是从过程(频率)考虑=====鼠标移动----表单改变事件
      // 函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行
      // 比如乘电梯，假设delay是10秒，name防抖就是电梯每进来一个人就要等10秒再运行，而节流就是电梯保证每10秒可以运行一次
      if (this.isLoading) {
        return;
      }
      this.isLoading = true;
      let timer = setTimeout(async () => {
        clearTimeout(this.timer);
        let res = await request(`goods/qsearch?query=${this.keyword}`);
        console.log(res);
        this.searchResult = res.data.message;
        this.isLoading = false;
      }, 2000);
    },
    // 给取消按钮绑定点击事件.清空输入框
    cancelHandle() {
      this.keyword = "";
    }
  }
};
</script>

<style lang="scss" scoped>
@import "main.scss";
</style>

