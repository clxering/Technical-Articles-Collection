# Refactoring the `if` Condition

> 转译自：https://www.samanthaming.com/tidbits/26-refactoring-if-condition

Refactoring the `if` condition using our falsy value concept. Remember last week I talked about ["Falsy Values"](http://www.samanthaming.com/tidbits/25-js-essentials-falsy-values). Here’s where it becomes handy. Since values in JS will evaluate to either true or false. You can leverage that concept to shorten your condition in the `if` statement. Whoa! isn’t your code so much cleaner! That’s why knowing your JS essentials are so important 😉 👍

```js
// Refactoring the `if` condition

let isPerson = false;

// ❌
if (isPerson === false)

// ✅ Much better
if (!isPerson)
```

## More Examples

If you're just checking if a value is truthy or falsy, you can skip the comparison and just pass in the value. Since the value will have a boolean equivalent - it will evaluate to either true or false.

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

```js
// ❌
isPerson === false ? "👻" : "🙂";

// ✅ Much better
isPerson ? "👻" : "🙂";
```

## Community Examples

## false vs falsy value

Note the 2 statements are slightly different:

```js
// This is checking if the value is a "false" value
if (isPerson === false)

// This is checking if value is "falsy"
if(!isPerson)
```

Thanks: @RanqueBenoit

## Good Variable Names

@jsawbrey: I find the "is" naming convention also very helpful. I make them all the time. "isThisThing". In TypeScript, that naming convention helps a lot when making type guards.

@jsawbrey: if a function sets a variable, "setThing" is good. If a function toggles a variable (on/off, true/false), "toggleThing" is good.

Thanks: @jsawbrey

## Good Variable Name: On vs Handle

On the note of good variable names. Here's another cool example. This is an excerpt from Adam's medium article, [Use React to make a photo follow the mouse](https://medium.com/@agm1984/use-react-to-make-a-photo-follow-the-mouse-aka-transform-perspective-or-tilt-7c38f1b3a623).

@agm1984: Notice how we called the Class Methods handle rather than on. I like to remind people about the distinction between the two. on refers to the event on which we are doing something. handle refers to the action we are taking or the result of the event.

@agm1984: If we were delegating the handling up to a parent or calling back to some other location, we should use on. This is why you see callbacks that look like this:

```js
<button onClick={onClick}>Submit</button>
```

Thanks: @agm1984

## Resources

- [12 Good JavaScript Shorthand Techniques](https://hackernoon.com/12-amazing-javascript-shorthand-techniques-fef16cdbc7fe)
- [Code Tidbit: JS Essentials - Falsy Values](http://www.samanthaming.com/tidbits/25-js-essentials-falsy-values)
