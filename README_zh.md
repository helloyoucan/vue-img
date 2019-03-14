### @helloyoucan/vue-img

通过npm使用

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
      type: String
    },
    loadingImg: {// 加载图片的loading图片
      type: String
    },
    errorSrc: { // 图片加载失败时代替的图片
      type: String,
      default: 'xxx'
    },
    showSource: { // 是否以date-src的属性显示原图片到dom上
      type: Boolean,
      default: false
    },
    lazy: {// 懒加载
      type: Boolean,
      default: false
    },
    center: { // 是否根据图片尺寸垂直居中或者水平居中
      type: Boolean,
      default: false
    },
    ratio: {// 比例差系数，用于center===true时，用于图片原生尺寸和渲染尺寸的width/height比例的差值对比然后去设置图片的width或者height为auto的依据
      type: Number,
      default: 0
    }
  }
```

#### 全局设置（非必要，有内置）

```javascript
Vue.prototype.$IMG = {
  errorImg:"", 
  loadingImg:"",
  lazy:true,
  alt:"",
  showSource:false,
  center: false
}

```

#### 效果

![result image](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/0roocard.gif)
![result image](https://github-1252517012.cos.ap-guangzhou.myqcloud.com/vue-img/vue-img.png)