# 字符串的扩展

## codePointAt

JavaScript内部。字符以`UTF-16`的格式存储，每个字符固定`2`个字节。

对于那些需要`4`个字符（Unicode码点大于`0xFFFF`的字符），会认为只两个字符。

```javascript
var s = '𠮷';

console.log(s.length); // 2
console.log(s.charAt(0)); // ''
console.log(s.charAt(1)); // ''
console.log(s.charCodeAt(0)); // 55362
console.log(s.charCodeAt(1)); // 57271
console.log(s.codePointAt(0)); // 134071
```

`charAt`无法获取到`4`个字节的整个字符。
`charCodeAt`只能分别返回前后两个字节的值。
`codePointAt`可以直接返回4个字节的值。

需要注意的是，`codePointAt`返回的是十进制，如果需要十六进制。那么使用`toString`就可以了。

```javascript
var s = '𠮷';

console.log(s.codePointAt(0).toString(16)); // 20bb7
```

还有一点需要注意，虽然`codePoinAt`可以返回4个字节。但是在JavaScript中`𠮷`被当做两个字符，所以在`𠮷`后面的字符其实位置是`2`开始的。

```javascript
var s = '𠮷s';

console.log(s.codePointAt(0)); // 134071 是𠮷
console.log(s.codePointAt(2)); // 97 是s
```

那么`1`是什么？

```javascript
console.log(s.codePointAt(1)); // 57271
```

其实只`𠮷`的后两个字节。

为了避免这个问题，我们可以使用`for...of `循环。

```javascript
var s = '𠮷a';
for (let ch of s) {
  console.log(ch.codePointAt(0).toString(16));
}

// 20bb7
// 61
```

`codePointAt`方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。

```javascript
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}

is32Bit('𠮷'); // true
is32Bit('a'); // false
```

## String.fromCodePoint

ES5中`String.fromCharCode`，用于返回码点对应的字符，但是无法返回大于`0xFFFF`的字符（如`𠮷`）。

ES6中`String.fromCodePoint`，用于返回大于`0xFFFF`的字符。

传入多个参数则返回字符串。

```javascriot
console.log(String.fromCodePoint(0x20BB7)); // 𠮷

console.log(String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'); // true
```

## 字符串的遍历器接口

ES6中，字符串添加了遍历器，这样不仅可以遍历字符串，而且还可以识别大于`0xFFFF`的字符串。

```javascript
var text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}

// ' '
// ' '

for (let i of text) {
  console.log(i);
}

// 𠮷
```

## at

ES5中，`charAt`用来返回制定位置的字符，但是该方法不能返回大于`0xFFFF`的字符。

`at`方法，不仅可以返回小于`0xFFFF`的字符，大于`0xFFFF`也可以。

```javascript
'abc'.at(0); // a
'𠮷'.at(0); // 𠮷
```

## normaliza

对于带有音调的字符，Unicode提供了两种方法，一种是提供带`重音符符号`的字符，比如`Ǒ`（\u01D1）。另一种是将两个字符`ˇ`（\u004F）、`O`（\u030C）合成一个字符`Ǒ`（\u004F\u030C）。

但是JavaScript无法判断两种是否相等。所以`normaliza`用来将用来判断两种字符是否相等。

```javascript
'\u01D1' === '\u004F\u030C'; //false

'\u01D1'.normalize() === '\u004F\u030C'.normalize(); //true
```

`normalize`可以接受一个参数，有四种可选值：

* `NFC`，默认参数，表示“标准等价合成”（Normalization Form Canonical Composition），返回多个简单字符的合成字符。所谓“标准等价”指的是视觉和语义上的等价。
* `NFD`，表示“标准等价分解”（Normalization Form Canonical Decomposition），即在标准等价的前提下，返回合成字符分解的多个简单字符。
* `NFKC`，表示“兼容等价合成”（Normalization Form Compatibility Composition），返回合成字符。所谓“兼容等价”指的是语义上存在等价，但视觉上不等价，比如“囍”和“喜喜”。（这只是用来举例，normalize方法不能识别中文。）
* `NFKD`，表示“兼容等价分解”（Normalization Form Compatibility Decomposition），即在兼容等价的前提下，返回合成字符分解的多个简单字符。

但是`normalize`目前不能识别三个或三个以上字符的合成。

## includes、startsWith、endsWith

* `includs`：返回布尔值，表示是否找到了参数字符串。
* `startsWith`：返回布尔值，表示参数字符串是否在源字符串的头部。
* `endsWith`：返回布尔值，表示参数字符串是否在源字符串的尾部。

```javascript
let s = 'Hello world!';

console.log(s.startsWith('Hello')); // true
console.log(s.endsWith('!')); // true
console.log(s.includes('o')); // true
```

第二个参数，表示开始搜索的位置。

```javascript
let s = 'Hello world!';

console.log(s.startsWith('world', 6)); // true
console.log(s.endsWith('Hello', 5)); // true
console.log(s.includes('Hello', 6)); // false
```

## repeat

`repeat`将当前字符串重复`n`次后返回一个新的字符串。

* 当参数大于`0`，重复`n`次。
* 当参数小于`-1`或`Infinity`，会报错。
* 当参数为小数，就向下取整。
* 当参数小于`0`大于`-1`时，视为`0`。
* 当参数为`NaN`，视为`0`。
* 当参数是字符串时，先将其转换为数字后判断。

## padStart、padEnd

两个方法用来当字符串长度不够时补全字符串，`padStart`从头部补全，`padEnd`从尾部补全。

第一个参数是`预计字符串长度`，第二个参数是`用来补全的字符串`。

* 当字符串长度等于或者大于第一个参数，则返回`原字符串`
* 当第二个参数加上原字符串长度大于第一个参数是，则对第二个参数的字符串进行截取。
* 如果忽略了第二个字符串，则用空格补全。

## 模板字符串

```javascript
let a = 1;
let str = `hello world ${a}!`; // 等价于 'hello world' + a + '!';
```

但是在模板字符串中，空格、缩进和换行都会被保留，如果不想要换行，可以使用`trim`消除。

当然可以在`${}`使用对象属性，调用函数等等。在`${}`都默认调用`toString`方法。

模板字符串可以嵌套，可以引用。

## 标签模板

它可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符串。这被称为“标签模板”功能（tagged template）。

```javascript
alert`123`;
// 等同于
alert(123);
```


