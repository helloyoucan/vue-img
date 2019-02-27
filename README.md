[查看中文文档](README_zh.md)

### @helloyoucan/vue-img

Use by npm

> npm install @helloyoucan/vue-img

vue version：2.0+

```javascript
import vImg from '@helloyoucan/vue-img'

Vue.prototype.$IMG = {
  lazy:false,
  center:true
}
Vue.component('v-img',vImg)
```



Img tag for a layer of packaging, there are three main characteristics

> Lazy loading
>
> The loading status of the image
>
> Handling after load failure



#### props

```javascript
props: {
    src: { // Image path
      type: String,
      default: '#'
    },
    alt: {// Native attributes of the img tag
      type: String,
      default: alt
    },
    lowSrc: {// The URL for the low-resolution version of the image
      type: String
    },
    loadingImg: {// Loading a loading image of the image
      type: String,
      default: loadingImg
    },
    errorSrc: { // The image that was replaced when the image failed to load
      type: String,
      default: errorImg
    },
    showSource: { // Whether to display the original image to the dom as a date-src attribute
      type: Boolean,
      default: showSource
    },
    lazy: {// Lazy loading
      type: Boolean,
      default: isLazy
    },
    center: { // Whether to center vertically or horizontally according to picture size
      type: Boolean,
      default: center
    },
    ratio: {// When center===true, it is used to compare the difference between the original size of the image and the width/height of the rendering size, and then set the width or height of the image as the basis of auto
      type: Number,
      default: 1
    }
  }
```

#### Global Settings (not required, built-in)

```javascript
Vue.prototype.$IMG = {
  errorImg:"", //Replace images after failed loading, default to built-in
  loadingImg:"",//For images displayed during loading, the built-in css-icon is used by default
  lazy:false,//Whether to use lazy loading or not, the default is false, which requires the browser to support IntersectionObserver (ie does not support).
  alt:"",//The native Alt attribute of the img tag
  showSource:false,//Whether to display the source image as data-src, data-lowsrc,,the default is false (used when loading fails)
  center:false//Whether to center vertically or horizontally according to picture size,the default is false
}

```

#### result

![result image](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/vue-img.png)
