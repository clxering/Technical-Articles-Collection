
# React Context

> 转译自：https://www.robinwieruch.de/react-context

React Context is a powerful feature. If your React application grows in size beyond a small application, there is nothing wrong in giving it a try. Many third-party libraries like Redux are using it under the hood anyway, so why not learning about it.

React Context 是一个强大的特性。如果你的 React 应用程序的大小超过了一定规模，那么尝试一下也没什么错。许多像 Redux 这样的第三方库都在使用它，所以为什么不学习一下呢？

Especially if your component hierarchy grows in vertical size, it becomes tedious [passing props several React components down](/react-pass-props-to-component/) -- from a parent component to a deeply nested child component. Most often all the React components in between are not interested in these props and just pass the props to the next child component until it reaches the desired child component.

特别是当组件层次结构垂直增长时，从父组件 [为多个 React 组件向下传递属性（暂缺译文）]() 变得非常繁琐，特别是深度嵌套的子组件。大多数情况下，中间的 React 组件对这些属性不感兴趣，只是将属性传递给下一个子组件，直到它到达所需的子组件。

This tutorial gives you a walkthrough of using React Context for a simple use case.

本教程为你提供了一个简单用例使用 React 上下文的演练。

# React Context: Why

Do you remember the last time when you had to pass props several components down your component tree? In React, quite often you are confronted with this problem which is called "prop drilling":

你还记得之前提过的为多个 React 组件向下传递属性的场景吗？在 React 中，你经常会遇到这个问题，它被称为 "prop drilling":

```js
          +----------------+
          |                |
          |        A       |
          |        |Props  |
          |        v       |
          |                |
          +--------+-------+
                   |
         +---------+-----------+
         |                     |
         |                     |
+--------+-------+    +--------+-------+
|                |    |                |
|                |    |        +       |
|       B        |    |        |Props  |
|                |    |        v       |
|                |    |                |
+----------------+    +--------+-------+
                               |
                      +--------+-------+
                      |                |
                      |        +       |
                      |        |Props  |
                      |        v       |
                      |                |
                      +--------+-------+
                               |
                      +--------+-------+
                      |                |
                      |        +       |
                      |        |Props  |
                      |        C       |
                      |                |
                      +----------------+
```

In return, this clutters every component in between which has to pass down these props without using them. React Context gives you a way out of this mess. Instead of passing down the props down through each component, you can *tunnel* props through these components implicitly with React Context. If a component needs access to the information from the context, it can *consume* it on demand, because a top-level component *provides* this information in the context.

返回时，这使每个组件之间混乱，它们必须传递这些属性而不又使用。React 上下文为你提供了一个摆脱困境的方法。与通过每个组件传递属性不同，你可以使用 React 上下文隐式地把这些组件当成「隧道」一样传递属性。如果组件需要访问来自上下文的信息，它可以按需「使用」这些信息，因为顶级组件在上下文中「提供」这些信息。

```js
          +----------------+
          |                |
          |       A        |
          |                |
          |     Provide    |
          |     Context    |
          +--------+-------+
                   |
         +---------+-----------+
         |                     |
         |                     |
+--------+-------+    +--------+-------+
|                |    |                |
|                |    |                |
|       B        |    |        D       |
|                |    |                |
|                |    |                |
+----------------+    +--------+-------+
                               |
                      +--------+-------+
                      |                |
                      |                |
                      |        E       |
                      |                |
                      |                |
                      +--------+-------+
                               |
                      +--------+-------+
                      |                |
                      |        C       |
                      |                |
                      |     Consume    |
                      |     Context    |
                      +----------------+
```

**What are use cases for React Context?** For instance, imagine your React application has a theme for a color set. There are various components in your application which need to know about the theme to style themselves. Thus, at your top-level component, you can make the theme accessible for all the React child components below. That's where React's Context comes into play.

什么是 React 上下文的使用场景？例如，假设 React 应用程序有一个颜色设置的主题。应用程序中有很多组件需要知道这个主题来设置自己的风格。因此，在顶级组件中，可以使下面的 React 子组件能够访问该主题。这就是 React 的上下文发挥作用的地方。

```js
          +----------------+
          |                |
          |       A        |
          |                |
          |     Provide    |
          |       Theme    |
          +--------+-------+
                   |
         +---------+-----------+
         |                     |
         |                     |
+--------+-------+    +--------+-------+
|                |    |                |
|                |    |                |
|       B        |    |        D       |
|                |    |                |
|                |    |                |
+----------------+    +--------+-------+
                               |
                      +--------+-------+
                      |                |
                      |                |
                      |        E       |
                      |                |
                      |                |
                      +--------+-------+
                               |
                      +--------+-------+
                      |                |
                      |        C       |
                      |                |
                      |     Consume    |
                      |       Theme    |
                      +----------------+
```

**Who provides/consumes React Context?** React component A -- our top-level component -- provides the context and React component C -- as one of the child components -- consumes the context. Somewhere in between are components D and E though. Since components D and E don't care about the information, they don't consume the context. Only component C consumes it. If component any other component below component A wants to access the context, it can consume it though.

**谁提供/消费 React 上下文？** React 组件 A（顶级组件）提供上下文，React 组件 C（子组件之一）使用上下文。介于两者之间的 D 和 E。因为组件 D 和 E 不关心信息，所以它们不消耗上下文。只有 C 组件使用它。如果组件 A 下面的任何其他组件想要访问上下文，也可以使用它。

# React Context: How

First, you have to create the React Context itself which gives you access to a Provider and Consumer component. When you create the context with React by using `createContext`, you can pass it an initial value. The initial value can be null too.

首先，你必须创建 React 上下文，它允许你访问提供者和使用者组件。当你使用 `createContext` 创建带有 React 的上下文时，你可以向它传递一个初始值。初始值也可以是 null。

```js
// src/ThemeContext.js

import React from 'react';

const ThemeContext = React.createContext(null);

export default ThemeContext;
```

Second, component A would have to provide the context with the given Provider component. In this case, its `value` is given to it right away, but it can be anything from component state (e.g. [fetched data](/react-fetching-data)) to props. If the value comes from a modifiable [React State](/react-state), the value passed to the Provider component can be changed too.

第二，组件 A 必须通过 Provider 组件提供上下文。在这种情况下，它的 `值` 是直接传给它的，可以是任何东西，不管是组件状态（例如 [使用 fetch 获取数据]()）还是属性。如果值来自可修改的 [React 状态]()，则传递给 Provider 组件的值也可以更改。

```js
// src/ComponentA.js

import React from 'react';
import ThemeContext from './ThemeContext';

const A = () => (
  <ThemeContext.Provider value="green">
    <D />
  </ThemeContext.Provider>
);
```

Component A displays only component D, doesn't pass any props to it though, but rather makes the value `green` available to all the React components below. One of the child components will be component C that consumes the context eventually.

组件 A 只显示组件 D，但不向它传递任何属性，而是将值 `green` 提供给下面的 React 组件。其中一个子组件将是最终使用上下文的组件 C。

Third, in your component C, below component D, you could consume the context object. Notice that component A doesn’t need to pass down anything via component D in the props so that it reaches component C.

第三，在组件 D 之下的组件 C 中，可以使用上下文对象。注意，组件 A 不需要通过属性中的组件 D 传递任何东西就可以到达组件 C。

```js
// src/ComponentC.js

import React from 'react';
import ThemeContext from './ThemeContext';

const C = () => (
  <ThemeContext.Consumer>
    {value => (
      <p style={{ color: value }}>
        Hello World
      </p>
    )}
  </ThemeContext.Consumer>
);
```

The component can derive its style by consuming the context. The Consumer component makes the passed context available by using a [render prop](/react-render-props). As you can imagine, following this way every component that needs to be styled according to the theme could get the necessary information from React's Context by using the ThemeContext's Consumer component now. You only have to use the Provider component which passes the value once somewhere above them.

组件可以通过使用上下文派生其样式。Consumer 组件通过 [属性渲染]() 使传递的上下文可用。可以想象，按照这种方式，需要根据主题进行样式化的每个组件现在都可以通过使用 ThemeContext 的 Consumer 组件从 React 的上下文中获得必要的信息。你只需使用 Provider 组件，该组件将值传递一次到需要的地方。

# React Context: When

When should you use React Context? Generally speaking there are two use cases when to use it:

什么时候应该使用 React 上下文？一般来说，使用它有两个场景：

* When your React component hierarchy grows vertically in size and you want to be able to pass props to child components without bothering components in between. We have used this use case as example throughout this whole React Context tutorial.

当你的 React 组件层次结构垂直增长时，你希望能够将属性传递给子组件，而不需要在中间插入组件。在整个 React 上下文教程中，我们一直使用这个用例作为示例。

* When you want to have [advanced state management in React with React Hooks](/react-state-usereducer-usestate-usecontext/) for passing state and state updater functions via React Context through your React application. Doing it via React Context allows you to create a shared and global state.

当你希望使用 [React 中的 React hook 进行高级状态管理]() ，想通过 React 上下文让应用程序传递状态和状态更新函数时。通过 React 上下文可以创建共享的全局状态。

A running application which uses React's Context can be found in this [GitHub repository](https://github.com/the-road-to-learn-react/react-context-example). After all, React Context is a great way to pass props to deeply nested React components, because it doesn't bother the components in between.

可以在这个 [GitHub 知识库](https://github.com/the-road-to-learn-react/react-context-example) 中找到使用 React 上下文的应用程序。毕竟，React 上下文是向深度嵌套的 React 组件传递属性的好方法，因为它不会干扰中间的组件。

---

This tutorial is part 1 of 2 in this series.

> Part 2: React's useContext Hook