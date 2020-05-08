# Flatten Array using Array.flat() in JavaScript

**JavaScript 中使用 Array.flat() 扁平化数组**

> 转译自：https://www.samanthaming.com/tidbits/71-how-to-flatten-array-using-array-flat

It was always complicated to flatten an array in #JavaScript. Not anymore! ES2019 introduced a new method that flattens arrays. And there's a "depth" parameter, so you can pass in ANY levels of nesting. AMAZING 🤩

在 JavaScript 中扁平化数组总是很复杂。但现在不是了！ES2019 推出了一种扁平化数组新方法。还有一个 depth 参数，因此可以传入任何层次的嵌套。真是令人吃惊。

```js
const nested = [ ['📦', '📦'], ['📦']];

const flattened = nested.flat();

console.log(flattened);
// ['📦', '📦', '📦']
```

## Setting `depth` parameter

**设置 depth 参数**

Here's the syntax for this method:

以下是该方法的语法：

```js
array.flat(<depth>);
```

By default, `flat()` will only flatten one layer deep. In other words, `depth` is `1`.

默认情况下，`flat()` 只会使一个层变平。换句话说，`depth` 等于 `1`。

```js
array.flat();

// Same as

array.flat(1);
```

## Deeper Nested Arrays

**深层嵌套的数组**

The great thing is that this method also works beyond 1 level deep. You simply have to set the appropriate `depth` parameter to flatten deeper nested arrays.

这种方法也可以在一层以上的深度工作。只需要设置适当的 depth 参数就可以使嵌套更深的数组扁平化。

```js
const twoLevelsDeep = [[1, [2, 2], 1]];

// depth = 1
twoLevelsDeep.flat()
// [1, [2, 2], 1]

// depth = 2
twoLevelsDeep.flat(2)
// [1, 2, 2, 1]
```

## Infinitely Nested Arrays

**无限嵌套的数组**

Let's say, you want to go infinitely deep. Not a problem, we can do that too. Just pass `Infinity`.

比方说，你想要无限深入。没问题，通过 `Infinity` 参数我们也能做到。

```js
const veryDeep = [[1, [2, 2, [3,[4,[5,[6]]]]], 1]];

veryDeep.flat(Infinity);
// [1, 2, 2, 3, 4, 5, 6, 1]
```

## Array with Empty Slots

**带空槽位的数组**

One other cool thing that `flat()` can do is remove empty slots in an array.

`flat()` 可以做的另一件很酷的事情是删除数组中的空槽。

```js
const missingNumbers = [1, ,3, ,5];

missingNumbers.flat();
// [1, 3, 5];
```

## Browser Support

**浏览器支持**

`flat` is a super new feature introduced in ES2019, so forget Internet Explorer or Edge. But no surprise there 😅

`flat` 是 ES2019 推出的一个超级新功能，所以忘掉 Internet Explorer 或 Edge 吧。这不足为奇

[Browser Support: flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat#Browser_compatibility)

## Alternative Solution

**可选择的解决方案**

Since support is not great. Here are some alternative solutions.

因为支持并不多。这里有一些可供选择的解决方案。

## ES6 Solution

Here is a ES6 solution. This only work for one level nested array.

这是一个 ES6 解决方案。这只适用于一个层嵌套数组。

```js
const oneLevelDeep = [ [1, 2], [3]];

const flattened = [].concat(...oneLevelDeep);
// [1, 2, 3,]
```

## Older Browser Solution

Here's one for older browser and pre ES6. Again, this only works for one level nested array.

这是一个为老浏览器和 ES6 之前的版本准备的。同样，这只适用于一层嵌套数组。

```js
const oneLevelDeep = [ [1, 2], [3]];

const flattened = [].concat.apply([], oneLevelDeep);
// [1, 2, 3,]
```

## Recursion

**递归**

For arrays with deeper nesting, you can use recursion. Here's the solution from [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat#Alternative):

对于嵌套更深的数组，可以使用递归。这是来自 [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat#Alternative) 的解决方案：

```js
var arr1 = [1,2,3,[1,2,3,4, [2,3,4]]];

function flattenDeep(arr1) {
   return arr1.reduce((acc, val) => Array.isArray(val) ? acc.concat(flattenDeep(val)) : acc.concat(val), []);
}
flattenDeep(arr1);// [1, 2, 3, 1, 2, 3, 4, 2, 3, 4]
```

## Community Input

**社区观点**

💬 Got any interesting use cases of Array.flat()?

有什么关于 Array.flat() 的有趣用例吗？

- [@hocine_abdellatif](https://www.instagram.com/hocine_abdellatif/): A little far use case but probably worth mentioning, in machine learning if you have for example an array of generations, each generation is an array of students, if you want to have every student from every generation,this method will be very practical.

有点偏门的用例，但可能值得一提，在机器学习中，如果你有一个 generation 数组，每个 generation 都是一个学生数组，如果你想让每个学生都来自每个 generation，这个方法会很实用。

- @devjacks: I wrote a code test with a problem like this a few weeks ago. This would have made my life so much easier! 😂

几个星期前，我编写了一个带有类似问题的代码测试。这会让我的生活更轻松！

```js
// Please write a function to search for 213

function search(needle, haystack) {}

const haystack =[1, 4, [5,6,7, [8, 18, [34,177,[98,[210,[213]]]]]]];
const needle = 213;
```

## Resources

- [MDN Web Docs: flat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
- [Alligator: Flatten Arrays in Vanilla JavaScript with flat and flatMap](https://alligator.io/js/flat-flatmap/)
- [Flatten array of arrays with JavaScript](http://joelabrahamsson.com/flatten-array-of-arrays-with-javascript/)
- [Stack Overflow: Merge/flatten an array of arrays](https://stackoverflow.com/questions/10865025/merge-flatten-an-array-of-arrays)
- [Flattening multidimensional Arrays in JavaScript](https://www.jstips.co/en/javascript/flattening-multidimensional-arrays-in-javascript/)
- [Flattening Arrays in JavaScript with flat and flatMap](https://davidtang.io/2019/03/09/flattening-arrays-in-javascript-with-flat-and-flatMap.html)

