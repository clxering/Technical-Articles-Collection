# 5 Ways to Convert a Value to String in JavaScript

**JavaScript 中转换数值为字符串的五种方法**

> 转译自：https://www.samanthaming.com/tidbits/62-5-ways-to-convert-value-to-string

If you're following the Airbnb's Style Guide, the preferred way is using "String()" 👍

如果遵循 Airbnb 的风格指南，那么首选的方法是使用 String()

It's also the one I use because it's the most explicit - making it easy for other people to follow the intention of your code 🤓

它也是我所使用的，因为它是最明确的，使其他人更容易了解代码的意图。

Remember the best code is not necessarily the most clever way, it's the one that best communicates the understanding of your code to others 💯

记住，最好的代码不一定是最聪明的方法，却是最好地向他人传达你对代码理解的方法。

```js
const value = 12345;

// Concat Empty String
value + "";

// Template Strings
`${value}`;

// JSON.stringify
JSON.stringify(value);

// toString()
value.toString();

// String()
String(value);

// RESULT
// '12345'
```

## Comparing the 5 ways

**比较五种方法**

Alright, let's test the 5 ways with different values. Here are the variables we're going to test these against:

好的，让我们用不同的值来测试这五种方法。下面是我们要测试的变量：

```js
const string = "hello";
const number = 123;
const boolean = true;
const array = [1, "2", 3];
const object = { one: 1 };
const symbolValue = Symbol("123");
const undefinedValue = undefined;
const nullValue = null;
```

## Concat Empty String

**通过 concat 与空字符串连接**

```js
string + ""; // 'hello'
number + ""; // '123'
boolean + ""; // 'true'
array + ""; // '1,2,3'
object + ""; // '[object Object]'
undefinedValue + ""; // 'undefined'
nullValue + ""; // 'null'

// ⚠️
symbolValue + ""; // ❌ TypeError
```

From here, you can see that this method will throw an `TypeError` if the value is a `Symbol`. Otherwise, everything looks pretty good.

从这里可以看到，如果值是 `Symbol`，那么该方法将抛出一个 `TypeError`。其他一切情况看起来都很好。

## Template String

**模板字符串**

```js
`${string}`; // 'hello'
`${number}`; // '123'
`${boolean}`; // 'true'
`${array}`; // '1,2,3'
`${object}`; // '[object Object]'
`${undefinedValue}`; // 'undefined'
`${nullValue}`; // 'null'

// ⚠️
`${symbolValue}`; // ❌ TypeError
```

The result of using **Template String** is essentially the same as **Concat Empty String**. Again, this might not be the ideal way when dealing with `Symbol` as it will throw a `TypeError`.

使用 **模板字符串** 的结果本质上与 **通过 concat 与空字符串连接** 相同。同样，这可能不是处理 `Symbol` 的理想方法，因为它会抛出 `TypeError`

This is the TypeError if you're curious: `TypeError: Cannot convert a Symbol value to a string`

如果你好奇，这是一个类型错误：`TypeError: Cannot convert a Symbol value to a string`

## JSON.stringify()

```js
// ⚠️
JSON.stringify(string); // '"hello"'
JSON.stringify(number); // '123'
JSON.stringify(boolean); // 'true'
JSON.stringify(array); // '[1,"2",3]'
JSON.stringify(object); // '{"one":1}'
JSON.stringify(nullValue); // 'null'
JSON.stringify(symbolValue); // undefined
JSON.stringify(undefinedValue); // undefined
```

So you typically would NOT use JSON.stringify to convert a value to a string. And there's really no coercion happening here. I mainly included this way to be complete. So you are aware of all the tools available to you. And then you can decide what tool to use and not to use depending on the situation 👍

一般不使用 JSON.stringify 将值转换为字符串。这里不强制。主要为了说明能用这个方式来完成。让你知道所有可用的工具。然后你可以根据情况决定使用什么工具，或不使用什么工具。

One thing I want to point out because you might not catch it. When you use it on an actual `string` value, it will change it to a string with **quotes**.

我想指出一件事，因为你们可能听不懂。当您在实际的 `string` 值上使用它时，它会将其更改为一个带有 **引号** 的字符串。

You can read more about this in Kyle Simpson, "You Don't Know JS series":
[JSON Stringification](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch4.md#json-stringification)

你可以在 Kyle Simpson 的《你不知道 JS 系列》中了解更多：JSON Stringification

**Side note on the importance of knowing your fundamentals!**

**注意基本知识的重要性!**

Yes, you may have noticed in my code notes, I frequently quote Kyle's books. I honestly have learned a lot of it. Not coming from a computer science background, there is a lot of fundamentals concept I'm lacking. And his book has made me realize the importance of understanding the fundamentals. For those, who want to be a serious programmer, the way to level up is really TRULY understand the fundamentals. Without it, it's very hard to level up. You end up guessing the problem. But if you know the fundamentals, you will understand the "why" of something. And knowing the "why" will help you better execute the "how". Anyhoo, highly recommend this series for those trying to becoming a senior programmer!

是的，你可能已经注意到在我的代码注释中，我经常引用 Kyle 的书。说实话，我学到了很多。我没有计算机科学的背景，有很多基础的概念是我所缺乏的。他的书让我意识到了解基础知识的重要性。对于那些想要成为一名真正的程序员的人来说，升级的方法是真正理解基本原理。没有它，就很难升级。你最后只能猜测问题的答案。但是如果你知道基本原理，你就会明白为什么。知道「为什么」会帮助你更好地执行「如何」。无论如何，强烈推荐本系列，为那些试图成为一个高级程序员的人们!

## toString()

```js
string.toString(); // 'hello'
number.toString(); // '123'
boolean.toString(); // 'true'
array.toString(); // '1,2,3'
object.toString(); // '[object Object]'
symbolValue.toString(); // 'Symbol(123)'

// ⚠️
undefinedValue.toString(); // ❌ TypeError
nullValue.toString(); // ❌ TypeError
```

So the battle really comes down to `toString()` and `String()` when you want to convert a value to a **string**. This one does a pretty good job. Except it will throw an error for `undefined` and `null`. So definitely be mindful of this

因此，想要将一个值转换为 **字符串** 时，问题实际上归结为 `toString()` 和 `String()`。这是好的做法。但是它会抛出一个错误 `undefined` 和 `null`。所以一定要注意这一点。

## String()

```js
String(string); // 'hello'
String(number); // '123'
String(boolean); // 'true'
String(array); // '1,2,3'
String(object); // '[object Object]'
String(symbolValue); // 'Symbol(123)'
String(undefinedValue); // 'undefined'
String(nullValue); // 'null'
```

Alright, I think we found the winner 🏆

好的，我想我们找到了赢家。

As you can see, the `String()` method handles the `null` and `undefined` quite well. No errors are thrown - unless that's what you want. Remember my suggestion is generally speaking. You will know your app the best, so you should pick the most suitable way for your situation.

如你所见，`String()` 方法很好地处理了 `null` 和 `undefined`。不会抛出任何错误（除非你希望这样）。我的建议是一般情况。你最了解你的应用程序，所以你应该选择最适合你的情况的方法。

## Conclusion: String() 🏆

After showing you how all the different methods handle different type of value. Hopefully, you are aware of the differences and you will know what tool to pick up the next time you tackle your code. If you're not sure, `String()` is always a good default 👍

在展示了所有不同的方法如何处理不同类型的值之后。希望你已经意识到了它们之间的区别，并且知道下次处理代码时应该使用什么工具。如果您不确定，`String()` 总是一个好的默认值。

## Why you shouldn't use `new String()`

**为什么不应该使用 `new String()`**

You might notice in Airbnb's styleguide, it mentioned to not use `new String()`. Let's see why:

你可能会注意到在 Airbnb 的风格指南中，它提到不要使用 `new String()`。让我们看看为什么：

```js
const number = 123;

typeof new String(number); // 'object'
```

So when you create a value using a constructor with the `new` keyword, you're actually creating an object wrapper. And this is what it outputs:

因此，当使用带有 `new` 关键字的构造函数创建一个值时，实际上是在创建一个对象包装器。这是它的输出：

```js
console.log(new String(number));
// String {"123"}
```

> The point is, new String("123") creates a string wrapper object around "123", not just the primitive "123" value itself. [edited]

重点是，`new String("123")` 创建一个包装 "123" 的字符串包装器对象，而不仅仅是原始的 "123" 值本身。

[Kyle Simpson's You Don't Know JS](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch3.md#chapter-3-natives)

One reason why you'd do this is:

你这样做的一个原因是：

> There's very little practical use for String objects as created by new String("foo"). The only advantage a String object has over a primitive string value is that as an object it can store properties:

对于由 `new String("foo")` 创建的 String 对象，几乎没有什么实际用途。一个字符串对象相对于原始字符串值的唯一优势是，作为一个对象，它可以存储属性：

```js
var str = "foo";
str.prop = "bar";
alert(str.prop); // undefined

var str = new String("foo");
str.prop = "bar";
alert(str.prop); // "bar"
```

[StackOverflow: What's the point of new String(“x”) in JavaScript?](https://stackoverflow.com/questions/5750656/whats-the-point-of-new-stringx-in-javascript)

## Community Input

**社区观点**

[@MaxStalker](https://twitter.com/MaxStalker/status/1132375146340278273): I would use a different method depending on the application:

我会根据不同的应用程序使用不同的方法：

- "" + val: simply cast number to string - let's say inside of the .map()

简单地将数字转换为字符串，比方说在 .map() 内部。

- JSON.stringify(val): need to convert small non-nested object

需要转换小的非嵌套对象。

- .toString(radix): convert number to hexidecimal or binary

将数字转换为十六进制或二进制。

[@frontendr](https://twitter.com/frontendr/status/1132393041350856704): Carefully when using JSON.stringify, that will change a string into a string with quotes 😉

使用 JSON.stringify 时要小心，会将字符串转换为带引号的字符串。

[@super.pro.dev](https://www.instagram.com/super.pro.dev/): I also know: new String (foo) but I don't like this method (it will create an object of String, in contrast to String (without "new") which create string primitive)

我还知道：new String (foo)，但我不喜欢这个方法，它会创建一个 String 对象，与 String(没有「new」)创建 String 基本数据类型形成对比。

[@BrunoGiubilei](https://twitter.com/BrunoGiubilei/status/1132959435599618053): when concat empty string, it's mostly correct to declare the empty strings first, because when you concat more one values, the sum has been processed first.

当用空字符串拼接时，首先声明空字符串通常是正确的，因为当您拼接多个值时，累加首先被处理：

```js
1 + 2 + 3 + ""; // 6
"" + 1 + 2 + 3; // 123
```

## Resources

- [MDN Web Docs: toString](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toString)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript#coercion--strings)
- [2ality: Converting a value to string in JavaScript](http://2ality.com/2012/03/converting-to-string.html)
- [Convert Number to String](https://stackabuse.com/javascript-convert-number-to-string/)
- [Stack Overflow: Casting to string](https://stackoverflow.com/questions/11083254/casting-to-string-in-javascript)
- [Addition operator in details](https://dmitripavlutin.com/javascriptss-addition-operator-demystified/)
- [YDKJS: Coercion](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch4.md#tostring)
