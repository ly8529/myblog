---
title: css 盒模型
---
# 基本概念
css盒模型 分为标准模型和IE模型
* IE模型 width=content+border+padding,高度同理
* 标准模型 width=content

# css如何设置这两种模型
```
box-sizing：content-box  //标准模型
box-sizing：border-box  //IE模型
```

# js如何获取盒模型对应的宽和高
```
dom.style.width
dom.currentStyle.width
window.getComputerStyle(dom).width
dom.getBoundingClientRect().width
```


# 根据盒模型解释边距重叠


