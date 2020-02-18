# useReducer vs useState in React（useReducer 与 useState 在 React 中的对比）

> 转译自：https://www.robinwieruch.de/react-usereducer-vs-usestate#when-to-use-usestate-or-usereducer

Since [React Hooks](https://www.robinwieruch.de/react-hooks/) have been released, [function components](https://www.robinwieruch.de/react-function-component/) in React can use state and side-effects. There are two main hooks that are used for [modern state management in React](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/): useState and useReducer. This tutorial doesn't explain both React hooks in detail, but explains their different use case scenarios. There are many people who ask me whether to use useState or useReducer; that's why I thought getting together all my thoughts in one article is the best thing to deal with it.

自 [React Hooks](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-React-Hooks.md) 发布，React 中的 [函数组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Function-Components.md) 就可以使用状态和副作用。有两个用于 [React 现代状态管理](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-State-Hooks-useReducer-useState-useContext.md) 的重要钩子：useState 和 useReducer。本教程没有详细解释这两个 React 钩子，但是解释了它们不同的应用场景。很多人问我是使用 useState 还是 useReducer；这就是为什么会把我所有的想法集中在一篇文章里，我认为这是解决这个问题的最好办法。

## Table of Contents（目录列表）

- [When to use useState or useReducer?（何时使用 useState 或 useReducer）](#when-to-use-usestate-or-usereducer%E4%BD%95%E6%97%B6%E4%BD%BF%E7%94%A8-usestate-%E6%88%96-usereducer)
- [Simple vs. Complex State with Hooks（钩子的简单与复杂状态）](#simple-vs-complex-state-with-hooks%E9%92%A9%E5%AD%90%E7%9A%84%E7%AE%80%E5%8D%95%E4%B8%8E%E5%A4%8D%E6%9D%82%E7%8A%B6%E6%80%81)
- [Simple vs. Complex State Transitions with Hooks（使用钩子实现简单状态转换到复杂状态）](#simple-vs-complex-state-transitions-with-hooks%E4%BD%BF%E7%94%A8%E9%92%A9%E5%AD%90%E5%AE%9E%E7%8E%B0%E7%AE%80%E5%8D%95%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2%E5%88%B0%E5%A4%8D%E6%9D%82%E7%8A%B6%E6%80%81)
- [Multiple State Transitions operate on one State Object（在一个状态对象上进行多个状态转换操作）](#multiple-state-transitions-operate-on-one-state-object%E5%9C%A8%E4%B8%80%E4%B8%AA%E7%8A%B6%E6%80%81%E5%AF%B9%E8%B1%A1%E4%B8%8A%E8%BF%9B%E8%A1%8C%E5%A4%9A%E4%B8%AA%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2%E6%93%8D%E4%BD%9C)
- [Logic for State Changes（状态更改的逻辑）](#logic-for-state-changes%E7%8A%B6%E6%80%81%E6%9B%B4%E6%94%B9%E7%9A%84%E9%80%BB%E8%BE%91)
- [Trigger of the State Change（触发状态改变）](#trigger-of-the-state-change%E8%A7%A6%E5%8F%91%E7%8A%B6%E6%80%81%E6%94%B9%E5%8F%98)

## When to use useState or useReducer?（何时使用 useState 或 useReducer）

Everyone starting out with React Hooks gets to know pretty soon the useState hook. It's there to update state in React function components by offering to set the initial state and returning the actual state and an updater function:

每个人从 React Hooks 开始，很快就会了解 useState 钩子。它可以通过设置初始状态并返回实际状态和更新函数来修改 React 函数组件中的状态：

```js
import React, { useState } from "react";

const Counter = () => {
  const [count, setCount] = useState(0);

  const handleIncrease = () => {
    setCount(count => count + 1);
  };

  const handleDecrease = () => {
    setCount(count => count - 1);
  };

  return (
    <div>
      <h1>Counter with useState</h1>
      <p>Count: {count}</p>

      <div>
        <button type="button" onClick={handleIncrease}>
          +
        </button>
        <button type="button" onClick={handleDecrease}>
          -
        </button>
      </div>
    </div>
  );
};

export default Counter;
```

In contrast, the useReducer hook can be used to update state as well, but it happens in a **more sophisticated** way with a given reducer function and an initial state which returns the actual state and a dispatch function to alter the state in an implicit way by **mapping actions to state transitions**:

相比之下，useReducer 钩子也可以用来更新状态，但它 **更复杂**，通过给定 reducer 函数和一个初始状态，返回的实际状态和 dispatch 函数来改变状态的隐式方法，完成从 action 映射到状态的转换：

```js
import React, { useReducer } from "react";

const counterReducer = (state, action) => {
  switch (action.type) {
    case "INCREASE":
      return { ...state, count: state.count + 1 };
    case "DECREASE":
      return { ...state, count: state.count - 1 };
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  const handleIncrease = () => {
    dispatch({ type: "INCREASE" });
  };

  const handleDecrease = () => {
    dispatch({ type: "DECREASE" });
  };

  return (
    <div>
      <h1>Counter with useReducer</h1>
      <p>Count: {state.count}</p>

      <div>
        <button type="button" onClick={handleIncrease}>
          +
        </button>
        <button type="button" onClick={handleDecrease}>
          -
        </button>
      </div>
    </div>
  );
};

export default Counter;
```

Even though both components use different React Hooks for the state management, they solve the same business case. So when would you use which state management solution? Let's dive into it ...

尽管这两个组件使用不同的 React 钩子进行状态管理，但它们解决的是相同的业务案例。那么何时使用这些状态管理解决方案呢？让我们开始吧。

## Simple vs. Complex State with Hooks（钩子的简单与复杂状态）

The previous reducer example already encapsulated the `count` property into a state object. We could have done it simpler by using the count as the actual state. Refactoring the code to not having a state object, but only the count integer as JavaScript primitive, we can already see that the use case doesn't have a complex state to manage:

前面的 reduce 示例已经将 `count` 属性封装到一个状态对象中。我们可以用 count 作为实际状态来实现得更简单。将代码重构，不使用状态对象，但只有整型变量 count 作为 JavaScript 的基本数据类型，我们已经可以看到，案例中不需要管理复杂的状态：

```js
import React, { useReducer } from "react";

const counterReducer = (state, action) => {
  switch (action.type) {
    case "INCREASE":
      return state + 1;
    case "DECREASE":
      return state - 1;
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [count, dispatch] = useReducer(counterReducer, 0);

  const handleIncrease = () => {
    dispatch({ type: "INCREASE" });
  };

  const handleDecrease = () => {
    dispatch({ type: "DECREASE" });
  };

  return (
    <div>
      <h1>Counter with useReducer</h1>
      <p>Count: {count}</p>

      <div>
        <button type="button" onClick={handleIncrease}>
          +
        </button>
        <button type="button" onClick={handleDecrease}>
          -
        </button>
      </div>
    </div>
  );
};

export default Counter;
```

The example shows that we may be better off with a simpler useState hook here, because there is no complex state object involved. We could refactor our state object to a primitive.

这个案例表明，我们最好使用更简单的 useState 钩子，因为它不涉及复杂的状态对象。我们可以将状态对象重构为基本数据类型。

Anyway, I would argue **once you move past managing a primitive (e.g. string, integer, boolean) but rather a complex object (e.g. with arrays and additional primitives), you may be better of using useReducer** to manage this object. Perhaps a good rule of thumb:

无论如何，我认为 **一旦不再管理一个基本数据类型（如字符串、整数、布尔值），而是管理一个复杂的对象（如数组和基本数据类型之外的类型），使用 useReducer 可能会更好** 。也许这是一个很好的经验法则：

- Use useState whenever you manage a JS primitive (e.g. string, boolean, integer).

当管理 JS 的基本数据类型（如 string、boolean、integer）时，使用 useState。

- Use useReducer whenever you manage an object or array.

当管理对象或数组时，使用 useReducer。

The rule of thumb suggests, for instance, once you spot `const [state, setState] = useState({ firstname: 'Robin', lastname: 'Wieruch' })` in your code, you may be better off with useReducer instead of useState.

经验法则表明，假设在你的代码中发现了 `const [state, setState] = useState({ firstname: 'Robin', lastname: 'Wieruch' })`，最好使用 useReducer 而不是 useState。

## Simple vs. Complex State Transitions with Hooks（使用钩子实现简单状态转换到复杂状态）

We didn't use by chance two different action types (`INCREASE` and `DECREASE`) for our previous state transitions. What could we have done differently? By using the optional payload that can be used within every dispatched action object, we could say from the outside by how much we want to increase or decrease the count; moving the state transition towards being more implicit:

在前面的状态转换中，如果我们没有使用两种不同的 action 类型（`INCREASE` 和 `DECREASE`）。我们可以做些什么呢？通过每个分派 action 对象中公用的可选关键数据，我们可以从外部决定我们想要增加或减少的数量；使状态过渡更加含蓄：

```js
import React, { useReducer } from "react";

const counterReducer = (state, action) => {
  switch (action.type) {
    case "INCREASE_OR_DECREASE_BY":
      return state + action.by;
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [count, dispatch] = useReducer(counterReducer, 0);

  const handleIncrease = () => {
    dispatch({ type: "INCREASE_OR_DECREASE_BY", by: 1 });
  };

  const handleDecrease = () => {
    dispatch({ type: "INCREASE_OR_DECREASE_BY", by: -1 });
  };

  return (
    <div>
      <h1>Counter with useReducer</h1>
      <p>Count: {count}</p>

      <div>
        <button type="button" onClick={handleIncrease}>
          +
        </button>
        <button type="button" onClick={handleDecrease}>
          -
        </button>
      </div>
    </div>
  );
};

export default Counter;
```

But we didn't. That's one important lesson when using reducers: Always try to be explicit with your state transitions. The latter example with only one state transitions tries to put every logic into one block, but that's not very much desired when using a reducer function. Rather we want to be able to reason effortless about our state transitions. By having two state transitions instead, as before in our code, we can always reason about it by just reading the action type's name.

但是我们没有这么做。这是使用 reducer 时的一个重要教训：始终显式地处理状态转换。后一个案例只有一个状态转换，试图将每个逻辑放入一个块中，但在使用 reducer 函数时，这并不是很理想。相反，我们希望能够毫不费力地推断我们的状态转换。与前面的代码一样，通过使用两个状态转换，我们总是可以通过读取 action 类型的名称来推断它。

**Using useReducer over useState gives us predictable state transitions.** It comes in very powerful when your state changes become more complex and you want to have one place -- the reducer function -- to reason about them. The reducer functions encapsulates this logic perfectly.

**在 useState 基础上使用 useReducer 可以提供可预测的状态转换。** 当状态变化变得更加复杂，并且希望有一个地方（即 reducer 函数）来对它们进行推断时，它就会非常强大。reducer 函数完美地封装了这个逻辑。

A rule of thumb may suggest: Once you spot multiple `setState()` calls in succession, try to encapsulate these things in one reducer function to dispatch only one action instead.

经验法则也许能说明这一点：一旦连续地发现多个 `setState()` 调用，尝试将这些东西封装到一个 reducer 函数中，从而只分派一个 action。

A great side-effect of having all state in one object is the possibility to use the [browser's local storage](https://www.robinwieruch.de/local-storage-react/) for it. That's how you could always cache a slice of your state with local storage and retrieve it as initial state for useReducer whenever you restart your application.

在一个对象中包含所有状态的一个很好的副作用是可以使用 [浏览器的本地存储（暂缺译文）]()。这就是为什么你总是可以使用本地存储缓存状态的一部分，并在重新启动应用程序时将其作为 useReducer 的初始状态进行检索。

## Multiple State Transitions operate on one State Object（在一个状态对象上进行多个状态转换操作）

Once your application grows in size, you will most likely deal with more complex state and state transitions. That's what we went through in the last two sections of this tutorial. However, one thing to notice is that the state object didn't just grew in complexity, but also in size of operations that are performed on this object.

一旦应用程序发展到一定规模，你将有可能处理更复杂的状态和状态转换。这就是我们在本教程的最后两部分中讨论的内容。然而，需要注意的一件事是，state 对象不仅增加了复杂性，而且在对该对象执行的操作的规模上也增加了。

Take for instance the following reducer that operates on one state object with multiple state transitions:

举个例子，下面的 reducer 操作一个状态对象与多个状态转换：

```js
const todoReducer = (state, action) => {
  switch (action.type) {
    case "DO_TODO":
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: true };
        } else {
          return todo;
        }
      });
    case "UNDO_TODO":
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: false };
        } else {
          return todo;
        }
      });
    case "ADD_TODO":
      return state.concat({
        task: action.task,
        id: action.id,
        complete: false
      });
    default:
      throw new Error();
  }
};
```

It only makes sense to keep everything in one state object (e.g. list of todo items) while operating with multiple state transitions on it. It would be less predictable and maintainable implementing the same business logic with useState instead.

在操作多个状态转换时，只需要将所有内容保存在一个状态对象中（例如待办事项列表）。如果用 useState 实现相同的业务逻辑，那么它的可预测性和可维护性就会降低。

Often you will start out with useState but refactor your state management to useReducer, because the state object becomes more complex or the number of state transitions add up over time. However, there are other cases as well where it makes sense to group different properties, that don't belong together on first glance, in one state object. For instance, this [tutorial that showcases how to fetch data with useEffect, useState, and useReducer](https://www.robinwieruch.de/react-hooks-fetch-data/) groups properties that are dependent on each other together in one state object:

通常，你从 useState 开始将状态管理重构为 useReducer 是因为状态对象变得更加复杂，或者状态转换的数量随时间增加。然而，在其他一些情况下，在一个状态对象中对不同的属性进行分组是有意义的，这些属性初看并不属于同一个状态对象。例如，本 [教程演示了如何使用 useEffect、useState 和 useReducer 获取数据（暂缺译文）]()，并将相互依赖的属性分组在一个状态对象中：

```js
const [state, dispatch] = useReducer(dataFetchReducer, {
  isLoading: false,
  isError: false,
  data: initialData
});
```

One could argue that the `isLoading` and `isError` properties could be managed separately in two useState hooks, but when looking at the reducer function, one can see that it's best to put them together in one state object because they conditionally dependent on each other:

有人可能会说，`isLoading` 和 `isError` 属性可以在两个 useState 钩子中单独管理，但当查看 reduce 函数时，可以看到，最好将它们放在一个状态对象中，因为它们有条件地相互依赖:

```js
const dataFetchReducer = (state, action) => {
  switch (action.type) {
    case "FETCH_INIT":
      return {
        ...state,
        isLoading: true,
        isError: false
      };
    case "FETCH_SUCCESS":
      return {
        ...state,
        isLoading: false,
        isError: false,
        data: action.payload
      };
    case "FETCH_FAILURE":
      return {
        ...state,
        isLoading: false,
        isError: true
      };
    default:
      throw new Error();
  }
};
```

After all, not only the state object complexity or the number of state transitions is important, but also **how properties fit together in context to be managed in one state object**. If everything is managed at different places with useState, it becomes harder to reason about the whole thing as one unit. Another important point is the improved developer experience: Since you have this one place with one state object and multiple transitions, it's far easier to debug your code if anything goes wrong.

不仅状态对象的复杂性或状态转换的数量很重要，而且属性 **如何在 context 中相互配合以便在一个状态对象中进行管理也很重要**。如果都分散在不同的地方用 useState 来管理，那么就很难将其作为一个单元来考虑。另一个重要的方面是能改善的开发人员体验：因为有集中管理状态对象和多个状态转换的地方，所以如果出了问题，调试代码要容易得多。

A great side-effect of having all state transitions neatly in one reducer function is the **ability to export the reducer for unit tests**. It's simpler to reason about a state object with multiple state transitions if you just need to test all state transitions by having only one function: `(state, action) => newState`. You can test all state transitions by providing all available action types and various matching payloads.

将所有状态转换整齐地放在一个 reducer 函数中有一个很大的副作用，那就是 **可以导出用于单元测试的 reducer** 。如果只需要使用一个函数 `(state, action) => newState` 来测试所有状态转换，那么推断一个具有多个状态转换的状态对象会更简单。你可以通过提供所有可用的 action 类型和各种匹配的关键数据来测试所有状态转换。

## Logic for State Changes（状态更改的逻辑）

There is a difference of _where the logic for state transitions is placed when using useState or useReducer_. As we have seen for the previous useReducer examples, the logic for the state transitions happens within the reducer function. The action only comes with the minimum information to perform the transition based on the current state: `(state, action) => newState`. This comes especially handy if you rely on the current state to update your state.

在使用 useState 或 useReducer 时，状态转换的逻辑放在哪里？正如我们在前面的 useReducer 案例中所看到的，状态转换的逻辑在 reducer 函数中。该 action 只提供了基于当前状态执行转换的最小信息：`(state, action) => newState`。如果依赖于当前状态来更新状态，这将特别方便。

```js
const todoReducer = (state, action) => {
  switch (action.type) {
    case "DO_TODO":
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: true };
        } else {
          return todo;
        }
      });
    case "UNDO_TODO":
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: false };
        } else {
          return todo;
        }
      });
    case "ADD_TODO":
      return state.concat({
        task: action.task,
        id: action.id,
        complete: false
      });
    default:
      throw new Error();
  }
};
```

Everything your React component cares about is dispatching the action:

你的 React 组件所关心的一切都是发送 action：

```js
import uuid from "uuid/v4";

// Somewhere in your React components ...

const handleSubmit = event => {
  dispatch({ type: "ADD_TODO", task, id: uuid() });
};

const handleChange = () => {
  dispatch({
    type: todo.complete ? "UNDO_TODO" : "DO_TODO",
    id: todo.id
  });
};
```

Now imagine you would perform the same state transitions but with useState instead. There is no pre-defined entity like the reducer where all business logic is situated. There is no clear separation -- as far as you don't extract the logic into separate functions -- and all your state relevant logic ends up in your handlers which call the state updater functions from useState eventually. Over time, it becomes harder to separate state logic from view logic and the components grow in complexity. Reducers instead offer the perfect place for logic that alters the state.

现在，假设你将执行相同的状态转换，但是使用 useState。在所有业务逻辑所在的位置上，不存在类似于 reducer 这样的预定义实体。没有明确的分离（只要你不将逻辑提取到单独的函数中）并且所有与状态相关的逻辑最终都将在处理程序中调用状态更新器函数。随着时间的推移，将状态逻辑从视图逻辑中分离出来变得越来越困难，组件也变得越来越复杂。reducer 为改变状态的逻辑提供了完美的场所。

## Trigger of the State Change（触发状态改变）

The vertical component tree in React becomes deeper once you grow your application. If the state is simple and belongs co-located (state + state trigger) to a component (e.g. [search input field which is made a controlled component](https://www.robinwieruch.de/react-controlled-components/)), using useState may be the perfect fit. The state is encapsulated within this one component:

React 中的垂直组件树在应用程序扩展后会变得更深。如果是简单的状态，并且属于一个组件（例如，[参考 input field，它是一个受控组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-Controlled-Components-in-React.md)），使用 useState 可能是最合适的。状态封装在这一个组件中：

```js
import React, { useState } from "react";

const App = () => {
  const [value, setValue] = useState("Hello React");

  const handleChange = event => setValue(event.target.value);

  return (
    <div>
      <label>
        My Input:
        <input type="text" value={value} onChange={handleChange} />
      </label>

      <p>
        <strong>Output:</strong> {value}
      </p>
    </div>
  );
};

export default App;
```

However, sometimes you want to [manage state at a top-level but trigger the state changes](https://www.robinwieruch.de/react-global-state-without-redux/) somewhere deep down in your component tree. It's possible to pass both the updater function from useState or the dispatch function from useReducer [via props](https://www.robinwieruch.de/react-pass-props-to-component/) down the component tree, but using [React's context API](https://www.robinwieruch.de/react-context/) may be a valid alternative to avoid the prop drilling (passing props trough each component level). Then having _one_ dispatch function that is used with different action types and payloads may be the better option than using _multiple_ updater functions from useState that need to be passed down individually. The dispatch function can be passed down _once_ with React's useContext hook. A good example how this works can be seen in this [state management tutorial for React using useContext](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/).

然而，有时你希望在顶层管理状态，在组件树深处触发状态更改。虽然可以 [通过 props（暂缺译文）]() 从组件树向下传递 useState 的 updater 函数或 useReducer 的 dispatch 函数，但是使用 [React 的 context API（暂缺译文）]() 是避免 props 传递的有效方法（通过每个组件层传递 props）。那么，与使用 useState 需要单独传递的 _多个_ updater 函数相比，使用 _一个_ dispatch 函数与不同的 action 类型和关键数据一起使用可能是更好的选择。dispatch 函数可以通过 React 的 useContext 钩子向下传递 _一次_。在 [使用 useContext 进行 React 的状态管理教程](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-State-Hooks-useReducer-useState-useContext.md) 中可以看到一个很好的案例。

The decision whether to use useState or useReducer isn't always black and white. There are many shades of grey in between. However, I hope the article gave you a few key understandings on when to use useState or useReducer. Here you can find a [GitHub repository](https://github.com/the-road-to-learn-react/react-hooks-usestate-vs-usereducer) with a few examples. The following facts give you a summarized overview, however they only reflect my opinion on this topic:

决定是使用 useState 还是 useReducer 并不总是泾渭分明的。其间有许多灰色地带。然而，我希望这篇文章能给你一些关于何时使用 useState 或 useReducer 的关键理解。你可以在这个 [GitHub 存储库](https://github.com/the-road-to-learn-react/react-hooks-usestate-vs-usereducer) 找到一些示例。下面的事实可以给你一个概括，但它们只是反映了我对这个话题的看法：

**Use useState if:**

- A) if you manage JavaScript primitives as state

如果你管理的状态是 JavaScript 基本数据类型。

- B) if you have simple state transitions

如果仅涉及简单的状态转换。

- C) if you want to have business logic within your component

如果希望在组件中包含业务逻辑。

- D) if you have different properties that don't change in any correlated manner and can be managed by multiple useState hooks

如果有不同的属性，这些属性不会以任何相关的方式改变，并且可以由多个 useState 钩子管理。

- E) if your state is co-located to your component

如果你的状态位于组件中。

- F) if you've got a small application (but the lines are blurry here)

如果应用程序规模很小（但是这里的界限很模糊）

**Use useReducer if:**

- A) if you manage JavaScript objects or arrays as state

如果你管理的状态是 JavaScript 对象或数组。

- B) if you have complex state transitions

如果涉及复杂的状态转换。

- C) if you want to move business logic into reducers

如果希望把业务逻辑迁移到 reducer。

- D) if you have different properties that are tied together and should be managed in one state object

如果有不同的属性绑定在一起，并且应该在一个状态对象中进行管理。

- E) if you want to update state deep down in your component tree

如果你想更新组件树深处的状态。

- F) if you've got a medium size application (but the lines are blurry here)

如果应用程序规模中等（但是这里的界限很模糊）

- G) if you want have an easier time testing it

如果你希望测试起来更容易。

- H) if you want a more predictable and maintainable state architecture

如果你想要一个更加可预测和可维护的状态体系结构。

_Note: Check out when to use [useReducer or Redux](https://www.robinwieruch.de/redux-vs-usereducer) if you are interested in a comparison._

_注意：如果对类似的比较感兴趣，可查看何时使用 [useReducer 或 Redux（暂缺译文）]()。_

If you want to go through a more comprehensive example where useState and useReducer are used together, check out this extensive walkthrough for [modern state management in React](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/). It almost mimics Redux by using [React's useContext Hook](https://www.robinwieruch.de/react-usecontext-hook) for "global" state management where it's possible to pass down the dispatch function once.

如果你想通过一个更全面的例子来了解 useState 和 useReducer 一起使用的情况，请查看 [React 中关于现代状态管理](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-State-Hooks-useReducer-useState-useContext.md) 的详细介绍。它很大程度上模仿了 Redux，使用 [React 的 useContext 钩子（暂缺译文）]() 来进行「全局」状态管理，这样就可以只传递 dispatch 函数一次即可。
