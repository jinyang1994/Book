# 函数的扩展

## 函数参数的默认值

### 基本用法

```javascript
function Point(x = 0, y = 0) {
    this.x = x;
    this.y = y;
}
```

参数变量不能再使用`let`或`const`声明。

```javascript
function foo(x = 5) {
    let x = 1; // error
    const x = 2; // error
}
```

不能有同名参数。
参数默认值是**惰性求值**。

### 与解构赋值默认值结合使用



