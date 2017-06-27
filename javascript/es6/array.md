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


