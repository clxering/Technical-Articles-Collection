# Better Array check with Array.isArray

**Array.isArray 是更好的数组检查方式**

> 转译自：https://www.samanthaming.com/tidbits/63-better-array-check-with-array-isarray

In JavaScript, arrays are not true arrays. They are actually objects. So you can't simply do a `typeof` check. Because it will return `object` 😱

在 JavaScript 中，数组不是真正的数组。它们实际上是对象。所以你不能简单地做一个 `typeof` 检查。因为它会返回 `object`

But not a problem! Use `Array.isArray()` -- finally, there is an easier way to check if a value is an actual array 🎉

但不是问题！使用 `Array.isArray()` 就可以了。最后，有一种更简单的方法来检查一个变量是否确为数组。

```js
const books = ['📕', '📙', '📗'];

// Old way
Object.prototype.toString.call(books) === '[object Array]';

// ✅ Better
Array.isArray(books);
```

## Array is not a true array

**数组不是真正的数组**

Let's see what I mean by this, `array` is not a true array.

让我们看看这句话的意思，`array` 不是真正的数组。

```js
const array = [];

typeof array; // 'object'
```

☝️That's why you can't use your typical `typeof`. Because array is an `object` type 😕

这就是为什么你不能使用典型的 `typeof`。因为数组是一个 `object` 类型。

## Array.isArray Demo

Alright, let's try this method on other values and see what we get 👩‍🔬

好的，让我们在其他值上试试这个方法，看看能得到什么。

## These are all arrays, and will return `true`

**这些都是数组，并返回 `true`**

```js
// Empty Array
Array.isArray([]); // true

// Array
Array.isArray(['📓']); // true

// Array Constructor
Array.isArray(new Array('📓')); // true
```

## These are NOT arrays and will return `false`

**这些都不是数组，并返回 `false`**

```js
// Object
Array.isArray({}); // false

// Object
Array.isArray({book: '📓'}); // false

// Number
Array.isArray(123); // false

// Boolean
Array.isArray(true); // false

// Boolean
Array.isArray(false); // false

// String
Array.isArray('hello'); // false

// Null
Array.isArray(null); // false

// Undefined
Array.isArray(undefined); // false

// NaN
Array.isArray(NaN); // false
```

## instanceof vs Array.isArray

Another popular choice you might is using `instanceof`

另一个常用的选择是 `instanceof`

```js
const books = ['📕', '📙', '📗'];

books instanceof Array; // true
```

## But...

The problem is it doesn't work with multiple context (e.g. frames or windows). Because each frame has different scopes with its own execution environment. Thus, it has a different global object and different constructors. So if you try to test an array against that frame's context, it will NOT return `true`, it will return incorrectly as `false`.

问题是它不能用于多个上下文（例如框架或窗口）。因为每一框架都有不同的作用域和它自己的执行环境。因此，它有不同的全局对象和不同的构造函数。因此，如果尝试根据该框架的上下文测试一个数组，它将不会返回 `true`，而是会错误地返回 `false`

🤯 What are you talking about??? 👈 If this is floating in your mind. Don't worry, I was too. To understand this, you need to understand JavaScript's execution context. Here's a great video explaining it, [An Introduction to Functions, Execution Context and the Call Stack](https://youtu.be/exrc_rLj5iw). This is a bit more of an advanced topic, so if you're just a beginner, feel free to skip through it. And when you get a bit more comfortable with JavaScript, then definitely return to this topic. In the meantime, let me try to explain this "multiple context" in non-dev terms.

你在说什么？如果这话漂浮在你的脑海里。别担心，我也是。要理解这一点，需要理解 JavaScript 的执行上下文。这里有一个很好的视频来解释它，[函数、执行上下文和调用堆栈的介绍](https://youtu.be/exrc_rLj5iw)。这是一个更高级的主题，所以如果你只是一个初学者，可以跳过它。当你对 JavaScript 熟悉了一些之后，一定要回到这个主题。同时，让我试着用非开发术语来解释这个「多重上下文」。

## Explanation in **non-dev** terms

**用非开发术语来解释**

You can think of frames like different planets. Every planet has its own system, with different gravity pull and composition. So `instanceof` only works on our planet, Earth. If you bring it to Mars, it won't work. However, with `Array.isArray()` it will work on any planet. It's universal. That's why you should use `Array.isArray()`.

你可以把框架想象成不同的行星。每个行星都有自己的系统，有着不同的引力和组成。所以 `instanceof` 只适用于我们的星球，地球。如果你把它带到火星，它不会工作。但是，使用 `Array.isArray()` 可以在任何行星上工作。这是普遍的。这就是为什么应该使用 `Array.isArray()` 的原因。

```js
// Creating our new "planet" called mars
const mars = document.createElement('iframe');
document.body.appendChild(iframe);
xArray = window.frames[window.frames.length-1].Array;

// Let's make an array in our new "planet", mars
var marsArray = new xArray('👩', '👨');

// Using the instanceof tool to test the marsArray
marsArray instanceof Array;
//  false --> ❌ doesn't work

// Now, let's try using our universal tool
Array.isArray(marsArray)
// true --> ✅ great, it works!
```

## Community Input

**社区观点**

- Code Sample by [@_botol](https://www.instagram.com/_botol/)

https://jsfiddle.net/botol/ryu324gw

```js
// HTML
<iframe id='example'></iframe>

// JS
const frame = document.querySelector('#example');
frame.contentDocument.write('<script>object = {}<\/script>');

const frameObj = frame.contentWindow.object;
console.log(frameObj); // Object {}
console.log(typeof frameObj); // object
console.log(frameObj instanceof Object); // false
console.log(frameObj instanceof frame.contentWindow.Object); // true
```

译注：原文并没有摘录上述代码

- Cale Shapera:

- [@caleshapera](https://medium.com/@caleshapera/useful-131bc462ae9f): I like to use isArray along with array.length to error check whether I should process a variable.

我喜欢使用 isArray 和 array.length 来检查是否应该处理变量。

```js
if (!Array.isArray(array) || !array.length) {
  // array does not exist, is not an array, or is empty
  // ⇒ do not attempt to process array
} else {
  // ⇒ process array
}
```

- Russel P: It's worth noting that `Array.isArray(books)` is the ES5 equivalent to `books && typeof books === 'object' && books.constructor === Array`

值得注意的是，`Array.isArray(books)` 相当于 ES5 的 `books && typeof books === 'object' && books.constructor === Array`

## Resources

- [MDN Web Docs: Array.isArray()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)
- [w3schools: Array.isArray()](https://www.w3schools.com/jsref/jsref_isarray.asp)
- [MDN Web Docs: instanceof and multiple context](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof#instanceof_and_multiple_context_(e.g._frames_or_windows))
- [MDN Web Docs: instanceof vs isArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray#instanceof_vs_isArray)
- [2ality: instanceof](http://2ality.com/2013/01/categorizing-values.html)
- [StackOverflow: How do you check if a variable is an array in JavaScript?](https://stackoverflow.com/questions/767486/how-do-you-check-if-a-variable-is-an-array-in-javascript)
- [StackOverflow: How to check if an object is an array?](https://stackoverflow.com/questions/4775722/how-to-check-if-an-object-is-an-array)
- [StackOverflow: Difference between using Array.isArray and instanceof Array](https://stackoverflow.com/questions/22289727/difference-between-using-array-isarray-and-instanceof-array)
- [StackOverflow: Is instanceof Array better than isArray in JavaScript?](https://stackoverflow.com/questions/28779255/is-instanceof-array-better-than-isarray-in-javascript)
- [GitHub Issue Discussion: instanceof with multiple windows/iframes](https://github.com/mrdoob/three.js/issues/5886)
- [instanceof considered harmful](http://perfectionkills.com/instanceof-considered-harmful-or-how-to-write-a-robust-isarray/)
- [How to better check data types in javascript](https://webbjocke.com/javascript-check-data-types/)

