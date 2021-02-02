<template>
  <view class="lucky-box" :style="{ width: boxWidth + 'px', height: boxHeight + 'px' }">
    <canvas id="lucky-canvas" @touchstart="handleClick" :style="{ width: boxWidth + 'px', height: boxHeight + 'px' }" canvasId="luckyWheel" />
    <view class="lucky-wheel-btn" @touchstart="toPlay" :style="{ width: btnWidth + 'px', height: btnHeight + 'px' }"></view>
    <view v-if="flag !== 'WEB'" class="lucky-imgs">
      <view v-for="(block, index) in blocks" :key="index">
        <view v-if="block.imgs">
          <image v-for="(img, i) in block.imgs" :key="i" :src="img.src" @load="e => imgBindload(e, 'blocks', index, i)"></image>
        </view>
      </view>
    </view>
    <view v-if="flag !== 'WEB'" class="lucky-imgs">
      <view v-for="(prize, index) in prizes" :key="index">
        <view v-if="prize.imgs">
          <image v-for="(img, i) in prize.imgs" :key="i" :src="img.src" @load="e => imgBindload(e, 'prizes', index, i)"></image>
        </view>
      </view>
    </view>
    <view v-if="flag !== 'WEB'" class="lucky-imgs">
      <view v-for="(btn, index) in buttons" :key="index">
        <view v-if="btn.imgs">
          <image v-for="(img, i) in btn.imgs" :key="i" :src="img.src" @load="e => imgBindload(e, 'buttons', index, i)"></image>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import Taro from '@tarojs/taro'
import { LuckyWheel } from 'lucky-canvas'
import { changeUnits, resolveImage, getFlag } from '../utils'
export default {
  props: {
    width: { type: [String, Number], default: '600rpx' },
    height: { type: [String, Number], default: '600rpx' },
    blocks: { type: Array, default: () => [] },
    prizes: { type: Array, default: () => [] },
    buttons: { type: Array, default: () => [] },
    defaultStyle: { type: Object, default: () => { return {} } },
    defaultConfig: { type: Object, default: () => { return {} } },
  },
  data () {
    return {
      ctx: null,
      flag: getFlag(),
      $lucky: null,
      boxWidth: 300,
      boxHeight: 300,
      btnWidth: 0,
      btnHeight: 0,
    }
  },
  mounted () {
    try {
      this.init()
      this.$emit('success')
    } catch (err) {
      this.$emit('error', err)
    } finally {
      this.$emit('finally')
    }
  },
  methods: {
    async imgBindload (res, name, index, i) {
      const img = this[name][index].imgs[i]
      resolveImage(res, img)
    },
    init () {
      this.boxWidth = changeUnits(this.width)
      this.boxHeight = changeUnits(this.height)
      const ctx = this.ctx = Taro.createCanvasContext('luckyWheel', this)
      const $lucky = this.$lucky = new LuckyWheel({
        flag: this.flag,
        ctx,
        width: this.boxWidth,
        height: this.boxHeight,
        unitFunc: (num, unit) => changeUnits(num + unit),
        beforeInit: function () {
          const Radius = Math.min(this.config.width, this.config.height) / 2
          ctx.translate(-Radius, -Radius)
        },
        beforeDraw: function () {
          ctx.translate(this.Radius, this.Radius)
        },
        afterDraw () {
          ctx.draw()
        }
      }, {
        ...this.$props,
        start: (...rest) => this.$emit('start', ...rest),
        end: (...rest) => this.$emit('end', ...rest),
      })
      // 动态设置按钮大小
      this.btnWidth = $lucky.maxBtnRadius * 2
      this.btnHeight = $lucky.maxBtnRadius * 2
    },
    play (...rest) {
      this.$lucky.play(...rest)
    },
    stop (...rest) {
      this.$lucky.stop(...rest)
    },
    toPlay () {
      this.$lucky.startCallback()
    },
    handleClick (e) {
      const { x, y } = e.changedTouches[0]
      this.$lucky.drawEasterEggs(x, y, function () {
        this.ctx.draw(true)
      })
    }
  }
}
</script>

<style spcoed>
  .lucky-box {
    position: relative;
    overflow: hidden;
  }
  #lucky-canvas {
    position: absolute;
  }
  .lucky-wheel-btn {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background: rgba(0, 0, 0, 0);
    border-radius: 50%;
  }
  .lucky-imgs {
    width: 0;
    height: 0;
    visibility: hidden;
  }
</style>
