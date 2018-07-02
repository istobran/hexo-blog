---
title: 细说 javascript 的数据类型
date: 2018-06-14 12:00:26
categories: 技术研究
urlname: data-types-designing-of-javascript
tags: [javascript, ECMAScript]
---

现在出来写前端有一段时间了，现在来回头看看，当初大学刚学搞前端的时候，为了快速适应新的语言，直接看后面的内容去了，跳过了基本数据类型的介绍，想当然的觉得 javascript 的数据类型应该也跟其他弱类型语言差不了太多，从而忽略了基本数据类型上的细节。最近随着对 js 语言了解和应用的加深，发现 js 的基本数据类型里面真的大有文章，甚至有不少的语言设计缺陷在里面。所以我打算专门写一篇关于基本数据类型的文章，专门来谈谈 js 在基本数据类型上的这些设计和缺陷。
<!-- more -->

# 1. javascript 的诞生背景

要说 javascript 的设计缺陷，我觉得不得不先谈下 js 诞生的背景，据说 javascript 的作者 [Brendan Eich](https://zh.wikipedia.org/wiki/%E5%B8%83%E8%98%AD%E7%99%BB%C2%B7%E8%89%BE%E5%85%8B) 在设计和实现它的时候，总共加起来居然只花了十天的时间。。。

想一想，咱一个项目周期一般都是一个月左右，而这位大佬只花了十天就是设计出了一门语言！也许这就是真大佬的实力吧。。。

有兴趣的可以看下[阮一峰的这篇文章](http://www.ruanyifeng.com/blog/2011/06/birth_of_javascript.html)了解一下。但也就是在这种高压，并要求苛刻的背景下，就算是大佬也无法避免地在语言设计上出现了一些缺陷错误，有些随着时间的推移，想改都已经改不掉了，因为一改就会所有网站的 js 代码都得跟着改，由于当今 web 网站体量极为庞大，这么做会得罪很多人，所以这些缺陷错误都慢慢地成为了历史遗留问题。

作为一个 js 程序员，必须时时刻刻都要清楚牢记这些缺陷，在写代码的时候要尽可能地随时避开这些坑，这样子我们才能写出更加健壮，稳定，不坑自己的代码。

# 2. 基础数据类型表

首先必须要明确的一点是 javascript 这门语言根据 [ECMAScript 2015](https://www.ecma-international.org/ecma-262/6.0/#sec-ecmascript-data-types-and-values) 标准的定义，只有 7 种数据类型，它们分别是：

  1. Number
  2. Boolean
  3. String
  4. Symbol（ES2015 中加入）
  5. Null
  6. Undefined
  7. Object

其中原始 (Primitive) 数据类型为 **Number**, **Boolean**, **String**, **Symbol**, **Null**, **Undefined**

而引用 (Reference) 数据类型为 **Object**

其他什么 Array, Function, RegExp 等的都是 Object 的子类。

# 3. typeof 操作符上的问题

我们先来看一个 [ECMAScript 2015](https://www.ecma-international.org/ecma-262/6.0/#sec-typeof-operator) 中规范的表格：

**Table 35 — typeof Operator Results**

Type of val	| Result
-----------|-------------
Undefined	| "undefined"
Null | "object"
Boolean | "boolean"
Number | "number"
String | "string"
Symbol | "symbol"
Object (ordinary and does not implement [[Call]]) | "object"
Object (standard exotic and does not implement [[Call]]) | "object"
Object (implements [[Call]]) | "function"
Object (non-standard exotic and does not implement [[Call]]) | Implementation-defined. Must not be "undefined", "boolean", "function", "number", "symbol", or "string".

这个表格第一眼看过去会发现规范中存在着至少两个问题：

## 3.1. typeof null 的值为 "object"

前面说好的 null 是单独的一种数据类型，为什么 typeof null 又返回了 "object" 呢？？？

这实在是太奇怪了，网上搜查了一番，看到了[这个](https://www.zhihu.com/question/21691758)。原来 typeof null 就是一个设计缺陷，把 null 设计成了 0x00 ，而用 typeof 去判断类型的时候，如果低三位都是 0 就返回 "object"；

> 000: object. The data is a reference to an object.

这个问题就是前面所说的无法修改的设计缺陷，返回错误也拿它没办法，用户体量实在太庞大了，曾经有提案 typeof null === 'null'. 但提案被拒绝
[harmony:typeof_null [ES Wiki]](http://wiki.ecmascript.org/doku.php?id=harmony%3atypeof_null)

## 3.2. 为什么要多出个 "function" 类型出来

难道函数也要单独算一种类型？这跟前面基本数据类型的定义又对不上了。

带着这个问题我又去搜了一圈，找到了这篇东西

[JavaScript 里 Function 也算一种基本类型？](https://www.zhihu.com/question/24804474/answer/29040916)

> 如果抛开typeof null的谜题，那么剩下唯一不对应的地方，就是function被单列出来。我个人的看法，这是因为function确实很特殊，特殊到从实际应用场景考虑确实应该将其单独列出来。spec没有将其单列，是因为它同样有所有其他object的特性——而按照我的看法（爱民应该也是这样的看法），这仅仅是因为当初JS是如此设计和实现的。如果当初function不是作为一种特殊object来实现的（这完全是可能的，而且其实这样做更清晰），spec自然应该将其单列为类型之一。

如果按照这个说法这么一看，function 就是因为它的特殊性被开了特例，所以 typeof 这个函数也并不是完全按照前面基本数据类型来的，另外规范里面还有一种区分普通对象的 exotic 对象，这种对象可以根据浏览器实现来控制 typeof 的返回值，所以 typeof 现在的返回值也不仅仅只有基本数据类型和 function 了。

作为 typeof 的使用者，对于 typeof 这个操作符还是应该记住这个规律：

- 对于基本类型，除 null 以外，均可以返回正确的结果。
- 对于引用类型，除 function 以外，一律返回 object 类型。
- 对于 null ，返回 object 类型。
- 对于 function 返回  function 类型。

## 3.3. "number" 类型中的特殊值

对于 NaN 和 Infinity 这两个特殊值，typeof 遇到他们都会返回 number

```javascript
typeof Infinity   // "number"
typeof NaN        // "number"
```

Infinity 其实还好，但是 NaN(Not a Number)把它设计成 number 类型之后，使得有些时候用 typeof 在判断数字的时候不会达到我们想要的效果：

```javascript
var response = {}
var num = parseInt(response.a)
if (typeof num === 'number') {
    num = num + 10 // 执行后num仍然是NaN
}
```

服务器下发的数据中，缺少了 response.a 的数值字段，如果没有提前对这个字段是否为空做判断，就会发生上面的情况，它一旦出现，有时候会非常难找原因，因为在编译的时候它也不会报错，开发者不容易发现这个问题。

在这里也顺带说下，其实判断一个值是否为数字的最好的办法是借助 isFinite 函数来进行判断，因为 isFinite 函数本身会把传入值转换为 number，所以还得提前再加一层判断：
```javascript
function isNumber(val) {
    return typeof val === "number" && isFinite(val)
}
```

# 4. instanceof 操作符上的问题

关于 instanceof 这个操作符，可以在 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof) 上看到它的定义：

> instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

从这个定义上可以看出，instanceof 操作符相当于在原型链上查找一个 constructor，如果这个 constructor 存在的话，该运算符返回 true， 否则返回 false。

举个例子：
```javascript
function Animal() {}
function Dog() {}
Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;

var adog = new Dog();
console.log(adog instanceof Dog);    // 返回 true
console.log(adog instanceof Animal);   // 返回 true
```

平时这样子正常使用它是感觉不出来有问题的，然而，当遇到下面这些情况的时候，就需要注意了。

## 4.1. 当页面中存在一个或多个 frame 的时候

```javascript
[] instanceof window.frames[0].Array
```

这个跨环境的判断会返回 false，因为 `Array.prototype !== window.frames[0].Array.prototype`

这在跨 frame 传递数据对象的时候，需要注意的一个问题。

## 4.2. 一些相对特殊的判断

```javascript
Object instanceof Object    // true
Function instanceof Function      // true
Function instanceof Object    // true

Dog instanceof Object     // true
Dog instanceof Function   // true
Dog instanceof Dog    // false

Number instanceof Number    // false
Boolean instanceof Boolean    // false
String instanceof String    // false
Symbol instanceof Symbol    // false
```

虽然前面的规范表格里面有提到过，js 里的 Function 是一个实现了 [[call]] 方法的 Object，它具有 Object 对象的全部特性，也就是说，Function 是继承了 Object 对象的，所以 `Function instanceof Object` 不难理解。

但是 `Function instanceof Function` 和 `Object instanceof Object`，实在是比较容易懵，一开始我也没想明白为什么会这样，后来看到了 [IBM 上的一篇文章](https://www.ibm.com/developerworks/cn/web/1306_jiangjj_jsinstanceof/index.html)，里面对这个 instanceof 的执行过程做了一个模拟还原：

```javascript
function instance_of(L, R) {//L 表示左表达式，R 表示右表达式
 var O = R.prototype;// 取 R 的显示原型
 L = L.__proto__;// 取 L 的隐式原型
 while (true) { 
   if (L === null) 
     return false; 
   if (O === L)// 这里重点：当 O 严格等于 L 时，返回 true 
     return true; 
   L = L.__proto__; 
 } 
}
```

可以看出，instanceof 操作符会在左边的表达式中递归地向上查找 \_\_proto\_\_ 属性值，这个 \_\_proto\_\_ 对应着规范上的 [[Prototype]] 属性，再来对应到我们现在的问题上，最关键的是这一段：

    清单 7. Object instanceof Object
    ... 
    L = ObjectL.__proto__ = Function.prototype

等下，ObjectL.\_\_proto\_\_ 竟然是 Function.prototype ？？？

看到这里我才突然想起来，原来 Object 和 Function 他们本身都是作为函数来提供给用户调用的，所以他们必须是 Function，按照这个思路，`Object instanceof Object` 和 `Function instanceof Function` 返回 true 也就合情合理了。

而对于 `Dog instanceof Dog` 为什么返回 false，也是因为

    L = FooL.__proto__ = Function.prototype

左侧表达式一开始就为 Function.prototype 了，这样子的话左侧表达式的原型链在向上查找的过程中就找不到 Dog.prototype 了，结果自然返回的是 false。

## 4.3. 关于原始类型的包装类

对于四个原始类型的包装类，Number, Boolean, String, Symbol 返回 false 的原因会有所不同，我们来看下面一个例子：

```javascript
var str = "dog";
console.log(str instanceof String);   // false
console.log(str.__proto__ === String.prototype);    // true
```

按照这个结果，那前面的说法就不合逻辑了，str.\_\_proto\_\_ 都是String.prototype，把这个逻辑套到前面的函数里去

```javascript
instance_of(str, String);   // true
```

返回的就是 true，但是偏偏实际 instanceof 操作符返回的就是 false，这是为什么呢？

在翻阅了一轮资料之后，在 StackOverflow 上看到了这么一个[答案](https://stackoverflow.com/questions/15705232/confusion-about-prototype-chain-primitives-and-objects)。

原来，在我们对原始类型变量使用成员访问符 `.` 的时候，js 会自动调起装箱机制（auto-boxes），临时把原始数据类型通过语言内置的包装类给转换成对应的对象类型，在这个例子中相当于自动做了 `new String(str)` 这一步，所以我们才能在原始类型的变量上访问到 \_\_proto\_\_ 属性。

换句话说，原始数据类型本身是根本没有 \_\_proto\_\_ 这个对象属性的，在直接使用 instanceof 操作符的时候，js 运行环境并不会自动调起装箱过程，原始数据类型也并没有 [[Prototype]] 属性，所以它的返回的是 false，这也导致了 instanceof 操作符有无法用于判断原始数据类型的弊端。

# 5. 关于 null 和 undefined

javascript 有个非常独特的设计，就是 null 和 undefined 同时存在于这个语言中，这使得这门语言可以有两种表示空值的方法，这两个空值在使用过程中极其相似，比如
```javascript
if (!undefined) 
    console.log('undefined is false');
// undefined is false

if (!null) 
    console.log('null is false');
// null is false

undefined == null
// true
```
上面相等运算符甚至直接返回 true，很多开发者在实际开发中到底选择哪种也是随心所欲，根本没有去在乎过里面的区别，不过热衷于探索的我还是认为需要深究一下，它们之间究竟有什么区别呢？咱还是来看看定义吧。

## 5.1. 这两者在定义上的区别

- `null`，表示一个“无”的对象
- `undefined`，表示一个“无”的原始值

typeof 返回值的区别
```javascript
typeof null         //object
typeof undefined    //undefined
```

Number 转换上的区别
```javascript
Number(null)        // 0
+null               // 0
Number(undefined)   // NaN
+undefined          // NaN
```

出现时机上的区别
```javascript
var name;
console.log(name); //undefined
name = 'night';
console.log(name); //night
name = null;
console.log(name); //null
```

看完这三个例子，可以得出一些规律：

**对于 null：**
  1. 变量不主动赋值 null 的话是不会出现 null 的
  2. null 在转换为数字的时候会被转换为 0
  3. typeof null 的值返回的是 "object"

**对于 undefined：**
  1. 变量在声明的时候会被默认初始化为 undefined
  2. undefined 在转换为数字的时候会被转换为 NaN
  3. typeof undefined 会返回的是 "undefined"

## 5.2. 同时存在的历史原因

虽然我们得到了前面的规律，也可以看出来确实存在一些区别，可是这些区别直观的看过去它们实际上都是些无关痛痒的区别。按理说，这门语言其实只要一个空值就行了，两个空值在很多时候都作用重叠了，它为什么要这样子设计呢？

翻了一圈，看到了阮老师写的一篇 [undefined 与 null 的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)，里面提到了

> 1995年JavaScript诞生时，最初像Java一样，只设置了null作为表示"无"的值。
>
> 但是，JavaScript的设计者Brendan Eich，觉得这样做还不够，有两个原因。
> 
> 首先，null像在Java里一样，被当成一个对象。但是，JavaScript的数据类型分成原始类型（primitive）和合成类型（complex）两大类，Brendan Eich觉得表示"无"的值最好不是对象。
> 
> 其次，JavaScript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果null自动转为0，很不容易发现错误。
> 
> 因此，Brendan Eich又设计了一个undefined。

可见，undefined 是在 null 之后设计的，设计它的本意是希望能够补全 null 的不足，然而当真正这么实践的时候，却发现两个空值的作用和含义基本一致，当初只需要改进 null 值就好了，根本不需要多设计一个空值出来。

## 5.3. 二者的缺陷

即使 js 发展到了今天，其实不论是 null 还是 undefined，它们二者仍然存在着各自本身的缺陷。

先说下 null 值。前面也提到了，它作为原始数据类型，typeof null 的值返回 "object" 本身就是个巨大的设计缺陷，所以检测 null 值，用 typeof 是检测不出来的，一般建议下面这种方法：

```javascript
function isNull(val) {
  return val === null;
}
```

而检测对象的值，用 typeof 也没办法区分出 null 和真正的对象，所以要区分出真正的对象，还得用这种方法：

```javascript
function isEntityObject(val) {
    return val && typeof val === "object";
}
```

再来看看 undefined 值的问题，undefined 按理说也应该是一个原始类型的值，然而如果你接触 js 不深的话，你一定做梦也想不到 undefined 竟然是一个全局变量！
```javascript
window.hasOwnProperty("undefined")    // true
```

因为它既不是 js 里的关键字也不是保留字，可以在[规范](https://www.ecma-international.org/ecma-262/6.0/#sec-names-and-keywords)里面看到，undefined 即不存在于关键字列表中，也不存在于保留字列表中，所以它理论上就是一个变量，是可以被任意修改覆盖的。比如

```javascript
var undefined = "oops";
alert(undefined);
```

据说这段代码在 IE9 以下真的会弹出 `oops`，有兴趣的可以试一下，不过现在的浏览器已经锁定了全局 undefined 的值，上面的操作已经不能够修改 undefined 的值了，但是这段代码这么执行也并不会报错，相比 null，给它赋值就会报错。

```javascript
var null = 1;
VM122:1 Uncaught SyntaxError: Unexpected token null
```

然而，它也仅仅只是锁定了全局 undefined 的值，如果在局部声明一个 undefined 变量，它还是可以进行修改

```javascript
!function() {
  var undefined = "changed value";
  console.log(undefined);
}();
// changed value
```

所以，要想获得唯一准确的 undefined 值，而不是想借用 undefined 变量的话，得走一条弯路：去构造一个 undefined 值，这时候，js 的一个老古董 `void` 关键字就派上用场了。

## 5.4. 巧用 void 关键字

对于这个关键字，其实我们并不陌生，我们以前经常会见到 `javascript:void(0);` 这种用法，它与 `<a href="#">link</a>` 的区别是后者在点击了之后会跳到页面的最顶部去，改成用 `<a href="javascript:void(0);">link</a>` 就可以避免这个问题。

说到底其实 void 这个关键字，它本身的作用只有一个，就是返回一个 undefined 值，它是一个一元操作符，无论后面传入什么参数进去，他都会返回 undefined，比如：

```javascript
void 0;
void "you are useless?";
void false;
void [];
void /(useless)/ig;
void function(){ console.log("you are so useless?"); }
//... always return undefined
```

所以当我们需要使用真正的 undefined 值，而不是全局的 undefined 变量的时候，可以通过 void 关键字去生成一个出来，比如
```javascript
var scheduleFlush = void 0;
```

# 6. 利用 [[Class]] 精确判断数据类型

看过了前面 typeof 让人失望的表现之后，再来看看我们到底应该如何稳妥的判断数据类型，目前来说，要检测基本数据类型和内置对象，最好的方法是使用 `toString` 函数来进行判断。

toString 函数是 Object.prototype 上的方法，所以所有对象都拥有 toString 方法，它可以返回对象内部属性 **[[Class]]** 的值。但 Array, Date 等对象会重写从 Object.prototype 继承来的 toString 方法，从而返回不同的对象值，下面是一些例子：

```javascript
toString.call({});          // "[object Object]"
toString.call(function() {});     // "[object Function]"

toString.call([]);          // "[object Array]"
toString.call(/a/);         // "[object RegExp]"
toString.call(JSON);        // "[object JSON]"
toString.call(new Date());  // "[object Date]"

toString.call(0);           // "[object Number]"
toString.call(false);       // "[object Boolean]"
toString.call('foo');       // "[object String]"
toString.call(Symbol());    // "[object Symbol]"

toString.call(NaN);         // "[object Number]"
toString.call(Infinity);    // "[object Number]"

// Since ECMAScript 5 （JavaScript 1.8.5）
toString.call(null);        // "[object Null]"
toString.call(undefined);   // "[object Undefined]"
```

可见 toString 函数几乎所有的内置类型都可以返回预期的正确结果，而且由于返回值是字符串，也可以用于解决前面所提到的跨 frame 判断数据类型的问题。

不过这里值得一提的是，基本数据类型 number, boolean, string, symbol 在调用 toString 这个函数的时候，也是用了包装类来进行隐式自动装箱的，所以它们返回的是首字母大写的 Number, Boolean, String, Symbol。

而对于 null 和 undefined，却不存在 Null 和 Undefined 这两个包装类，根据 ES2015 规范中对于 [toString 函数的定义](https://www.ecma-international.org/ecma-262/6.0/#sec-object.prototype.tostring)

    1. If the this value is undefined, return "[object Undefined]".
    2. If the this value is null, return "[object Null]".
    ...

可知，它们是在函数判断的时候直接返回的。也就是说，null 和 undefined 这两个值在 toString 函数处理的时候不需要经过装箱处理，内部代码在直接判断的过程中就进行了返回。

`toString` 也不是完美的，它无法检测用户自定义类型。 因为 Object.prototype 是不知道用户会创造什么类型的， 它只能检测 ECMA 标准中的那些内置类型。

```javascript
toString.call(new Dog())   // [object Object]
```

所以如果你要判断的是基本数据类型或 JavaScript 内置对象，推荐使用 `toString`，如果要判断的时自定义对象类型，还是得使用 `instanceof` 操作符。