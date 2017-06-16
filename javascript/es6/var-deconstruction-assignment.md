# 变量的解构赋值

## 数组的解构赋值

`模式匹配`写法，只要两边匹配，左边的变量会赋予对应右边的值

```javascript
let [foo, [[bar], baz]] = [1, [[2], 3]];

console.log(foo, bar, baz); //1, 2, 3 
```

如果匹配时，右边没有对应的值，那么这个变量就等于`undefined`。

```javascript
let [x, y] = [1, 2, 3];

console.log(x, y) //1, 2
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
console.log(x) // 1

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

### 对象的解构赋值

对象解构赋值与数组类似，但是不同的是，取值时是根据对象的属性来进行对应的取值。
同样的，如果找不到对应的值，就等于`undefined`。

```javascript
let { foo, bar } = { bar: 'aaa', foo: 'bbb' };
console.log(foo, bar) // aaa, bbb
```

如果变量名和属性名不一致，必须遵循下面的写法。

```javascript
var { foo: baz } = { foo: 'aaa' };
console.log(baz) // aaa
```




