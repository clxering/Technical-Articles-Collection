# React State Hooks: useReducer, useState, useContext

**React 状态钩子：useReducer、useState、useContext**

> 转译自：https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext

If you haven't used state management excessively in [React Function Components](https://www.robinwieruch.de/react-function-component/), this tutorial may help you to get a better understanding of how [React Hooks](https://www.robinwieruch.de/react-hooks/) -- such as useState, useReducer, and useContext -- can be used in combination for impressive state management in React applications. In this tutorial, we will almost reach the point where these hooks mimic sophisticated state management libraries like [Redux](https://www.robinwieruch.de/react-redux-tutorial/) for globally managed state. Let's dive into the application which we will implement together step by step.

如果你还没有在 [React 函数组件](/React/React-Function-Components.md) 中过多地使用状态管理，本教程可以帮助你更好地理解如何将 [React 钩子](/React/What-are-React-Hooks.md)（如 useState、useReducer 和 useContext）组合起来，在 React 应用程序中进行状态管理。在本教程中，我们要用钩子模拟复杂的状态管理库，比如全局管理状态的 [Redux（暂无译文）]()①。让我们深入研究一个逐步实现的应用程序。

**译注 ①：该篇文章年份稍早，性价比正在评估中**

## Table of Contents

**目录列表**

- [React useState: simple State（React useState：简单状态）](#react-usestate-simple-state)
- [React useReducer: complex State（React useReducer：复杂状态）](#react-usereducer-complex-state)
- [React useContext: global State（React useContext：全局状态）](#react-usecontext-global-state)

## React useState: simple State

**React useState：简单状态**

We start with a list of items -- in our scenario a list of todo items -- which are rendered in our function component with a [JavaScript Map Method for Arrays](https://www.robinwieruch.de/javascript-map-array/). Each todo item rendered as list item receives a [key attribute](https://www.robinwieruch.de/react-list-key) to notify React about its place in the [rendered list](https://www.robinwieruch.de/react-list-component):

我们从一个项目列表开始（在我们的场景中是一个待办事项列表），它在我们的函数组件中使用一个基于 [数组的 JavaScript map 方法](/JavaScript/Deep-Dive-into-JavaScript-Array-Map-Method.md) 渲染。作为列表项渲染的每个待办事项将接收一个 [key 属性（暂无译文）]() 来告知 React 其在 [渲染列表（暂无译文）]() 中的位置：

```js
import React from "react";

const initialTodos = [
  {
    id: "a",
    task: "Learn React",
    complete: true,
  },
  {
    id: "b",
    task: "Learn Firebase",
    complete: true,
  },
  {
    id: "c",
    task: "Learn GraphQL",
    complete: false,
  },
];

const App = () => (
  <div>
    <ul>
      {initialTodos.map((todo) => (
        <li key={todo.id}>
          <label>{todo.task}</label>
        </li>
      ))}
    </ul>
  </div>
);

export default App;
```

In order to add a new todo item to our list of todo items, we need an input field to give a new todo item a potential `task` property. The `id` and `complete` properties will be automatically added to the item. In React, we can use the State Hook called `useState` to manage something like the value of an input field as state within the component:

为了将一个新的待办事项添加到列表中，我们需要一个 input field 来给新的待办事项添加一个 `task` 属性。`id` 和 `complete` 属性将自动添加。在 React 中，我们可以使用状态钩子 `useState` 来管理组件中的状态，比如 input field 的值：

```js
import React, { useState } from 'react';

...

const App = () => {
  const [task, setTask] = useState('');

  const handleChangeInput = event => {

  };

  return (
    <div>
      <ul>
        {initialTodos.map(todo => (
          <li key={todo.id}>
            <label>{todo.task}</label>
          </li>
        ))}
      </ul>

      <input type="text" value={task} onChange={handleChangeInput} />
    </div>
  );
};
```

We also had to give our Function Arrow Component a body with an explicit return statement to get the `useState` hook in between. Now, we can change the `task` state with our handler function, because we have the input's value at our disposal in React's synthetic event:

我们还必须为箭头函数组件提供一个带有显式返回语句的主体，以便在两者之间获得 `useState` 钩子。现在，我们可以使用处理函数改变 `task` 状态，因为我们可以在 React 的合成事件中处理输入值：

```js
const App = () => {
  const [task, setTask] = useState("");

  const handleChangeInput = (event) => {
    setTask(event.target.value);
  };

  return (
    <div>
      <ul>
        {initialTodos.map((todo) => (
          <li key={todo.id}>
            <label>{todo.task}</label>
          </li>
        ))}
      </ul>

      <input type="text" value={task} onChange={handleChangeInput} />
    </div>
  );
};
```

Now the input field has become a [controlled input field](https://www.robinwieruch.de/react-controlled-components/), because the value comes directly from the React managed state and the handler changes the state. We implemented our first managed state with the State Hook in React. The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/3e6e5a27561bd0e0cc99e39efb853a187ac7339e/src/App.js).

现在 input field 已经变成了 [受控 input field](/React/What-are-Controlled-Components-in-React.md)，因为值直接来自 React 托管的状态，由处理程序更改状态。我们使用 React 中的状态钩子实现了第一个托管状态。完整的源代码可以在 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/3e6e5a27561bd0e0cc99e39efb853a187ac7339e/src/App.js) 看到。

To continue, let's implement a submit button to add the new todo item to the list of items eventually:

继续，让我们实现一个提交按钮，最终可将新的待办事项目添加到列表中：

```js
const App = () => {
  const [task, setTask] = useState("");

  const handleChangeInput = (event) => {
    setTask(event.target.value);
  };

  const handleSubmit = (event) => {
    if (task) {
      // add new todo item
    }

    setTask("");

    event.preventDefault();
  };

  return (
    <div>
      <ul>
        {initialTodos.map((todo) => (
          <li key={todo.id}>
            <label>{todo.task}</label>
          </li>
        ))}
      </ul>

      <form onSubmit={handleSubmit}>
        <input type="text" value={task} onChange={handleChangeInput} />
        <button type="submit">Add Todo</button>
      </form>
    </div>
  );
};
```

The submit handler doesn't add the new todo item yet, but it makes the input field's value empty again after submitting the new todo item. Also it prevents the [default behavior of the browser, because otherwise the browser would perform a refresh after the submit button has been clicked](https://www.robinwieruch.de/react-preventdefault).

submit 处理程序目前还不能添加新待办事项，应该在提交新待办事项之后使 input field 的值再次变为空。它还应该防止 [浏览器的默认行为，否则浏览器会在单击 submit 按钮后执行刷新（暂无译文）]()。

In order to add the todo item to our list of todo items, we need to make the todo items managed as state within the component as well. We can use again the useState hook:

为了能够将待办事项添加到列表中，我们还需要将待办事项纳入组件的状态管理。我们可以再次使用 useState：

```js
const App = () => {
  const [todos, setTodos] = useState(initialTodos);
  const [task, setTask] = useState('');

  ...

  return (
    <div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <label>{todo.task}</label>
          </li>
        ))}
      </ul>

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={task}
          onChange={handleChangeInput}
        />
        <button type="submit">Add Todo</button>
      </form>
    </div>
  );
};
```

By having the `setTodos` function at our disposal, we can add the new todo item to the list. The [built-in array concat method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) can be used for this kind of scenario. Also the [shorthand property name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) is used to allocate the task property in the object:

通过使用 `setTodos` 函数，我们可以将新的待办事项添加到列表中。[数组内置的 concat 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) 可用于这种场景。此外，[简写属性名](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) 用于分配对象中的 task 属性：

```js
const App = () => {
  const [todos, setTodos] = useState(initialTodos);
  const [task, setTask] = useState('');

  const handleChangeInput = event => {
    setTask(event.target.value);
  };

  const handleSubmit = event => {
    if (task) {
      setTodos(todos.concat({ id: 'd', task, complete: false }));
    }

    setTask('');

    event.preventDefault();
  };

  ...
};
```

There is one flaw in this implementation. The new todo item has always the same identifier, which shouldn't be this way for a unique identifier. That's why we can use a library to generate a unique identifier for us. First, you can install it on the command line:

这个实现有一个缺陷。新待办事项总是具有相同的标识符，标识符不应该是这样，应该是唯一的。我们可以使用一个库来为我们生成唯一标识符。首先，你可以在命令行执行安装：

```js
npm install uuid
```

Second, you can use it to generate a unique identifier:

第二步，可以用它来生成一个唯一标识符：

```js
import React, { useState } from 'react';
import uuid from 'uuid/v4';

const initialTodos = [
  {
    id: uuid(),
    task: 'Learn React',
    complete: true,
  },
  {
    id: uuid(),
    task: 'Learn Firebase',
    complete: true,
  },
  {
    id: uuid(),
    task: 'Learn GraphQL',
    complete: false,
  },
];

const App = () => {
  const [todos, setTodos] = useState(initialTodos);
  const [task, setTask] = useState('');

  const handleChangeInput = event => {
    setTask(event.target.value);
  };

  const handleSubmit = event => {
    if (task) {
      setTodos(todos.concat({ id: uuid(), task, complete: false }));
    }

    setTask('');

    event.preventDefault();
  };

  ...
};
```

You have implemented your second use case for managing state in React by appending an item to a list of items. Again it was possible with the useState hook. The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/37144e9ef12bb3c46ed509e26e7ab49c46cfa3d5/src/App.js) and all changes [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/37144e9ef12bb3c46ed509e26e7ab49c46cfa3d5).

你已经实现了在 React 中管理状态的第二个用例，可以将一个待办事项添加到列表中。这里两次使用到 useState 钩子。完整的源代码可以在 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/37144e9ef12bb3c46ed509e26e7ab49c46cfa3d5/src/App.js) 查看，所有的修改可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/37144e9ef12bb3c46ed509e26e7ab49c46cfa3d5) 查看。

Last but not least, let's implement a checkbox for each todo item in the list to toggle their complete flags from false to true or true to false.

最后，让我们为列表中的每个待办事项实现一个复选框，用于将它们的 complete 标志从 false 切换为 true 或 true 切换为 false。

```js
const App = () => {
  const [todos, setTodos] = useState(initialTodos);
  const [task, setTask] = useState('');

  const handleChangeCheckbox = event => {

  };

  ...

  return (
    <div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <label>
              <input
                type="checkbox"
                checked={todo.complete}
                onChange={handleChangeCheckbox}
              />
              {todo.task}
            </label>
          </li>
        ))}
      </ul>

      ...
    </div>
  );
};
```

Since we need the id of the todo item in our handler function, and not the event, we use a wrapping arrow function to pass the identifier of the individual todo item to our handler:

我们在处理函数中需要待办事项的 id，而不是 event，因此我们使用一个包装箭头函数来传递单个待办事项的标识符给我们的处理程序：

**译注：直接赋值匿名函数给 onChange 可能会引发性能问题，每次渲染都创建一个匿名对象，还可能引发子组件不必要的重新渲染。**

```js
const App = () => {
  const [todos, setTodos] = useState(initialTodos);
  ...

  const handleChangeCheckbox = id => {

  };

  ...

  return (
    <div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <label>
              <input
                type="checkbox"
                checked={todo.complete}
                onChange={() => handleChangeCheckbox(todo.id)}
              />
              {todo.task}
            </label>
          </li>
        ))}
      </ul>

      ...
    </div>
  );
};
```

Last, by having the id at our disposal, we can only alter the affected todo item in our list -- by negating the complete flag -- and return every other todo item as before. [By using the map method, we return a new array made up of the changed todo item and the remaining todo items](https://www.robinwieruch.de/react-state-array-add-update-remove/):

最后，通过使用 id，我们只更改列表中受影响的待办事项（通过将 complete 标志取非）并像以前一样返回每个其他待办事项。[通过使用 map 方法，我们返回一个新数组，该数组由更改后的待办事项和剩余的待办事项组成（暂缺译文）]()：

```js
const App = () => {
  const [todos, setTodos] = useState(initialTodos);
  ...

  const handleChangeCheckbox = id => {
    setTodos(
      todos.map(todo => {
        if (todo.id === id) {
          return { ...todo, complete: !todo.complete };
        } else {
          return todo;
        }
      })
    );
  };

  ...

  return (
    <div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <label>
              <input
                type="checkbox"
                checked={todo.complete}
                onChange={() => handleChangeCheckbox(todo.id)}
              />
              {todo.task}
            </label>
          </li>
        ))}
      </ul>

      ...
    </div>
  );
};
```

That's it. The new todo items are immediately set as state for the list of todo items with the `setTodos` function. The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/8f727cebe7079a0f72d6104e63712e1026ed1806/src/App.js) and all changes [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/8f727cebe7079a0f72d6104e63712e1026ed1806). Congratulations, you have implemented a whole todo application with three use cases for state management with the useState hook:

就是这样。使用 `setTodos` 函数立即将新的待办事项纳入列表的状态。完整的源代码可以可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/8f727cebe7079a0f72d6104e63712e1026ed1806/src/App.js) 查看，所有的变化所有的修改可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/8f727cebe7079a0f72d6104e63712e1026ed1806) 查看。祝贺你，你已经实现了一个完整的待办事项应用程序，其中包含三个使用 useState 钩子进行状态管理的用例：

- input field state for tracking task property of new todo item

用于跟踪新待办事项的 task 属性的 input field 状态

- adding a todo item to list with a submit button

使用 submit 按钮向列表添加待办事项

- checking (and unchecking) a todo item with checkboxes

用复选框选中（和取消选中）待办事项

## Exercises:

- Read more about [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook)

阅读更多：[React 的 useState 钩子（暂缺译文）]()

## React useReducer: complex State

**React useReducer：复杂状态**

The useState hook is great to manage simple state. However, once you run into more complex state objects or state transitions -- which you want to keep maintainable and predictable --, the [useReducer hook](https://www.robinwieruch.de/react-usereducer-hook/) is a great candidate to manage them. [Here you can find a comparison of when to use the useState or useReducer hook.](https://www.robinwieruch.de/react-usereducer-vs-usestate/) Let's continue implementing our application with the useReducer hook by going through a simpler example first. In our next scenario, we want to add buttons to filter our list of todos for three cases:

useState 钩子是强大的简单状态管理利器。然而，一旦遇到更复杂的状态对象或状态转换（希望保持可维护性和可预测性），[useReducer 钩子（暂缺译文）]() 是管理它们的最佳选项。在 [这里你可以找到何时使用 useState 或 useReducer 钩子的比较](/React/How-to-useReducer-in-React.md)。让我们通过一个更简单的示例来继续使用 useReducer 钩子实现我们的应用程序。在我们的下一个场景中，我们想添加三个按钮来过滤我们的待办事项列表：

- show all todo items（显示所有待办事项）
- show only complete todo items（仅显示已完成待办事项）
- show only incomplete todo items（仅显示未完成待办事项）

Let's see how we can implement these with three buttons:

让我们看看如何用三个按钮实现这些需求：

```js
const App = () => {
  ...

  const handleShowAll = () => {

  };

  const handleShowComplete = () => {

  };

  const handleShowIncomplete = () => {

  };

  ...

  return (
    <div>
      <div>
        <button type="button" onClick={handleShowAll}>
          Show All
        </button>
        <button type="button" onClick={handleShowComplete}>
          Show Complete
        </button>
        <button type="button" onClick={handleShowIncomplete}>
          Show Incomplete
        </button>
      </div>

      ...
    </div>
  );
};
```

We will care later about these. Next, let's see how we can map the three cases in a reducer function:

我们稍后会讲到实现细节。接下来，让我们看看如何在一个 reducer 函数中映射这三种情况：

```js
const filterReducer = (state, action) => {
  switch (action.type) {
    case "SHOW_ALL":
      return "ALL";
    case "SHOW_COMPLETE":
      return "COMPLETE";
    case "SHOW_INCOMPLETE":
      return "INCOMPLETE";
    default:
      throw new Error();
  }
};
```

A reducer function always receives the current state and an action as arguments. Depending on the mandatory type of the action, it decides what task to perform in the switch case statement, and returns a new state based on the implementation details. In our case, the implementation details are straightforward:

reducer 函数始终将当前状态和 action 作为参数接收。根据 action 强制要求存在的 type 属性，决定在 switch case 语句中执行什么任务，并根据实现细节返回一个新状态。在我们的例子中，实现细节很简单：

- In case of action type `SHOW_ALL`, return `ALL` string as state.

如果 action 类型为 `SHOW_ALL`，则返回 `ALL` 字符串作为状态。

- In case of action type `SHOW_COMPLETE`, return `COMPLETE` string as state.

如果 action 类型为 `SHOW_COMPLETE`，则返回 `COMPLETE` 字符串作为状态。

- In case of action type `SHOW_INCOMPLETE`, return `INCOMPLETE` string as state.

如果 action 类型为 `SHOW_INCOMPLETE`，则返回 `INCOMPLETE` 字符串作为状态。

- If none of the action types are matched, throw an error to notify us about a bad implementation.

如果没有匹配任何 action 类型，则抛出一个错误来通知我们错误的实现。

Now we can use the reducer function in a useReducer hook. It takes the reducer function and an initial state and returns the filter state and the dispatch function to change it:

现在我们可以在 useReducer 钩子中使用 reducer 函数了。用 reducer 函数、初始状态、返回过滤后状态、 dispatch 函数来修改代码：

```js
import React, { useState, useReducer } from 'react';

...

const App = () => {
  const [filter, dispatchFilter] = useReducer(filterReducer, 'ALL');

  ...
};
```

First, the dispatch function can be used with an action object -- with an action type which is used within the reducer to evaluate the new state:

首先，dispatch 函数可以与 action 对象一起使用（action 的 type 属性用于在 reducer 中计算新状态）：

```js
const App = () => {
  const [filter, dispatchFilter] = useReducer(filterReducer, 'ALL');

  ...

  const handleShowAll = () => {
    dispatchFilter({ type: 'SHOW_ALL' });
  };

  const handleShowComplete = () => {
    dispatchFilter({ type: 'SHOW_COMPLETE' });
  };

  const handleShowIncomplete = () => {
    dispatchFilter({ type: 'SHOW_INCOMPLETE' });
  };

  ...
};
```

Second, -- after we are able to transition from state to state with the reducer function and the action with action type -- the filter state can be used to show only the matching todo items by using the [built-in array filter method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter):

第二步，（在我们能够使用 reducer 函数和 action 类型从一个状态转换到另一个状态之后）filter 状态可以使用 [数组内置的 filter 方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 只显示匹配的待办事项：

```js
const App = () => {
  const [filter, dispatchFilter] = useReducer(filterReducer, 'ALL');

  ...

  const filteredTodos = todos.filter(todo => {
    if (filter === 'ALL') {
      return true;
    }

    if (filter === 'COMPLETE' && todo.complete) {
      return true;
    }

    if (filter === 'INCOMPLETE' && !todo.complete) {
      return true;
    }

    return false;
  });

  ...

  return (
    <div>
      ...

      <ul>
        {filteredTodos.map(todo => (
          <li key={todo.id}>
            <label>
              <input
                type="checkbox"
                checked={todo.complete}
                onChange={() => handleChangeCheckbox(todo.id)}
              />
              {todo.task}
            </label>
          </li>
        ))}
      </ul>

      ...
    </div>
  );
};
```

The filter buttons should work now. Every time a button is clicked an action with an action type is dispatched for the reducer function. The reducer function computes then the new state. Often the current state from the reducer function's argument is used to compute the new state with the incoming action. But in this simpler example, we only transition from one JavaScript string to another string as state.

过滤器按钮现在应该可以工作了。每当单击一个按钮时，就会为 reducer 函数分配一个类型的 action。然后计算 reducer 函数的新状态。通常使用来自 reducer 函数参数的当前状态来计算对应 action 的新状态。在这个很简单的示例中，我们只将一个 JavaScript 字符串转换为另一个字符串作为状态。

The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/08d4b7130613eef209687f5f4c270c86716f1f09/src/App.js) and all changes [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/08d4b7130613eef209687f5f4c270c86716f1f09).

完整的源代码可以可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/08d4b7130613eef209687f5f4c270c86716f1f09/src/App.js) 查看，所有的变化所有的修改可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/08d4b7130613eef209687f5f4c270c86716f1f09) 查看。

_Note: The shown use case -- also every other use case with [useReducer -- can be implemented with useState](https://www.robinwieruch.de/javascript-reducer/) as well. However, even though this one is a simpler example for the sake of learning about it, it shows clearly how much its helps for the reasoning for the state transitions by just reading the reducer function._

_注意：展示的用例（以及 useReducer 的所有其他用例）[也可以用 useState 实现（暂缺译文）]()。然而，尽管是为了学习目设计的简单的示例，但是通过阅读 reducer 函数，对了解状态转换的逻辑会有很大帮助。_

The useReducer hook is great for predictable state transitions as we have seen in the previous example. Next, we are going to see how it is a good fit for complex state objects too. Therefore, we will start to manage our todo items in a reducer hook and manipulate it with the following transitions:

useReducer 钩子非常适合于可预测的状态转换，正如我们在前面的示例中看到的那样。接下来，我们将看到它如何很好的应对复杂状态对象。因此，我们将开始管理我们的待办事项目在一个 reducer 钩子和操作它与以下过渡：

- Toggle todo item to complete.（转换待办事项到已完成）
- Toggle todo item to incomplete.（转换待办事项到未完成）
- Add todo item to the list of todo items.（增加待办事项到列表）

The reducer would look like the following:

reducer 应该像下面这个样子：

```js
const todoReducer = (state, action) => {
  switch (action.type) {
    case "DO_TODO":
      return state.map((todo) => {
        if (todo.id === action.id) {
          return { ...todo, complete: true };
        } else {
          return todo;
        }
      });
    case "UNDO_TODO":
      return state.map((todo) => {
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
        complete: false,
      });
    default:
      throw new Error();
  }
};
```

The following transitions are implemented in the reducer:

以下转换在 reducer 中的实现细节：

- `DO_TODO`: If an action of this kind passes the reducer, the action comes with an additional payload, the todo item's `id`, to identify the todo item that should be changed to **complete** status.

`DO_TODO`：如果此类 action 经过 reducer，则该 action 附带一个额外的关键信息，即待办事项的 `id`，用于标识应该更改为 **complete** 状态的待办事项。

- `UNDO_TODO`: If an action of this kind passes the reducer, the action comes with an additional payload, the todo item's `id`, to identify the todo item that should be changed to **incomplete** status.

如果此类 action 经过 reducer，则该 action 附带一个额外的关键信息，即待办事项的 `id`，用于标识应该更改为 **incomplete** 状态的待办事项。

- `ADD_TODO`: If an action of this kind passes the reducer, the action comes with an additional payload, the new todo item's `task`, to concat the new todo item to the current todo items in the state.

如果此类 action 经过 reducer，则该 action 附带一个额外的关键信息，即待办事项的 `task`，将新待办事项拼接到状态中的原事项列表。

Instead of the useState hook from before, we can manage our todos with this new reducer and the initially given todo items:

我们可以用新的 reducer 和最初给定的事项列表，管理我们的事项列表，代替以前的 useState 钩子：

```js
const App = () => {
  const [todos, dispatchTodos] = useReducer(todoReducer, initialTodos);
  const [filter, dispatchFilter] = useReducer(filterReducer, 'ALL');
  const [task, setTask] = useState('');

  ...
};
```

If someone toggles a todo item with the checkbox element, a new handler is used to dispatch an action with the appropriate action type depending on the todo item's complete status:

如果使用复选框元素切换待办事项，那么将使用一个新的处理程序，根据待办事项的完整状态来 dispatch 具有适当操作类型的 action：

```js
const App = () => {
  const [todos, dispatchTodos] = useReducer(todoReducer, initialTodos);
  ...

  const handleChangeCheckbox = todo => {
    dispatchTodos({
      type: todo.complete ? 'UNDO_TODO' : 'DO_TODO',
      id: todo.id,
    });
  };

  ...

  return (
    <div>
      ...

      <ul>
        {filteredTodos.map(todo => (
          <li key={todo.id}>
            <label>
              <input
                type="checkbox"
                checked={todo.complete}
                onChange={() => handleChangeCheckbox(todo)}
              />
              {todo.task}
            </label>
          </li>
        ))}
      </ul>

      ...
    </div>
  );
};
```

If someone submits a new todo item with the button, the same handler is used but to dispatch an action with the correct action type and the name of the todo item (`task`) and its identifier (`id`) as payload:

如果用按钮提交了一个新的待办事项，那么使用相同的处理程序，但是将 action 的 type 属性和待办事项的名称 `task` 及其标识符 `id` 作为关键信息来分派一个 action：

```js
const App = () => {
  const [todos, dispatchTodos] = useReducer(todoReducer, initialTodos);
  ...

  const handleSubmit = event => {
    if (task) {
      dispatchTodos({ type: 'ADD_TODO', task, id: uuid() });
    }

    setTask('');

    event.preventDefault();
  };

  ...

  return (
    <div>
      ...

      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={task}
          onChange={handleChangeInput}
        />
        <button type="submit">Add Todo</button>
      </form>
    </div>
  );
};
```

Now everything that has been managed by useState for our todo items is managed by useReducer now. The reducer describes what happens for each state transition and how this happens by moving the implementation details in there. The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/0f33780b85cddb9a48b0237fa9ca59f483659499/src/App.js) and all changes [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/0f33780b85cddb9a48b0237fa9ca59f483659499).

现在所有由 useState 管理的 todo 项目都由 useReducer 管理。通过将实现细节移动到其中，reducer 描述了每个状态转换发生的情况以及如何发生。完整的源代码可以可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/0f33780b85cddb9a48b0237fa9ca59f483659499/src/App.js) 查看，所有的变化所有的修改可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/0f33780b85cddb9a48b0237fa9ca59f483659499) 查看。

You have seen how useState and useReducer can be used for simple and complex state management whereas useReducer gives you clear state transitions -- thus improved predictability -- and a better way to manage complex objects.

你已经看到了如何将 useState 和 useReducer 用于简单和复杂的状态管理，而 useReducer 为你提供了清晰的状态转换（从而提高了可预测性）和更好的管理复杂对象的方法。

## Exercises:

- Read more about [React's useReducer Hook](https://www.robinwieruch.de/react-usereducer-hook)

阅读更多：[React 的 useReducer 钩子](/React/How-to-useReducer-in-React.md)

## React useContext: global State

**React useContext：全局状态**

We can take our state management one step further. At the moment, the state is managed co-located to the component. That's because we only have one component after all. What if we would have a deep component tree? How could we dispatch state changes from anywhere?

我们可以使我们的状态管理更进一步。目前，状态被托管到组件级别的位置。那是因为我们毕竟只有一个组件。如果我们有一个深度组件树呢？我们如何从任何地方调度状态更改？

Let's dive into [React's Context API and the useContext hook](https://www.robinwieruch.de/react-context/) to mimic more a Redux's philosophy by making state changes available in the whole component tree. Before we can do this, let's refactor our one component into a component tree. First, the App component renders all its child components and passes the necessary state and dispatch functions to them:

让我们深入了解 [React 的上下文 API 和 useContext 钩子](/React/React-Context.md)，通过在整个组件树中提供状态更改来更多地模仿 Redux 的思想。在此之前，让我们先将一个组件重构为一个组件树。首先，App 组件渲染其所有的子组件，并将必要的状态和 dispatch 函数传递给它们：

```js
const App = () => {
  const [filter, dispatchFilter] = useReducer(filterReducer, "ALL");
  const [todos, dispatchTodos] = useReducer(todoReducer, initialTodos);

  const filteredTodos = todos.filter((todo) => {
    if (filter === "ALL") {
      return true;
    }

    if (filter === "COMPLETE" && todo.complete) {
      return true;
    }

    if (filter === "INCOMPLETE" && !todo.complete) {
      return true;
    }

    return false;
  });

  return (
    <div>
      <Filter dispatch={dispatchFilter} />
      <TodoList dispatch={dispatchTodos} todos={filteredTodos} />
      <AddTodo dispatch={dispatchTodos} />
    </div>
  );
};
```

Second, the Filter component with its buttons and handlers which are using the dispatch function:

第二步，Filter 组件、按钮及其处理程序，其中使用的 dispatch 函数如下：

```js
const Filter = ({ dispatch }) => {
  const handleShowAll = () => {
    dispatch({ type: "SHOW_ALL" });
  };

  const handleShowComplete = () => {
    dispatch({ type: "SHOW_COMPLETE" });
  };

  const handleShowIncomplete = () => {
    dispatch({ type: "SHOW_INCOMPLETE" });
  };

  return (
    <div>
      <button type="button" onClick={handleShowAll}>
        Show All
      </button>
      <button type="button" onClick={handleShowComplete}>
        Show Complete
      </button>
      <button type="button" onClick={handleShowIncomplete}>
        Show Incomplete
      </button>
    </div>
  );
};
```

Third, the TodoList and TodoItem components. Since the individual TodoItem component defines its own handler, the `onChange` event handler doesn't need to pass the todo item anymore. The item is already available in the component itself:

第三，TodoList 和 TodoItem 组件。由于单独的 TodoItem 组件定义了自己的处理程序，`onChange` 事件处理程序不再需要传递待办事项。该事项已经在组件本身：

```js
const TodoList = ({ dispatch, todos }) => (
  <ul>
    {todos.map((todo) => (
      <TodoItem key={todo.id} dispatch={dispatch} todo={todo} />
    ))}
  </ul>
);

const TodoItem = ({ dispatch, todo }) => {
  const handleChange = () =>
    dispatch({
      type: todo.complete ? "UNDO_TODO" : "DO_TODO",
      id: todo.id,
    });

  return (
    <li>
      <label>
        <input
          type="checkbox"
          checked={todo.complete}
          onChange={handleChange}
        />
        {todo.task}
      </label>
    </li>
  );
};
```

Last, the AddTodo component which uses is own local state to manage the value of the input field:

最后，AddTodo 组件使用自己的本地状态来管理 input field 的值：

```js
const AddTodo = ({ dispatch }) => {
  const [task, setTask] = useState("");

  const handleSubmit = (event) => {
    if (task) {
      dispatch({ type: "ADD_TODO", task, id: uuid() });
    }

    setTask("");

    event.preventDefault();
  };

  const handleChange = (event) => setTask(event.target.value);

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={task} onChange={handleChange} />
      <button type="submit">Add Todo</button>
    </form>
  );
};
```

In the end, we have a component tree whereas each component receives [state as props](https://www.robinwieruch.de/react-pass-props-to-component/) and dispatch functions to alter the state. Most of the state is managed by the parent App component. That's it for the refactoring. The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/d024a244193142fed675ce43d67c71039947548c/src/App.js) and all changes [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/d024a244193142fed675ce43d67c71039947548c).

最后，我们拥有了一个组件树，而每个组件接收 [状态作为 props（暂缺译文）]()，并使用 dispatch 函数来更改状态。大部分状态由父 App 组件管理。这就是重构内容。完整的源代码可以可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/d024a244193142fed675ce43d67c71039947548c/src/App.js) 查看，所有的变化所有的修改可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/d024a244193142fed675ce43d67c71039947548c) 查看。

Now, the component tree isn't very deep and it isn't difficult to pass props down. However, in larger applications it can be a burden to pass down everything several levels. That's why React came up with the idea of the context container. Let's see how we can pass the dispatch functions down with React's Context API. First, we create the context:

现在，组件树不是很深，传递 props 并不困难。但是，在较大的应用程序中，将所有内容传递到多个层级会成为一种负担。这就是 React 提出 Context 容器构想的原因。让我们看看如何使用 React 的 Context API 传递 dispatch 函数。首先，我们创建 context：

```js
import React, { useState, useReducer, createContext } from 'react';
...

const TodoContext = createContext(null);

...
```

Second, the App can use the context's Provider method to pass implicitly a value down the component tree:

第二步，App 组件能够使用 context 的 Provider 方法在组件树向下隐式传递一个值。

```js
const App = () => {
  const [filter, dispatchFilter] = useReducer(filterReducer, 'ALL');
  const [todos, dispatchTodos] = useReducer(todoReducer, initialTodos);

  const filteredTodos = todos.filter(todo => {
    ...
  });

  return (
    <TodoContext.Provider value={dispatchTodos}>
      <Filter dispatch={dispatchFilter} />
      <TodoList dispatch={dispatchTodos} todos={filteredTodos} />
      <AddTodo dispatch={dispatchTodos} />
    </TodoContext.Provider>
  );
};
```

Now, the dispatch function doesn't need to be passed down to the components anymore, because it's available in the context:

现在，dispatch 函数不再需要传递给组件，因为它在 context 中可用：

```js
const App = () => {
  const [filter, dispatchFilter] = useReducer(filterReducer, 'ALL');
  const [todos, dispatchTodos] = useReducer(todoReducer, initialTodos);

  const filteredTodos = todos.filter(todo => {
    ...
  });

  return (
    <TodoContext.Provider value={dispatchTodos}>
      <Filter dispatch={dispatchFilter} />
      <TodoList todos={filteredTodos} />
      <AddTodo />
    </TodoContext.Provider>
  );
};
```

The useContext hook helps us to retrieve the value from the context in the AddTodo component:

useContext 钩子帮助我们从 AddTodo 组件的 context 中检索值：

```js
import React, {
  useState,
  useReducer,
  useContext,
  createContext,
} from 'react';

...

const AddTodo = () => {
  const dispatch = useContext(TodoContext);

  const [task, setTask] = useState('');

  const handleSubmit = event => {
    if (task) {
      dispatch({ type: 'ADD_TODO', task, id: uuid() });
    }

    setTask('');

    event.preventDefault();
  };

  const handleChange = event => setTask(event.target.value);

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={task} onChange={handleChange} />
      <button type="submit">Add Todo</button>
    </form>
  );
};
```

Also the TodoItem component makes use of it and the dispatch function doesn't need to be passed through the TodoList component anymore:

而且，TodoItem 组件也使用了它，dispatch 函数不再需要通过 TodoList 组件传递：

```js
const TodoList = ({ todos }) => (
  <ul>
    {todos.map((todo) => (
      <TodoItem key={todo.id} todo={todo} />
    ))}
  </ul>
);

const TodoItem = ({ todo }) => {
  const dispatch = useContext(TodoContext);

  const handleChange = () =>
    dispatch({
      type: todo.complete ? "UNDO_TODO" : "DO_TODO",
      id: todo.id,
    });

  return (
    <li>
      <label>
        <input
          type="checkbox"
          checked={todo.complete}
          onChange={handleChange}
        />
        {todo.task}
      </label>
    </li>
  );
};
```

The application works again, but we are able to dispatch changes for our todo list from anywhere. If you want to continue with this application, experiment with passing down the dispatch function for the filter reducer as well. Moreover, you can pass the state coming from useReducer with React's Context API down as well. Just try it yourself. The whole source code can be seen [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/95806796700fc2ca5cd204dd2f4542ad6f08e7ee/src/App.js) and all changes [here](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/95806796700fc2ca5cd204dd2f4542ad6f08e7ee).

应用程序再次运行，我们能够从任何地方 dispatch 待办事项列表的更改。如果你想继续使用这个应用程序，也可以尝试为过滤器 reducer 传递 dispatch 函数。此外，你还可以通过向下传递 React 的状态，它来自整合了 Context API 的 useReducer。你自己试试吧。完整的源代码可以可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/blob/95806796700fc2ca5cd204dd2f4542ad6f08e7ee/src/App.js) 查看，所有的变化所有的修改可以到 [这里](https://github.com/the-road-to-learn-react/react-with-redux-philosophy/commit/95806796700fc2ca5cd204dd2f4542ad6f08e7ee) 查看。

## Exercises:

- Read more about [React's useContext Hook](https://www.robinwieruch.de/react-usecontext-hook)

阅读更多内容：[React 的 useContext 钩子](/React/How-to-useContext-in-React.md)

- Read more about [implementing Redux with React Hooks](https://www.robinwieruch.de/redux-with-react-hooks)

阅读更多内容：[使用 React 钩子实现 Redux（暂缺译文）]()

You have learned how modern state management is used in React with useState, useReducer and useContext. Whereas useState is used for simple state (e.g. input field), useReducer is a greater asset for complex objects and complicated state transitions. In larger applications, useContext can be used to pass down dispatch functions (or state) from the useReducer hook.

你已经了解在 React 中如何使用 useState、useReducer 和 useContext 实施现代状态管理。useState 用于简单的状态（例如 input field），而 useReducer 是用于复杂对象和复杂状态转换的更好的方式。在较大的应用程序中，可以使用 useContext 从 useReducer 钩子传递 dispatch 函数（或状态）。
