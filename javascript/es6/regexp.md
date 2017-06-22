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

## 字符串的正则方法

ES6将字符串的上面有关正则的方法`match()`、`replace()`、`search()`、`split()`，定义到了RegExp对象上面。

* `String.prototype.match`调用`RegExp.prototype[Symbol.match]`
* `String.prototype.replace`调用`RegExp.prototype[Symbol.replace]`
* `String.prototype.search`调用`RegExp.prototype[Symbol.search]`
* `String.prototype.split`调用`RegExp.prototype[Symbol.split]`

## u修饰符

用来正确出来大于`0xFFFF`的Unicode字符。

* 在正则中`.`字符在正则表达式中，含义是除了换行符以外的所有字符，但是它不能识别大于`0xFFFF`的Unicode字符。这时候需要加上`u`修饰符。
* 在正则中使用`{}`表示`Unicode`字符。这时候也需要加上`u`修饰符，才能识别大于`0xFFFF`的字符。
* 



