# How to useMemo in React

> 转译自：https://www.robinwieruch.de/react-usememo-hook

React's useMemo Hook can be used to **optimize the computation costs** of your [React function components](https://www.robinwieruch.de/react-function-component). We will got through an example component to illustrate the problem first, and then solve it with **React's useMemo Hook**.

React 的 useMemo 钩子可以用于优化 React [函数组件](/React/React-Function-Components.md) 的计算成本。我们首先通过一个示例组件来说明这个问题，然后使用 React 的 useMemo 钩子来解决它。

Keep in mind that most of the performance optimizations in React are premature. React is fast by default, so _every_ performance optimization is opt-in in case something starts to feel slow.

请记住，React 中的大多数性能优化都还不成熟。在默认情况下，React 是快速的，所以每次性能优化都是可选的，如果感觉有些东西开始慢了。

_Note: Don't mistake React's useMemo Hook with [React's memo API](https://www.robinwieruch.de/react-memo). While useMemo is used to memoize values, React memo is used to wrap React components to prevent re-renderings._

注意：不要将 React 的 useMemo 钩子与 React 的 memo API 混淆。useMemo 用于记忆值，[React memo API](React/How-to-use-React-memo.md)
用于包装 React 组件以防止重新渲染。

_Note: Don't mistake React's useMemo Hook with [React's useCallback Hook](https://www.robinwieruch.de/react-usecallback-hook). While useMemo is used to memoize values, useCallback is used to memoize functions._

注意：不要将 React 的 useMemo 钩子与 React 的 useCallback 钩子混淆。useMemo 用于缓存值，而 [React's useCallback Hook（暂缺译文）]() 用于缓存函数。

Let's take the following example of a React application which [renders a list](https://www.robinwieruch.de/react-list-component) of users and allows us to filter the users by their name. The catch: The filter happens only when a user explicitly clicks a button; not already when the user types into the input field:

让我们以下面的 React 应用程序为例，该应用程序 [渲染一个列表组件（缺少译文）]() ，并允许我们按用户名过滤用户。注意：过滤只在用户显式地单击按钮时发生；当用户输入字段时不触发：

```js
import React from "react";

const users = [
  { id: "a", name: "Robin" },
  { id: "b", name: "Dennis" },
];

const App = () => {
  const [text, setText] = React.useState("");
  const [search, setSearch] = React.useState("");

  const handleText = (event) => {
    setText(event.target.value);
  };

  const handleSearch = () => {
    setSearch(text);
  };

  const filteredUsers = users.filter((user) => {
    return user.name.toLowerCase().includes(search.toLowerCase());
  });

  return (
    <div>
      <input type="text" value={text} onChange={handleText} />
      <button type="button" onClick={handleSearch}>
        Search
      </button>

      <List list={filteredUsers} />
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

Even though the `filteredUsers` don't change when someone types into the input field, because they change only when clicking the button via the `search` state, the filter's callback function runs again and again for every keystroke in the input field:

即使当有人在输入字段中输入时 `filteredUsers` 不会改变，因为只有在通过搜索状态单击按钮时才会改变，过滤器的回调函数会对输入字段中的每一次按键一次又一次地运行：

```js
function App() {
  ...

  const filteredUsers = users.filter((user) => {
    console.log('Filter function is running ...');
    return user.name.toLowerCase().includes(search.toLowerCase());
  });

  ...
}
```

This doesn't slow down this small React application. However, if we would deal with a large set of data in this array and run the filter's callback function for every keystroke, we would maybe slow down the application. Therefore, you can use React's useMemo Hook to **memoize a functions return value(s)** and to run a function only if its dependencies (here `search`) have changed:

这不会减慢这个小 React 应用程序的速度。但是，如果我们要处理这个数组中的大量数据，并为每次击键运行过滤器的回调函数，则可能会降低应用程序的速度。因此，你可以使用 React 的 useMemo 钩子来记忆一个函数的返回值，并且只在它的依赖关系（这里搜索）发生变化时才运行一个函数：

```js
function App() {
  ...

  const filteredUsers = React.useMemo(
    () =>
      users.filter((user) => {
        console.log('Filter function is running ...');
        return user.name.toLowerCase().includes(search.toLowerCase());
      }),
    [search]
  );

  ...
}
```

Now, this function is only executed once the `search` state changes. It doesn't run if the `text` state changes, because that's not a dependency for this filter function and thus not a dependency in the dependency array for the useMemo hook. Try it yourself: Typing something into the input field should't trigger the logging, but executing the search with a button click will trigger it.

现在，这个函数只在搜索状态改变时执行。如果文本状态发生变化，它就不会运行，因为它不是此过滤器函数的依赖项，因此也不是 useMemo 钩子依赖项数组中的依赖项。您自己尝试一下：在输入字段中输入内容不会触发日志记录，但是单击按钮执行搜索将触发日志记录。

After all, you may be wondering why you wouldn't use React's useMemo Hook on all your value computations or why React's useMemo Hook isn't the default for all value computations in the first place. Internally React's useMemo Hook has to compare the dependencies from the dependency array for every re-render to decide whether it should re-compute the value. Often the computation for this comparison can be more expensive than just re-computing the value. In conclusion, React's useMemo Hook is used to memoize values.

毕竟，您可能想知道为什么不在所有值计算中使用 React 的 useMemo 钩子，或者为什么 React 的 useMemo 钩子不是所有值计算的默认值。在内部，React 的 useMemo 钩子必须在每次重新渲染时比较依赖项数组中的依赖项，以决定是否应该重新计算值。这种比较的计算通常比重新计算值要昂贵得多。总之，React 的 useMemo 钩子用于记忆值。
