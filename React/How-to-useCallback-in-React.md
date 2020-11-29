# How to useCallback in React

> 转译自：https://www.robinwieruch.de/react-usecallback-hook

React's useCallback Hook can be used to **optimize the rendering behavior** of your [React function components](https://www.robinwieruch.de/react-function-component). We will go through an example component to illustrate the problem first, and then solve it with **React's useCallback Hook**.

React 的 useCallback 钩子可以用来 **优化 React [函数组件](/React/React-Function-Components.md) 的渲染行为**。我们首先通过一个示例组件来说明这个问题，然后使用 **React 的 useCallback 钩子** 来解决它。

Keep in mind that most of the performance optimizations in React are premature. React is fast by default, so _every_ performance optimization is opt-in in case something starts to feel slow.

请记住，React 中的大多数性能优化都还不成熟。在默认情况下，React 是快速的，所以每次性能优化都不是必须的，除非有些东西开始明显感觉慢了。

_Note: Don't mistake React's useCallback Hook with [React's useMemo Hook](https://www.robinwieruch.de/react-usememo-hook). While useCallback is used to memoize functions, useMemo is used to memoize values._

注意：不要将 React 的 useCallback 钩子与 [React 的 useMemo 钩子（暂缺译文）]() 混淆。useCallback 用于记忆函数，而 useMemo 用于记忆值。

_Note: Don't mistake React's useCallback Hook with [React's memo API](https://www.robinwieruch.de/react-memo). While useCallback is used to memoize functions, React memo is used to wrap React components to prevent re-renderings._

注意：不要将 React 的 useCallback 钩子与 React 的 memo API 混淆。useCallback 用于记忆函数，[React memo API](React/How-to-use-React-memo.md) 用于包装 React 组件以防止重新渲染。

Let's take the following example of a React application which [renders a list](https://www.robinwieruch.de/react-list-component) of items and allows us to [add items](https://www.robinwieruch.de/react-add-item-to-list) and [remove items](https://www.robinwieruch.de/react-remove-item-from-list) (with [callback handlers](https://www.robinwieruch.de/react-event-handler)) from this list (here users). We are using [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook) to make this list stateful:

让我们以下面的 React 应用程序为例，该应用程序 [渲染一个项目列表（暂缺译文）]()，并允许我们向这个列表添加项目（用户）和删除项目（使用回调处理）。我们使用 React 的 [useState 钩子（暂缺译文）]() 使这个列表有状态：

```js
import React from "react";
import { v4 as uuidv4 } from "uuid";

const App = () => {
  const [users, setUsers] = React.useState([
    { id: "a", name: "Robin" },
    { id: "b", name: "Dennis" },
  ]);

  const [text, setText] = React.useState("");

  const handleText = (event) => {
    setText(event.target.value);
  };

  const handleAddUser = () => {
    setUsers(users.concat({ id: uuidv4(), name: text }));
  };

  const handleRemove = (id) => {
    setUsers(users.filter((user) => user.id !== id));
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleText} />
      <button type="button" onClick={handleAddUser}>
        Add User
      </button>

      <List list={users} onRemove={handleRemove} />
    </div>
  );
};

const List = ({ list, onRemove }) => {
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} onRemove={onRemove} />
      ))}
    </ul>
  );
};

const ListItem = ({ item, onRemove }) => {
  return (
    <li>
      {item.name}
      <button type="button" onClick={() => onRemove(item.id)}>
        Remove
      </button>
    </li>
  );
};

export default App;
```

Following this guide about [React memo](https://www.robinwieruch.de/react-memo) (if you don't know React memo, read the guide first and then come back), which has similar components to our example, we want to prevent the re-rendering for every component when a user uses the input field.

按照 [React memo](/React/How-to-use-React-memo) 的指南（如果你不知道 React memo，请先阅读指南，然后再返回），它的组件与我们的示例类似，我们希望防止用户输入字段时对每个组件重新渲染。

```js
const App = () => {
  console.log('Render: App');

  ...
};

const List = ({ list, onRemove }) => {
  console.log('Render: List');
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} onRemove={onRemove} />
      ))}
    </ul>
  );
};

const ListItem = ({ item, onRemove }) => {
  console.log('Render: ListItem');
  return (
    <li>
      {item.name}
      <button type="button" onClick={() => onRemove(item.id)}>
        Remove
      </button>
    </li>
  );
};
```

Typing into the input field for adding an item to the list should only trigger a re-render for the App component, but not for its child components which don't care about this state change. Thus, normally React memo will be used to prevent the update of the child components:

在向列表中添加条目应该只会触发 App 组件的重新渲染，而不会触发不关心状态变化的子组件的重新渲染。因此，通常会使用 React memo 来防止子组件的更新：

```js
const List = React.memo(({ list, onRemove }) => {
  console.log("Render: List");
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} onRemove={onRemove} />
      ))}
    </ul>
  );
});

const ListItem = React.memo(({ item, onRemove }) => {
  console.log("Render: ListItem");
  return (
    <li>
      {item.name}
      <button type="button" onClick={() => onRemove(item.id)}>
        Remove
      </button>
    </li>
  );
});
```

However, perhaps to your surprise, both function components still re-render when typing into the input field. For typing one character into the input field, you should still see the same output as before:

但是，可能令人惊讶的是，在输入字符时，两个函数组件仍然重新渲染。在输入框中输入一个字符，你仍然会看到相同的输出：

```text
// after typing one character into the input field

Render: App
Render: List
Render: ListItem
Render: ListItem
```

Let's have a look at the [props](https://www.robinwieruch.de/react-pass-props-to-component) that are passed to the List component. The `list` should stay intact, as long as no item is added or removed from the list, even though the App component re-renders after typing something into the input field. So the culprit is the `onRemove` callback handler.

让我们看看传递给 List 组件的 [属性（暂无译文）]()。`list` 应该保持不变，只要没有条目被添加或删除，即使 App 组件在输入一些内容时也不会重新渲染。因此，罪魁祸首是 onRemove 回调。

Whenever the App component re-renders, which it does whenever someone types into the input field, the `handleRemove` handler function in the App component gets re-defined. When passing this new handler as prop (callback handler) to the List component, the List component notices it as a changed prop compared to the previous render. That's why the re-render for the List and ListItem components kicks in.

每当 App 组件重新渲染时（当有人键入字符时，它就会这样），App 组件中的 handleRemove 函数就会被重新定义。当将这个新的处理程序作为属性（回调函数）传递给 List 组件时，List 组件会注意到它是与前一个渲染相比已更改的属性。这就是导致重新渲染 List 和 ListItem 组件的原因。

Finally we have our use case for React's useCallback Hook. We can use useCallback to **memoize a function** which means that this function gets only re-defined when its dependencies change:

最后，我们有了 React 的 useCallback 钩子的用例。我们可以使用 useCallback 来 **记忆一个函数**，这意味着这个函数只有在指定的依赖关系改变时才会被重新定义：

```js
const App = () => {
  ...

  const handleRemove = React.useCallback(
    (id) => setUsers(users.filter((user) => user.id !== id)),
    [users]
  );

  ...
};
```

If the stateful `users` change, due to someone adding an item or removing an item from the list, the handler function gets re-defined and the child components should re-render. However, if someone only types into the input field, the function stays isn't re-defined and stays intact. Therefore, the child components don't receive changed props and don't re-render for this case.

如果由于添加项或删除项而导致 `users` 状态发生更改，则函数将被重新定义，子组件将重新渲染。但是，如果只输入字符，函数就不会被重新定义，并且保持不变。因此，子组件不会接收更改后的属性，也不会在这种情况下重新渲染。

After all, you may be wondering why you wouldn't use React's useCallback Hook on all your functions or why React's useCallback Hook isn't the default for all functions in the first place. Internally React's useCallback Hook has to compare the dependencies from the dependency array for every re-render to decide whether it should re-define the function. Often the computation for this comparison can be more expensive than just re-defining the function.

你可能想知道，为什么不在所有函数上使用 React 的 useCallback 钩子，或者为什么 React 的 useCallback 钩子不是所有函数的默认设置。在内部，React 的 useCallback 钩子必须在每次重新渲染时比较依赖项数组中的依赖项，以决定是否应该重新定义函数。这种比较的计算通常比重新定义函数的代价更大。

In conclusion, React's useCallback Hook is used to memoize functions. It's already a small performance benefits in itself, without passing this function to another component, because this way functions aren't re-initialized for every re-render of a component. However, as you have seen, React's useCallback Hook starts to shine when using it together with React's memo API.

总之，React 的 useCallback 钩子用于记忆函数。在不将此函数传递给另一个组件的情况下，它本身已经带来了一点性能上的好处，因为通过这种方式，在组件的每次重新渲染时，函数不会重新初始化。然而，正如你所看到的，将 React 的 useCallback 钩子与 React 的 memo API 一起使用时，它开始展现其优势。
