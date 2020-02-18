# React Function Components（React 的函数组件）

> 转译自：https://www.robinwieruch.de/react-function-component

React Function Components -- also known as React Functional Components -- are the status quo of writing modern React applications. In the past, there have been various [React Component Types](https://www.robinwieruch.de/react-component-types/), but with the introduction of [React Hooks](https://www.robinwieruch.de/react-hooks/) it's possible to write your entire application with just functions as React components.

React Function Components（也称为 React 函数组件）是现代 React 应用程序的组成模块。在过去，有多种 [React 组件类型（暂缺译文）]()，但是随着 [React 钩子](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-React-Hooks.md) 的引入，仅用 React 函数组件来编写整个应用程序成为可能。

This in-depth guide shows you everything about React Function Components -- which are basically **just JavaScript Functions being React Components** which return JSX (React's Syntax) -- so that after you have read this tutorial you should be well prepared to implement modern React applications with them.

这个深入的指南向你展示了 React 函数组件的所有内容（基于返回 JSX （React 语法）的 React 组件的 JavaScript 函数），因此，在阅读本教程之后，你应该做好充分准备，用它们实现现代的 React 应用程序。

*Note: There are several synonyms for this kind of component in React. You may have seen different variations such as "React Function only Component" or "React Component as Function".*

*注意：React 中有几种关于此类组件的同义词。你可能见过不同的变化，如 React Function only Component 或 React Component as Function*

## Table of Contents（目录列表）

- [React Function Component Example（函数组件的例子）](#react-function-component-example函数组件的例子)
- [React Function Component: props（函数组件的 props）](#react-function-component-props函数组件的-props)
- [React Arrow Function Component（React 的箭头函数组件）](#react-arrow-function-componentreact-的箭头函数组件)
- [React Stateless Function Component（React 的无状态函数组件）](#react-stateless-function-componentreact-的无状态函数组件)
- [React Function Component: state（React 函数组件之：状态）](#react-function-component-statereact-函数组件之状态)
- [React Function Component: Event Handler（React 函数组件之：事件处理程序）](#react-function-component-event-handlerreact-函数组件之事件处理程序)
- [React Function Component: Callback Function（React 函数组件之：回调函数）](#react-function-component-callback-functionreact-函数组件之回调函数)
  - [Override Component Function with React（覆盖 React 组件）](#override-component-function-with-react覆盖-react-组件)
  - [Async Function in Component with React（在 React 组件的异步函数）](#async-function-in-component-with-react在-react-组件的异步函数)
- [React Function Component: Lifecycle（React 函数组件之生命周期）](#react-function-component-lifecyclereact-函数组件之生命周期)
  - [React Functional Component: Mount（React 函数组件之挂载）](#react-functional-component-mountreact-函数组件之挂载)
  - [React Functional Component: Update（React 函数组件之：更新）](#react-functional-component-updatereact-函数组件之更新)
- [Pure React Function Component（纯 React 函数组件）](#pure-react-function-component纯-react-函数组件)
- [React Function Component: Export and Import（React 函数组件之：Export 和 Import）](#react-function-component-export-and-importreact-函数组件之export-和-import)
- [React Function Component: ref（React 函数组件之：ref）](#react-function-component-refreact-函数组件之ref)
- [React Function Component: PropTypes（React 函数组件之：PropTypes）](#react-function-component-proptypesreact-函数组件之proptypes)
- [React Function Component: TypeScript（React 函数组件之：TypeScript）](#react-function-component-typescriptreact-函数组件之typescript)
- [React Function Component vs Class Component（React 的函数组件和类组件）](#react-function-component-vs-class-componentreact-的函数组件和类组件)

## React Function Component Example（函数组件的例子）

Let's start with a simple example of a Functional Component in React defined as App which returns JSX:

让我们从一个简单例子开始，它定义了一个 App 函数组件，并返回 JSX：

```js
import React from 'react';

function App() {
  const greeting = 'Hello Function Component!';

  return <h1>{greeting}</h1>;
}

export default App;
```

That's already the essential React Function Component Syntax. The definition of the component happens with just a JavaScript Function which has to return JSX -- React's syntax for defining a mix of HTML and JavaScript whereas the JavaScript is used with curly braces within the HTML. In our case, we render a variable called *greeting*, which is defined in the component's function body, and is returned as HTML headline in JSX.

这已经是基本的 React 函数组件语法了。组件的定义只需要一个 JavaScript 函数，该函数必须返回 JSX（React 用于定义混合 HTML 和 JavaScript 的语法，而 JavaScript 在 HTML 中使用花括号）。在例子中，我们渲染了一个名为 greeting 的变量，它在组件的函数体中定义，并在 JSX 中作为 HTML 标题返回。

*Note: If you are familiar with React Class Components, you may have noticed that a Functional Component is a React Component without render function. Everything defined in the function's body is the render function which returns JSX in the end.*

*注意：如果你熟悉 React 类组件，你可能已经注意到函数组件是没有 render 函数的 React 组件。在函数体中定义的所有内容都是 render 函数，该函数最后返回 JSX。*

Now, if you want to render a React Component inside a Function Component, you define another component and render it as HTML element with JSX within the other component's body:

如果你想在函数组件中渲染一个 React 组件，你需要定义一个新组件并将其渲染为 HTML 元素，而 JSX 则在新组件的主体中：

```js
import React from 'react';

function App() {
  return <Headline />;
}

function Headline() {
  const greeting = 'Hello Function Component!';

  return <h1>{greeting}</h1>;
}

export default App;
```

Basically you have a function as Child Component now. Defining React Components and rendering them within each other makes [Composition in React](https://www.robinwieruch.de/react-component-composition/) possible. You can decide where to render a component and how to render it.

你现在有了一个函数作为子组件。定义 React 组件并互相渲染，使得 [在 React 中组合（暂缺译文）]() 成为可能。你可以决定在何处渲染组件以及如何渲染它。

## React Function Component: props（函数组件的 props）

Let's learn about a React Function Component with props. In React, [props are used to pass information from component to component](https://www.robinwieruch.de/react-pass-props-to-component/). If you don't know about props in React, cross-read the linked article. Essentially props in React are always passed down the component tree:

让我们学习一下 React 函数组件的 props。在 React 中，[props 用于在组件之间传递信息（暂缺译文）]()。如果你不知道 React 里的 props，可交叉阅读相关文章。本质上，React 中的 props 总是沿着组件树向下传递：

```js
import React from 'react';

function App() {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
}

function Headline(props) {
  return <h1>{props.value}</h1>;
}

export default App;
```

Props are the React Function Component's parameters. Whereas the component can stay generic, we decide from the outside what it should render (or how it should behave). When rendering a component (e.g. Headline in App component), you can pass props as HTML attributes to the component. Then in the Function Component the props object is available as argument in the function signature.

props 是 React 函数组件的参数。虽然组件可以保持通用，但是我们从外部决定它应该渲染什么（或者它应该有什么行为）。当渲染一个组件时（例如在 App 组件中的 Headline），你可以将 props 作为 HTML 属性传递给组件。在函数组件中，props 对象在函数签名中是可用的参数。

Since props are always coming as object, and most often you need to extract the information from the props anyway, [JavaScript object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) comes in handy. You can directly use it in the function signature for the props object:

由于 props 总是以对象形式出现，而且大多数情况下无论如何都需要从 props 中提取信息，因此 [JavaScript 对象解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) 就派上用场了。你可以直接在 props 对象的函数签名中使用：

```js
import React from 'react';

function App() {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
}

function Headline({ value }) {
  return <h1>{value}</h1>;
}

export default App;
```

*Note: If you forget the JavaScript destructuring and just access props from the component's function signature like `function Headline(value1, value2) { ... }`, you may see a "props undefined"-messages. It doesn't work this way, because props are always accessible as first argument of the function and can be destructured from there: `function Headline({ value1, value2 }) { ... }`.*

*注意：如果你忘记了 JavaScript 的解构，而只是从组件的函数签名，如 `function Headline(value1, value2) { ... }` 访问 props，你可能会看到一个 "props undefined" 的消息。它不是这样工作的，因为 props 总是作为函数的第一个参数，可以从函数签名那里解构：`function Headline({ value1, value2 }) { ... }`。*

If you want to learn more tricks and tips about React props, again check out the linked article from the beginning of this section. There you will learn about cases where you don't want to destructure your props and simply pass them to the next child component with the ...syntax known as spread operator.

如果你想了解更多关于 React props 的技巧和提示，请再次查看本节开头的链接文章。在文章中，你将了解不对 props 解构的情况，并简单地用带有 `…` 的语法将它们传递给下一个子组件，这种语法称为扩展运算符。

## React Arrow Function Component（React 的箭头函数组件）

With the introduction of JavaScript ES6, [new coding concepts were introduced to JavaScript and therefore to React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/). For instance, a JavaScript function can be expressed as lambda ([arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)). That's why a Function Component is sometimes called Arrow Function Components (or maybe also Lambda Function Component). Let's see our refactored React Component with an Arrow Function:

随着 JavaScript ES6 的引入，[新的编码概念被引入到 JavaScript 中，React 也不例外（暂缺译文）]()。例如，一个 JavaScript 函数可以表示为 lambda [箭头函数（暂缺译文）]()。这就是为什么一个函数组件有时被称为箭头函数组件（或者 Lambda 函数组件）。让我们看看用箭头函数重构的 React 组件：

```js
import React from 'react';

const App = () => {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
};

const Headline = ({ value }) => {
  return <h1>{value}</h1>;
};

export default App;
```

Both React Arrow Function Components use a function block body now. However, the second component can be made more lightweight with a concise body for the function, because it only returns the output of the component without doing something else in between. When leaving away the curly braces, the explicit return becomes an implicit return and can be left out as well:

两个 React 箭头函数组件现在都使用一个函数块体。但是，第二个组件可以用一个简洁的函数体使其更轻量级，因为它只返回组件的输出，而不做其他事情。当去掉花括号时，显式 return 变成隐式 return，也可以省略：

```js
import React from 'react';

const App = () => {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
};

const Headline = ({ value }) =>
  <h1>{value}</h1>;

export default App;
```

When using arrow functions for React components, nothing changes for the props. They are still accessible as arguments as before. It's a React Function Component with ES6 Functions expressed as arrows instead of ES5 Functions which are the more default way of expressing functions in JS.

当 React 组件使用箭头函数时，props 不会发生任何变化。它们仍然可以作为参数使用。它是一个用箭头表示 ES6 函数的 React 函数组件，而不是在 JS 中表示函数的默认方式 ES5 函数。

*Note: If you run into a "React Component Arrow Function Unexpected Token" error, make sure that JavaScript ES6 is available for your React application. Normally when using create-react-app this should be given, otherwise, if you set up the project yourself, [Babel is enabling ES6 and beyond features for your React application](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/).*

*注意：如果遇到 React Component Arrow Function Unexpected Token 错误，请确保 React 应用程序可用 JavaScript ES6。通常情况下，当使用 create-react-app 时，应该确认这个选项，否则，如果你自己设置项目，[Babel 将为你的 React 应用程序启用 ES6 和更多功能。（暂缺译文）]()*

## React Stateless Function Component（React 的无状态函数组件）

Every component we have seen so far can be called Stateless Function Component. They just receive an input as props and return an output as JSX: `(props) => JSX`. The input, only if available in form of props, shapes the rendered output. These kind of components don't manage state and don't have any side-effects (e.g. accessing the browser's local storage). People call them Functional Stateless Components, because they are stateless and expressed by a function. However, React Hooks made it possible to have state in Function Components.

到目前为止，我们看到的每个组件都可以称为无状态函数组件。它们只是接收一个 props 的输入，并返回一个 JSX：`(props) => JSX` 作为输出。输入只能以 props 的形式提供给输出渲染。这些组件不管理状态，也没有任何副作用（例如访问浏览器的本地存储）。人们称它们为无状态函数组件，因为它们是无状态的，并由函数表示。但是，React 钩子使得函数组件中有状态成为可能。

## React Function Component: state（React 函数组件之：状态）

[React Hooks](https://www.robinwieruch.de/react-hooks/) made it possible to use state (and side-effects) in Function Components. Finally we can create a React Function Component with state! Let's say we moved all logic to our other Function Component and don't pass any props to it:

[React 钩子](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-React-Hooks.md) 使得在函数组件中使用状态（和副作用）成为可能。最后，我们可以创建一个有状态的 React 函数组件！我们把所有的逻辑都移到了另一个函数组件上，并且没有给它传递任何 props：

```js
import React from 'react';

const App = () => {
  return <Headline />;
};

const Headline = () => {
  const greeting = 'Hello Function Component!';

  return <h1>{greeting}</h1>;
};

export default App;
```

So far, a user of this application has no way of interacting with the application and thus no way of changing the greeting variable. The application is static and not interactive at all. State is what makes React components interactive; and exciting as well. A React Hook helps us to accomplish it:

到目前为止，此应用程序的用户还无法与应用程序交互，因此无法更改 greeting 变量。应用程序是静态的，完全没有交互性。状态使 React 组件具有交互性；也很令人兴奋。React Hook 帮助我们完成它：

```js
import React, { useState } from 'react';

const App = () => {
  return <Headline />;
};

const Headline = () => {
  const [greeting, setGreeting] = useState(
    'Hello Function Component!'
  );

  return <h1>{greeting}</h1>;
};

export default App;
```

The useState hook takes an initial state as parameter and returns an array which holds the current state as first item and a function to change the state as second item. We are using [JavaScript array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) to access both items with a shorthand expression. In addition, the destructuring let's us name the variables ourselves.

useState 钩子将初始状态作为参数，并返回一个数组，其中当前状态是第一项，更改状态的函数是第二项。我们正在使用 [JavaScript 数组解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 来访问这两个项与一个缩写表达式。另外，解构还可以让我们自己给变量命名。

Let's add an input field to change the state with the `setGreeting()` function:

让我们添加一个 input field 来改变 `setGreeting()` 函数的状态：

```js
import React, { useState } from 'react';

const App = () => {
  return <Headline />;
};

const Headline = () => {
  const [greeting, setGreeting] = useState(
    'Hello Function Component!'
  );

  return (
    <div>
      <h1>{greeting}</h1>

      <input
        type="text"
        value={greeting}
        onChange={event => setGreeting(event.target.value)}
      />
    </div>
  );
};

export default App;
```

By providing an event handler to the input field, we are able to do something with a callback function when the input field changes its value. As argument of the callback function we receive a [synthetic React event](https://reactjs.org/docs/events.html) which holds the current value of the input field. This value is ultimately used to set the new state for the Function Component with an inline arrow function. We will see later how to extract this function from there.

通过向 input field 提供事件处理程序，我们可以在 input field 更改其值时使用回调函数来完成某些事情。作为回调函数的参数，我们接收一个保存 input field 当前值的 [synthetic React 事件（暂缺译文）]()。此值最终用于设置带有内联箭头函数的函数组件的新状态。稍后我们将看到如何从那里提取这个函数。

*Note: The input field receives the value of the component state too, because you want to control the state (value) of the input field and don't let the native HTML element's internal state take over. Doing it this way, the component has become a [controlled component](https://www.robinwieruch.de/react-controlled-components/).*

*注意：input field 也接收组件状态的值，因为你希望控制输入字段的状态（值），而不让本机 HTML 元素的内部状态接管。这样做，组件就变成了[受控组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-Controlled-Components-in-React.md)。*

As you have seen, React Hooks enable us to use state in React (Arrow) Function Components. Whereas you would have used a setState method to write state in a Class Component, you can use the useState hook to write state in a Function Component.

如你所见，React 钩子使我们能够在 React（箭头）函数组件中使用状态。虽然可以使用 setState 方法在类组件中写入状态，但是可以使用 useState 钩子在函数组件中写入状态。

*Note: If you want to use [React's Context](https://www.robinwieruch.de/react-context/) in Function Components, check out [React's Context Hook called useContext](https://www.robinwieruch.de/react-usecontext-hook) for reading from React's Context in a component.*

注意：如果你想在函数组件中使用 [React 的上下文（暂缺译文）]()，请查看 [React 的上下文钩子 useContext（暂缺译文）]()，以便从组件中的 React 上下文中读取内容。

## React Function Component: Event Handler（React 函数组件之：事件处理程序）

In the previous example you have used an *onChange* event handler for the input field. That's appropriate, because you want to be notified every time the internal value of the input field has changed. In the case of other HTML form elements, you have several other [React event handlers](https://reactjs.org/docs/handling-events.html) at your disposal such as onClick, onMouseDown, and onBlur.

在前面的示例中，你已经为 input field 使用了 onChange 事件处理程序。这是恰当的，因为你希望在每次 input field 的内部值发生更改时都得到通知。对于其他 HTML 表单元素，你可以使用其他几个 [React 事件处理程序（暂缺译文）]()，如 onClick、onMouseDown 和 onBlur。

*Note: The onChange event handler is only one of the handlers for HTML form elements. For instance, a button would offer an onClick event handler to react on click events.*

*注意：onChange 事件处理程序只是 HTML 表单元素的处理程序之一。例如，按钮将提供一个 onClick 事件处理程序来对单击事件作出反应。*

So far, we have used an arrow function to inline the event handler for out input field. What about extracting it as standalone function inside the component? It would become a named function then:

到目前为止，我们已经使用了一个箭头函数来内联 input field 的事件处理程序，并影响输出。将它作为独立的函数提取到组件中呢？它将成为一个具名函数：

```js
import React, { useState } from 'react';

const App = () => {
  return <Headline />;
};

const Headline = () => {
  const [greeting, setGreeting] = useState(
    'Hello Function Component!'
  );

  const handleChange = event => setGreeting(event.target.value);

  return (
    <div>
      <h1>{greeting}</h1>

      <input type="text" value={greeting} onChange={handleChange} />
    </div>
  );
};

export default App;
```

We have used an arrow function to define the function within the component. If you have used class methods in React Class Components before, this way of defining functions inside a React Function Component is the equivalent. You could call it the "React Function Component Methods"-equivalent to class components. You can create or add as many functions inside the Functional Component as you want to act as explicit event handlers or to encapsulate other business logic.

我们使用了一个箭头函数来定义组件中的函数。如果你以前在 React 类组件中使用过类方法，那么在 React 函数组件中定义函数的这种方法是等效的。你可以称之为「React 函数组件方法」（相当于类组件）。你可以在函数组件中创建或添加尽可能多的函数，以充当显式事件处理程序或封装其他业务逻辑。

## React Function Component: Callback Function（React 函数组件之：回调函数）

Everything happens in our Child Function Component. There are no props passed to it, even though you have seen before how a string variable for the greeting can be passed from the Parent Component to the Child Component. Is it possible to pass a function to a component as prop as well? Somehow it must be possible to call a component function from the outside! Let's see how this works:

一切都在我们的子函数组件中发生。没有向它传递任何 props，即使你以前已经看到了如何将用于 greeting 的字符串变量从父组件传递到子组件。有可能将一个函数作为 props 传递给一个组件吗？无论如何，必须能够从外部调用组件函数！让我们看看它是如何工作的：

```js
import React, { useState } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState(
    'Hello Function Component!'
  );

  const handleChange = event => setGreeting(event.target.value);

  return (
    <Headline headline={greeting} onChangeHeadline={handleChange} />
  );
};

const Headline = ({ headline, onChangeHeadline }) => (
  <div>
    <h1>{headline}</h1>

    <input type="text" value={headline} onChange={onChangeHeadline} />
  </div>
);

export default App;
```

That's all to it. You can pass a function to a Child Component and handle what happens up in the Parent Component. You could also execute something in between in the Child Component (Headline component) for the `onChangeHeadline` function -- like trimming the value -- to add extra functionality inside the Child Component. That's how you would be able to call a Child Component's function from a Parent Component.

仅此而已。可以将函数传递给子组件并处理父组件中发生的事情。你还可以在子组件（Headline 组件）中为 `onChangeHeadline` 函数执行一些操作（比如调整值），以在子组件中添加额外的功能。这就是如何从父组件调用子组件的函数。

Let's take this example one step further by introducing a Sibling Component for the Headline component. It could be an abstract Input component:

让我们通过为 Headline 组件引入一个 Sibling 组件来进一步研究这个示例。它可以是一个抽象的 Input 组件：

```js
import React, { useState } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState(
    'Hello Function Component!'
  );

  const handleChange = event => setGreeting(event.target.value);

  return (
    <div>
      <Headline headline={greeting} />

      <Input value={greeting} onChangeInput={handleChange}>
        Set Greeting:
      </Input>
    </div>
  );
};

const Headline = ({ headline }) => <h1>{headline}</h1>;

const Input = ({ value, onChangeInput, children }) => (
  <label>
    {children}
    <input type="text" value={value} onChange={onChangeInput} />
  </label>
);

export default App;
```

I find this is a perfect yet minimal example to illustrate how to pass functions between components as props; and more importantly how to share a function between components. You have one Parent Component which manages the logic and two Child Components -- which are siblings -- that receive props. These props can always include a callback function to call a function in another component. Basically that's how it's possible to call a function in different components in React.

我发现这是一个完美而又简单的例子，它演示了如何在组件之间传递函数；更重要的是如何在组件之间共享一个函数。你有一个管理逻辑的父组件和两个接收 props 的子组件（它们是兄弟组件）。这些支持总是可以包含一个回调函数来调用另一个组件中的函数。基本上这就是在 React 中调用不同组件中的函数的方法。

### Override Component Function with React（覆盖 React 组件）

It's shouldn't happen often, but I have heard people asking me this question. How would you override a component's function? You need to take the same approach as for overriding any other passed prop to a component by giving it a default value:

这种情况不应该经常发生，但是有人问我这个问题。如何覆盖组件的函数？你需要采取相同的方法覆盖任何其他通过的 props 组件给它一个默认值：

```js
import React from 'react';

const App = () => {
  const sayHello = () => console.log('Hello');

  return <Button handleClick={sayHello} />;
};

const Button = ({ handleClick }) => {
  const sayDefault = () => console.log('Default');

  const onClick = handleClick || sayDefault;

  return (
    <button type="button" onClick={onClick}>
      Button
    </button>
  );
};

export default App;
```

You can assign the default value in the function signature for the destructuring as well:

你也可以在函数签名中为解构指定默认值：

```js
import React from 'react';

const App = () => {
  const sayHello = () => console.log('Hello');

  return <Button handleClick={sayHello} />;
};

const Button = ({ handleClick = () => console.log('Default') }) => (
  <button type="button" onClick={handleClick}>
    Button
  </button>
);

export default App;
```

You can also give a React Function Component default props -- which is another alternative:

你也可以给一个 React 函数组件默认的 props（这是另一种选择）：

```js
import React from 'react';

const App = () => {
  const sayHello = () => console.log('Hello');

  return <Button handleClick={sayHello} />;
};

const Button = ({ handleClick }) => (
  <button type="button" onClick={handleClick}>
    Button
  </button>
);

Button.defaultProps = {
  handleClick: () => console.log('Default'),
};

export default App;
```

All of these approaches can be used to define default props (in this case a default function), to be able to override it later from the outside by passing an explicit prop (e.g. function) to the component.

所有这些方法都可以用来定义默认的 props（在本例中是一个默认的函数），以便以后通过向组件传递一个显式的 props（例如函数）来从外部覆盖它。

### Async Function in Component with React（在 React 组件的异步函数）

Another special case may be an async function in a React component. But there is nothing special about it, because it doesn't matter if the function is asynchronously executed or not:

另一种特殊情况可能是 React 组件中的异步函数。但是它没有什么特别之处，因为它与函数是否异步执行无关：

```js
import React from 'react';

const App = () => {
  const sayHello = () =>
    setTimeout(() => console.log('Hello'), 1000);

  return <Button handleClick={sayHello} />;
};

const Button = ({ handleClick }) => (
  <button type="button" onClick={handleClick}>
    Button
  </button>
);

export default App;
```

The function executes delayed without any further instructions from your side within the component. The component will also rerender asynchronously in case props or state have changed. Take the following code as example to see how we set state with a artificial delay by using `setTimeout`:

该函数延迟执行，没有来自组件内部的任何进一步指令。如果 props 或状态发生了更改，组件还将异步地重新运行。以下面的代码为例，看看我们如何使用 `setTimeout` 设置一个人为的延迟状态：

```js
import React, { useState } from 'react';

const App = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = () =>
    setTimeout(
      () => setCount(currentCount => currentCount + 1),
      1000
    );

  const handleDecrement = () =>
    setTimeout(
      () => setCount(currentCount => currentCount - 1),
      1000
    );

  return (
    <div>
      <h1>{count}</h1>

      <Button handleClick={handleIncrement}>Increment</Button>
      <Button handleClick={handleDecrement}>Decrement</Button>
    </div>
  );
};

const Button = ({ handleClick, children }) => (
  <button type="button" onClick={handleClick}>
    {children}
  </button>
);

export default App;
```

Also note that we are using a callback function within the `setCount` state function to access the current state. Since setter functions from `useState` are executed asynchronously by nature, you want to make sure to perform your state change on the current state and not on any stale state.

还要注意，我们在 `setCount` 状态函数中使用了一个回调函数来访问当前状态。由于 `useState` 中的 setter 函数本质上是异步执行的，因此你需要确保对当前状态执行状态更改，而不是对任何陈旧状态执行状态更改。

*Experiment: If you wouldn't use the callback function within the State Hook, but rather act upon the count variable directly (e.g. `setCount(count + 1)`), you wouldn't be able to increase the value from 0 to 2 with a quick double click, because both times the function would be executed on a count state of 0.*

*实验：如果你不使用 State Hook 中的回调函数，而是直接作用于 count 变量（例如 `setCount(count + 1)`），你无法通过快速双击将值从 0 增加到 2，因为两次都将在 count 状态为 0 时执行函数。*

Read more about [how to fetch data with Function Components with React Hooks](https://www.robinwieruch.de/react-hooks-fetch-data/).

阅读更多：[如何使用 React 钩子的函数组件获取数据（暂缺译文）]()

## React Function Component: Lifecycle（React 函数组件之生命周期）

If you have used React Class Components before, you may be used to lifecycle methods such as componentDidMount, componentWillUnmount and shouldComponentUpdate. You don't have these in Function Components, so let's see how you can implement them instead.

如果你以前使用过 React 类组件，那么你可能已经熟悉了诸如 componentDidMount、componentWillUnmount 和 shouldComponentUpdate 之类的生命周期方法。在函数组件中没有这些，所以让我们看看如何实现它们。

First of all, you have no constructor in a Function Component. Usually the constructor would have been used in a React Class Component to allocate initial state. As you have seen, you don't need it in a Function Component, because you allocate initial state with the useState hook and set up functions within the Function Component for further business logic:

首先，函数组件中没有构造函数。通常在 React 类组件中使用构造函数来分配初始状态。正如你所看到的，在函数组件中不需要它，是因为使用 useState 钩子分配初始状态，并在函数组件中为进一步的业务逻辑设置函数：

```js
import React, { useState } from 'react';

const App = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  return (
    <div>
      <h1>{count}</h1>

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

export default App;
```

### React Functional Component: Mount（React 函数组件之挂载）

Second, there is the mounting lifecycle for React components when they are rendered for the first time. If you want to execute something when a React Function Component **did mount**, you can use the useEffect hook:

其次，在首次渲染 React 组件时，有一个挂载生命周期。如果你想在一个 React 函数组件 **挂载** 时做些事情，你可以使用 useEffect 钩子：

```js
import React, { useState, useEffect } from 'react';

const App = () => {
  const [count, setCount] = useState(0);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  useEffect(() => setCount(currentCount => currentCount + 1), []);

  return (
    <div>
      <h1>{count}</h1>

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

export default App;
```

If you try out this example, you will see the count 0 and 1 shortly displayed after each other. The first render of the component shows the count of 0 from the initial state -- whereas after the component did mount actually, the Effect Hook will run to set a new count state of 1.

如果你尝试使用这个示例，你将看到 0 和 1 分别显示在一起。组件的第一次渲染显示了从初始状态开始的 0 count（然而，在组件实际挂载之后，Effect Hook 将运行以设置新的 count 状态为1）。

It's important to note the empty array as second argument for the Effect Hook which makes sure to trigger the effect only on component load (mount) and component unload (unmount).

一定要注意，空数组是 Effect 钩子的第二个参数，它确保只在组件加载（mount）和组件卸载（unmount）时触发效果。

*Experiment: If you would leave the second argument of the Effect Hook empty, you would run into an infinite loop of increasing the count by 1, because the Effect Hook always runs after state has changed. Since the Effect Hook triggers another state change, it will run again and again to increase the count.*

*实验：如果你让 Effect 钩子的第二个参数为空，你会进入一个无限循环，计数增加 1，因为 Effect 钩子总是在状态改变后运行。因为 Effect Hook 会触发另一个状态改变，所以它会一次又一次的运行来增加计数。*

### React Functional Component: Update（React 函数组件之：更新）

Every time incoming props or state of the component change, the component triggers a rerender to display the latest status quo which is often derived from the props and state. A render executes everything within the Function Component's body.

每当输入的 props 或组件的状态更改时，组件将触发一个重新运行程序来显示最新的状态，这些状态通常来自 props 和状态。渲染执行函数组件主体内的所有内容。

*Note: In case a Function Component is not updating properly in your application, it's always a good first debugging attempt to console log state and props of the component. If both don't change, there is no new render executed, and hence you don't see a console log of the output in the first place.*

*注意：如果在你的应用程序中某个函数组件没有正确地更新，那么首先尝试调试以控制该组件的日志状态和 props 总是好的。如果两者都没有改变，就不会执行新的渲染，因此你不会首先看到输出的控制台日志。*

```js
import React, { useState, useEffect } from 'react';

const App = () => {
  console.log('Does it render?');

  const [count, setCount] = useState(0);

  console.log(`My count is ${count}!`);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  return (
    <div>
      <h1>{count}</h1>

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

export default App;
```

If you want to act upon a rerender, you can use the Effect Hook again to do something after the component did update:

如果你想在渲染时动作，你可以使用 Effect 钩子在组件更新后再次做一些事情：

```js
import React, { useState, useEffect } from 'react';

const App = () => {
  const initialCount = +localStorage.getItem('storageCount') || 0;
  const [count, setCount] = useState(initialCount);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  useEffect(() => localStorage.setItem('storageCount', count));

  return (
    <div>
      <h1>{count}</h1>

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

export default App;
```

Now every time the Function Component rerenders, the count is stored into the browser's local storage. Every time you fresh the browser page, the count from the browser's local storage, in case there is a count in the storage, is set as initial state.

现在，每次函数组件重新渲染时，count 都存储在浏览器的本地存储中。每次刷新浏览器页面时，来自浏览器本地存储的 count（如果存储中有）都被设置为初始状态。

You can also specify when the Effect Hook should run depending on the variables you pass into the array as second argument. Then every time one of the variables change, the Effect Hook runs. In this case it makes sense to store the count only if the count has changed:

还可以根据传递给数组的第二个参数变量来确定何时运行 Effect Hook。然后，每当变量发生变化时，Effect Hook 就会运行。在这种情况下，只有在 count 发生变化时才有必要存储计数：

```js
import React, { useState, useEffect } from 'react';

const App = () => {
  const initialCount = +localStorage.getItem('storageCount') || 0;
  const [count, setCount] = useState(initialCount);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  useEffect(() => localStorage.setItem('storageCount', count), [
    count,
  ]);

  return (
    <div>
      <h1>{count}</h1>

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

export default App;
```

By using the second argument of the [Effect Hook with care](https://reactjs.org/docs/hooks-effect.html), you can decide whether it runs:

[小心使用 Effect Hook（暂缺译文）]() 的第二个参数，你可以决定它是否运行：

- every time (no argument)

每一次（无参数）

- only on mount and unmount (`[]` argument)

只在挂载和非挂载时（`[]` 参数）

- only when a certain variable changes (e.g. `[count]` argument)

只有当某个变量改变时（例如 `[count]`）

*Note: A React Function Component force update can be done by using this [neat trick](https://reactjs.org/docs/hooks-faq.html#is-there-something-like-forceupdate). However, you should be careful when applying this pattern, because maybe you can solve the problem a different way.*

使用这个 [精妙的技巧（暂缺译文）]() 可以完成一个 React 函数组件的强制更新。但是，在应用此模式时应该小心，毕竟还可以用其他途径解决问题。

## Pure React Function Component（纯 React 函数组件）

React Class Components offered the possibility to decide whether a component has to rerender or not. It was achieved by using the PureComponent or shouldComponentUpdate to [avoid performance bottlenecks in React by preventing rerenders](https://www.robinwieruch.de/react-prevent-rerender-component/). Let's take the following extended example:

React 类组件提供了一种可能性，可以决定组件是否必须重新渲染。它是通过使用 PureComponent 或 shouldComponentUpdate 来 [避免 React 中的性能瓶颈（暂缺译文）]()，从而防止渲染。让我们举一个扩展的例子：

```js
import React, { useState } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState('Hello React!');
  const [count, setCount] = useState(0);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  const handleChange = event => setGreeting(event.target.value);

  return (
    <div>
      <input type="text" onChange={handleChange} />

      <Count count={count} />

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

const Count = ({ count }) => {
  console.log('Does it (re)render?');

  return <h1>{count}</h1>;
};

export default App;
```

In this case, every time you type something in the input field, the App component updates its state, rerenders, and rerenders the Count component as well. React memo -- which is one of  [React's top level APIs](https://reactjs.org/docs/react-api.html) -- can be used for React Function Components to prevent a rerender when the incoming props of this component haven't changed:

在这种情况下，每当你在 input field 中键入一些内容时，App 组件都会更新其状态、重新渲染以及重新渲染 Count 组件。React memo（[React 的顶级 API 之一（暂缺译文）]()）可用于 React 函数组件，以防止在该组件的传入 props 未更改时重新渲染：

```js
import React, { useState, memo } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState('Hello React!');
  const [count, setCount] = useState(0);

  const handleIncrement = () =>
    setCount(currentCount => currentCount + 1);

  const handleDecrement = () =>
    setCount(currentCount => currentCount - 1);

  const handleChange = event => setGreeting(event.target.value);

  return (
    <div>
      <input type="text" onChange={handleChange} />

      <Count count={count} />

      <button type="button" onClick={handleIncrement}>
        Increment
      </button>
      <button type="button" onClick={handleDecrement}>
        Decrement
      </button>
    </div>
  );
};

const Count = memo(({ count }) => {
  console.log('Does it (re)render?');

  return <h1>{count}</h1>;
});

export default App;
```

Now, the Count component doesn't update anymore when the user types something into the input field. Only the App component rerenders. This performance optimization shouldn't be used as default though. I would recommend to check it out when you run into issues when the rerendering of components takes too long (e.g. rendering and updating a large list of items in a Table component).

现在，当用户在 input field 中输入内容时，Count 组件不再更新。只有 App 组件重新渲染。不过，这种性能优化不应该当为默认行为。我的建议是，在重新渲染组件的时间太长（例如，在一个 Table 组件中渲染和更新一个很大的项目列表）而遇到问题时再进行。

## React Function Component: Export and Import（React 函数组件之：Export 和 Import）

Eventually you will separate components into their own files. Since React Components are functions (or classes), you can use the standard [import](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/import) and [export](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) statements provided by JavaScript. For instance, you can define and export a component in one file:

最终你会将组件各自分离到独立的文件中。因为 React 组件是函数（或类），所以可以使用 JavaScript 提供的标准 [import（暂缺译文）]() 和 [export（暂缺译文）]() 语句。例如，你可以在一个文件中定义和导出一个组件：

```js
// src/components/Headline.js

import React from 'react';

const Headline = (props) => {
  return <h1>{props.value}</h1>;
};

export default Headline;
```

And import it in another file:

在另一个文件中将其导入：

```js
// src/components/App.js

import React from 'react';

import Headline from './Headline.js';

const App = () => {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
};

export default App;
```

*Note: If a Function Component is not defined, console log your exports and imports to get a better understanding of where you made a mistake. Maybe you used a named export and expected it to be a default export.*

*注意：如果没有定义函数组件，控制台将记录你的导出和导入，以便更好地了解在哪里犯了错误。可能使用了一个具名导出，并错误认为它是一个默认导出。*

If you don't care about the component name by defining the variable, you can keep it as Anonymous Function Component when using a default export on the Function Component:

如果你不介意组件名称由变量定义，你可以让它作为匿名函数组件，此时使用默认导出函数组件：

```js
import React from 'react';

import Headline from './Headline.js';

export default () => {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
};
```

However, when doing it this way, React Dev Tools cannot identify the component because it has no display name. You may see an Unknown Component in your browser's developer tools.

但是，当这样做时，React Dev 工具无法识别组件，因为它没有显示名称。可能会在浏览器的开发工具中看到一个未知的组件。

## React Function Component: ref（React 函数组件之：ref）

A React Ref should only be used in rare cases such as accessing/manipulating the DOM manually (e.g. focus element), animations, and integrating third-party DOM libraries (e.g. D3). If you have to use a Ref in a Function Component, you can define it within the component. In the following case, the input field will get focused after the component did mount:

React Ref 应该只在极少数情况下使用，比如手动访问或操作 DOM（比如 focus 元素）、动画以及集成第三方 DOM 库（比如 D3）。如果必须在函数组件中使用 Ref，则可以在组件中定义它。在以下情况下，组件挂载完成后，input field 将被聚焦：

```js
import React, { useState, useEffect, useRef } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState('Hello React!');

  const handleChange = event => setGreeting(event.target.value);

  return (
    <div>
      <h1>{greeting}</h1>

      <Input value={greeting} handleChange={handleChange} />
    </div>
  );
};

const Input = ({ value, handleChange }) => {
  const ref = useRef();

  useEffect(() => ref.current.focus(), []);

  return (
    <input
      type="text"
      value={value}
      onChange={handleChange}
      ref={ref}
    />
  );
};

export default App;
```

However, React Function Components cannot be given refs! If you try the following, the ref will be assigned to the component instance but not to the actual DOM node.

但是，React 函数组件不能提供 ref！如果你尝试以下操作，ref 被分配给组件实例，而不是实际的 DOM 节点。

```js
// Doesn't work!

import React, { useState, useEffect, useRef } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState('Hello React!');

  const handleChange = event => setGreeting(event.target.value);

  const ref = useRef();

  useEffect(() => ref.current.focus(), []);

  return (
    <div>
      <h1>{greeting}</h1>

      <Input value={greeting} handleChange={handleChange} ref={ref} />
    </div>
  );
};

const Input = ({ value, handleChange, ref }) => (
  <input
    type="text"
    value={value}
    onChange={handleChange}
    ref={ref}
  />
);

export default App;
```

It's not recommended to pass a ref from a Parent Component to a Child Component and that's why the assumption has always been: React Function Components cannot have refs. However, if you need to pass a ref to a Function Component -- because you have to measure the size of a function component's DOM node, for example, or like in this case to focus an input field from the outside -- you can [forward the ref](https://reactjs.org/docs/forwarding-refs.html):

不建议将 ref 从父组件传递给子组件，这就是为什么我们总是假设：React 函数组件不能有 ref。但是，如果你需要将一个 ref 传递给一个函数组件（例如，因为你必须测量函数组件的 DOM 节点的大小，或者像本例中那样从外部聚焦一个 input field），那么你可以 [转发 ref（暂缺译文）]()。

```js
// Does work!

import React, {
  useState,
  useEffect,
  useRef,
  forwardRef,
} from 'react';

const App = () => {
  const [greeting, setGreeting] = useState('Hello React!');

  const handleChange = event => setGreeting(event.target.value);

  const ref = useRef();

  useEffect(() => ref.current.focus(), []);

  return (
    <div>
      <h1>{greeting}</h1>

      <Input value={greeting} handleChange={handleChange} ref={ref} />
    </div>
  );
};

const Input = forwardRef(({ value, handleChange }, ref) => (
  <input
    type="text"
    value={value}
    onChange={handleChange}
    ref={ref}
  />
));

export default App;
```

There are a few other things you may want to know about React Refs, so check out this article: [How to use Ref in React](https://www.robinwieruch.de/react-ref-attribute-dom-node/) or the [official React documentation](https://reactjs.org/docs/refs-and-the-dom.html).

关于 React Ref，你可能还想了解其他一些事情，所以请阅读：如何在 [React 中使用 Ref（暂缺译文）]() 或 [官方 React 文档](https://reactjs.org/docs/refs-and-the-dom.html)。

## React Function Component: PropTypes（React 函数组件之：PropTypes）

PropTypes can be used for React Class Components and Function Components the same way. Once you have defined your component, you can assign it PropTypes to validate the incoming props of a component:

PropTypes 可以以相同的方式用于 React 类组件和函数组件。一旦你定义了你的组件，你可以分配它 PropTypes 来验证组件的输入 props：

```js
import React from 'react';
import PropTypes from 'prop-types';

const App = () => {
  const greeting = 'Hello Function Component!';

  return <Headline value={greeting} />;
};

const Headline = ({ value }) => {
  return <h1>{value}</h1>;
};

Headline.propTypes = {
  value: PropTypes.string.isRequired,
};

export default App;
```

*Note that you have to install the standalone [React prop-types](https://github.com/facebook/prop-types), because it has been removed from the React core library a while ago. If you want to learn more about PropTypes in React, check out the [official documentation](https://reactjs.org/docs/typechecking-with-proptypes.html).*

*请注意，你必须安装独立的 [React prop-types](https://github.com/facebook/prop-types)，因为它已经从 React 核心库中删除了一段时间了。如果你想了解更多关于 React 中 PropTypes 的信息，请查看 [官方文档](https://reactjs.org/docs/typechecking-with-proptypes.html)。*

In addition, previously you have seen the usage of default props for a Function Component. For the sake of completeness, this is another one:

此外，在前面你已经看到了函数组件默认 props 的使用。为了完整起见，这是另一个例子：

```js
import React from 'react';

const App = () => {
  const greeting = 'Hello Function Component!';

  return <Headline headline={greeting} />;
};

const Headline = ({ headline }) => {
  return <h1>{headline}</h1>;
};

Headline.defaultProps = {
  headline: 'Hello Component',
};

export default App;
```

*Note that you can also use the default assignment when destructuring the value from the props in the function signature (e.g. `const Headline = ({ headline = 'Hello Component' }) =>`) or the `||` operator within the Function Component's body (e.g. `return <h1>{headline || 'Hello Component'}</h1>;`).*

注意，你还可以在从函数签名中的 props 中解构值时使用默认赋值（例如 `const Headline = ({ headline = 'Hello Component' }) =>`）或者 `||` 运算符，在函数组件主体中（例如 `return <h1>{headline || 'Hello Component'}</h1>;`）。

However, if you really want to go all-in with strongly typed components in React, you have to check out TypeScript which is briefly shown in the next section.

## React Function Component: TypeScript（React 函数组件之：TypeScript）

If you are looking for a type system for your React application, you should give TypeScript for React Components a chance. A strongly typed language like TypeScript comes with many benefits for your developer experience ranging from IDE support to a more robust code base. You may wonder: How much different would a React Function Component with TypeScript be? Check out the following typed component:

如果你正在为你的 React 应用程序寻找类型系统，你应该给 React 组件的 TypeScript 一个机会。像 TypeScript 这样的强类型语言无论 IDE 支持还是更健壮的代码库，都会为你的开发体验带来很多好处。你可能想知道：React 函数组件与 TypeScript 有多大的不同？查看以下类型的组件：

```js
import React, { useState } from 'react';

const App = () => {
  const [greeting, setGreeting] = useState(
    'Hello Function Component!'
  );

  const handleChange = event => setGreeting(event.target.value);

  return (
    <Headline headline={greeting} onChangeHeadline={handleChange} />
  );
};

const Headline = ({
  headline,
  onChangeHeadline,
}: {
  headline: string,
  onChangeHeadline: Function,
}) => (
  <div>
    <h1>{headline}</h1>

    <input type="text" value={headline} onChange={onChangeHeadline} />
  </div>
);

export default App;
```

It only defines the incoming props as types. However, most of the time type inference just works out of the box. For instance, the use State Hook from the App component doesn't need to be typed, because from the initial value the types for `greeting` and `setGreeting` are inferred.

它只将传入的 props 定义为类型。然而，大多数时候类型推断都是开箱即用的。例如，不需要输入来自 App 组件的 useState Hook，因为从初始值可以推断出 `greeting` 和 `setGreeting` 的类型。

If you want to know how to get started with TypeScript in React, check out this [comprehensive cheatsheet ranging from TypeScript setup to TypeScript recipes](https://github.com/sw-yx/react-typescript-cheatsheet). It's well maintained and my go-to resource to learn more about it.

如果你想知道如何在 React 中使用 TypeScript，可以查看 [comprehensive cheatsheet ranging from TypeScript setup to TypeScript recipes](https://github.com/sw-yx/react-typescript-cheatsheet)。它得到了很好的维护，是我了解它的首选资源。

## React Function Component vs Class Component（React 的函数组件和类组件）

This section will not present you any performance benchmark for Class Components vs Functional Components, but a few words from my side about where React may go in the future.

本节不会向你展示任何类组件与函数组件的性能基准测试，但是我将简要介绍未来 React 的发展方向。

Since React Hooks have been introduced in React, Function Components are not anymore behind Class Components feature-wise. You can have state, side-effects and lifecycle methods in React Function Components now. That's why I strongly believe React will move more towards Functional Components, because they are more lightweight than Class Components and offer a sophisticated API for reusable yet encapsulated logic with React Hooks.

自从在 React 中引入了 React 钩子之后，函数组件的特性就不再落后于类组件了。你现在可以在 React 函数组件中使用状态、副作用和生命周期方法。这就是为什么我坚信 React 将更多地向函数组件发展，因为它们比类组件更轻量，并为使用 React 钩子的可重用但封装的逻辑提供了成熟的 API。

For the sake of comparison, check out the implementation of the following Class Component vs Functional Component:

为了比较，请查看以下类组件和函数组件的实现：

```js
// Class Component

class App extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      value: localStorage.getItem('myValueInLocalStorage') || '',
    };
  }

  componentDidUpdate() {
    localStorage.setItem('myValueInLocalStorage', this.state.value);
  }

  onChange = event => {
    this.setState({ value: event.target.value });
  };

  render() {
    return (
      <div>
        <h1>Hello React ES6 Class Component!</h1>

        <input
          value={this.state.value}
          type="text"
          onChange={this.onChange}
        />

        <p>{this.state.value}</p>
      </div>
    );
  }
}

// Function Component

const App = () => {
  const [value, setValue] = React.useState(
    localStorage.getItem('myValueInLocalStorage') || '',
  );

  React.useEffect(() => {
    localStorage.setItem('myValueInLocalStorage', value);
  }, [value]);

  const onChange = event => setValue(event.target.value);

  return (
    <div>
      <h1>Hello React Function Component!</h1>

      <input value={value} type="text" onChange={onChange} />

      <p>{value}</p>
    </div>
  );
};
```

If you are interested in moving from Class Components to Function Components, check out this guide: [A migration path from React Class Components to Function Components with React Hooks](https://www.robinwieruch.de/react-hooks-migration). However, there is no need to panic because you don't have to migrate all your React components now. Maybe it's a better idea to start implementing your future components as Function Components instead.

如果你对从类组件迁移到函数组件感兴趣，请查看以下指南：[使用 React 钩子将 React 类组件迁移到函数组件的路径（暂缺译文）]()。但是，没有必要盲目，因为你现在不必迁移所有的 React 组件。也许最好开始将未来的组件实现为函数组件。

The article has shown you almost everything you need to know to get started with React Function Components. If you want to dig deeper into testing React Components for instance, check out this in-depth guide: [Testing React Components](https://www.robinwieruch.de/react-testing-tutorial). Anyway, I hope there have been a couple of best practices for using Functional Components in React as well. Let me know if anything is missing!

本文向你展示了开始使用 React 函数组件所需的几乎所有知识。例如，如果你想更深入地研究 testing React 组件，请查看以下深入指南：[Testing React Components（暂缺译文）]()。无论如何，我希望已经有了一些在 React 中使用函数组件的最佳实践。如果有什么遗漏，请告诉我！
