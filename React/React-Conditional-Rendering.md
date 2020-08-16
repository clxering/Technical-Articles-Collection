# React Conditional Rendering

> 转译自：https://www.robinwieruch.de/conditional-rendering-react

Conditional rendering in React isn't difficult. In JSX - the syntax extension used for React - you can use plain JavaScript which includes if else statements, ternary operators, switch case statements, and much more. In a conditional render, a React component decides based on one or several conditions which DOM elements it will return. For instance, based on some logic it can either return a list of items or a text that says "Sorry, the list is empty". When a component has a conditional rendering, the appearance of the rendered component differs based on the condition. The article aims to be an exhaustive list of options for conditional renderings in React and best practices for these patterns.

React 中的条件渲染并不难。在 React 的语法扩展 JSX 中，可以使用简单的 JavaScript，其中包括 if else 语句、三元运算符、switch case 语句等。在条件渲染中，React 组件根据一个或多个条件来决定返回哪些 DOM 元素。例如，根据某些逻辑，它可以返回一个项目列表，也可以返回一个文本「对不起，列表是空的」。当组件被条件渲染时，组件的外观将根据条件的不同而有所不同。本文的目的是为 React 中的条件渲染和条件渲染模式的最佳实践提供一个详尽的指引列表。

## Table of Contents

- [Conditional Rendering in React: if](#conditional-rendering-in-react-if)
- [Conditional Rendering in React: if else](#conditional-rendering-in-react-if-else)
- [Conditional Rendering in React: ternary](#conditional-rendering-in-react-ternary)
- [Conditional Rendering in React: &&](#conditional-rendering-in-react-&&)
- [Conditional Rendering in React: switch case](#conditional-rendering-in-react-switch-case)
- [Multiple Conditional Renderings in React](#multiple-conditional-renderings-in-react)
- [Nested Conditional Rendering in React](#nested-conditional-rendering-in-react)
- [Conditional Rendering with HOC](#conditional-rendering-with-hoc)
- [If Else Components in React](#if-else-components-in-react)

## Conditional Rendering in React: if

The most basic conditional rendering logic in React is done with a single **if** statement. Imagine you don't want to render something in your [React component](/react-function-component), because it doesn't have the necessary [React props](/react-pass-props-to-component/) available. For instance, a [List component in React](/react-list-component) shouldn't render the list HTML elements in a view if there is no list of items in the first place. You can use a plain JavaScript if statement to return earlier (guard pattern):

React 中最基本的条件渲染逻辑是使用单个 **if** 语句完成的。假设某些内容在没有必要的 [React 属性（暂缺译文）]() 时就不在 [React 组件](/React/React-Function-Components.md) 中渲染。例如，一个 [React 中的 List 组件（暂缺译文）]() 如果一开始就没有项目列表的话，就不应该在视图中渲染列表 HTML 元素。你可以使用一个简单的 JavaScript if 语句提前返回（卫语句模式）：

```js
const users = [
  { id: "1", firstName: "Robin", lastName: "Wieruch" },
  { id: "2", firstName: "Dennis", lastName: "Wieruch" }
];

function App() {
  return (
    <div>
      <h1>Hello Conditional Rendering</h1>
      <List list={users} />
    </div>
  );
}

function List({ list }) {
  if (!list) {
    return null;
  }

  return (
    <ul>
      {list.map(item => (
        <Item key={item.id} item={item} />
      ))}
    </ul>
  );
}

function Item({ item }) {
  return (
    <li>
      {item.firstName} {item.lastName}
    </li>
  );
}
```

Try it yourself by setting `users` to null oder undefined. If the information from the props is null or undefined, the React component returns null in the conditional rendering. There, a React component that returns null instead of JSX will render nothing.

你可以通过将 `users` 设置为 null 或 undefined 来进行尝试。如果属性为 null 或 undefined，则 React 组件在条件渲染中返回 null。在这里，提前返回 null，而不是让 JSX 的 React 组件渲染不出任何内容。

In this example, we have done the conditional rendering based on props, but the conditional rendering could be based on [state](/react-state) and [hooks](/react-hooks) too. Notice, how we didn't use the if statement inside the JSX yet but only outside before the return statement.

在这个例子中，我们已经完成了基于属性的条件渲染，但是条件渲染也可以基于 [状态（暂缺译文）]() 和 [钩子](/React/What-are-React-Hooks.md)。注意，我们还没有在 JSX 内部使用 if 语句，而是只在 return 语句之前使用。

## Conditional Rendering in React: if else

Let's move on with the previous example to learn about **if else** statements in React. If there is no list, we render nothing and hide the HTML as we have seen before with the single if statement. However, you may want to show a text as feedback for your user when the list is empty for a better user experience. This would work with another single if statement, but we will expand the example with an if else statement instead:

让我们继续前面的例子，学习 React 中的 **if else** 语句。如果没有列表，我们将什么也不渲染，并隐藏 HTML，就像我们之前看到的使用单一 If 语句一样。但是，为了获得更好的用户体验，希望在列表为空时显示文本作为给用户的反馈。这可以用另一个单独的 if 语句实现，但是我们将用一个 if else 语句来扩展这个例子：

```js
function List({ list }) {
  if (!list) {
    return null;
  }

  if (!list.length) {
    return <p>Sorry, the list is empty.</p>;
  } else {
    return (
      <div>
        {list.map(item => (
          <Item item={item} />
        ))}
      </div>
    );
  }
}
```

Now, the List component renders either nothing, a text, or the list of items based on some JavaScript logic. Even though the previous example shows you how to use if else statements in React, I suggest to use single if statements every time you want to guard your main return (here: returning the list) as a best practice:

现在，List 组件要么什么也不渲染，要么渲染文本，要么渲染基于某些 JavaScript 逻辑的项列表。尽管前面的例子展示了如何在 React 中使用 if else 语句，但我建议在每次需要「保护」主体输出（这里是返回列表）时使用 if 卫语句作为最佳实践：

```js
function List({ list }) {
  if (!list) {
    return null;
  }

  if (!list.length) {
    return <p>Sorry, the list is empty.</p>;
  }

  return (
    <div>
      {list.map(item => (
        <Item item={item} />
      ))}
    </div>
  );
}
```

This is way more readable than the previous if else conditional rendering. All the guards are neatly aligned as single if statements before the main return statement which can be interpreted as an implicit else statement too. Still, none of the if and else statements were used inside the return statement yet.

这比前一个 if else 条件渲染更具可读性。所有的卫语句都整齐地排列在主体返回语句之前，主体返回语句也可以解释为隐式的 else 语句。但是，在 return 语句中还没有使用 if 和 else 语句。

## Conditional Rendering in React: ternary

It's true that we can use JavaScript in JSX, but it becomes difficult when using statements like **if, else, and switch case within JSX**. There is no real way to inline it. Another way to express an if else statement in JavaScript is the **[ternary operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)**:

的确，我们可以在 JSX 中使用 JavaScript，但是在 **JSX 中使用诸如 if、else 和 switch case** 这样的语句时，就变得困难了。没有真正的方法来内联它。在 JavaScript 中表达 if else 语句的另一种方法是 **[三元操作符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)**：

```js
// if else
function getFood(isVegetarian) {
  if (isVegetarian) {
    return "tofu";
  } else {
    return "fish";
  }
}

// ternary operator
function getFood(isVegetarian) {
  return isVegetarian ? "tofu" : "fish";
}
```

For instance, imagine your component shows either a preview or edit mode. The condition is a JavaScript boolean which comes in as React prop. You can use the boolean to decide which element you want to conditionally render:

例如，假设你的组件显示预览或编辑模式。条件是一个 JavaScript 布尔值，作为 React 的属性。你可以使用布尔值来决定你想要条件渲染哪个元素：

```js
function Recipe({ food, isEdit }) {
  return (
    <div>
      {food.name}

      {isEdit ? <EditRecipe food={food} /> : <ShowRecipe food={food} />}
    </div>
  );
}
```

The parentheses `()` around both implicit return statements in the ternary operator enable you to return a single or multiple HTML elements or React components from there. If it's just a single element though, you can omit the parentheses.

三元运算符中两个隐式返回语句周围默认存在圆括号 `()` ，使你能够返回一个或多个 HTML 元素或 React 组件。如果只是单个元素，可以忽略括号。

Note: Sometimes you want to wrap multiple lines of elements with a div element as one block though. Anyway, try to keep it lightweight. If the wrapper between the `()` grows too big, consider extracting it as a component as shown in the example.

注意：有时你希望用一个 div 元素将多行元素包装成一个块。不管怎样，尽量让它轻一些。如果 `()` 之间的包装代码太多，可以考虑将它提取为一个组件，如示例所示。

The ternary operation makes the conditional rendering in React not only more concise, but gives you an easy way to **inline the conditional rendering in your return**. This way, only a one part of your JSX is conditionally rendered, while other parts can stay intact without any condition.

三元操作不仅使 React 中的条件渲染更加简洁，而且为 **在返回中内联条件渲染** 提供了一种简单的方法。通过这种方式，只有一部分 JSX 是有条件地渲染的，而其他部分可以保持完整而没有任何条件。

## Conditional Rendering in React: &&

It happens often that you want to _render either an element or nothing_. You have learned that a simple if condition helps with that issue. However, then again you want to be able to inline the condition like a ternary operator. Take the following loading indicator component which uses a conditional ternary operator to return either the element or nothing:

经常发生的情况是，你想要渲染一个元素或者什么都不想渲染。你已经了解到，简单的 if 条件有助于解决这个问题。但是，你希望能够像三元运算符那样内联条件。以下面的加载指示符组件为例，它使用一个条件三元运算符来返回元素或什么都不返回：

```js
function LoadingIndicator({ isLoading }) {
  return <div>{isLoading ? <p>Loading...</p> : null}</div>;
}
```

This works just fine and you are done inlining the condition in your JSX. However, there exists an alternative way that omits the necessity to return null.

这能够很好执行，你已经在 JSX 中内联了条件。但是，有种替代方法可以省略返回 null 这一必要步骤。

The **logical && operator** helps you to make conditions that would return null more concise. In JavaScript, a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false:

**逻辑 && 运算符** 使返回 null 的条件更加简洁。在 JavaScript 中，`true && 'Hello World'` 计算结果总是 'Hello World'。`false && 'Hello World'` 计算结果总是 false。

```js
const result = true && "Hello World";
console.log(result);
// Hello World

const result = false && "Hello World";
console.log(result);
// false
```

In React, you can make use of this behaviour. If the condition is true, the expression after the logical && operator will be the output. If the condition is false, React ignores and skips the expression:

在 React 中可以利用这种行为。如果条件为真，则逻辑 && 操作符后面的表达式将输出。如果条件为 false，则 React 将忽略并跳过表达式。

```js
function LoadingIndicator({ isLoading }) {
  return <div>{isLoading && <p>Loading...</p>}</div>;
}
```

That's your way to go when you want to **return nothing or an element inside JSX**. It's also called short-circuit evaluation which makes it even more concise than a ternary operator.

当你想要 **不返回任何内容或 JSX 内的元素时**，你可以这样做。它也被称为短路求值，比三元运算符更简洁。

## Conditional Rendering in React: switch case

Now there might be cases where you have multiple conditional renderings. Take for example a notification component that renders an error, warning, or info component based on a status string:

现在可能有多个条件渲染的情况。以一个通知组件为例，它根据 status 字符串渲染一个错误、警告或信息组件:

```js
function Notification({ text, status }) {
  if (status === "info") {
    return <Info text={text} />;
  }

  if (status === "warning") {
    return <Warning text={text} />;
  }

  if (status === "error") {
    return <Error text={text} />;
  }

  return null;
}
```

You can use a **switch case operator** for _multiple conditional renderings_:

可以使用 **switch case 操作符** 来处理 _多条件渲染_：

```js
function Notification({ text, status }) {
  switch (status) {
    case "info":
      return <Info text={text} />;
    case "warning":
      return <Warning text={text} />;
    case "error":
      return <Error text={text} />;
    default:
      return null;
  }
}
```

It's wise to use the default for the switch case operator, because a React component always has to return an element or null. If a component has a conditional rendering based on a string, it makes sense to describe the interface of the component with TypeScript:

添加 switch case 操作符的 default 分支是明智的做法，因为 React 组件必须返回一个元素或 null。如果组件有基于字符串的条件渲染，用 TypeScript 描述组件的接口是有意义的：

```js
type Status = "info" | "warning" | "error";

type NotificationProps = {
  text: string,
  status: Status
};

function Notification({ text, status }: NotificationProps) {
  switch (status) {
    case "info":
      return <Info text={text} />;
    case "warning":
      return <Warning text={text} />;
    case "error":
      return <Error text={text} />;
    default:
      return null;
  }
}
```

A switch case is a good start for multiple conditional renderings. But it comes with the same drawbacks like an if else statement. A switch case cannot be used within JSX; can it? Actually it's possible with a conditional rendering function which is self invoking:

对于多条件渲染来说，switch case 是一个很好的开端。但是它也有与 if else 语句相同的缺点。不能在 JSX 中使用；可以吗？实际上，条件渲染如果通过自调用函数进行，就是可以的：

```js
function Notification({ text, status }) {
  return (
    <div>
      {(function() {
        switch (status) {
          case "info":
            return <Info text={text} />;
          case "warning":
            return <Warning text={text} />;
          case "error":
            return <Error text={text} />;
          default:
            return null;
        }
      })()}
    </div>
  );
}
```

Optionally make the switch case more concise with an conditional rendering arrow function:

使用 ES6 箭头函数会使 switch case 更简洁，当然这是可选的做法。

```js
function Notification({ text, status }) {
  return (
    <div>
      {(() => {
        switch (status) {
          case "info":
            return <Info text={text} />;
          case "warning":
            return <Warning text={text} />;
          case "error":
            return <Error text={text} />;
          default:
            return null;
        }
      })()}
    </div>
  );
}
```

In conclusion, the switch case operator helps you to have multiple conditional renders. But is it the best way to do that? Let's see how we can have multiple conditional renderings with enums instead.

总之，switch case 操作符可以帮助你实现多条件渲染。但这是最好的方法吗？让我们看看如何使用枚举实现多条件渲染。

## Multiple Conditional Renderings in React

A JavaScript object with key value pairs for a mapping is called an enum:

能够映射键值对的 JavaScript 对象称为枚举：

```js
const NOTIFICATION_STATES = {
  info: "Did you know? ...",
  warning: "Be careful here ...",
  error: "Something went wrong ..."
};
```

An **enum** is a great way to handle _conditional rendering with multiple conditions_ in React. They are switch case statements on steroids, because they can be used within the JSX. Let's consider the notification component again, but this time with an enum as inlined object (inner curly braces):

**枚举** 是 React 中处理 _多条件渲染_ 的好方法。They are switch case statements on steroids，因为它们可以在 JSX 中使用。让我们再次考虑通知组件，但这次使用枚举作为内联对象（它在内花括号中）:

```js
function Notification({ text, status }) {
  return (
    <div>
      {
        {
          info: <Info text={text} />,
          warning: <Warning text={text} />,
          error: <Error text={text} />
        }[status]
      }
    </div>
  );
}
```

The status property key helps us to retrieve the value from the object. That's neat, isn't it? It is much more readable compared to the switch case operator.

state 属性键帮助我们从对象中检索值。很简洁，不是吗？与 switch case 操作符相比，它更具可读性。

In this example, we had to use an inlined object, because the values of the object depend on the text property. That would be my recommended way anyway. However, if it wouldn't depend on the text property, you could use an enum as a constant for the conditional render:

在本例中，我们必须使用内联对象，因为对象的值依赖于 text 属性。这是我推荐的方法。但是，如果它不依赖于 text 属性，你可以将枚举作为条件渲染的常量：

```js
const NOTIFICATION_STATES = {
  info: <Info />,
  warning: <Warning />,
  error: <Error />
};

function Notification({ status }) {
  return <div>{NOTIFICATION_STATES[status]}</div>;
}
```

This cleans things up in the JSX. If we would still rely on the text property from before, we could use a conditional rendering with a function to retrieve the value too:

这就像把 JSX 清理了一遍。如果我们仍然依赖于以前的 text 属性，我们可以使用带函数的条件渲染来检索值：

```js
const getNotification = text => ({
  info: <Info text={text} />,
  warning: <Warning text={text} />,
  error: <Error text={text} />
});

function Notification({ status, text }) {
  return <div>{getNotification(text)[status]}</div>;
}
```

After all, the enum conditional rendering in React is more elegant than the switch case statement. Objects as enums open up plenty of options to have multiple conditional renderings. Permutations of booleans are possible too:

毕竟，与 switch case 语句相比，枚举方法更具可读性。枚举对象提供了大量的操作来实现多条件渲染。Permutations of booleans are possible too:

```js
function Message({ isExtrovert, isVegetarian }) {
  const key = `${isExtrovert}-${isVegetarian}`;

  return (
    <div>
      {
        {
          "true-true": <p>I am an extroverted vegetarian.</p>,
          "true-false": <p>I am an extroverted meat eater.</p>,
          "false-true": <p>I am an introverted vegetarian.</p>,
          "false-false": <p>I am an introverted meat eater.</p>
        }[key]
      }
    </div>
  );
}
```

The last example is a bit over the top though and I wouldn't advice to use it. However, enums are one of my favorite React patterns when it comes to conditional rendering.

最后一个例子的方式有点夸张，我不建议使用。然而，在条件渲染方面，枚举是我最喜欢的 React 模式之一。

## Nested Conditional Rendering in React

What about **nested conditional renderings** in React? Yes, it is possible. For instance, let's have a look at the List component from before that shows either a list, an empty text, or nothing:

**嵌套条件渲染** 在 React 中是否可行呢？是的，可能可以实现。例如，让我们看看之前的 List 组件，它要么显示列表，要么显示一个空文本，要么什么都不显示：

```js
function List({ list }) {
  const isNotAvailable = !list;
  const isEmpty = !list.length;

  return (
    <div>
      {isNotAvailable ? (
        <p>Sorry, the list is not there.</p>
      ) : isEmpty ? (
        <p>Sorry, the list is empty.</p>
      ) : (
        <div>
          {list.map(item => (
            <Item item={item} />
          ))}
        </div>
      )}
    </div>
  );
}
```

It works, however I would recommend to avoid nested conditional renders, because they are verbose which makes it less readable. Instead try the following solutions:

它起作用了，但是我建议避免嵌套的条件渲染，因为它们太冗长，可读性很差。推荐尝试以下方法：

- The guard pattern with only if statements before the main return statement.

在主体返回之前使用只有 if 语句的卫语句模式。

- Splitting the component into multiple components whereas each component takes care of its own non nested conditional rendering.

将组件分解为多个组件，而每个组件负责自己的非嵌套条件渲染。

## Conditional Rendering with HOC

[Higher-Order Components (HOCs)](/react-higher-order-components/) are a perfect match for a conditional rendering in React. HOCs can help with multiple use cases, yet one use case could be to alter the look of a component with a conditional rendering. Let's check out a HOC that either shows a element or a component:

[高阶组件（HOCs）（暂缺译文）]() 是 React 中条件渲染的完美搭档。高阶组件有很多使用场景，其中一个就是使用条件渲染来改变组件的外观。让我们看一个高阶组件，用于显示一个元素或组件：

```js
// Higher-Order Component
function withLoadingIndicator(Component) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (!isLoading) {
      return <Component {...props} />;
    }

    return (
      <div>
        <p>Loading</p>
      </div>
    );
  };
}

const ListWithLoadingIndicator = withLoadingIndicator(List);

function App({ list, isLoading }) {
  return (
    <div>
      <h1>Hello Conditional Rendering</h1>

      <ListWithLoadingIndicator isLoading={isLoading} list={list} />
    </div>
  );
}
```

In this example, the List component can focus on rendering the list. It doesn't have to bother with a loading status. A HOC hides away all the noise from your actual component. Ultimately, you could add multiple higher-order components (composition) to hide away more than one conditional rendering edge case. As alternative to HOCs, you could also use [conditional rendering with a render prop](/react-render-props).

在本例中，List 组件可以专注于渲染列表。它不需要关心加载状态。A HOC hides away all the noise from your actual component. 最后，你可以添加多个高阶组件（组合）来隐藏 more than one conditional rendering edge case. 作为高阶组件的替代品，你还可以使用 [渲染属性实现条件渲染（暂缺译文）]()。

## If Else Components in React

Last but not least, there are external libraries to deal with conditional renderings on a markup level. They add control components to enable conditional renderings without the JS in JSX:

最后，还有一些外部库来处理标签层面上的条件渲染。通过在 JSX 中添加控件组件来进行渲染，而不使用 JavaScript 的条件渲染。

```js
<Choose>
  <When condition={isLoading}>
    <div>
      <p>Loading...</p>
    </div>
  </When>
  <Otherwise>
    <div>
      {list.map(item => (
        <Item item={item} />
      ))}
    </div>
  </Otherwise>
</Choose>
```

Some people use it, but personally I wouldn't recommend it. JSX allows you to use the powerful set of JavaScript functionalities to handle conditional renderings yourself. There is no need to add templating components to enable it. A lot of people consider React including JSX as their library of choice, because they can handle the rendering with pure HTML and JS in JSX.

有些人会使用它。就我个人而言，我不会推荐。JSX 允许你使用强大的 JavaScript 功能集来处理条件渲染。不需要添加模板组件来启用条件渲染。很多人认为 React（包括 JSX）是他们的首选库，因为他们可以在 JSX 中使用纯 HTML 和 JS 处理渲染。

I hope this React tutorial was helpful for you to learn about conditional renderings. If you liked it, please share it with your friends. In the end, I got an all conditional renderings in a cheatsheet for you:

我希望这个 React 教程对你学习条件渲染有帮助。如果你喜欢它，请与你的朋友分享。最后，我在「便条」上给你一个全条件渲染图：

- if

  - most basic conditional rendering

  最基础的条件渲染

  - use to opt-out early from a rendering (guard pattern)

  用于从渲染中提前退出（卫语句模式）

  - cannot be used within return statement and JSX (except self invoking function)

  不能在 return 语句和 JSX 中使用（自调用函数除外）

- if-else

  - use it rarely, because it's verbose

  很少使用它，因为它很冗长

  - instead, use ternary operator or logical && operator

  取而代之的是，使用三元运算符或逻辑 && 运算符

  - cannot be used inside return statement and JSX (except self invoking function)

  不能在 return 语句和 JSX 中使用（自调用函数除外）

- ternary operator

  - use it instead of an if-else statement

  用于取代 if-else 语句

  - it can be used within JSX and return statement

  能在 return 语句和 JSX 中使用

- logical && operator

  - use it when one side of the ternary operation would return null

  当三元操作的一端返回 null 时使用

  - it can be used inside JSX and return statement

  能在 return 语句和 JSX 中使用

- switch case

  - avoid using it, because it's too verbose

  避免使用，因为它太冗长

  - instead, use enums

  取而代之的是，使用枚举

  - cannot be used within JSX and return (except self invoking function)

  不能在 return 语句和 JSX 中使用（自调用函数除外）

- enums

  - use it for conditional rendering based on multiple states

  基于多个状态的条件渲染可以用它

  - perfect to map more than one condition

  可完美映射多个条件

- nested conditional rendering

  - avoid them for the sake of readability

  为了可读性，避免使用它们

  - instead, split out components, use if statements, or use HOCs

  取而代之的是，拆分组件，使用 if 语句或使用高阶组件

- HOCs

  - components can focus on their main purpose

  让组件能够聚焦于主要目的

  - use HOC to shield away conditional rendering

  使用高阶组件屏蔽条件渲染

  - use multiple composable HOCs to shield away multiple conditional renderings

  使用多个可组合的高阶组件来屏蔽多条件渲染

- external templating components

  - avoid them and be comfortable with JSX and JS

  不要使用它们，而应该使用 JSX 和 JS
