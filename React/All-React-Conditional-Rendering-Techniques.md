
# All React Conditional Rendering Techniques

> 转译自：https://www.robinwieruch.de/conditional-rendering-react


A conditional render in React is no witchcraft. In JSX - the syntax extension used for React - you can use pure JavaScript. In JavaScript you should be familiar with if-else or switch case statements, [because they are the one of the fundamental pillars for learning React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/). You can use these in JSX as well, since JSX only mixes HTML and JavaScript.

React 中的条件渲染不是巫术。在 JSX（React 的语法扩展）中可以使用纯 JavaScript。在 JavaScript 中，你应该熟悉 if-else 或 switch case 语句，[因为它们是学习 React 的基本支柱之一（暂缺译文）]()。你也可以在 JSX 中使用它们，因为 JSX 只混合 HTML 和 JavaScript。

But what is conditional rendering in React? In a conditional render a component decides based on one or several conditions which elements it will return. For instance, it can either return a list of items or a message that says "Sorry, the list is empty". When a component has conditional rendering, the instance of the rendered component can have different looks.

但是 React 中的条件渲染是什么呢？在条件渲染中，组件根据一个或多个条件来决定返回哪些元素。例如，它可以返回一个项目列表，也可以返回一条消息说「对不起，列表是空的」。当组件具有条件渲染时，渲染组件的实例可以具有不同的外观。

The article aims to be an exhaustive list of options for conditional renderings in React. If you know more alternatives, feel free to contribute.

本文的目的是为 React 中的条件渲染提供一个详尽的方案列表。如果你知道更多的选择，请随意贡献。

# Table of Contents

（目录，略）

# if else in React

The easiest way to have a conditional rendering is to use an if else in React in your render method. Imagine you don't want to render your component, because it doesn't have the necessary props. For instance, a List component shouldn't render the list when there is no list of items. You can use an if statement to return earlier from the render lifecycle.

React 获得条件渲染的最简单方法是在 render() 方法中使用 if else。假设你不想渲染组件，因为它没有必要的属性。例如，当没有列表项时，列表组件不应该渲染列表。可以使用 if 语句从渲染周期中提前返回。

```js
function List({ list }) {
  if (!list) {
    return null;
  }

  return (
    <div>
      {list.map(item => <ListItem item={item} />)}
    </div>
  );
}
```

A component that returns null will render nothing. However, you might want to show a text when a list is empty to give your app user some feedback for a better user experience.

返回 null 的组件将不会渲染任何内容。但是，你可能希望在列表为空时显示文本，以便为应用程序用户提供一些反馈，获得更好的用户体验。

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
        {list.map(item => <ListItem item={item} />)}
      </div>
    );
  }
}
```

The List renders either nothing, a text or the list of items. The if-else statement is the most basic option to have a conditional rendering in React.

该列表要么不渲染任何内容，要么渲染文本或项列表。if-else 语句是 React 中拥有条件渲染的最基本选项。

# ternary operation in React

You can make your if-else statement more concise by using a [ternary operation](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

```javascript
condition ? expr1 : expr2
```

For instance, imagine you have a toggle to switch between two modes, edit and view, in your component. The derived condition is a simple boolean. You can use the boolean to decide which element you want to return.

```javascript
function Item({ item, mode }) {
  const isEditMode = mode === 'EDIT';

  return (
    <div>
      { isEditMode
        ? <ItemEdit item={item} />
        : <ItemView item={item} />
      }
    </div>
  );
}
```

If your blocks in both branches of the ternary operation are getting bigger, you can use parentheses.

```javascript
function Item({ item, mode }) {
  const isEditMode = mode === 'EDIT';

  return (
    <div>
      {isEditMode ? (
        <ItemEdit item={item} />
      ) : (
        <ItemView item={item} />
      )}
    </div>
  );
}
```

The ternary operation makes the conditional rendering in React more concise than the if-else statement. It is simple to inline it in your return statement.

# logical && operator in React

It happens often that you want to render either an element or nothing. For instance, you could have a LoadingIndicator component that returns a loading text or nothing. You can do it in JSX with an if statement or ternary operation.

```javascript
function LoadingIndicator({ isLoading }) {
  if (isLoading) {
    return (
      <div>
        <p>Loading...</p>
      </div>
    );
  } else {
    return null;
  }
}

function LoadingIndicator({ isLoading }) {
  return (
    <div>
      { isLoading
        ? <p>Loading...</p>
        : null
      }
    </div>
  );
}
```

But there is an alternative way that omits the necessity to return null. The logical `&&` operator helps you to make conditions that would return null more concise.

How does it work? In JavaScript a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false.

```javascript
const result = true && 'Hello World';
console.log(result);
// Hello World

const result = false && 'Hello World';
console.log(result);
// false
```

In React you can make use of that behaviour. If the condition is true, the expression after the logical && operator will be the output. If the condition is false, React ignores and skips the expression.

```javascript
function LoadingIndicator({ isLoading }) {
  return (
    <div>
      { isLoading && <p>Loading...</p> }
    </div>
  );
}
```

That's your way to go when you want to return an element or nothing. The technique is also called short-circuit evaluation. It makes it even more concise than a ternary operation when you would return null for a condition.

# switch case operator in React

Now there might be cases where you have multiple conditional renderings. For instance, the conditional rendering could apply based on different states. Let's imagine a notification component that can render an error, warning or info component based on the input state. You can use a switch case operator to handle the conditional rendering of these multiple states.

```javascript
function Notification({ text, state }) {
  switch(state) {
    case 'info':
      return <Info text={text} />;
    case 'warning':
      return <Warning text={text} />;
    case 'error':
      return <Error text={text} />;
    default:
      return null;
  }
}
```

Please note that you always have to use the `default` for the switch case operator. In React a component always has to return an element or null.

As a little side information: When a component has a conditional rendering based on a state, it makes sense to describe the interface of the component with `React.PropTypes`.

```javascript
function Notification({ text, state }) {
  switch(state) {
    case 'info':
      return <Info text={text} />;
    case 'warning':
      return <Warning text={text} />;
    case 'error':
      return <Error text={text} />;
    default:
      return null;
  }
}

Notification.propTypes = {
   text: React.PropTypes.string,
   state: React.PropTypes.oneOf(['info', 'warning', 'error'])
}
```

Now you have one generic component to show different kinds of notifications. For instance, based on the `state` prop the component could have different looks. An error could be red, a warning would be yellow and an info could be blue.

An alternative way would be to inline the switch case. Therefore you would need a self invoking JavaScript function.

```javascript
function Notification({ text, state }) {
  return (
    <div>
      {(function() {
        switch(state) {
          case 'info':
            return <Info text={text} />;
          case 'warning':
            return <Warning text={text} />;
          case 'error':
            return <Error text={text} />;
          default:
            return null;
        }
      })()}
    </div>
  );
}
```

Again it can be more concise with an ES6 arrow function.

```javascript{4}
function Notification({ text, state }) {
  return (
    <div>
      {(() => {
        switch(state) {
          case 'info':
            return <Info text={text} />;
          case 'warning':
            return <Warning text={text} />;
          case 'error':
            return <Error text={text} />;
          default:
            return null;
        }
      })()}
    </div>
  );
}
```

In conclusion, the switch case operator helps you to have multiple conditional renderings. But is it the best way to do that? Let's see how we can have multiple conditional renderings with enums.

# Conditional Rendering with enums

In JavaScript an object can be used as an enum when the object is used as a map of key value pairs.

```javascript
const ENUM = {
  a: '1',
  b: '2',
  c: '3',
};
```

An enum is a great way to have multiple conditional renderings. Let's consider the notification component again. This time we can use the enum as inlined object.

```javascript
function Notification({ text, state }) {
  return (
    <div>
      {{
        info: <Info text={text} />,
        warning: <Warning text={text} />,
        error: <Error text={text} />,
      }[state]}
    </div>
  );
}
```

The state property key helps us to retrieve the value from the object. That's neat, isn't it? It is much more readable compared to the switch case operator.

In this case we had to use an inlined object, because the values of the object depend on the `text` property. That would be my recommended way anyway. However, if it wouldn't depend on the text property, you could use an external static enum too.

```javascript
const NOTIFICATION_STATES = {
  info: <Info />,
  warning: <Warning />,
  error: <Error />,
};

function Notification({ state }) {
  return (
    <div>
      {NOTIFICATION_STATES[state]}
    </div>
  );
}
```

Although we could use a function to retrieve the value, if we would depend on the `text` property.

```javascript
const getSpecificNotification = (text) => ({
  info: <Info text={text} />,
  warning: <Warning text={text} />,
  error: <Error text={text} />,
});

function Notification({ state, text }) {
  return (
    <div>
      {getSpecificNotification(text)[state]}
    </div>
  );
}
```

After all, the enum approach in comparison to the switch case statement is more readable. Objects as enums open up plenty of options to have multiple conditional renderings. Consider this last example to see what's possible:

```javascript
function FooBarOrFooOrBar({ isFoo, isBar }) {
  const key = `${isFoo}-${isBar}`;
  return (
    <div>
      {{
        ['true-true']: <FooBar />,
        ['true-false']: <Foo />,
        ['false-true']: <Bar />,
        ['false-false']: null,
      }[key]}
    </div>
  );
}

FooBarOrFooOrBar.propTypes = {
   isFoo: React.PropTypes.boolean.isRequired,
   isBar: React.PropTypes.boolean.isRequired,
}
```

# Multi-Level Conditional Rendering in React

What about nested conditional renderings? Yes, it is possible. For instance, let's have a look at the List component that can either show a list, an empty text or nothing.

```javascript
function List({ list }) {
  const isNull = !list;
  const isEmpty = !isNull && !list.length;

  return (
    <div>
      { isNull
        ? null
        : ( isEmpty
          ? <p>Sorry, the list is empty.</p>
          : <div>{list.map(item => <ListItem item={item} />)}</div>
        )
      }
    </div>
  );
}

// Usage

<List list={null} /> // <div></div>
<List list={[]} /> // <div><p>Sorry, the list is empty.</p></div>
<List list={['a', 'b', 'c']} /> // <div><div>a</div><div>b</div><div>c</div><div>
```

It works. However I would recommend to keep the nested conditional renderings to a minimum. It makes it less readable. My recommendation would be to split it up into smaller components which themselves have conditional renderings.

```javascript
function List({ list }) {
  const isList = list && list.length;

  return (
    <div>
      { isList
        ? <div>{list.map(item => <ListItem item={item} />)}</div>
        : <NoList isNull={!list} isEmpty={list && !list.length} />
      }
    </div>
  );
}

function NoList({ isNull, isEmpty }) {
  return (!isNull && isEmpty) && <p>Sorry, the list is empty.</p>;
}
```

Still, it doesn't look that appealing. Let's have a look at higher order components and how they would help to tidy it up.

# With Higher Order Components

Higher order components (HOCs) are a perfect match for conditional rendering in React. HOCs can have multiple use cases. Yet one use case could be to alter the look of a component. To make the use case more specific: it could be to apply a conditional rendering for a component. Let's have a look at a HOC that either shows a loading indicator or a desired component.

```javascript
// HOC declaration
function withLoadingIndicator(Component) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (!isLoading) {
      return <Component { ...props } />;
    }

    return <div><p>Loading...</p></div>;
  };
}

// Usage
const ListWithLoadingIndicator = withLoadingIndicator(List);

<ListWithLoadingIndicator
  isLoading={props.isLoading}
  list={props.list}
/>
```

In the example, the List component can focus on rendering the list. It doesn't have to bother with a loading state. Ultimately you could add more HOCs to shield away multiple conditional rendering edge cases.

A HOC can opt-in one or multiple conditional renderings. You could even use multiple HOCs to handle several conditional renderings. After all, a HOC shields away all the noise from your component. If you want to dig deeper into conditional renderings with higher order components, you should read the [conditional rendering with HOCs article](https://www.robinwieruch.de/react-higher-order-components/).

# External Templating Components

Last but not least there exist external solutions to deal with conditional renderings. They add control components to enable conditional renderings without JavaScript in JSX. Then it is not question anymore on how to use if else in React.

```javascript
<Choose>
  <When condition={isLoading}>
    <div><p>Loading...</p></div>
  </When>
  <Otherwise>
    <div>{list.map(item => <ListItem item={item} />)}</div>
  </Otherwise>
</Choose>
```

Some people use it. Personally I wouldn't recommend it. JSX allows you to use the powerful set of JavaScript functionalities to handle conditional rendering. There is no need to add templating components to enable conditional rendering. A lot of people consider React including JSX as their library of choice, because they can handle the rendering with pure HTML and JS in JSX.

<Divider />

In the end you might wonder: When to use which type of conditional render? My opinionated answer:

* if-else
    * is the most basic conditional rendering
    * beginner friendly
    * use if to opt-out early from a render method by returning null
* ternary operator
    * use it over an if-else statement
    * it is more concise than if-else
* logical && operator
    * use it when one side of the ternary operation would return null
    * but be careful that you don't run into bugs when using multiple conditions
* switch case
    * verbose
    * can only be inlined with self invoking function
    * avoid it, use enums instead
* enums
    * perfect to map different states
    * perfect to map more than one condition
* multi-level/nested conditional renderings
    * avoid them for the sake of readability
    * split up components into more lightweight components with their own simple conditional rendering
    * use HOCs
* HOCs
    * use them to shield away conditional rendering
    * components can focus on their main purpose
* external templating components
    * avoid them and be comfortable with JSX and JavaScript

In conclusion, I hope you can make use of the alternatives for conditional rendering. I am keen to hear your ways of doing it.

<ReadMore label="The SoundCloud Client in React + Redux" link="https://www.robinwieruch.de/the-soundcloud-client-in-react-redux" />

<ReadMore label="Tips to learn React + Redux" link="https://www.robinwieruch.de/tips-to-learn-react-redux" />
