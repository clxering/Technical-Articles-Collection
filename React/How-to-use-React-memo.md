# How to use React memo

> 转译自：https://www.robinwieruch.de/react-memo

React's memo API can be used to **optimize the rendering behavior** of your [React function components](https://www.robinwieruch.de/react-function-component). We will got through an example component to illustrate the problem first, and then solve it with **React's memo API**.

React 的 memo API 可以用来优化 React [函数组件](/React/React-Function-Components.md) 的渲染行为。我们将首先通过一个示例组件来说明这个问题，然后使用 React 的 memo API 解决它。

Keep in mind that most of the performance optimizations in React are premature. React is fast by default, so *every* performance optimization is opt-in in case something starts to feel slow.

请记住，React 中的大多数性能优化都还不成熟。在默认情况下，React 是快速的，所以每次性能优化都不是必须的，除非有些东西开始明显感觉慢了。

*Note: If your React component is still rendering with React memo, check out this guide about [React's useCallback Hook](https://www.robinwieruch.de/react-usecallback-hook). Often a re-rendering is associated with a callback handler which changes for every render.*

注意：如果你的 React 组件仍然在用 React memo 渲染，请查看 [React's useCallback Hook（暂缺译文）]() 指南。通常，重新渲染与一个回调处理程序相关联，回调处理程序在每次渲染时都会改变。

*Note: Don't mistake React's memo API with [React's useMemo Hook](https://www.robinwieruch.de/react-usememo-hook). While React memo is used to wrap React components to prevent re-renderings, useMemo is used to memoize values.*

注意：不要将 React 的 memo API 与 [React 的 useMemo 钩子（暂缺译文）]() 混淆。React memo 用于包装 React 组件以防止重新渲染，而 useMemo 用于记忆值。

Let's take the following example of a React application which [renders a list](https://www.robinwieruch.de/react-list-component) of items and allows us to [add items](https://www.robinwieruch.de/react-add-item-to-list) to this list (here users). We are using [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook) to make this list stateful:

让我们以下面的 React 应用程序为例，该应用程序 [渲染一个项目列表（暂缺译文）]()，并允许我们向这个列表添加项目（用户）。我们使用 React 的 [useState 钩子（暂缺译文）]() 使这个列表有状态：

```js
import React from 'react';
import { v4 as uuidv4 } from 'uuid';

const App = () => {
  const [users, setUsers] = React.useState([
    { id: 'a', name: 'Robin' },
    { id: 'b', name: 'Dennis' },
  ]);

  const [text, setText] = React.useState('');

  const handleText = (event) => {
    setText(event.target.value);
  };

  const handleAddUser = () => {
    setUsers(users.concat({ id: uuidv4(), name: text }));
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleText} />
      <button type="button" onClick={handleAddUser}>
        Add User
      </button>

      <List list={users} />
    </div>
  );
};

const List = ({ list }) => {
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} />
      ))}
    </ul>
  );
};

const ListItem = ({ item }) => {
  return <li>{item.name}</li>;
};

export default App;
```

If you include a `console.log` statement in the component's function body of the App, List, and ListItem components, you will see that these logging statements run every time someone types into the input field:

如果你在 App、List 和 ListItem 组件的组件函数体中包含一个 `console.log` 语句，你会看到每当输入时，这些日志语句就会运行：

```js
const App = () => {
  console.log('Render: App');

  ...
};

const List = ({ list }) => {
  console.log('Render: List');
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} />
      ))}
    </ul>
  );
};

const ListItem = ({ item }) => {
  console.log('Render: ListItem');
  return <li>{item.name}</li>;
};
```

After typing into the input field, all components re-render because the App component updates its state and all of its child components will re-render by default.

在输入后，所有组件都会重新渲染，因为应用程序组件更新了它的状态，并且默认情况下，它的所有子组件都会重新渲染。

```text
// after typing one character into the input field

Render: App
Render: List
Render: ListItem
Render: ListItem
```

That's the default behavior given by React and most of the time it's fine to keep it this way as long as your application doesn't start to feel slow.

这是 React 的默认行为，在大多数情况下，只要应用程序没有开始感觉很慢，保持这种方式就没有问题。

Once it starts to feel slow, because, in contrast to our small list, maybe a huge list of items renders every time again when a user types into the input field, you can use React's memo API to **memoize your component's function**:

一旦它开始让人感到缓慢，因为，与我们的小列表相比，若每次用户输入时可能会有一个巨大的项目列表渲染，你可以使用 React 的 memo API 来 **记忆你的组件的功能**：

```js
const List = React.memo(({ list }) => {
  console.log('Render: List');
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} />
      ))}
    </ul>
  );
});

const ListItem = ({ item }) => {
  console.log('Render: ListItem');
  return <li>{item.name}</li>;
};
```

Now, when typing into the input field, only the App component re-renders again, because only this component is affected by the changed state. The List component receives its memoized props from before which haven't changed and thus doesn't re-render at all. The ListItem follows suit without using React's memo API, because the List component prevents the re-render already.

现在，在输入时，只有 App 组件再次重新渲染，因为只有这个组件会受到状态更改的影响。列表组件接收以前没有改变的记忆属性，因此根本不重新渲染。ListItem 在不使用 React 的 memo API 的情况下遵循了同样的规则，因为 List 组件已经阻止了重新渲染。

```text
// after typing one character into the input field

Render: App
```

That's React's memo function in a nutshell. It seems like we don't need to memo the ListItem component, however, once you new add an item to the list with the button, you will see the following output with the current implementation:

简单地说，这就是 React 的 memo 功能。似乎我们不需要将 memo 应用在 ListItem 组件，然而，一旦你用按钮向列表中添加一个项目，你会看到以下输出与当前的实现:

```text
// after adding an item to the list

Render: App
Render: List
Render: ListItem
Render: ListItem
Render: ListItem
```

After adding an item to the list, the list has changed and the List component will update. That's the desired behavior now, because we want to render all the items (2 items) plus the new item (1 item). However, perhaps it would be more sufficient to only render the one new item instead of all items:

在向列表中添加项后，列表已经更改，列表组件将更新。这就是现在想要的行为，因为我们想渲染所有的项（2 项）加上新项（1 项）。然而，也许只渲染一个新项目而不是所有项目会更贴切：

```js
const List = React.memo(({ list }) => {
  console.log('Render: List');
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} />
      ))}
    </ul>
  );
});

const ListItem = React.memo(({ item }) => {
  console.log('Render: ListItem');
  return <li>{item.name}</li>;
});
```

After trying the scenario from before, by adding an item to the list, with the new implementation with React's memo function, you should see the following output:

在尝试了之前的场景之后，通过向列表中添加一个项，使用带有 React 的 memo 函数的新实现，应该会看到以下输出：

```text
// after adding an item to the list

Render: App
Render: List
Render: ListItem
```

Only the new item renders, all the previous items remain the same in the list and thus don't re-render at all. Now only the components which are affected from the state changes rerender.

只有新项目渲染，所有之前的项目在列表中保持不变，因此根本不重新渲染。现在只有受到状态更改影响的组件才会重新渲染。

After all, you may be wondering why you wouldn't use React memo on all your components or why React memo isn't the default for all React components in the first place. Internally React's memo function has to compare the previous props with the new props to decide whether it should re-render the component. Often the computation for this comparison can be more expensive than just re-rendering the component.

你可能想知道为什么不在所有组件上使用 React memo，或者为什么 React memo 不是所有 React 组件的默认设置。React 内部的 memo 函数必须比较以前的属性和新的属性，以决定是否应该重新渲染组件。通常，这种比较的计算成本可能比重新渲染更高。

In conclusion, React's memo function shines when your React components become slow and you want to improve their performance. Often this happens in data heavy components, like huge lists, where lots of components have to rerender once one data point changes.

总之，当 React 组件变慢并且想要改进它们的性能时，React 的 memo 函数就会凸显光芒。这通常发生在数据量大的组件中，比如巨大的列表，当一个数据点发生变化时，许多组件必须重新渲染。