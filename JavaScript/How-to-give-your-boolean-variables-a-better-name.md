# How to give your boolean variables a better name 👏

**怎样给 boolean 变量起一个好名字**

> 转译自：https://www.samanthaming.com/tidbits/34-better-boolean-variable-names

Coming up with good variable names can always be a challenge. So there is some convention you can follow that makes the process easier. For boolean values, you can simply prefix it with `is`, `has`, or `can`. Just by reading the name, you can easily infer that this variable will give you boolean value. Awesome! 👍

想出好的变量名一直是个挑战。所以你可以遵循一些惯例来简化这个过程。对于 boolean 变量，可以简单地在前面加上 `is`、`has` 或`can`。仅通过名称，就可以很容易地推断出该变量是 boolean 变量。这么做太棒了！

```js
// Better Boolean Variable Names

// ❌ bad
let person = true;
let age = true;
let dance = true;

// ✅ Prefix with: is, has, can
let isPerson = true;
let hasAge = true;
let canDance = true;
```

## Why Variable Names Matter?

**为什么变量名很重要？**

Having a well named variable is definitely one of the most important thing for code readability. A good variable name should also provide meaning. That way it’s easy for others to read your code or even yourself in the future. I’m sure we all had the frustration of going back to our code and wonder what the heck is variable “adt”. Don’t ask me, I don’t even know. Acronym is probably the worst. Unless, it’s something super common like DVD.

对于代码可读性来说，拥有一个命名良好的变量名无疑是最重要的事情之一。好的变量名还应该见名知意。如此别人就很容易阅读你的代码，甚至也便于将来你自己也能理解。我敢肯定，我们都有类似挫折，回顾我们的代码，并想知道什么是变量 adt。别问我，我都不知道。首字母缩写词可能是最差的命名。除非是很普遍的名词，如 DVD。

## Bad Variable Names to Avoid

**要避免的糟糕的变量名**

You should be descriptive with your naming. So make sure you avoid these bad variable names.

你的命名应该是描述性的。所以一定要避免这些糟糕的变量名。

Avoid these bad variable names:

避免这些糟糕的变量名：

- ❌ single letter names

单个字母

- ❌ acronyms

首字母缩写词

- ❌ abbreviations

缩写

- ❌ meaningless names

无意义名称

```js
// Avoid Single Letter Names

let h; // 😱  huh, what's h??

// Avoid Acronyms

let cra; // I bet you have no idea what this is unless you're from Canada 🇨🇦

// Avoid Abbreviations

let categ; // Sure we can deduce you're saying category here, but let's just used the full name, so it's not a guessing game 😜

// Avoid Meaningless Names

let foo; // what is foo? 🧐
```

## Community Feedback

**社区反馈**

@thecodercoder: Good advice! Descriptive names are way better. I used to try to keep names as short as possible, but realized that they really need to explain what they are!

好建议！有描述性的名字更好。我曾经试着让名字尽量简短，但我意识到真的需要通过名称解释他们是什么！

@tirpus_hahs: It is very important to name variables what it describes so we don't have to comment out code. This allows us to read code like a story telling.

将变量命名为它所描述的内容是非常重要的，这样我们就不必注释代码。允许我们像阅读故事一样阅读代码。

@\_\_offblackYeah: my booleans go to the level of "isAnimatedWhenNotInViewport", "isScrollPositionGreaterThanTolerance" lol

我的 boolean 变量会到 isAnimatedWhenNotInViewport、isScrollPositionGreaterThanTolerance 这种级别，哈哈

@masonhale: Good suggestion. I also use ‘do’ as a prefix for Boolean settings/preferences as in `doSendReminder` or `doShowDetails`

好建议。我也使用 do 作为 Boolean 变量设置/首选项的前缀，例如 `doSendReminder` 和 `doShowDetails`

@ben336: This is good advice. Also avoid negative variable names like isNotLoaded. The positive forms are almost always clearer

这是一个很好的建议。还要避免使用否定语气的变量名，如 isNotLoaded。肯定语气的形式更清晰

@sunnysinghio: What do you think about `handleValidateForm` considering it's an event handler? It's a more common practice that I've seen, albeit longer than `did`.

将 `handleValidateForm` 视为一个事件处理程序，你对此有何看法？这是我见过的最常见的做法，尽管比 `did` 的时间要长。

@styfle: I use handleClick for the function that will handle it and onClick for the property name which typically just passes it through

我使用 handleClick 作为处理函数，使用 onClick 作为属性名传递

```html
<form url="example.example/post" onClick="{handleClick}" />
```

@maxstalker: What are your thoughts on adjectives

你对形容词有什么看法

```js
const sortable = true; // instead of canBeStorted
const hidden = true; // instead os ifShown
```

youthoverturn:

```
is/has + noun
enable/disable + verb
```

## Resources

- [Be Expressive: How to Give Your Variables Better Names](https://spin.atomicobject.com/2017/11/01/good-variable-names/)
- [The art of naming variables](https://hackernoon.com/the-art-of-naming-variables-52f44de00aad)
- [The Importance Of Naming In Programming](https://carlalexander.ca/importance-naming-programming/)
- [More on JavaScript Variable Naming Conventions](https://www.htmlgoodies.com/html5/javascript/back-by-popular-demand-more-on-javascript-variable-naming-conventions.html)
- [JavaScript naming convention](http://trungk18.github.io/experience/javascript-naming-convention/)
- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)
- [W3School: JavaScript Coding Conventions](https://www.w3schools.com/js/js_conventions.asp)
