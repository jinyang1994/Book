# ECMAScript 6

## let 和 const

### let 命令
用来声明变量，与`var`类似，但是不同的是，`let`是块级作用域。

!> JavaScript引擎内部会在`for`循环中记录上一次循环的变量，在初始化时会根据上一次的变量值去计算本轮的值。另外，`let`在`for`循环中，循环语句与循环体享受不同的作用域。

### 不存在变量提升

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

### 暂时性死区

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


### 不允许重复声明

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

