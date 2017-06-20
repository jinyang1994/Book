# 字符串的扩展

## 字符的Unicode表书法

## codePointAt()

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
`codePointAt`可以直接返回4个字节的码点。

