# How to useContext in React?

> 转译自：https://www.robinwieruch.de/react-usecontext-hook

This tutorial is part 2 of 2 in this series.

> Part 1: [React Context](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Context.md)

---

[React's Function Components](https://www.robinwieruch.de/react-function-component) come with [React Hooks](https://www.robinwieruch.de/react-hooks) these days. Not only can [React Hooks be used for State in React](https://www.robinwieruch.de/react-state) but also for using React's Context in a more convenient way. This tutorial shows you how to use React's useContext Hook. Before, make sure to read the previous tutorial for getting an introduction to [React's Context](https://www.robinwieruch.de/react-context/):

[React 的函数组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Function-Components.md) 这些天随着 [React 钩子](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-React-Hooks.md) 一起发布。[React 钩子不仅可以用于状态管理（暂缺译文）]()，还可以更方便地应用于 React 上下文。本教程展示如何使用 React 的 useContext 钩子。在此之前，请务必阅读前面的教程以获得对 [React's Context](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Context.md) 的认知：

- Why React Context?
- What is React Context?
- How to use React Context?
- When to use React Context?

If you can answer these questions by reading the previous tutorial, return to this tutorial to learn about React's useContext Hook. Let's get started ...

如果你可以通过阅读前一篇教程来回答这些问题，那么请回到本教程来学习 React 的 useContext 钩子。让我们开始吧……

## React's useContext Hook

React's Context is initialized with React's `createContext` top-level API. In this case, we are using React's Context for sharing a theme (e.g. color, paddings, margins, font-sizes) across our React components. For the sake of keeping it simple, the theme will only be a color (e.g. `green`) here:

React 的上下文由 React 的 `createContext` 顶级 API 初始化。在本例中，我们使用 React 的上下文在组件之间共享一个主题（例如颜色、划片、边距、字体大小）。为了简单起见，主题将只使用一种颜色（`green`）：

```js
// src/ThemeContext.js

import React from "react";

const ThemeContext = React.createContext(null);

export default ThemeContext;
```

The Context's Provider component can be used to provide the theme to all React child components below this React top-level component which uses the Provider:

上下文的 Provider 组件可向下面所有 React 子组件提供主题：

```js
// src/ComponentA.js

import React from "react";
import ThemeContext from "./ThemeContext";

const A = () => (
  <ThemeContext.Provider value="green">
    <D />
  </ThemeContext.Provider>
);
```

Somewhere below component A, not component D in this case, but any other child component, let's say component C, will use this theme to style itself:

在组件 A 下面的某个地方，这里不是组件 D，而是任何其他的子组件，比如组件 C，都可以使用这个主题来设置自己的样式：

```js
// src/ComponentC.js

import React from "react";
import ThemeContext from "./ThemeContext";

const C = () => (
  <ThemeContext.Consumer>
    {color => <p style={{ color }}>Hello World</p>}
  </ThemeContext.Consumer>
);
```

That's the most basic approach of using React's Context API with a single top-level Provider component and one Consumer component in a React child component sitting somewhere below. There can be many more child components using the Consumer component though, but they have to be located somewhere below the component using the Provider component.

这是使用 React 上下文 API 的最基本方法，其中包含一个顶级 Provider 组件和位于某个位置的子组件包含的 Consumer 组件。虽然可以有更多的子组件使用 Provider 组件，但是它们是使用 Provider 的组件的子组件。

Now comes the crucial part where we shift towards React's useContext Hook. As you can see, the Consumer component coming from React's Context is by default a [render prop component](https://www.robinwieruch.de/react-render-props). In a world where we can use React Hooks, a render prop component isn't always the best choice. Let's see the previous example with React's useContext Hook instead:

现在是我们转向 React 的 useContext 钩子的关键阶段。如你所见，来自 React 上下文的 Consumer 组件默认为 [属性渲染（暂缺译文）]()。在我们可以使用 React 钩子的世界里，属性渲染并不总是最佳选择。让我们看看之前的例子，用 React 的 useContext 钩子：

```js
// src/ComponentC.js

import React from "react";
import ThemeContext from "./ThemeContext";

const C = () => {
  const color = React.useContext(ThemeContext);

  return <p style={{ color }}>Hello World</p>;
};
```

React's useContext just uses the `Context` object -- which you have created before -- to retrieve the most recent value from it. Using the React Hooks instead of the Consumer component, makes the code more readable, less verbose, and doesn't introduce a kinda artificial component -- the Consumer component -- in between.

React 的 useContext 只使用 `Context` 对象（之前创建的对象）来检索距离最近的上下文值。使用 React 钩子而不是 Consumer 组件，可以提高代码的可读性，减少代码的冗余，并且不会引入一种人为的组件（Consumer 组件）。