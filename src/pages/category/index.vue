<template>
    <div>
        <!-- 搜索条组件~ -->
        <search-bar></search-bar>
        <!-- 菜单和内容区域 -->
        <div class="content">
            <div class="left">
                <div :class="{active:currentIndex==index}" v-for="(item,index) in cate" :key="index" class="menu-item">
                    {{item.cat_name}}
                </div>
            </div>
            <div class="right"></div>
        </div>
    </div>
</template>

<script>
import searchBar from "@/components/searchbar";
import request from '../../utils/request';
export default {
  data() {
    return {
        cate:[],
        currentIndex:0
    };
  },
  components: {
    searchBar
  },
  methods: {
    async catedata() {
        let ret = await request('categories')
        this.cate = ret.data.message
        console.log(ret);
    }
  },
  mounted(){
      this.catedata()
  }
};
</script>

<style lang="scss" scoped>
@import "./main.scss";
</style>

