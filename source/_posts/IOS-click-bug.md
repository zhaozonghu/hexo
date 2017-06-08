---
title: iOS Safari 中click点击事件失效的解决办法
categories: javascript
tags: [web,js]
---
###### 问题描述

当使用委托给一个元素添加click事件时，如果事件是委托到 document 或 body 上，并且委托的元素是默认不可点击的（如 div, span 等），此时 click 事件会失效。

###### 解决办法

解决办法有 4 种可供选择：

​1.将 click 事件直接绑定到目标​元素（​​即 .target）上

2.将目标​元素换成<a>或者button等可点击的​元素

3.​将click事件委托到​​​​​非document或body的​​父级元素上

4.​给​目标元素加一条样式规则cursor: pointer;

​推荐后两种。从解决办法来看，​推测在 safari 中，不可点击的元素的点击事件不会冒泡到父级元素。通过添加 cursor: pointer 使得元素变成了可点击的了。

