
<div align="center">
  <img src="https://raw.githubusercontent.com/LuckDraw/lucky-canvas/master/logo.png" width="128" alt="logo" />
  <h1>taro-luck-draw 抽奖插件</h1>
  <p>一个基于 Taro 的 ( 大转盘 / 九宫格 ) 抽奖插件</p>
  <p>
    <a href="https://github.com/LuckDraw/taro-luck-draw/stargazers" target="_black">
      <img src="https://img.shields.io/github/stars/LuckDraw/taro-luck-draw?color=%23ffca28&logo=github&style=flat-square" alt="stars" />
    </a>
    <a href="https://github.com/LuckDraw/taro-luck-draw/network/members" target="_black">
      <img src="https://img.shields.io/github/forks/LuckDraw/taro-luck-draw?color=%23ffca28&logo=github&style=flat-square" alt="forks" />
    </a>
    <a href="https://www.npmjs.com/package/taro-luck-draw" target="_black">
      <img src="https://img.shields.io/npm/v/taro-luck-draw?color=%23ffca28&logo=npm&style=flat-square" alt="version" />
    </a>
    <a href="https://www.npmjs.com/package/taro-luck-draw" target="_black">
      <img src="https://img.shields.io/npm/dm/taro-luck-draw?color=%23ffca28&logo=npm&style=flat-square" alt="downloads" />
    </a>
  </p>
  <p>
    <a href="https://github.com/buuing" target="_black">
      <img src="https://img.shields.io/badge/Author-%20buuing%20-7289da.svg?&logo=github&style=flat-square" alt="author" />
    </a>
    <a href="https://github.com/LuckDraw/taro-luck-draw/blob/master/LICENSE" target="_black">
      <img src="https://img.shields.io/github/license/LuckDraw/taro-luck-draw?color=%232DCE89&logo=github&style=flat-square" alt="license" />
    </a>
  </p>
</div>

<br />

## 官方文档 & Demo演示

> **中文**：[https://100px.net/document/taro.html](https://100px.net/document/taro.html)  

> **English**：**If anyone can help translate the document, please contact me** `ldq404@qq.com`

<br />

- **在 js / jq 中使用 [lucky-canvas](https://github.com/luckdraw/lucky-canvas)**

- **在 vue 中使用 [vue-luck-draw](https://github.com/luckdraw/vue-luck-draw)**

- **在 react 中使用 [react-luck-draw](https://github.com/luckdraw/react-luck-draw)**

- **在 uni-app 中使用 [uni-luck-draw](https://github.com/luckdraw/uni-luck-draw)**

- **在 taro 中使用 [taro-luck-draw](https://github.com/luckdraw/taro-luck-draw)**

- **在 微信小程序 中使用 [mini-luck-draw](https://github.com/luckdraw/mini-luck-draw)**

<br />

## 在 Taro 中使用

### 安装

```shell
# npm 安装：
npm install tato-luck-draw

# yarn 安装：
yarn add tato-luck-draw
```

<br />

### 使用

- [点击查看 taro-vue2.x 示例 Demo](https://github.com/LuckDraw/taro-luck-draw#vue2x-%E7%A4%BA%E4%BE%8B-demo)
- [点击查看 taro-vue3.x 示例 Demo](https://github.com/LuckDraw/taro-luck-draw#vue3x-%E7%A4%BA%E4%BE%8B-demo)

- **`taro-vue`**

```vue
<template>
  <view>
    <!-- 大转盘抽奖 -->
    <LuckyWheel width="600rpx" height="600rpx" ...你的配置 />
    <!-- 九宫格抽奖 -->
    <LuckyGrid width="600rpx" height="600rpx" ...你的配置 />
  </view>
</template>

<script>
import { LuckyWheel, LuckyGrid } from 'taro-luck-draw/vue'
export default {
  components: { LuckyWheel, LuckyGrid },
}
</script>
```

- **`taro-react`**

```jsx
// 开发中
```

<br />

### vue2.x 示例 Demo

```vue
<template>
  <view>
    <LuckyWheel
      ref="$lucky"
      width="600rpx"
      height="600rpx"
      :prizes="prizes"
      :blocks="blocks"
      :buttons="buttons"
      :defaultStyle="defaultStyle"
      @start="startCallback"
      @end="endCallback"
    ></LuckyWheel>
  </view>
</template>

<script>
import { LuckyWheel } from 'taro-luck-draw/vue'
export default {
  components: { LuckyWheel },
  data () {
    return {
      blocks: [
        { padding: '13px', background: '#d64737' }
      ],
      prizes: [
        { title: '1元红包', background: '#f9e3bb', fonts: [{ text: '1元红包', top: '18%' }] },
        { title: '100元红包', background: '#f8d384', fonts: [{ text: '100元红包', top: '18%' }] },
        { title: '0.5元红包', background: '#f9e3bb', fonts: [{ text: '0.5元红包', top: '18%' }] },
        { title: '2元红包', background: '#f8d384', fonts: [{ text: '2元红包', top: '18%' }] },
        { title: '10元红包', background: '#f9e3bb', fonts: [{ text: '10元红包', top: '18%' }] },
        { title: '50元红包', background: '#f8d384', fonts: [{ text: '50元红包', top: '18%' }] },
      ],
      buttons: [
        { radius: '50px', background: '#d64737' },
        { radius: '45px', background: '#fff' },
        { radius: '41px', background: '#f6c66f', pointer: true },
        {
          radius: '35px', background: '#ffdea0',
          fonts: [{ text: '开始\n抽奖', fontSize: '18px', top: -18 }]
        }
      ],
      defaultStyle: {
        fontColor: '#d64737',
        fontSize: '14px'
      },
    }
  },
  methods: {
    // 点击抽奖按钮会触发star回调
    startCallback () {
      // 调用抽奖组件的play方法开始游戏
      this.$refs.$lucky.play()
      // 模拟调用接口异步抽奖
      setTimeout(() => {
        // 假设拿到后端返回的中奖索引
        const index = Math.random() * 6 >> 0
        // 调用stop停止旋转并传递中奖索引
        this.$refs.$lucky.stop(index)
      }, 3000)
    },
    // 抽奖结束会触发end回调
    endCallback (prize) {
      console.log(`恭喜你获得${prize.title}`)
    },
  }
}
</script>
```

### vue3.x 示例 Demo

```vue
<template>
  <view>
    <LuckyWheel
      ref="$lucky"
      width="600rpx"
      height="600rpx"
      :prizes="prizes"
      :blocks="blocks"
      :buttons="buttons"
      :defaultStyle="defaultStyle"
      @start="startCallback"
      @end="endCallback"
    ></LuckyWheel>
  </view>
</template>

<script>
import { ref, reactive, toRefs } from 'vue'
import { LuckyWheel } from 'taro-luck-draw/vue'
export default {
  components: { LuckyWheel },
  setup () {
    const $lucky = ref(null)
    const state = reactive({
      blocks: [
        { padding: '13px', background: '#d64737' }
      ],
      prizes: [
        { title: '1元红包', background: '#f9e3bb', fonts: [{ text: '1元红包', top: '18%' }] },
        { title: '100元红包', background: '#f8d384', fonts: [{ text: '100元红包', top: '18%' }] },
        { title: '0.5元红包', background: '#f9e3bb', fonts: [{ text: '0.5元红包', top: '18%' }] },
        { title: '2元红包', background: '#f8d384', fonts: [{ text: '2元红包', top: '18%' }] },
        { title: '10元红包', background: '#f9e3bb', fonts: [{ text: '10元红包', top: '18%' }] },
        { title: '50元红包', background: '#f8d384', fonts: [{ text: '50元红包', top: '18%' }] },
      ],
      buttons: [
        { radius: '50px', background: '#d64737' },
        { radius: '45px', background: '#fff' },
        { radius: '41px', background: '#f6c66f', pointer: true },
        {
          radius: '35px', background: '#ffdea0',
          fonts: [{ text: '开始\n抽奖', fontSize: '18px', top: -18 }]
        }
      ],
      defaultStyle: {
        fontColor: '#d64737',
        fontSize: '14px'
      },
    })
    // 点击抽奖按钮会触发star回调
    function startCallback () {
      // 调用抽奖组件的play方法开始游戏
      $lucky.value.play()
      // 模拟调用接口异步抽奖
      setTimeout(() => {
        // 假设拿到后端返回的中奖索引
        const index = Math.random() * 6 >> 0
        // 调用stop停止旋转并传递中奖索引
        $lucky.value.stop(index)
      }, 3000)
    }
    // 抽奖结束会触发end回调
    function endCallback (prize) {
      console.log(`恭喜你获得${prize.title}`)
    }
    return {
      ...toRefs(state),
      startCallback,
      endCallback,
      $lucky
    }
  }
}
</script>
```

<br />

### **如果您觉得这个项目还不错, 可以在 [Github](https://github.com/LuckDraw/taro-luck-draw) 上面帮我点个`star` ☜(ﾟヮﾟ☜)**

<br />

## 推荐的开源项目

- [🎁 h5-Dooring 一款功能强大，高可扩展的H5可视化编辑器](https://github.com/MrXujiang/h5-Dooring)
