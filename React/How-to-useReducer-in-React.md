
## How to useReducer in React?（在 React 中如何使用 useReducer）

> 转译自：https://www.robinwieruch.de/react-usereducer-hook

Since [React Hooks]() have been released, [function components]() can use state and side-effects. There are two hooks that are used for modern state management in React: useState and useReducer. This tutorial goes step by step through a useReducer example in React for getting you started with this React Hook for state management.

自 [React Hooks]() 发布以来，[函数组件]() 可以使用状态和副作用。React 中有两个用于现代状态管理的 hook：useState 和 useReducer。本教程将逐步介绍 React 中的 useReducer 示例，帮助你初步使用 React Hook 用于状态管理。

# Reducer in React（React 的 Reducer 实现）

If you haven't heard about reducers as concept or as implementation in JavaScript, you should read more about them over here: [Reducers in JavaScript](). This tutorial builds up on this knowledge, so be prepared what's coming. The following function is a reducer function for managing state transitions for a list of items:

如果你还没有听说过 JavaScript 中的 reduce 概念或实现，那么你应该在这里阅读更多关于它们的内容：[Reducers in JavaScript]()。本教程建立在这些知识的基础上，所以请做好准备。下面的函数是一个用于管理 item 列表状态转换的 reducer 函数：

```js
const todoReducer = (state, action) => {
  switch (action.type) {
    case 'DO_TODO':
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: true };
        } else {
          return todo;
        }
      });
    case 'UNDO_TODO':
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: false };
        } else {
          return todo;
        }
      });
    default:
      return state;
  }
};
```

There are two types of actions for an equivalent of two state transitions. They are used to toggle the `complete` boolean to true or false of a todo item. As additional payload an identifier is needed which coming from the incoming action's payload.

有两种类型的 action 分别对应两种状态转换。它们被用于将待办 item 的 `complete` 布尔值属性切换为 true 或 false。As additional payload an identifier is needed which coming from the incoming action's payload.

The state which is managed in this reducer is an array of items:

该 reducer 中管理的状态是一个 item 数组：

```javascript
const todos = [
  {
    id: 'a',
    task: 'Learn React',
    complete: false,
  },
  {
    id: 'b',
    task: 'Learn Firebase',
    complete: false,
  },
];
```

In code, the reducer function could be used the following way with an initial state and action:

在代码中，可以通过以下方式使用 reducer 函数，并带有初始状态和 action：

```js
const todos = [
  {
    id: 'a',
    task: 'Learn React',
    complete: false,
  },
  {
    id: 'b',
    task: 'Learn Firebase',
    complete: false,
  },
];

const action = {
  type: 'DO_TODO',
  id: 'a',
};

const newTodos = todoReducer(todos, action);

console.log(newTodos);
```

输出结果：
```js
[
  {
    id: 'a',
    task: 'Learn React',
    complete: true,
  },
  {
    id: 'b',
    task: 'Learn Firebase',
    complete: false,
  },
]
```

So far, everything demonstrated here is not related to React. If you have any difficulties to understand the reducer concept, please revisit the referenced tutorial from the beginning for Reducers in JavaScript. Now, let's dive into React's useReducer hook to integrate reducers in React step by step.

到目前为止，这里所演示的一切都与 React 无关。如果你在理解 reducer 概念上有任何困难，请重新阅读在 JavaScript 中从一开始引用的 reducer 教程。现在，让我们深入到 React 的 useReducer 钩子中，一步一步地将 reducer 集成到 React 中。

# React's useReducer Hook（React 的 useReducer 钩子）

The useReducer hook is used for complex state and state transitions. It takes a reducer function and an initial state as input and returns the current state and a dispatch function as output with [array destructuring]():

useReducer 钩子用于复杂的状态和状态转换。它以一个 reducer 函数和一个初始状态作为输入，返回当前状态和一个 dispatch 函数作为输出，并伴随着 [array destructuring]()：

```js
const initialTodos = [
  {
    id: 'a',
    task: 'Learn React',
    complete: false,
  },
  {
    id: 'b',
    task: 'Learn Firebase',
    complete: false,
  },
];

const todoReducer = (state, action) => {
  switch (action.type) {
    case 'DO_TODO':
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: true };
        } else {
          return todo;
        }
      });
    case 'UNDO_TODO':
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: false };
        } else {
          return todo;
        }
      });
    default:
      return state;
  }
};

const [todos, dispatch] = useReducer(todoReducer, initialTodos);
```

The dispatch function can be used to send an action to the reducer which would implicitly change the current state:

可以使用 dispatch 函数向 reducer 发送一个 action，该 action 将隐式地改变当前状态：

```js
const [todos, dispatch] = React.useReducer(todoReducer, initialTodos);

dispatch({ type: 'DO_TODO', id: 'a' });
```

The previous example wouldn't work without being executed in a React component, but it demonstrates how the state can be changed by dispatching an action. Let's see how this would look like in a React component. We will start with a [React component rendering a list of items](). Each item has a checkbox as [controlled component]():

如果不在 React 组件中执行，前面的示例将无法工作，但它演示了如何通过 dispatch 操作来更改状态。我们来看看 React 组件是怎样的。我们将从一个 [呈现 item 列表的 React 组件]() 开始。每个 item 都有一个复选框作为 [受控组件]()：

```js
import React from 'react';

const initialTodos = [
  {
    id: 'a',
    task: 'Learn React',
    complete: false,
  },
  {
    id: 'b',
    task: 'Learn Firebase',
    complete: false,
  },
];

const App = () => {
  const handleChange = () => {};

  return (
    <ul>
      {initialTodos.map(todo => (
        <li key={todo.id}>
          <label>
            <input
              type="checkbox"
              checked={todo.complete}
              onChange={handleChange}
            />
            {todo.task}
          </label>
        </li>
      ))}
    </ul>
  );
};

export default App;
```

It's not possible to change the state of an item with the handler function yet. However, before we can do so, we need to make the list of items stateful by using them as initial state for our useReducer hook with the previously defined reducer function:

目前还不能使用 handler 函数更改一个 item 的状态。但是，在能这样做之前，我们需要使用之前定义的 reducer 函数将 item 列表作为 useReducer 钩子的初始状态，从而使其有初始状态：

```js
import React from 'react';

const initialTodos = [...];

const todoReducer = (state, action) => {
  switch (action.type) {
    case 'DO_TODO':
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: true };
        } else {
          return todo;
        }
      });
    case 'UNDO_TODO':
      return state.map(todo => {
        if (todo.id === action.id) {
          return { ...todo, complete: false };
        } else {
          return todo;
        }
      });
    default:
      return state;
  }
};

const App = () => {
  const [todos, dispatch] = React.useReducer(
    todoReducer,
    initialTodos
  );

  const handleChange = () => {};

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          ...
        </li>
      ))}
    </ul>
  );
};

export default App;
```

Now we can use the handler to dispatch an action for our reducer function. Since we need the `id` as the identifier of a todo item in order to toggle its `complete` flag, we can pass the item within the handler function by using a encapsulating arrow function:

现在我们可以使用 handler 来为我们的 reducer 函数分派一个 action。由于我们需要 `id` 作为每个待办 item 的标识符，以便切换其 `complete` 标志，我们可以通过使用箭头函数在 handler 函数中传递 item：

```js
const App = () => {
  const [todos, dispatch] = React.useReducer(
    todoReducer,
    initialTodos
  );

  const handleChange = todo => {
    dispatch({ type: 'DO_TODO', id: todo.id });
  };

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <label>
            <input
              type="checkbox"
              checked={todo.complete}
              onChange={() => handleChange(todo)}
            />
            {todo.task}
          </label>
        </li>
      ))}
    </ul>
  );
};
```

This implementation works only one way though: Todo items can be completed, but the operation cannot be reversed by using our reducer's second state transition. Let's implement this behavior in our handler by checking whether a todo item is completed or not:

这个实现只能以一种方式工作：将待办 item 标记为已完成，但是不能通过我们的 reducer 状态转换来逆转这一操作。让我们在 handler 中通过检查一个待办 item 是否完成来实现这个行为：

```js
const App = () => {
  const [todos, dispatch] = React.useReducer(
    todoReducer,
    initialTodos
  );

  const handleChange = todo => {
    dispatch({
      type: todo.complete ? 'UNDO_TODO' : 'DO_TODO',
      id: todo.id,
    });
  };

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <label>
            <input
              type="checkbox"
              checked={todo.complete}
              onChange={() => handleChange(todo)}
            />
            {todo.task}
          </label>
        </li>
      ))}
    </ul>
  );
};
```

Depending on the state of our todo item, the correct action is dispatched for our reducer function. Afterward, the React component is rendered again but using the new state from the useReducer hook. The demonstrated useReducer example can be found in this [GitHub repository]().

根据待办 item 的状态，为我们的 reducer 函数分配正确的 action。然后，再次渲染 React 组件，但是使用来自 useReducer 钩子的新状态来渲染。演示的 useReducer 示例可以在这个 [GitHub 存储库]() 中找到。

React's useReducer hook is a powerful way to manage state in React. It can be used [with useState and useContext]() for modern state management in React. Also, it is often used [in favor of useState]() for complex state and state transitions. After all, the useReducer hook hits the sweet spot for middle sized applications that don't need [Redux for React]() yet.

useReducer 钩子是 React 状态管理的强大方法。它可以与 [useState 和 useContext]() 一起用于 React 中的现代状态管理。而且，在复杂的状态和状态转换中，它经常被用于 [支持 useState]()。毕竟，useReducer 钩子适用于中等规模的应用程序，这些应用程序还不需要引入 [React 的 Redux]()。