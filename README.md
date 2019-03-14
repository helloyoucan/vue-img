[查看中文文档](README_zh.md)

### @helloyoucan/vue-img

Use by npm

> npm install @helloyoucan/vue-img

vue version：2.0+

```javascript
//main.js
import vImg from '@helloyoucan/vue-img'

Vue.prototype.$IMG = {
  lazy:true,
  center:true
}
Vue.component('v-img',vImg)

```
//APP.vue
```
<v-img :src="imgSrc @load="handleLoad" @error="handleError" />
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
	width:{
      type:String,
    },
    height:{
      type:String,
    },
    src: { // Image path
      type: String,
      default: '#'
    },
    alt: {// Native attributes of the img tag
      type: String
    },
    loadingImg: {// Loading a loading image of the image
      type: String
    },
    errorSrc: { // The image that was replaced when the image failed to load
      type: String,
      default: 'xxxx'
    },
    showSource: { // Whether to display the original image to the dom as a date-src attribute
      type: Boolean,
      default: false
    },
    lazy: {// Lazy loading
      type: Boolean,
      default: false
    },
    center: { // Whether to center vertically or horizontally according to picture size
      type: Boolean,
      default: false
    },
    ratio: {// When center===true, it is used to compare the difference between the original size of the image and the width/height of the rendering size, and then set the width or height of the image as the basis of auto
      type: Number,
      default: 0
    }
  }
```

#### Global Settings (not required, built-in)

```javascript
Vue.prototype.$IMG = {
  errorImg:"",
  loadingImg:"",
  lazy:false,
  showSource:false,
  center:false,
  ratio:0
}

```

#### result
![result image](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/0roocard.gif)
![result image](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/vue-img.png)
