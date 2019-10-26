---
title: go.js插件使用总结
date: 2019-10-26 20:01:10
tags: 
- go.js
- 可视化 
---

# 最近工作项目中用到了go.js的流程图插件，各种设置参数还不是很明白，故在此作一个小结

## go.js是一个使用canvas绘制流程图的js插件。

[Layout](https://gojs.net/latest/intro/layouts.html)
流程的布局方式有很多种：
GridLayout
网格布局，整个画布被分为无数个单元格，节点默认在小格子内
-  TreeLayout
树状布局，类似二叉树的那样一层层往下
ForceDirectedLayout
力导向布局
LayeredDigraphLayout
分层有向图布局
CircularLayout
循环布局，节点之前可能头尾相连

[GraphObject](https://gojs.net/latest/api/symbols/GraphObject.html)
有图表对象才能构成一个图表

Panel
面板将其他GraphObjects作为其元素。面板的元素按它们在元素集合中出现的顺序绘制。
Shape
图形单元，绘制出自己想要的图形
TextBlock
文本域块，定义之后可以显示文字
Picture
可以展示图片或者是视频帧
Placeholder
没有使用过


[Panel](https://gojs.net/latest/api/symbols/Panel.html)
面板中就是着重要讲的节点了，它也分为好多种布局方式，这里的布局是节点内的元素布局，每个Panel都有一个类型并建立自己的坐标系。Panel的类型决定了它的大小和排列：
Position => 定位方式排列元素，每个元素都有自己的坐标
Vertical/Horizontal => 水平对齐或者垂直对齐排列
Table => 元素在表格单元格内依次排列
| 标题  | :tw-1f1fd:  |
| ------------ | ------------ |
| ---内容-- |
也可以对行列进行跨行或跨列的定义
Grid => 依据网格线对元素进行排列
Auto => 不对元素排列方式做限制，元素以默认从上倒下的方式排列
Link => 线条是一种特殊的面板，这个之后再介绍
其他没有说明的，在项目中没有实际用到，需要的时候可以自行查询API

[Part](https://gojs.net/latest/api/symbols/Part.html)
part继承自panel，所以其他GraphObject的其他元素一般都在这里展示。

接下来的Node和Part类似，所以不过多介绍。但是需要讲一下节点模板`NodeTemplateMap`，模板是定义了一种类节点，想创建节点的时候，只需要通过调用对应类名的节点模板，就可以在画布上看到节点。

Diagram
画布作为节点的容器，框架对其做了较为严格的限制，通过绑定id，GoJS就可以初始化，定义画布。画布主要分为两部分，一是节点，二是连线。节点即上面所说节点模板，连线上面也提到过，是一种特殊的节点，`linkTemplate`是用来定义连线模板的，里面可以对连线进行一些设置，比如连线方式，转角角度，曲度以及颜色等属性。

