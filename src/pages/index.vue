<template>
  <view class="home">
    <mescroll-uni
      class="scroll-wrapper"
      :fixed="false"
      height="100%"
      ref="mescrollRef"
      @init="mescrollInit"
      @down="downCallback"
      @up="upCallback"
      :down="downOption"
      :up="upOption"
      top="0"
    >
      <PreviewImage />
    </mescroll-uni>
    <TabBar current="index" />
  </view>
</template>

<script>
import TabBar from '@/pages/component/tabBar.vue'
import PreviewImage from '@/pages/component/Preview-image.vue'
export default {
  components: {
    TabBar,
    PreviewImage,
  },
  data() {
    return {
      downOption: {
        textLoading: '加载中 ...',
        textOutOffset: '释放刷新',
        use: true, // 是否启用下拉刷新; 默认true
        auto: false, // 是否在初始化完毕之后自动执行下拉刷新的回调; 默认true
        native: false, // 启用系统自带的下拉组件,默认false;仅mescroll-body生效,mescroll-uni无效(native: true, 则需在pages.json中配置"enablePullDownRefresh":true)
      },
      // 上拉加载的常用配置
      upOption: {
        use: true, // 是否启用上拉加载; 默认true
        auto: false, // 是否在初始化完毕之后自动执行上拉加载的回调; 默认true
        page: {
          num: 1, // 当前页码,默认0,回调之前会加1,即callback(page)会从1开始
          size: 5, // 每页数据的数量,默认10
        },
        noMoreSize: 1, // 配置列表的总数量要大于等于5条才显示'-- END --'的提示
        empty: {
          tip: '暂无数据',
          use: false,
          // icon: '/static/realFix-module/no-search@2x.png',
        },

        textNoMore: '-------  ' + '你已经到底了哟' + '  -------',
        toTop: {
          src: '/static/top@2x.png',
        },
      },
    }
  },
  methods: {
    //滚动组件初始化
    mescrollInit(mescroll) {
      this.mescroll = mescroll
    },
    /*下拉刷新的回调*/
    downCallback() {
      console.log('downCallback')
      this.mescroll.endSuccess()
    },
    // 上拉更新更多
    upCallback() {
      console.log('upCallback')
      //  this.mescroll.endErr()
      this.mescroll.endByPage(0, 0)
    },
  },
}
</script>

<style lang="scss" scoped>
.home {
  color: #000;
}
</style>