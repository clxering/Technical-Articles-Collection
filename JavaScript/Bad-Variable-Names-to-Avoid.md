# Bad Variable Names to Avoid 🙅‍♀️

**避免糟糕的变量名**

> 转译自：https://www.samanthaming.com/tidbits/36-bad-variable-names-to-avoid

In an earlier post, I mentioned [how you can give your boolean variables a better name](https://www.samanthaming.com/tidbits/34-better-boolean-variable-names) by prefixing it with is/has/can. Now I want to give advice on bad variable names you should avoid 💥

在之前的一篇文章中，我提到了 [如何给布尔变量起一个更好的名字（暂缺译文）]()，方法是在它前面加上 is/has/can。现在我想就应该避免的糟糕的变量名给出一些建议。

Reading code is tough already, so don’t make it even more complicated by using names that they have to guess. Your code should convey understanding. So it’s better to use words that everyone can understand. This will make it a lot easier for others and even yourself to work with your code. Yay! Win Win 😊

阅读代码已经很困难了，所以不要使用需要猜测的变量名称来使这件事变得更加复杂。你的代码应该传达理解。所以最好使用大家都能理解的单词。这将使其他人甚至自己更容易地使用代码。

```js
// Avoid Single Letter Names
let n = 'use name instead'

// Avoid Acronyms
let cra = 'no clue what this is'

// Avoid Abbreviations
let cat = 'cat or category??'

// Avoid Meaningless Names
let foo = 'what is foo??'
```

## Community Feedback

**社区反馈**

- [@avi.codes](https://www.instagram.com/avi.codes/): Using camelCase while naming variables and UPPERCASE while naming constants.

在命名变量时使用小驼峰，在命名常量时使用大写。

- [@jonashavers](https://www.instagram.com/jonashavers/): That's always better than feeling the need to add a comment to describe what you mean and what the variable is used for. The goal is: "don't make me think"

这总比需要添加注释来描述变量的含义和用途要好。这么做的目标是：「不要让我思考」

## Navigate your Code Base with Better Variable Names

**好的变量名能更好导航代码库**

Having a good descriptive name also make searching and finding a lot easier in a large code base 💯

具有良好的描述性名称，使得在大型代码库中进行搜索和查找变得容易得多。

- [@agm1984](https://twitter.com/agm1984/status/1048670897895141376): I subscribe to the idea that, just because you can make your names short doesn't mean you should. You might open a 200 line file to line 175, see `this.toggle()` and be like, toggle what? Nothing beats a good keyword search that finds only 2-5 matches in 2 files instead of like 29 matches in 18 files.

我同意这样的观点，你可以把名字变短，但并不意味着你应该这么做。可以打开一个 200 行的文件至第 175 行，查看 `this.toggle()` 这行代码，这是要切换什么？能够在更少的源码文件中定位关键字终究是好的。

```js
// Bad
const toggle = () => {}

// Good
const toggleCountrySelector = () => {}
```

Thanks: [@agm1984](https://twitter.com/agm1984/status/1048670897895141376)

## Resources

- [Be Expressive: How to Give Your Variables Better Names](https://spin.atomicobject.com/2017/11/01/good-variable-names/)
- [The art of naming variables](https://hackernoon.com/the-art-of-naming-variables-52f44de00aad)
- [The Importance Of Naming In Programming](https://carlalexander.ca/importance-naming-programming/)
- [More on JavaScript Variable Naming Conventions](https://www.htmlgoodies.com/html5/javascript/back-by-popular-demand-more-on-javascript-variable-naming-conventions.html)
- [JavaScript naming convention](http://trungk18.github.io/experience/javascript-naming-convention/)

