# How to give your boolean variables a better name 👏

> 转译自：https://www.samanthaming.com/tidbits/34-better-boolean-variable-names

Coming up with good variable names can always be a challenge. So there is some convention you can follow that makes the process easier. For boolean values, you can simply prefix it with `is`, `has`, or `can`. Just by reading the name, you can easily infer that this variable will give you boolean value. Awesome! 👍

```js
// Better Boolean Variable Names

// ❌ bad
let person = true
let age = true
let dance = true

// ✅ Prefix with: is, has, can
let isPerson = true
let hasAge = true
let canDance = true
```

## Why Variable Names Matter?

Having a well named variable is definitely one of the most important thing for code readability. A good variable name should also provide meaning. That way it’s easy for others to read your code or even yourself in the future. I’m sure we all had the frustration of going back to our code and wonder what the heck is variable “adt”. Don’t ask me, I don’t even know. Acronym is probably the worst. Unless, it’s something super common like DVD.

## Bad Variable Names to Avoid

You should be descriptive with your naming. So make sure you avoid these bad variable names.

Avoid these bad variable names:

- ❌ single letter names
- ❌ acronyms
- ❌ abbreviations
- ❌ meaningless names

```js
// Avoid Single Letter Names

let h; // 😱  huh, what's h??

// Avoid Acronyms

let cra; // I bet you have no idea what this is unless you're from Canada 🇨🇦

// Avoid Abbreviations

let categ; // Sure we can deduce you're saying category here, but let's just used the full name, so it's not a guessing game 😜


// Avoid Meaningless Names

let foo; // what is foo? 🧐
```

## Community Feedback

@thecodercoder: Good advice! Descriptive names are way better. I used to try to keep names as short as possible, but realized that they really need to explain what they are!

@tirpus_hahs: It is very important to name variables what it describes so we don't have to comment out code. This allows us to read code like a story telling.

@__offblackYeah: my booleans go to the level of "isAnimatedWhenNotInViewport", "isScrollPositionGreaterThanTolerance" lol

@masonhale: Good suggestion. I also use ‘do’ as a prefix for Boolean settings/preferences as in `doSendReminder` or `doShowDetails`

@ben336: This is good advice. Also avoid negative variable names like isNotLoaded. The positive forms are almost always clearer

@sunnysinghio: What do you think about `handleValidateForm` considering it's an event handler? It's a more common practice that I've seen, albeit longer than `did`.

@styfle: I use handleClick for the function that will handle it and onClick for the property name which typically just passes it through

```html
<Form
  url="example.example/post"
  onClick={handleClick}
/>
```

@maxstalker: What are your thoughts on adjectives

```js
const sortable = true // instead of canBeStorted
const hidden = true // instead os ifShown
```

youthoverturn:

```
is/has + noun
enable/disable + verb
```

## Resources

- [Be Expressive: How to Give Your Variables Better Names](https://spin.atomicobject.com/2017/11/01/good-variable-names/)
- [The art of naming variables](https://hackernoon.com/the-art-of-naming-variables-52f44de00aad)
- [The Importance Of Naming In Programming](https://carlalexander.ca/importance-naming-programming/)
- [More on JavaScript Variable Naming Conventions](https://www.htmlgoodies.com/html5/javascript/back-by-popular-demand-more-on-javascript-variable-naming-conventions.html)
- [JavaScript naming convention](http://trungk18.github.io/experience/javascript-naming-convention/)
- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)
- [W3School: JavaScript Coding Conventions](https://www.w3schools.com/js/js_conventions.asp)

