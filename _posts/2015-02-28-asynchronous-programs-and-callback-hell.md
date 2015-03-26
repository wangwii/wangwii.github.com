---
layout: post
title: "异步编程及回调函数地狱"
tagline: "Promises/A and Generators"
comments: false
category: "Note"
tags: [javascript]
---
{% include JB/setup %}

这事得先从Continuation passing style[CSP](http://en.wikipedia.org/wiki/Continuation-passing_style)说起，
它是函数式编程中的概念，也是一种编程风格，引用[InfoQ](http://www.infoq.com/cn/articles/nodejs-callback-hell)：

> 这种风格的函数要有额外的参数：“后续逻辑体”（也就是[Callback](http://en.wikipedia.org/wiki/Callback_(computer_programming))），
> 比如带一个参数的函数，CPS函数计算出结果值后并不是直接返回，
> 而是调用那个后续逻辑函数，并把这个结果作为它的参数。从而实现计算结果在逻辑步骤之间的传递，以及逻辑的延续。
> 也就是说如果要调用CPS函数，调用方函数要提供一个后续逻辑函数来接收CPS函数的“返回”值。

CSP这种风格很好的应对了异步编程，特别是像[Node.js](http://nodejs.org/)异步API司空常见，在带来性能提升的同时，也带来了新的问题。
问题之一就是 [Callback hell](http://callbackhell.com/)，简单说就是深度嵌套的回调函数，例如：

{% highlight javascript linenos %}
getData(function(x){
    var mydata = processData(x);
    getMoreData(mydata, function(y){
        var mydata2 = processData(y);
        getMoreDataAgain(mydata2, function(z){ 
            ....
        });
    });
});
{% endhighlight %}

幸运的是社区已经找到了解决这一问题的有效方法，流程控制工具、[Promises](https://www.promisejs.org/) 以及 
[JavaScript Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)。

####流程控制工具 
[Nimble](http://caolan.github.io/nimble/)是流程控制工具的典型代表，它提供了
[Parallel]
/[Series](http://blog.rajatpandit.com/2014/02/23/serial-control-flow-in-node-js/) 
方法对用来编排你的函数调用。还有提供类似功能的library例如：[fastsync](https://www.npmjs.com/package/fastsync)

#### Promises
[Promises](http://wiki.commonjs.org/wiki/Promises) 是一种异步编程模式
当前最广为使用的是 [Promises/A](http://wiki.commonjs.org/wiki/Promises/A) 和
[Promises/A+](https://promisesaplus.com/)【[中文翻译](http://www.ituring.com.cn/article/66566)】
貌似还有其它及个版本：[B](http://wiki.commonjs.org/wiki/Promises/B), 
[D](http://wiki.commonjs.org/wiki/Promises/D), 
[KISS](http://wiki.commonjs.org/wiki/Promises/KISS)。

简单地说 Promise 代表一个异步操作、代表将在未来完成的任务，
是否想起了Java的[FutureTask](http://docs.oracle.com/javase/8/docs/api/java/util/concurrent/FutureTask.html)）呢？
你可以在Promise对象上注册回调函数，监听该任务是*完成*或者*失败*。

从实现角度来看，Promises是一个有限状态机，共有三种状态：pending（执行中）、fulfilled（执行成功）和rejected（执行失败）。
其中pending为初始状态，fulfilled和rejected为结束状态（结束状态表示promise的生命周期已结束）。
状态转换关系为：pending->fulfilled 或者 pending->rejected，随着状态的转换将触发事件并执行回调函数。
而且它也提供了链式调用（通过then方法）将业务处理步骤链接起来，使得代码阅读更加流畅。

实现 Promises/A+ 模式的
[类库](http://www.cnblogs.com/oooweb/p/javascript-promises-a-comparison-of-libraries.html)
列表。

#### JavaScript Generators
http://howtonode.org/generators-vs-fibers
