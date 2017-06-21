# 正则的扩展

## RegExp构造函数

在ES5中，RegExp构造函数的参数有两种情况。

* 第一个参数是字符串，第二个参数是正则表达式的修饰符。
* 第一个参数是正则表达式，此时第二个参数不允许使用。

```javascript
var regex = new RegExp('xyz', 'i');
// 等价于
var regex = /xyz/i;
// 等价于
var regex = new RegExp(/xyz/i);
```

ES6中，当第一个参数是正则表达式时，允许使用第二个参数，这时，第二个参数会覆盖掉原先的修饰符。

```javascript
console.log(new RegExp(/abc/ig, 'i').flags); // i
```

