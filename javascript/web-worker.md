# JavaScript 运行机制

>参考     
>[《Js运行机制深度剖析》](https://juejin.im/post/594156e6fe88c2006a4e5235)    
>[《JavaScript运行机制详解：再谈Event Loop》](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

## 为什么JavaScript是单线程

单线程用一句话来概括就是，同一个时间内只能做同一件事。

JavaScript之所以是单线程，原因在于JavaScript主要用于交互方面。如果多线程在同一时间内对同一个`DOM`进行不同的操作，例如，一个`删除`，一个`增加`。那么以哪个线程为准。

所以为了简化，JavaScript采用单线程。

为了利用多核CPU的计算能力，HTML5中新增了`Web Worker标准`，允许JavaScript脚本创建多个线程，但是在`《JavaScript 权威指南》`一书中，明确指出，`Web Worker`是获取不到`document`、`window`对象的。也就是说，`Web Worker`是无法操作`DOM`的。另外`Web Worker`和主线程之间的通讯只能靠异步传递来完成。

## 图示

![9472122021c614a6dddd180d3416423e](http://cdn.bigtiger.me/9472122021c614a6dddd180d3416423e.png)

## 任务队列

单线程就需要排队，前一个任务完成，才能执行后一个任务。

所以JavaScript把任务分成两种

* 同步任务（synchronous）：只有当前任务执行完成，再能往下进行。
* 异步任务（asynchronous）：进入`任务队列`，当这个任务可以执行了，才会进入主线程执行。

异步任务还分为：

* 计时函数，如`setTimeout`和`setInterval`。
* 事件监听，如发起一个`ajax`请求。

[《JS运行机制深度剖析》](https://juejin.im/post/594156e6fe88c2006a4e5235)一文中之处。任务队列是由`事件监听线程`和`计时线程`组成。

并且还提到将当前的执行上下文的概念。当任务可以执行后，会将该任务的执行上下文带入到`任务队列`中，对执行上下文的概念。下面会介绍到。

不过我对[《JS运行机制深度剖析》](https://juejin.im/post/594156e6fe88c2006a4e5235)文中反复使用栈来描述异步的这个线程，不是特别认同，我觉得[《JavaScript运行机制详解：再谈Event Loop》](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)使用的队列更加形象。

所以在JavaScript运行机制中，分为`执行栈`和`任务队列`。先去执行`执行栈`，当在`执行栈`中的任务完成后，会去`任务队列`中查找第一个可以执行`异步任务`（如计时函数执行时间到了、网络请求完成），加入到执行栈去执行。这个步骤会不断重复。

### 执行上下文

JavaScript执行的时候所在的`运行环境/作用域`。

* 全局执行上下文/作用域：js代码的默认执行环境（只有一个）
* 函数执行上下文/作用域：每个函数对应的执行环境（无限多个）
* eval 代码执行上下文：使用 eval 执行的脚步的执行环境

### 执行栈

`执行栈`遵循`先进后出`原则。

![1cd76098600a34b926cffaa6bf24c489](http://cdn.bigtiger.me/1cd76098600a34b926cffaa6bf24c489.gif)

在一个函数被调用时，JavaScript会创建一个上下文，此时会创建`作用域链`、`变量`、`函数和参数`、`this`。然后开始执行代码。


## 事件和回调函数

当`异步任务`可以执行后，就会在`任务队列`中添加一个任务。

所以在`任务队列`中的任务，都是已经完成前置，可以立即执行的。

任务队列原则是`先进先出`。

当`异步任务`完成后，需要执行的事先制定一系列操作，被称为`回调函数`。

## Event Loop

Event Loop（事件循环）指的是，主线程不断重复去`任务队列`中读取事件。

## 定时器

定时器：`setTimeout`、`setInterval`两个函数。

```javascript
setTimeout(function () { 
    console.log(1);
}, 0);
console.log(2);

// 2
// 1
```

!> HTML5标准规定setTimeout()的第二个参数的最小值，不得低于4毫秒。如果低于这个值，就会自动增加。但是老版本是10毫秒。涉及到DOM操作的是16毫秒

当调用setTimeout的时候，其实被加入了执行队列，接下来执行主线程的任务，而当主线程的任务清空后，才回去调用任务队列里面的任务。

## Node.js的Event Loop

还没搞清Node.js，学明白了再写。














