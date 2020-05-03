# JS Essentials: Falsy Values

> 转译自：https://www.samanthaming.com/tidbits/25-js-essentials-falsy-values

This is part of my essentials series. They’re not the most exciting posts. But it is important to know these fundamentals to understand why JS does the things it does. It will help you debug better because you will know why things aren’t working and help you go from a junior developer to a intermediate developer. Let’s go! 💪

Values in JS have an inherent Boolean value associated with. So it’s super important to know what those values would be evaluated to (true or false), especially when it comes to comparisons and conditionals 🤓

I use to try to figure out what values would be true. Don’t do that. Instead, memorize what values are Falsy. If it’s not on the list, it’s considered truthy 👍

```js
// JS Essentials: Falsy Values

false
undefined
null
NaN
0 or +0 or -0
"" or '' or `` (empty string)

// Everything else is truthy
```

## Values that are actually Truthy

Here are some sample values that you might have thought was true (at least I did), but it actually evaluates to `true`. Remember if it's not on the false list, it's true!

```js
// All these will return **true**

Boolean([]); // Empty []
Boolean({}); // Empty {}
Boolean("0"); // String containing zero
Boolean("false"); // String containing the text false
Boolean(function() {}); // Empty function
Boolean(-Infinity); // Infinity or Negative Infinity
Boolean(new Date()); // Date object
```

## Using Falsy Value in Conditional Statements

Since every value in JS evaluates to a boolean (true or false). We can shorten our conditional in the if statement.

```js
// ❌
if(false === null)
if(undefined === null)
if(null === null)
if(NaN === null)
if(0 === null)
if("" === null)
if(value === null)

// ✅  Much better
if(value)
```

## Using Falsy value in Ternary Operators

Similarly, you can use it to shorten your condition in the ternary operators.

```js
// ❌
"" === false ? "🙂" : "🙃";

// ✅  Much better
"" ? "🙂" : "🙃";
```

## Using Falsy value in Logical Operators

Here's an interesting one. You can use `||` and `&&` to return a value. Definitely knowing what value is falsy and truthy is super important.

`||` comparisons will stop as soon as a truthy expression is evaluated

```js
const or = "" || "hi"; // "hi"
const or = [] || "hi"; // []
```

`&&` comparison will stop as soon as a falsy expression is evaluated

```js
const and = "" && "hi"; // ""
const and = [] && "hi"; // "hi"
```

## Community Examples

## Using Falsy Values for Unit Testing

Jason A: Sometimes when unit testing I use an array of falsy values and loop over them to see if my fallback works correctly, sorta like this:

```js
[false, undefined, NaN].forEach(el =>
  expect(funcImTesting(el).to.be("my fallback"))
);
```

Thanks: @jsawbrey

## Resources

- [MDN Web Docs - Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)
- [JS Falsy Object & Comparisons](https://medium.com/sons-of-javascript/javascript-falsy-objects-and-comparisons-92d5888be09d)
- [Truth & Falsy](https://j11y.io/javascript/truthy-falsey/)
- [Dealing with Javascript falsy values](https://medium.com/@sgobinda007/dealing-with-javascript-falsy-values-d75a2f1b1c90)
