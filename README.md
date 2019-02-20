[中文文档](README_zh.md)

#### @helloyoucan/vue-img

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



对img标签进行一层封装，主要有三个特点

> 懒加载
>
> 图片的loading状态
>
> 加载失败后的处理



##### props

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

##### 全局设置（非必要，有内置）

```javascript
Vue.prototype.$IMG = {
  errorImg:"", //Replace images after failed loading, default to built-in
  loadingImg:"",//For images displayed during loading, the built-in css-icon is used by default
  lazy:true,//Whether to use lazy loading or not, the default is true, which requires the browser to support IntersectionObserver (ie does not support).
  alt:"",//The native Alt attribute of the img tag
  showSource:true//Whether to display the source image as data-src, data-lowsrc (used when loading fails)
}

```

##### 效果

![效果图](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANUAAAJGCAYAAADMLcBHAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAA6WSURBVHhe7d1Jj5zlesbxfLQoqwy7RIqUYRElm+yyyufIGjMfG2IwQxiFIYjJzDaDA5IxIBAQwEwhR5kU5eRJP9048XFu2/30fYmudv1+0iUd2V11ukv1p7rL7/v2bwwgSlQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEtcF+/I8x3rs0xjMfj3HqvTF+8c7e7t/53898tPN334zxy52PYbOIagP907+P8drnYxw7O8Zfnx7jTx4Y43ePj/Fbd+7t906M8ac7fzb/7tibY7y687HzNmwGUW2Yz34c4/jOq9FfPjrGb/9ijN+84/r7nZ2PmR87X8E+/+ef7oRDJaoNMl9t7n57jD84WQd0vc3bzBi9Yh0+UW2Q7/9tL4w/vO+KWP5279u8v3lljNvO7W3+7/lnv7/zd5c/7o9O7f2s9aOoDp2oNsyXvxzj9p1w/uyhMf7qiTFOnt97s2K+Av3Xr/b2w058889O/sPOxzw5xp8/vPft36V/+elOOFSi2kAzrL//eIyz/zjGv/7nT39YmH93budjXvhkjK8FtTFEtaF+9d97u5H9fhw/H1FBmKggTFQQJioIExWEiQrCRAVhooIwUW0o//h7dIlqA81j+F74dO8QpBsdpjQPZZqHNM1Dm9gMotowM6h5pPpf/N3ewbLzoNl58Ow8iPbyAbXz4NrdA2rP7x10Ow++nQfhCmsziGqDzNM25mnzf3zq/07pmKd3XOvUj3layOWPm6eLzBjn6SMcLlFtkPkKNMM46EmK8wRHJykePlFtmHlK/Dw3ap4iP0+VrwK6cvOU+/mxM8Z5Kj6HT1QbaL7azIu5zIu6zG/z5kVe5sVeLl/4ZV4EZl4MZv7dvDjMvEiMV6jNIaoNNi8/Ni9DNi9HNk+Vv3yJsvlz17xs2XyzYl7GjM0iKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhN4zqhx9+MLNrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8YqojJrrCIqs8Yqomru4sWL4/Tp0+PEiRPj2LFj45Zbbjnyu+OOO8aDDz44XnvttXHp0qXy67a9VUR1wH311VfjscceK5+UN9PuvPPO8c4775SPgYkqti+++GL3lal6Et6sO3PmTPlYbPsqolrct99+O06ePFk+8W72ecX6/6uIanEvv/xy+YTbhs1vBf2M9euriGph33333e4Tq3rCbcvmmxfVY7Otq4hqYRcuXCifaNu0+a5g9dhs6yqiWtj8r3T1RNumzbfbq8dmW1cR1cJeeuml8om2basem21dRVQLE9XeqsdmW1cR1cJEtbfqsdnWVUS1sE5Ud99993jkkUfGww8/PG6//fbyY47KqsdmW1cR1cIOEtX8wf7tt98e33///f/ezzfffDNefPHF8uOPwq58TLZ9FVEtbDWqeYDthx9+WN7X3KuvvlrebtNXfS3buoqoFrYa1dNPP13ez+XNV6977rmnvO0mr/patnUVUS1sNar33nuvvJ8r99xzz5W33eRVX8e2riKqha1GNc+1qu7nyh3GsYTHjx8fTzzxxHjhhRfGK6+8svs53HvvveXHVqu+jm1dRVQLW43qrbfeKu/nyj311FPlbdO79dZbx7PPPjs++eST8vOYb57s91vR6vbbuoqoFrYa1QMPPPBr7/pdvXmi48/x9vo8Xu/zzz8vP4crt993JKvbbusqolrYalRz8zbVfc3zsua/W1W3Se7JJ5/cPbq++hyu3vxWsLqPq1fddltXEdXCDhLV3Dzt/oMPPtj9Fmu+Op0/f/5nOdFx/kPzfoOaE9X6KqJa2EGjOozNf3T+8ssvy6/jWtvvUfjVbbd1FVEt7ChF9frrr5dfw/UmqvVVRLWwoxLVPM5w/sxWfQ3X2wyxur+rV912W1cR1cKOSlTzXbzq87/R3nzzzfL+rl51221dRVQLOypRffbZZ+Xnf6OJan0VUS3sKER16tSp8nPfz86ePVve59Wrbrutq4hqYUchqvm2ePW572fzCJDqPq9eddttXUVUCzsKUR30W785Ua2vIqqFbXpU999/f/l573eiWl9FVAtLRzWPDZzv1M0n87yk8nyj4Pnnnx+PPvrouOuuu8rbXG/z9tXnvd+Jan0VUS0sFdU87eLTTz8t/z8ubx6IO48on2cHz8ON5lHm1X1d3jx14yD/NnXlRLW+iqgW1o1qHpH+7rvvlvd9o83jBt9///3dV7bHH39891XuvvvuGw899NB45plndn8TSXW7lYlqfRVRLawT1bwG+8cff1ze76ZMVOuriGphB43qtttuGx999FF5n5s0Ua2vIqqFHTSqo/J7nUS1voqoFnaQqObp8tV9beJEtb6KqBa2GtV8W/zrr78u72sTJ6r1VUS1sNWozp07V97Ppk5U66uIamErUc0rE13voi+bOFGtryKqha1ENZ+g1X1s8kS1voqoFrbfqOaZtysXXNmUiWp9FVEtbL9RzSu/Vrff9IlqfRVRLWy/UV3rKrCbPlGtryKqhe03qqP4rd+c0+nXVxHVwvYb1bxYZnX7Td58p3IeDV99PVevuv22riKqhe03qnms3/y5ah6etLL57ddh7I033tg92r36WqpVj822riKqhe03qpt91WOzrauIamH7vYLrzbx5OenqsdnWVUS1sAsXLpRPtG3a/LU81WOzrauIamHzXb15smH1ZNuWzVfr6rHZ1lVEtbjD+HWim7L5H5RLly6Vj8u2riKqxc2Lq/wcv1tqEzffoawek21eRVQH2LzIyokTJ8on3s26M2fOlI/Ftq8iqgNu/kbE+RsSqyfgzbT5LZ9XqGuvIqrmLl68OE6fPr37ynXs2LHyiXnUNt82n+/yzTcl/Ax1/VVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVEZdZYRVRmjVVuGBWwRlQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCsJEBWGigjBRQZioIExUECYqCBMVhIkKwkQFYaKCMFFBmKggTFQQJioIExWEiQrCRAVhooIwUUGYqCBMVBAmKggTFYSJCqLG+B9+zL8JxDWGCQAAAABJRU5ErkJggg==)
