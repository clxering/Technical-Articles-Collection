# Bad Variable Names to Avoid 🙅‍♀️

> 转译自：https://www.samanthaming.com/tidbits/36-bad-variable-names-to-avoid

In an earlier post, I mentioned [how you can give your boolean variables a better name](https://www.samanthaming.com/tidbits/34-better-boolean-variable-names) by prefixing it with is/has/can. Now I want to give advice on bad variable names you should avoid 💥

Reading code is tough already, so don’t make it even more complicated by using names that they have to guess. Your code should convey understanding. So it’s better to use words that everyone can understand. This will make it a lot easier for others and even yourself to work with your code. Yay! Win Win 😊

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

- @avi.codes: Using camelCase while naming variables and UPPERCASE while naming constants.

- @jonashavers: That's always better than feeling the need to add a comment to describe what you mean and what the variable is used for. The goal is: "don't make me think"

### Navigate your Code Base with Better Variable Names

Having a good descriptive name also make searching and finding a lot easier in a large code base 💯

- @agm1984: I subscribe to the idea that, just because you can make your names short doesn't mean you should. You might open a 200 line file to line 175, see `this.toggle()` and be like, toggle what? Nothing beats a good keyword search that finds only 2-5 matches in 2 files instead of like 29 matches in 18 files.

```js
// Bad
const toggle = () => {}

// Good
const toggleCountrySelector = () => {}
```

Thanks: @agm1984

## Resources

[Be Expressive: How to Give Your Variables Better Names](https://spin.atomicobject.com/2017/11/01/good-variable-names/)
[The art of naming variables](https://hackernoon.com/the-art-of-naming-variables-52f44de00aad)
[The Importance Of Naming In Programming](https://carlalexander.ca/importance-naming-programming/)
[More on JavaScript Variable Naming Conventions](https://www.htmlgoodies.com/html5/javascript/back-by-popular-demand-more-on-javascript-variable-naming-conventions.html)
[JavaScript naming convention](http://trungk18.github.io/experience/javascript-naming-convention/)

