# 第二章：函数的性质

函数式编程 **不是使用 `function` 关键字编程。** 哦，如果它真有那么简单 —— 我在这里就可以结束这本书了！不管怎么说，函数确实是 FP 的中心。而使我们的代码成为 _函数式_ 的，是我们如何使用函数。

但是，你确信你知道 _函数_ 是什么意思吗？

在这一章中，我们将要通过讲解函数的所有基本方面来为本书的剩余部分打下基础。事实上，这里的内容即便是对非 FP 程序员来说也是应当知道的关于函数的一切。但是，毫无疑问地，如果我们想要从 FP 的概念中学到竟可能多的东西， _知道_ 函数的里里外外是必要的。

振作起来，关于函数东西可能比你已经知道的东西多得多。

## 什么是函数？

“什么是函数？” 这个问题表面上看起来有一个显而易见的答案：一个函数是一个可以被执行一次或多次的代码的集合。

虽然这个定义是合理的，但它丢掉了一些非常重要的实质，而那是 _函数_ 在 FP 中的核心。所以，让我们挖开表层来更完整地理解函数吧。

### 数学简忆

我知道我承诺过尽可能远离数学，但稍稍忍耐我片刻，在继续之前我们快速地检视一些东西：代数中有关函数和图像的基础。

你还记得在学校里学过的关于 `f(x)` 的一些东西吗？等式 `y = f(x)` 呢？

比如说一个等式这样定义的：<code>f(x) = 2x<sup>2</sup> + 3</code>。这是什么意思？给这个函数画出图像是什么意思？这就是图像：

<p align="center">
    <img src="./images/fig1.png" width="40%">
</p>

你能注意到，对于任何 `x` 的值，比如 `2`，如果你将它插入这个等式，你会得到 `11`。那么 `11` 是什么？它是函数 `f(x)` 的 _返回值_，代表我们刚才说到的 `y` 值。

换句话说，在图像的曲线上有一个点 `(2,11)`。而且对于我们插入的任意的 `x` 的值，我们都能得到另一个与之相对应的 `y` 值作为一个点的坐标。比如另外一个点 `(0,3)`，以及另一个点 `(-1,5)`。将这些点放在一起，你就得到了上面的抛物线图像。

那么这到底与 FP 有什么关系？

在数学中，一个函数总是接受输入，并且总是给出一个输出。一个你将经常听到的 FP 术语是“态射（morphism）”；这个很炫的词用来描述一个值的集合映射到另一个值的集合，就像一个函数的输入与这个函数的输入的关系一样。

在代数中，这些输入与输出经常被翻译为被绘制的图像的坐标的一部分。然而，我们我可以使用各种各样的输入与输出定义函数，而且它们几乎不会在视觉上被表示为图像曲线上的点。

### 函数 vs 过程

那么为什么说了半天数学和图像？因为在某种意义上，函数式编程就是以这种数学意义上的 _函数_ 来使用函数。

你可能更习惯于将函数考虑为过程（procedures）。它有什么区别？一个任意功能的集合。它可能有输入，也可能没有。它可能有一个输出（`return` 值），也可能没有。

而一个函数接收输入并且绝对总是有一个 `return` 值。

如果你打算进行函数式编程，**你就应当尽可能多地使用函数**，而尽可能地避免过程。你所有的 `function` 都应当接收输入并返回输出。为什么？

这个问题的答案有许多层次的含义，我们将在这本书中逐一揭示它们。

## 函数输入

至此，我们可以得出结论说函数必须拥有输入。让我们深入地看看函数输入是如何工作的。

你有时会听到人们称它们为“实际参数（arguments）”，而有时称为“形式参数（parameters）”。那么这都是什么意思？

_实际参数_ 是你传入的值，而 _形式参数_ 是在函数内部被命名的变量，它们接收那些被传入的值。例如：

```js
function foo(x, y) {
  // ..
}

var a = 3;

foo(a, a * 2);
```

`a` 和 `a * 2`（实际上，是 `a * 2` 的结果，也就是 `6`） 是 `foo(..)` 调用的 _实际参数_。`x` 和 `y` 是接收实际参数值（分别是 `3` 和 `6`）的 _形式参数_。

**注意：** 在 JavaScript 中，不要求 _实际参数_ 的数量与 _形式参数_ 的数量相吻合。如果你传入的 _实际参数_ 多于被声明来接受它们的 _形式参数_ ，那么这些值会原封不动地被传入。这些值可以用几种不同的方式访问，包括你可能听说过的老旧的 `arguments` 对象。如果你传入的 _实际参数_ 少于被声明的 _形式参数_，那么每一个无人认领的形式参数都被看做是一个 “undefined” 变量，这意味着它在这个函数的作用域中存在而且可用，只是初始值是空的 `undefined`。

### 默认形式参数

在 ES6 中，形式参数可以声明 _默认值_。在这个形式参数所对应的实际参数没有被传递，或者被传递了 `undefined` 值的情况下，默认的赋值表达式就会顶替上来。

考虑如下代码：

```js
function foo(x = 3) {
  console.log(x);
}

foo(); // 3
foo(undefined); // 3
foo(null); // null
foo(0); // 0
```

考虑所有可能增强你函数可用性的默认情况通常都是一种好的做法。然而，从阅读与理解函数如何被调用的各种方式上看，默认形式参数可能会导致更多的复杂性。在你有多依赖这个特性的问题上要做出明智的决定。

### 输入计数

一个函数所 “期待” 的实际参数的数量 —— 你可能想向它传递多少实际参数 —— 是由被声明的形式参数的数量决定的。

```js
function foo(x, y, z) {
  // ..
}
```

`foo(..)` _期待_ 三个实际参数，因为它拥有三个被声明的形式参数。这个数量有一个特殊的术语名称：元（arity）。元是函数声明中形式参数的数量。`foo(..)` 的元是 `3`。

此外，一个元为 1 的函数也被称为 “一元（unary）”，元为 2 的函数也被称 “二元（binary）”，而一个元为 3 或更高的函数被称为 “n 元（n-ary）”。

你可能会想在运行时期间检查一个函数引用来判定它的元。这可以通过这个函数引用的 `length` 属性来完成：

```js
function foo(x, y, z) {
  // ..
}

foo.length; // 3
```

一个在执行期间判定元的原因可能是，一段代码从多个源头接受一个函数引用，并且根据每个函数引用的元来发送不同的值。

例如，想象这样一种情况，一个函数引用 `fn` 可能期待一个，两个，或三个实际参数，但你总是想要在最后一个位置上传递变量 `x`：

```js
// `fn` 是某个函数的引用
// `x` 存在并带有某个值

if (fn.length == 1) {
  fn(x);
} else if (fn.length == 2) {
  fn(undefined, x);
} else if (fn.length == 3) {
  fn(undefined, undefined, x);
}
```

**提示：** 一个函数的 `length` 属性是只读的，而且它在你声明这个函数时就已经被决定了。它应当被认为实质上是一种元数据，用来描述这个函数意料之中的用法。

一个要小心的坑是，特定种类的形式参数列表可以使函数的 `length` 属性报告的东西与你期待的不同：

```js
function foo(x, y = 2) {
  // ..
}

function bar(x, ...args) {
  // ..
}

function baz({ a, b }) {
  // ..
}

foo.length; // 1
bar.length; // 1
baz.length; // 1
```

那么如何计数当前函数调用收到的实际参数数量呢？这曾经是小菜一碟，但现在情况变得稍微复杂一些。每个函数都有一个可以使用的 `arguments`（类数组）对象，它持有每个被传入的实际参数的引用。你可检查 `arguments` 的 `length` 属性来搞清楚有多少参数被实际传递了：

```js
function foo(x, y, z) {
  console.log(arguments.length);
}

foo(3, 4); // 2
```

在 ES5（具体地说，strict 模式）中，`arguments` 在某种程度上被认为是废弃了；许多人都尽量避免使用它。在 JS 中，无论对未来的发展有多大帮助，我们都 “绝不会” 破坏向下的兼容性，所以 `arguments` 将永远不会被移除。但是现在通常建议你尽可能地避免使用它。

然而，我建议 `arguments.length`，而且仅有它，在你需要关心被传入的实际参数的数量时是可以继续使用的。某个未来版本的 JS 中很有可能会加入一个特性，在不必查询 `arguments.length` 的情况下判定被传递的实际参数数量；如果这真的发生了，那我们就可以完全放弃 `arguments` 的使用了。

小心：**绝不要** 按位置访问实际参数，比如 `arguments[1]`。只使用 `arguments.length`，而且仅在你必须这么做的情况下。

除非，你如何访问一个在超出被声明的形式参数位置上传入的实际参数？我一会就会回答这个问题；但首先，退一步问你自己，“为什么我想要这么做？”。把这个问题认真地考虑几分钟。

这种情况的发生应该非常少见；在你编写函数时它不应当是你通常所期望的，或者要依靠的东西。如果你发现自己身陷于此，那么就再多花 20 分钟，试着用一种不同的方式来设计这个函数的交互。即使这个参数是特殊的，也给它起个名字。

一个接收不确定数量的实际参数的函数签名称为可变参函数（variadic function）。有些人喜欢这种风格的函数设计，但我想你将会发现 FP 程序员经常想要尽量避免这些。

好了，在这一点上唠叨得够多了。

假定你需要以一种类似数组下标定位的方式来访问实际参数，这可能是因为你正在访问一个没有正式形式参数位置的实际参数。我们该如何做？

ES6 前来拯救！让我们使用 `...` 操作符来声明我们的函数 —— 它有多个名称：“扩散”、“剩余”、或者（我最喜欢的）“聚集”。

```js
function foo(x, y, z, ...args) {
  // ..
}
```

看到形式参数列表中的 `...args` 了吗？这是一种 ES6 声明形式，它告诉引擎去收集（嗯哼，聚集）所有剩余的（如果有的话）没被赋值给命名形式参数的实际参数，并将它们放到名为 `args` 的真正的数组中。`args` 将总是一个数组，即便是空的。但它 **不会** 包含那些已经赋值给形式参数 `x`、`y`、和 `z` 的值，只有被传入的超过前三个值的所有东西。

```js
function foo(x, y, z, ...args) {
  console.log(x, y, z, args);
}

foo(); // undefined undefined undefined []
foo(1, 2, 3); // 1 2 3 []
foo(1, 2, 3, 4); // 1 2 3 [ 4 ]
foo(1, 2, 3, 4, 5); // 1 2 3 [ 4, 5 ]
```

所以，如果你 _真的_ 想要设计一个解析任意多实际参数的函数，就在末尾使用 `...args`（或你喜欢的其他任何名字）。现在，你将得到一个真正的、没有被废弃的、不讨人嫌的数组来访问那些实际参数。

只不过要注意，值 `4` 在这个 `args` 的位置 `0` 上，而不是位置 `3`。而且它的 `length` 值将不会包括 `1`、`2`、和 `3` 这三个值。`...args` 聚集所有其余的东西，不包含 `x`、`y`、和 `z`。

你甚至 _可以_ 在没有声明任何正式形式参数的参数列表中使用 `...` 操作符：

```js
function foo(...args) {
  // ..
}
```

无论实际参数是什么，`args` 现在都是一个完全的实际参数的数组，而且你可以使用 `args.length` 来知道究竟有多少个实际参数被传入了。而且如果你选择这样做的话，你可以安全地使用 `args[1]` 或 `args[317]`。但是，拜托不要传入 318 个实际参数。

#### 实际参数数组

要是你想要传递一个值的数组作为你函数调用的实际参数呢？

```js
function foo(...args) {
  console.log(args[3]);
}

var arr = [1, 2, 3, 4, 5];

foo(...arr); // 4
```

我们的新朋友 `...` 又一次被使用了，但现在不是在形式参数列表中；它也可以在调用点的实际参数列表中使用。在这样的上下文环境中它将拥有相反的行为。在形式参数列表中，我们说它将实际参数 _聚集_ 在一起。在实际参数列表中，它将它们 _扩散_ 开来。所以 `arr` 的内容实际上被扩散为 `foo(..)` 调用的各个独立的实际参数。你能看出这与仅仅传入 `arr` 数组的整个引用有什么不同吗？

顺带一提，多个值与 `...` 扩散是可以穿插的，只要你认为合适：

```js
var arr = [2];

foo(1, ...arr, 3, ...[4, 5]); // 4
```

以这种对称的感觉考虑 `...`：在一个值的列表的位置，它 _扩散_。在一个赋值的位置 —— 比如形式参数列表，因为实际参数被 _赋值给_ 了形式参数 —— 它 _聚集_。

不管你调用哪一种行为，`...` 都令使用实际参数列表变得非常简单。用`slice(..)`、`concat(..)` 和 `apply(..)` 来倒腾我们实际参数值数组的日子一去不复返了。

**提示：** 实际上，这些方法不是完全没用。在这本书的代码中将会有几个地方依赖他们。但是在大多数地方，`...` 自然更具描述性更易读，因此还是首选。

#### 形式参数解构

考虑前一节的可变参函数 `foo(..)`：

```js
function foo(...args) {
  // ..
}

foo(...[1, 2, 3]);
```

要是我们想改变这种互动方式，让我们函数的调用方传入一个值的数组而非各个独立的实际参数值呢？只要去掉这两个 `...` 就好：

```js
function foo(args) {
  // ..
}

foo([1, 2, 3]);
```

这很简单。但如果我们想给被传入的数组的前两个值赋予形式参数名呢？我们不再声明独立的形式参数了，看起来我们失去了这种能力。

谢天谢地，ES6 解构（destructuring）就是答案。解构为你想看到的某种结构（对象，数组等）声明了一个 _范例_，描述应当如何将它分解（分配）为各个独立的部分。

赋值：

<a name="funcparamdestr"></a>

```js
function foo([x, y, ...args] = []) {
  // ..
}

foo([1, 2, 3]);
```

你发现现在形式参数列表周围的方括号 `[ .. ]` 了吗？这被称为形式参数数组解构。

在这个例子中，解构告诉引擎在这个赋值的位置（也就是形式参数）上期待一个数组。范例中说将这个数组的第一个值赋值给称为 `x` 的本地形式参数变量，第二个赋值给 `y`，而剩下的所有东西都 _聚集_ 到 `args` 中。

### 声明式风格的重要性

考虑一下我们刚刚看到的被解构的 `foo(..)`，我们本可以手动地处理形式参数：

```js
function foo(params) {
  var x = params[0];
  var y = params[1];
  var args = params.slice(2);

  // ..
}
```

但是现在我们要强调一个在[第一章](ch1.md/#readability)中仅仅简要引入的原则：声明式代码要比指令式代码在沟通交流上更高效。

声明式代码（例如，前面的 `foo(..)` 代码段中的解构，或者 `...` 操作符的用法）关注于一段代码的结果应当是什么样子。

指令式代码（就像刚才代码段中的手动赋值）关注于如何得到结果。如果稍后再读这段代码，你就不得不在大脑中将它全部执行一遍来得到期望的结果。它的结果被 _编码_ 在这里，但由于被我们 _如何_ 得到它的细节朦胧了双眼而不清晰。

前一个 `foo(..)` 被认为可读性更强，因为解构将不必要的 _如何_ 处理形式参数输入的细节隐藏了起来；读者可以将注意力集中在我们将要对这些形式参数做 _什么_ 上。这明显是最重要的问题，所以这才是读者应当集中去理解的东西。

不论什么地方，也不论我们的语言或库/框架允许我们这样做到多深的程度，**我们都应当努力使用声明式的、自解释的代码。**

## 命名实际参数

正如我们可以解构数组形式参数，我们还可以解构对象形式参数：

```js
function foo({ x, y } = {}) {
  console.log(x, y);
}

foo({
  y: 3,
}); // undefined 3
```

我们将一个对象作为实际参数传入，它被解构为两个分离的形式参数变量 `x` 和 `y`，被传入的对象中具有相应属性名称的值将会赋予这两个变量。对象中不存在 `x` 属性并不要紧；它会如你所想地那样得到一个 `undefined` 变量。

但是在这个形式参数对象解构中我想让你关注的部分是被传入 `foo(..)` 的对象。

像 `foo(undefined,3)` 这样普通的调用点，位置用于将实际参数映射到形式参数上；我们将 `3` 放在第二个位置上使它被赋值给形式参数 `y`。但是在这种引入了形式参数解构的新型调用点中，一个简单的对象-属性指示了哪个形式参数（`y`）应该被赋予实际参数值 `3`。

我们不必在这个调用点中说明 `x`，因为我们实际上不关心 `x`。我们只是省略它，而不是必须去做传入 `undefined` 作为占位符这样令人分心的事情。

有些语言直接拥有这种特性：命名实际参数。换句话说，在调用点中，给一个输入值打上一个标签来指示它映射到哪个形式参数上。JavaScript 不具备命名实际参数，但是形式参数对象解构是最佳替代选项。

使用对象解构传入潜在的多个实际参数 —— 这样做的另一个与 FP 相关的好处是，只接收一个形式参数（那个对象）的函数与另一个函数的单一输出组合起来要容易得多。我们会在[第四章](ch4.md)更多地讲解这一点。

### 无序形式参数

命名实际参数的另一个关键的好处是，它通过作为对象的属性名指定，从根本上是没有顺序的。这意味着我们可以以任意的顺序指定输入：

```js
function foo({ x, y } = {}) {
  console.log(x, y);
}

foo({
  y: 3,
}); // undefined 3
```

我们简单地省略跳过了形式参数 `x`。或者如果我们关心它的话，甚至可以在对象字面量中把它放在 `y` 的后面指定。调用点不再会因为 `undefined` 这样的顺序占位符跳过一个形式参数而搞得乱糟糟了。

命名实际参数要灵活的多，而且从可读性的角度来看很吸引人，特别是在函数可以接受三个、四个或更多的输入时。

**提示：** 如果这种风格的函数形式参数对你很有用，或者你很感兴趣，可以看看我在[附录 C 中的 FPO 库](apC.md/#bonus-fpo)。

回想一下，“元”这个术语指一个函数期待接收多少形式参数。一个元为 1 的函数也被称为一元函数。在 FP 中，我们将尽可能使我们的函数是一元的，而且有时我们甚至会使用各种函数式技巧将一个高元函数转换为一个一元的形式。

**注意：** 在第三章中，我们将重温这种命名实际参数解构技巧，来对付恼人的形式参数顺序问题。

## 函数输出

让我们将注意力从函数的输入转移到它的输出上。

在 JavaScript 中，函数总是返回一个值。这三个函数都拥有完全相同的 `return` 行为：

```js
function foo() {}

function bar() {
  return;
}

function baz() {
  return undefined;
}
```

如果你没有 `return` 或者你仅有一个空的 `return;`，那么 `undefined` 值就会被隐含地 `return`。

但是要尽可能地保遵循 FP 中函数定义的精神 —— 使用函数而不是过程 —— 我们的函数应当总是拥有输出，这意味着它们应当明确地 `return` 一个值，而且通常不是 `undefined`。

一个 `return` 语句只能返回一个单一的值。所以如果你的函数需要返回多个值，你唯一可行的选项是将它们收集到一个像数组或对象这样的复合值中：

```js
function foo() {
  var retValue1 = 11;
  var retValue2 = 31;
  return [retValue1, retValue2];
}
```

然后，我们使用从 `foo()` 中返回的数组分别给 `x` 和 `y` 赋值：

```js
var [x, y] = foo();
console.log(x + y); // 42
```

将多个值收集到一个数组（或对象）中返回，继而将这些值解构回独立的赋值，对于函数来说是一种透明地表达多个输出的方法。

**提示：** 如果我没有这么提醒你，那将是我的疏忽：花点时间考虑一下，一个需要多个输出的函数是否能够被重构来避免这种情况，也许分成两个或更多更小的意图单一的函数？有时候这是可能的，有时候不；但你至少应该考虑一下。

### 提前返回

`return` 语句不仅是从一个函数中返回一个值。它还是一种流程控制结构；它会在那一点终止函数的运行。因此一个带有多个 `return` 语句的函数就拥有多个可能的出口，如果有许多路径可以产生输出，那么这就意味着阅读一个函数来理解它的输出行为可能更加困难。

考虑如下代码：

```js
function foo(x) {
  if (x > 10) return x + 1;

  var y = x / 2;

  if (y > 3) {
    if (x % 2 == 0) return x;
  }

  if (y > 1) return y;

  return x;
}
```

突击测验：不使用浏览器运行这段代码，`foo(2)` 返回什么？`foo(4)` 呢？`foo(8)` 呢？`foo(12)` 呢？

你对自己的答案有多自信？你为这些答案交了多少智商税？我考虑它时，前两次都错了，而且我是用写的！

我认为这里存在的一部分可读性问题是，我们不仅将 `return` 用于返回不同的值，而且还将它作为一种流程控制结构，在特定的情况下提前退出函数的执行。当然有更好的方式编写这种流程控制（例如 `if` 逻辑等等），但我也认为有办法使输出的路径更加明显。

**注意：** 突击测验的答案是 `2`、`2`、`8`、和 `13`。

考虑一下这个版本的代码：

```js
function foo(x) {
  var retValue;

  if (retValue == undefined && x > 10) {
    retValue = x + 1;
  }

  var y = x / 2;

  if (y > 3) {
    if (retValue == undefined && x % 2 == 0) {
      retValue = x;
    }
  }

  if (retValue == undefined && y > 1) {
    retValue = y;
  }

  if (retValue == undefined) {
    retValue = x;
  }

  return retValue;
}
```

这个版本无疑更加繁冗。但我要争辩的是它的逻辑追溯起来更简单，因为每一个 `retValue` 可能被设置的分支都被一个检查它是否已经被设置过的条件 _守护_ 着。

我们没有提前从函数中 `return` 出来，而是使用了普通的流程控制（`if` 逻辑）来决定 `retValue` 的赋值。最后，我们单纯地 `return retValue`。

我并不是在无条件地宣称你应当总是拥有一个单独的 `return`，或者你绝不应该提早 `return`，但我确实认为你应该对 `return` 在你的函数定义中制造隐晦流程控制的部分多加小心。试着找出表达逻辑的最明确的方式；那通常是最好的方式。

### 没有被 `return` 的输出

你可能在你写过的大部分代码中用过，但可能没有太多考虑过的技术之一，就是通过简单地改变函数外部的变量来使它输出一些或全部的值。

记得我们本章早先的 <code>f(x) = 2x<sup>2</sup> + 3</code> 函数吗？我们可以用 JS 这样定义它：

```js
var y;

function f(x) {
  y = 2 * Math.pow(x, 2) + 3;
}

f(2);

y; // 11
```

我知道这是一个愚蠢的例子；我们本可以简单地 `return` 值，而非在函数内部将它设置在 `y` 中：

```js
function f(x) {
  return 2 * Math.pow(x, 2) + 3;
}

var y = f(2);

y; // 11
```

两个函数都完成相同的任务。那么我们有任何理由择优选用其中之一吗？**有，绝对有。**

一个解释它们的不同之处的方式是，第二个版本中的 `return` 标明了一个明确的输出，而前者中的 `y` 赋值是一种隐含的输出。此时你能已经有了某种指引你的直觉；通常，开发者们优先使用明确的模式，而非隐含的。

但是改变外部作用域中的变量，就像我们在 `foo(..)` 内部中对 `y` 赋值所做的，只是得到隐含输出的方式之一。一个更微妙的例子是通过引用来改变非本地值。

考虑如下代码：

```js
function sum(list) {
  var total = 0;
  for (let i = 0; i < list.length; i++) {
    if (!list[i]) list[i] = 0;

    total = total + list[i];
  }

  return total;
}

var nums = [1, 3, 9, 27, , 84];

sum(nums); // 124
```

这个函数最明显的输出是我们明确地 `return` 的和 `124`。但你发现其他的输出了吗？试着运行这代码然后检查 `nums` 数组。现在你发现不同了吗？

现在在位置 `4` 上取代 `undefined` 空槽值的是一个 `0`。看起来无害的 `list[i] = 0` 操作影响了外部的数组值，即便我们操作的是本地形式参数变量 `list`。

为什么？因为 `list` 持有一个 `nums` 引用的引用拷贝，而不是数组值 `[1,3,9,..]` 的值拷贝。因为 JS 对数组，对象，以及函数使用引用和引用拷贝，所以我们可以很容易地从我们的函数中制造不经意的输出。

这种隐含的函数输出在 FP 世界中有一个特殊名称：副作用（side effects）。而一个 _没有副作用_ 的函数也有一个特殊名称：纯函数（pure function）。我们将在[第五章](ch5.md)中更多地讨论这些内容，但要点是，我们将尽一切可能优先使用纯函数并避免副作用。

## 函数的函数

函数可以接收并返回任意类型的值。一个接收或返回一个或多个其他函数的函数有一个特殊的名称：高阶函数（higher-order function）。

考虑如下代码：

```js
function forEach(list, fn) {
  for (let v of list) {
    fn(v);
  }
}

forEach([1, 2, 3, 4, 5], function each(val) {
  console.log(val);
});
// 1 2 3 4 5
```

`forEach(..)` 是一个高阶函数，因为它接收一个函数作为实际参数。

一个高阶函数还可以输出另一个函数，比如：

```js
function foo() {
  return function inner(msg) {
    return msg.toUpperCase();
  };
}

var f = foo();

f("Hello!"); // HELLO!
```

`return` 不是“输出”另一个函数的唯一方法：

```js
function foo() {
  bar(function inner(msg) {
    return msg.toUpperCase();
  });
}

function bar(func) {
  func("Hello!");
}

foo(); // HELLO!
```

高阶函数的定义就是将其他函数看做值的函数。FP 程序员一天到晚都在写这些东西！

### 保持作用域

在一切编程方式 —— 特别是 FP —— 中最强大的东西之一，就是当一个函数位于另一个函数的作用域中时如何动作。当内部函数引用外部函数的一个变量时，这称为闭包（closure）。

实用的定义是:

> 闭包是在一个函数即使在不同的作用域中被执行时，也能记住并访问它自己作用域之外的变量。

考虑如下代码：

```js
function foo(msg) {
  var fn = function inner() {
    return msg.toUpperCase();
  };

  return fn;
}

var helloFn = foo("Hello!");

helloFn(); // HELLO!
```

在 `foo(..)` 的作用域中的形式参数变量 `msg` 在内部函数中被引用了。当 `foo(..)` 被执行，内部函数被创建时，它就会捕获对 `msg` 变量的访问权，并且即使在被 `return` 之后依然保持这个访问权。

一旦我们有了 `helloFn`，一个内部函数的引用，`foo(..)` 已经完成运行而且它的作用域看起来应当已经消失了，这意味着变量 `msg` 将不复存在。但是这没有发生，因为内部函数拥有一个对 `msg` 的闭包使它保持存在。只要这个内部函数（现在在一个不同的作用域中通过 `helloFn` 引用）存在，被闭包的变量 `msg` 就会保持下来。

再让我们看几个闭包在实际中的例子：

```js
function person(name) {
  return function identify() {
    console.log(`I am ${name}`);
  };
}

var fred = person("Fred");
var susan = person("Susan");

fred(); // I am Fred
susan(); // I am Susan
```

内部函数 `identify()` 闭包着形式参数 `name`。

闭包允许的访问权不仅仅限于读取变量的原始值 —— 它不是一个快照而是一个实时链接。你可以更新这个值，而且在下一次访问之前这个新的当前状态会被一直记住。

```js
function runningCounter(start) {
  var val = start;

  return function current(increment = 1) {
    val = val + increment;
    return val;
  };
}

var score = runningCounter(0);

score(); // 1
score(); // 2
score(13); // 15
```

**警告：** 由于我们将在本书稍后深入讲解的一些理由，这种使用闭包来记住改变的状态（`val`）的例子可能是你想要尽量避免的。

如果你有一个操作需要两个输入，你现在知道其中之一但另一个将会在稍后指定，你就可以使用闭包来记住第一个输入：

```js
function makeAdder(x) {
  return function sum(y) {
    return x + y;
  };
}

// 我们已经知道 `10` 和 `37` 都是第一个输入了
var addTo10 = makeAdder(10);
var addTo37 = makeAdder(37);

// 稍后，我们指定第二个输入
addTo10(3); // 13
addTo10(90); // 100

addTo37(13); // 50
```

一般说来，一个 `sum(..)` 函数将会拿着 `x` 和 `y` 两个输入并把它们加在一起。但是在这个例子中我们首先收到并（通过闭包）记住值 `x`，而值 `y` 是在稍后被分离地指定的。

**注意：** 这种在连续的函数调用中指定输入的技术在 FP 中非常常见，而且拥有两种形式：局部应用（partial application）与柯里化（currying）。我们将在[第三章](ch3.md/#some-now-some-later)中更彻底地深入它们。

当然，因为在 JS 中函数只是值，所以我们可以通过闭包来记住函数值。

```js
function formatter(formatFn) {
  return function inner(str) {
    return formatFn(str);
  };
}

var lower = formatter(function formatting(v) {
  return v.toLowerCase();
});

var upperFirst = formatter(function formatting(v) {
  return v[0].toUpperCase() + v.substr(1).toLowerCase();
});

lower("WOW"); // wow
upperFirst("hello"); // Hello
```

与其将 `toUpperCase()` 和 `toLowerCase()` 的逻辑在我们的代码中散布/重复得到处都是，FP 鼓励我们创建封装（encapsulate） —— “包起来”的炫酷说法 —— 这种行为的简单函数。

具体地说，我们创建了两个简单的一元函数 `lower(..)` 和 `upperFirst(..)`，在我们程序的其余部分中，这些函数将会更容易地与其他函数组合起来工作。

**提示：** 你是否发现了 `upperFirst(..)` 本可以使用 `lower(..)` 呢?

我们将在本书的剩余部分重度依赖闭包。如果不谈整个编程世界，它可能是一切 FP 中最重要的基础实践。确保你非常熟悉它！

## 语法

在我们从这个函数的入门教程启程之前，让我们花点儿时间讨论一下它们的语法。

与本书的其他许多部分不同，这一节中的讨论带有最多的个人意见与偏好，不论你是否同意或者反对这里出现的看法。这些想法非常主观，虽然看起来许多人感觉它们更绝对。

不过说到头来，由你决定。

### 名称有何含义？

从语法上讲，函数声明要求包含一个名称：

```js
function helloMyNameIs() {
  // ..
}
```

但是函数表达式可以以命名和匿名两种形式出现：

```js
foo(function namedFunctionExpr() {
  // ..
});

bar(function () {
  // <-- 看，没有名称！
  // ..
});
```

顺便问一下，我们说匿名究竟是什么意思？具体地讲，函数有一个 `name` 属性，它持有这个函数在语法上被赋予的名称的字符串值，比如 `"helloMyNameIs"` 或者 `"namedFunctionExpr"`。这个 `name` 属性最常被用于你的 JS 环境的控制台/开发者工具中，当这个函数存在于调用栈中时（通常是由于异常）将它显示出来。

匿名函数通常被显示为 `(anonymous function)`。

如果你曾经在除了一个异常的调用栈轨迹以外没有任何可用信息的情况下调试 JS 程序，你就可能感受过看到一行接一行的 `(anonymous function)` 的痛苦。对于该异常从何而来，这种列表不会给开发者任何线索。它帮不到开发者。

如果你给你的函数表达式命名，那么这个名称将总是被使用。所以如果你使用了一个像 `handleProfileClicks` 这样的好名字取代 `foo`，那么你将得到有用得多的调用栈轨迹。

在 ES6 中，匿名函数表达式可以被 _名称推断（name inferencing）_ 所辅助。考虑如下代码：

```js
var x = function () {};

x.name; // x
```

如果引擎能够猜测你 _可能_ 想让这个函数叫什么名字，它就会立即这么做。

但要小心，不是所有的语法形式都能从名称推断中受益。函数表达式可能最常出现的地方就是作为一个函数调用的实际参数：

```js
function foo(fn) {
  console.log(fn.name);
}

var x = function () {};

foo(x); // x
foo(function () {}); //
```

当从最近的外围语法中无法推断名称时，它会保留一个空字符串。这样的函数将会在调用栈轨迹中报告为 `(anonymous function)`。

除了调试的问题之外，被命名的函数还有其他的好处。首先，语法名称（也叫词法名称）对于内部自引用十分有用。自引用对于递归（同步和异步）来说是必要的，在事件处理器中也十分有帮助。

考虑这些不同的场景：

```js
// 同步递归：
function findPropIn(propName, obj) {
  if (obj == undefined || typeof obj != "object") return;

  if (propName in obj) {
    return obj[propName];
  } else {
    for (let prop of Object.keys(obj)) {
      let ret = findPropIn(propName, obj[prop]);
      if (ret !== undefined) {
        return ret;
      }
    }
  }
}
```

```js
// 异步递归
setTimeout(function waitForIt() {
  // `it` 还存在吗？
  if (!o.it) {
    // try again later
    setTimeout(waitForIt, 100);
  }
}, 100);
```

```js
// 解除事件处理器绑定
document.getElementById("onceBtn").addEventListener(
  "click",
  function handleClick(evt) {
    // 解除事件绑定
    evt.target.removeEventListener("click", handleClick, false);

    // ..
  },
  false
);
```

在所有这些情况下，命名函数的名称都是它内部的一个有用且可靠的自引用。

另外，即使是在一个一行函数的简单情况下，将它们命名也会使代码更具自解释性，因此使代码对于那些以前没有读过它的人来说变得更易读：

```js
people.map(function getPreferredName(person) {
  return person.nicknames[0] || person.firstName;
});
// ..
```

函数名 `getPreferredName(..)` 告诉读者映射操作的意图是什么，而这仅从代码来看的话没那么明显。这个名称标签使得代码更具可读性。

另一个匿名函数表达式常见的地方是 IIFE（即时调用的函数表达式）：

```js
(function () {
  // 看，我是一个 IIFE！
})();
```

你几乎永远看不到 IIFE 为它们的函数表达式使用名称，但它们应该这么做。为什么？为了我们刚刚讲过的所有理由：调用栈轨迹调试、可靠的自引用、与可读性。如果你实在想不出任何其他名称，至少要使用 IIFE 这个词：

```js
(function IIFE() {
  // 你已经知道我是一个 IIFE 了！
})();
```

我的意思是有多种理由可以解释为什么 **命名函数总是优于匿名函数。** 事实上，我甚至可以说基本上不存在匿名函数更优越的情况。对于命名的另一半来说它们根本没有任何优势。

编写匿名函数不可思议地容易，因为那样会减少一个让我们投入精力找出的名称。

我承认；我和所有人一样有罪。我不喜欢在命名上挣扎。我想到的头个名称通常都很差劲。我不得不一次又一次地重新考虑命名。我宁愿撒手不管而使用匿名函数表达式。

但我们是在用好写与难读做交易。这不是一桩好买卖。由于懒惰或没有创意而不想为你的函数找出名称，是一个使用匿名函数的太常见，但很烂的借口。

**为每个函数命名。** 如果你坐在那里很为难，不能为你写的某个函数想出一个好名字，那么我会强烈地感觉到你还没有完全理解这个函数的目的 —— 或者它的目的太泛泛或太抽象了。你需要回过头去重新设计这个函数，直到它变得更清晰。而到了那个时候，一个名称将显而易见。

我的做法是，如果我无法给函数起一个好名字，我会最初给它命名为 `TODO`。我确定自己会在稍后提交代码之前搜索 “TODO” 注释的时候处理它。

我可以用我的经验作证，在给某个东西良好命名的挣扎中，我通常对它有了更好的理解，甚至经常为了改进可读性和可维护性而重构它的设计。

这种时间上的投资是值得的。

### 没有 `function` 的函数

至此我们一直在使用完全规范的函数语法。但毫无疑问你也听说过关于新的 ES6 `=>` 箭头函数语法的讨论。

比较一下：

```js
people.map(function getPreferredName(person) {
  return person.nicknames[0] || person.firstName;
});
// ..

people.map((person) => person.nicknames[0] || person.firstName);
```

哇哦。

关键词 `function` 不见了，`return`、括号（`( )`）、花括号（`{ }`）、和引号（`;`）也不见了。所有这些，换来了所谓的大箭头符号（`=>`）。

但这里我们省略了另一个东西。你发现了吗？函数名 `getPreferredName`。

没错；`=>` 箭头函数是词法上匿名的；没有办法在语法上给它提供一个名称。它们的名称可以像普通函数那样被推断，但同样地，在最常见的函数表达式作为实际参数的情况下它帮不上什么忙。

如果由于某些原因 `person.nicknames` 没有被定义，一个异常被抛出，这意味着 `(anonymous function)` 将会位于调用栈轨迹的顶端。呃。

老实说，对我而言，`=>` 箭头函数的匿名性是一把指向心脏的 `=>` 匕首。我无法忍受命名的缺失。它更难读、更难调试、而且不可能进行自引用。

如果说这还不够坏，那另一个打脸的地方是，如果你的函数定义有不同的场景，你就必须趟过一大堆有微妙不同的语法。我不会在这里涵盖它们的所有细节，但简单地说：

```js
people.map((person) => person.nicknames[0] || person.firstName);

// 多个形式参数？需要 ( )
people.map((person, idx) => person.nicknames[0] || person.firstName);

// 形式参数解构？需要 ( )
people.map(({ person }) => person.nicknames[0] || person.firstName);

// 形式参数默认值？需要 ( )
people.map((person = {}) => person.nicknames[0] || person.firstName);

// 返回一个对象？需要 ( )
people.map((person) => ({
  preferredName: person.nicknames[0] || person.firstName,
}));
```

在 FP 世界中 `=>` 激动人心的地方主要在于它几乎完全符合数学上函数的符号，特别是在像 Haskell 这样的 FP 语言中。箭头函数语法 `=>` 的形状可以进行数学上的交流。

再挖深一些，我觉得支持 `=>` 的争辩是，通过使用轻量得多的语法，我们减少了函数之间的视觉边界，这允许我们像曾经使用懒惰表达式那样使用简单的函数表达式 —— 这是另一件 FP 程序员们最喜欢的事。

我想大多数 FP 程序员将会对我在意的这些问题不屑一顾。他们深爱着匿名函数，也爱简洁的语法。但正如我之前说的：这由你来决定。

**注意：** 虽然在实际中我不喜欢在我的生产代码中使用 `=>`，但它们对快速探索代码很有用。另外，我们将会在本书剩余部分的许多地方使用箭头函数 —— 特别是当我们展示常用的 FP 工具时 —— 当简洁性在代码段有限的物理空间中成为不错的优化方式时。这种方式是否会使你即将用于生产的代码可读性提高或降低，你要做出自己的决断。

## This 是什么？

如果你对 JavaScript 中的 `this` 绑定规则不熟悉，我推荐你看看我的 “你不懂 JS：this 与对象原型” 一书。对于这一节的目的来说，我假定你知道在一个函数调用中 `this` 是如何被决定的（四种规则之一）。但就算你对 _this_ 还不甚了解，好消息是我们会得出这样的结论：如果你想使用 FP，那么你就不应当使用 `this`。

**注意：** 我们正在讨论一个最终推论为不应使用的话题。为什么！？因为 `this` 的话题对本书后面涵盖的其他话题有影响。例如，函数纯粹性的概念受到 `this` 实质上是函数的一个隐含输入的影响（见[第五章](ch5.md)）。另外，我们对 `this` 的看法会影响我们是否选择使用数组方法（`arr.map(..)`）还是独立工具（`map(..,arr)`）（见[第九章](ch9.md)）。理解 `this` 实质上是要理解为什么 `this` 确实 _不_ 应成为你 FP 的一部分！

JavaScript 的 `function` 拥有一个在每次函数调用时自动绑定的 `this` 关键字。这个 `this` 关键字可以用许多不同的方式描述，但我喜欢的说法是它为函数的运行提供了一个对象上下文环境。

对于你的函数来说，`this` 是一个隐含形式参数输入。

考虑如下代码：

```js
function sum() {
  return this.x + this.y;
}

var context = {
  x: 1,
  y: 2,
};

sum.call(context); // 3

context.sum = sum;
context.sum(); // 3

var s = sum.bind(context);
s(); // 3
```

当然，如果 `this` 可以是一个函数的隐含输入，那么相同的对象环境就可以作为明确的实际参数发送：

```js
function sum(ctx) {
  return ctx.x + ctx.y;
}

var context = {
  x: 1,
  y: 2,
};

sum(context);
```

更简单。而且这种代码在 FP 中处理起来容易得多。当输入总是明确的时候，将多个函数组合在一起，或者使用我们将在下一章中学到的其他搬弄输入的技术都将简单得多。要使这些技术与 `this` 这样的隐含输入一起工作，在不同场景下要么很尴尬要么就是几乎不可能。

我们可以在一个基于 `this` 的系统中利用其他技巧，例如原型委托（也在 “你不懂 JS：this 与对象原型” 一书中有详细讲解）：

```js
var Auth = {
  authorize() {
    var credentials = `${this.username}:${this.password}`;
    this.send(credentials, (resp) => {
      if (resp.error) this.displayError(resp.error);
      else this.displaySuccess();
    });
  },
  send(/* .. */) {
    // ..
  },
};

var Login = Object.assign(Object.create(Auth), {
  doLogin(user, pw) {
    this.username = user;
    this.password = pw;
    this.authorize();
  },
  displayError(err) {
    // ..
  },
  displaySuccess() {
    // ..
  },
});

Login.doLogin("fred", "123456");
```

**注意：** `Object.assign(..)` 是一个 ES6+ 工具，用于从一个或多个源对象向一个目标对象进行属性的浅赋值拷贝：`Object.assign( target, source1, ... )`。

如果你解读这段代码有困难：我们有两个分离的对象 `Login` 和 `Auth`，`Login` 实施了向 `Auth` 的原型委托。通过委托与隐含的 `this` 上下文环境共享，这两个对象在 `this.authorize()` 函数调用中被虚拟地组合在一起，这样在 `Auth.authorize(..)` 函数中 `this` 上的属性/方法被动态地共享。

由于各种原因这段代码不符合 FP 的种种原则，但是最明显的问题就是隐含的 `this` 共享。我们可以使它更明确一些，保持代码可以更容易地向 FP 的方向靠拢：

```js
// ..

authorize(ctx) {
    var credentials = `${ctx.username}:${ctx.password}`;
    Auth.send( credentials, function onResp(resp){
        if (resp.error) ctx.displayError( resp.error );
        else ctx.displaySuccess();
    } );
}

// ..

doLogin(user,pw) {
    Auth.authorize( {
        username: user,
        password: pw
    } );
}

// ..
```

在我看来，这其中的问题并不是使用了对象来组织行为。而是我们试图使用隐含输入取代明确输入。当我带上我的 FP 帽子时，我会想将 `this` 这东西留在衣架上。

## 总结

函数十分强大。

但我们要清楚什么是函数。它不只是一个语句/操作的集合。具体地说，一个函数需要一个或多个输入（理想情况，只有一个！）以及一个输出。

函数内部的函数可以拥有外部变量的闭包，为稍后的访问记住它们。这是所有种类的编程中最重要的概念之一，而且是 FP 基础中的基础。

要小心匿名函数，特别是箭头函数 `=>`。它们写起来方便，但是将作者的成本转嫁到了读者身上。我们学习 FP 的所有原因就是写出可读性更强的代码，所以先不要那么快就赶这个潮流。

不要使用 `this` 敏感的函数。别这么干。

现在你应当在思想中对 _函数_ 在函数式编程中意味着什么发展出了清晰而多彩的观点。是时候搬弄函数以使它们协作的时候了，下一章将会教授各种你在路上需要的关键技术。
