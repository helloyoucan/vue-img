<template>
  <div class="v-img">
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
        :lowsrc="imgLowSrc"
        :alt="alt"
        :data-src="showSource&&src"
        :data-lowsrc="showSource&&lowSrc"
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
    src: { // 图片路径
      type: String,
      default: '#'
    },
    alt: {// img标签的原生属性
      type: String,
      default: function() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.alt ? Vue.prototype.$IMG.alt : undefined
      }
    },
    lowSrc: {// 图像的低分辨率版本的 URL
      type: String
    },
    loadingImg: {// 加载图片的loading图片
      type: String,
      default: function() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.errorImg ? Vue.prototype.$IMG.errorImg : undefined
      }
    },
    errorSrc: { // 图片加载失败时代替的图片
      type: String,
      default: function() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.errorImg ? Vue.prototype.$IMG.errorImg : defaultErrorImg
      }
    },
    showSource: { // 是否以date-src的属性显示原图片到dom上
      type: Boolean,
      default: function() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.showSource ? Vue.prototype.$IMG.showSource : false
      }
    },
    lazy: {// 懒加载
      type: Boolean,
      default: function() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.lazy ? Vue.prototype.$IMG.lazy : false
      }
    },
    center: { // 是否根据图片尺寸垂直居中或者水平居中
      type: Boolean,
      default: function() {
        return Vue.prototype.$IMG && Vue.prototype.$IMG.center ? Vue.prototype.$IMG.center : false
      }
    },
    ratio: {// 比例差系数，用于center===true时，用于图片原生尺寸和渲染尺寸的width/height比例的差值对比然后去设置图片的width或者height为auto的依据
      type: Number,
      default: 1
    }
  },
  data() {
    let state = 1 // 0:等待加载 1:加载中 2:加载完成 3:加载失败
    if (this.lazy && window.IntersectionObserver !== undefined) { // 是否启用懒加载并且支持IntersectionObserver
      state = 0
    }
    return {
      state, // 组件内置状态
      imgSrc: state === 0 ? '' : this.src,
      imgLowSrc: state === 0 ? '' : this.lowSrc,
      observer: null,
      lazySrc: this.src,
      lazyLowSrc: this.lowSrc,
      imgStyle: {}
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
          // this.$refs['loading'] && this.$refs['loading'].remove()
          break
        case 3:
          this.imgSrc = this.errorSrc
          if (this.imgLowsrc) {
            this.imgLowsrc = this.errorImg
          }
          //  this.$refs['loading'] && this.$refs['loading'].remove()
          break
      }
    }
  },
  created() {
  },
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
    handleError() {
      if (this.state === 1) {
        this.state = 3
      }
    },
    handleLoad() {
      if (this.state === 1) {
        const $el_img = this.$el.querySelector('.v_i-target')
        if (this.center) {
          const width_ratio = $el_img.naturalWidth / $el_img.width
          const height_ratio = $el_img.naturalHeight / $el_img.height
          if (width_ratio - height_ratio > this.ratio) {
            this.imgStyle = { width: '100%', height: 'auto' }
          } else if (height_ratio - width_ratio > this.ratio) {
            this.imgStyle = { width: 'auto', height: '100%' }
          } else {
            this.imgStyle = { width: '100%', height: '100%' }
          }
        } else {
          this.imgStyle = { width: '100%', height: '100%' }
        }
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

  .v-img,
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
    transform: translate(-50%, -50%);
    animation: myRotate 1s infinite;
    width: 15px;
    height: 15px;
    border: 4px solid #409EFF;
    border-right-color: transparent;
    border-left-color: transparent;
    border-radius: 50%;
  }
</style>
