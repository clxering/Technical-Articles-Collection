# What are Controlled Components in React?（什么是 React 的受控组件）

> 转译自：https://www.robinwieruch.de/react-controlled-components

There are quite a lot of articles about React out there speaking about controlled and uncontrolled components without explaining them. It has been quite similar for my articles, whereas I always tried to add at least one or two sentences explaining them, but in the end, I thought it would be great to have a brief tutorial just showing a simple example for controlled components in React.

有很多关于 React 的文章讨论受控和非受控组件，却没有解释它们。对于我的文章来说，情况非常类似，不过我总是试图添加至少一两句话来解释，而且在最后，我认为最好有一个简短的教程来展示 React 中的受控组件的简单示例。

Let's take the following input field element which is rendered within our [function component](https://www.robinwieruch.de/react-function-component/). Even though the input field is the uncontrolled input element here, we are often referring to the enclosing App component being the uncontrolled component:

让我们使用下面的 input field 元素，它是在我们的 [函数组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Function-Components.md) 中呈现的。虽然这里的 input field 是非受控的 input 元素，但我们通常说封装它的 App 组件是非受控组件：

```js
import React from "react";

const App = () => (
  <div>
    <label>
      My uncontrolled Input: <input type="text" />
    </label>
  </div>
);

export default App;
```

_Note: It doesn't matter for controlled or uncontrolled elements whether the component itself is a [function or class component](https://www.robinwieruch.de/react-component-types/). Uncontrolled elements -- such as text inputs, checkboxes, radio buttons, and entire forms with inputs -- can always be uncontrolled or controlled._

_注意：对于受控或非受控元素，组件本身是 [函数还是类组件（暂缺译文）]() 并不重要。不受控的元素，例如文本输入、复选框、单选按钮和带有 input 的整个表单，总是可以不受控或受控。_

It's an uncontrolled input field, because once you start the application, you can type something into the field and see changes even though we are not giving any instructions in our source code. There is no line written to display the value in the input field and no line written to change the value when we type something into it. After all, that's because we deal with HTML here and it's the native behavior of the input field, because it manages its own internal state.

这是一个不受控的 input field，因为一旦启动应用程序，可以在其中键入一些内容并查看更改，即使我们在源代码中没有给出任何指令。在 input field 中没有用于显示值的 line written，也没有用于在输入内容时更改值的 line written。毕竟，这是因为我们在这里处理 HTML，而它是 input field 的本机行为，因为它管理自己的内部状态。

## Uncontrolled vs. Controlled Component（非受控和受控组件）

Let's see another case where it isn't clear whether we are dealing with an uncontrolled or controlled component. The next example adds [state management](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext) with [React Hooks](https://www.robinwieruch.de/react-hooks/) to our function component:

让我们来看另一个例子，在这个例子中，我们不清楚是在处理非受控组件还是受控组件。该例子添加了 [状态管理](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-State-Hooks-useReducer-useState-useContext.md) 和 [React 钩子](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/What-are-React-Hooks.md) 到函数组件中：

```js
import React, { useState } from "react";

const App = () => {
  const [value, setValue] = useState("");

  const handleChange = event => setValue(event.target.value);

  return (
    <div>
      <label>
        My still uncontrolled Input:
        <input type="text" onChange={handleChange} />
      </label>

      <p>
        <strong>Output:</strong> {value}
      </p>
    </div>
  );
};

export default App;
```

We also show the current value as output. Ask yourself: Why is this component (element) still uncontrolled? When you start the application, the input field shows the same value as the output paragraph. That should be alright, shouldn't it? Let's see why it isn't. Try the following initial state instead:

我们还将当前值在 output paragraph 显示。问问你自己：为什么这个组件（元素）仍然没有受控？启动应用程序时，input field 显示与 output paragraph 有相同的值。应该没问题，不是吗？让我们看看问题在哪。尝试添加以下初始状态：

```js
import React, { useState } from "react";

const App = () => {
  // 添加初始状态'Hello React'
  const [value, setValue] = useState("Hello React");

  const handleChange = event => setValue(event.target.value);

  return (
    <div>
      <label>
        My still uncontrolled Input:
        <input type="text" onChange={handleChange} />
      </label>

      <p>
        <strong>Output:</strong> {value}
      </p>
    </div>
  );
};

export default App;
```

Now you can see the difference. While the input field shows an empty field, the output paragraph shows the initial state. Only when you start typing into the input field, both elements _seem_ to synchronize, but they don't, because the input field still tracks its own internal state while the output paragraph is driven by the actual React state coming from the handler function. So even though they output the same when you start typing, the underlying source of the value is different:

现在你可以看到区别了。input field 显示一个空字段，而 output paragraph 显示初始状态。只有当你开始在 input field 输入时，两个元素似乎才会同步，但它们并不会同步，因为 input field 仍然跟踪它自己的内部状态，而 output paragraph 则由处理程序函数的实际 React 状态驱动。因此，即使你开始输入时它们的显示是一样的，但显示值的底层来源是不同的：

- input field receives its value from internal DOM node state

input field 从内部 DOM 节点状态接收其值

- output paragraph receives its value from React's state

output paragraph 从 React 的状态接收其值

Having an uncontrolled element/component in your React application can lead to unwanted behavior and therefore bugs. You want to drive your UI from one source of truth instead; which in React should be props and state. Given the same props and state to a component, it should always render the same output: `(props, state) => view`.

在 React 应用程序中有一个非受控的元素或组件可能会导致不必要的行为，从而导致 bug。你想把你的 UI 从一个真实的来源；在 React 中应该是属性和状态。给定一个组件相同的属性和状态，它应该总是呈现相同的输出：`(props, state) => view`

## From Uncontrolled to Controlled Component（从非受控组件到受控组件）

You can change the input from uncontrolled to controlled by controlling its value yourself. For instance, in this case the input field offers a value attribute:

可以通过控制自身的值来将 input 从非受控更改为受控。例如，在这种情况下，input field 提供了一个 value 属性：

```js
import React, { useState } from "react";

const App = () => {
  const [value, setValue] = useState("Hello React");

  const handleChange = event => setValue(event.target.value);

  return (
    <div>
      <label>
        My controlled Input:
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

By giving the input the value from React's state, it doesn't use anymore its internal state, but the state you provided from React. Now the initial state should be seen for the input field and for the output paragraph once you start the application. Also when you type something in the input field, both input field and output paragraph are synchronized by React's state. The input field has become a controlled element and the App component a controlled component. You are in charge what is displayed in your UI. You can see different input elements implemented as controlled components in this [GitHub repository](https://github.com/the-road-to-learn-react/react-controlled-components-examples).

传给 input 的 value 来自 React 的状态，它不再使用自己的内部状态，而是使用你在 React 中提供的状态。现在，启动应用程序后，应该可以看到 input field 和 output paragraph 的初始状态。另外，当你在 input field 中键入某些内容时，input field 和 output paragraph 都通过 React 的状态进行同步。input field 变成了受控元素，App 组件变成了受控组件。你负责 UI 中显示的内容。你可以在这个 [GitHub 存储库](https://github.com/the-road-to-learn-react/react-controlled-components-examples) 中看到作为受控组件实现的不同 input 元素。
