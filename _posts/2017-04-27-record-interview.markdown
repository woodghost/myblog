---
layout: post
title: 面试内容总结
date: 2017-04-01 16:43:24.000000000 +09:00
tags: javascript
---

今天是神奇的一天，第一次知道这里居然真的有人看，原本以为只有自己会看。。。
整理回忆一下今天面试的内容，总结学习也可以方便其他求职的小伙伴.

在网上搜了搜，果然有很多360奇舞团的面试题啊，原来颜海镜也去面试过……我用过他fullpage.js的插件。。。。

题目大抵是没有变的，考的都是基础知识，js主要考察 function context scope

最后舔着个熊脸说一句，能有幸参加个一面也已经很荣幸了哈哈


下面代码的输出值是？

```
alert(1&&2); //2
```
后来又被问了 一个浮点数和0位运算的问题   `1.2345 | 0` 这样

2.正则表达式匹配，开头为010,01N, 02N或0NNN，后面是-7-8个数字的电话号码。
印象中我是这么写的
```
/^0[1,2]?0?N{1,3}-\d{7,8}$/
```

3.写出下面代码的输出值：

```
var obj = {
    a: 10,
    b: function () {console.log(this.a *= 2)};
};

var a = 1;
var objb = obj.b;

obj.b();//20  obj里面的a变成20
objb();//2     window的a变成2
obj.b.call(window);//4
```


4.写出下列代码的输出值：

```
function A() {

}
function B(a) {
    this.a = a;
}
function C(a) {
    if (a) {
        thia.a = a;
    }
}

A.prototype.a = 1;
B.prototype.a = 1;
C.prototype.a = 1;

console.log(new A());
console.log(new B());
console.log(new C(2));
```


5.写出下列代码的输出值：

```
var a = 1;
function b() {
    var a = 2;
    function c() {
        console.log(a);
    }

    return c;
}

b()();
```

### HTML&CSS

1.写出下列代码在各个浏览器中的颜色值?

```
background: red;
_background: green;
*background: blue;
background: black\9;


    color:blue; /*所有浏览器*/
    color:red\9; /*IE8以及以下版本浏览器*/
    *color:green; /*IE7及其以下版本浏览器*/
    _color:purple; /*IE6浏览器*/



```


2.添加些css让其水平垂直居中。

```
<div style="____________________________">

</div>


height: 200px;/*元素的高度*/
position: absolute;
top: 50%;  /*元素的顶部边界离父元素的的位置是父元素高度的一半*/
margin-top: -50px;  //  height/2, width/2    *margin, padding是相对于父元素的

```

3.如下代码，在空白处填写代码，是其点击时，前景色为白色，背景色为黑色。

```
<div onclick="_________________">ghost</div>

<div onclick="this.style.color=&quot;white&quot;;this.style.background='black';">ghost</div>
主要考察两点：这个动态函数中的this是元素本身，另外代码中不能包含非转义字符，可以使用单引号或者使用&quot;转义.
```

4. 选择器的优先级： 会显示哪个颜色
优先级id > class > tag
要是想取得最高优先级: ! important

5. 写三个块占满整行必须是正方形， width: 33.3%; padding-top: 33.3%    *margin, padding是相对于父元素的
这个我有点不太确定了，明天试一下。

.书写代码，点击时从1分钟开始，每秒递减到0。

<div onclick="test();">颜海镜</div>
.简述在IE下mouseover和mouseenter的区别？
