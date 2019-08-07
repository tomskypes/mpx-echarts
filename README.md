# MPX-Echart
mpx-chart适用于MPX小程序框架中，旨在提供连接小程序与echarts的中间组件。

# Getting Started

### Install
```bash
npm install mpx-echarts -S
```

### Usage

注意： 本组件默认提供了echarts文件，此文件为比较完整的图表集合，下面的示例也针对使用默认提供的图表。如果希望仅引入部分组件，请参考高阶使用部分。

首先在app.mpx中作为mpx插件进行注册

```vue
<script>
  import mpx from '@mpxjs/core'
  import apiProxy from '@mpxjs/api-proxy'
  import mpxEcharts from 'mpx-echarts'

  mpx.use(apiProxy, { usePromise: true }).use(mpxEcharts)
  // app.js
  App({})
</script>
```

在 页面/组件 中使用
```vue
<template>
  <view class="bar-container">
    <ec-canvas id="mychart-dom-bar" canvasId="mychart-bar" ec="{{ ec }}"></ec-canvas>
  </view>
</template>

<script>
  import mpx, { createComponent } from '@mpxjs/core'

  let chart = null

  createComponent({
    data: {
      ec: {
        onInit (canvas, width, height) {
          chart = mpx.echarts.init(canvas, null, {
            width: width,
            height: height
          })
          canvas.setChart(chart)

          var option = {
            xAxis: {
              type: 'category',
              data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
            },
            yAxis: {
              type: 'value'
            },
            series: [{
              data: [820, 932, 901, 934, 1290, 1330, 1320],
              type: 'line',
              smooth: true
            }]
          }

          chart.setOption(option)
          return chart
        }
      }
    }
  })
</script>

<style lang="stylus" scoped>
  .bar-container
    width 100%
    height 300px
    background #fff
</style>

<script type="application/json">
  {
    "component": true,
    "usingComponents": {
      "ec-canvas": "mpx-echarts/src/ecCanvas"
    }
  }
</script>

```

### 高阶使用

如果希望仅引入部分组件，可以参考echarts官方文档[自定义echarts文件](https://echarts.baidu.com/tutorial.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%9E%84%E5%BB%BA%20ECharts)定制echarts文件。引入方式如下（也可以参考[本文demo](https://github.com/echaoo/mpx-echarts/tree/master/example)）：

在app.mpx文件中引入install文件并在注册时以参数的形式传入echarts：
```vue
<script>
   import mpx from '@mpxjs/core'
   import apiProxy from '@mpxjs/api-proxy'
   import echarts from './lib/echarts'
   import mpxEcharts from '../src/install'
  mpx.use(apiProxy, { usePromise: true }).use(mpxEcharts, { echarts })
  
  // app.js
  App({})
</script>
```
定制的echarts文件，可以是打包好的echarts文件，也可以用以下方式引入(参考[本文demo](https://github.com/echaoo/mpx-echarts/blob/master/example/lib/echarts.js))：
```js
// 引入 echarts 主模块。
export * from 'echarts/lib/echarts';
// 引入饼图。
import 'echarts/lib/chart/pie';
// 柱状图
import 'echarts/lib/chart/bar';

// 引入提示框组件、标题组件、工具箱组件。
import 'echarts/lib/component/tooltip';
import 'echarts/lib/component/title';
import 'echarts/lib/component/toolbox';
import 'echarts/lib/component/legend';
```
其在组件中的使用和上面的示例相同。


### FAQ
1、使用时报以下错误信息是为什么？
 ```
thirdScriptError
t.addEventListener is not a function;at SelectorQuery callback function
TypeError: t.addEventListener is not a function
```
答： 这是你使用的mpx版本问题，使用一下命令更新到最新版本就好啦
```bash
npm i @mpxjs/core@latest @mpxjs/webpack-plugin@latest
```


