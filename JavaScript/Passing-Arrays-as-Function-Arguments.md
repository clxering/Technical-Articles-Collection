# Passing Arrays as Function Arguments

**将数组作为函数参数传递**

> 转译自：https://www.samanthaming.com/tidbits/48-passing-arrays-as-function-arguments

If you want to pass an array into a variadic function. You can use ES6 spread to turn that array into a list of arguments. Yay, so much cleaner and no useless null from the old apply way 👏

如果要将数组传递给可变参数函数。你可以使用 ES6 的扩展运算符将该数组转换为参数列表。是的，这么做多么简洁，不像以往要使用无效的 null

```js
function sandwich(a, b, c) {
  console.log(a); // '🍞'
  console.log(b); // '🥬'
  console.log(c); // '🥓'
}

const food = ["🍞", "🥬", "🥓"];

// Old way
sandwich.apply(null, food);

// ✅ ES6 way
sandwich(...food);
```

## Using it with `Math` functions

The ability to turn an array into a list of arguments is super handy with the `Math` functions.

将数组转换为参数列表的功能对于 `Math` 函数而言非常方便。

## Example: Find the Largest Number

**案例：找出最大值**

Let's say you want to find the largest number using the `Math.max()` function.

假设你想使用 `Math.max()` 函数查找最大的数。

```js
const largest = Math.max(5, 7, 3, 4);

console.log(largest); // 7
```

But rarely, would you pass in individual values. More likely, you would want to find the maximum element in an array. So the question now is, how do you pass an array of values into a function that accepts individual arguments and NOT an array?

很少会有传入单独的值的情况。更有可能的场景是，你希望找到数组中的最大元素。现在的问题是，如何将一个数组的值传递给一个接受单个参数而不是数组的函数？

This would be terrible:

这太可怕了：

```js
const numbers = [5, 7, 3];

// 🤮 Yuck!
Math.max(numbers[0], numbers[1], numbers[2]);

// ❌ And this won't work
Math.max(numbers); // NaN
```

Lucky for us, we can use ES6's Spread operator!

辛运的是，我们可以使用 ES6 的扩展运算符

```js
const numbers = [5, 7, 3];

// 😍 Much Better!
Math.max(...numbers); // 7
```

What `spread` is doing here is taking the array element and expanding or unpacking it into a list of arguments for our variadic function.

扩展运算符的作用是将数组元素展开或展开到变量函数的参数列表中。

```js
const numbers = [5, 7, 3];

console.log(...numbers); // 5 7 3
```

## Explaining `spread` in non-dev terms

**用非开发术语解释扩展运算符**

If you find this spread-ing thing still confusing. Maybe let me try to explain it with [Russian nesting dolls](https://en.wikipedia.org/wiki/Matryoshka_doll). So I like to think of the array as Russian nesting dolls. And what spread does is:

如果你发现扩展运算符仍然令人困惑。让我试着用俄罗斯套娃来解释一下。我喜欢数组想象成俄罗斯套娃。扩展运算符的作用是：

1. It unpacks (spread) the nested dolls into individual dolls.

它把嵌套的玩偶拆分成单独的玩偶。

2. And now you have all these individual dolls (arguments) to place nicely in your display case (function).

现在，得到了所有单独的玩偶（参数），可以很好地放置在展示案例（函数）中。

Not sure if this explanation helps? Leave a comment if it does, and I'll start explaining programming concepts in fun ways like this 😆

不知道这个解释是否有用？如果是这样，请留下评论，我将开始以有趣的方式解释编程概念

## Passing Multiple Arrays as Function Arguments

**传递多个数组作为函数参数**

Another superpower spread has is combining arrays.

另一个强有力的扩展是组合数组。

```js
const one = [1, 2, 3];
const two = [4, 5, 6];

const merged = [...one, ...two];
// [ 1, 2, 3, 4, 5, 6 ]
```

So we can use this superpower to pass multiple arrays as function arguments 💪

因此，我们可以使用这个超能力将多个数组作为函数参数传递

```js
const one = [1, 2, 3];
const two = [4, 5, 6];

Math.max(...one, ...two); // 6
```

For those keeners, wondering if you can pass in 3 arrays. Well, you betcha! It's like the energizer bunny, it keeps going and going and going .... (This post is not sponsored by Energizer lol. But that can change, hit me up Energizer. Me want some sponsor money 😂)

对于那些 keener，想知道是否可以传入三个数组。嗯,那还用说！它就像一个充满活力的兔子，它一直在前进，前进，前进……

```js
const one = [1, 2, 3];
const two = [4, 5, 6];
const three = [2, 100, 2];

Math.max(...one, ...two, ...three); // 100
```

## What is a variadic function?

**什么是可变参数函数**

So you may notice I use the term variadic functions. The computer science folks will have probably heard this term. But for the rest of the cool bees like myself 😝, it may not be so familiar. A variadic function is a function that accepts an infinite or variable number of arguments. And the `Math.max()` function is one of those variadic function.

你们可能注意到我用了可变函数这个术语。计算机科学专业的人可能听说过这个词。但对于其他像我一样的 cool bees 来说，对它可能不那么熟悉。可变参数函数是接受无限或可变数量参数的函数。而 `Math.max()` 函数就是那些可变参数函数中的其中之一。

## Resources

- [MDN Web Docs - Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [DWB - 6 Great Uses of the Spread Operator](https://davidwalsh.name/spread-operator)
- [Spreading arrays into arguments in JavaScript](http://2ality.com/2011/08/spreading.html)
- [JavaScript.info - Spread](https://javascript.info/rest-parameters-spread-operator)
- [Stack Overflow - Passing an array as a function parameter in JavaScript](https://stackoverflow.com/questions/2856059/passing-an-array-as-a-function-parameter-in-javascript)
- [Smashing Magazine - How To Use ES6 Arguments And Parameters](https://www.smashingmagazine.com/2016/07/how-to-use-arguments-and-parameters-in-ecmascript-6/)
