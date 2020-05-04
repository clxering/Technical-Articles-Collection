# 2 Ways to Merge Arrays in JavaScript

**JavaScript 中合并数组的两种方法**

> 转译自：https://www.samanthaming.com/tidbits/49-2-ways-to-merge-arrays

Here are 2 ways to combine your arrays and return a NEW array. I like using the Spread operator. But if you need older browser support, you should use Concat.

有两种方法合并数组。我喜欢使用**扩展运算符**。但是如果需要兼容老式浏览器，应该使用 Concat

```js
// 2 Ways to Merge Arrays

const cars = ["🚗", "🚙"];
const trucks = ["🚚", "🚛"];

// Method 1: Concat
const combined1 = [].concat(cars, trucks);

// Method 2: Spread
const combined2 = [...cars, ...trucks];

// Result
// [ '🚗', '🚙', '🚚', '🚛' ]
```

## Alternative Concat Syntax

**用 Concat 语法作为替代**

Alternatively, you can also write the `concat` method, in this regard:

或者，你也可以用 `concat` 方法：

```js
const cars = ["🚗", "🚙"];
const trucks = ["🚚", "🚛"];

const combined = cars.concat(trucks);
// [ '🚗', '🚙', '🚚', '🚛' ]

console.log(cars); // ['🚗', '🚙'];
console.log(trucks); // ['🚚', '🚛'];
```

As you can see, this way of writing it doesn't manipulate or change the existing array.

正如所看到的，这种方式不更改原有的数组。

## Which one should I pick?

**应该采用哪一种？**

Let's list out both versions, so you can see it in comparison.

让我们列出两种版本，以便比较。

```js
// Version A:
const combinedA = [].concat(cars, trucks);

// Version B:
const combinedB = cars.concat(trucks);
```

So now the question is, which one should I pick 🤔. I prefer **Version A** because I think the intention is a lot more clear. Just by looking at it, I know I'm creating a new array and I'm not manipulating the existing array. Whereas if I look at **Version B**, it appears like I'm adding the `trucks` array to the `cars` array, and it doesn't seem obvious to me that the `cars` array isn't being changed. But, maybe that's just me. I'd be curious to know what you think?

现在的问题是，我应该选择哪一个。我更喜欢 **版本 A**，我认为意图更清晰。仅仅通过查看它，我就知道正在创建一个新的数组，而不是在操作现有的数组。然而，如果我查看 **版本 B**，它看起来就像我将 `trucks` 数组添加到 `cars` 数组中，而且在我看来 `cars` 数组没有被更改的事实似乎并不明显。但是，也许只有我是这样。我很想知道你是怎么想的?

Since I don't really have a substantial reason besides aesthetics, I think you and your team should stick with whatever you choose 👍

因为除了「好看」之外，我并没有什么实质性的理由，所以我认为你和你的团队应该坚持所选择的任何东西。

## Difference between Spread vs Concat

**扩展运算符和 Concat 的不同点**

I prefer using `spread`, because I find it more concise and easier to write. BUT, there are still benefits of using `concat`.

我更倾向用扩展运算符，因为我觉得它更简洁，更容易写。但是，使用 `concat` 仍然有好处。

Spread is fantastic when you know beforehand that you're dealing with arrays. But what happens when the source is something else, like a string. And you want to add that string to the array. Let's walk through an example.

当你事先知道要处理数组时，扩展运算符是非常棒的。但如果来源是别的东西，比如字符串，会发生什么呢。你想把字符串添加到数组中。让我们看一个例子。

## Example: Dealing with an arbitrary argument

**例子：处理任意参数**

Let's say this is the outcome we want:

假设这是我们想要的结果：

```js
[1, 2, 3, "random"];
```

## A. Using Spread

**使用扩展运算符**

And if we follow the pattern we've been using and used the spread operator. Here's what happens:

如果我们按照一直使用的模式使用扩展运算符。会发生什么：

```js
function combineArray(array1, array2) {
  return [...array1, ...array2];
}

const isArray = [1, 2, 3];
const notArray = "random";

combineArray(isArray, notArray);
// 😱 [ 1, 2, 3, 'r', 'a', 'n', 'd', 'o', 'm' ]
```

☝️ If we spread our string, it will split the word into separate letters. So it doesn't achieve the result we want.

如果我们将字符串「展开」，它会将单词分割成不同的字母。所以它没有达到我们想要的结果。

## B. Using Concat

BUT, if we follow the concat pattern that we've been doing. Here's what happens:

但是，如果我们遵循一直在做的 concat 模式。又会发生什么：

```js
function combineArray(array1, array2) {
  return [].concat(array1, array2);
}

const isArray = [1, 2, 3];
const notArray = "random";

combineArray(isArray, notArray);
// ✅  [ 1, 2, 3, 'random' ]
```

☝️ Excellent! We get the result we want.

很棒！得到了我们想要的结果。

I know some of you are like, duh! I'll just write some conditional to make sure what I'm passing is an array and execute accordingly. Sure that'd work too. Or just write less code and use `concat` and be done with 😂

我知道你们有些人会说，废话！我只写一些条件语句来确保我传递的是一个数组，并相应地执行。当然这也行。或使用 `concat`，能写更少的代码。

## Verdict

**结论**

So here's the quick rule. If you know you're dealing with arrays, use `spread`. But if you might be dealing with the possibility with a non-array, then use `concat` to merge an array 👍

这就是快速法则。如果您知道正在处理数组，请使用扩展运算符。但如果你可能要处理一个非数组，那么使用 `concat` 来合并一个数组。

Anyways I just want to point that out, so you can use the most appropriate method depending on the problem you're trying to solve 👍

无论如何，我想指出的是，你可以使用最合适的方法，这取决于你要解决的问题

## Merge Array with Push 🤔

**用 Push 合并数组**

Some of you are asking, hey, can't I also use `push` to merge an array. And yes, you sure can! But when you use `push`, it manipulates or changes the existing array. It does NOT create a new array. So depending on what you're trying to do. Make sure you keep that in mind.

你们中的一些人可能会问，嘿，我能用 `push` 来合并一个数组吗？是的，你当然可以！但是当你使用 `push` 时，它会改变原有的数组。它不创建新数组。这取决于你想做什么。一定要记住这一点。

```js
const cars = ["🚗", "🚙"];
const trucks = ["🚚", "🚛"];

const combined = cars.push(...trucks);

console.log(combined); // 4
// ☝when you use push, it returns the LENGTH of the combined array

console.log(cars); // [ '🚗', '🚙', '🚚', '🚛' ]
console.log(trucks); // ['🚚', '🚛']
```

Also, when you're trying to push an array to another array. You will need to spread it, otherwise, you will end up getting a nested array. Of course, unless that's what you wanted 😜

另外，当你试图将一个数组「push」到另一个数组时。您需要「扩展」它，否则将得到一个嵌套的数组。当然，除非那是你想要的：

```js
const cars = ['🚗', '🚙'];
const trucks = ['🚚', '🚛'];

cars.push(trucks);
// 😱 cars: [ '🚗', '🚙', [ '🚚', '🚛' ] ]

cars.push(...trucks);
// ✅ cars: [ '🚗', '🚙', '🚚', '🚛' ]
```

## Browser Support

**浏览器支持**

Spread was introduced in ES6, so all modern browser supports it. Except for the "I'm too cool" Internet Explorer - no support there 😕. So if you need IE support, you want to use `concat` instead or use a compiler like [Babel](https://babeljs.io/).

扩展运算符是在 ES6 中引入的，所有现代浏览器都支持它。除了「我很酷」的 IE 浏览器（它不支持）。所以如果你需要 IE 支持，你可以使用 `concat` 或者编译器，比如[Babel](https://babeljs.io/)

- [Browser Support: Spread](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Browser_compatibility)
- [Browser Support: Concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat#Browser_compatibility)

## Resources

- [MDN Web Docs - Concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)
- [MDN Web Docs - Spread](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [Stack Overflow: spread operator vs array.concat](https://stackoverflow.com/questions/48865710/spread-operator-vs-array-concat)
