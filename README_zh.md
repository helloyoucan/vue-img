### @helloyoucan/vue-img

通过npm使用

> npm install @helloyoucan/vue-img

vue version：2.0+

```javascript
import vImg from '@helloyoucan/vue-img'

Vue.prototype.$IMG = {
  lazy:true,
  center:true
}
Vue.component('v-img',vImg)
```



对img标签进行一层封装，主要有三个特点

>懒加载
>
>图片的loading状态
>
>加载失败后的处理



#### props

```javascript
props: {
    src: { // 图片路径
      type: String,
      default: '#'
    },
    alt: {// img标签的原生属性
      type: String,
      default: alt
    },
    lowSrc: {// 图像的低分辨率版本的 URL
      type: String
    },
    loadingImg: {// 加载图片的loading图片
      type: String,
      default: loadingImg
    },
    errorSrc: { // 图片加载失败时代替的图片
      type: String,
      default: errorImg
    },
    showSource: { // 是否以date-src的属性显示原图片到dom上
      type: Boolean,
      default: showSource
    },
    lazy: {// 懒加载
      type: Boolean,
      default: isLazy
    },
    center: { // 是否根据图片尺寸垂直居中或者水平居中
      type: Boolean,
      default: center
    }
  }
```

#### 全局设置（非必要，有内置）

```javascript
Vue.prototype.$IMG = {
  errorImg:"", //加载时失败后的替换图片，默认使用内置
  loadingImg:"",//加载中显示的图片，默认使用内置的css-icon
  lazy:true,//是否使用懒加载，默认true，需要浏览器支持IntersectionObserver（ie不支持）
  alt:"",//img标签的原生属性alt
  showSource:false,//是否以data-src、data-lowsrc的方式显示源图片（当加载失败时使用）
  center: false // 是否根据图片尺寸垂直居中或者水平居中
}

```

#### 效果

![效果图](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/vue-img.png)
