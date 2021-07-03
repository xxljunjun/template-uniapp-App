<template>
  <view class="device">
    2222222222
    <TabBar current="device" />
  </view>
</template>

<script>
import TabBar from '@/pages/component/tabBar.vue'
export default {
  components: {
    TabBar,
  },
  data() {
    return {}
  },
  methods: {
    goToDetail() {
      console.log('22222222')
      uni.navigateTo({
        url: '/pages/component/device/shebei',
      })
    },
  },
}
</script>

<style lang="scss" scoped>
.device {
}
</style>