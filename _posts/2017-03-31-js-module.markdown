---
layout: post
title: JS模块化标准的总结归纳
date: 2017-03-31 16:43:24.000000000 +09:00
tags: javascript, module
---

#### `RequireJS` implements the AMD API `CommonJS`是一种规范 

以下是玉伯对这几种不同的模块化规范的解释

> 作者：玉伯
> 链接：https://www.zhihu.com/question/20351507/answer/14859415
> 来源：知乎

### AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。

### CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

类似的还有 CommonJS Modules/2.0 规范，是 BravoJS 在推广过程中对模块定义的规范化产出。
还有不少⋯⋯

这些规范的目的都是为了 JavaScript 的模块化开发，特别是在浏览器端的。
目前这些规范的实现都能达成浏览器端模块化开发的目的。

区别：

1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.

2. CMD 推崇依赖就近，AMD 推崇依赖前置。看代码：


```
// CMD
define(function(require, exports, module) {
var a = require('./a')
a.doSomething()
// 此处略去 100 行
var b = require('./b') // 依赖可以就近书写
b.doSomething()
// ...
})
```



```
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
a.doSomething()
// 此处略去 100 行
b.doSomething()
...
})
```


虽然 AMD 也支持 CMD 的写法，同时还支持将 require 作为依赖项传递，但 RequireJS 的作者默认是最喜欢上面的写法，也是官方文档里默认的模块定义写法。


3. AMD 的 API 默认是一个当多个用，CMD 的 API 严格区分，推崇职责单一。比如 AMD 里，require 分全局 require 和局部 require，都叫 require。CMD 里，没有全局 require，而是根据模块系统的完备性，提供 seajs.use 来实现模块系统的加载启动。CMD 里，每个 API 都简单纯粹。


4. 还有一些细节差异，具体看这个规范的定义就好，就不多说了。


### export vs module export
Module.exports才是真正的接口，exports只不过是它的一个辅助工具。　最终返回给调用的是Module.exports而不是exports。
所有的exports收集到的属性和方法，都赋值给了Module.exports。当然，这有个前提，就是Module.exports本身不具备任何属性和方法。如果，Module.exports已经具备一些属性和方法，那么exports收集来的信息将被忽略。


CMD代码规范这里useful info，因为这就是人家seats的官方说明文件啦

https://github.com/seajs/seajs/issues/242
除了给 exports 对象增加成员，还可以使用 return 直接向外提供接口。


```
define(function(require) {

// 通过 return 直接提供接口
return {
foo: 'bar',
doSomething: function() {}
};

});
exports Object
exports 是一个对象，用来向外提供模块接口。
define(function(require, exports) {

// 对外提供 foo 属性
exports.foo = 'bar';

// 对外提供 doSomething 方法
exports.doSomething = function() {};

});
```




> 另：一个做前端的女孩子的博客哦，http://www.cnblogs.com/skylar/p/4065455.html讲的也是蛮中肯的。
一个感觉很同好的程序员讲module.export 和 export的区别
http://weizhifeng.net/node-js-exports-vs-module-exports.html


你肯定对Node.js模块中用来创建函数的exports对象很熟悉（假设一个名为rocker.js的文件）：

```
exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

然后你在另一个文件中调用：

```
var rocker = require('./rocker.js');
rocker.name(); // 'My name is Lemmy Kilmister'
```

但是module.exports到底是个什么玩意儿? 它合法吗？
令人吃惊的是-module.exports是真实存在的东西。exports只是module.exports的辅助方法。你的模块最终返回module.exports给调用者，而不是exports。exports所做的事情是收集属性，如果module.exports当前没有任何属性的话，exports会把这些属性赋予module.exports。如果module.exports已经存在一些属性的话，那么exports中所用的东西都会被忽略。
把下面的内容放到rocker.js:

```
module.exports = 'ROCK IT!';
exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

然后把下面的内容放到另一个文件中，执行它：

```
var rocker = require('./rocker.js');
rocker.name(); // TypeError: Object ROCK IT! has no method 'name'
```

rocker模块完全忽略了exports.name，然后返回了一个字符串'ROCK IT!'。通过上面的例子，你可能认识到你的模块不一定非得是模块实例（module instances）。你的模块可以是任何合法的JavaScript对象 - boolean，number，date，JSON， string，function，array和其他。你的模块可以是任何你赋予module.exports的值。如果你没有明确的给module.exports设置任何值，那么exports中的属性会被赋给module.exports中，然后并返回它。

在下面的情况下，你的模块是一个类：

```
module.exports = function(name, age) {
    this.name = name;
    this.age = age;
    this.about = function() {
        console.log(this.name +' is '+ this.age +' years old');
    };
};
```

然后你应该这样使用它：

```
var Rocker = require('./rocker.js');
var r = new Rocker('Ozzy', 62);
r.about(); // Ozzy is 62 years old
```

在下面的情况下，你的模块是一个数组：

```
module.exports = ['Lemmy Kilmister', 'Ozzy Osbourne', 'Ronnie James Dio', 'Steven Tyler', 'Mick Jagger'];
```

然后你应该这样使用它：

```
var rocker = require('./rocker.js');
console.log('Rockin in heaven: ' + rocker[2]); //Rockin in heaven: Ronnie James Dio
```

现在你应该找到要点了 - 如果你想要你的模块成为一个特别的对象类型，那么使用module.exports；如果你希望你的模块成为一个传统的模块实例（module instance），使用exports。
把属性赋予`module.exports`的结果与把属性赋予给exports是一样的。看下面这个例子：

```
module.exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

下面这个做的是一样的事情：

```
exports.name = function() {
    console.log('My name is Lemmy Kilmister');
};
```

但是请注意，它们并不是一样的东西。就像我之前说的module.exports是真实存在的东西，exports只是它的辅助方法。话虽如此，exports还是推荐的对象，除非你想把你模块 的对象类型从传统的模块实例（module instance）修改为其他的。

“exports 和 module.exports 引用同一个对象”

> 以下内容大部分来源于Stack Overflow和Wikipedia

> CommonJS is a way of defining modules with the help of an exports object, that defines the module contents. Simply put, a CommonJS implementation might work like this:

```
// someModule.js
exports.doSomething = function() { return "foo"; };

//otherModule.js
var someModule = require('someModule'); // in the vein of node
exports.doSomethingElse = function() { return someModule.doSomething() + "bar"; };
```

Basically, CommonJS specifies that you need to have a require() function to fetch dependencies, an exports variable to export module contents and a module identifier (which describes the location of the module in question in relation to this module) that is used to require the dependencies. CommonJS has various implementations, including Node.js, which you mentioned.

CommonJS was not particularly designed with browsers in mind, so it doesn't fit in the browser environment very well (I really have no source for this--it just says so everywhere, including the RequireJS site.). Apparently, this has something to do with asynchronous loading, etc.


On the other hand, RequireJS implements AMD, which is designed to suit the browser environment. Apparently, AMD started as an spinoff of the CommonJS Transport format and evolved into its own module definition API. Hence the similarities between the two. The new feature in AMD is the define() -function that allows the module to declare its dependencies before being loaded. For example, the definition could be:

```
define('module/id/string', ['module', 'dependency', 'array'],
function(module, factory function) {
  return ModuleContents;
});
```

So, CommonJS and AMD are JavaScript module definition APIs that have different implementations, but both come from the same origins.
AMD is more suited for the browser, because it supports asynchronous loading of module dependencies.

### RequireJS is an implementation of AMD, while at the same time trying to keep the spirit of CommonJS (mainly in the module identifiers).

To confuse you even more, RequireJS, while being an AMD implementation, offers a CommonJS wrapper so CommonJS modules can almost directly be imported into use with RequireJS.

```
define(function(require, exports, module) {
  var someModule = require('someModule'); // in the vein of node
  exports.doSomethingElse = function() { return someModule.doSomething() + "bar"; };
});
```



AMD CMD commonjs requirejs……pageObject……
webpack
Question from stack overflow
Asynchronous module definition (AMD) and CommonJS are styles/specifications of writing modular code in javascript, while RequireJS and Browserify are 2 popular implementation/helper for them respectively.
That means AMD-requirejs Commonjs-Browserify


### RequireJS implements the **AMD(asynchronous module definitions )** API
http://requirejs.org/docs/whyamd.html
AMD

Asynchronous module definition (AMD)

```
define(function {
  console.log('Awesome');
});
```

CommonJS

```
module.exports = function(){
  console.log('Awesome');
}
```

In our MVC structure, the actual example:


```
//Outfit view in dejafm
var Slider = require('util/Slider'); var Tooltip = require('widget/Tooltip'); var BasicView = require('app/view/View'); var StyleTemplateView = require('app/view/StyleTemplateView'); var BasicModel = require('app/model/Model'); var StyleModel = require('app/model/StyleModel');

module.exports = new OutfitsView();

var Audios = {} module.exports = Audios;


//modules in deja3 index webpage
Util.js
exports.os = os; 
NavBar.js
exports.show = function show(){  el.removeAttr('hidden').removeClass('fadeOutUp').addClass('fadeInDown'); } exports.hide = function hide(){  el.removeClass('fadeInDown').addClass('fadeOutUp'); }
```

