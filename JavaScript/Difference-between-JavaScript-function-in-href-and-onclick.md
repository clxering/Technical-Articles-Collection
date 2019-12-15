# Difference between JavaScript function in href and onclick（JavaScript 函数在 href 和 onclick 中的区别）

> 转译自：https://www.codespeedy.com/difference-between-javascript-function-in-href-and-onclick/

> In this JavaScript post, I am going to clear the difference between JavaScript function in href and onclick. You will also learn where to use JavaScript function in href and onclick. In both the cases, you will get the same result but there are some behavioural differences. I am here to clear all the confusions arising in your mind regarding this.

> 在这篇JavaScript文章中，我将澄清 href 和 onclick 中的 JavaScript 函数之间的区别。您还将了解如何在 href 和 onclick 中使用 JavaScript 函数。在这两种情况下，你会得到相同的结果，但也存在一些行为差异。我来这里是为了澄清你对这件事的所有困惑。

Here I am going to show you example of JavaScript function implementation using href and onclick both

在这里，我将展示使用 href 和 onclick 两种方法实现 JavaScript 函数的示例

```html
<a href="javascript:inHrefFunction()">Click Me!</a>
```

This one is the implementation of JavaScript function in href.

这是在 href 中实现的 JavaScript 函数。

```html
<a href="#" onclick="onclickFunction()">Click Me!</a>
```

This one is the implementation of JavaScript function in onclick.Basically, there is no difference in the performance.

这是 onclick 中 JavaScript 函数的实现。在性能上基本没有区别。

[How to write inline JavaScript code in HTML file](https://www.codespeedy.com/how-to-write-inline-javascript-code-in-html/)

But the first one will fail for sure for the browsers where JavaScript is not enabled.And the second one will also fail but it will be better if the href pointed to a URL for users without JS enabled.

但是第一个（例子）在没有启用 JavaScript 的浏览器中肯定会失败。第二种方法也会失败，但如果 href 为不启用 JS 的用户指向 URL，那就更好了。

In onclick, you can pass this as an argument but you can not do it for href.Let’s see the below example:

在 onclick 中，您可以将其作为参数传递，但对于 href 则不能这样做。让我们看看下面的例子：

```html
<a href="#" onclick="document.write(this.innerHTML)">I am onclick</a>
```

Here you will get output:

将得到输出：

```
I am onclick
```

But if you call JavaScript from the href:

但如果你从 href 中调用函数：

```html
<a href="javascript:document.write(this.innerHTML)">I am href</a>
```

Here the output will be like this:

你的输出如下所示：

```
undefined
```

If you add JavaScript in href and your browser is not JS enabled then it is a great problem.But if you use JavaScript in onclick and add return false; or return doAnythingYouWant(); it will be better for you. As in this case if your browser is not JS enabled then it will navigate to the page you wanted. And if Js is enabled then you can display something with JS.

如果你在 href 中添加 JavaScript，而你的浏览器没有启用 JS，那么这是一个大问题。但如果在 onclick 中使用 JavaScript 并添加 `return false;` 或返回 `doAnythingYouWant();` 这对你有好处。在这种情况下，如果您的浏览器没有启用 JS，那么它将导航到您想要的页面。如果启用了 Js，你就可以用 Js 来显示一些东西。

You can Also read,

你也可以阅读（下列相关文章）

[Guess The Number Game Using JavaScript（用 JavaScript 实现猜数字游戏）](https://www.codespeedy.com/guess-the-number-game-using-javascript/)

[How to run multiple JavaScript functions onclick（onclick 如何运行多个 JavaScript 函数）](https://www.codespeedy.com/how-to-run-multiple-javascript-functions-onclick/)
