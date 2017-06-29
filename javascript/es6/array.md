# 数组的扩展

## Array.from

用来将`类数组对象`和`可遍历的对象`转换为数组。

```javascript
const arr = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    'length': 3,
};

// ES5
var arr1 = [].slice.call(arr); // ['a', 'b', 'c']

// ES6
var arr2 = Array.from(arr); // ['a', 'b', 'c']
```

在实际中，还有DOM操作返回的`NodeList`集合，以及函数内部的`arguments`对象，都可以使用`Array.from`转换数组。

只要部署了`Iterator`接口的数据结构，`Array.from`都能将其转为数组。后面会说到`Iterator`。

```javascript
Array.from('hello');
// ['h', 'e', 'l', 'l', 'o'];

let namesSet = new Set(['a', 'b']);
Array.from(nameSet); // ['a', 'b']
```

如果传入一个真正的数组，那么`Array.from`会返回一个同样的数组。


`...`运算符也会将某些数据结构转换为数组。

实质上调用的事`Symbol.iterator`。特征只有一点，必须要有`length`属性。如果没有，就无法转换。

`Array.from`接受的第二个参数是一个函数，用来对每个元素进行处理，然后返回到数组中。

```javascript
Array.from(arrayLike, x => x * x);
// 等价于
Array.from(arrayLike).map(x => x * x);
```

如果第二个参数中用到了`this`关键字，`Array.from`第三个参数可以用来指定`this`。

`Array.from`还可以将字符串转换为数组，然后返回字符串的长度可以识别大于`\uFFFF`的Unicode字符串。

## Array.of

用来将一组值转换为数组。主要解决`Array()`参数不同返回结果不一样的问题。

```javascript
// 没有参数
Array(); // []
Array(3); // [,,];
Array(3, 11, 8); // [3, 11, 8]
```

## 数组实例的copyWithin

用来将指定位置的成员复制到其他位置（会覆盖原有成员），并返回结果，使用这个方法，会修改原数组。

```javascript
Array.prototype.copyWithin(target, start = 0, end = this.length);
```

它接受三个参数

* target（必需）：从该位置开始替换数据。
* start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
* end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
```

上面代码表示将从3号位直到数组结束的成员（4和5），复制到从0号位开始的位置，结果覆盖了原来的1和2。

更多例子，没怎么理解，记下来接着看。

```javascript
// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}

// 将2号位到数组结束，复制到0号位
var i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// 对于没有部署TypedArray的copyWithin方法的平台
// 需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
```

## 数组实例的find和findIndex

`find`用来找到第一个符合条件的数组成员。没有找到则返回`undefined`。
`findIndex`返回第一个符合条件数组成员的下标。没有找到则返回`-1`。

他们两个都有一个回调函数，第一个参数是`当前的值`，第二个参数是`当前的下标`，第三个参数是`原数组`。

方法的第二个参数可以指定回调函数的`this`对象。

## 数组实例的fill

用来给数组指定值。方法接受三个参数，第一个是`填充的值1`，第二个是`填充的起始位置`，第三个是`填充的结束位置`。

```javascript
['a', 'b', 'c'].fill(7);
// [7, 7, 7]

['a', 'b', 'c'].fill(7, 1, 2);
// ['a', 7, 'c']
```

## 数组实例的entries、keys、values

ES6的`entries()`、`keys()`、`values()`用来遍历数组，他们会返回一个遍历器对象。可以使用`for...of`循环进行遍历。

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

也可以使用遍历器对象的`next`方法，进行遍历。

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```

## 数组实例的includes

用来检查数组里面是否存在某个值。第一个参数是`要查找的值`，第二个参数是`查找的起始位置`

```javascript
[1, 2, 3].includes(2); // true
[1, 2, 3].includes(3, 3); // false
```

## 数组的空位

`空位`指的是数组的某个位置没有**任何值**

`空位`不是`undefined`。

ES5对`空位`的处理。

* `forEach()`、`filter()`、`every()`、`some()`会跳过空位
* `map()`会跳过空位，但会保留这个指。
* `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

ES6 则是明确将空位转为`undefined`。


