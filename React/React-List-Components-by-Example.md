# React List Components by Example

> 转译自：https://www.robinwieruch.de/react-list-component

If you are new to React, most likely you want to know how to display a list of items in React's JSX syntax. This tutorial for List components in React gives you a step by step walkthrough on how to render a list of simple primitives, how to render a list of complex objects, and how to update the state of your list in React.

如果你是 React 的新手，那么您很可能想知道如何在 React 的 JSX 语法中显示项目列表。关于 React 中的列表组件，本教程将逐步介绍如何渲染简单基本数据类型列表、如何渲染复杂对象列表以及如何在 React 中更新列表的状态。

# How to display a list of items in React?

The following [function component](https://www.robinwieruch.de/react-function-component/) shows how to render a list of items (JS primitives). It would work the same with a list of numbers instead of strings. Since we can use JavaScript in JSX by using curly braces, we can use the [built-in JavaScript array map method](https://www.robinwieruch.de/javascript-map-array/) to iterate over our list items; and to map them from JavaScript primitive to HTML elements. Each element receives a mandatory [key prop](https://www.robinwieruch.de/react-list-key/):

下面的 [函数组件](/React/React-Function-Components.md) 显示了如何渲染项目列表（JS 基本数据类型）。对于数字列表，而不是字符串，它的工作原理是一样的。因为我们可以通过使用花括号在 JSX 中使用 JavaScript，我们可以使用内置的 [JavaScript 数组 map 方法](/JavaScript/Deep-Dive-into-JavaScript-Array-Map-Method.md) 来遍历我们的列表项；并将它们从 JavaScript 基本数据类型映射到 HTML 元素。每个元素都收到一个强制的关键属性：

```js
const SimpleList = () => (
  <ul>
    {['a', 'b', 'c'].map(function(item) {
      return <li key={item}>{item}</li>;
    })}
  </ul>
);
```

We didn't define the list but merely inlined it. In the case of declaring the list as variable, it would look like the following:

我们没有定义列表，只是将其内联。在将列表声明为变量的情况下，它看起来像下面这样：

```js
const list = ['a', 'b', 'c'];

const SimpleList = () => (
  <ul>
    {list.map(function(item) {
      return <li key={item}>{item}</li>;
    })}
  </ul>
);
```

We can also use JavaScript arrow function to make the inline function for the map more lightweight:

我们也可以使用 JavaScript 箭头函数使内联函数的地图更轻量：

```js
const list = ['a', 'b', 'c'];

const SimpleList = () => (
  <ul>
    {list.map(item => {
      return <li key={item}>{item}</li>;
    })}
  </ul>
);
```

Since we are not doing anything in the function's block body, we can also refactor it to a concise body and omit the return statement and the curly braces for the function body:

因为我们没有在函数体中做任何事情，我们也可以将它重构为一个简洁的函数体，省略函数体的 return 语句和花括号：

```js
const list = ['a', 'b', 'c'];

const SimpleList = () => (
  <ul>
    {list.map(item => (
      <li key={item}>{item}</li>
    ))}
  </ul>
);
```

If we would use the List as child component in another component, we could pass the list as [props](https://www.robinwieruch.de/react-pass-props-to-component/) to it:

如果我们在另一个组件中使用列表作为子组件，我们可以将列表作为 [属性（暂无译文）]() 传递给它：

```js
const mylist = ['a', 'b', 'c'];

const App = () => (
  <SimpleList list={mylist} />
);

const SimpleList = ({ list }) => (
  <ul>
    {list.map(item => (
      <li key={item}>{item}</li>
    ))}
  </ul>
);
```

That's a simple list component example for React. We only have a list of JavaScript primitives, like strings or integers, map over it, and render a HTML listitem for each item in our array.

这是 React 的一个简单列表组件示例。我们只有一个 JavaScript 基本数据类型列表，如字符串或整数，映射它，并为数组中的每一项呈现一个 HTML 列表项。

# How to display a list of objects in React?

It doesn't work any different for complex objects in JavaScript arrays. You iterate over the list with the map method again and output for each list item your HTML elements. Only the value argument given in the map function is an object and no primitive anymore. Hence you can access the object in your JSX to output the different properties of it:

对于 JavaScript 数组中的复杂对象，它没有任何不同。再次使用 map 方法遍历列表，并为每个列表项输出 HTML 元素。只有 map 函数中给出的值参数是对象，不再是基本数据类型了。因此，你可以访问您的 JSX 中的对象，以输出它的不同属性：

```js
import React from 'react';

const list = [
  {
    id: 'a',
    firstname: 'Robin',
    lastname: 'Wieruch',
    year: 1988,
  },
  {
    id: 'b',
    firstname: 'Dave',
    lastname: 'Davidds',
    year: 1990,
  },
];

const ComplexList = () => (
  <ul>
    {list.map(item => (
      <li key={item.id}>
        <div>{item.id}</div>
        <div>{item.firstname}</div>
        <div>{item.lastname}</div>
        <div>{item.year}</div>
      </li>
    ))}
  </ul>
);

export default ComplexList;
```

The implementation isn't much different here. We map a list of objects over to a list of HTML elements. If you would want a list table in favor of a bullet list, you would have to use table, th, tr, td elements instead of ul, li list elements.

这里的实现没有太大的不同。我们将一个对象列表映射到一个 HTML 元素列表。如果希望使用列表表而不是项目列表，则必须使用 table、th、tr、td 元素，而不是 ul、li 列表元素。

# How to display nested lists in React?

If you run into 2 dimensional lists, you can still use the same implementation techniques from before. First, map over your first array and then map over your second array within. For the sake of simplification, we use two times the same list in our nested list:

如果遇到二维列表，仍然可以使用以前的相同实现技术。首先，映射你的第一个数组然后映射你的第二个数组。为了简化，我们在嵌套列表中使用了两个相同的列表：

```js
import React from 'react';

const list = [
  {
    id: 'a',
    firstname: 'Robin',
    lastname: 'Wieruch',
    year: 1988,
  },
  {
    id: 'b',
    firstname: 'Dave',
    lastname: 'Davidds',
    year: 1990,
  },
];

const nestedLists = [list, list];

const NestedList = () => (
  <ul>
    {nestedLists.map((nestedList, index) => (
      <ul key={index}>
        <h4>List {index + 1}</h4>
        {nestedList.map(item => (
          <li key={item.id}>
            <div>{item.id}</div>
            <div>{item.firstname}</div>
            <div>{item.lastname}</div>
            <div>{item.year}</div>
          </li>
        ))}
      </ul>
    ))}
  </ul>
);

export default NestedList;
```

However, nesting multiple maps into each other becomes quickly a burden for the maintainability of the application. That's why it's great to extract smaller components from your large list component (e.g. NestedList, List, Item components).

# React List Components

In order to keep your React list components tidy, you can extract them to standalone component that only care about their concerns. For instance, the List component makes sure to map over the array to render a list of ListItem components for each item as child component:

```js
const list = [
  {
    id: 'a',
    firstname: 'Robin',
    lastname: 'Wieruch',
    year: 1988,
  },
  {
    id: 'b',
    firstname: 'Dave',
    lastname: 'Davidds',
    year: 1990,
  },
];

const App = () => <List list={list} />;

const List = ({ list }) => (
  <ul>
    {list.map(item => (
      <ListItem key={item.id} item={item} />
    ))}
  </ul>
);

const ListItem = ({ item }) => (
  <li>
    <div>{item.id}</div>
    <div>{item.firstname}</div>
    <div>{item.lastname}</div>
    <div>{item.year}</div>
  </li>
);
```

The List component offers an [API](https://www.robinwieruch.de/what-is-an-api-javascript/) to the outside: This way the App component can pass the array as list props to the List component. One little trick for [conditional rendering](https://www.robinwieruch.de/conditional-rendering-react/): If you don't know whether the incoming list is null or undefined, default to an empty list yourself:

```js
const List = ({ list }) => (
  <ul>
    {(list || []).map(item => (
      <ListItem key={item.id} item={item} />
    ))}
  </ul>
);
```

The List and ListItem component are used so often in React applications, that they can be taken as list template or boilerplate, because often you just copy and paste the same implementation for a simple list over to your code. So keeping this structure for a list component in mind doesn't harm.

# React List: Update Items

So far, we have only seen list items that are not changed, because they are just declared a variable or passed down as props. But what about a list managed as state? It's possible to add, update, and remove items to/in/from the list. For all following examples, we will start with the same boilerplate list component:

```js
import React from 'react';

const initialList = [];

const List = () => {
  const [list, setList] = React.useState(initialList);

  return (
    <div>
      <ul>
        {list.map(item => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    </div>
  );
};

export default List;
```

Let's dive into the different examples to update our list items with [React Hooks](https://www.robinwieruch.de/react-hooks). All the following patterns are the foundation for sophisticated [state management in React](https://www.robinwieruch.de/react-state).

## React List: Add Item

The following List component shows a stateful managed list where it's possible to add an item to the list with a [controlled form element](https://www.robinwieruch.de/react-controlled-components/):

```js
import React from 'react';

const initialList = [
  'Learn React',
  'Learn Firebase',
  'Learn GraphQL',
];

const ListWithAddItem = () => {
  const [value, setValue] = React.useState('');
  const [list, setList] = React.useState(initialList);

  const handleChange = event => {
    setValue(event.target.value);
  };

  const handleSubmit = event => {
    if (value) {
      setList(list.concat(value));
    }

    setValue('');

    event.preventDefault();
  };

  return (
    <div>
      <ul>
        {list.map(item => (
          <li key={item}>{item}</li>
        ))}
      </ul>

      <form onSubmit={handleSubmit}>
        <input type="text" value={value} onChange={handleChange} />
        <button type="submit">Add Item</button>
      </form>
    </div>
  );
};

export default ListWithAddItem;
```

By using the submit button to initiate the creation of the item, the handler makes sure to add the item to the stateful list. Also the [native browser behavior is prevented](https://www.robinwieruch.de/react-preventdefault) by using the click event; otherwise the browser would refresh after the submit event.

## React List: Update Item

The following List component shows a stateful managed list where it's possible to update an item in the list with a input element:

```js
import React from 'react';

const initialList = [
  { id: 'a', name: 'Learn React', complete: false },
  { id: 'b', name: 'Learn Firebase', complete: false },
  { id: 'c', name: 'Learn GraphQL', complete: false },
];

const ListWithUpdateItem = () => {
  const [list, setList] = React.useState(initialList);

  const handleChangeCheckbox = id => {
    setList(
      list.map(item => {
        if (item.id === id) {
          return { ...item, complete: !item.complete };
        } else {
          return item;
        }
      })
    );
  };

  return (
    <ul>
      {list.map(item => (
        <li key={item.id}>
          <label>
            <input
              type="checkbox"
              checked={item.complete}
              onChange={() => handleChangeCheckbox(item.id)}
            />
            {item.name}
          </label>
        </li>
      ))}
    </ul>
  );
};

export default ListWithUpdateItem;
```

By using the checkbox element to initiate the update of the item, the handler makes sure to toggle the boolean flag of the item in the stateful list.

## React List: Remove Item

The following List component shows a stateful managed list where it's possible to remove an item from the list with a button element:

```js
import React from 'react';

const initialList = [
  { id: 'a', name: 'Learn React' },
  { id: 'b', name: 'Learn Firebase' },
  { id: 'c', name: 'Learn GraphQL' },
];

const ListWithRemoveItem = () => {
  const [list, setList] = React.useState(initialList);

  const handleClick = id => {
    setList(list.filter(item => item.id !== id));
  };

  return (
    <ul>
      {list.map(item => (
        <li key={item.id}>
          <label>{item.name}</label>
          <button type="button" onClick={() => handleClick(item.id)}>
            Remove
          </button>
        </li>
      ))}
    </ul>
  );
};

export default ListWithRemoveItem;
```

By using the delete button to initiate the removal of the item, the handler makes sure to remove the item from the stateful list. React makes sure to update the list after the delete when using the function to set the state.

<Divider />

All the implementations from this tutorial can be found in this [GitHub repository](https://github.com/the-road-to-learn-react/react-list-component). These were the fundamentals to deal with lists in React. To follow up, here are some more (in-depth) tutorials that teach more in-detail topics:

* [React List with Scroll to Item](https://www.robinwieruch.de/react-scroll-to-item/)
* [React List with Pagination](https://www.robinwieruch.de/react-paginated-list/)
* [React List with Infinite Scroll](https://www.robinwieruch.de/react-infinite-scroll/)
* [React List with Filter, Sort, and Client-/Server-Side Search](https://www.robinwieruch.de/the-road-to-learn-react/)
