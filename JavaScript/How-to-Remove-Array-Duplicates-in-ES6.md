# How to Remove Array Duplicates in ES6

**ES6 中如何删除数组重复元素**

> 转译自：https://www.samanthaming.com/tidbits/43-3-ways-to-remove-array-duplicates

Here are 3 ways to filter out duplicates from an array and return only the unique values. My favorite is using Set cause it’s the shortest and simplest 😁

有三种方法可以过滤掉数组中的重复元素并只返回唯一的值。我最喜欢用 Set，因为它最简洁最简单。

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

// 1: "Set"
[...new Set(array)];

// 2: "Filter"
array.filter((item, index) => array.indexOf(item) === index);

// 3: "Reduce"
array.reduce(
  (unique, item) => (unique.includes(item) ? unique : [...unique, item]),
  []
);

// RESULT:
// ['🐑', 1, 2, 3];
```

## 1. Using `set`

Let me start first by explaining what Set is:

首先让我解释一下 Set：

`Set` is a new data object introduced in ES6. Because `Set` only lets you store unique values. When you pass in an array, it will remove any duplicate values.

`Set` 是在 ES6 中引入的一个新的数据对象。因为 `Set` 只允许存储唯一元素。当传入一个数组时，它将删除任何重复的元素。

Okay, let's go back to our code and break down what's happening. There are 2 things going on:

让我们回到我们的代码，分析一下发生了什么。有两件事正在发生：

1. First, we are creating a new `Set` by passing an array. Because `Set` only allows unique values, all duplicates will be removed.

首先，我们通过传递一个数组来创建新的 `Set`。因为 `Set` 只允许唯一的值，所以所有重复的值都将被删除。

2. Now the duplicates are gone, we're going to convert it back to an array by using the spread operator `...`

现在重复元素已经没有了，我们通过使用扩展运算符 `...` 将它转换回一个数组。

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

// Step 1
const uniqueSet = new Set(array);
// Set { '🐑', 1, 2, 3 }

// Step 2
const backToArray = [...uniqueSet];
// ['🐑', 1, 2, 3]
```

## Convert `Set` to an Array using `Array.from`

**使用 `Array.from` 将 `Set` 转为数组**

Alternatively, you can also use `Array.from` to convert a `Set` into an array:

或者，你可以用 `Array.from` 将 `Set` 转为数组：

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

Array.from(new Set(array));

// ['🐑', 1, 2, 3]
```

## 2: Using `filter`

In order to understand this option, let's go through what these two methods are doing: `indexOf` and `filter`

为了理解这个方式，我们来看一下这两个方法的作用：`indexOf` 和 `filter`

## indexOf

The `indexOf` method returns the first index it finds of the provided element from our array.

`indexOf` 返回它从数组中找到的所提供元素首次出现的索引。

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

array.indexOf("🐑"); // 0
array.indexOf(1); // 1
array.indexOf(2); // 2
array.indexOf(3); // 5
```

## filter

The `filter()` method creates a new array of elements that pass the conditional we provide. In other words, if the element passes and returns `true`, it will be included in the filtered array. And any element that fails or return `false`, it will be NOT be in the filtered array.

`filter()` 方法创建一个包含新元素的数组，这些元素符合我们提供的条件。换句话说，如果元素满足条件返回 `true`，它将被包含在经过筛选的数组中。任何失败或返回 `false` 的元素都不会出现在过滤后的数组中。

Let's step in and walk through what happens as we loop through the array.

让我们逐步了解当循环数组时发生了什么。

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

array.filter((item, index) => {
  console.log(
    // a. Item
    item,
    // b. Index
    index,
    // c. indexOf
    array.indexOf(item),
    // d. Condition
    array.indexOf(item) === index
  );

  return array.indexOf(item) === index;
});
```

Below is the output from the console.log showed above. The duplicates are where the index doesn’t match the indexOf. So in those cases, the condition will be false and won’t be included in our filtered array.

下面是 `console.log` 的输出。重复元素是指 `index` 与 `indexOf` 不匹配的元素。在这些情况下，条件为 `false`，不包含在过滤后的数组中。

| Item | Index | indexOf | Condition |
| :--: | :---: | :-----: | :-------: |
|  🐑  |   0   |    0    |   true    |
|  1   |   1   |    1    |   true    |
|  2   |   2   |    2    |   true    |
|  🐑  |   3   |    0    |   false   |
|  🐑  |   4   |    0    |   false   |
|  3   |   5   |    5    |   true    |

## Retrieve the duplicate values

**检索重复元素**

We can also use the filter method to retrieve the duplicate values from the array. We can do this by simply adjusting our condition like so:

我们还可以使用 filter 方法从数组中检索重复的元素。我们可以这样简单地调整我们的条件：

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

array.filter((item, index) => array.indexOf(item) !== index);

// ['🐑','🐑']
```

Again, let step through this and see the output:

同样，让我们一步一步的来看输出：

| Item | Index | indexOf | Condition |
| :--: | :---: | :-----: | :-------: |
|  🐑  |   0   |    0    |   false   |
|  1   |   1   |    1    |   false   |
|  2   |   2   |    2    |   false   |
|  🐑  |   3   |    0    |   true    |
|  🐑  |   4   |    0    |   true    |
|  3   |   5   |    5    |   false   |

## 3: Using `reduce`

The `reduce` method is used to reduce the elements of the array and combine them into a final array based on some reducer function that you pass.

`reduce` 方法用于减少数组中的元素，并根据给出的某个 reducer 函数将它们组合成最终的数组。

In this case, our reducer function is checking if our final array contains the item. If it doesn't, push that item into our final array. Otherwise, skip that element and return just our final array as is (essentially skipping over that element).

在本例中，我们的 reducer 函数将检查最终数组是否包含该项。如果没有，则将该项推入最终数组。否则，跳过该元素并仅返回我们的最终数组（实际上跳过了该元素）。

Reduce is always a bit more tricky to understand, so let's also step into each case and see the output:

Reduce 总是比较难理解，所以我们也来看看每种情况下的输出：

```js
const array = ["🐑", 1, 2, "🐑", "🐑", 3];

array.reduce((unique, item) => {
  console.log(
    // a. Item
    item,
    // b. Final Array (Accumulator)
    unique,
    // c. Condition (Remember it only get pushed if this returns `false`)
    unique.includes(item),
    // d. Reducer Function Result
    unique.includes(item) ? unique : [...unique, item]
  );

  return unique.includes(item) ? unique : [...unique, item];
}, []); // 👈 The initial value of our Accumulator is an empty array

// RESULT:
// ['🐑', 1, 2, 3];
```

And here's the output from the console.log:

这是 console.log 的输出：

| Item | Accumulator (BEFORE Reducer Function) | Push to Accumulator? | Accumulator (AFTER Reducer Function) |
| :--: | :-----------------------------------: | :------------------: | :----------------------------------: |
|  🐑  |                  []                   |         Yes          |               [ '🐑' ]               |
|  1   |                ['🐑']                 |         Yes          |             [ '🐑', 1 ]              |
|  2   |              [ '🐑', 1 ]              |         Yes          |            [ '🐑', 1, 2 ]            |
|  🐑  |            [ '🐑', 1, 2 ]             |          No          |            [ '🐑', 1, 2 ]            |
|  🐑  |            [ '🐑', 1, 2 ]             |          No          |            [ '🐑', 1, 2 ]            |
|  3   |            [ '🐑', 1, 2 ]             |         Yes          |          [ '🐑', 1, 2, 3 ]           |

## Resources

- [MDN Web Docs - Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [MDN Web Docs - Filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [MDN Web Docs - Reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
- [GitHubGist: Remove duplicates from JS array](https://gist.github.com/telekosmos/3b62a31a5c43f40849bb)
- [CodeHandbook: How to Remove Duplicates from JavaScript Array](https://codehandbook.org/how-to-remove-duplicates-from-javascript-array/)
