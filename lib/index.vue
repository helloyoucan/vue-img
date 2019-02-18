<template>
  <div class="v-img" ref="v-img">
    <img v-if="loadingImg" ref="loading" :src="loadingImg" alt="加载中..." class="v_i-loading-img">
    <div v-else ref="loading" class="v_i-loading-icon"></div>
    <img
        class="v_i-target"
        :src="imgSrc"
        :lowsrc="imgLowSrc"
        :alt="alt"
        :data-src="showSource&&src"
        :data-lowsrc="showSource&&lowSrc"
        @load="handleLoad"
        @error="handleError($event)">
  </div>
</template>
<script>
/** 对原生img标签封装一层，处理加载图片失败时的，显示一个默认图片*/
import defImgSquare from '@/assets/imgs/def_img_square.png'

export default {
  name: 'v-img',
  components: {},
  props: {
    src: { // 图片路径
      type: String,
      default: '#'
    },
    alt: {// img标签的原生属性
      type: String
    },
    lowSrc: {// 图像的低分辨率版本的 URL
      type: String
    },
    loadingImg: {// 加载图片的loading图片
      type: String
    },
    errorSrc: { // 图片加载失败时代替的图片
      type: String,
      default: defImgSquare
    },
    showSource: { // 是否以date-src的属性显示原图片到dom上
      type: Boolean,
      default: true
    },
    lazy: {// 懒加载
      type: Boolean,
      default: true
    }
  },
  data() {
    let state = 1 // 0:等待加载 1:加载中 2:加载完成 3:加载失败
    if (this.lazy && IntersectionObserver) { // 是否启用懒加载并且支持IntersectionObserver
      state = 0
    }
    return {
      state, // 组件内置状态
      imgSrc: this.lazy ? '' : this.src,
      imgLowSrc: this.lazy ? '' : this.lowSrc,
      observer: null,
      lazySrc: this.src,
      lazyLowSrc: this.lowSrc
    }
  },
  watch: {
    src(val) {
      if (this.state > 0) {
        this.imgSrc = val
      } else {
        this.lazySrc = val
      }
    },
    lowSrc(val) {
      if (this.state > 0) {
        this.imgLowSrc = val
      } else {
        this.lazyLowSrc = val
      }
    },
    state(val) {
      // 0:等待加载 1:加载中 2:加载完成 3:加载失败
      switch (val) {
        case 1:
          this.imgSrc = this.lazySrc
          this.lowSrc = this.lazyLowSrc
          break
        case 2:
          this.$refs['loading'] && this.$refs['loading'].remove()
          break
        case 3:
          this.imgSrc = this.errorSrc
          if (this.imgLowsrc) {
            this.imgLowsrc = this.errorImg
          }
          break
      }
    }
  },
  created() {},
  mounted() {
    if (this.state === 0) {
      this.observer = new IntersectionObserver(([entry]) => {
        if (entry && entry.isIntersecting) {
          this.state = 1
          this.observer.unobserve(entry.target)
        }
      })
      this.observer.observe(this.$el)
    }
  },
  methods: {
    handleError(e) {
      if (this.state === 1) {
        this.state = 3
      }
    },
    handleLoad() {
      if (this.state === 1) {
        this.state = 2
      }
    }
  },
  beforeDestroy() {
    this.observer && this.observer.disconnect()
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .v-img {
    overflow: hidden;
  }

  .v-img,
  .v_i-target,
  .v_i-loading-img,
  .v_i-loading-icon {
    width: 100%;
    height: 100%;
  }

  @keyframes myRotate {
    0% {
      transform: translate(-50%, -50%) rotate(0deg);
    }
    100% {
      transform: translate(-50%, -50%) rotate(360deg);
    }
  }

  .v_i-loading-icon {
    position: relative;
    &:after {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      animation: myRotate 1s infinite;
      width: 15px;
      height: 15px;
      border: {
        top: 4px solid #409EFF;
        right: 4px solid transparent;
        bottom: 4px solid #409EFF;
        left: 4px solid transparent;
      }
    ;
      border-radius: 50%;
    }
  }
</style>
