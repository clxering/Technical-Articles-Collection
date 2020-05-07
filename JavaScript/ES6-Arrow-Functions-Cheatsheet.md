# ES6 Arrow Functions Cheatsheet

**ES 6 箭头函数问题的备忘**

> 转译自：https://www.samanthaming.com/tidbits/47-arrow-functions-cheatsheet

Here’s a cheatsheet to show you the many ways to write your arrow functions.

这是一个备忘单，展示了许多写箭头函数的方法。

```js
// Explicit Return, Multi-Line
a => {
  return a
}

// Explicit Return, Single-Line
a => { return a }

// Implicit Return, Multi-line
a => (
  a
)

// Implicit Return, Single-Line
a => a

// Multiple Parameters, Parentheses Required
(a, b) => a, b
```

## Implicit vs Explicit Return

**隐式与显式返回**

We have several ways of writing our arrow functions. This is because arrow functions can have either "implied return" or "explicit return" keyword.

我们有几种写箭头函数的方法。这是因为箭头函数可以有 隐式返回 或 显式返回 关键字。

With normal functions, if you want to return something, you have to use the `return` keyword. Arrow functions also have that. When you use the `return` keyword, it's called an **explicit return**. However, arrow functions up their game and allow something called **implied return** where the `return` keyword can be skipped. Let's look at some examples 🤓:

对于普通函数，如果你想要返回某些内容，必须使用 `return` 关键字。箭头函数也有。当你使用 `return` 关键字时，它被称为 **显式返回**。然而，箭头函数有自己的玩法，允许所谓的 **隐式返回**，其中 `return` 关键字可以忽略。让我们看一些例子：

## Example A: Normal Function

```js
const sayHi = function(name) {
  return name
}
```

## Example B: Arrow Function with Explicit Return

```js
// Multi-line
const sayHi = (name) => {
  return name
}

// Single-line
const sayHi = (name) => { return name }
```

## Example C: Arrow Function with Implicit Return

```js
// Single-line
const sayHi = (name) => name

// Multi-line
const sayHi = (name) => (
  name
)
```

Notice the difference? When you use curly braces `{}`, you need to explicitly state the return. However, when you don't use curly braces, the `return` is implied and you don't need it.

注意到区别了吗？当使用大括号 `{}` 时，需要显式地声明返回。但是，当不使用花括号时，`return` 就会被隐藏，你可以不需要它。

There's actually a name for this. When you use curly braces like in Example b, it's called a `block body`. And the syntax in Example c is called a `concise body`.

这个有个名字。当你像在例子 B 中那样使用花括号时，它被称为 `block body`。例子 C 中的语法被称为 `concise body`。

⭐️ Here are the rules:

规则：

- Block body ➡️ `return` keyword is required
- Concise body ➡️ `return` keyword is implied and not needed

## Parentheses

**括号**

With a normal function, we always had to use parentheses. However, with Arrow Functions, parentheses are optional if there is ONLY one parameter.

对于普通函数，我们总是要用括号。但是，对于箭头函数，如果只有一个参数，括号是可选的。

## Parentheses are optional for a SINGLE parameter

```js
// Normal Function
const numbers = function(one) {}

// Arrow Function, with parentheses
const numbers = (one) => {}

// Arrow Function, without parentheses
const numbers = one => {}
```

## Parentheses are required for MULTIPLE parameters

```js
// Normal Function
const numbers = function(one, two) {}

// Arrow Function, with parentheses
const numbers = (one, two) => {}
```

## ⚠️ Arrow Functions Gotcha: Returning Objects

**箭头函数的问题：返回对象**

Remember I mentioned about the different body types - concise body and block body. Just to quickly update you in case you skipped that section (I'm a bit sad, but not offended 😝). Block body is where you use curly braces and have an explicit `return`. Concise body is where you don't use curly braces, and you skip the `return` keyword. Alright, now you're caught up, let's get back to the gotcha 🤯

我提到过不同的函数体类型（concise body 和 block body）。如果你跳过了这一部分，我就快速回顾一下（我有点难过，但不觉得是冒犯）。block body 是使用花括号并有一个明确的 `return`。concise body 是不使用花括号，忽略 `return` 关键字。好了，现在你抓住重点了，让我们回到问题上来。

Let's purposely break our code, so you can learn your lesson lol 😂

让我们故意破坏代码，这样就可以学到了。

```js
const me = () => { name: "samantha" };

me(); // undefined 😱
```

What?! Why isn't it returning my object. Don't worry, let's fix it by wrapping it in parentheses.

什么？！为什么它不返回预期的对象。别担心，让我们用括号把它括起来。

```js
const me = () => ({ name: "samantha" });

me(); // { name: "samantha" } ✅
```

⭐️ Here's the rule:

- For a concise body, wrap object literal in parentheses

对于 concise body，要把对象包裹在括号中

## Resources

- [MDN Web Docs - Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [JavaScript Arrow Function Return Rules](https://jaketrent.com/post/javascript-arrow-function-return-rules/)

