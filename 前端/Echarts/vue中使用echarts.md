# vue中使用echarts
## 导入echarts
在vue中可以使用组合函数按需的导入echarts组件

- eahcrts.ts
```ts
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import * as echarts from "echarts/core";
import {
  BarChart,
  // 系列类型的定义后缀都为 SeriesOption
  //   BarSeriesOption,
  //   LineChart,
  //   LineSeriesOption,
} from "echarts/charts";
import { LabelLayout, UniversalTransition } from "echarts/features";
import { CanvasRenderer } from "echarts/renderers";
import {
  TitleComponent,
  // 组件类型的定义后缀都为 ComponentOption
  TitleComponentOption,
  TooltipComponent,
  TooltipComponentOption,
  GridComponent,
  GridComponentOption,
  // 数据集组件
  DatasetComponent,
  DatasetComponentOption,
  // 内置数据转换器组件 (filter, sort)
  TransformComponent,
} from "echarts/components";
// 通过 ComposeOption 来组合出一个只有必须组件和图表的 Option 类型
type ECOption = echarts.ComposeOption<
  //   | BarSeriesOption
  //   | LineSeriesOption
  | TitleComponentOption
  | TooltipComponentOption
  | GridComponentOption
  | DatasetComponentOption
>;
// 注册必须的组件
echarts.use([
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  BarChart,
  LabelLayout,
  UniversalTransition,
  CanvasRenderer,
]);

//导出
export function useEchartsBar(el: HTMLElement) {
  const myEcharts = echarts.init(el);

  myEcharts.setOption({
    xAxis: {
      data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
    },
    yAxis: {},
    series: [
      {
        type: "bar",
        data: [23, 24, 18, 25, 27, 28, 25],
      },
    ],
  });
}```

- app.vue中使用
```ts
<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useEchartsBar } from "./utils/echart";
const main = ref<HTMLElement | null>(null);
onMounted(() => {
  if (!main.value) return;
  useEchartsBar(main.value);
});
</script>
<template>
  <div class="main" ref="main"></div>
</template>
<style lang="scss">
.main {
  width: 50%;
  height: 30rem;
}
</style>
```

## echarts初始化
- echarts初始化使用 `init` 方法
在使用人echarts表单时，需要给顶一个容器，并设置出事大小
也可以在echrats初始化时，设置初始大小
```ts
var myChart = echarts.init(document.getElementById('main'), null, {
    width: 600,
    height: 400
  });
```

## echarts的响应式
在有些场景下，我们希望当容器大小改变时，图表的大小也相应地改变，种情况下，可以监听页面的 `window.onresize` 事件获取浏览器大小改变的事件，然后调用echartsInstance.resize 改变图表的大小
```ts
var myChart = echarts.init(document.getElementById('main'));
  window.onresize = function() {
    myChart.resize();
  };
```

## echarts的销毁与重建
假设页面中存在多个标签页，每个标签页都包含一些图表。当选中一个标签页的时候，其他标签页的内容在 DOM 中被移除了。这样，当用户再选中这些标签页的时候，就会发现图表“不见”了。

本质上，这是由于图表的容器节点被移除导致的。即使之后该节点被重新添加，图表所在的节点也已经不存在了。

正确的做法是，在图表容器被销毁之后，调用 [`echartsInstance.dispose`](https://echarts.apache.org/api.html#echartsInstance.dispose) 销毁实例，在图表容器重新被添加后再次调用 [echarts.init](https://echarts.apache.org//api.html#echarts.init) 初始化。

## series
接收一个对象数组，每个对象为一个类别中的一条数据，含有多个对象则代表每个类别中含有多条数据
```js
		 {xAxis: {
		      data: ["星期一", "星期二", "星期三", "星期四", "星期五", "星期六", "星期日"],
	    },	
		 //y轴数据
	    yAxis: {},       
        data: [23, 24, 18, 25, 27, 28, 25],
        // echarts 表单的颜色
        color: ["#759aa0"],
        itemStyle: {
          //阴影
          shadowBlur: 10,
          shadowColor: "rgba(0, 0, 0, 0.5)",
        },
 ```
- xAxis: 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
- yAxis: 声明一个 Y 轴，数值轴。
- series:  声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
	- data: bar 数据
