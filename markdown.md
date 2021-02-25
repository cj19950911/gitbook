## 数局的拷贝

如何区分深拷贝与浅拷贝，简单点来说，就是假设B复制了A，当修改A时，看B是否会发生变化，如果B也跟着变了，说明这是浅拷贝，拿人手短，如果B没变，那就是深拷贝，自食其力。
2. 如果是引用数据类型，名字存在栈内存中，值存在堆内存中，但是栈内存会提供一个引用的地址指向堆内存中的值

## 数据处理存在的问题

先定义一个初始对象，供后面例子使用：
首先定义一个`currentState`对象，后面的例子使用到变量`currentState`时，如无特殊声明，都是指这个`currentState`对象
```javascript
let currentState = {
  p: {
    x: [2],
  },
}
```

哪些情况会一不小心修改原始对象？

```javascript
// Q1
let o1 = currentState;
o1.p = 1; // currentState 被修改了
o1.p.x = 1; // currentState 被修改了

// Q2
fn(currentState); // currentState 被修改了
function fn(o) {
  o.p1 = 1;
  return o;
};

// Q3
let o3 = {
  ...currentState
};
o3.p.x = 1; // currentState 被修改了

// Q4
let o4 = currentState;
o4.p.x.push(1); // currentState 被修改了
```
##


## 不可变性(Immutability)

var statement = 'I am an immutable value';
var otherStr = statement.slice(8, 17);

object.is

## 解决引用类型对象被修改的办法

1. 深度拷贝，但是深拷贝的成本较高，会影响性能；
2. 不可变性
3. immer
4. lodash deepclone


