
### 一、安装项目
+ npm install -g @vue/cli
+ vue create -p dcloudio/uni-preset-vue 你的项目名

### 二、如何将vue/cli项目打包成apk
+ npm run build:app-plus
	生成了dis文件夹
	在dist/build文件夹下在H-build编辑器中进行打包apk

### 三、生成android-apk证书
+ Android平台打包发布apk应用，需要使用数字证书（.keystore文件）进行签名，用于表明开发者身份。
+ 需要安装微信者开发工具
+ 安装JRE环境  //可从Oracle官方下载jre安装包：https://www.oracle.com/technetwork/java/javase/downloads/index.html
+ set PATH=%PATH%;"C:\Program Files\Java\jdk-16.0.1\bin"
+ keytool -genkey -alias yourapp.keystore -keyalg RSA -sigalg SHA1WithRSA -validity 20000 -keysize 1024 -keystore yourapp.keystore -v
+ keytool -importkeystore -srckeystore ./yourapp.keystore -destkeystore ./yourapp.keystore -deststoretype JKS
+ keytool -list -v -keystore ./HBuilder.keystore
***window+R启用管理员权限操作***

### 四、安装sass
+ npm install node-sass@4.14.1 --save-dev
+ npm install sass-loader@8.0.2 --save-dev 
***需要注意版本问题***

### 五、安装上拉加载和下拉刷新组件
+ npm install --save mescroll.js
```
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
```
```
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
```
```
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
```
```
import MescrollUni from "@/pages/mescroll-uni/mescroll-uni.vue"; //上拉加载和下拉刷新
Vue.component('mescroll-uni', MescrollUni)//上拉加载和下拉刷新
```

### 六、安装i18n国际化
+ npm install vue-i18n --save
+ //在i18n.js中
import Vue from 'vue';
import VueI18n from 'vue-i18n';
import zh from '@/i18n/zh';
import en from '@/i18n/en';
Vue.use(VueI18n);
const messages = {
	zh, // 这是zh: zh的简写，后面同理
	en,
};
export default new VueI18n({
	locale: 'zh',
	messages,
});
+ //在main.js中
import i18n from '@/utils/i18n';
new App(
	i18n,
).$mount()
Vue.prototype.$i18nMsg = i18n.messages[i18n.locale] //挂载上去this.$i18nMsg.xxx去访问
+ 在i18n文件夹中
zh.js
en.js
//this.$i18n.locale = this.$i18n.locale === 'zh' ? 'en' : 'zh'

### 七、安装vuex
+ npm install vuex --save
+ 新建store/index.js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const store = new Vuex.Store({
	state: {
		// state是存储中心，所有需要被共享或缓存的数据，都在这里定义
		status: true,
		token: "7758258",
	},
	getters: {
		// getters相当于组件的计算属性，它与state相关，当它所关系的state变量发生变化时，会自动重新计算
	},
	mutations: {
		// mutations是Vuex中专门用于更新state,同步任务
		toChaneStatus(state, pyylod) {
			console.log(state)
			console.log(pyylod)
		}
	},
	actions: {
		toChang(store, payload) {
			console.log(store)
			console.log(payload)
			store.commit("toChaneStatus", payload)
		}
		// actions是专门与后端api打交道的，异步任务;提交的是 mutation，而不是直接变更状态;可以包含任意异步操作。
	},
	modules: {
		//vuex分模块处理
	}
})
export default store
+ main.js
import Vuex from 'vuex'
import store from './store'
const app = new Vue({
	store
})
app.$mount()

### 八、安装vue-router
+ npm install vue-router
+ 在main.js中
import VueRouter from 'vue-router'
Vue.use(VueRouter)

### 九、封装uni.request(options)请求
+ @/serve/request.js
+ 解决跨域问题manifest.json
"h5": {
        "devServer": {
            "port": 8080,
            "disableHostCheck": true,
            "proxy": {
                "/he": {
                    "target": "https://way.jd.com",
                    "changeOrigin": true,
                    "secure": false,
                    "pathRewrite": {
                        "/he": "/he"
                    }
                }
            }
        }
    }

### 十、解决用户栏与顶部栏重合问题
+ 在manifest.json中
 "app-plus": {
        /* 导航栏和状态栏重叠问题 */
        "statusbar": {
            "immersed": false
        },
	}
### 十一、封装tabBar组件
+ 利用父子组件传值控制路由跳转时tabBar的class类名的变化实现切换效果
+ uniapp的获取路由的方法
	let routes = getCurrentPages() // 获取当前打开过的页面路由数组
    let curRoute = routes[routes.length - 1].route // 获取当前页面路由，也就是最后一个打开的页面路由

### 十二、图片预览插件v-viewer
+ npm install v-viewer -s
```
import Viewer from 'v-viewer'
import 'viewerjs/dist/viewer.css'
Vue.use(Viewer);
Viewer.setDefaults({
	Options: { "inline": true, "button": true, "navbar": true, "title": true, "toolbar": true, "tooltip": true, "movable": true, "zoomable": true, "rotatable": true, "scalable": true, "transition": true, "fullscreen": true, "keyboard": true, "url": "data-source" }
});
```
```
三种使用方式
组件
指令
api
```