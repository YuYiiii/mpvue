<template>
    <div>
        <!-- 搜索条组件~ -->
        <search-bar></search-bar>
        <!-- 菜单和内容区域 -->
        <div class="content">
            <div class="left">
                <div @click="changeBrand(index)" :class="{active:currentIndex==index}" v-for="(item,index) in cate" :key="index" class="menu-item">
                    {{item.cat_name}}
                </div>
            </div>
            <div class="right">
                <div class="brand-item" v-for="(item,index) in rightData" :key="index">
                    <div class="brand-title">{{item.cat_name}}</div>
                    <div class="brand-list">
                        <div class="brand" v-for="(img,index1) in item.children" :key="index1">
                            <img :src="img.cat_icon" alt="">
                            <p>{{img.cat_name}}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import searchBar from "@/components/searchbar";
import request from "../../utils/request";
export default {
  data() {
    return {
      cate: [],
      currentIndex: 0,
      rightData: []
    };
  },
  components: {
    searchBar
  },
  methods: {
    async catedata() {
      let ret = await request("categories");
      this.cate = ret.data.message;
      //   console.log(ret);
    },
    // 根据索引点击切换对应数据
    changeBrand(index) {
        console.log(index);
        
      this.currentIndex = index;
      this.rightData = this.cate[this.currentIndex].children;
    }
  },
  async mounted() {
    await this.catedata();
    // 从全部分类数据根据当前索引取出需要的数据
    this.rightData = this.cate[this.currentIndex].children;
    console.log(this.rightData);
  }
};
</script>

<style lang="scss" scoped>
@import "./main.scss";
</style>

