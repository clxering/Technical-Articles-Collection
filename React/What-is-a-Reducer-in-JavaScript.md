# What is a Reducer in JavaScript?（JavaScript 中的 Reducer 是什么？）

> 转译自：https://www.robinwieruch.de/javascript-reducer

The concept of a Reducer became popular in JavaScript with the rise of [Redux as state management solution for React](). But no worries, you don't need to learn Redux to understand Reducers. Basically reducers are there to manage state in an application. For instance, if a user writes something in an HTML input field, the application has to manage this UI state (e.g. [controlled components]()).

随着以 [Redux 作为 React 的状态管理解决方案]() 的兴起，Reducer 的概念在 JavaScript 中变得流行起来。但是不用担心，你不需要学习 Redux 来理解 Reducer。基本上，reducer 用于管理应用程序状态。例如，如果用户在 HTML 的 input 中写入内容，应用程序必须管理这个 UI 状态（例如 [受控组件]()）。

Let's dive into the implementation details: In essence, a reducer is a function which takes two arguments -- the current state and an action -- and returns based on both arguments a new state. In a pseudo function it could be expressed as:

让我们深入了解实现细节：本质上，reducer 是一个函数，它接受两个参数，当前状态和一个 action，并基于这两个参数返回一个新的状态。在伪函数中可以表示为：

```js
(state, action) => newState;
```

As example, it would look like the following in JavaScript for the scenario of increasing a number by one:

例如，在 JavaScript 中为一个数字加 1 的场景如下：

```js
function counterReducer(state, action) {
  return state + 1;
}
```

Or defined as JavaScript arrow function, it would look the following way for the same logic:

或者定义为 JavaScript 的箭头函数，它会以如下方式显示相同的逻辑：

```js
const counterReducer = (state, action) => {
  return state + 1;
};
```

In this case, the current state is an integer (e.g. count) and the reducer function increases the count by one. If we would rename the argument `state` to `count`, it may be more readable and approachable by newcomers to this concept. However, keep in mind that the `count` is still the state:

在本例中，当前状态是整数（例如 count），而 reducer 数将该计数增加 1。如果我们将参数 `state` 重命名为 `count`，那么这个概念对于新手来说，它可能更易于阅读和理解。但是，请记住 `count` 仍然是状态：

```js
const counterReducer = (count, action) => {
  return count + 1;
};
```

The reducer function is a pure function without any side-effects, which means that given the same input (e.g. `state` and `action`), the expected output (e.g. `newState`) will always be the same. This makes reducer functions the perfect fit for reasoning about state changes and testing them in isolation. You can repeat the same test with the same input as arguments and always expect the same output:

reducer 函数是一个没有任何副作用的纯函数，这意味着给定相同的输入（例如 `state` 和 `action`），期望的输出（例如 `newState`）总是一样的。这使得 reducer 函数非常适合于对状态变化进行推理并单独测试它们。你可以用相同的输入作为参数重复相同的测试，并且 expect 始终会得到相同的输出：

```js
expect(counterReducer(0)).to.equal(1); // successful test
expect(counterReducer(0)).to.equal(1); // successful test
```

That's the essence of a reducer function. However, we didn't touch the second argument of a reducer yet: the action. The `action` is normally defined as an object with a `type` property. Based on the type of the action, the reducer can perform conditional state transitions:

这就是 reducer 的本质。然而，我们还没有触及第二个参数：action。`action` 通常定义为带有 `type` 属性的对象。根据 action 类型，reducer 可以有条件的进行状态转换：

```js
const counterReducer = (count, action) => {
  if (action.type === "INCREASE") {
    return count + 1;
  }

  if (action.type === "DECREASE") {
    return count - 1;
  }

  return count;
};
```

If the action `type` doesn't match any condition, we return the unchanged state. Testing a reducer function with multiple state transitions -- given the same input, it will always return the same expected output -- still holds true as mentioned before which is demonstrated in the following test cases:

如果 action 的 `type` 不匹配任何条件，将返回未更改的状态。测试具有多个状态转换的 reducer 函数，给定相同的输入，它将始终返回相同的 expect 输出，这仍然适用于前面提到的情况，这在以下测试用例中得到了证明：

```js
// successful tests
// because given the same input we can always expect the same output
expect(counterReducer(0, { type: "INCREASE" })).to.equal(1);
expect(counterReducer(0, { type: "INCREASE" })).to.equal(1);

// other state transition
expect(counterReducer(0, { type: "DECREASE" })).to.equal(-1);

// if an unmatching action type is defined the current state is returned
expect(counterReducer(0, { type: "UNMATCHING_ACTION" })).to.equal(0);
```

However, more likely you will see a switch case statement in favor of if else statements in order to map multiple state transitions for a reducer function. The following reducer performs the same logic as before but expressed with a switch case statement:

但是，更有可能看到的用法是 switch case 语句，它比 if else 语句更有利，以便映射一个 reducer 函数的多个状态转换。下面的 reducer 执行与前面相同的逻辑，但用 switch case 语句表示：

```js
const counterReducer = (count, action) => {
  switch (action.type) {
    case "INCREASE":
      return count + 1;
    case "DECREASE":
      return count - 1;
    default:
      return count;
  }
};
```

In this scenario, the `count` itself is the state on which we are applying our state changes upon by increasing or decreasing the count. However, often you will not have a JavaScript primitive (e.g. integer for count) as state, but a complex JavaScript object. For instance, the count could be one property of our `state` object:

在这个场景中，`count` 本身就是我们通过增加或减少 count 来更改的状态。但是，通常情况下，不会将 JavaScript 基本数据类型（例如，integer 类型的 count）作为状态，而是一个复杂的 JavaScript 对象。例如，count 可以是我们的 `state` 对象的一个属性：

```javascript
const counterReducer = (state, action) => {
  switch (action.type) {
    case "INCREASE":
      return { ...state, count: state.count + 1 };
    case "DECREASE":
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
};
```

Don't worry if you don't understand immediately what's happening in the code here. Foremost, there are two important things to understand in general:

如果不能立即理解代码中发生的事情，请不要担心。首先，有两件重要的事情需要理解：

- **The state processed by a reducer function is immutable.** That means the incoming state -- coming in as argument -- is never directly changed. Therefore the reducer function always has to return a new state object. If you haven't heard about immutability, you may want to check out the topic immutable data structures.

**由 reducer 函数处理的状态是不可变的。** 这意味着传入的状态（作为参数传入）永远不会被直接更改。因此，reducer 函数总是必须返回一个新的状态对象。如果还没有听说过不变性，那么你可能想了解一下不可变数据结构。

- Since we know about the state being a immutable data structure, we can use the [JavaScript spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) to **create a new state object from the incoming state and the part we want to change** (e.g. `count` property). This way we ensure that the other properties that aren't touch from the incoming state object are still kept intact for the new state object.

因为我们知道状态是一个不可变的数据结构，所以我们可以使用 [扩展运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) **将传入的状态和我们想要更改的部分整合后创建一个新的状态对象** （例如 `count` 属性）。通过这种方式，我们可以确保传入状态对象没有涉及的其他属性仍然完整地保留在新状态对象中。

Let's see these two important points in code with another example where we want to change the last name of a person object with the following reducer function:

让我们通过另一个例子来看看代码中这两个重要的点，在这个例子中，我们希望使用以下的 reducer 函数来更改 person 对象的姓：

```js
const personReducer = (person, action) => {
  switch (action.type) {
    case "INCREASE_AGE":
      return { ...person, age: person.age + 1 };
    case "CHANGE_LASTNAME":
      return { ...person, lastname: action.lastname };
    default:
      return person;
  }
};
```

We could change the last name of a user the following way in a test environment:

在测试环境中，我们可以通过以下方式更改用户的姓氏：

```js
const initialState = {
  firstname: "Liesa",
  lastname: "Huppertz",
  age: 30
};

const action = {
  type: "CHANGE_LASTNAME",
  lastname: "Wieruch"
};

const result = personReducer(initialState, action);

expect(result).to.equal({
  firstname: "Liesa",
  lastname: "Wieruch",
  age: 30
});
```

You have seen that by using the JavaScript spread operator in our reducer function, we use all the properties from the current state object for the new state object but override specific properties (e.g. `lastname`) for this new object. That's why you will often see the spread operator for keeping state operation immutable (= state is not changed directly).

你已经看到，通过在我们的 reducer 函数中使用扩展运算符，我们将当前状态对象的所有属性都用于新的状态对象，但会覆盖特定的属性（例如，对于这个新对象。这就是为什么你经常看到扩展运算符保持状态操作不可变（= 状态没有直接改变）。

Also you have seen another aspect of a reducer function: **An action provided for a reducer function can have an optional payload** (e.g. `lastname`) next to the mandatory action type property. The payload is additional information to perform the state transition. For instance, in our example the reducer wouldn't know the new last name of our person without the extra information.

你还看到了 reducer 函数的另一个方面：**为 reducer 函数提供的 action 可以有一个可选的关键信息（payload）**（例如，`lastname`）在强制操作类型属性的旁边。关键信息是执行状态转换的附加信息。例如，在我们的示例中，如果没有额外的信息，reducer 将不知道我们的人的新姓。

Often the optional payload of an action is put into another generic `payload` property to keep the top-level of properties of an action object more general (.e.g `{ type, payload }`). That's useful for having type and payload always separated side by side. For our previous code example, it would change the action into the following:

通常，action 的可选关键信息被放入另一个通用的 `payload` 属性中，以使 action 对象的顶级属性更通用。保持将类型和关键信息分开很有用。对于我们之前的代码示例，action 更改为以下内容：

```js
const action = {
  type: "CHANGE_LASTNAME",
  payload: {
    lastname: "Wieruch"
  }
};
```

The reducer function would have to change too, because it has to dive one level deeper into the action:

reducer 函数也必须改变，因为它必须进入 action 的更深一层：

```js
const personReducer = (person, action) => {
  switch (action.type) {
    case "INCREASE_AGE":
      return { ...person, age: person.age + 1 };
    case "CHANGE_LASTNAME":
      return { ...person, lastname: action.payload.lastname };
    default:
      return person;
  }
};
```

Basically you have learned everything you need to know for reducers. They are used to perform state transitions from A to B with the help of actions that provide additional information. You can find reducer examples from this tutorial in this [GitHub repository]() including tests. Here again everything in a nutshell:

基本上你已经学了所有你需要知道的关于 reducer 的知识。它们用于在提供附加信息的 action 帮助下执行从 A 到 B 的状态转换。你可以在这个 [GitHub 库]() 中找到本教程中的 reducer 示例，包括测试。简而言之，一切都很简单：

- **Syntax:** In essence a reducer function is expressed as `(state, action) => newState`.
- **Immutability:** State is never changed directly. Instead the reducer always creates a new state.
- **State Transitions:** A reducer can have conditional state transitions.
- **Action:** A common action object comes with a mandatory type property and an optional payload:

  - The type property chooses the conditional state transition.
  - The action payload provides information for the state transition.

- **语法：** 从本质上讲，reducer 函数是用 `(state, action) => newState` 来表示的。
- **不变性：** 状态不会直接改变。而总是用 reducer 创建一个新的状态。
- **状态转换：** 一个 reducer 可以有条件的进行状态转换。
- **Action：** 一个通用的 action 对象带有一个强制性的类型属性和一个可选的关键信息：
  - type 属性是状态转换的条件
  - action payload 提供了状态转换的关键信息

Also check out this [tutorial if you want to know how to use reducers in React with the useReducer hook]().

如果你想知道如何在 useReducer 钩子中使用 reduce，也可以看看这篇 [教程]()。
