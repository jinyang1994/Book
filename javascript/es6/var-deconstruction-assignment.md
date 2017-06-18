# 变量的解构赋值

## 数组的解构赋值

`模式匹配`写法，只要两边匹配，左边的变量会赋予对应右边的值

```javascript
let [foo, [[bar], baz]] = [1, [[2], 3]];

console.log(foo, bar, baz); // 1, 2, 3 
```

如果匹配时，右边没有对应的值，那么这个变量就等于`undefined`。

```javascript
let [x, y] = [1, 2, 3];

console.log(x, y); // 1, 2
```

如果左右两边数据类型不相等，则会报错

```javascript
let [foo] = 1; // 报错
```

> 上面的语句都会报错，因为等号右边的值，要么转为对象以后不具备 Iterator 接口（前五个表达式），要么本身就不具备 Iterator 接口（最后一个表达式）。
> 注：这句没懂... 到时候回过头看

对于Set结构，也可以使用数组来解构赋值

```javascript
let [x, y, z] = new Set(['a', 'b', 'c']);

console.log(x); // 'a'
```

解构赋值可以设置默认值。

```javascript
let [foo = true] = [];

console.log(foo); // true
```

!> 需要注意的是，ES6自身内部使用`===`来判断当前变量是否存在，也就是说如果当前的值不等于`undefined`，那么默认值不会生效。并且默认值是惰性求值，也就是说，只有在动刀的时候，才会去求值。

```javascript
let [x = 1] = [undefined];

console.log(x); // 1

let [x = 1] = [null];

console.log(x); // null

// 惰性求值
let [x = f()] = [1]; // 函数f并没有执行

// 等价于
let x;
if ([1][0] === undefined) {
    x = f();
} else {
    x = [1][0];
}
```

当然也可以引用其他变量，但变量必须已经声明，否则会报错。

```javascript
let [x = 1, y = x] = [];

console.log(x, y); // 1, 1

let [x = 1, y = z] = []; // ReferenceError
```

嵌套时要保持结构（或者说父子间的嵌套关系）的一致，否则会报错。

```javascript
let [x, [y]] = [1, 2]; // 报错
let [x, [y]] = [1, [2]]; // 成功
```

## 对象的解构赋值

对象解构赋值与数组类似，但是不同的是，取值时是根据对象的属性来进行对应的取值。
同样的，如果找不到对应的值，就等于`undefined`。

```javascript
let { foo, bar } = { bar: 'aaa', foo: 'bbb' };

console.log(foo, bar); // aaa, bbb
```

事实上，是下面的简写。

```javascript
let { foo: foo, bar: bar } = { foo: 'aaa', bar: 'bbb' };
```

如果变量名和属性名不一致，必须遵循下面的写法。

```javascript
var { foo: baz } = { foo: 'aaa' };

console.log(baz); // aaa
```

也就是说，真正赋值的是`value`。而不是`key`。

注意！使用这种写法是变量的生命和赋值是一体的，也就是说。如果重新声明就会报错。

```javascript
let foo;
let { foo } = { foo: 1 }; // SyntaxError: Duplicate declaration "foo"
```

```javascript
let foo;

({ foo } = { foo: 1 });
console.log(foo); // 1
```

上面的代码，使用了`()`将其变成了一个代码块，而不是赋值语句。

和数组一样，对象的解构赋值也可以嵌套。

```javascript
let obj = {
    p: [
        'hello',
        { y: 'world' },
    ],
};
let { p: [x, { y }] } = obj;

console.log(x, y); // hello, world
```

同样，也可以指定默认值。并且严格遵循`=== undefined`。

```javascript
let { x = 3 } = {};
let { x: y = 1 } = {};
let { z = 3 } = { z: null };

console.log(x, y, z); // 3, 1, null
```

保持结构的统一（子属性所在的父属性存在），否则会报错。原因很简单，当父属性不存在时，是`undefined`，再去子属性会报错。

```javascript
let { foo: { bar } } = { baz: 'baz' }; // Uncaught TypeError: Cannot match against 'undefined' or 'null'.
```

解构赋值允许左边什么都不写。当然也不会起到任何作用。

```javascript
({} = [true, false]);
```

由于数组本质上也是对象，所以可以对数组进行解构。

```javascript
let arr = [1, 2, 3];
let { 0: first, [arr.length - 1]: last } = arr;

console.log(first, last); // 1, 3
```

## 字符串的解构赋值

解构时，会将`字符串`转换成`类数字对象`，同样类数组对象存在一个`length`属性。

```javascript
let [a, b, c, d, e] = 'hello';
let { length: len } = 'hello';

console.log(a, b, c, d, f); // h, e, l, l, o
console.log(len); // 5
```

## 数值和布尔值的解构赋值

解构时，如果右侧是数值或者布尔值，则会先转为对象。

```javascript
let { toString: s } = 123;
s === Number.prototype.toString; // true

let { toString: s } = true;
s === Boolean.prototype.toString; // true
```

因为数值和布尔值都有`toString`属性，所以s可以取到值。

## undefined和null的结构赋值

解构赋值的原则是，只要右侧不是数组或对象。就先将其转换成对象。

由于`undefined`和`null`无法转换成对象，所以会报错。

## 函数参数的结构赋值

函数的参数也可以结构赋值。

```javascript
function add([x, y]) {
    return x + y;
}

add([1, 2]); // 3
```

在参数解构时。就已经将参数`[1, 2]`解构成`x = 1`和`y = 2`。

参数也可以赋默认值

```javascript
function move({ x = 0, y = 0 } = {}) {
    return [x, y];
}

move({ x: 3, y: 8 }) // [3, 8]
move({ x: 3}) // [3, 0]
move({}) // [0, 0]
move() // [0, 0]
```

注意下面的写法。

```javascript
function move({ x, y } = { x: 0, y: 0 }) {
    return [x, y];
}

move({ x: 3, y: 8}); // [3, 8]
move({ x: 3 }); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```

原因是，当参数为`undefined`是才能触发默认值，上面的三种并不是`undefined`，所以在传入的参数中解构。

## 圆括号的问题

一个式子到底是模式，还是表达式。只有到解析`=`才能知道。

我们编程的原则是尽量不使用`()`。

### 不能使用圆括号的情况

* 变量声明语句中，不能带圆括号

```javascript
let [(a)] = [1];
```

* 函数参数也属于变量声明，不能带圆括号

```javascript
function f([(z)]) {
    return z;
}
```

* 赋值语句中，不能将**整个模式**或者**嵌套模式的一层**放在圆括号中。

```javascript
// 整个模式
({ p: a }) = { p: 42 };

// 嵌套模式的一层
```

### 可以使用圆括号的情况

非模式部分可以使用圆括号。以下都是正确的。

```javascript
[(b)] = [3]; 
({ p: (d) } = {}); 
[(parseInt.prop)] = [3];
```

## 用途

### 交换变量的值

```javascript
let x = 1;
let y = 2;

[x, y] = [y, x];
```

### 从函数返回多个值

```javascript
function example() {
    return [1, 2, 3];
}

let [a, b, c] = example();
```

### 函数参数的定义

```javascript
function f([x, y, z]) { ... }

f([1, 2, 3]);
```

### 提取JSON数据

```javascript
let json = {
    id: 1,
    status: 'OK',
}

let { id, status } = json;
```

### 函数参数的默认值

```javascript
let ajax = function (url, {
    async = true
    // ... more config
}) {
    // ... do stuff
};
```

这样可以避免在函数内部写`var foo = config.async || true`。

### 遍历Map结构

```javascript
let map = new Map();
map.set('a', 1);
map.set('b', 2);

for (let [key, value] of map) {
    console.log(key, value);
}

// 获取键名
for (let [key] of map) {
    console.log(key);
}

// 获取键值
for (let [,value] of map) {
    console.log(value);
}
```

### 输入模块的指定方法

```javascript
const { a, b } = require('test');

import { a, b } from 'test';
```








