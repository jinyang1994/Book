# ES6 - let 和 const

## let 命令
用来声明变量，与`var`类似，但是不同的是，`let`是块级作用域。

!> JavaScript引擎内部会在`for`循环中记录上一次循环的变量，在初始化时会根据上一次的变量值去计算本轮的值。另外，`let`在`for`循环中，循环语句与循环体享受不同的作用域。

## 不存在变量提升

首先回忆一下什么是变量提升。
在使用`var`声明变量的时候，变量可以在声明之前就使用，但是值为`undefined`。

```javascript
console.log(foo); // undefined

var foo = 1;
```

实际执行的是以下操作：

```javascript
var foo;

console.log(foo);
foo = 1;
```

但是在使用`let`声明变量的时候，不存在变量提升的情况。也就是在声明前使用变量，会抛出一个错误。

```javascript
console.log(foo); // ReferenceError
let foo = 1;
```

## 暂时性死区

在块级作用域中存在`let`命令，那么就在受外界影响。

```javascript
var tmp = 123;

if (true) {
    tmp = 222; // ReferenceError
    let tmp;
}
```

使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）

!> “暂时性死区”也意味着`typeof`不再是一个百分之百安全的操作。

两个死区比较隐蔽的例子：

```javascript
function bar(x = y, y = 2) {
    return [x, y];
}

bar(); // ReferenceError: 由于在x = y时，y还没有声明，属于”死区“
```

```javascript
let x = x; // ReferenceError: x is not defined
```


## 不允许重复声明

`let`不允许在相同作用域内，重复声明同一个变量。

```javascript
function () {
    var a = 10;
    let a = 1;
}

function () {
    var a = 10;
    let a = 1;
}
```

因此，不能在函数内部重新声明参数。


```javascript
function (arg) {
  let arg; // 报错
}

function (arg) {
  {
    let arg; // 不报错
  }
}
```

## 块级作用域

ES6之前是没有块级作用域的概念的。

没有块级作用域会导致

* 内层变量覆盖外层变量
* 循环变量泄露

ES6出现了块级作用域之后

* 允许块级作用域任意嵌套
* 内层作用域定义外层作用域的变量
* 不必要再使用立即执行函数表达式（IIFE）

需要注意的是，ES6规定在浏览器环境中的实现，在函数方面稍有不同。    

* 允许在块级作用域内声明函数。    
* 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。    
* 同时，函数声明还会提升到所在的块级作用域的头部。    

** 因为浏览器与其他环境的不同，所以建议不要在块级作用域内声明函数，如果需要，请使用函数表达式 **

另外，块级作用域只有在`{ }`才成立，如果没有使用，则会报错。

```javascript
if (true) {
    function f() {} // 不报错
}

if (true) 
    function f() {} // 报错
```


**块级作用域的本质是一个语句，是一个没有返回值得代码块**

但是在一个提案中，提到了`do`表达式使块级作用域变成表达式，当然也可以有**返回值**

```javascript
let x = do {
    let t = f();
    t * t + 1;
}
```

## const 命令

`const`命令，用来声明一个常量，在ES6中是不允许`const`声明时不进行赋值的。

当然`const`与`let`相同，他们都是块级作用域，同样也存在暂时性死区。

```javascript
if (true) {
    console.log(MAX) // ReferenceError
    const MAX = 5;
}

MAX // Uncaught ReferenceError: MAX is not defined
```

这里有个误解。`const`并不是变量的值不可改变，而是变量指向的内存地址不可改变。对于`基本数据类型`来说，他们的值就存在对应的内存地址中，但`引用数据类型`，存在内存地址中的仅仅是一个指针，这就导致我们并不能控制`引用数据类型`的数据结构。

```javascript
const foo = [];
foo.push(1); // 不报错

foo = [1]; // 报错，因为指针改变了
```

彻底冻结对象的函数（对象本身和属性都不可改变）

```javascript
const constantize = (obj) => {
    Object.freeze(obj);
    Object.keys(obj).forEach((key, i) => {
        if (typeof obj[key] === 'object') {
            constantize(obj[key]);
        }
    }
}
```

## 6种声明变量的方式

* `var`命令和`function`命令
* `let`命令和`const`命令
* `import`命令和`class`命令

## window对象

`let`命令、`const`命令、`class`命令在顶层对象声明时，并不像`var`命令和`function`命令那样，称为顶层对象的属性。

```javascript
var a = 1;
console.log(window.a) // 1

let b = 1;
console.log(window.b) // undefined
```

## global对象

ES5在各环境中实现的不统一。

* 浏览器里面，顶层对象是`window`，但 Node 和 Web Worker 没有`window`。
* 浏览器和 Web Worker 里面，`self`也指向顶层对象，但是Node没有`self`。
* Node 里面，顶层对象是`global`，但其他环境都不支持。

为了兼容各个环境，一般是使用`this`变量，但是有局限性

* 全局环境中，`this`会返回顶层对象。但是，Node模块和ES6模块中，`this`返回的是当前模块。
* 函数里面的`this`，如果函数不是作为对象的方法运行，而是单纯作为函数运行，`this`会指向顶层对象。但是，严格模式下，这时`this`会返回`undefined`。
* 不管是严格模式，还是普通模式，`new Function('return this')()`，总是会返回全局对象。但是，如果浏览器用了CSP（Content Security Policy，内容安全政策），那么`eval`、`new Function`这些方法都可能无法使用。

勉强的兼容办法

```javascript
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

现在有一个提案就是，在各个环境中都存在`global`。

现在可以使用第三方的库`system.global`来实现，所有环境都可以拿到`global`。

这两种写法可以保证各个环境都存在`global`对象

```javascript
// CommonJS的写法
require('system.global/shim')();

// ES6模块的写法
import shim from 'system.global/shim'; 
shim();
```

这两种写法可以将顶层对象放入变量`global`

```javascript
// CommonJS的写法
var global = require('system.global')();

// ES6模块的写法
import getGlobal from 'system.global';
const global = getGlobal();
```

## 变量的解构赋值

### 数组的解构赋值


