# Refactoring the `if` Condition

**重构 if 条件语句**

> 转译自：https://www.samanthaming.com/tidbits/26-refactoring-if-condition

Refactoring the `if` condition using our falsy value concept. Remember last week I talked about ["Falsy Values"](http://www.samanthaming.com/tidbits/25-js-essentials-falsy-values). Here’s where it becomes handy. Since values in JS will evaluate to either true or false. You can leverage that concept to shorten your condition in the `if` statement. Whoa! isn’t your code so much cleaner! That’s why knowing your JS essentials are so important 😉 👍

使用 falsy 值的概念重构 `if` 条件。记得上周我讲过 [「Falsy 值」（暂缺译文）](http://www.samanthaming.com/tidbits/25-js-essentials-falsy-values)。讨论了它的方便之处。因为 JS 中的值要么为 true，要么为 false。你可以在 if 语句中利用这个概念来缩短你的条件。哇！你的代码不是更简洁了吗？That’s why knowing your JS essentials are so important.

译注：falsy 值（虚值) 是在 Boolean 上下文中认定为 false 的值。可参见 https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy

```js
// Refactoring the `if` condition

let isPerson = false;

// ❌
if (isPerson === false)

// ✅ Much better
if (!isPerson)
```

## More Examples

**更多例子**

If you're just checking if a value is truthy or falsy, you can skip the comparison and just pass in the value. Since the value will have a boolean equivalent - it will evaluate to either true or false.

如果只是检查某个值是 truthy 的还是 falsy，那么可以跳过比较，直接传入值。由于该值将具有一个布尔值（它的值将为 true 或 false）。

```js
let isPerson = false;

// ❌
if (isPerson === false)
if (isPerson === null)
if (isPerson === undefined)
if (isPerson === 0)
if (isPerson === "")
if (isPerson === NaN)

// ✅ Much better
if (!isPerson)
```

## Using falsy values in Ternary Operators

**在三元运算符中使用 falsy 值**

```js
// ❌
isPerson === false ? "👻" : "🙂";

// ✅ Much better
isPerson ? "👻" : "🙂";
```

## Community Examples

**社区案例**

## false vs falsy value

Note the 2 statements are slightly different:

注意这两种说法略有不同：

```js
// This is checking if the value is a "false" value
if (isPerson === false)

// This is checking if value is "falsy"
if(!isPerson)
```

Thanks: [@RanqueBenoit](https://twitter.com/RanqueBenoit/status/1023284890723393541)

## Good Variable Names

**好的变量名称**

@jsawbrey: I find the "is" naming convention also very helpful. I make them all the time. "isThisThing". In TypeScript, that naming convention helps a lot when making type guards.

我发现「is」的命名约定同样有好处。我一直都在做。「isThisThing」。在 TypeScript 中，命名约定在创建类型保护时非常有用。

@jsawbrey: if a function sets a variable, "setThing" is good. If a function toggles a variable (on/off, true/false), "toggleThing" is good.

如果函数设置一个变量，「setThing」是好的。如果函数切换一个变量（on/off、true/false），「toggleThing」是好的。

Thanks: [@jsawbrey](https://twitter.com/jsawbrey/status/1023306248542871552)

## Good Variable Name: On vs Handle

**好的变量名称：On 和 Handle**

On the note of good variable names. Here's another cool example. This is an excerpt from Adam's medium article, [Use React to make a photo follow the mouse](https://medium.com/@agm1984/use-react-to-make-a-photo-follow-the-mouse-aka-transform-perspective-or-tilt-7c38f1b3a623).

注意良好的变量命名。这是另一个很酷的例子。这是摘自 Adam 的 medium 文章，[使用 React 实现图片跟随鼠标](https://medium.com/@agm1984/use-react-to-make-a-photo-follow-the-mouse-aka-transform-perspective-or-tilt-7c38f1b3a623)

@agm1984: Notice how we called the Class Methods handle rather than on. I like to remind people about the distinction between the two. on refers to the event on which we are doing something. handle refers to the action we are taking or the result of the event.

Notice how we called the Class Methods handle rather than on. 我想提醒大家这两者之间的区别。on 指我们正在做某事的事件。handle 是指我们正在采取的行动或事件的结果。

@agm1984: If we were delegating the handling up to a parent or calling back to some other location, we should use on. This is why you see callbacks that look like this:

如果我们将处理委托给父进程或者调用其他位置，我们应该使用 on。这就是为什么你会看到这样的回调：

```js
<button onClick={onClick}>Submit</button>
```

Thanks: [@agm1984](https://twitter.com/agm1984/status/1023362508772401152)

## Resources

- [12 Good JavaScript Shorthand Techniques](https://hackernoon.com/12-amazing-javascript-shorthand-techniques-fef16cdbc7fe)
- [Code Tidbit: JS Essentials - Falsy Values](http://www.samanthaming.com/tidbits/25-js-essentials-falsy-values)
