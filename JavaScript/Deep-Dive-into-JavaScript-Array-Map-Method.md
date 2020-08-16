# Deep Dive into JavaScript's Array Map Method

**深入研究 JavaScript 的数组 map 方法**

> 转译自：https://www.robinwieruch.de/javascript-map-array

The **Map Function** is one of the **many Methods** existing on the **JavaScript Array prototype**. If you want to do a deep dive on prototypical inheritance, here's a great read by Kyle Simpson on [how prototypes work under the hood](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch5.md). For this article it will be sufficient to know that the methods on the Array prototype are available to every array that we declare in our code.

Map 方法是 **JavaScript 数组原型** 的众多方法之一。如果你想深入了解原型继承，这里有一篇由 Kyle Simpson 写的关于 [原型如何在幕后工作（该链接已失效）]() 的文章。对于本文来说，只要知道数组原型上的方法对于在代码中声明的每个数组都可用就足够了。

Specifically, the Array Map Method operates on an array to run a transformation on every element of the array. It does so through use of a *callback function* which is called for each item of the array. After running the callback function on each item, the Map Method returns *the transformed array*, leaving the *original array* unchanged. Let's take a quick look at how that looks in practice:

具体来说，数组的 map 方法对数组进行操作，可对数组的每个元素进行转换。它通过使用一个回调函数来实现，该函数被数组的每个元素调用。在每个元素上运行回调函数后，返回转换后的数组，保持原始数组不变。让我们快速浏览它在应用中的样子：

```js
const originalArray = [1, 2, 3, 4, 5];

const newArray = originalArray.map(function addOne(number) {
  return number + 1;
});

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [2, 3, 4, 5, 6]
```

The Map Method is called on our array of `[1, 2, 3, 4, 5]` as the original array. In the callback function, it then passes through every single item (value) in the array by calling the `addOne` function with the item. The first argument of the callback function is the currently iterated value of the array. Once it completes passing through the array it returns the new array of `[2, 3, 4, 5, 6]` back to us. For the sake of completeness, you can also pass an anonymous function as callback function to the map method:

map 方法被原始数组 [1,2,3,4,5] 调用。在回调函数中，它通过调用参数是数组元素的 `addOne` 方法来遍历数组中的每个元素（值）。回调函数的第一个参数是数组的当前迭代元素。一旦它完成了对数组的遍历，它将返回新的数组 [2,3,4,5,6]。你也可以传递一个匿名函数作为回调函数：

```js
const originalArray = [1, 2, 3, 4, 5];

const newArray = originalArray.map(function (number) {
  return number + 1;
});

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [2, 3, 4, 5, 6]
```

However, if you decide to extract the callback function as standalone function declared as a variable, you have to name it again in order to pass it to the map method:

然而，如果你决定提取回调函数作为一个独立函数，将其声明为一个变量，你必须再次命名它，以便传递它给 map 方法：

```js
const originalArray = [1, 2, 3, 4, 5];

function addOne(number) {
  return number + 1;
}

const newArray = originalArray.map(addOne);

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [2, 3, 4, 5, 6]
```

Now you might be asking, why don't we just use a `for` loop instead? After all, we're looping through the array and executing code on each item, we may as well, right? We could even push the transformed items to a new array in order to make sure we don't modify the original array. Why don't we just do this?

现在你可能会问，为什么我们不使用 for 循环呢？毕竟，我们是在遍历数组并在每一个元素上执行代码，对吧？我们甚至可以将转换后的元素推入一个新的数组，以确保我们没有修改原始数组。为什么我们不这么做呢？

```js
const originalArray = [1, 2, 3, 4, 5];
const newArray = [];

for (let i = 0; i < originalArray.length; i++) {
  newArray[i] = originalArray[i] + 1;
}

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [2, 3, 4, 5, 6]
```

JavaScript includes these built-in Array methods -- including the Map Method -- for a reason. It's not a secret that when you're programming in JavaScript you'll probably be dealing with arrays a lot, and chances are you'll find yourself transforming those arrays quite often. Having utility methods like the Map Method that operates on Arrays not only help us to drastically cut down on the amount of typing that we need to do, they help our code become more readable (in many cases) by having us only describe the part of the loop that will actually change each time we're transforming array data: the transformation which is the business logic of the callback function passed to the map method.

JavaScript 包含这些内置数组方法（包括 map 方法）是有原因的。当使用 JavaScript 编程时，可能会经常处理数组，并且可能会发现自己经常转换这些数组，这已经不是什么秘密了。拥有实用方法，像操作数组的 map 方法不仅帮助我们大大减少我们需要输入的工作量，让我们只描述在每次转换数组数据时实际会改变的循环部分使我们的代码变得更具可读性（在许多情况下）：转换的业务逻辑是传递给 map 方法的回调函数。

**A word about immutable data structures:** The Array Map Method helps us keep our data pure as we go through encouraging *immutable data structures*. The Map Method never changes the original array, which helps us to predictably reason about what value each variable holds as we read through our code.

**关于不可变数据结构**：数组 map 方法帮助我们在提倡不可变数据结构时保持数据的纯粹性。map 方法从不更改原始数组，这有助于我们在阅读代码时预测每个变量持有的值。

However, this isn't an article about `map` versus `for` loops! There's plenty of stuff on the internet about that, and frankly, sometimes a "for"-loop will be a better choice than a Map Function. And if you're new to the Map Function but you're familiar with "for"-loops, it might be helpful to think of the Map Method as a "for"-loop internally. As we go on further in this tutorial, we'll dive into some more examples on how map works and look at some practical ways that we can leverage this method in our day-to-day use cases.

然而，这不是一篇关于 map 和 for 循环的文章！在因特网上有很多关于这个的讨论，坦率地说，有时候 `for循环` 将是一个比 map 函数更好的选择。如果你是 map 函数的新手，但熟悉 `for循环`，那么在内部将 map 方法看作 `for循环` 可能会有所帮助。随着本教程的深入，我们将深入研究更多关于 map 如何工作的示例，并查看在日常用例中利用该方法的一些实际应用。

## Array Map Method with Arrow Functions as Callback Function

**数组 map 方法的回调函数使用箭头函数**

In the first couple examples, we used the `function` keyword to define our callback function. However, you might also be familiar with the ES2015 (or ES6) [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), also known as *lambda* in various programming languages, for anonymous functions. Using the arrow function syntax for the callback function in a Map Method is very common, mainly because it allows us to define all of the logic related to the Map Operation inline without becoming too syntactically burdensome. Here's an example of that same Map Method usage from earlier, but using an arrow function:

在前两个示例中，我们使用 function 关键字来定义回调函数。但是，你可能也熟悉 ES2015 的匿名函数（或 ES6 [箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)），在各种编程语言中也称为 lambda。在 map 方法中使用箭头函数作为回调函数的语法是很常见的，这主要是因为它允许我们内联地定义所有与 map 操作相关的逻辑，而不会使语法变得过于繁琐。这里有一个例子，同样的 map 方法，但使用了箭头函数：

```js
const originalArray = [1, 2, 3, 4, 5];

const newArray = originalArray.map(number => number + 1);

console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(newArray); // [2, 3, 4, 5, 6]
```

Granted, there are a few nuances that you want to be aware of in using an arrow function instead of the `function` keyword. For example, arrow functions will show up as anonymous function in a stack trace. Using the full function syntax allows us to give our callback function a name that will show in the stacktrace in our developer tools. However, the arrow function syntax is also more concise, which makes callbacks in a Map Method effortless to read.

当然，在使用箭头函数取代 function 关键字时，需要注意一些细微差别。例如，箭头函数将在堆栈跟踪中显示为匿名函数。使用完整的函数语法允许我们为回调函数指定一个名称，该名称将在开发人员工具的 stacktrace 中显示。然而，箭头函数语法也更加简洁，这使得 map 方法的回调易于阅读。

**A word about arrow functions**: If you want to look at a more in-depth explanation of the nuance between arrow functions and the traditional function syntax, I'd highly recommend [this article](https://medium.freecodecamp.org/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26) on FreeCodeCamp's blog. There's a lot of people on both sides of the "use vs not use arrow functions" debate, and both sides make a lot of great points. However, we're not gonna dive too far into that debate for now. For the rest of this article I'm just gonna use the arrow syntax, as of right now it's my personal preference, especially for things like the `Array.map` Method Callback Functions.

**关于箭头函数**：如果你想更深入地了解箭头函数和传统函数语法之间的细微差别，我强烈推荐这篇在 FreeCodeCamp 博客上的 [文章](https://medium.freecodecamp.org/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26)。关于「使用和不使用箭头函数」的争论有很多，双方都提出了很多重要的观点。但是，我们现在不打算深入讨论这个问题。在本文的其余部分，我将使用箭头函数语法，目前这是我个人的偏好，特别是对于像数组 map 方法回调函数而言。

## The Map Method's Callback Function

**map 方法的回调函数**

Understanding how the callback function in `map` works is crucial to using the Map Method effectively. In this section we'll take a look at the what arguments are passed to the callback function and some ways that we can use those arguments. The Map Method's callback takes three arguments, although you can write a callback only using one or two arguments as well. Here are the three arguments that it takes: `array.map((value, index, array) => { ... });`.

理解 map 中的回调函数如何工作是有效使用的关键。在这一节中，我们将了解传递给回调函数的参数以及使用这些参数的一些方法。map 方法的回调接受三个参数，尽管你也可以只使用一个或两个参数编写回调。下面是它接受的三个参数：`array.map((value, index, array) => { ... });`

## value

This is the current *value* being processed in the iteration while going through each item in the array. If we ran `[1, 2, 3].map(value => value + 1)`, our callback function would be run with a `value` of `1` the first time, and then it would be called again with `2` and `3` as we iterate through the array. Whereas `value` is the more general naming for this argument, people tend to specify the argument's name as well as we did before by calling it `number`.

这是遍历数组中的每一元素时在迭代中处理的当前值。如果我们运行 `[1, 2, 3].map(value => value + 1)`，我们的回调函数将在第一次运行时值为 1，然后在遍历数组时再次以 2 和 3 调用它。然而 value 是这个参数的更一般的命名，人们倾向于指定参数的名称，就像我们之前所做的那样，称它为 number。

## index

The second argument to the callback function is the _index_ of the item that we are currently processing. Taking our example array of `[1, 2, 3]`, if we run `[1, 2, 3].map((value, index) => index)` we'll see our callback get run with `0` the first time, `1` the second time, and `2` on the final time. This second argument is extremely useful if we're trying to use `map` to generate data or if we need to use the index to access a corresponding item in a _different_ array. We'll look at some more practical ways we can use the `index` argument to do some cool things with `Array.map` later on.

回调函数的第二个参数是我们当前正在处理的元素的索引。以示例数组 [1,2,3] 为例，如果我们运行 `[1, 2, 3].map((value, index) => index)`。我们将看到我们的回调在第一次运行时为 0，第二次为 1，最后一次为 2。如果我们试图使用 map 来生成数据，或者如果我们需要使用索引来访问不同数组中的对应元素，那么第二个参数非常有用。我们会看一些更实用的方法，我们稍后可以使用索引参数让 `Array.map` 来做一些很酷的事情。

## array

The final argument to `map`'s callback function is the `array` that `map` was originally called upon. Chances are you will not often need to use this argument. The reason is that if you've already got the array tied to a variable, you've already got a reference to the original array that `map` was called upon. For example:

map 回调函数的最后一个参数是最初调用的数组。你可能不需要经常使用这个参数。原因是，如果你已经将数组绑定到一个变量，map 被调用时你已经得到了对原始数组的引用。例如:

```js
const myArray = [1, 2, 3];

// using the third argument to map
myArray.map((value, index, array) => {
  return array[index] + 1;
});

// using the variable that holds the original array
myArray.map((value, index) => {
  return myArray[index] + 1;
});

// just using map without accessing the array manually
myArray.map((value) => {
  return value + 1;
});
```

Even though you might not often need the third argument to `map`, it's still good to know that it exists! Every once in a while you'll come across a situation where it comes in handy—for example, when chaining array methods or when you don't have the array bound to a variable.

尽管你使用 map 时可能不经常需要第三个参数，但知道它的存在仍然很好！每隔一段时间，你就会遇到它非常有用的情况。例如，当链接数组方法或当你没有将数组绑定到变量时。

## How to use the Map Method along with other Array Methods

**如何将 map 方法与其他数组方法配合使用**

JavaScript's `Array.map` method is just one of many methods for operating on arrays. In order to use it effectively we need to not only understand *how the Map Method works*, but how it can work in combination with other common array methods. After all, `map` is only one of the tools in our [array methods toolbelt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array), and it's important that we use the right tool for each use case. In this section we're going to examine how the Map Method compares to some other commonly used array methods, and some use cases where another array method might be a better option.

JavaScript Array.map 方法只是对数组进行操作的众多方法之一。为了有效地使用它，我们不仅需要了解 map 方法是如何工作的，还需要了解它如何与其他常用数组方法结合使用。毕竟，map 只是我们的数组方法工具袋中的工具之一，对于每个用例，使用正确的工具是很重要的。在本节中，我们将研究如何将 map 方法与其他一些常用数组方法进行配合，以及在一些用例中，其他数组方法可能是更好的选择。

## Using map vs forEach

Although `map` does iterate through the entire array and it does execute the callback function one time for each item in the array, there's also another method that does a very similar thing: the `forEach` Method.

虽然 `map` 会遍历整个数组，并且它会为数组中的每一元素执行一次回调函数，但是还有另一个方法做了非常类似的事情：`forEach` 方法。

While `forEach` does iterate through the entire array and it does execute its callback function once for every item in the array, there's one major distinction: `forEach` doesn't return anything. In the case of `map`, the return value of the callback function is used as the transformed value in our new array. However, `forEach` doesn't return anything, and if the callback function returns a value, nothing is done with that value.

虽然 forEach 会遍历整个数组，并且也会对数组中的每一元素执行一次回调函数，但有一个主要的区别：forEach 不返回任何内容。在 map 的情况下，回调函数的返回值被用作新数组中转换后的值。但是，forEach 不返回任何东西，而且如果回调函数返回一个值，也不会对该值执行任何操作。

We can use this characteristic of `map` and `forEach`'s return values to inform us as to when we should use the map Method and when we should use the forEach Method. Since `forEach` doesn't do anything with the return values of its callback function, we can safely assume that whenever we're not using the return value of our callback function, this would be a better use case for `forEach` over `map`. For example, this usage of `map` would be better written with a `forEach`:

我们可以使用 map 和 forEach 返回值的这个特性来提示我们何时应该使用 map 方法，何时应该使用 forEach 方法。因为 forEach 不会对其回调函数的返回值做任何事情，所以我们可以安全地假设，当我们没有使用回调函数的返回值时，这将是 forEach 优于 map 的一个更好的用例。例如，使用 forEach 来编写下列案例将比使用 map 更好：

```js
const myArray = [1, 2, 3, 4];

myArray.map(number => {
  console.log(number);
});

// nothing changes except the method we used
myArray.forEach(number => {
  console.log(number);
});
```

However, whenever we plan on *using the return value* from our callback function, this is likely the time that we're gonna reach for `map` instead of `forEach`. If we want to take our array and transform it to a new array, this is a better usage for `map`. For example, this usage of `forEach` would be better written as a `map`:

然而，当我们打算使用回调函数的返回值时，这很可能是我们要使用 map 而不是 forEach 的时候。如果我们想把数组转换成一个新的数组，这是 map 更适合的场景。例如，以下 forEach 的用法最好用 map 取而代之：

```js
const originalArray = [1, 2, 3, 4];
const newArray = [];

originalArray.forEach((number, i) => {
  newArray[i] = number * 2;
});

console.log(newArray); // [2, 4, 6, 8]
```

Since we're pushing a value to a new array and transforming the value, we're essentially recreating all the things that `map` does automatically for us. So, to sum `map` and `forEach` up, if your callback returns a value, you're probably gonna be using `map`, and if it doesn't, `forEach` is probably the better choice.

因为我们将一个值推入一个新的数组并转换这个值，我们本质上重新创建了所有 map 为我们自动做的事情。要对 map 和 forEach 求和，如果你的回调返回一个值，你可能会使用 map，如果没有，forEach 可能是更好的选择。

## Using map and filter

The Filter Method differs from the Map Method in a few ways. While `filter` and `map` are both immutable operations, because they return a new array, they have different purposes. True to its name, `filter` produces a shorter array that has filtered *out* any items that didn't meet a condition. In contrast `map` doesn't ever change the array length—just the values of the items within.

filter 方法与 map 方法有几个不同之处。虽然 filter 和 map 都是不可变的操作，因为它们返回一个新数组，所以它们有不同的用途。filter 会产生一个较短的数组，该数组过滤掉了不满足条件的所有元素。相反，map 不会改变数组的长度：只改变其中元素的值。

If you're looking to remove or delete an item from your array, `filter` is gonna be your friend. However, we can use the Filter Method in combination with the Map Method to do some cool things. For example, we can use `filter` to sanitize our array's values before we use `map` to transform them:

如果你想从数组中删除元素，filter 会是好选择。然而，我们可以使用 filter 方法和 map 方法结合来做一些很酷的事情。例如，在使用 map 转换数组值之前，我们可以使用 filter 来「过滤」数组值：

```js
const originalArray = [1, 2, undefined, 3];

const newArray = originalArray
  .filter(value => {
    return Number.isInteger(value);
  }).map(value => {
    return value * 2;
  });

console.log(newArray); // [2, 4, 6]
```

If we didn't include the `filter` step before the `map`, we'd get `NaN` as the third element in the array, which could seriously trip us up later on in our usage of this new array. However, because we used `filter` to sanitize the array's values we can feel safer about using the transformed values.

如果我们没有在使用 map 之前包含 filter 步骤，我们将遍历到数组中的第三个元素 NaN，这将在以后使用这个新数组时给我们带来严重的麻烦。但是，因为我们使用 filter 来清理数组的值，所以使用转换后的值会更安全。

Believe it or not, some languages have a dedicated function for running this combination of `filter` and `map`, called `filterMap`. However, since we don't have an `Array.filterMap` function in JavaScript, it's useful to know that we can do this combination to sanitize our mapped data.

不置可否，一些语言有专门的函数来运行 filter 和 map 的组合，称为 filterMap。但是，在 JavaScript 中没有 filterMap 函数，我们知道上述通过组合方式来清理数据是很有用的。

## Using map and reduce

Another fairly similar method to `map` is the Reduce Method. However, `Array.reduce` is *far more flexible*.

另一种非常类似的 map 方法的是 Reduce 方法。然而 Array.reduce 要灵活得多。

If you're unfamiliar with `reduce`, it mainly works like this: the `reduce` method also takes a callback as its first argument. This callback receives something called an *accumulator* as its first argument and a value in the array as its second argument (along with the index as its third and the original array as the fourth). What you do with the value is entirely up to you! However, *whatever you return from the callback function* will be used as the *accumulator* argument in the callback for the next iteration.

如果你不熟悉 reduce，它主要是这样工作的：reduce 方法也接受一个回调作为它的第一个参数。这个回调接收到一个称为累加器的东西作为它的第一个参数，数组中的一个值作为它的第二个参数（同时索引作为它的第三个参数，原始数组作为第四个参数）。你如何处理这些值完全取决于你自己！但是，从回调函数返回的内容将在下一次迭代的回调中用作累加器参数。

The second argument to `reduce` is the original *accumulator* -- think of it kind of as the seed. This second argument will be used as the *accumulator* for the *first time the callback is fired*.

reduce 的第二个参数是原始的累加器（可以把它看作种子）。第二个参数将在第一次触发回调时用作累加器。

The *accumulator* can be anything—an array, an object, a string, or even a single number! This aspect of `reduce` makes it extremely versatile since we can iterate through the array once and transform it into *any data structure*. In fact, `reduce` is versatile enough that we can even use it to do the exact same thing that `map` does:

累加器可以是任何东西（一个数组、一个对象、一个字符串，甚至一个数字），reduce 的这个方面使得它非常通用，因为我们可以遍历数组一次，并将其转换为任何数据结构。事实上，reduce 是万能的，我们甚至可以用它来做和 map 一样的事情：

```js
const originalArray = [1, 2, 3, 4, 5];
const newArray = originalArray.reduce((accumulator, value, index) => {
  accumulator[index] = value * 2;
  return accumulator;
}, []);

console.log(newArray); // [2, 4, 6, 8, 10]
```

However, just because we can use `reduce` to do the same thing as `map` doesn't mean we should! In fact, because `map` only requires us to declare our transformation we'll find that it's much cleaner and more readable if we are only transforming values in an array. If we expect to get an array back of transformed values, `map` is likely a better choice than `reduce`.

然而，仅仅因为我们可以使用 reduce 来做与 map 相同的事情，并不意味着我们应该这么做！实际上，因为 map 只需要我们声明转换，所以如果只转换数组中的值，我们会发现它更干净、更可读。如果我们希望获得一个由转换后的值组成的数组，map 可能是比 reduce 更好的选择。

However, if we wanted to use `map` to transform our array into a new object, we couldn't do it. In this case `reduce` would be the best choice since we have much finer grained control over the shape of what it returns. For example, we can use `reduce` to turn an array of strings into object keys.

但是，如果我们想使用 map 将数组转换为一个新对象，我们就做不到这一点。在这种情况下，reduce 将是最好的选择，因为我们能更细粒度的控制它返回的形式。例如，我们可以使用 reduce 将一个字符串数组转换为对象键。

```js
const myArray = ['a', 'b', 'c', 'd'];

const myObject = myArray.reduce((accumulator, value) => {
  accumulator[value] = true;
}, {});

console.log(myObject); // { a: true, b: true, c: true, d: true }
```

To sum it up, if you want to get an array of transformed values, use `map`. But if you need to return something other than an array, you'll likely want to reach for `reduce`.

总而言之，如果你希望转换后获得一个值数组，请使用 map。但是，如果需要返回数组以外的内容，则可能需要使用 reduce。

## Using map and reverse

Occasionally, you might need to map an array and reverse it as well. It's good to know in this case that although `map` is immutable, the Reverse Method isn't! Using `reverse` on an array will actually reverse *the original array*. So, if you need to map and reverse the array, make sure that you do `map` first, and *then* `reverse`. This way you create a new array with `map` before you `reverse` it:

有时，可能需要遍历一个数组并将其反转。在这种情况下，虽然 map 是不可变的，但是 reverse 方法不是！在数组上使用 reverse 实际上会使原始数组反转。所以，如果你需要对数组进行 map 和 reverse，确保先执行 map，然后 reverse。这样你就在执行 reverse 前用 map 创建了一个新的数组：

```js
// Don't do this!
const originalArray = [1, 2, 3, 4, 5];
const reverseNewArray = originalArray.reverse().map(number => number * 2);
console.log(originalArray); // [5, 4, 3, 2, 1]
console.log(reverseNewArray); // [10, 8, 6, 4, 2]

// Instead, do this!
const originalArray = [1, 2, 3, 4, 5];
const reverseNewArray = originalArray.map(number => number * 2).reverse();
console.log(originalArray); // [1, 2, 3, 4, 5]
console.log(reverseNewArray); // [10, 8, 6, 4, 2]
```

If all you need to do is `reverse` an array (you don't need to transform the values), you don't need to use `map` to clone the array! While you *could* produce an unaltered array clone with `map(value => value)`, you can also produce a cloned array with `.slice()`. This creates a new array for us to reverse so that we don't mutate the original:

如果所需要做的只是反转一个数组（不需要转换值），那么就不需要使用 map 来克隆数组！虽然可以使用 `map(value => value)` 生成一个未修改的数组克隆，但也可以使用 `.slice()` 生成一个克隆数组。这样创建了一个新的数组，让我们反向，这样我们就不会改变原来的数组：

```js
const originalArray = [1, 2, 3, 4, 5]
const newArray = originalArray.slice().reverse()

console.log(newArray) // [5, 4, 3, 2, 1]
```

## Map Method for complex Data Operations

**Map 方法用于复杂数据操作**

While we can certainly use the Map Method for simple operations like adding 1 to every number in the array, it turns out that it's super flexible—we can do a ton of things armed with this simple method and our callback function. Let's dive into a few of them!

虽然我们当然可以使用 map 方法进行一些简单的操作，比如给数组中的每个数字都加 1，但事实证明它非常灵活（使用这个简单的方法和我们的回调函数），我们可以做很多事情。让我们来看看其中几个吧！

## Extracting object keys with map

**使用 map 提取对象的键**

For example, if we wanted to use map to extract a *single key* from *every item in an array of objects*, we could do it like this:

例如，如果我们想使用 map 从一个对象数组的每个元素中提取一个键，我们可以这样做：

```js
const originalArray = [
  { a: 1, b: 'first' },
  { a: 2, b: 'second' },
  { a: 3, b: 'third' },
];

const newArray = originalArray.map(object => object.b);

console.log(newArray); // ['first', 'second', 'third']
```

In this case, our callback function isn't doing much—it just takes each object and returns the value at the `b` key. As a result we end up transforming our array of objects into an array of strings.

在本例中，我们的回调函数没有做太多（它只是获取每个对象并返回 b 键的值）。结果，我们最终将对象数组转换为字符串数组。

## Using map to iterate through an object

**使用 map 迭代对象**

Sometimes you want to iterate through all of the items _in an object itself_ as opposed to an array of objects. A common example might be if you have an object where each key represents a unique id, but all of the values might be a similar type (sort of like a [JavaScript Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)). While `map` won't work directly on objects, we can use `map` to transform all of the values of an object through combining `map` with `Object.entries`.

有时，希望遍历一个对象本身（而不是一个对象数组）中的所有元素。常见的例子是，如果一个对象，其中每个键表示一个惟一的 id，但是所有的值都可能是类似的类型（类似于一个 [JavaScript Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)）。虽然 map 不能直接在对象上工作，但我们可以通过将 map 与 `Object.entries` 结合使用来转换对象的所有值。

`Object.entries` was added to JavaScript in [ES2017](http://2ality.com/2016/02/ecmascript-2017.html), and has [decent browser support](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries#Browser_compatibility) today (that is, if you're not supporting IE11). What `Object.entries` does is it takes an object for its argument and spits out a *two dimensional array* (an array of arrays). Each item in the array is a array containing exactly two items: the first is the key, and the second is the value. `Object.entries`, similar to `map` creates a *new array* and does not mutate the original object.

`Object.entries` 被添加到 JavaScript [ES2017](http://2ality.com/2016/02/ecmascript-2017.html) 中，现在已经有了不错的 [浏览器支持](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries#Browser_compatibility)（如果你不支持 IE11 的话）。`Object.entries` 所做的是它能接受一个对象作为其参数并输出一个二维数组（数组的数组）。数组中的每一元素都是恰好包含两个元素的数组：第一个是键，第二个是值。`Object.entries` 类似于 map 创建一个新的数组，并且不会改变原始对象。

If we leverage `Object.entries` to transform our object into an array, *then* we can use map to run whatever transformations we want on our data:

如果我们利用 `Object.entries` 将我们的对象转换为数组，那么我们就可以使用 map 来运行任何我们想要的数据转换：

```js
const object = {
  a: 1,
  b: 2,
  c: 3,
};

const array = Object.entries(object);
console.log(array); // [['a', 1], ['b', 2], ['c', 3]]

const newArray = array.map(([key, value]) => [key, value * 2]);  // 第10行
console.log(newArray); // [['a', 2], ['b', 4], ['c', 6]]
```

In line 10, we've used [array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to make our callback function a little bit easier to read. Since we know that each value in the array is a two-item array, we can assume that the first item will always be the `key` and the second item will always be the `value`. We proceed to multiply each value by 2, leaving all of the keys unaltered.

在第 10 行中，我们使用了 [数组解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 来使我们的回调函数更容易阅读。因为我们知道数组中的每个值都是一个两元素数组，我们可以假设第一个元素永远是键，第二个元素永远是值。我们继续将每个值乘以 2，保持所有键值不变。

If you're cool with your transformed data being stored in an array of arrays, feel free to stop transforming it here. But perhaps you want your data to be back in its original object shape. In order to do this we'll need to combine our `map` with a `reduce` function to zip the array back up into an object:

如果你愿意将转换后的数据存储在数组数组中，那么可以在这里停止转换。但是，也许你希望你的数据回到它原来的对象形式。为了做到这一点，我们需要结合我们的 map 和 reduce 函数压缩数组回到一个对象：

```js
...

const newObject = newArray.reduce((accumulator, [key, value]) => {
    accumulator[key] = value;
    return accumulator;
  }, {});

console.log(newObject); // { a: 2, b: 4, c: 6 }
```

By using `reduce` to turn our `mapped` array back into an object, we get a new object that has all of the transformed values *without mutating the original object*. However, you'll probably notice that we kind of had to jump through a few hoops in order to use `map` over our object. While it's useful to know how we can use `map` to iterate over object keys, I personally think that this specific case is a prime example of the `map` vs `reduce` scenario (or `map` vs `forEach`) from earlier. If we want to transform our object by multiplying each value by two, we can simply do so by combining `Object.entries` and `reduce`/`forEach`.

通过使用 reduce 将映射数组转换回对象，我们将得到一个新的对象，该对象具有所有已转换的值，而不改变原始对象。不过，你可能会注意到，为了在对象上使用 map，我们需要跨越一些障碍。虽然了解如何使用 map 遍历对象键很有用，但我个人认为，这个特定的案例是之前 map vs reduce 场景（或 map vs forEach）的主要示例。如果我们想通过将每个值乘以 2 来转换对象，我们可以简单地通过组合 `Object.entries` 和 `reduce`/`forEach`

```js
const object = {
  a: 1,
  b: 2,
  c: 3,
};

const entries = Object.entries(object);

const newObject = entries.reduce((accumulator, [key, value]) => {
  accumulator[key] = value * 2;
  return accumulator;
}, {});

// also works using forEach and mutating an object
const newObject = {};
entries.forEach(([key, value]) => {
  newObject[key] = value * 2;
});

console.log(newObject); // { a: 2, b: 4, c: 6 }
```

In conclusion, `map` *can* be used to iterate over object keys and values as long as you transform the object keys and values into an array (via `Object.entries` or `Object.keys`). However, `map` isn't going to be capable of turning your transformed array back into an object—you will need to rely on something else such as `reduce` if you need your transformed data in an object.

总之，只要将对象键和值转换为数组（通过 `Object.entries` 或 `Object.keys`），就可以使用 map 迭代对象键和值。然而，map 并不能够将转换后的数组转换回对象（如果需要将转换后的数据放入对象中，则需要依赖其他东西，如reduce）。

## Conditional Map: Changing Items in an Array

**有条件的 map：改变数组中的元素**

Another extremely useful way that we can use `map` is to only change a few items within the original array. For example, perhaps we only want to transform the numbers in an array that are 10 or above.

使用 map 的另一种非常有用的场景是只更改原始数组中的少数元素。例如，也许我们只想转换 10 及以上的数组中的数字。

```js
const originalArray = [5, 10, 15, 20];

const newArray = originalArray.map(number => {
  if (number >= 10) {
    return number * 2;
  }

  return number;
});

console.log(newArray); // [5, 20, 30, 40]
```

In this example we include a conditional statement *inside of our callback function* in order to return the modified value only when the number is 10 or higher. However, we also need to make sure we return something when we *don't* want to transform the number. We can just return `number` unchanged at the bottom of our callback function and we'll make sure that all numbers 10 and above are changed, while all numbers below 10 aren't. However, we can make this callback function with the conditional a lot shorter if we use a ternary statement to declare our conditional logic.

在本例中，我们在回调函数中包含了一个条件语句，以便仅在数字为 10 或更大时返回修改后的值。但是，当我们不想转换数字时，我们还需要确保返回一些东西。我们可以只在回调函数的底部不变地返回数字，我们将确保所有 10 及以上的数字都改变了，而所有 10 及以下的数字都没有改变。但是，如果我们使用三元语句来声明条件逻辑，我们可以使这个带有条件的回调函数变得更短。

```js
const originalArray = [5, 10, 15, 20];

const newArray = originalArray.map(number =>
  number >= 10 ? number * 2 : number,
);

console.log(newArray); // [5, 20, 30, 40]
```

The best thing about using `map` to conditionally update items in an array is that you can make that condition as strict or as loose as you'd like: you can even use `map` to update a *single item*:

使用 map 来有条件地更新数组中的元素的最好的事情是，你可以使条件如你想的那样严格或宽松：你甚至可以使用 map 来更新单个元素：

```js
const originalArray = [5, 10, 15, 20];

const newArray = originalArray.map(number =>
  number === 10 ? number * 2 : number,
);

console.log(newArray); // [5, 20, 15, 20]
```

Although this does iterate through the entire array to find and update a single item, I think it's very elegant and quite readable. I'd argue that unless you're operating on huge arrays with many, many items, you probably won't experience too much of a bottleneck using `map` and a conditional statement to update a single item.

虽然它会遍历整个数组来查找和更新单个元素，但我认为它非常优雅且可读性很强。我认为，除非在具有许多元素的大型数组上操作，否则使用 map 条件语句更新单个元素可能不会遇到太多瓶颈。

## Map Method for 2-dimensional Arrays

**map 方法在二维数组的应用**

Also called a map within a map: Sometimes you'll come across a *multidimensional array* -- that is, an array with nested arrays inside of it. You've probably seen these before, they looks like this:

也称为 map 中的 map：有时会遇到多维数组（即内部嵌套数组的数组）。你可能以前见过这些，它们看起来是这样的：

```js
const myArray = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
```

We can use `map` to operate on these arrays as well—although it will only operate on the *top-level array*. If we call `map` on our array, our callback will get called with the `[1, 2, 3]` array the first time, `[4, 5, 6]` the second, and finally `[7, 8, 9]`.

我们也可以使用 map 对这些数组进行操作（尽管它只对顶级数组进行操作）。如果我们在数组上调用 map，我们的回调将在第一次调用 [1,2,3] 数组，第二次调用 [4,5,6]，最后调用 [7,8,9]。

If you'd like to keep the array two-dimensional, then you can proceed as usual with your callback function. Just remember that the callback function receives an *array* as the first argument! If you wanted to transform the internal arrays, you'll have to do a `map` inside your `map`:

如果你希望保持数组为二维，那么你可以继续使用回调函数。请记住，回调函数接收的第一个参数是一个数组！如果你想转换内部数组，你必须在你的 map 中再做一个 map：

```js
const myArray = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];

const newArray = myArray.map(value => value.map(number => number * 2));

console.log(newArray); // [[2, 4, 6], [8, 10, 12], [14, 16, 18]]
```

However, if you'd like to turn your two-dimensional array into a *one-dimensional array* of transformed values, `map` isn't gonna be nearly as useful. What you're looking for is a `flatMap` function—which was recently released in [ES2019](http://2ality.com/2017/04/flatmap.html). What `flatMap` does is take a multidimensional array and turns it into single-dimensional array of transformed values. If you're not able to use the latest and greatest JavaScript features in ES2019, you can recreate your own `flatMap` function [by using `reduce`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap#Alternative)

然而，如果你想把你的二维数组转换成一个一维的转换值数组，map 就没有那么有用了。你正在寻找的是一个扁平化的功能，它最近在 ES2019 年发布。flatMap 所做的是将一个多维数组转换为转换后的值的一维数组。如果你不能在 ES2019 中使用最新最好的 JavaScript 特性，你可以通过使用 reduce 重新创建你自己的 flatMap 函数。

## Debugging the Map Method

**调试 map 方法**

There are a couple of common pitfalls you can run into when using JavaScript's Array Map Method. Let's dive into a few of them to give you an easier time getting started with it.

在使用 JavaScript 的数组 map 方法时，可能会遇到几个常见的陷阱。让我们深入研究其中的几个，让你更容易上手。

## When map is not defined as a function

**map 作为函数未定义**

Perhaps the most common bug that you might encounter is the following: *map is not a function*. The reason that you would come across this error is that `map` is only a method on JavaScript arrays. If you try to call `map` on an `object` or on `null` or anything else, you'll get this error.

也许你可能遇到的最常见的错误如下：map不是函数。你可能会遇到这个错误的原因是 map 只是 JavaScript 数组上的一个方法。如果你尝试在一个对象或 null 或任何其他东西上调用 map，你会得到这个错误。

This can be pretty common when you're dealing with data that you can't fully trust. For example, think of a key in an API response that could be either an array *or* `null`. Later on, you want to operate on the data, but if you just confidently use `map` on the data you could end up with this "map is not a function"-exception. However, we can use a little bit of JavaScript logic to sanitize the data *before* we do our `map`:

在处理不能完全信任的数据时，这种情况很常见。例如，考虑 API 响应中的一个键，它可以是数组或 null。稍后，希望对数据进行操作，但如果你只是自信地在数据上使用 map，那么你最终可能会得到「map不是函数」的异常。然而，我们在使用 map 之前可以使用一点 JavaScript 逻辑来清理数据：

```js
// originalArray could either be [1, 2, 3, 4] or null
const newArray = (originalArray || []).map(number => number * 2);
```

By adding `(originalArray || [])` before our `map` function, we guarantee that by the time we use `map` we're dealing with an array instead of `null`. This protects our program from raising an exception when the list is `null`. And because we're mapping over an empty array, we'll just get an empty array back in return.

通过在 map 函数之前添加 `(originalArray || [])`，我们可以保证在使用 map 时处理的是数组而不是 null。这可以防止我们的程序在为空时引发异常。如果我们操作的是一个空数组，我们会正常返回一个空数组。

Although it's a good tool to have in your toolbelt, I wouldn't lean on this trick too heavily. First off, it won't work on an object or string or any non-falsy item, so it's not 100% safe. Furthermore, if you've got data coming into your application that isn't reliable, you'll probably get more mileage out of normalizing data as it enters your app. That way, you can safely assume that you're dealing with an array instead of having to resort to [overly defensive programming](https://medium.com/@vcarl/overly-defensive-programming-e7a1b3d234c2).

虽然在工具袋中这是一个很好的工具，不过我不会过分依赖这个技巧。首先，它不能工作在一个对象或字符串或任何 non-falsy 元素，所以它不是 100% 安全。此外，你的应用程序中有一些不可靠的数据，当数据进入你的应用程序时，你可能会从规范化数据中获得更多的好处。这样，你可以安全地假设你正在处理一个数组，而不是指望过度防御性的编程。

## Logging values inside of map

Sometimes, when you're doing a `map` function you need to debug some values in the callback function. And if you're using arrow functions for your callbacks, adding a console log inside the arrow function requires adding curly braces, an explicit `return`, and the logging statement:

有时，在执行 map 函数时，需要调试回调函数中的一些值。如果你使用箭头函数来进行回调，在箭头函数中添加一个控制台日志需要添加花括号，一个显式返回和日志记录语句：

```js
// Before
const newArray = originalArray.map(value => value * 2);

// After
const newArray = originalArray.map(value => {
  console.log(value);
  return value * 2;
});
```

Over time this can begin to feel a little cumbersome, especially if you're deep into debugging some issue. However, we can leverage a little JavaScript logic to make debugging map a whole lot easier:

随着时间的推移，这可能会开始感到有点麻烦，特别是当你要深入调试某个问题时。然而，我们可以利用一点 JavaScript 逻辑，使调试 map 更容易：

```js
// Before
const newArray = originalArray.map(value => value * 2);

// After
const newArray = originalArray.map(value => console.log(value) || value * 2);
```

All we have to do is add the `console.log(value)` with a `||` in front of our normal return value! Because `console.log` returns `undefined`, the `map` callback falls back to returning the `value * 2`. This nifty trick allows us to add logging statements to our `map` callbacks without having to convert the function syntax (and makes it a lot easier to clean up `console.log` statements when you're done).

我们所要做的就是在正常返回值前面添加带有 || 的 `console.log(value)`！因为 `console.log` 返回 undefined，所以映射回调将返回 `value * 2`。这个漂亮的技巧允许我们将日志语句添加到 map 回调中，而不必转换函数语法（并且使清理 `console.log` 语句变得更加容易）。

The JavaScript `Array.map` method is extremely useful for operating and transforming sets of data. I'm a huge fan of using `map` all over the place—it's one of my favorite tools to have in my toolbelt. If you're interested in seeing more of my writing or want to hit me up with any questions about the `map` method, please feel free to contact me! You can reach me on [Twitter](https://mobile.twitter.com/benjamminj) or follow my [Medium](https://medium.com/@benjamin.d.johnson).

JavaScript 的 Array.map 方法对于数据集的操作和转换是非常有用的。我非常喜欢使用 map（这是我工具带中最喜欢的工具之一）。如果你对我的作品感兴趣，或者对 map 方法有任何疑问，请随时联系我！你可以在 [Twitter](https://mobile.twitter.com/benjamminj) 上联系我或者关注我的 [Medium](https://medium.com/@benjamin.d.johnson)。
