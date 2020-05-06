# Code Smell: No AND in Function name

**函数名不要有 AND 在其中**

> 转译自：https://www.samanthaming.com/tidbits/66-no-and-in-function-name

Function should adhere to the Single Responsibility Principle - meaning it should do one thing and one thing only. So if your function name includes "AND", it means you're doing too much. Solution? Remove the "AND" and split it into separate functions 👍

函数应该坚持单一责任原则，即只做一件事。所以如果你的函数名包含 "AND"，那就意味着你做得太多了。解决方案呢？删除 "AND" 并将其拆分为单独的函数。

```js
// ❌ Bad
function teaAndSugar() {}

// ✅ Better
function tea() {}
function sugar() {}
```

## Single Responsibility Principle (SRP)

**单一责任原则（SRP）**

> Every module should have one single responsibility. This means two separate concerns/responsibilities/tasks should always be implemented in separate modules.

每个模块都应该有一个单独的职责。这意味着两个独立的关注点/职责/任务应该总是在独立的模块中实现。

[Principles Wiki: SRP](http://www.principles-wiki.net/principles:single_responsibility_principle)

And the Rationale behind is:

背后的理由是：

> When this rule is not adhered to, one module has several tasks. If one of these tasks changes, there is the risk that this also has an effect on the other task that normally should be independent. Thus unrelated functionality may break.

如果不遵守该规则，一个模块就会涉及多个任务。如果其中一个任务发生了变化，就有可能对另一个通常应该独立的任务产生影响。因此，不相关的功能可能会中断。

When you follow the Single Responsibility Principle, you create a code base that is more flexible and modular.

当你遵循单一职责原则时，可创建一个更加灵活和模块化的代码基础。

## SRP Benefits in Non Dev Terms

**SRP 在非开发方面的好处**

Let's try to explain this in non-dev terms. Let's say you're a chef and you're trying to order ingredients for your kitchen. Two sellers approach you with their options. Seller A tells you, we have all the ingredients you need and everything is mixed for you. Seller B tells you, we have all the ingredients you need and will sell them to you separately. Which one would you buy? Sure Seller A option is pretty good because everything is pre-mixed. BUT the recipes you can make is very limited because you're confined to recipes that require all 3 ingredients. However, with Seller B, the recipes you make are endless. You can make desserts and savory recipes 👩‍🍳

让我们试着用非开发术语来解释这一点。假设你是一名厨师，正在为厨房订购食材。两个卖家向你提供他们的选择。卖家 A 告诉你，我们有你需要的所有配料，一切都为你准备好了。卖家 B 告诉你，我们有你需要的所有原料，会分开卖给你。你会买哪一个？当然卖方 A 是相当好的，因为一切都是预先混合的。但是你能做的食谱是非常有限的，因为你被限制在需要所有三种配料的食谱里。然而，对于卖家 B，你做的食谱是无止境的。你可以制作甜点和美味的食谱。

Seller A:

卖家 A：

Buying pre-mixed ingredients limit you to recipes that require ALL 3 items.

购买预先混合的原料，将你的食谱限制在三项原料中。

```js
function flourAndSugarAndEgg() {}
```

Seller B:

卖家 B：

Buying individual ingredients removes the limitation and allows you to create far more recipes 🏆

购买单独的食材消除了这个限制，允许你创建更多的食谱。

```js
function flour() {}

function sugar() {}

function egg() {}
```

## Maintainability Benefit

**可维护性的好处**

Another great thing with sticking with this rule is maintainability. When you just start out, sure it may seem a lot easier just to put everything together. But I guarantee you that over time, as you add more functionality or make changes, one singular function that does everything becomes very messy to maintain.

坚持这个规则的另一个好处是可维护性。刚开始的时候，把所有的东西放在一起看起来会容易得多。但我可以向你保证，随着时间的推移，当你添加更多的功能或做出改变时，只有一个单一的函数，做任何事情就变得非常混乱，难以维护。

## Explained in Non-Dev Terms

**用非开发术语解释**

Let's explain this with another non-dev term explanation. Let's say you're a big Lego builder and you bought yourself a brand new Lego set. You're super excited you open the new set and dump all the pieces into a container. Unfortunately, you have a final exam the next week so you have no time to build it yet. A few weeks later, your rich aunt buys a few more Lego sets. I mentioned your aunt is rich because we all know Lego sets are ridiculously expensive 😂. Again you open the new set and dump them in the same container, thinking that it's no big deal. Not to be outdone by your rich aunt, your rich grandma also wants to win your love, so she buys more Lego sets for you. Again, you didn't think it'd be a big deal, you open everything and dump them all in the same container. Okay, a few weeks have passed and now you're ready to build your Lego sets. Guess what happened? You're now knocking your head against the wall. Because all the pieces are mixed into one single container and you don't know which is which. However, if you had kept all the Lego sets in its own container, you wouldn't have this problem 💩

让我们从另一个非开发角度来解释这个问题。假设你是一个大的乐高建造者，你给自己买了一套全新的乐高积木。你超级兴奋地打开新积木，把所有的碎片都倒进一个容器里。不幸的是，下周你有一个期末考试，所以你还没有时间去完成它。几周后，你有钱的阿姨又买了几套乐高玩具。我提到你阿姨很有钱，因为我们都知道乐高玩具贵得离谱。再次打开新的积木并将它们转储到同一个容器中，你认为这没什么大不了的。为了不被你有钱的阿姨超越，你有钱的奶奶也想赢得你的爱，所以她给你买了更多的乐高玩具。同样，你不认为这是什么大问题，你打开所有的东西，把它们也都倒在同一个容器里。好了，几个星期过去了，现在你准备好建造你的乐高积木了。猜猜发生了什么？你现在正在碰壁。因为所有的碎片都混合在一个容器里，你不知道头绪。但是，如果事先将所有的乐高积木保存在「独立」的容器中，就不会出现这个问题。

That's why one function should do one thing and one thing only. When it's doing more than one thing. It may not seem like it now, but over time and with changing requirements, this function will become bloated and it will become extremely difficult to maintain.

这就是为什么一个函数应该只做一件事。当它在做不止一件事的时候。现在看起来可能没什么，但是随着时间的推移和需求的变化，这个函数将变得非常臃肿，并且将变得非常难以维护。

## Community Input

**社区观点**

- [@MaxStalker](https://twitter.com/samantha_ming/status/1204431457843761154): function names are much more helpful when they "command" action (in form of a imperative verb)

函数名体现「命令式」的动作时更有用（以祈使动词的形式）

译注：祈使句（Imperative Sentence）用于表达命令、请求、劝告、警告、禁止等句子叫做祈使句，祈使句最常用于表达命令，因此在学校文法中也常称为命令句。

- [@Skateside](https://twitter.com/Skateside/status/1142508099753975809): Another pro tip: start the function names with a verb. This makes your intentions clearer and easier to explain - "this one makes tea, that one adds sugar."

另一个专业提示：以动词开头的函数名。这让你的意图更清晰，也更容易解释「这个用来泡茶，那个用来加糖。」

```js
function makeTea() {}
function addSugar() {}
```

- [@sunnysingh.io](https://www.instagram.com/sunnysingh.io/): Generic functions like `getData()` 😝 Um... what type of data? Unless it's a top-level utility, I like being specific like `getUser()`, `getPost()`, etc.

像 `getData()` 这样的通用函数，结果是什么类型的数据？除非它是一个顶层工具，否则我喜欢将它指定为 `getUser()`、`getPost()` 等。

- [@Mouadovicc](https://twitter.com/Mouadovicc/status/1142524184997838848): I prefer to use `drinkTea` and `drinkSugar` by replacing AND by a unified word in this case is drink

例如 drink 是统一的单词的情况下，我更喜欢用 `drinkTea` 和 `drinkSugar` 来代替 AND。

- [@SohelAhmedM](https://twitter.com/samantha_ming/status/1204431457843761154): Very helpful. I used to do doOneTwoAndThree(). Mistake corrected

很有帮助，纠正了以往的错误。我过去常这么干 doOneTwoAndThree()

## Resources

- [Things I Learnt The Hard Way](https://blog.juliobiason.net/thoughts/things-i-learnt-the-hard-way/)
- [Why is the use of conjunctions in method names a bad naming convention?](https://softwareengineering.stackexchange.com/questions/255669/why-is-the-use-of-conjunctions-in-method-names-a-bad-naming-convention)
- [SamanthaMing: Bad Variable Names to Avoid](https://www.samanthaming.com/tidbits/36-bad-variable-names-to-avoid)
- [SamanthaMing: How to give your boolean variables a better name](https://www.samanthaming.com/tidbits/34-better-boolean-variable-names)
- [Understanding SOLID Principles: Single Responsibility](https://codeburst.io/understanding-solid-principles-single-responsibility-b7c7ec0bf80)
- [Principles Wiki: SRP](http://www.principles-wiki.net/principles:single_responsibility_principle)
