# 5 Best Practices to Write Quality Arrow Functions

**编写高质量箭头函数的五个最佳实践**

> 转译自：https://dmitripavlutin.com/javascript-arrow-functions-best-practices/

The arrow function deserves the popularity. Its syntax is concise, binds this lexically, fits great as a callback.

箭头函数理应受欢迎。它的语法很简洁，以词法绑定绑定 this，非常适合作为回调。

In this post, you’ll read 5 best practices to get even more benefits from the arrow functions.

在这篇文章中，你将阅读到五个最佳实践，以便从箭头函数中获得更多的好处。

## 1. Arrow function name inference

**箭头函数名推断**

The arrow function in JavaScript is anonymous: the name property of the function is an empty string ''.

JavaScript 中的箭头函数是匿名的：函数的 name 属性是一个空字符串 `''`。

```js
( number => number + 1 ).name; // => ''
```

The anonymous functions are marked as anonymous during a debug session or call stack analysis. Unfortunately, anonymous gives no clue about the code being executed.

在调试或调用堆栈分析期间，匿名函数被标记为匿名。不幸的是，对于正在执行的代码，匿名意味着不会给出任何提示。

Here’s a debug session of a code that executes anonymous functions:

下面是执行匿名函数的代码的调试：

![5-Best-Practices-to-Write-Quality-Arrow-Functions-1](/pic/5-Best-Practices-to-Write-Quality-Arrow-Functions-1.png)

The call stack on the right side consists of 2 functions marked as anonymous. You can’t get anything useful from such call stack information.

右边的调用堆栈由两个标记为匿名的函数组成。你无法从这样的调用堆栈信息中获得任何有用的信息。

Fortunately, the function name inference (a feature of ES2015) can detect the function name under certain conditions. The idea of name inference is that JavaScript can determine the arrow function name from its syntactic position: e.g. from the variable name that holds the function object.

幸运的是，函数名称推断（ES2015 的一个特性）可以在特定条件下检测函数名称。名称推断的思想是 JavaScript 可以从其语法位置确定箭头函数名：例如，保存匿名函数的对象变量名。

Let’s see how function name inference works:

让我们看看函数名称推断如何工作：

```js
const increaseNumber = number => number + 1;

increaseNumber.name; // => 'increaseNumber'
```

Because the variable increaseNumber holds the arrow function, JavaScript decides that 'increaseNumber' could be a good name for that function. Thus the arrow function receives the name 'increaseNumber'.

因为变量 increenumber 保存了箭头函数，所以 JavaScript 决定使用 increenumber 作为该函数的好名称。因此，箭头函数的名称为 increenumber。

> A good practice is to use function name inference to name the arrow functions.

一个好的实践是使用函数名称推断来命名箭头函数。

Now let’s check a debug session with code that uses name inference:

现在让我们查看一个代码调试案例，它使用了名称推断：

![5-Best-Practices-to-Write-Quality-Arrow-Functions-2](/pic/5-Best-Practices-to-Write-Quality-Arrow-Functions-2.png)

Because the arrow functions have names, the call stack gives more information about the code being executed:

因为箭头函数有名字，调用堆栈提供了更多关于正在执行的代码的信息:

- handleButtonClick function name indicates that a click event had happened

handleButtonClick 函数名表示发生了单击事件

- increaseCounter increases a counter variable.

increaseCounter 用于增加 counter 变量

## 2. Inline when possible

**尽可能使用内联形式**

An inline function is a function that has only one expression. I like about arrow functions the ability to compose short inline functions.

内联形式的函数是只有一个表达式的函数。我喜欢关于箭头函数的能力，编写短内联形式的函数。

For example, instead of using the long form of an arrow function:

例如，不使用长形式的箭头函数：

```js
const array = [1, 2, 3];

array.map((number) => {
  return number * 2;
});
```

You could easily remove the curly braces { } and return statement when the arrow function has one expression:

当箭头函数有一个表达式时，你可以很容易地删除花括号 {} 和返回语句：

```js
const array = [1, 2, 3];

array.map(number => number * 2);
```

Here’s my advice:

我的建议：

> When the function has one expression, a good practice is to inline the arrow function.

当函数有一个表达式时，好的做法是使用内联形式的箭头函数。

## 3. Fat arrow and comparison operators

The comparison operators >, <, <= and >= look similar to the fat arrow => (which defines the arrow function).

比较操作符 >、<、<= 和 >= 看起来类似于胖箭头 =>（它定义了箭头函数）。

When these comparison operators are used in an inline arrow function, it creates some confusion.

当在内联形式的箭头函数中使用这些比较操作符时，会产生一些混淆。

Let’s define an arrow function that uses <= operator:

让我们定义一个使用 <= 操作符的箭头函数：

```js
const negativeToZero = number => number <= 0 ? 0 : number;
```

The presence of both symbols => and <= on the same line is misleading.

在同一行中同时出现 => 和 <= 是有误导性的。

To distinguish clearly the fat arrow from the comparison operator, the first option is to wrap the expression into a pair of parentheses:

为了明确区分胖箭头和比较运算符，第一个做法是将表达式括在一对括号中：

```js
const negativeToZero = number => (number <= 0 ? 0 : number);
```

The second option is to deliberately define the arrow function using a longer form:

第二个选项是使用更长的形式来定义箭头函数：

```js
const negativeToZero = number => {
  return number <= 0 ? 0 : number;
};
```

These refactorings eliminate the confusion between fat arrow symbol and comparison operators.

这些重构消除了胖箭头符号和比较操作符之间的混淆。

> If the arrow function contains the operators >, <, <= and >=, a good practice is to wrap the expression into a pair of parentheses or deliberately use a longer arrow function form.

如果箭头函数包含操作符 >、<、<= 和 >=，一个好的做法是将表达式包装成一对括号，或者特意使用更长的箭头函数形式。

## 4. Constructing plain objects

**构建简单的对象**

An object literal inside an inline arrow function triggers a syntax error:

内联形式的箭头函数中的对象文字会触发语法错误：

```js
const array = [1, 2, 3];

// throws SyntaxError!
array.map(number => { 'number': number });
```

JavaScript considers the curly braces a code block, rather than an object literal.

JavaScript 认为花括号是一个代码块，而不是一个对象文字。

Wrapping the object literal into a pair of parentheses solves the problem:

将对象文字用一对圆括号包装就解决了这个问题：

```js
const array = [1, 2, 3];

// Works!
array.map(number => ({ 'number': number }));
```

If the object literal has lots of properties, you can even use newlines, while still keeping the arrow function inline:

如果对象文字有很多属性，你甚至可以使用换行，同时仍然保持箭头函数内联形式：

```js
const array = [1, 2, 3];

// Works!
array.map(number => ({
  'number': number
  'propA': 'value A',
  'propB': 'value B'
}));
```

My recommendation:

我的建议：

> Wrap object literals into a pair of parentheses when used inside inline arrow functions.

当在内联形式的箭头函数中使用对象时，将对象文字用一对圆括号包装。

## 5. Be aware of excessive nesting

**注意过度的嵌套**

The arrow function syntax is short, which is good. But as a side effect, it could be cryptic when many arrow functions are nested.

箭头函数语法很短，这很好。但是也有一个副作用，当许多箭头函数嵌套在一起时，它的可读性可能是模糊的。

Let’s consider the following scenario. When a button is clicked, a request to server starts. When the response is ready, the items are logged to console:

让我们考虑以下场景。当单击按钮时，将启动对服务器的请求。当响应准备好后，记录到控制台：

```js
myButton.addEventListener('click', () => {
  fetch('/items.json')
    .then(response => response.json())
    .then(json => {
      json.forEach(item => {
        console.log(item.name);
      });
    });
});
```

The arrow functions are 3 levels nesting. It takes effort and time to understand what the code does.

这个箭头函数是三层嵌套。理解代码的作用需要时间成本。

To increase readability of nested functions, the first approach is to introduce variables that each holds an arrow function. The variable should describe concisely what the function does (see arrow function name inference best practice).

为了增加嵌套函数的可读性，第一种方法是引入变量，每个变量都包含一个箭头函数。变量应该简洁地描述函数的功能（参见箭头函数名称推断最佳实践）。

```js
const readItemsJson = json => {
  json.forEach(item => console.log(item.name));
};

const handleButtonClick = () => {
  fetch('/items.json')
    .then(response => response.json())
    .then(readItemsJson);
};

myButton.addEventListener('click', handleButtonClick);
```

The refactoring extracts the arrow functions into variables readItemsJson and handleButtonClick. The level of nesting decreases from 3 to 2. Now, it’s easier to understand what the script does.

重构将箭头函数提取为变量 readItemsJson 和 handleButtonClick。嵌套的数量从三个减少到两个。现在，更容易理解脚本的作用。

Even better you could refactor the entire function to use async/await syntax, which is a great way to solve the nesting of functions:

更好的做法是，你可以使用 async/await 语法重构整个函数，这是解决函数嵌套的好方法：

```js
const handleButtonClick = async () => {
  const response = await fetch('/items.json');
  const json = await response.json();
  json.forEach(item => console.log(item.name));
};

myButton.addEventListener('click', handleButtonClick);
```

Resuming:

继续上一个建议：

> A good practice is to avoid excessive nesting of arrow functions by extracting them into variables as separate functions or, if possible, embrace async/await syntax.

一个好的实践是通过将箭头函数作为单独的函数，并提取到变量中来避免过多的嵌套，或者，如果可能的话，采用 async/await 语法。

## 6. Conclusion

**结论**

The arrow functions in JavaScript are anonymous. To make debugging productive, a good practice is to use variables to hold the arrow functions. This allows JavaScript to infer the function names.

JavaScript 中的箭头函数是匿名的。为了使调试更高效，一个好的实践是使用变量来保存箭头函数。这允许 JavaScript 推断函数名。

An inline arrow function is handy when the function body has one expression.

当函数体有一个表达式时，使用内联形式的箭头函数很方便。

The operators >, <, <= and >= look similar to the fat arrow =>. Care must be taken when these operators are used inside inline arrow functions.

操作符 >、<、<= 和 >= 看起来类似于胖箭头 =>。在内联形式的箭头函数中使用这些操作符时，必须小心。

The object literal syntax { prop: 'value' } is similar to a code of block { }. So when the object literal is placed inside an inline arrow function, you need to wrap it into a pair of parentheses: () => ({ prop: 'value' }).

对象的字面语法 `{prop: 'value'}` 类似于块 {} 的代码。因此，当对象文字被放置在内联形式的箭头函数中时，需要将其包装在一对圆括号中：`()=> ({prop: 'value'})`。

Finally, the excessive nesting of functions obscures the code intent. A good approach to reduce the arrow functions nesting is to extract them into variables. Alternatively, try to use even better features like async/await syntax.

最后，函数的过度嵌套降低了代码的可读性。减少箭头函数嵌套的一个好方法是将它们提取到变量中。或者，尝试使用更好的特性，比如 async/await 语法。