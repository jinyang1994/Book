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

