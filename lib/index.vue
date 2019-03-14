<template>
  <div class="v-img" :style="{width:width,height:height}">
    <template v-if="state<2">
      <img v-if="loadingImg" ref="loading" :src="loadingImg" alt="加载中..." class="v_i-loading-img">
      <div v-else ref="loading" class="v_i-loading-icon"></div>
    </template>
    <img
        v-if="state>0"
        class="v_i-target"
        :class="{center:center,loading:this.state===1}"
        :style="imgStyle"
        :src="imgSrc"
        :alt="alt"
        :data-src="showSource&&src"
        @load="handleLoad"
        @error="handleError">
  </div>
</template>
<script>
import Vue from 'vue'
import defaultErrorImg from '../assets/def_img_square.png'

export default {
  name: 'v-img',
  components: {},
  props: {
    width: {
      type: String
    },
    height: {
      type: String
    },
    src: { // 图片路径
      type: String,
      default: '#'
    },
    alt: {// img标签的原生属性
      type: String,
      default: function() {
        return (Vue.prototype.$IMG || {}).alt
      }
    },
    loadingImg: {// 加载图片的loading图片
      type: String,
      default: function() {
        return (Vue.prototype.$IMG || {}).loadingImg
      }
    },
    errorSrc: { // 图片加载失败时代替的图片
      type: String,
      default() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.errorSrc
          ? Vue.prototype.$IMG.errorSrc : defaultErrorImg
      }
    },
    showSource: { // 是否以date-src的属性显示原图片到dom上
      type: Boolean,
      default() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.showSource
          ? Vue.prototype.$IMG.showSource : false
      }
    },
    lazy: {// 懒加载
      type: Boolean,
      default() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.lazy
          ? Vue.prototype.$IMG.lazy : false
      }
    },
    center: { // 是否根据图片尺寸垂直居中或者水平居中
      type: Boolean,
      default() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.center
          ? Vue.prototype.$IMG.center : false
      }
    },
    ratio: {// 比例系数，用于center===true时，用于图片原生尺寸和渲染尺寸的width/height比例的差值对比然后去设置图片的width或者height为auto的依据
      type: Number,
      default() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.ratio
          ? Vue.prototype.$IMG.ratio : 0
      }
    }
  },
  data() {
    let state = 1 // 0:懒加载等待加载 1:加载中 2:加载完成 3:加载失败
    if (this.lazy && window.IntersectionObserver !== undefined) { // 是否启用懒加载并且支持IntersectionObserver
      state = 0
    }
    return {
      state, // 组件内置状态
      imgSrc: state === 0 ? '' : this.src,
      observer: null,
      lazySrc: this.src,
      imgStyle: {}
    }
  },
  watch: {
    src(newVal) {
      this.lazySrc = newVal
      if (this.state === 1) {
        this.lazySrc = newVal
        this.imgSrc = newVal
      }
      if (this.state > 1) { // 不在懒加载等待状态
        this.state = 1
      }
    },
    state(val) {
      // 0:等待加载 1:加载中 2:加载完成 3:加载失败
      switch (val) {
        case 1:
          this.imgSrc = this.lazySrc
          break
        case 2:
          break
        case 3:
          this.imgSrc = this.errorSrc
          break
      }
    }
  },
  mounted() {
    if (this.state === 0) {
      this.observer = new IntersectionObserver(([entry]) => {
        if (entry && entry.isIntersecting) {
          this.state = 1
          this.observer.unobserve(entry.target)
          this.observer.disconnect()
        }
      })
      this.observer.observe(this.$el)
    }
  },
  methods: {
    handleError(e) {
      if (this.state === 1) {
        this.$emit('load', e)
        this.state = 3
      }
    },
    handleLoad(e) {
      if (this.state === 1) {
        this.$emit('load', e)
        // 加载失败时不做居中处理
        if (this.center) {
          const $el_img = this.$el.querySelector('.v_i-target')
          const width_ratio = $el_img.naturalWidth / this.$el.clientWidth
          const height_ratio = $el_img.naturalHeight / this.$el.clientHeight
          if (width_ratio - height_ratio > this.ratio) {
            this.imgStyle = { width: '100%', height: 'auto' }
          } else if (height_ratio - width_ratio > this.ratio) {
            this.imgStyle = { width: 'auto', height: '100%' }
          } else {
            this.imgStyle = { width: '100%', height: '100%' }
          }
        }
        // 先调整居中位置，再显示图片
        this.state = 2
      }
    }
  },
  beforeDestroy() {
    this.observer && this.observer.disconnect()
  }
}
</script>

<style>
  .v-img {
    position: relative;
  }

  /* .v-img, */
  .v_i-target,
  .v_i-loading-img,
  .v_i-loading-icon {
    width: 100%;
    height: 100%;
  }

  .v_i-target.center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    /* max-width: 100%; */
    /* max-height: 100%; */
  }

  .v_i-target.loading {
    opacity: 0;
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
  }

  .v_i-loading-icon:after {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    -webkit-animation: myRotate 1s infinite;
    animation: myRotate 1s infinite;
    width: 30px;
    height: 30px;
    max-width: 100%;
    max-height: 100%;
    border: 2px solid #409EFF;
    /* border-right-color: transparent; */
    border-left-color: transparent;
    border-radius: 50%;
  }
</style>
