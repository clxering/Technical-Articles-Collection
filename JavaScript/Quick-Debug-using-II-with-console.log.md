# Quick Debug using || with console.log

**console.log 用 || 快速调试**

> 转译自：https://www.samanthaming.com/tidbits/54-quick-debug-wth-console-log-using-or-operator

It's always a pain to debug 1-line arrow function with a console.log. Why? b/c we need to convert it to a multi-line first. No more! Just use `||` before your expression. It outputs both your `console.log` and expression 👍

使用 console.log 调试单行箭头函数总是一件痛苦的事情。为什么？我们需要先把它转换成多行形式。不需要多想！在你的表达之前使用 `||`。它输出到你的 `console.log` 和 expression

And clean up is a breeze! No more messy re-conversion back to a 1-line. Just remove your `console.log`. And you're done 😆

清理是一件轻而易举的事！No more messy re-conversion back to a 1-line. 只需删除 `console.log` 即可。和你做

```js
// ✅
() => console.log('🤖') || expression

// ❌
() => {
  console.log('🤖')
  return expression
}
```

## Example

Let's take a look at a simple example of how this would work:

让我们来看一个简单的例子，看看它将如何工作：

```js
const numbers = [1, 2, 3];

numbers.map(number => number * 2);

// ✅ Debug quickly by prepending with `||`
numbers.map(number => console.log(number) || number * 2);

// ❌ No need to expand it to multi line
numbers.map(number => {
  console.log(number);
  return number * 2;
});
```

## How does the `||` work?

Often times we think the `||` operator is only used in conditional statements. However, you can also think of it as a `selector operator`. It will always evaluate one of the 2 expressions.

通常我们认为 `||` 运算符只在条件语句中使用。然而，你也可以把它看作一个 `selector operator`。它总是对这两个表达式中的一个求值。

Because `console.log` always return `undefined`, which is a falsy value. The second expression will always be evaluated 👍

因为 `console.log` 总是返回 `undefined`，这是一个错误值。第二个表达式都会求值

To learn more about the `||` operator, check out my previous post [here](https://www.samanthaming.com/tidbits/52-3-ways-to-set-default-value)

想了解更多关于 `||` 运算符的内容，可以参阅之前的文章 [here](https://www.samanthaming.com/tidbits/52-3-ways-to-set-default-value)

## Community Input

## Using the Comma operator

@phenax5: You can also use the comma operator. Separate two expressions with a comma and it will execute the first one and then the second and return with the second one.

还可以使用逗号运算符。用逗号分隔两个表达式，它将执行第一个表达式，然后执行第二个表达式，并返回第二个表达式。

```js
a => (console.log(a), a + 5);
```

And let me break up the example so it's very clear where the 2 expressions are:

让我来分解这个例子，这样就很清楚这两个表达式是什么：

```js
a => (console.log(a), a + 5);
```

⚠️ **Watch your comma placement**

**注意逗号的位置**

But make sure you don't do this. I made that mistake when I first saw his example. You don't want to stick the expression inside the `console.log`. If you do that, your function would not return anything and nothing would be evaluated. Hence, breaking your function. So be careful with your comma placement 😅

千万别这么做。当我第一次看到他的例子时，我就犯了那个错误。不要将表达式粘贴到 `console.log` 中。如果这样做，函数将不返回任何值，也不计算任何值。因此，这破坏了功能。所以要注意逗号的位置

```js
a => (
  console.log(a, a + 5),
)
```

## Using `||` with TypeScript

If you're working with TypeScript and depends on how you set it up. Using this debugging technique might give you an error and prevent your code from compiling. In that case, you can suppress the error using `ts-ignore`.

Using `ts-ignore` should not be a huge deal in this case because the console.log is only there temporarily while you debug. Once you're done, you should definitely remove it.

```js
// @ts-ignore: Unreachable code error
() => console.log("🤖") || expression;
```

Thanks: @stramel89

## Resources

- [MDN Web Docs: Console.log()](https://developer.mozilla.org/en-US/docs/Web/API/Console/log)
- [MDN Web Docs: Logical OR ||](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Logical_OR_()>)
- [3 Ways to Set Default Value in JavaScript](https://www.samanthaming.com/tidbits/52-3-ways-to-set-default-value)
