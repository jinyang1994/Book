# 数值的扩展

## 二进制和八进制表示法

ES6提供了二进制和八进制数值的新写法，分别使用`0b/0B`和`0o/0O`表示。

```javascript
0b111110111 === 503 // true
0o767 === 503 // true
```

!> 在ES5开始，严格模式下不允许使用`0`来表示八进制。ES6明确要使用`0o`来表示。

可以使用`Number`方法来转换成十进制。

## Number.isFinite、Number.isNaN

ES6在`Number`提供了`Numebr.isFinite()`检查是否为有限值和`Number.isNaN()`是否为`NaN`两个方法。

## Number.parseInt、Number.parseFloat

将两个`方法`添加到了`Number`对象上。

## Number.isInteger

这个`方法`用来判断一个值是否为整数，在JavaScript内部`3`
和`3.0`会判断成同一个值。

## Number.EPSILON

这个`属性`是一个极小的常量，用来判断浮点数计算的误差是否在合理范围内。如果计算结果小于`Number.EPSILON`，我们就任务是正确结果。

```javascript
0.1 + 0.2
// 0.30000000000000004

0.1 + 0.2 - 0.3
// 5.551115123125783e-17

5.551115123125783e-17 < Number.EPSILON
// true
```

## 安全整数和Number.isSafeInteger

JavaScript能够识别的数值范围是`-2^53`到`2^53`之间。`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`用来表示上限和下限。

`Number.isSafeInteger()`用来判断一个数值是否在这个范围内。

**注意！**在运算时，不要只验证最后结果是否安全，也要验证每个参与验算的值是否安全。

```javascript
Number.isSafeInteger(9007199254740993)
// false
Number.isSafeInteger(990)
// true
Number.isSafeInteger(9007199254740993 - 990)
// true
9007199254740993 - 990
// 返回结果 9007199254740002
// 正确答案应该是 9007199254740003
```

## Math对象的扩展

### Math.trunc

这个`方法`用来去除小数，返回整数部分。无法去除的，就返回`NaN`。

### Math.sign

这个`方法`用来判断一个数是正数、复述、还是零。

* 参数为正数，返回+1
* 参数为负数，返回-1
* 参数为0，返回0
* 参数为-0，返回-0
* 其他值，返回NaN

### Math.cbrt

这个`方法`用来计算一个数的立方根。

### Math.clz32

这个`方法`用来返回一个数的32位无符号整数形式有多少个前导0。

`clz32`这个函数名就来自”count leading zero bits in 32-bit binary representations of a number“（计算32位整数的前导0）的缩写。

对于小数。这个方法只考虑整数部分。

### Math.imul

这个`方法`返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数。

### Math.frountd

这个`方法`用来返回一个数的单精度浮点数形式。

```javascript
Math.fround(0)     // 0
Math.fround(1)     // 1
Math.fround(1.337) // 1.3370000123977661
Math.fround(1.5)   // 1.5
```

### Math.hypot

这个`方法`用来返回所有参数的平方和的平方根。

```javascript
Math.hypot(3, 4); // 3 * 3 + 4 * 4 = 25； 结果等于5
```

### 对数方法

#### Math.expm1

`Math.expm1(x)`返回ex - 1，即`Math.exp(x) - 1`。

#### Math.log1p

`Math.log1p(x)`方法返回`1 + x`的自然对数，即`Math.log(1 + x)`。如果`x`小于-1，返回`NaN`。

#### Math.log10

`Math.log10(x)`返回以10为底的`x`的对数。如果`x`小于0，则返回NaN。

#### Math.log2

`Math.log2(x)`返回以2为底的`x`的对数。如果`x`小于0，则返回NaN。

### 三角函数方法

* `Math.sinh(x)` 返回`x`的双曲正弦（hyperbolic sine）
* `Math.cosh(x)` 返回`x`的双曲余弦（hyperbolic cosine）
* `Math.tanh(x)` 返回`x`的双曲正切（hyperbolic tangent）
* `Math.asinh(x)` 返回`x`的反双曲正弦（inverse hyperbolic sine）
* `Math.acosh(x)` 返回`x`的反双曲余弦（inverse hyperbolic cosine）
* `Math.atanh(x)` 返回`x`的反双曲正切（inverse hyperbolic tangent）

### Math.signbit

`Math.sign()`用来判断一个值的正负，但是如果参数是`-0`，它会返回`-0`。

```javascript
Math.sign(-0); // -0
```

在`Math.sign()`在判断`0`不是特别有用。因为IEEE 754规定第一位是符号位，`0`表示正数，`1`表示负数。所以会有两种零，`+0`是符号位为`0`时的零值，`-0`是符号位为`1`时的零值。实际编程中，判断一个值是`+0`还是`-0`非常麻烦，因为它们是相等的。

```javascript
+0 === -0 // true
```

这个`方法`判断一个数的符号位是否设置了。

```javascript
Math.signbit(2) //false
Math.signbit(-2) //true
Math.signbit(0) //false
Math.signbit(-0) //true
```

该方法的算法如下：

* 如果参数是`NaN`，返回`false`。
* 如果参数是`-0`，返回`true`。
* 如果参数是负值，返回`true`。
* 其他情况返回`false`。

### 指数运算符

指数运算符`**`。
赋值运算符`**=`。

```javascript
2 ** 2 // 4

let b = 4;
b **= 3;
//  等同于 b = b * b * b;
```

在V8引擎中，指数运算符与`Math.pow`的实现不相同。

```javascript
Math.pow(99, 99)
// 3.697296376497263e+197

99 ** 99
// 3.697296376497268e+197
```




