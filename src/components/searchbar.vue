<template>
    <div class="search-bar">
        <div class="search-input">
            <navigator url='/pages/search/main'>
                <icon type="search" color="#f40" size='16px'></icon>
                <span>搜索</span>
            </navigator>
        </div>
    </div>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
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
</style>
