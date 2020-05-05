# 3 Ways to Clone Objects in JavaScript

**JavaScript 中拷贝对象的三种方法**

> 转译自：https://www.samanthaming.com/tidbits/70-3-ways-to-clone-objects

Because objects in JavaScript are references values, you can't simply just copy using the `=`. But no worries, here are 3 ways for you to clone an object 👍

因为 JavaScript 中的对象是引用值，所以不能简单地使用 `=` 进行复制。但是不用担心，这里有三种方法可以让你拷贝一个对象。

```js
const food = { beef: '🥩', bacon: '🥓' }


// "Spread"
{ ...food }


// "Object.assign"
Object.assign({}, food)


// "JSON"
JSON.parse(JSON.stringify(food))

// RESULT:
// { beef: '🥩', bacon: '🥓' }
```

## Objects are Reference Types

**对象是引用类型**

Your first question might be, why can't I use `=`. Let's see what happens if we do that:

你的第一个问题可能是，为什么不能使用 `=`。让我们看看如果那么做会发生什么：

```js
const obj = { one: 1, two: 2 };

const obj2 = obj;

console.log(
  obj, // {one: 1, two: 2};
  obj2 // {one: 1, two: 2};
);
```

So far, both object seems to output the same thing. So no problem, right. But let's see what happens if we edit our second object:

到目前为止，两个对象似乎输出相同的内容。所以没问题。但让我们看看如果我们编辑第二个对象会发生什么：

```js
const obj2.three = 3;

console.log(obj2);
// {one: 1, two: 2, three: 3}; <-- ✅

console.log(obj);
// {one: 1, two: 2, three: 3}; <-- 😱
```

WTH?! I changed `obj2` but why was `obj` also affected. That's because Objects are reference types. So when you use `=`, it copied the pointer to the memory space it occupies. Reference types don't hold values, they are a pointer to the value in memory.

什么? !我改变了 `obj2` ，但为什么 `obj` 也受到影响。这是因为对象是引用类型。因此，当使用 `=` 时，它将指针复制到它占用的内存空间。引用类型不保存值，它们是指向内存中的值的指针。

If you want to learn more about this, check out Gordon's Zhu [Watch and Code](https://watchandcode.com/) course. It's free to enroll and watch the video "Comparison with objects". He gives a super awesome explanation on it.

如果你想了解更多这方面的知识，请查看 Gordon's Zhu [Watch and Code](https://watchandcode.com/) 的课程。注册并观看「与对象比较」视频是免费的。他给了一个超级棒的解释。

## 1. Using Spread

Using spread will clone your object. Note this will be a shallow copy. As of this post, the spread operator for cloning objects is in Stage 4. So it's not officially in the specifications yet. So if you were to use this, you would need to compile it with Babel (or something similar).

使用扩展运算符拷贝你的对象。注意，这是一个浅拷贝。在这篇文章中，拷贝对象的扩展运算符处于 Stage 4。所以它还没有正式的规范。因此，如果要使用它，需要使用 Babel（或类似的东西）编译它。

```js
const food = { beef: "🥩", bacon: "🥓" };

const cloneFood = { ...food };

console.log(cloneFood);
// { beef: '🥩', bacon: '🥓' }
```

## 2. Using Object.assign

Alternatively, `Object.assign` is in the official released and also create a shallow copy of the object.

或者，使用 `Object.assign`，它是正式发布的，会创建一个浅拷贝的对象。

```js
const food = { beef: "🥩", bacon: "🥓" };

const cloneFood = Object.assign({}, food);

console.log(cloneFood);
// { beef: '🥩', bacon: '🥓' }
```

Note the empty {} as the first argument, this will ensure you don't mutate the original object 👍

注意空的 {} 是第一个参数，确保不改变原来的对象。

## 3. Using JSON

This final way will give you a deep copy. Now I will mention, this is a quick and dirty way of deep cloning an object. For a more robust solution, I would recommend using something like [lodash](https://lodash.com/docs/#cloneDeep)

最后一种方法会得到一个深拷贝。现在我要提一下，这是一种快速而不完善的深拷贝对象的方法。更健壮的解决方案，我建议使用 [lodash](https://lodash.com/docs/#cloneDeep)

```js
const food = { beef: "🥩", bacon: "🥓" };

const cloneFood = JSON.parse(JSON.stringify(food));

console.log(cloneFood);
// { beef: '🥩', bacon: '🥓' }
```

## Lodash DeepClone vs JSON

Here's a comment from the community. Yes, it was for my previous post, [How to Deep Clone an Array](https://www.samanthaming.com/tidbits/50-how-to-deep-clone-an-array). But the idea still applies to objects.

下面是来自社区的评论。是的，这是我之前的帖子，[How to Deep Clone an Array](https://www.samanthaming.com/tidbits/50-how-to-deep-clone-an-array)。但这个想法仍然适用于对象。

[Alfredo Salzillo](https://dev.to/alfredosalzillo/comment/96ne): I'd like you to note that there are some differences between deepClone and JSON.stringify/parse.

Alfredo Salzillo：我希望你注意到，深拷贝与 JSON.stringify/parse 之间存在一些差异。

- **JSON.stringify/parse** only work with Number and String and Object literal without function or Symbol properties.
- **deepClone** work with all types, function and Symbol are copied by reference.

Here's an example:

这是一个例子：

```js
const lodashClonedeep = require("lodash.clonedeep");

const arrOfFunction = [
  () => 2,
  {
    test: () => 3
  },
  Symbol("4")
];

// deepClone copy by refence function and Symbol
console.log(lodashClonedeep(arrOfFunction));
// JSON replace function with null and function in object with undefined
console.log(JSON.parse(JSON.stringify(arrOfFunction)));

// function and symbol are copied by reference in deepClone
console.log(
  lodashClonedeep(arrOfFunction)[0] === lodashClonedeep(arrOfFunction)[0]
);
console.log(
  lodashClonedeep(arrOfFunction)[2] === lodashClonedeep(arrOfFunction)[2]
);
```

[@OlegVaraksin](https://twitter.com/OlegVaraksin/status/1152850845303824384): The JSON method has troubles with circular dependencies. Furthermore, the order of properties in the cloned object may be different.

@OlegVaraksin：JSON 方法有循环依赖的问题。此外，拷贝对象中的属性顺序可能不同。

## Shallow Clone vs Deep Clone

**浅拷贝和深拷贝**

When I used spread `...` to copy an object, I'm only creating a shallow copy. If the array is nested or multi-dimensional, it won't work. Here's our example we will be using:

当我使用扩展运算符拷贝对象时，只创建了浅拷贝。如果数组是嵌套的或多维的，就不能工作。下面是我们要用的例子：

```js
const nestedObject = {
  country: '🇨🇦',
  {
    city: 'vancouver'
  }
};
```

## Shallow Copy

**浅拷贝**

Let's clone our object using spread:

让我们用扩展运算符拷贝对象：

```js
const shallowClone = { ...nestedObject };

// Changed our cloned object
clonedNestedObject.country = "🇹🇼";
clonedNestedObject.country.city = "taipei";
```

So we changed our cloned object by changing the city. Let's see the output.

我们通过改变城市来改变拷贝对象。让我们看看输出。

```js
console.log(shallowClone);
// {country: '🇹🇼', {city: 'taipei'}} <-- ✅

console.log(nestedObject);
// {country: '🇨🇦', {city: 'taipei'}} <-- 😱
```

A shallow copy means the first level is copied, deeper levels are referenced.

浅拷贝意味着第一个层次被拷贝，更深层次被引用。

## Deep Copy

**深拷贝**

Let's take the same example but applying a deep copy using "JSON"

让我们举一个相同的例子，但是使用「JSON」应用深度拷贝：

```js
const deepClone = JSON.parse(JSON.stringify(nestedObject));

console.log(deepClone);
// {country: '🇹🇼', {city: 'taipei'}} <-- ✅

console.log(nestedObject);
// {country: '🇨🇦', {city: 'vancouver'}} <-- ✅
```

As you can see, the deep copy is a true copy for nested objects. Often time shallow copy is good enough, you don't really need a deep copy. It's like a nail gun vs a hammer. Most of the time the hammer is perfectly fine. Using a nail gun for some small arts and craft is often case an overkill, a hammer is just fine. It's all about using the right tool for the right job 🤓

正如所看到的，深拷贝是嵌套对象的真正复制。通常浅拷贝就足够了，你不需要深拷贝。这就像钉枪和锤子的区别。大多数情况下，锤子是完美的。对于一些小型工艺品来说，使用钉枪通常是一种大材小用，用锤子就可以了。关键是要为正确的工作使用正确的工具。

## Performance

**性能**

Unfortunately, I can't write a test for spread because it's not officially in the spec yet. Nevertheless, I included it in the test so you can run it in the future 😝. But the result shows `Object.assign` is a lot faster than `JSON`.

不幸的是，我不能为扩展运算符编写测试，因为它还没有正式在规范中。尽管如此，我还是将它包含在测试中，以便将来可以运行它。但结果显示 `Object.assign` 比 `JSON` 要快得多。

[Performance Test](https://jsperf.com/3-ways-to-clone-object/1)

## Community Input

**社区意见**

## Object.assign vs Spread

[@d9el](https://dev.to/adameier/comment/d9el): It's important to note that Object.assign is a function which modifies and returns the target object. In Samantha's example using the following,

@d9el：需要注意的是 Object.assign 是一个修改和返回目标对象的函数。在 Samantha 的例子中，

```js
const cloneFood = Object.assign({}, food);
```

`{}` is the object that is modified. The target object is not referenced by any variable at that point, but because `Object.assign` returns the target object, we are able to store the resulting assigned object into the `cloneFood` variable. We could switch our example up and use the following:

`{}` 是被修改的对象。此时目标对象不被任何变量引用，而是因为 `Object.assign` 返回目标对象，我们能够将结果赋值对象存储到 `cloneFood` 变量中。我们可以切换我们的例子，使用以下内容：

```js
const food = { beef: "🌽", bacon: "🥓" };

Object.assign(food, { beef: "🥩" });

console.log(food);
// { beef: '🥩', bacon: '🥓' }
```

Obviously, the value of `beef` in our food object is wrong, so we can assign the correct value of `beef` using `Object.assign`. We aren't actually using the returned value of the function at all, but we are modifying our target object which we have referenced with the const `food`.

显然，我们的 food 对象中的 `beef` 的值是错误的，因此我们可以使用 `Object.assign` 来分配 `beef` 的正确值。我们实际上根本没有使用函数的返回值，但是我们正在修改我们的目标对象，我们已经在 `const food` 中引用了它。

Spread on the other hand is an operator which copies properties of one object into a new object. If we wanted to replicate the above example using spread to modify our variable `food...`

另一方面，扩展运算符将一个对象的属性复制到一个新对象中。如果我们想复制上面的例子，使用扩展运算符来修改我们的变量 `food…`

```js
const food = { beef: '🌽', bacon: '🥓' };

food = {
  ...food,
  beef: '🥩',
}
// TypeError: invalid assignment to const `food'
```

`...` we get an error, because we use spread when creating new objects, and therefore are assigning a whole new object to `food` which was declared with `const`, which is illegal. So we can either choose to declare a new variable to hold our new object in, like the following:

我们会得到一个错误，因为我们在创建新对象时使用了扩展运算符，将整个新对象赋值给 `food`，它声明为 `const`，所以赋值是非法的。因此，我们可以选择声明一个新变量来保存我们的新对象，如下所示：

```js
const food = { beef: "🌽", bacon: "🥓" };

const newFood = {
  ...food,
  beef: "🥩"
};

console.log(newFood);
// { beef: '🥩', bacon: '🥓' }
```

or we could declare `food` with `let` or `var` which would allow us to assign a whole new object:

或者将 `food` 用 `let` 或 `var` 声明，以便允许我们赋值：

```js
let food = { beef: '🌽', bacon: '🥓' };

food = {
  ...food,
  beef: '🥩',
}

console.log(food);
// { beef: '🥩', bacon: '🥓' }
```

Thanks: [@d9el](https://dev.to/adameier/comment/d9el)

## Deep Clone using External Libraries

**使用外部库进行深拷贝**

- @lesjeuxdebebel: Personally I use jquery with `$.extend()`; function
- @edlinkiii: underscore.js ~~ `_.clone()`
- @Percy_Burton: The only way I've known to do this is with the Lodash library, cloneDeep method.

## More Ways using JavaScript

@hariharan_d3v: `Object.fromEntries(Object.entries(food))` shallow clones the object.

译注：原文 shallow 带中括号，可能是笔误

## Resources

- [MDN Web Docs: Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
- [Stack Overflow: What is the most efficient way to deep clone an object in JavaScript?](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript)
- [2ality: Rest/Spread Properties](https://2ality.com/2016/10/rest-spread-properties.html)
- [Stack Overflow: Object spread vs. Object.assign](https://stackoverflow.com/questions/32925460/object-spread-vs-object-assign)
