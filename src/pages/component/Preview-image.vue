<template>
  <view class="box">
    <!-- 一种用法 -->
    <viewer :images="imgs">
      <img v-for="src in imgs" :src="src.url" :key="src.title" />
    </viewer>
    <hr />
    <!-- 二种用法 -->
    <div class="images" v-viewer>
      <img v-for="src in images" :key="src" :src="src" />
    </div>
    <!-- 三种用法 -->
    <button type="button" @click="show">Click to show</button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      imgs: [
        {
          url: '/static/top@2x.png',
          title: '图片1',
        },
      ],
      images: [
        'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=85690711,3884201894&fm=27&gp=0.jpg',
      ],
    }
  },
  methods: {
    show() {
      this.$viewerApi({
        images: this.images,
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.box {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 50rpx;
}
</style>