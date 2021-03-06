---
author: jiguang
title: IE6/7 reflow bug
excerpt: >
  IE6/7在使用Javascript操作DOM之后经常出现的奇怪样式错误的一种解决办法。
layout: post
category:
  - JavaScript
tags:
  - bug
  - IE
  - reflow
post_format: [ ]
---
在调试IE6/7的时候，经常可以遇到这样一种bug，疑似reflow造成的，但具体根源没搞清楚，只是reflow的方法可以解决。

**症状：**

常常发生在JS修改数据或者DOM结构的部分，样式会出现错乱，包括定位错误，宽高错误，文字居中等问题；

仅在JS修改后才出现问题，一般页面载入时是好的。

**确诊：**

利用调试工具在JS修改结构的容器上修改任意属性为任意值，甚至空白均可修复此bug；

甚至有时候一打开调试工具的瞬间问题就解决了，无法定位到出问题的元素。

目前[笔者][1]还没有找到这个问题的根源，大致应该是一个浏览器渲染顺序的问题，JS修改数据的DOM部分的样式可能是它的父级容器或者祖先容器的几个样式叠加的结果。而IE6/7并没能重新计算改变的部分的样式与其祖先的关系，所以导致错误。

**解决方案：**

如果有精力的话，最好从样式本身入手，一般都会是一堆float和position相互作用的结果，但没有找到规律。

另外一种快速的解决方案是在JS修改DOM结构，或者填充数据之后加上这样一句代码：

    document.body.className = document.body.className;

这句代码可以强制reflow整个body部分，当然其中的body可以换成其他容器，但一定要包含被修改的部分。这个操作会对页面性能有一点影响，但是可以快速解决这个奇怪的bug。

如果大家有什么更好的解决办法或者有高人知其原理，欢迎留言赐教。

补充资料20120209：[《浏览器工作原理浅析》][2] from [CSS森林][3]

 [1]: http://jiguang.github.com "笔者"
 [2]: http://www.cssforest.org/blog/index.php?id=195
 [3]: http://www.cssforest.org/blog/