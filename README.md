[中文文档](README_zh.md)

### @helloyoucan/vue-img

Use by npm

> npm install @helloyoucan/vue-img

vue version：2.0+

```javascript
import vImg from '@helloyoucan/vue-img'

Vue.prototype.$IMG = {
  errorImg:"", //Replace images after failed loading, default to built-in
  loadingImg:"",//For images displayed during loading, the built-in css-icon is used by default
  lazy:true,//Whether to use lazy loading or not, the default is true, which requires the browser to support IntersectionObserver (ie does not support).
  alt:"",//The native Alt attribute of the img tag
  showSource:true//Whether to display the source image as data-src, data-lowsrc (used when loading fails)
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
    }
  }
```

#### Global Settings (not required, built-in)

```javascript
Vue.prototype.$IMG = {
  errorImg:"", //Replace images after failed loading, default to built-in
  loadingImg:"",//For images displayed during loading, the built-in css-icon is used by default
  lazy:true,//Whether to use lazy loading or not, the default is true, which requires the browser to support IntersectionObserver (ie does not support).
  alt:"",//The native Alt attribute of the img tag
  showSource:true//Whether to display the source image as data-src, data-lowsrc (used when loading fails)
}

```

#### result

![result image](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/vue-img.png)
