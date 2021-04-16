### 请说明HTML布局元素的分类有哪些？描述每种布局元素的应用场景



**一、分类**

**1）内联元素：**

```html
span	a	b	strong	i	em	br	input	textarea
```

本身属性为display:inline；

和其他行内元素从左到右在一行显示，不可以直接控制宽度、高度等其他相关css属性，但是可以直接设置内外边距的左右值

宽高是由本身内容大小决定的（文字、图片等）

只能收纳文本或者其他行内元素，不能嵌套块级元素

**2）块状元素：**

```
div	h1-h6	hr	menu	ol	ul	li	dl	table	p	form
```

本身属性为display:block;

独占一行，每一个块级元素都会从新的一行重新开始，从上到下排布，可以直接控制宽度、高度等其他相关css属性，例如（padding，margin）

在不设置宽度的情况下，块级元素的宽度是它父级元素内容的宽度

在不设置高度的情况下，块级元素的高度是它本身内容的告诉

**3）内联块状元素**

内联块状元素综合了前两种的特性却又各有取舍

不自动换行，能够识别width和height，line-height，padding，margin

默认排列方式从左到右

**二、应用场景**

- 内联元素：用于不指定宽高，宽高由内容指定；

- 块状元素：用于指定宽高，标签占满一行；

- 内联块状元素：用于指定元素宽高，不占满一行；



### HTML标签b和strong的区别

**一、b和strong的区别**

两者虽然在网页中显示效果一样，但实际目的不同。

<b>这个标签对应bold，即文本加粗，其目的仅仅是为了加粗显示文本，是一种样式/风格需求；

<strong>这个标签意思是加强，表示该文本比较重要，提醒读者/终端注意。为了达到这个目的，浏览器等终端将其加粗显示；

<b>为了加粗而加粗，<strong>为了标明重点而加粗。





### 说一下减少dom数量的办法？一次性给你大量的dom怎么优化？

**一、减少DOM数量的方法**

1.可以使用伪元素，阴影实现的内容尽量不使用DOM实现，如清除浮动、样式实现等；

2.按需加载，减少不必要的渲染；

3.结构合理，语义化标签；

**二、大量DOM时的优化**

当对DOM元素进行一系列操作时，对DOM进行访问和修改DOM引起的重绘和重排都比较消耗性能，所以关于操作DOM，应该从以下几点出发：

**1.缓存DOM对象**

首先不管在什么场景下。操作DOM一般首先会去访问DOM，尤其是像循环遍历这种时间复杂度可能会比较高的操作。那么可以在循环之前就将主节点，不必循环的DOM节点先获取到，那么在循环里就可以直接引用，而不必去重新查询。

```javascript
let rootItem = document.querySelector('#app');
let childList = rootItem.child;	// 假设全是dom节点
for(let i=0;i<chileList.len;i++){
    
}
```

2.文档片段

3.用innerHtml代替高频的appendChild





### HTML5有哪些新特性？如何处理HTML5新标签的浏览器兼容问题？如何区分HTML和HTML5？

**一、HTML5新特性**

1.拖拽释放（Drag and drop）API

2.语义化更好的标签内容（header、nav、footer、aside、article、section）

3.音频、视频API（audio、video）

4.画布（Canvas）API

5.地理（Geolocation）API

6.本地离线存储localStorage长期存储数据，浏览器关闭后数据不会丢失

7.sessionStorage的数据在浏览器关闭后自动删除

8.表单控件，calendar、date、time、email、url、search

9.新的技术webworker、websocket、Geolocation

**二、HTML5兼容问题处理**

封装好的js库——html5shiv.js

**三、如何区分HTML和HTML5**

1.文档类型声明

- HTML声明：

- HTML5声明：<!doctype html>

2.结构语义























