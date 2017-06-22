# JavaScript 运行机制

>参考     
>[《Js运行机制深度剖析》](https://juejin.im/post/594156e6fe88c2006a4e5235)    
>[《JavaScript运行机制详解：再谈Event Loop》](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

## 为什么JavaScript是单线程

单线程用一句话来概括就是，同一个时间内只能做同一件事

JavaScript之所以是单线程，原因在于JavaScript主要用于交互方面。如果多线程在同一时间内对同一个`DOM`进行不同的操作，例如，一个`删除`，一个`增加`。那么以哪个线程为准。

所以为了简化，JavaScript采用单线程。



