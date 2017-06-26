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

