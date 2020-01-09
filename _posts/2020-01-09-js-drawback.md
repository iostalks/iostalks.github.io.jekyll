---
layout: articles
title: JavaScript 糟粕集
categories: [JavaScript]
---

JavaScript 是 Brendan Eich 花了十天开发出来的，如此短暂的时间开发一门语言，难免会有考虑不周的地方。

虽然 EAMCScript 规范不断的更新迭代，然而很多浏览器可能还在使用老的规范，原本设计不合理的功能，或许已经被使用，并发布上线了。

<!--more-->

如果在新版本中强行的移除，会导致无法预料的灾难，因此即使是 bug 依然得在新版本中保留。

这对于 JavaScript 新秀很不友好，除了要掌握它新的特性，还得了解老版本留下来的坑。

JavaScript 虽然有不少糟粕，但也有众多的优点。这也是为什么大多数 JSer 对它又恨又爱。

吐槽归吐槽，理解 JavaScript 缺陷背后的原因，往往能帮助我们理解这门语言的设计初衷，从而更好的应用它。

这篇文章将罗列 JavaScript 中常见的糟粕。

### 1. typeof null

ES6 之后，JavaScript 中存在 7 中内置类型：空(null), 未定义(undefined), 布尔(boolean), 数字(number), 字符串(string), 对象(object), 符号(symbol)。

我们可以使用 typeof 来检测值的类型，它会返回一个字符串。其中六种值都能返回正确的类型，唯独 null 一枝独秀：

``` js
typeof null === ‘object’
```

null 作为值类型，typeof 返回的却是 object。因此在使用 typeof 检测对象的时候，要留意值为 null 的情况。

正确判断 object 类型，应该排除 null 的情况。

``` js
function isObject(obj) {
  return !obj && typeof obj === 'object'
}
```

### 2. NaN

NaN（Not a Number） 表示一个非数字字面量。在执行数学运算时，出现非数值类型，或者不合法的计算，会得到这个结果。

需要注意的是 NaN 的类型仍旧是 number。

``` js
typeof NaN === ‘number’ // true
```

它有一个特殊的地方是，不等于任何值，包括它自己。

``` js
NaN === NaN // false
NaN !== NaN // true
```

ES6 之前全局的 isNaN() 用来检测一个变量是不是 NaN，但这个函数存在缺陷，建议使用 ES6 之后 Number 提供的 Number.isNaN() 方法。

NaN 是一个全局的属性，效果等同于 Nubmer.isNaN。

NaN 还是合法的变量名，如果在全局范围内误用，会出现意想不到的结果。

``` js
var NaN = 101;
console.log(NaN) // 101
console.log(Number.isNaN(1 / 0)); // true
```

### 3. undefined 和 null

undefined 表示变量未定义，在其它语言中，如果变量（对象）没有定义，默认会初始化为 null。

JavaScript 变量如果没有赋值，默认为 undefined，null 就显得有些多余。

在概念上它们还有区别的，以坐地铁为例， undefined 表示没有位置，而 null 表示的是有位置，但是这个位置没人座。

很少有场景需要区分的这么细，一般我们判断对象是否存在，都会把它转为布尔类型，null 和 undefined 转为布尔类型都是 false。

undefined 其实是一个全局变量，它不可改，不可配置，不可枚举。

``` js
const desc = Object.getOwnPropertyDescriptor(global, 'undefined');
console.log(desc);
/**
{ value: undefined,
  writable: false,
  enumerable: false,
  configurable: false
}
*/
```

在语法层面 undefined 能够作为普通变量来使用，因此存在一定的安全隐患。好在它不会覆盖全局的 undefined 的，误用的可能性并不大。

``` js
var undefined = 1;
console.log(undefined) // 1
console.log(global.undefined) // undefined
```

(未完待续...)
