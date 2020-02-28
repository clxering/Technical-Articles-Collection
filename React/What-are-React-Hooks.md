# What are React Hooks?（React 钩子是什么？）

> 转译自：https://www.robinwieruch.de/react-hooks

React Hooks were introduced at [React Conf October 2018](https://www.youtube.com/watch?v=dpw9EHDh2bM) as a way to use state and side-effects in React function components. Whereas function components have been called functional stateless components (FSC) before, they are finally able to use state with React Hooks. Therefore, many people refer to them as function components now.

React 钩子在 [React Conf October 2018](https://www.youtube.com/watch?v=dpw9EHDh2bM) 发布，作为在 React 函数组件中使用状态和副作用的一种方式。虽然函数组件以前被称为函数无状态组件（FSC），但是它们最终能够与 React 钩子结合使用状态。Therefore, many people refer to them as function components now.

In this walkthrough, I want to explain the motivation behind hooks, what will change in React, why we shouldn't panic, and how React hooks can be used in function components by showcasing common React Hooks such as the state and effect hooks by example. This tutorial is only an introduction to React Hooks. At the end of this tutorial, you will find many more tutorials to learn about React Hooks in depth.

在本演示中，我将通过示例展示常见的 React 钩子（如 state 钩子和 effect 钩子），解释 React 钩子背后的动机、对 React 的影响、为什么我们不应该恐慌、以及如何在函数组件中使用钩子。本教程只是对 React 钩子的介绍。在本教程的最后，你将找到更多的教程来深入了解 React 钩子。

## Why React Hooks?

React Hooks were invented by the React team to introduce state management and side-effects in [function components](https://www.robinwieruch.de/react-function-component/). It's their way of making it more effortless to use only React function components without the need to refactor a React function component to a React class component for using lifecycle methods, in order to use have side-effects, or local state. React Hooks enable us to write React applications with only function components.

React 钩子是由 React 团队发明的，用于在 [函数组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Function-Components.md) 中引入状态管理和副作用。这是一种更轻松的方式，只使用 React 函数组件，而不需要将 React 函数组件重构为 React 类组件来使用生命周期方法，以便使用具有副作用或局部状态的方法。React 钩子让我们只使用函数组件就能编写 React 应用程序。

**Unnecessary Component Refactorings:** Previously, only React class components were used for local state management and lifecycle methods. The latter have been essential for introducing side-effects, such as listeners or data fetching, in React class components.

**不必重构组件：** 以前，只有 React 类组件能用于本地状态管理，拥有生命周期方法。后者对于在 React 类组件中引入副作用（如监听器或数据获取）至关重要。

```js
import React from "react";

class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}

export default Counter;
```

Only if you didn't need state or lifecycle methods, React functional _stateless_ components could be used. And because React function components are more lightweight (and elegant), people already used plenty of function components. This came with the drawback of refactoring components from React function components to React class components every time state or lifecycle methods were needed (and vice versa).

只有在不需要状态或生命周期方法的情况下，才可以使用 React 函数（无状态）组件。而且因为 React 函数组件更轻量（也更优雅），人们已经使用了大量的函数组件。这样做的缺点是，每当需要状态或生命周期方法时，都要将组件从 React 函数组件重构为 React 类组件（反之亦然）。

```js
import React, { useState } from "react";

// how to use the state hook in a React function component
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}

export default Counter;
```

With Hooks there is no need for this refactoring. Side-effects and state are finally available in React function components. That's why a rebranding from functional stateless components to function components would be reasonable.

有了钩子，就不需要进行这种重构了。在 React 函数组件中终于可以看到副作用和状态。这就是为什么将函数（无状态）组件重新命名为函数组件是合理的。

**Side-effect Logic:** In React class components, side-effects were mostly introduced in lifecycle methods (e.g. componentDidMount, componentDidUpdate, componentWillUnmount). A side-effect could be [fetching data in React](https://www.robinwieruch.de/react-fetching-data/) or [interacting with the Browser API](https://www.robinwieruch.de/react-intersection-observer-api/). Usually these side-effects came with a setup and clean up phase. For instance, if you would miss to remove your listener, you could run into [React performance issues](https://www.robinwieruch.de/react-warning-cant-call-setstate-on-an-unmounted-component/).

**副作用逻辑:** 在 React 类组件中，副作用主要是在生命周期方法中引入的（如 componentDidMount、componentDidUpdate、componentWillUnmount）。副作用可能是 [在 React 中获取数据（暂缺译文）]() 或 [与浏览器 API 交互（暂缺译文）]()。通常这些副作用伴随着设置和清理阶段。例如，如果你没有删除监听器，可能会遇到 [React 性能问题（暂缺译文）]()。

```js
// side-effects in a React class component
class MyComponent extends Component {
  // setup phase
  componentDidMount() {
    // add listener for feature 1
    // add listener for feature 2
  }

  // clean up phase
  componentWillUnmount() {
    // remove listener for feature 1
    // remove listener for feature 2
  }

  ...
}

// side-effects in React function component with React Hooks
function MyComponent() {
  useEffect(() => {
    // add listener for feature 1 (setup)
    // return function to remove listener for feature 1 (clean up)
  });

  useEffect(() => {
    // add listener for feature 2 (setup)
    // return function to remove listener for feature 2 (clean up)
  });

  ...
}
```

Now, if you would introduce more than one of these side-effects in a React class component's lifecycle methods, all side-effects would be grouped by lifecycle method but not by side-effect. That's what React Hooks are going to change by encapsulating a side-effect in one hook whereas every hook has its own side-effect with a setup and clean up phase. You will see later in a tutorial how this works for real by adding and removing listeners in a React Hook.

现在，如果你想在一个 React 类组件的生命周期方法中引入多个这样的副作用，那么所有的副作用都将按照生命周期方法分组，而不是按照副作用分组。这就是 React Hooks 要改变的地方，它将副作用封装在一个 Hooks 中，而每个 Hooks 在设置和清理阶段都有自己的副作用。在稍后的教程中，你将看到如何在 React 钩子中添加和删除监听器。

**React's Abstraction Hell:** Abstraction and thus reusability were introduced with [Higher-Order Components](https://www.robinwieruch.de/react-higher-order-components/) and [Render Prop Components](https://www.robinwieruch.de/react-render-props/) in React. There is also [React's Context with its Provider and Consumer Components](https://www.robinwieruch.de/react-context/) that introduce another level of abstraction. All of these advanced patterns in React are using so called wrapping components. The implementation of the following components shouldn't be foreign to developers who are creating larger React applications.

**React 的抽象地狱:** 在 React 中引入了 [高阶组件（暂缺译文）]() 和 [属性渲染（暂缺译文）]()，从而引入了可重用性。还有 [React 的上下文及其 Provider 和 Consumer 组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Context.md)，它引入了另一层抽象。React 中的所有这些高级模式都使用了所谓的包装组件。对于创建大型 React 应用程序的开发人员来说，以下组件的实现并不陌生。

```js
import { compose } from "recompose";
import { withRouter } from "react-router-dom";

function App({ history, state, dispatch }) {
  return (
    <ThemeContext.Consumer>
      {theme => <Content theme={theme}>...</Content>}
    </ThemeContext.Consumer>
  );
}

export default compose(withRouter, withReducer(reducer, initialState))(App);
```

Sophie Alpert coined it "the wrapper hell" in React. You are not only seeing it in the implementation, but also when inspecting your components in the browser. There are dozens of wrapped components due to Render Prop Components (including Consumer components from React's Context) and Higher-Order Components. It becomes an unreadable component tree, because all the abstracted logic is covered up in other React components. The actual visible components are hard to track down in the browser's DOM. So what if these additional components were not needed because the logic is only encapsulated in functions as side-effects instead? Then you would remove all these wrapping components and flatten your component tree's structure:

Sophie Alpert 在 React 中创造了「包装地狱」这个词。不仅可以在实现中看到它，还可以在浏览器中查看组件。由于要渲染 prop 组件（包括来自 React 上下文的 Consumer 组件）和高阶组件，所以有数十个包装组件。它变成了一个不可读的组件树，因为所有的抽象逻辑都包含在其他的 React 组件中。在浏览器的 DOM 中很难找到实际可见的组件。那么，如果因为逻辑只封装在函数中作为副作用而不需要这些额外的组件呢？然后你会删除所有这些包装组件，并扁平化你的组件树的结构：

```js
function App() {
  const theme = useTheme();
  const history = useRouter();
  const [state, dispatch] = useReducer(reducer, initialState);

  return <Content theme={theme}>...</Content>;
}

export default App;
```

That's what React Hooks are bringing on the table. All side-effects are sitting directly in the component without introducing other components as container for business logic. The container disappears and the logic just sits in React Hooks that are only functions. [Andrew Clark already left a statement in favor of React Hooks in his popular Higher-Order Component library called recompose.](https://github.com/acdlite/recompose/commit/7867de653abbb57a49934e52622a60b433bda918)

这就是 React 钩子带来的东西。所有副作用都直接位于组件中，而不引入其他组件作为业务逻辑的容器。容器消失了，逻辑只放在 React 钩子中，而这些钩子只是函数。[Andrew Clark 已经在他的高阶组件库 recompose 中留下了支持 React 钩子的声明。](https://github.com/acdlite/recompose/commit/7867de653abbb57a49934e52622a60b433bda918)

**JavaScript Class Confusion:** JavaScript mixes two worlds pretty well: Object-oriented programming (OOP) and functional programming. React introduces many developers to both worlds. On the one side, React (and Redux) introduced people to functional programming (FP) with function compositions, general programming concepts with functions (e.g. higher-order functions, JavaScript built-in methods like map, reduce, filter) and other terms such as immutability and side-effects. React itself didn't really introduce these things, because they are features of the language or the programming paradigm itself, but they are heavily used in React whereas [every React developer becomes automatically a better JavaScript developer](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/).

**JavaScript 类的困惑：** JavaScript 很好地混合了两个世界，面向对象编程（OOP）和函数式编程。React 向这两个领域引进了许多开发人员。一方面，React（和 Redux）通过函数组合向人们介绍了函数编程（FP），使用函数的一般编程概念（例如，高阶函数，JavaScript 内置方法，如 map、reduce、filter）和其他术语，如不变性和副作用。React 本身并没有真正引入这些东西，因为它们是该语言或编程范式本身的特性，但是它们在 React 中大量使用，而 [每个 React 开发人员都自然而然会成为更好的 JavaScript 开发人员（暂缺译文）]()。

On the other side, React uses JavaScript classes as one way to define React components. A class is only the declaration whereas the actual usage of the component is the instantiation of it. It creates a class instance whereas the `this` object of the class instance is used to interact with class methods (e.g. setState, forceUpdate, other custom class methods). However, classes come with a steeper learning curve for React beginners who are not coming from an OOP background. That's why class bindings, the `this` object and inheritance can be confusing. I have [a few chapters in my React book](https://www.robinwieruch.de/the-road-to-learn-react/) focusing only on this aspect of React which is always the most confusing thing about React for beginners.

另一方面，React 使用 JavaScript 类作为定义 React 组件的一种方式。类只是声明，而组件的实际使用是它的实例化。它创建一个类实例，而类实例的 `this` 对象用于与类方法（例如 setState、forceUpdate、其他自定义类方法）交互。然而，对于那些没有 OOP 背景的 React 初学者来说，课程的学习曲线更陡峭。这就是为什么类绑定、`this` 对象和继承会令人困惑。[在我的 React 书中有几章（暂缺译文）]() 关注 React 的这一方面，这也是对初学者来说 React 最让人困惑的地方。

```js
// I THOUGHT WE ARE USING A CLASS. WHY IS IT EXTENDING FROM SOMETHING?
class Counter extends Component {
  // WAIT ... THIS WORKS???
  state = { value: 0 };

  // I THOUGH IT'S THIS WAY, BUT WHY DO I NEED PROPS HERE?
  // constructor(props) {
  //  SUPER???
  //  super(props);
  //
  //  this.state = {
  //    value: 0,
  //  };
  // }

  // WHY DO I HAVE TO USE AN ARROW FUNCTION???
  onIncrement = () => {
    this.setState(state => ({
      value: state.value + 1
    }));
  };

  // SHOULDN'T IT BE this.onDecrement = this.onDecrement.bind(this); in the constructor???
  // WHAT'S this.onDecrement = this.onDecrement.bind(this); DOING ANYWAY?
  onDecrement = () => {
    this.setState(state => ({
      value: state.value - 1
    }));
  };

  render() {
    return (
      <div>
        {this.state.value}

        {/* WHY IS EVERYTHING AVAILABLE ON "THIS"??? */}
        <button onClick={this.onIncrement}>+</button>
        <button onClick={this.onDecrement}>-</button>
      </div>
    );
  }
}
```

Now, many people argue React shouldn't take JavaScript classes away because people don't understand them. After all, they belong to the language. However, one of the hypotheses of introducing the Hooks API is a smoother learning curve for React beginners when writing their React components without JavaScript classes in the first place.

现在，很多人认为 React 不应该因为人们难以理解而取消 JavaScript 类。毕竟，它们属于语言的一部分。然而，引入钩子 API 的一个假设是，对于 React 初学者来说，在编写他们的 React 组件时，如果一开始就不使用 JavaScript 类，那么他们的学习曲线会更平滑。

### Exercises:

- Read more about [React Function Components](https://www.robinwieruch.de/react-function-component/)

阅读有关 [函数组件](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Function-Components.md) 的更多内容。

## React Hooks: What changes in React?

Every time a new feature is introduced, people are concerned about it. There is one side of the group that is ecstatic about the change, and the other side that fears the change. I heard the most common concerns for React Hooks are:

每次引入一个新特性，人们都会关注它。团队中有一方对变化欣喜若狂，而另一方则害怕变化。我听过 React 钩子最常见的问题是：

- Everything changes! _Subtle panic mode ..._
- React is becoming bloated like Angular!
- It’s useless, classes worked fine.
- It’s magic!

Let me address these concerns here:

让我在此说明这些问题：

**Everything changes:** React Hooks will change how we write React applications in the future. However, at the moment, nothing changes. You can still write class components with local state and lifecycle methods and deploy advanced patterns such as Higher-Order Components or Render Prop Components. Nobody takes these learnings away from you. See how I upgraded all my open source projects from older versions to React 16.6. and none of of these projects had problems. They are using HOCs, Render Props and I believe even the old context API (correct me if am wrong). Everything I have learned all these years still works. The React team makes sure that React stays backward compatible. It will be the same with React 16.7.

**一切都改变了：** React 钩子将改变我们未来编写 React 应用的方式。然而，此时此刻，一切都没有改变。你仍然可以使用本地状态和生命周期方法编写类组件，并部署高级模式，如高阶组件或 Render Prop 组件。没人能从你身上学到东西。看看我是如何将我所有的开源项目从旧版本升级到 React 16.6 的。这些项目都没有出现问题。他们使用 HOCs、Render Props、我相信甚至是旧的上下文 API（如果我错了，请纠正我）。这些年来我所学到的一切仍然有效。React 团队确保 React 保持向后兼容。React 16.7 也是一样。

**注：原文此处有图片，对内容帮助不大，省略。**

**React is becoming bloated like Angular:** React was always seen as a library with a slim API. That's true and shall be true in the future. However, in order to adjust things that were the status quo of building component-based applications a few years ago, and not to be overtaken by other libraries who adapt to the new status quo, React introduces changes in favor of older APIs. If React would start out fresh this year, maybe there would be only function components and hooks. But React was released a couple of years ago and needs to adapt to keep up with the status quo or to invent a status quo. Maybe there will follow deprecations of React class components and lifecycle methods in a few years in favor of React function components and hooks, but at the moment, the React team keeps React class components in their repertoire of tools. After all, the React team utilizes hooks as an invention to run a marathon with React an not to win a sprint. Obviously, React Hooks add yet another API to React, but it is in favor to simplify React's API in the future. I like this transition more than having a React 2 where everything is different.

**React 变得像 Angular 一样臃肿:** React 一直被看作是一个 API 很精简的库。这是真的，将来也会是真的。然而，为了调整几年前构建基于组件的应用程序的现状，并且不被适应新现状的其他库所取代，React 引入了有利于旧 API 的更改。如果今年 React 能重新启动，也许就只有函数组件和钩子了。但是 React 是几年前发布的，需要适应现状，或者创造一个现状。或许在未来几年内，React 类组件和生命周期方法将被弃用，转而支持 React 函数组件和钩子，但目前，React 团队将 React 类组件保留在他们的工具库中。毕竟，React 团队利用钩子作为一项发明来跑马拉松，而 React 则不是为了赢得短跑比赛。显然，React 钩子添加了其他 API 到 React，但是在未来简化 React 的 API 是有好处的。我更喜欢这种转变，而不是「一切都不同」的反应。

**It’s useless, classes worked fine:** Imagine you would start from zero to learn React and you would be introduced to React with Hooks. Maybe [create-react-app](https://github.com/facebook/create-react-app) wouldn't start out with a React class component but with a React function component. Everything you need to learn for your components would be React Hooks. They manage state and side-effects, so you would only need to know about the state and the effect hook. It's everything a React class component did for you before. It will be simpler for React beginners to learn React without all the other overhead that comes with JavaScript classes (inheritance, this, bindings, super, ...). Imagine React Hooks as a new way of how to write React components - It's a new mindset. I am a skeptical person myself, but once I wrote a couple of simpler scenarios with React Hooks, I was convinced that this is the simplest way to write but also to learn React. As someone who is doing lots of React workshops, I argue that it takes away all the frustration classes bring on the table for React beginners.

**没用，类工作得很好：** 想象从零开始学习 React，你会被首先推荐用钩子。也许 [create-react-app](https://github.com/facebook/create-react-app) 不会从一个 React 类组件开始，而是从一个 React 函数组件开始。你需要为组件学习的所有内容都是 React 钩子。它们管理状态和副作用，因此你只需要了解状态和 effect 钩子。它是 React 类组件以前为你做的所有事情。对于 React 初学者来说，学习 React 会更简单，而不会有 JavaScript 类带来的其他开销（继承、this、绑定、超类……)。把 React 钩子想象成一种编写 React 组件的新方式（这是一种新的思维方式）。我自己就是一个多疑的人，但是当我用 React 钩子写了几个简单的场景后，我确信这是最简单的写作方法，也是学习 React 的方法。作为一个参加了很多 React 研讨会的人，我认为它消除了 React 初学者所面临的所有挫折。

**It’s magic:** React is known to be down to earth with JavaScript. Writing React applications makes you a better JavaScript developer - that's one of the best things about React when someone asks me: "Why should I learn React?". Whether there comes another library in the future or not, everyone is prepared by honing their JavaScript skills and general programming skills when using React. It's one of the things that made Redux, often used in React, popular: There is no magic, it is plain JavaScript. Now these React Hooks come along the way, introduce something stateful in a previously often pure function component, a couple of not easily to accept rules, and many don't understand what's going on under the hood. But think about it this way: A function component in React is not a mere function. You still have to import React as library to your source code file. It does something with your function, because the function becomes a function component in React land. This function component comes with hidden implementations that were there all the time. How else would it have been possible to use functions as function components as we did it before React Hooks were introduced? And people accepted it too, even though it's kinda magic. Now, the only thing changed (and maybe it has already been this way before) is that these function components come with an extra hidden object that keeps track of hooks. To quote Dan Abramov from [his article about hooks](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889): _"Perhaps you’re wondering where React keeps the state for Hooks. The answer is it’s kept in the exact same place where React keeps state for classes. React has an internal update queue which is the source of truth for any state, no matter how you define your components."_.

**它是魔法：** React is known to be down to earth with JavaScript. 编写 React 应用程序可以让你成为更好的 JavaScript 开发人员，当有人问我：「为什么我要学习 React？」。不管将来是否会出现另一个库，每个人在使用 React 时都要磨练自己的 JavaScript 技能和通用编程技能。这是使 Redux（经常用于 React）流行起来的原因之一：它没有魔力，它是普通的 JavaScript。现在，这些 React 钩子出现了，在以前常常是纯函数组件中引入了一些有状态的东西，一些不容易接受的规则，还有许多不理解发生了什么的底层。但是这样想：React 中的一个函数组件不仅仅是一个函数。你仍然需要将 React 作为库导入源代码文件。它对你的函数做了一些事情，因为这个函数变成了 React 中的一个函数组件。这个函数组件带有一直存在的隐藏实现。在引入 React 钩子之前，我们是如何将函数用作函数组件的呢？人们也接受了它，尽管它有点神奇。现在，惟一改变的是（可能以前已经这样做了）这些函数组件带有一个额外的隐藏对象，用于跟踪钩子。引用 Dan Abramov [他关于钩子的文章中的话](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889) ：「也许你想知道 React 将钩子的状态保存在哪里。答案是，它与 React 保持类状态的位置完全相同。React 有一个内部更新队列，它是任何状态的真实来源，不管你如何定义你的组件。」

**Finally, think about it this way:** Component-based solutions such as Angular, Vue, and React are pushing the boundaries of web development with every release. They build up on top of technologies that were invented more than two decades ago. They adapt them to make web development effortless in 2018 and not 1998. They optimize them like crazy to meet the needs in the here and now. We are building web applications with components and not with HTML templates anymore. We are not there yet, but I imagine a future where we sit together and invent a component-based standard for the browser. Angular, Vue and React are only the spearhead of this movement.

**最后，请这样想：** 基于组件的解决方案，如 Angular、Vue 和 React，每一个版本都在挑战 web 开发的极限。它们建立在 20 多年前发明的技术之上。在 2018 年（而不是 1998 年），他们对其进行了改良，使 web 开发变得轻松。他们疯狂地优化以满足此时此地的需求。我们正在使用组件构建 web 应用程序，而不再使用 HTML 模板。虽然我们还没有达到那个目标，但我想象着这样一个未来：我们坐在一起，为浏览器发明一个基于组件的标准。Angular、Vue 和 React 只是这场运动的先锋。

In the following, I want to dive into a few popular React Hooks by example to get you up to speed. All examples can be found in this [GitHub repository](https://github.com/the-road-to-learn-react/react-hooks-introduction).

在接下来的文章中，我想通过一些流行的 React 钩子示例来帮助你快速上手。所有的例子都可以在这个 [GitHub 库](https://github.com/the-road-to-learn-react/react-hooks-introduction) 中找到。

### Exercises:

- Read more about [React Class to Function Component Migration](https://www.robinwieruch.de/react-hooks-migration)

阅读更多有关 [React 类组件向函数组件迁移（暂缺译文）]() 的内容

## React useState Hook

You have seen the useState Hook before in a code snippet for a typical counter example. It is used to manage local state in function components. Let's use the hook in a more elaborate example where we are going to manage an array of items. In another article of mine, you can learn more about [managing arrays as state in React](https://www.robinwieruch.de/react-state-array-add-update-remove/), but this time we are doing it with React hooks. Let's get started:

你已经在一个典型的反例代码片段中看到过 useState 钩子。它用于管理函数组件中的本地状态。让我们在一个更详细的示例中使用这个钩子，在这个示例中我们将管理一个数组。在我的另一篇文章中，你可以了解更多关于 [在 React 中将数组作为状态来管理（暂缺译文）]() 的内容，但这次我们使用的是 React 钩子。让我们开始：

```js
import React, { useState } from "react";

const INITIAL_LIST = [
  {
    id: "0",
    title: "React with RxJS for State Management Tutorial",
    url: "https://www.robinwieruch.de/react-rxjs-state-management-tutorial/"
  },
  {
    id: "1",
    title: "A complete React with Apollo and GraphQL Tutorial",
    url: "https://www.robinwieruch.de/react-graphql-apollo-tutorial"
  }
];

function App() {
  const [list, setList] = useState(INITIAL_LIST);

  return (
    <ul>
      {list.map(item => (
        <li key={item.id}>
          <a href={item.url}>{item.title}</a>
        </li>
      ))}
    </ul>
  );
}

export default App;
```

The useState hook accepts an initial state as argument and returns, [by using array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring), two variables that can be named however you want to name them. Whereas the first variable is the actual state, the second variable is a function to update the state by providing a new state.

useState 钩子接受初始状态作为参数并返回，[通过使用数组解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring)，这两个变量可以按你希望的方式命名。第一个变量是实际的状态，而第二个变量是一个通过提供新状态来更新状态的函数。

The goal of this scenario is to remove an item from the list. In order to accomplish it, every item in the rendered list has a button with a click handler. The click handler can be inlined in the function component, because it will make use of `list` and `setList` later. Hence you don't need to pass these variables to the handler, because they are already available from the outer scope of the component.

这个场景的目标是从 list 中删除一个项目。为了完成它，渲染 list 中的每个项目都有一个带有处理程序的单击按钮。单击处理程序可以内嵌在函数组件中，因为它稍后将使用 `list` 和 `setList`。因此，你不需要将这些变量传递给处理程序，因为它们已经可以从组件的外部范围中获得。

```js
function App() {
  const [list, setList] = useState(INITIAL_LIST);

  function onRemoveItem() {
    // remove item from "list"
    // set the new list in state with "setList"
  }

  return (
    <ul>
      {list.map(item => (
        <li key={item.id}>
          <a href={item.url}>{item.title}</a>
          <button type="button" onClick={onRemoveItem}>
            Remove
          </button>
        </li>
      ))}
    </ul>
  );
}
```

Somehow we need to know about the item that should be removed from the list. Using a higher-order function, we can pass the identifier of the item to the handler function. Otherwise we wouldn't be able to identify the item that should be removed from the list.

我们需要知道哪些项目应该从列表中删除。使用高阶函数，我们可以将项的标识符传递给处理程序函数。否则我们将无法识别应该从列表中删除的项目。

```js
function App() {
  const [list, setList] = useState(INITIAL_LIST);

  function onRemoveItem(id) {
    // remove item from "list"
    // set the new list in state with "setList"
  }

  return (
    <ul>
      {list.map(item => (
        <li key={item.id}>
          <a href={item.url}>{item.title}</a>
          <button type="button" onClick={() => onRemoveItem(item.id)}>
            Remove
          </button>
        </li>
      ))}
    </ul>
  );
}
```

Finally, use the identifier to filter the list with a built-in array method. It returns a new list which is used to set the new state of the list.

最后，使用标识符通过内置的数组方法筛选 list。它返回一个用于设置新状态的新 list。

```js
function App() {
  const [list, setList] = useState(INITIAL_LIST);

  function onRemoveItem(id) {
    const newList = list.filter(item => item.id !== id);
    setList(newList);
  }

  return (
    <ul>
      {list.map(item => (
        <li key={item.id}>
          <a href={item.url}>{item.title}</a>
          <button type="button" onClick={() => onRemoveItem(item.id)}>
            Remove
          </button>
        </li>
      ))}
    </ul>
  );
}
```

That should do the job. You are able to remove an item from the list based on the identifier you pass to the handler. The handler then filters the list and sets the new state of the list with the `setList` function.

应该可以了。可以根据传递给处理程序的标识符从 list 中删除项。然后处理程序过滤 list 并使用 `setList` 函数设置 list 的新状态。

The useState hook gives you everything you need to manage state in a function component: initial state, the latest state, and a state update function. Everything else is JavaScript again. Furthermore, you don't need to bother about the state object with its shallow merge as before in a class component. Instead, you encapsulate one domain (e.g. list) with useState, but if you would need another state (e.g. counter), then just encapsulate this domain with another useState.

useState 钩子为你提供了在函数组件中管理状态所需的一切：初始状态、最新状态和状态更新函数。其他一切都是 JavaScript。而且，你不需要像以前在类组件中那样，用它的浅合并来处理状态对象。相反，你可以用 useState 封装一个域（如 list），但如果需要另一个状态（如 counter），则只需用另一个 useState 封装此域即可。

### Exercises:

- Read more about [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook)

阅读更多有关 [useState 钩子（暂缺译文）]() 的内容

## React useEffect Hook

Let's head over to the next hook called useEffect. As mentioned before, function components should be able to manage state and side-effects with hooks. Managing state was showcased with the useState hook. Now comes the useEffect hook into play for side-effects which are usually used for interactions with the Browser/DOM API or external API like data fetching. Let's see how the useEffect hook can be used for interaction with the Browser API by implementing a simple stopwatch. You can see how it is done in a React class component in this [GitHub repository](https://github.com/the-road-to-learn-react/react-interval-setstate-unmounted-component-performance).

让我们转到下一个钩子，即 useEffect。如前所述，函数组件应该能够使用钩子管理状态和副作用。useState 钩子展示了状态管理。现在出现了 useEffect 钩子来处理副作用，这些副作用通常用于与浏览器/DOM API 或外部 API（如数据获取）的交互。让我们来看看如何通过 useEffect 钩子与浏览器 API 进行交互实现一个简单的秒表。你可以在这个 [GitHub 库](https://github.com/the-road-to-learn-react/react-interval-setstate-unmounted-component-performance) 中的 React 类组件中看到它是如何完成的。

```js
import React, { useState } from "react";

function App() {
  const [isOn, setIsOn] = useState(false);

  return (
    <div>
      {!isOn && (
        <button type="button" onClick={() => setIsOn(true)}>
          Start
        </button>
      )}

      {isOn && (
        <button type="button" onClick={() => setIsOn(false)}>
          Stop
        </button>
      )}
    </div>
  );
}

export default App;
```

There is no stopwatch yet. But at least there are is a [conditional rendering](https://www.robinwieruch.de/conditional-rendering-react/) to show either a "Start" or "Stop" button. The state for the boolean flag is managed by the useState hook.

现在还没有秒表。但至少有一个 [条件渲染](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-Conditional-Rendering.md#conditional-rendering-in-react-if) 来显示「Start」或「Stop」按钮。boolean 标志的状态由 useState 钩子管理。

Let's introduce our side-effect with useEffect that registers an interval. The function used for the interval emits a console logging every second to your developer tools of your browser.

让我们介绍一下实现副作用和 interval 的 useEffect。用于 interval 的函数每秒钟向浏览器的开发工具发出一个控制台日志记录。

```js
import React, { useState, useEffect } from "react";

function App() {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    setInterval(() => console.log("tick"), 1000);
  });

  return (
    <div>
      {!isOn && (
        <button type="button" onClick={() => setIsOn(true)}>
          Start
        </button>
      )}

      {isOn && (
        <button type="button" onClick={() => setIsOn(false)}>
          Stop
        </button>
      )}
    </div>
  );
}

export default App;
```

In order to remove the interval when the component unmounts (but also after every other render update), you can return a function in useEffect for anything to be called for the clean up. For instance, there shouldn't be any memory leak left behind when the component isn't there anymore.

为了消除组件卸载时的 interval（以及在每次其他渲染更新之后），你可以在 useEffect 中返回一个函数，以便在清理时调用。例如，当组件不存在时，不应该留下任何内存泄漏隐患。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    const interval = setInterval(() => console.log('tick'), 1000);

    return () => clearInterval(interval);
  });

  ...
}

export default App;
```

Now, you want to setup the side-effect when mounting the component and the clean up the side-effect when unmounting the component. If you would log how many times the function within the effect is called, you would see that it sets a new interval every time the state of the component changes (e.g. click on "Start"/"Stop" button).

现在，你需要在安装组件时设置副作用，并在卸载组件时清除副作用。如果你要记录调用效果中的函数的次数，那么你将看到每当组件的状态发生变化时，它都会设置一个新的 interval（例如，单击「Start」/「Stop」按钮）。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    console.log('effect runs');
    const interval = setInterval(() => console.log('tick'), 1000);

    return () => clearInterval(interval);
  });

  ...
}

export default App;
```

In order to only run the effect on mount and unmount of the component, you can pass it an empty array as second argument.

为了只在组件的挂载和卸载上运行效果，可以将一个空数组作为第二个参数传递给它。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    const interval = setInterval(() => console.log('tick'), 1000);

    return () => clearInterval(interval);
  }, []);

  ...
}

export default App;
```

However, since the interval is cleaned up after every render too, we need to set the interval in our update cycle too. But we can tell the effect to run only when the `isOn` variable changes. Only when one of the variables in the array changes, the effect will run during the update cycle. If you keep the array empty, the effect will only run on mount and unmount, because there is no variable to be checked for running the side-effect again.

但是，由于 interval 也会在每次渲染后清除，所以我们也需要在更新周期中设置。但是，我们可以告诉 effect，只有当 `isOn` 变量改变才运行。只有当数组中的一个变量发生变化时，该 effect 才会在更新周期中运行。如果保持数组为空，则该效果将仅在挂载和卸载时运行，因为不需要检查变量是否再次运行副作用。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    const interval = setInterval(() => console.log('tick'), 1000);

    return () => clearInterval(interval);
  }, [isOn]);

  ...
}

export default App;
```

The interval is running whether the `isOn` boolean is true or false. It would be great to only run it when the stopwatch is activated.

不管 `isOn` 布尔值是 true 还是 false，interval 都在运行。如果只在秒表被激活时才运行它，那就太好了。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);

  useEffect(() => {
    let interval;

    if (isOn) {
      interval = setInterval(() => console.log('tick'), 1000);
    }

    return () => clearInterval(interval);
  }, [isOn]);

  ...
}

export default App;
```

Now introduce another state in your function component to keep track of the timer of the stopwatch. It is used to update the timer, but only when the stopwatch is activated.

现在在函数组件中引入另一种状态来跟踪秒表的计时器。它用于更新计时器，但仅在秒表激活时使用。

```js
import React, { useState, useEffect } from "react";

function App() {
  const [isOn, setIsOn] = useState(false);
  const [timer, setTimer] = useState(0);

  useEffect(() => {
    let interval;

    if (isOn) {
      interval = setInterval(() => setTimer(timer + 1), 1000);
    }

    return () => clearInterval(interval);
  }, [isOn]);

  return (
    <div>
      {timer}

      {!isOn && (
        <button type="button" onClick={() => setIsOn(true)}>
          Start
        </button>
      )}

      {isOn && (
        <button type="button" onClick={() => setIsOn(false)}>
          Stop
        </button>
      )}
    </div>
  );
}

export default App;
```

There is still one mistake in the code. When the interval is running, it updates the timer every second by increasing it by one. However, it always relies on a stale state for the timer. Only when the `inOn` boolean flag changes the state is fine. In order to receive always the latest state for the timer when the interval is running, you can use a function instead for the state update function which always has the latest state.

> 译注：原文 `inOn` 笔误，应为 `isOn`

代码中还有一个错误。当 interval 正在运行时，它每秒钟通过增加一个计时器来更新计时器。但是，它总是依赖于计时器的陈旧状态。只有当 `isOn` 布尔标志改变状态时才是正确的。为了在 interval 运行时始终接收计时器的最新状态，你可以使用一个函数来代替始终具有最新状态的状态更新函数。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);
  const [timer, setTimer] = useState(0);

  useEffect(() => {
    let interval;

    if (isOn) {
      interval = setInterval(
        () => setTimer(timer => timer + 1),
        1000,
      );
    }

    return () => clearInterval(interval);
  }, [isOn]);

  ...
}

export default App;
```

An alternative would have been to run the effect also when the timer changes. Then the effect would receive the latest timer state.

另一种方法是在计时器改变时也运行该 effect。然后，effect 将接收最新的计时器状态。

```js
import React, { useState, useEffect } from 'react';

function App() {
  const [isOn, setIsOn] = useState(false);
  const [timer, setTimer] = useState(0);

  useEffect(() => {
    let interval;

    if (isOn) {
      interval = setInterval(
        () => setTimer(timer + 1),
        1000,
      );
    }

    return () => clearInterval(interval);
  }, [isOn, timer]);

  ...
}

export default App;
```

That's the implementation for the stopwatch that uses the Browser API If you want to continue, you can extend the example by providing a "Reset" button too.

这是秒表的实现，它使用了浏览器 API，如果你想继续，你也可以通过提供一个「Reset」按钮来扩展这个例子。

```js
import React, { useState, useEffect } from "react";

function App() {
  const [isOn, setIsOn] = useState(false);
  const [timer, setTimer] = useState(0);

  useEffect(() => {
    let interval;

    if (isOn) {
      interval = setInterval(() => setTimer(timer => timer + 1), 1000);
    }

    return () => clearInterval(interval);
  }, [isOn]);

  const onReset = () => {
    setIsOn(false);
    setTimer(0);
  };

  return (
    <div>
      {timer}

      {!isOn && (
        <button type="button" onClick={() => setIsOn(true)}>
          Start
        </button>
      )}

      {isOn && (
        <button type="button" onClick={() => setIsOn(false)}>
          Stop
        </button>
      )}

      <button type="button" disabled={timer === 0} onClick={onReset}>
        Reset
      </button>
    </div>
  );
}

export default App;
```

That's it. The useEffect hook is used for side-effects in React function components that are used for interacting with the Browser/DOM API or other third-party APIs (e.g. data fetching). You can read more about [the useEffect hook in React's documentation](https://reactjs.org/docs/hooks-effect.html).

就是这样。useEffect 钩子用于 React 函数组件中的副作用，这些组件用于与浏览器/DOM API 或其他第三方 API（如数据获取）进行交互。你可以在 React 的文档中阅读更多关于 [useEffect 钩子](https://reactjs.org/docs/hooks-effect.html) 的信息。

## React Custom Hooks

Last but not least, after you have learned about the two most popular hooks that introduce state and side-effects in function components, there is one last thing I want to show you: custom hooks. That's right, you can implement your own custom React Hooks that can be reused in your application or by others. Let's see how they work with an example application which is able to detect whether your device is online or offline.

最后，但并非最不重要的是，在你了解了在函数组件中引入状态和副作用的两个最流行的钩子之后，我想向你展示最后一件事：自定义钩子。没错，你可以实现自己的自定义 React 钩子，这些钩子可以在你的应用程序中重用，也可以由其他人重用。让我们看看它们是如何与一个能够检测你的设备是在线还是离线的示例应用程序一起工作的。

```js
import React, { useState } from "react";

function App() {
  const [isOffline, setIsOffline] = useState(false);

  if (isOffline) {
    return <div>Sorry, you are offline ...</div>;
  }

  return <div>You are online!</div>;
}

export default App;
```

Again, introduce the useEffect hook for the side-effect. In this case, the effect adds and removes listeners that check if the device is online or offline. Both listeners are setup only once on mount and cleaned up once on unmount (empty array as second argument). Whenever one of the listeners is called, it sets the state for the `isOffline` boolean.

再次，介绍用于副作用的 useEffect 钩子。在本例中，effect 是添加和删除检查设备是在线还是离线的监听器。两个监听器在 mount 上只设置一次，在 unmount 上清除一次（空数组作为第二个参数）。无论何时调用一个监听器，它都会设置 `isOffline` 布尔值的状态。

```js
import React, { useState, useEffect } from "react";

function App() {
  const [isOffline, setIsOffline] = useState(false);

  function onOffline() {
    setIsOffline(true);
  }

  function onOnline() {
    setIsOffline(false);
  }

  useEffect(() => {
    window.addEventListener("offline", onOffline);
    window.addEventListener("online", onOnline);

    return () => {
      window.removeEventListener("offline", onOffline);
      window.removeEventListener("online", onOnline);
    };
  }, []);

  if (isOffline) {
    return <div>Sorry, you are offline ...</div>;
  }

  return <div>You are online!</div>;
}

export default App;
```

Everything is nicely encapsulated in one effect now. It's a great functionality which should be reuse somewhere else too. That's why we can extract the functionality as its a custom hook which follows the same naming convention as the other hooks.

现在所有的东西都很好地封装在一个 effect 中。这是一个很好的功能，应该在其他地方重用。这就是为什么我们可以提取功能，因为它是一个自定义钩子，它遵循与其他钩子相同的命名约定。

```js
import React, { useState, useEffect } from "react";

function useOffline() {
  const [isOffline, setIsOffline] = useState(false);

  function onOffline() {
    setIsOffline(true);
  }

  function onOnline() {
    setIsOffline(false);
  }

  useEffect(() => {
    window.addEventListener("offline", onOffline);
    window.addEventListener("online", onOnline);

    return () => {
      window.removeEventListener("offline", onOffline);
      window.removeEventListener("online", onOnline);
    };
  }, []);

  return isOffline;
}

function App() {
  const isOffline = useOffline();

  if (isOffline) {
    return <div>Sorry, you are offline ...</div>;
  }

  return <div>You are online!</div>;
}

export default App;
```

Extracting the custom hook as function was not the only thing. You also have to return the `isOffline` state from the custom hook in order to use it in your application to show a message to users who are offline. Otherwise, it should render the normal application. That's it for the custom hook that detects whether you are online or offline. You can read more about [custom hooks in React's documentation](https://reactjs.org/docs/hooks-custom.html).

将自定义钩子提取为函数并不是惟一的事情。你还必须从自定义钩子返回 `isOffline` 状态，以便在你的应用程序中使用它来向脱机用户显示消息。否则，它应该渲染正常的应用程序。这就是检测你是在线还是离线的自定义钩子。你可以在 React 的文档中阅读更多关于 [自定义钩子](https://reactjs.org/docs/hooks-custom.html) 的信息。

React Hooks being reusable is the best thing about them, because there is the potential to grow an ecosystem of custom React Hooks that can be installed from npm for any React application. And not only for React applications. Evan You, creator of Vue, [is hooked (!) by them as well](https://twitter.com/youyuxi/status/1056042395891105793). Maybe we will see a bridge between both ecosystems where it is possible to share hooks between Vue and React.

React 钩子是可重用的，这是它们最大的优点，因为有可能形成一个自定义 React 钩子生态系统，可以从 npm 安装到任何 React 应用程序。而且不仅用于 React 应用程序。Evan You, Vue 的创造者，也被他们吸引住了。也许我们会看到在这两个生态系统之间架起一座桥梁，这样就可以在 Vue 和 React 之间共享钩子。

### Exercises:

- Read more about [React State with Hooks](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext)

阅读更多关于 [React 钩子的状态管理](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/React-State-Hooks-useReducer-useState-useContext.md) 的内容

- Read more about [How to fetch data with React Hooks](https://www.robinwieruch.de/react-hooks-fetch-data)

阅读更多关于 [如何通过 React 钩子获取数据（缺少译文）]() 的内容

If you want to dive deeper into the state and effect hooks, check out my other React Hook tutorials:

如果你想深入了解状态和 effect 挂钩，看看我的其他 React 钩子教程：

- [How to useEffect Hook?](https://www.robinwieruch.de/react-usecontext-hook/)

> 译注：原文链接有误，不应该是 https://www.robinwieruch.de/react-usecontext-hook/

[怎样使用 useEffect 钩子（缺少译文）]()

- [How to useReducer Hook?](https://www.robinwieruch.de/react-usereducer-hook/)

[怎样使用 useReducer 钩子](https://github.com/clxering/Technical-Articles-Collection/blob/master/React/How-to-useReducer-in-React.md)

Check out the official [FAQ](https://reactjs.org/docs/hooks-faq.html) and [Rules](https://reactjs.org/docs/hooks-rules.html) for hooks in React's documentation to learn more about their fine-grained behaviour. In addition, you can checkout [all officially available React Hooks](https://reactjs.org/docs/hooks-reference.html) too.

在 React 的文档中查看官方的 [问答](https://reactjs.org/docs/hooks-faq.html) 和 [规则](https://reactjs.org/docs/hooks-rules.html) 以了解它们的详细行为。此外，你还可以查看 [所有正式可用的 React 钩子](https://reactjs.org/docs/hooks-reference.html)。
