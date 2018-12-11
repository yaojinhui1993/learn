# EChart

记录 EChart 的使用。
基本上，每个 chart 都包含有： title, legend, tooltip, dataset, series 这几个值。

* title 用来配置 chart 的标题，包括标题文字，标题颜色等。
* legend 用来配置图例组件，展示不同系列的标记：颜色和名字。通过点击，可以控件哪些系列可以显示，哪些不能显示。
* tooltip 用来配置鼠标在 chart 上时给用户的提示。
* dataset, EChart4 新增的功能，配置 chart 的数据集。可以通过结合 xAxis, yAxis, series 的轴目配置，显示出想要的 chart 出来。代替了 EChart3 的 series.data 功能。
* series，系列列表，每个系列通过自己的 type 指定图表的类型。如 `type=bar` 则表示自己的图表是柱状图或条形图。

以上是所有图形通用的属性。

## 使用 `vue-echarts` 来在 Vue 使用 EChart

用法很简单，有全局安装和局部安装两种做法。全局安装的思路就是导入 `vue-echarts` 后，将其

## Bar

柱状图，条形图。

1. 主体部分就是 data，如何将数据显示到柱状态图上呢？
    首先定义好 title, legend, tooltip 等基本配置，然后将 data 放入到 `dataset.source` 属性上，并指定 `xAxis`, `yAxis` 的值，声明 x 轴和 y 轴的类型 (类目轴、数值轴等), 默认情况下， EChart 数将 data 的第一列当作 x 轴上的每个类目；将第一行当作一个系列，在 `series` 属性中，每定一个系列，就对应 data 中的每一列。
    也就是说，要想显示柱状图或条形图，只要设置好 `dataset.source` 和 `xAxis`, `yAxis`, `series` 的值，图形就能够显示出来。

2. 要导入的什么包？
    1. EChart, `vue-echarts/components/ECharts`;
    2. bar, `echarts/lib/chart/bar`;
    3. 功能组件，如 `echarts/lib/component` 下的 `tooltip`, `title`, `dataset` 等

## polar

极坐标图。对于此图，我目前还不知道其用图是什么，但还是分析下代码中的内容。

1. 如何才能显示一个极坐标图？
    首先，设置 `title`, `tooltip`, `legend` 等基本属性。接着，可以通过 `polar` 属性设置下图形相对于极坐标系下的位置设定。可以通过 `tooltip.axisPointer.type=cross` 来让 EChart 自动设置显示哪个轴的 axisPointer。 通过设置 `angleAxis` 来设置极坐标系的角度轴，可以通过 `radiusPointer` 的值来设置径向轴。最后设置下 series 的值，通过 `series.coordinateSystem=polar` 来声明该系列使用的坐标系是极坐标系，通过 `series.type` 设置为 line 来说明该系列是折线图，传入 data 到 `series.data` 属性，就能够获取到该系列的图形了。如果想要动画的话，可以通过 `animationDuration` 属性值来设置动画时间。
2. 需要导入的组件有哪些？
    `vue-echarts/components/ECharts`, `echarts/lib/chart/line`, `echarts/lib/components/polar`

## pie

饼图。

1. 如何才能显示一个饼图？
    首先，先设置 `title`, `tooltip`, `legend` 等基本属性，然后通过 `series` 的值，传入 data 到 `series[0].data` 就可以将饼图给显示出来了。饼图在 `series[0].data` 中的格式为： `{name: lorem, value: ipsem}` 。如果想要设置 series 的样式的话，可以设置 `series[0].itemStyle` 的值。
2. 如何注册一个 theme?
    首先，准备一个 theme, 具体的格式可以查官网。然后将该 theme 通过 `ECharts.registerTheme('theme-name', theme)` 的形式进行注册。最后，在 `v-echart` 的 theme 属性上声明使用 `theme-name` 后，该图形就引入该 theme 了。
3. 要导入哪些组件？
    `vue-echarts/components/ECharts`, `echarts/lib/chart/pie`, `echarts/lib/components/tooltip`, `echarts/lib/components/legend`, `echarts/lib/components/title`

## scatter

散点图。目前不知道有什么用，看不懂，就先不分析了。

## map

地图。这个是主要要用到的图，我要深入学习下。

1. 如何才能够显示一张地图？
    首先，要从外部导入中国地图文件，并使用 `ECharts.registerMap('china', chinaMap)` 的形式，将其注册到 ECharts。然后，设置 `backgroundColor`, `title`, `legend`, `tooltip` 等通用的值。然后通过设置 `geo` 的属性值，将 `geo.map` 设置为 `china`, 最后在 series 中，设置系列值，将 `series[0].coordinateSystem` 设置为 `geo`, 说明该系列的使用的坐标系 为地理坐标系，然后设置 `series[0].data` 的值，就能够将想要的数据显示到地图上了。需要注意的是，当 `series.type=scatter` 时，要想在地图上在相应的位置显示想应的散点，传入的 data 格式为： `{name: 'example_area', vale: [longitude, latitude]}` 。通过 `series[0].hoverAnimation` 这个属性，当鼠标放在 series 上时，会产生动画。

2. 需要导入的组件有哪些？
    `vue-echarts/components/ECharts`, `echarts/lib/chart/map`, `echarts/lib/chart/scatter`, `echarts/lib/components/geo`, `echarts/lib/components/tooltip`, `echarts/lib/components/legend`, `echarts/lib/components/title`

## 整体学习 ECharts

通过看官方文档，和实际操作，系统性的学习下 ECharts

### 在 webpack 中使用 ECharts

1. 全局引用 ECharts

    `import echarts from echarts`

2. 局部引用 ECharts

    1. 引入 ECharts 主模块: `import echarts from 'ECharts/lib/echarts'`
    2. 引入图: `import 'echarts/lib/chart/bar'`
    3. 引件组件: `import 'echarts/lib/component/title'`

### 个性化图表的样式

ECharts 能够从「全局」、「系列」、「数据」三个层次设置数据图形的样式。

`itemStyle` 可以设置一些通用的样式：阴影、透明度、颜色、边框颜色、边框宽度等。 「emphasis」属性是鼠标 hover 时的高亮样式。

`backgroundColor` 设置全局的背景色。

`textStyle` 设置文本的样式。

`labelLine.lineStyle` 设置标签的视觉引导线样式。标签的视觉引导线在饼图中存在。

`visualMap` 组件可以根据数据的数值显示每个图形的样式。

### ECharts 中的样式简介

颜色主题(Theme)：更改全局样式

调用盘，可以设置全局的调色篇盘或着系列自己的调色盘。该调色盘设置的项会被自动的选择一种颜色。

直接设置样式：在 itemStyle, lineStyle, areaStyle, label 等地方，设置图形元素的颜色、线宽、点的大小、标签的文字、标签的大小等。

高亮的样式，通过 emphasis 直接设置，其结构与普通样式相同。

`visualMap` 组件，能指定数据到颜色、图形尺寸的映射规则。

### 异步数据加载和更新

如何异步加载数据后，再显示 chart？

1. 获取完数据之后，再通过 `setOption` 填入数据和配置项。
2. 先设置其它样式，显示一个空的坐标系，然后获取数据后填入数据。需要注意的是, ECharts 在更新数据时，要通过 `series.name` 属性对应的相应的系列，所以推荐更新数据时加上系列的 name 值。可以通过 `myChart.showLoading()` 和 `myChart.hideLoading()` 属性来显示一个默认的加载动画。

数据的动态更新：很简单，由于 ECharts 是数据驱动，所以只要实时的改变数据，并通过 `setOption` 填入数据， ECharts 就可以通过动画来表现两组数据的变化。

### 使用 dataset 管理数据

`dataset` 组件，用于单独的数据集声明。从而数据可以被单独的管理，被多个组件复用。并且可以基于数据，指定数据到视觉的映射。

数据到图形的映射：

1. 通过 `series.seriesLayoutBy` 属性指定 dataset 为 column 或 row。
2. 指定维度映射的规则：使用「series.encode」将 dataset 的维度映射到坐标轴、 tooltip、 label、visualMap 等。

数据项: item。 维度： dimension