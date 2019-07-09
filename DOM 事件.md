---
title: DOM 事件
---
# DOM事件
参考博客：https://www.jianshu.com/p/1eb41968c8e3
https://developer.mozilla.org/zh-CN/docs/Web/Events#标准事件

## 1、DOM 事件的级别
DOM0
```
element.onclick = function(){}
```
DOM2
```
element.addEventListener('click',function(){},false)
```
DOM3 
DOM3比DOM2多了一些事件类型，最后一个参数false表示事件在冒泡阶段触发，为true表示在捕获阶段触发。
注：IE只有冒泡阶段
```
element.addEventListener('keyup',function(){},false)
```

## 2、DOM 事件模型
捕获和冒泡，IE只有冒泡

## 3、DOM 事件流
捕获阶段-->目标元素阶段-->冒泡阶段

## 4、描述DOM 事件捕获的具体流程
点击页面上一个元素时，捕获流程为window-->document-->html-->body-->父元素-->子元素
冒泡过程则相反，由内层元素向外层元素传播


## 5、event对象的常见应用

* event.stopPropagation()
* event.preventDefault()
* event.currentTarget
* event.target
* event.stopImmediatePropagation()

## 6、自定义事件

```
var eve = new Event('custome');
ele.addEventLister('custome',function(){
  console.info('custome');
});
ele.dispatchEvent(eve);
```
todo:写个demo



