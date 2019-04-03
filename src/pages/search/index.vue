<template>
    <div>
        <div class="search">
            <div class="search-content">
                <div class="search-input">
                    <icon type='search' size='16' color='red' />
                    <input v-model="keyword" placeholder="ヾ(•ω•`。)请输入关键字" />
                </div>
                <button v-if="keyword" class="cancel">取消</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
        keyword:''
    };
  }
};
</script>

<style lang="scss" scoped>
@import "main.scss";
</style>

